---
title: 'Azure Defender für Containerregistrierungen: Vorteile und Features'
description: Enthält eine Beschreibung der Vorteile und Features von Azure Defender für Containerregistrierungen.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 12264a79ee5428e98d6cf7d37bef6706295e68dc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448373"
---
# <a name="introduction-to-azure-defender-for-container-registries"></a>Einführung in Azure Defender für Containerregistrierungen

Azure Container Registry (ACR) ist ein verwalteter, privater Docker-Registrierungsdienst, der Ihre Containerimages für Azure-Bereitstellungen in einer zentralen Registrierung speichert und verwaltet. Er basiert auf der Open-Source-Docker-Registrierung 2.0.

Aktivieren Sie **Azure Defender für Containerregistrierungen** auf Abonnementebene, um alle auf Azure Resource Manager basierenden Registrierungen in Ihrem Abonnement zu schützen. Mit Security Center werden dann Images gescannt, die in die Registrierung gepusht oder importiert werden oder innerhalb der letzten 30 Tage gepullt wurden. Dieses Feature wird pro Image abgerechnet.

## <a name="what-are-the-benefits-of-azure-defender-for-container-registries"></a>Welche Vorteile hat die Nutzung von Azure Defender für Containerregistrierungen?

Security Center identifiziert auf Azure Resource Manager basierende ACR-Registrierungen in Ihrem Abonnement und ermöglicht für die Images Ihrer Registrierung eine nahtlose native Azure-Sicherheitsrisikobewertung und -verwaltung.

**Azure Defender für Containerregistrierungen** verfügt über eine Option für die Überprüfung auf Sicherheitsrisiken, mit der die Images in Ihren Azure Resource Manager-basierten Azure Container Registry-Registrierungen gescannt und die Sicherheitsrisiken Ihrer Images eingehender untersucht werden können. Der integrierte Scanner wird von Qualys bereitgestellt. Hierbei handelt es sich um einen branchenführenden Anbieter von Tools zur Überprüfung auf Sicherheitsrisiken.

Werden Probleme gefunden – von Qualys oder Security Center –, werden Sie auf dem Security Center-Dashboard benachrichtigt. Für jedes Sicherheitsrisiko bietet Security Center Handlungsempfehlungen sowie eine Klassifizierung des Schweregrads und Anleitungen für die Behebung des Problems. Ausführliche Informationen zu Security Center-Empfehlungen für Container finden Sie in der [Referenzliste der Empfehlungen](recommendations-reference.md#recs-containers).

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Security Center liefert Details zu den einzelnen gemeldeten Sicherheitsrisiken und verfügt über eine Klassifizierung des Schweregrads. Außerdem erhalten Sie Anleitungen zur Behebung der spezifischen Sicherheitsrisiken, die für jedes Image gefunden wurden.

Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.


## <a name="when-are-images-scanned"></a>Wann werden Images überprüft?

Für das Scannen von Images gibt es drei Trigger:

- **On push** (Beim Pushen): Immer, wenn ein Image an Ihre Registrierung gepusht wird, überprüft Security Center es automatisch. Zum Auslösen der Überprüfung eines Images müssen Sie es also an Ihr Repository pushen.

- **Recently pulled** (Vor Kurzem gepullt): Da jeden Tag neue Sicherheitsrisiken ermittelt werden, werden mit **Azure Defender für Containerregistrierungen** auch alle Images gescannt, die innerhalb der letzten 30 Tage gepullt wurden. Für das erneute Scannen fallen keine zusätzlichen Kosten an. Wie oben erwähnt, erfolgt die Berechnung nur einmal pro Image.

- **On import** (Beim Importieren): Azure Container Registry verfügt über Importtools, mit denen Images aus Docker Hub, Microsoft Container Registry oder einer anderen Azure-Containerregistrierung in Ihre Registrierung eingefügt werden können. Mit **Azure Defender für Containerregistrierungen** werden alle unterstützten Images gescannt, die Sie importieren. Weitere Informationen finden Sie unter [Importieren von Containerimages in eine Containerregistrierung](../container-registry/container-registry-import-images.md).
 
Die Überprüfung dauert normalerweise nicht länger als zwei Minuten, aber der Vorgang kann auch einmal 15 Minuten in Anspruch nehmen. Die Ergebnisse werden als Security Center-Empfehlungen bereitgestellt, z. B. wie folgt:

[![Beispiel einer Azure Security Center-Empfehlung über in einem gehosteten Azure Container Registry-Image (ACR) erkannte Sicherheitsrisiken](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)


## <a name="how-does-security-center-work-with-azure-container-registry"></a>Funktionsweise von Security Center mit Azure Container Registry

Unten ist ein allgemeines Diagramm zu den Komponenten und Vorteilen angegeben, mit denen der Schutz Ihrer Registrierungen mit Security Center sichergestellt wird.

![Allgemeine Übersicht zu Azure Security Center und Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Häufig gestellte Fragen zum Überprüfen von Azure Container Registry-Images

### <a name="how-does-security-center-scan-an-image"></a>Wie überprüft Security Center ein Image?
Das Image wird aus der Registrierung gepullt. Anschließend wird es in einer isolierten Sandbox mit dem Qualys-Scanner ausgeführt, der eine Liste bekannter Sicherheitsrisiken extrahiert.

Security Center filtert und klassifiziert die Ergebnisse des Scanners. Wenn ein Image fehlerfrei ist, markiert Security Center es entsprechend. Security Center generiert Sicherheitsempfehlungen nur für Images, bei denen Probleme behoben werden müssen. Indem Sie nur benachrichtigt werden, wenn Probleme auftreten, reduziert Security Center das Potenzial von unerwünschten Informationswarnungen.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Kann ich die Scanergebnisse über die REST-API abrufen?
Ja. Die Ergebnisse befinden sich unter [Sub-Assessments Rest API](/rest/api/securitycenter/subassessments/list/) (Unterbewertungen-REST-API). Außerdem können Sie Azure Resource Graph (ARG) verwenden, die Kusto-ähnliche API für alle Ihre Ressourcen: Mit einer Abfrage kann ein bestimmter Scan abgerufen werden.
 
### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>Welche Typen von Registrierungen werden überprüft? Welche Typen werden abgerechnet?
Eine Liste der Typen von Containerregistrierungen, die von Azure Defender für Containerregistrierungen unterstützt werden, finden Sie unter [Verfügbarkeit](defender-for-container-registries-usage.md#availability).

Wenn Sie nicht unterstützte Registrierungen mit Ihrem Azure-Abonnement verbinden, werden sie nicht überprüft und Ihnen nicht in Rechnung gestellt.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Containersicherheitsfeatures von Security Center finden Sie unter:

- [Containersicherheit in Security Center](container-security.md)

- [Einführung in Azure Defender für Kubernetes](defender-for-kubernetes-introduction.md)


