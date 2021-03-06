---
title: Dienstkontingente und Limits
description: Erfahren Sie mehr über Azure Batch-Standardkontingente, Limits und Einschränkungen sowie die Anforderung von Kontingentsteigerungen.
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: 4c13df8b537d701400a22cd2871e7f8362f02455
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84417275"
---
# <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits

Ebenso wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen in Verbindung mit dem Batch-Dienst Limits. Viele dieser Limits sind Standardkontingente, die von Azure auf Abonnement- oder Kontoebene angewendet werden.

Bedenken Sie diese Kontingente beim Entwerfen und Hochskalieren Ihrer Batchworkloads. Wenn Ihr Pool beispielsweise nicht die von Ihnen vorgegebene Anzahl von Computeknoten erreicht, haben Sie möglicherweise die Kernkontingentgrenze für Ihr Batch-Konto erreicht.

Sie können mehrere Batch-Workloads in einem Batch-Konto ausführen oder Ihre Workloads auf Batch-Konten in demselben Abonnement, aber verschiedenen Azure-Regionen aufteilen.

Wenn Sie Produktionsworkloads in Batch ausführen möchten, müssen Sie möglicherweise ein oder mehrere Kontingente über den Standardwert erhöhen. Wenn Sie ein Kontingent erhöhen möchten, können Sie kostenlos eine [Anfrage an den Onlinekundensupport](#increase-a-quota) richten.

## <a name="resource-quotas"></a>Ressourcenkontingente

Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support.

Beachten Sie, dass Kontingente keine garantierten Werte darstellen. Kontingente können basierend auf Änderungen vom Batch-Dienst oder aufgrund einer Benutzeranforderung für eine Änderung des Kontingentwerts variieren.

[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

### <a name="cores-quotas-in-user-subscription-mode"></a>Kontingente für Kerne im Modus „Benutzerabonnement“

Wenn Sie ein [Batch-Konto](accounts.md) erstellen, bei dem der Poolzuordnungsmodus auf **Benutzerabonnement** festgelegt ist, werden Kontingente unterschiedlich angewendet. In diesem Modus werden Batch-VMs und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Die Kontingente für Kerne von Azure Batch gilt nicht für Konten, die in diesem Modus erstellt werden. Stattdessen werden die Kontingente in Ihrem Abonnement für regionale Computekerne und andere Ressourcen angewendet.

Weitere Informationen zu diesen Kontingenten finden Sie unter [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="pool-size-limits"></a>Poolgrößenbeschränkungen

Die Poolgrößenbeschränkungen werden vom Batch-Dienst festgelegt. Im Gegensatz zu [Ressourcenkontingenten](#resource-quotas) können diese Werte nicht geändert werden. Nur bei Pools mit Kommunikation zwischen den einzelnen Knoten und benutzerdefinierten Images weichen die Einschränkungen vom Standardkontingent ab.

| **Ressource** | **Maximales Limit** |
| --- | --- |
| **Serverknoten im Pool mit [aktivierter Kommunikation zwischen den Knoten](batch-mpi.md)**  ||
| Poolzuordnungsmodus für den Batch-Dienst | 100 |
| Poolzuordnungsmodus für das Batch-Abonnement | 80 |
| **Serverknoten in einem [Pool, der mit einer Ressource mit verwaltetem Image erstellt wurde](batch-custom-images.md)** <sup>1</sup> ||
| Dedizierte Knoten | 2000 |
| Knoten mit niedriger Priorität | 1000 |

<sup>1</sup> Für Pools, bei denen die Kommunikation zwischen den Knoten nicht aktiviert ist

## <a name="other-limits"></a>Andere Limits

Zusätzliche Beschränkungen, die vom Batch-Dienst festgelegt werden. Im Gegensatz zu [Ressourcenkontingenten](#resource-quotas) können diese Werte nicht geändert werden.

| **Ressource** | **Maximales Limit** |
| --- | --- |
| [Gleichzeitige Aufgaben](batch-parallel-node-tasks.md) pro Computeknoten | 4 x Anzahl der Kerne des Knotens |
| [Anwendungen](batch-application-packages.md) pro Batch-Konto | 20 |
| Anwendungspakete pro Anwendung. | 40 |
| Anwendungspakete pro Pool | 10 |
| Maximale Lebensdauer von Tasks | 180 Tage<sup>1</sup> |
| [Bereitstellungen](virtual-file-mount.md) pro Computeknoten | 10 |

<sup>1</sup> Die maximale Lebensdauer eines Tasks (vom Hinzufügen zum Auftrag bis zum Abschluss) beträgt 180 Tage. Abgeschlossene Tasks bleiben sieben Tage lang erhalten. Daten für Tasks, die nicht innerhalb der maximalen Lebensdauer abgeschlossen wurden, stehen hingegen nicht zur Verfügung.

## <a name="view-batch-quotas"></a>Anzeigen von Batch-Kontingenten

So zeigen Sie die Kontingente Ihres Batch-Kontos im [Azure-Portal](https://portal.azure.com) an:

1. Wählen Sie **Batch-Konten** und dann das Batch-Konto aus, das Sie interessiert.
1. Wählen Sie im Menü des Batch-Kontos die Option **Kontingente** aus.
1. Zeigen Sie die Kontingente an, die derzeit für das Batch-Konto gelten.

    ![Batch-Kontokontingente][account_quotas]

## <a name="increase-a-quota"></a>Erhöhen eines Kontingents

Sie können über das [Azure-Portal](https://portal.azure.com) eine Kontingenterhöhung für Ihr Batch-Konto oder Ihr Abonnement anfordern. Die Art der Kontingenterhöhung richtet sich nach dem Poolzuordnungsmodus Ihres Batch-Kontos. Um eine Erhöhung des Kontingents anzufordern, müssen Sie die VM-Serien angeben, deren Kontingent erhöht werden soll. Wenn die Erhöhung des Kontingents angewendet wird, gilt sie für alle VM-Serien.

1. Wählen Sie auf Ihrem Portaldashboard die Kachel **Hilfe und Support** oder das Fragezeichen ( **?** ) in der oberen rechten Ecke des Portals.
1. Wählen Sie **Neue Supportanfrage** > **Grundlagen** aus.
1. Gehen Sie unter **Grundlegende Einstellungen** wie folgt vor:
   
    1. **Problemtyp** > **Grenzwerte für Dienste und Abonnements (Kontingente)**
   
    1. Wählen Sie Ihr Abonnement aus.
   
    1. **Kontingenttyp** > **Batch**
      
       Wählen Sie **Weiter** aus.
    
1. Unter **Details** finden Sie folgende Informationen:
      
    1. Geben Sie unter **Details angeben** den Speicherort, den Kontingenttyp und das Batch-Konto an.
    
       ![Erhöhen des Batch-Kontingente][quota_increase]

       Es gibt folgende Kontingenttypen:

       * **Pro Batch-Konto**  
         Spezifische Werte für ein einzelnes Batch-Konto, einschließlich dedizierter und mit niedriger Priorität versehener Kerne, sowie die Anzahl der Aufträge und Pools.
        
       * **Pro Region**  
         Werte, die für alle Batch-Konten in einer Region gelten und die Anzahl der Batch-Konten pro Region und Abonnement beinhalten.

       Das Kontingent mit niedriger Priorität ist ein einzelner Wert für alle VM-Serien. Wenn Sie eingeschränkte SKUs benötigen, müssen Sie **Kerne mit niedriger Priorität** auswählen und die anzufordernden VM-Produktfamilien einschließen.

    1. Wählen Sie einen **Schweregrad** gemäß der [geschäftlichen Auswirkung](https://aka.ms/supportseverity) aus.

       Wählen Sie **Weiter** aus.

1. Gehen Sie unter **Kontaktinformationen** wie folgt vor:
   
    1. Wählen Sie eine **bevorzugte Kontaktmethode**aus.
   
    1. Überprüfen Sie die erforderlichen Kontaktdetails, und geben Sie sie ein.
   
       Wählen Sie **Erstellen** aus, um die Supportanfrage zu übermitteln.

Nachdem Sie die Supportanfrage übermittelt haben, wird sich der Azure-Support mit Ihnen in Verbindung setzen. Kontingentanfragen können innerhalb weniger Minuten abgeschlossen werden oder bis zu zwei Werktage dauern.

## <a name="related-quotas-for-vm-pools"></a>Verknüpfte Kontingente für VM-Pools

Batch-Pools in der Konfiguration von virtuellen Computern, die in einem virtuellen Azure-Network bereitgestellt wurden, ordnen automatisch zusätzliche Azure-Netzwerkressourcen zu. Die folgenden Ressourcen sind für alle 50 Poolknoten in einem virtuellen Netzwerk erforderlich:

- Eine [Netzwerksicherheitsgruppe](../virtual-network/security-overview.md#network-security-groups)
- Eine [öffentliche IP-Adresse](../virtual-network/public-ip-addresses.md)
- Ein [Load Balancer](../load-balancer/load-balancer-overview.md)

Diese Ressourcen werden in dem Abonnement zugeordnet, das das virtuelle Netzwerk enthält, das beim Erstellen des Batch-Pools bereitgestellt wurde. Diese Ressourcen werden durch die [Ressourcenkontingente](../azure-resource-manager/management/azure-subscription-service-limits.md) des Abonnements beschränkt. Wenn Sie große Poolbereitstellungen in einem virtuellen Netzwerk planen, überprüfen Sie die Kontingente des Abonnements für diese Ressourcen. Fordern Sie bei Bedarf eine Erhöhung im Azure-Portal an, indem Sie **Hilfe und Support** auswählen.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten eines Azure Batch-Kontos im Azure-Portal](batch-account-create-portal.md).
* Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
* Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.png
[quota_increase]: ./media/batch-quota-limit/quota-increase.png
