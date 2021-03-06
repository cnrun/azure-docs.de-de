---
title: Serverseitige Leistungsabfragen
description: Durchführen von serverseitigen Leistungsabfragen mit API-Aufrufen
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: cd255896d57d6bda60ec8874430fa994eae69f40
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89613639"
---
# <a name="server-side-performance-queries"></a>Serverseitige Leistungsabfragen

Eine gute Renderingleistung auf dem Server ist für stabile Bildfrequenzen und eine hohe Benutzerfreundlichkeit von entscheidender Bedeutung. Es ist wichtig, die Leistungsmerkmale auf dem Server sorgfältig zu überwachen und bei Bedarf zu optimieren. Leistungsdaten können über dedizierte API-Funktionen abgefragt werden.

Die größte Auswirkung auf die Renderingleistung haben die Modelleingabedaten. Sie können die Eingabedaten wie unter [Konfigurieren der Modellkonvertierung](../../how-tos/conversion/configure-model-conversion.md) beschrieben optimieren.

Auch die clientseitige Anwendungsleistung kann einen Engpass darstellen. Es empfiehlt sich, eine [Ablaufverfolgung für die Leistung](../../how-tos/performance-tracing.md) (:::no-loc text="performance trace":::) durchzuführen, um die clientseitige Leistung eingehend zu analysieren.

## <a name="clientserver-timeline"></a>Zeitachse für Client/Server

Bevor näher auf die verschiedenen Latenzwerte eingegangen wird, ist es ratsam, einen Blick auf die Synchronisierungspunkte zwischen Client und Server auf der Zeitachse zu werfen:

![Zeitachse der Pipeline](./media/server-client-timeline.png)

Die Abbildung enthält folgende Informationen:

* Vom Client wird mit einer konstanten Bildfrequenz von 60 Hz (alle 16,6 ms) eine *Stellungsschätzung* ausgelöst.
* Der Server startet das Rendering anschließend basierend auf dieser Stellung.
* Der Server sendet das codierte Videobild zurück.
* Der Client decodiert das Bild, führt einige zusätzliche CPU- und GPU-Schritte aus und zeigt das Bild anschließend an.

## <a name="frame-statistics-queries"></a>Abfragen zur Framestatistik

Die Framestatistik enthält allgemeine Informationen zum letzten Frame, z. B. zur Latenz. Da die in der Struktur `FrameStatistics` bereitgestellten Daten auf der Clientseite gemessen werden, wird für die API ein synchroner Aufruf durchgeführt:

```cs
void QueryFrameData(AzureSession session)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        // do something with the result
    }
}
```

```cpp
void QueryFrameData(ApiHandle<AzureSession> session)
{
    FrameStatistics frameStatistics;
    if (*session->GetGraphicsBinding()->GetLastFrameStatistics(&frameStatistics) == Result::Success)
    {
        // do something with the result
    }
}
```

Das abgerufene Objekt `FrameStatistics` enthält die folgenden Member:

| Member | Erklärung |
|:-|:-|
| latencyPoseToReceive | Die Latenz für die Schätzung der Kamerastellung auf dem Clientgerät, bis für die Clientanwendung ein Serverframe für diese Stellung vollständig verfügbar ist. Dieser Wert umfasst den Netzwerkroundtrip, die Renderzeit des Servers, die Videodecodierung und die Jitterkorrektur. Siehe **Intervall 1 in der obigen Abbildung**.|
| latencyReceiveToPresent | Latenz der Verfügbarkeit eines empfangenen Remote-Frames, bis die Client-App „PresentFrame“ für die CPU aufruft. |
| latencyPresentToDisplay  | Latenz der Darstellung eines Frames in der CPU, bis die Anzeige verfügbar ist. Dieser Wert umfasst die GPU-Zeit des Clients, Framepufferung des Betriebssystems, Umprojizierung der Hardware und geräteabhängige Scandauer der Anzeige. Siehe **Intervall 2 in der obigen Abbildung**.|
| timeSinceLastPresent | Die Zeit zwischen nachfolgenden Aufrufen und „PresentFrame“ in der CPU. Höhere Werte als die Anzeigedauer (z. B. 16,6 ms auf einem Clientgerät mit 60 Hz) deuten auf Probleme hin, die deshalb auftreten, weil die Clientanwendung die CPU-Workload nicht rechtzeitig abgearbeitet hat. Siehe **Intervall 3 in der obigen Abbildung**.|
| videoFramesReceived | Die Anzahl von Frames, die vom Server innerhalb der letzten Sekunde empfangen wurden. |
| videoFrameReusedCount | Anzahl von innerhalb der letzten Sekunde empfangenen Frames, die auf dem Gerät mehr als einmal verwendet wurden. Mit anderen Werten als Null wird angegeben, dass Frames wiederverwendet und umprojiziert werden mussten, weil entweder Netzwerkjitter aufgetreten ist oder der Server sehr viel Zeit für das Rendern benötigt hat. |
| videoFramesSkipped | Anzahl von empfangenen Frames innerhalb der letzten Sekunde, die decodiert, aber nicht in der Anzeige dargestellt wurden, weil ein neuer Frame eingetroffen ist. Mit anderen Werten als Null wird angegeben, dass aufgrund von Netzwerkjitter mehrere Frames verzögert waren und dann auf einmal auf dem Clientgerät eingetroffen sind. |
| videoFramesDiscarded | Hierfür besteht eine starke Ähnlichkeit mit **videoFramesSkipped**. Der Grund für das Verwerfen ist aber, dass ein Frame so spät eingetroffen ist, dass sogar eine Korrelation mit einer ausstehenden Stellung nicht mehr möglich ist. Wenn dieser Fall eintritt, bestehen im Netzwerk schwerwiegende Konflikte.|
| videoFrameMinDelta | Minimale Zeitspanne zwischen zwei aufeinanderfolgenden Frames, die innerhalb der letzten Sekunde eingetroffen sind. Zusammen mit „videoFrameMaxDelta“ ist dieser Bereich ein Hinweis auf Jitter, der durch das Netzwerk oder den Videocodec verursacht wird. |
| videoFrameMaxDelta | Maximale Zeitspanne zwischen zwei aufeinanderfolgenden Frames, die innerhalb der letzten Sekunde eingetroffen sind. Zusammen mit „videoFrameMinDelta“ ist dieser Bereich ein Hinweis auf Jitter, der durch das Netzwerk oder den Videocodec verursacht wird. |

Die Summe aller Latenzwerte ist normalerweise deutlich größer als die verfügbare Framedauer bei 60 Hz. Dies ist in Ordnung, weil mehrere Frames gleichzeitig verarbeitet werden und neue Frameanforderungen mit der gewünschten Bildfrequenz ausgelöst werden. Dies ist in der Abbildung dargestellt. Wenn die Latenz aber zu groß wird, wirkt sich dies auf die [Late Stage Reprojection (LSR)](../../overview/features/late-stage-reprojection.md) (Umprojizierung zu einem späten Zeitpunkt) aus und kann ggf. die allgemeine Benutzerfreundlichkeit beeinträchtigen.

`videoFramesReceived`, `videoFrameReusedCount` und `videoFramesDiscarded` können verwendet werden, um die Netzwerk- und Serverleistung zu messen. Wenn `videoFramesReceived` niedrig und `videoFrameReusedCount` hoch ist, kann dies ein Hinweis auf eine Netzwerküberlastung oder eine schlechte Serverleistung sein. Ein hoher Wert für `videoFramesDiscarded` ist ebenfalls ein Hinweis auf eine Überlastung des Netzwerks.

Anhand von `timeSinceLastPresent`, `videoFrameMinDelta` und `videoFrameMaxDelta` können Sie die Varianz von eingehenden Videoframes und lokalen Aufrufen ablesen. Eine hohe Varianz deutet auf eine instabile Bildfrequenz hin.

Keiner der obigen Werte ist ein eindeutiger Hinweis auf die reine Netzwerklatenz (rote Pfeile in der Abbildung), da der genaue Zeitraum, der vom Server für das Rendern benötigt wird, vom Roundtripwert `latencyPoseToReceive` abgezogen werden muss. Der serverseitige Teil der allgemeinen Latenz enthält Informationen, die für den Client nicht verfügbar sind. Im nächsten Absatz wird aber beschrieben, wie durch eine zusätzliche Eingabe des Servers eine Näherung für diesen Wert erzielt wird und wie er mit dem Wert `networkLatency` verfügbar gemacht wird.

## <a name="performance-assessment-queries"></a>Abfragen zur Leistungsbewertung

*Abfragen zur Leistungsbewertung* liefern ausführlichere Informationen zur CPU- und GPU-Workload auf dem Server. Da die Daten vom Server angefordert werden, wird zum Abfragen einer Leistungsmomentaufnahme das übliche asynchrone Muster verwendet:

```cs
PerformanceAssessmentAsync _assessmentQuery = null;

void QueryPerformanceAssessment(AzureSession session)
{
    _assessmentQuery = session.Actions.QueryServerPerformanceAssessmentAsync();
    _assessmentQuery.Completed += (PerformanceAssessmentAsync res) =>
    {
        // do something with the result:
        PerformanceAssessment result = res.Result;
        // ...

        _assessmentQuery = null;
    };
}
```

```cpp
void QueryPerformanceAssessment(ApiHandle<AzureSession> session)
{
    ApiHandle<PerformanceAssessmentAsync> assessmentQuery = *session->Actions()->QueryServerPerformanceAssessmentAsync();
    assessmentQuery->Completed([] (ApiHandle<PerformanceAssessmentAsync> res)
    {
        // do something with the result:
        PerformanceAssessment result = res->GetResult();

        // ...

    });
}
```

Im Gegensatz zum Objekt `FrameStatistics` enthält das Objekt `PerformanceAssessment` serverseitige Informationen:

| Member | Erklärung |
|:-|:-|
| timeCPU | Durchschnittliche Dauer der Server-CPU pro Frame in Millisekunden |
| timeGPU | Durchschnittliche Dauer der Server-GPU pro Frame in Millisekunden |
| utilizationCPU | Gesamtauslastung der Server-CPU in Prozent |
| utilizationGPU | Gesamtauslastung der Server-GPU in Prozent |
| memoryCPU | Gesamtauslastung des Serverhauptspeichers in Prozent |
| memoryGPU | Gesamtauslastung des dedizierten Videospeichers in Prozent der Server-GPU |
| networkLatency | Ungefähre durchschnittliche Netzwerkroundtrip-Latenz in Millisekunden. In der obigen Abbildung entspricht dies der Summe der roten Pfeile. Der Wert wird berechnet, indem die tatsächliche Renderingdauer des Servers vom Wert `latencyPoseToReceive` von `FrameStatistics` subtrahiert wird. Diese Näherung ist zwar nicht exakt, aber es ist ein Hinweis auf die ungefähre Netzwerklatenz – unabhängig von den auf dem Client berechneten Latenzwerten. |
| polygonsRendered | Die Anzahl von Dreiecken, die in einem Frame gerendert werden. Diese Zahl enthält auch die Dreiecke, die später beim Rendern herausgefiltert werden. Dies bedeutet, dass diese Zahl für unterschiedliche Kamerapositionen nicht stark variiert. Je nach der Rate, die für das Herausfiltern der Dreiecke anfällt, kann die Leistung aber erheblich variieren.|

Als Hilfe beim Bewerten der Werte verfügt jeder Teil über eine Qualitätsklassifizierung wie **Great** (Hervorragend), **Good** (Gut), **Mediocre** (Mittel) oder **Bad** (Schlecht).
Diese Bewertungsmetrik ist ein grober Hinweis auf die Serverintegrität, der nicht als absolut angesehen werden sollte. Angenommen, für die GPU-Dauer wird eine „mittlere“ Bewertung angezeigt. Die Bewertung ist mittelmäßig, weil der Wert in der Nähe des allgemeinen Budgets für die Framedauer liegt. In Ihrem Fall kann dies aber trotzdem ein guter Wert sein, weil Sie ein komplexes Modell rendern.

## <a name="statistics-debug-output"></a>Debugausgabe für Statistik

Die Klasse `ARRServiceStats` ist eine C#-Klasse, die sowohl die Framestatistik als auch die Abfragen zur Leistungsbewertung umschließt und über nützliche Funktionen verfügt, mit denen Statistiken als aggregierte Werte oder als vordefinierte Zeichenfolge zurückgegeben werden können. Der folgende Code ist die einfachste Möglichkeit, um in Ihrer Clientanwendung serverseitige Statistiken anzuzeigen.

```cs
ARRServiceStats _stats = null;

void OnConnect()
{
    _stats = new ARRServiceStats();
}

void OnDisconnect()
{
    _stats = null;
}

void Update()
{
    if (_stats != null)
    {
        // update once a frame to retrieve new information and build average values
        _stats.Update(Service.CurrentActiveSession);

        // retrieve a string with relevant stats information
        InfoLabel.text = _stats.GetStatsString();
    }
}
```

Mit dem obigen Code wird die Beschriftung mit dem folgenden Text gefüllt:

![Zeichenfolgenausgabe von „ArrServiceStats“](./media/arr-service-stats.png)

Mit der API `GetStatsString` wird eine Zeichenfolge mit allen Werten formatiert, aber jeder einzelne Wert kann auch programmgesteuert über die `ARRServiceStats`-Instanz abgefragt werden.

Es sind auch Varianten von Membern vorhanden, mit denen die Werte im Laufe der Zeit aggregiert werden. Es werden Member mit dem Suffix `*Avg`, `*Max` oder `*Total` angezeigt. Mit dem Member `FramesUsedForAverage` wird angegeben, wie viele Frames für diese Aggregation verwendet wurden.

## <a name="api-documentation"></a>API-Dokumentation

* [C# RemoteManager.QueryServerPerformanceAssessmentAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.queryserverperformanceassessmentasync)
* [C++ RemoteManager::QueryServerPerformanceAssessmentAsync()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#queryserverperformanceassessmentasync)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Leistungsüberwachungen](../../how-tos/performance-tracing.md)
* [Konfigurieren der Modellkonvertierung](../../how-tos/conversion/configure-model-conversion.md)
