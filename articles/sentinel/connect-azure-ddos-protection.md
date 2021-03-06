---
title: Verbinden von Azure DDoS Protection-Daten mit Azure Sentinel
description: Es wird beschrieben, wie Sie Azure DDoS Protection-Daten in Azure Sentinel erfassen, damit Sie sie anzeigen, analysieren und untersuchen können.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 09/01/2020
ms.author: yelevin
ms.openlocfilehash: e8e44f69965af1987bd5f023644d966b3caf1c77
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89505776"
---
# <a name="connect-data-from-azure-ddos-protection"></a>Herstellen einer Verbindung für Daten aus Azure DDoS Protection

> [!IMPORTANT]
> Der Azure DDoS Protection-Datenconnector in Azure Sentinel befindet sich derzeit in der öffentlichen Vorschauphase (Public Preview).
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Ein DDoS-Angriff (Distributed Denial of Service) hat das Ziel, die Ressourcen einer Anwendung zu verbrauchen, damit diese für berechtigte Benutzer nicht mehr verfügbar ist. Jeder Endpunkt, der öffentlich über das Internet erreichbar ist, kann Ziel von DDoS-Angriffen werden. Die Verwendung von [Azure DDoS Protection](../virtual-network/ddos-protection-overview.md) in Kombination mit bewährten Anwendungsentwurfsmethoden stellt einen zuverlässigen Schutz vor DDoS-Angriffen dar. Sie können Azure DDoS Protection-Protokolle mit Azure Sentinel verbinden. Dies ermöglicht es Ihnen, Protokolldaten in Arbeitsmappen anzuzeigen, sie zum Erstellen benutzerdefinierter Warnungen zu verwenden und sie zur Verbesserung Ihrer Untersuchungen zu integrieren. 

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen Lese- und Schreibberechtigungen für den Azure Sentinel-Arbeitsbereich.

- Sie müssen über einen konfigurierten [Azure DDoS Protection-Standardplan](../virtual-network/manage-ddos-protection.md#create-a-ddos-protection-plan) verfügen.

- Sie müssen über ein konfiguriertes [virtuelles Netzwerk mit Aktivierung von Azure DDoS Standard](../virtual-network/manage-ddos-protection.md#enable-ddos-for-a-new-virtual-network) verfügen.

## <a name="connect-to-azure-ddos-protection"></a>Herstellen einer Verbindung mit Azure DDoS Protection
    
1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Data connectors** (Datenconnectors).

1. Wählen Sie im Katalog für Datenconnectors die Option **Azure DDoS Protection** und dann im Vorschaubereich **Connectorseite öffnen** aus.

1. Aktivieren Sie **Diagnoseprotokolle** für alle Firewalls, deren Protokolle Sie verbinden möchten:

    1. Wählen Sie den Link **Diagnoseeinstellungen öffnen >** und dann in der Liste eine Ressource vom Typ **Öffentliche IP-Adresse** aus.

    1. Wählen Sie **+Diagnoseeinstellung hinzufügen**  aus.

    1. Unter **Diagnoseeinstellungen**:
       - Geben Sie im Feld **Name der Diagnoseeinstellung** einen Namen ein.

       - Aktivieren Sie das Kontrollkästchen **An Log Analytics senden**. Zwei neue Felder werden darunter angezeigt. Wählen Sie das relevante **Abonnement** und den **Log Analytics-Arbeitsbereich** (in dem sich Azure Sentinel befindet) aus.

       - Aktivieren Sie die Kontrollkästchen der Regeltypen, deren Protokolle Sie erfassen möchten. Wir empfehlen Ihnen die Verwendung von **DDoSProtectionNotifications**, **DDoSMitigationFlowLogs** und **DDoSMitigationReports**.

    1. Klicken Sie am oberen Bildschirmrand auf **Speichern**. Wiederholen Sie diesen Prozess für alle zusätzlichen Firewalls (öffentliche IP-Adressen), für die Sie den DDoS-Schutz aktiviert haben.

1. Suchen Sie nach **AzureDiagnostics**, um in Log Analytics das relevante Schema für Azure DDoS Protection-Warnungen zu verwenden.

> [!NOTE]
>
> Mit diesem speziellen Datenconnector werden die Konnektivitätsstatusindikatoren (ein Farbstreifen im Datenconnectors-Katalog sowie Verbindungssymbole neben den Datentypnamen) nur dann als *verbunden* (grün) angezeigt, wenn Daten irgendwann innerhalb der letzten zwei Wochen erfasst wurden. Wenn zwei Wochen ohne Datenerfassung vergangen sind, wird der Connector als „getrennt“ angezeigt. In dem Moment, in dem weitere Daten den Connector passieren, wird der Status wieder als *verbunden* angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie für Azure DDoS Protection-Protokolle eine Verbindung mit Azure Sentinel herstellen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
