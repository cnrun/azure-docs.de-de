---
title: Übersicht über Azure Service Fabric
description: Service Fabric ist eine Plattform für verteilte Systeme, mit der skalierbare, zuverlässige und leicht zu verwaltende Microservices erstellt werden können.
ms.topic: overview
ms.date: 09/22/2020
ms.custom: contentperfq1
ms.openlocfilehash: 3c282178decc1a07b2c0acc102b279688c42d52e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91300648"
---
# <a name="overview-of-azure-service-fabric"></a>Übersicht über Azure Service Fabric

Azure Service Fabric ist eine [Plattform für verteilte Systeme](#container-orchestration), die das Packen, Bereitstellen und Verwalten skalierbarer und zuverlässiger Microservices und Container vereinfacht. Service Fabric verfügt außerdem über einfache Lösungen für die komplexen Herausforderungen bei der [Entwicklung und Verwaltung](#application-lifecycle-management) von cloudnativen Anwendungen.

Ein wichtiges Merkmal von Service Fabric ist die Ausrichtung auf die Erstellung von zustandsbehafteten Diensten. Sie können das Service Fabric-[Programmiermodell](#stateless-and-stateful-microservices) verwenden oder zustandsbehaftete Dienste in Containern ausführen, die in einer beliebigen Sprache bzw. mit beliebigem Code geschrieben wurden. Sie können Service Fabric-Cluster nicht nur in Azure erstellen, sondern an [beliebigen Orten](#any-os-any-cloud), z. B. Windows Server und Linux in lokalen Umgebungen und anderen öffentlichen Clouds.

![Die Service Fabric-Plattform verfügt über Funktionen für Lebenszyklusverwaltung, Verfügbarkeit, Orchestrierung, Programmiermodelle, Integrität und Überwachung, Tools für Entwicklung/Betrieb und Autoskalierung in Azure, lokal, in anderen Clouds und auf Ihrem Entwicklungscomputer.][Image1]

Service Fabric liegt schon heute zahlreichen Microsoft-Diensten zugrunde. Hierzu zählen unter anderem Azure SQL-Datenbank, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype for Business und viele weitere Azure-Kerndienste.

## <a name="container-orchestration"></a>Containerorchestrierung

Service Fabric ist der Microsoft-Dienst für die [Containerorchestrierung](service-fabric-cluster-resource-manager-introduction.md) und dient zum Bereitstellen und Verwalten von Microservices in einem Cluster mit Computern. Hierfür wurden die Erkenntnisse genutzt, die beim Ausführen von Microsoft-Diensten in großem Umfang gewonnen wurden. Mit Service Fabric können Anwendungen in Sekundenschnelle mit hoher Dichte mit Hunderten oder Tausenden von Anwendungen oder Containern pro Computer bereitgestellt werden. Bei Verwendung von Service Fabric können Sie in derselben Anwendung beide Dienste in den Prozessen und Diensten der Container gemischt nutzen.

[Informieren Sie sich über Service Fabric](service-fabric-content-roadmap.md), z. B. wichtige Konzepte, Programmiermodelle, Anwendungslebenszyklus, Tests, Cluster und Integritätsüberwachung.

## <a name="stateless-and-stateful-microservices"></a>Zustandslose und zustandsbehaftete Microservices

Service Fabric bietet eine ausgereifte Lightweightlaufzeit, die zustandslose und zustandsbehaftete Microservices unterstützt. Ein wichtiges Merkmal von Service Fabric ist die stabile Unterstützung zur Erstellung zustandsbehafteter Dienste. Dies kann bei Service Fabric entweder über [integrierte Programmiermodelle](service-fabric-choose-framework.md) oder zustandsbehaftete Dienste in Containern erfolgen.

Informieren Sie sich über weitere [Anwendungsszenarien](service-fabric-application-scenarios.md), die von den zustandsbehafteten Service Fabric-Diensten profitieren.

## <a name="application-lifecycle-management"></a>Anwendungslebenszyklusverwaltung

Service Fabric verfügt über Unterstützung für den gesamten Anwendungslebenszyklus und die CI/CD-Vorgänge von Cloudanwendungen, einschließlich der Container: von der Entwicklung über die Bereitstellung, die tägliche Überwachung, Verwaltung und Wartung bis zur abschließenden Außerbetriebnahme. Service Fabric ist z.B. in CI/CD-Tools integriert wie z.B. [Azure Pipelines](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html), und [Octopus Deploy](https://octopus.com/) und kann mit jedem anderen beliebten CI/CD-Tool verwendet werden.

Weitere Informationen zur Verwaltung des Anwendungslebenszyklus finden Sie unter [Anwendungslebenszyklus](service-fabric-application-lifecycle.md). Informationen zur Bereitstellung von vorhandenen Anwendungen für Service Fabric finden Sie unter [Bereitstellen einer ausführbaren Gastanwendungsdatei](service-fabric-deploy-existing-app.md).

## <a name="any-os-any-cloud"></a>Jedes Betriebssystem, jede Cloud

Sie können Cluster für Service Fabric in vielen Umgebungen erstellen, z. B. [unter Azure oder lokal](service-fabric-deploy-anywhere.md) oder unter [Windows Server oder Linux](service-fabric-linux-windows-differences.md). Sie können auch Cluster auf anderen öffentlichen Clouds erstellen. Die Entwicklungsumgebung im Service Fabric SDK ist identisch mit der Produktionsumgebung ohne Beteiligung von Emulatoren. Anders ausgedrückt: Die Komponenten, die in Ihrem lokalen Entwicklungscluster ausgeführt werden, werden auch in anderen Umgebungen für Ihren Cluster bereitgestellt.

Für die [Windows-Entwicklung](service-fabric-get-started.md) ist das Service Fabric .NET SDK in Visual Studio und PowerShell integriert. Für die [Linux-Entwicklung](service-fabric-get-started-linux.md) ist das Service Fabric Java SDK in Eclipse integriert, und Yeoman wird zum Generieren von Vorlagen für Java, .NET Core und Containeranwendungen verwendet.

## <a name="compliance"></a>Kompatibilität

Der Azure Service Fabric-Ressourcenanbieter ist in allen Azure-Regionen verfügbar und mit allen Compliancezertifizierungen von Azure konform, z. B. SOC, ISO, PCI-DSS, HIPAA und DSGVO. Eine vollständige Liste finden Sie unter [Microsoft Compliance-Angebote](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Nächste Schritte

Erstellen und Bereitstellen Ihrer ersten Anwendung in Azure Service Fabric:

> [!div class="nextstepaction"]
> [Service Fabric-Schnellstart][sf-quickstart]

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
[sf-quickstart]: ./service-fabric-quickstart-dotnet.md
