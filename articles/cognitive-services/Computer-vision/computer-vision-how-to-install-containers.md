---
title: Installieren und Ausführen von Containern – maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: Informationen zum Herunterladen, Installieren und Ausführen von Containern für maschinelles Sehen in diesem Schritt-für-Schritt-Tutorial.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/03/2020
ms.author: aahi
ms.custom: seodec18
ms.openlocfilehash: bc55ab2697d8278bd975f618d17804499ba0128d
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982073"
---
# <a name="install-and-run-read-containers-preview"></a>Installieren und Ausführen von Lesecontainern (Vorschauversion)

[!INCLUDE [container hosting on the Microsoft Container Registry](../containers/includes/gated-container-hosting.md)]

Container ermöglichen Ihnen, die APIs für maschinelles Sehen in Ihrer eigenen Umgebung auszuführen. Container eignen sich hervorragend für bestimmte Sicherheits- und Datengovernanceanforderungen. In diesem Artikel erfahren Sie, wie Sie einen Container für maschinelles Sehen herunterladen, installieren und ausführen.

Mit dem *Read*-Container können Sie *gedruckten Text* in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen erkennen und extrahieren. Hierzu zählen z.B. Belege, Poster und Visitenkarten. Darüber hinaus erkennt der *Read*-Container *handschriftlichen Text* in Bildern und bietet Unterstützung für PDF, TIFF und mehrseitige Dateien. Weitere Informationen finden Sie in der Dokumentation zur [Lese-API](concept-recognizing-text.md#read-api).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Container müssen die folgenden Voraussetzungen erfüllt sein:

|Erforderlich|Zweck|
|--|--|
|Docker-Engine| Die Docker-Engine muss auf einem [Hostcomputer](#the-host-computer) installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) und [Linux](https://docs.docker.com/engine/installation/#supported-platforms) zur Verfügung. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können. <br><br> **Unter Windows** muss Docker auch für die Unterstützung von Linux-Containern konfiguriert werden.<br><br>|
|Kenntnisse zu Docker | Sie sollten über Grundkenntnisse der Konzepte von Docker, einschließlich Registrierungen, Repositorys, Container und Containerimages, verfügen und die grundlegenden `docker`-Befehle kennen.| 
|Ressource für maschinelles Sehen |Um den Container zu verwenden, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ **Maschinelles Sehen** sowie den zugehörigen API-Schlüssel und Endpunkt-URI. Beide Werte stehen auf der Übersichts- und auf der Schlüsselseite der Ressource zur Verfügung und werden zum Starten des Containers benötigt.<br><br>**{API_KEY}** : Einer der beiden verfügbaren Ressourcenschlüssel auf der Seite **Schlüssel**<br><br>**{ENDPOINT_URI}** : Der Endpunkt, der auf der Seite **Übersicht** angegeben ist|

## <a name="request-approval-to-run-the-container"></a>Anfordern der Genehmigung für die Containerausführung

Füllen Sie das [Anforderungsformular](https://aka.ms/cognitivegate) aus, und übermitteln Sie es, um die Genehmigung für die Containerausführung anzufordern. 

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Der Hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Unterstützung von Advanced Vector Extensions

Der **Hostcomputer** ist der Computer, auf dem der Docker-Container ausgeführt wird. Der Host *muss* [Advanced Vector Extensions](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2) unterstützen. Sie können die AVX2-Unterstützung auf Linux-Hosts mit dem folgenden Befehl überprüfen:

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

> [!WARNING]
> Der Hostcomputer *muss* AVX2 unterstützen. Ohne AVX2-Unterstützung funktioniert der Container *nicht* ordnungsgemäß.

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Abrufen des Containerimages mit `docker pull`

Für das Lesen stehen Containerimages zur Verfügung.

| Container | Container Registry/Repository/Imagename |
|-----------|------------|
| Read 3.0-preview | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview` |
| Read 3.1-preview | `mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview` |

Verwenden Sie den Befehl [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage herunterzuladen.

### <a name="docker-pull-for-the-read-container"></a>„docker pull“ für den Container für das Lesen

# <a name="version-31-preview"></a>[Version 3.1-preview](#tab/version-3-1)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview
```

# <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

```bash
docker pull mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview
```

---

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Verwenden des Containers

Wenn sich der Container auf dem [Hostcomputer](#the-host-computer) befindet, können Sie über den folgenden Prozess mit dem Container arbeiten.

1. [Führen Sie den Container aus](#run-the-container-with-docker-run), und verwenden Sie dabei die erforderlichen Abrechnungseinstellungen. Es sind noch weitere [Beispiele](computer-vision-resource-container-config.md) für den Befehl `docker run` verfügbar. 
1. [Fragen Sie den Vorhersageendpunkt des Containers ab.](#query-the-containers-prediction-endpoint) 

## <a name="run-the-container-with-docker-run"></a>Ausführen des Containers mit `docker run`

Verwenden Sie den Befehl [docker run](https://docs.docker.com/engine/reference/commandline/run/), um den Container auszuführen. Genaue Informationen dazu, wie Sie die Werte `{ENDPOINT_URI}` und `{API_KEY}` abrufen, erhalten Sie unter [Ermitteln erforderlicher Parameter](#gathering-required-parameters).

Es sind [Beispiele](computer-vision-resource-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar.

# <a name="version-31-preview"></a>[Version 3.1-preview](#tab/version-3-1)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.1-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Dieser Befehl:

* Führt den Container für das Lesen aus dem Containerimage aus
* Ordnet 8 CPU-Kerne und 18 GB Arbeitsspeicher zu.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

# <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

```bash
docker run --rm -it -p 5000:5000 --memory 18g --cpus 8 \
mcr.microsoft.com/azure-cognitive-services/vision/read:3.0-preview \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}

```

Dieser Befehl:

* Führt den Container für das Lesen aus dem Containerimage aus
* Ordnet 8 CPU-Kerne und 18 GB Arbeitsspeicher zu.
* Macht den TCP-Port 5000 verfügbar und ordnet eine Pseudo-TTY-Verbindung für den Container zu.
* Entfernt den Container automatisch, nachdem er beendet wurde. Das Containerimage ist auf dem Hostcomputer weiterhin verfügbar.

---


Es sind noch weitere [Beispiele](./computer-vision-resource-container-config.md#example-docker-run-commands) für den Befehl `docker run` verfügbar. 

> [!IMPORTANT]
> Die Optionen `Eula`, `Billing` und `ApiKey` müssen angegeben werden, um den Container auszuführen, andernfalls wird der Container nicht gestartet.  Weitere Informationen finden Sie unter [Abrechnung](#billing).

Wenn Sie einen höheren Durchsatz (beispielsweise bei der Verarbeitung mehrseitiger Dateien) benötigen, ziehen Sie die Bereitstellung mehrerer v3.0- oder v3.1-Container [in einem Kubernetes-Cluster](deploy-computer-vision-on-premises.md) mithilfe von [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-create) und [Azure Queue](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) in Betracht.

Wenn Sie Azure Storage zum Speichern von Images für die Verarbeitung verwenden, können Sie eine [Verbindungszeichenfolge](https://docs.microsoft.com/azure/storage/common/storage-configure-connection-string) erstellen, die beim Aufrufen des Containers verwendet werden soll.

So finden Sie die Verbindungszeichenfolge:

1. Navigieren Sie im Azure-Portal zu **Speicherkonten**, und suchen Sie Ihr Konto.
2. Klicken Sie in der linken Navigationsliste auf **Zugriffsschlüssel**.
3. Die Verbindungszeichenfolge befindet sich unterhalb von **Verbindungszeichenfolge**.

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Abfragen des Vorhersageendpunkts des Containers

Der Container stellt REST-basierte Endpunkt-APIs für die Abfragevorhersage bereit. 

# <a name="version-31-preview"></a>[Version 3.1-preview](#tab/version-3-1)

Verwenden Sie für Container-APIs den Host `http://localhost:5000`. Sie können den Swagger-Pfad unter `http://localhost:5000/swagger/vision-v3.0-preview-read/swagger.json` anzeigen.

# <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Verwenden Sie für Container-APIs den Host `http://localhost:5000`. Sie können den Swagger-Pfad unter `http://localhost:5000/swagger/vision-v3.1-preview-read/swagger.json` anzeigen.

---

### <a name="asynchronous-read"></a>Asynchrones Lesen


# <a name="version-31-preview"></a>[Version 3.1-preview](#tab/version-3-1)

Sie können die Vorgänge `POST /vision/v3.1/read/analyze` und `GET /vision/v3.1/read/operations/{operationId}` kombiniert verwenden, um asynchron in einem Bild zu lesen, ähnlich wie der Dienst für maschinelles Sehen die entsprechenden REST-Vorgänge verwendet. Die asynchrone POST-Methode gibt eine `operationId` zurück, die als Bezeichner für die HTTP GET-Anforderung verwendet wird.


Wählen Sie auf der Swagger-Benutzeroberfläche `asyncBatchAnalyze` aus, um die Option im Browser zu erweitern. Wählen Sie dann **Jetzt ausprobieren** > **Datei auswählen** aus. In diesem Beispiel verwenden wir das folgende Bild:

![Tabstopps und Leerzeichen](media/tabs-vs-spaces.png)

Wenn die asynchrone POST-Anforderung erfolgreich ausgeführt wurde, gibt sie den Statuscode **HTTP 202** zurück. Teil der Antwort ist ein `operation-location`-Header, der den Ergebnisendpunkt für die Anforderung enthält.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.1/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

Die `operation-location` ist die vollqualifizierte URL, auf die über HTTP GET zugegriffen wird. Hier ist die JSON-Antwort der Ausführung der URL `operation-location` aus dem vorherigen Bild:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:30:14Z",
  "lastUpdatedDateTime": "2020-09-02T10:30:15Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "appearance": {
              "style": "handwriting",
              "styleConfidence": 0.999
            },
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "appearance": {
              "style": "print",
              "styleConfidence": 0.603
            },
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

# <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

Sie können die Vorgänge `POST /vision/v3.0/read/analyze` und `GET /vision/v3.0/read/operations/{operationId}` kombiniert verwenden, um asynchron in einem Bild zu lesen, ähnlich wie der Dienst für maschinelles Sehen die entsprechenden REST-Vorgänge verwendet. Die asynchrone POST-Methode gibt eine `operationId` zurück, die als Bezeichner für die HTTP GET-Anforderung verwendet wird.

Wählen Sie auf der Swagger-Benutzeroberfläche `asyncBatchAnalyze` aus, um die Option im Browser zu erweitern. Wählen Sie dann **Jetzt ausprobieren** > **Datei auswählen** aus. In diesem Beispiel verwenden wir das folgende Bild:

![Tabstopps und Leerzeichen](media/tabs-vs-spaces.png)

Wenn die asynchrone POST-Anforderung erfolgreich ausgeführt wurde, gibt sie den Statuscode **HTTP 202** zurück. Teil der Antwort ist ein `operation-location`-Header, der den Ergebnisendpunkt für die Anforderung enthält.

```http
 content-length: 0
 date: Fri, 04 Sep 2020 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v3.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

Die `operation-location` ist die vollqualifizierte URL, auf die über HTTP GET zugegriffen wird. Hier ist die JSON-Antwort der Ausführung der URL `operation-location` aus dem vorherigen Bild:

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-09-02T10:24:49Z",
  "lastUpdatedDateTime": "2020-09-02T10:24:50Z",
  "analyzeResult": {
    "version": "3.0.0",
    "readResults": [
      {
        "page": 1,
        "angle": 2.12,
        "width": 502,
        "height": 252,
        "unit": "pixel",
        "language": "",
        "lines": [
          {
            "boundingBox": [58, 42, 314, 59, 311, 123, 56, 121],
            "text": "Tabs vs",
            "words": [
              {
                "boundingBox": [85, 45, 242, 62, 241, 122, 83, 123],
                "text": "Tabs",
                "confidence": 0.981
              },
              {
                "boundingBox": [258, 64, 314, 72, 314, 123, 256, 123],
                "text": "vs",
                "confidence": 0.958
              }
            ]
          },
          {
            "boundingBox": [286, 171, 415, 165, 417, 197, 287, 201],
            "text": "paces",
            "words": [
              {
                "boundingBox": [303, 175, 415, 167, 415, 198, 306, 199],
                "text": "paces",
                "confidence": 0.918
              }
            ]
          }
        ]
      }
    ]
  }
}
```

---

> [!IMPORTANT]
> Wenn Sie mehrere Lesecontainer hinter einem Lastenausgleich bereitstellen (z. B. unter Docker Compose oder Kubernetes), benötigen Sie einen externen Cache. Da der Verarbeitungscontainer und der GET-Anforderungscontainer möglicherweise nicht identisch sind, werden die Ergebnisse in einem externen Cache gespeichert und dort für die Container freigegeben. Ausführliche Informationen zu Cacheeinstellungen finden Sie unter [Konfigurieren von Docker-Containern für maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/computer-vision-resource-container-config).

### <a name="synchronous-read"></a>Synchrones Lesen

Mit dem folgenden Vorgang können Sie ein Bild synchron lesen: 

# <a name="version-31-preview"></a>[Version 3.1-preview](#tab/version-3-1)

`POST /vision/v3.1/read/syncAnalyze` 

# <a name="version-30-preview"></a>[Version 3.0-preview](#tab/version-3)

`POST /vision/v3.0/read/SyncAnalyze`

---

Wenn das Bild vollständig gelesen wird, dann – und nur dann – gibt die API eine JSON-Antwort zurück. Die einzige Ausnahme hierzu ist das Auftreten eines Fehlers. Bei einem Fehler wird der folgende JSON-Code zurückgegeben:

```json
{
    "status": "Failed"
}
```

Das JSON-Antwortobjekt enthält dasselbe Objektdiagramm wie die asynchrone Version. Wenn Sie ein JavaScript-Benutzer sind und Typsicherheit wünschen, können Sie die JSON-Antwort mithilfe von TypeScript umwandeln.

Ein Beispiel für einen Anwendungsfall finden Sie in <a href="https://aka.ms/ts-read-api-types" target="_blank" rel="noopener noreferrer">dieser TypeScript-Sandbox<span class="docon docon-navigate-external x-hidden-focus"></span></a>. Wählen Sie **Ausführen** aus, um die Benutzerfreundlichkeit visuell darzustellen.

## <a name="stop-the-container"></a>Beenden des Containers

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie den Container mit einer [Ausgabenbereitstellung](./computer-vision-resource-container-config.md#mount-settings) ausführen und die Protokollierung aktiviert ist, generiert der Container Protokolldateien. Diese sind hilfreich, um Probleme beim Starten oder Ausführen des Containers zu beheben.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Abrechnung

Die Cognitive Services-Container senden Abrechnungsinformationen an Azure und verwenden dafür die entsprechende Ressource in Ihrem Azure-Konto.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Weitere Informationen zu diesen Optionen finden Sie unter [Konfigurieren von Containern](./computer-vision-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie die Konzepte und den Workflow zum Herunterladen, Installieren und Ausführen von Containern für maschinelles Sehen kennengelernt. Zusammenfassung:

* Für das maschinelle Sehen ist ein Linux-Container für Docker verfügbar, der das Lesen kapselt.
* Containerimages werden aus der Containerregistrierung „Containervorschau“ in Azure heruntergeladen.
* Containerimages werden in Docker ausgeführt.
* Sie können entweder die REST-API oder das SDK verwenden, um Vorgänge in Lesecontainern über den Host-URI des Containers aufzurufen.
* Bei der Instanziierung eines Containers müssen Sie Abrechnungsinformationen angeben.

> [!IMPORTANT]
> Für die Ausführung von Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit Azure verbunden sind. Kunden müssen sicherstellen, dass Container jederzeit Abrechnungsinformationen an den Messungsdienst übermitteln können. Cognitive Services-Container senden keine Kundendaten (etwa das analysierte Bild oder den analysierten Text) an Microsoft.

## <a name="next-steps"></a>Nächste Schritte

* Konfigurationseinstellungen finden Sie unter [Konfigurieren von Containern](computer-vision-resource-container-config.md).
* Lesen Sie [Übersicht über maschinelles Sehen](overview.md), um weitere Informationen zur Erkennung von gedrucktem und handschriftlichem Text zu erhalten.
* Details zu den vom Container unterstützten Methoden finden Sie unter [Maschinelles Sehen-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).
* Unter [Häufig gestellte Fragen (FAQ)](FAQ.md) finden Sie Informationen zum Beheben von Problemen im Zusammenhang mit der Funktionalität des maschinellen Sehens.
* Verwenden weiterer [Cognitive Services-Container](../cognitive-services-container-support.md)
