---
title: Häufig gestellte Fragen zu Azure Resource Mover
description: Erhalten Sie Antworten auf häufig gestellte Fragen zu Azure Resource Mover.
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 09/14/2020
ms.author: raynew
ms.openlocfilehash: 68e5f937b8ad8367abf488598bda311a39d462c6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90600652"
---
# <a name="common-questions"></a>Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zu [Azure Resource Mover](overview.md) beantwortet.

## <a name="general"></a>Allgemein

### <a name="is-resource-mover-generally-available"></a>Ist Resource Mover allgemein verfügbar?

Azure Resource Mover befindet sich derzeit in der öffentlichen Vorschau. Produktionsworkloads werden unterstützt.



## <a name="moving-across-regions"></a>Regionsübergreifendes Verschieben

### <a name="can-i-move-resources-across-any-regions"></a>Kann ich Ressourcen in beliebige Regionen verschieben?

Derzeit können Sie Ressourcen aus beliebigen öffentlichen Quellregionen in beliebige öffentliche Zielregionen verschieben, abhängig von den [Ressourcentypen, die in dieser Region verfügbar sind](https://azure.microsoft.com/global-infrastructure/services/). Das Verschieben von Ressourcen in Azure Government-Regionen wird derzeit nicht unterstützt.

### <a name="what-resources-can-i-move-across-regions-using-resource-mover"></a>Welche Ressourcen kann ich mithilfe von Resource Mover regionsübergreifend verschieben?

Mit Resource Mover können Sie derzeit die folgenden Ressourcen regionsübergreifend verschieben:

- Azure-VMs und zugehörige Datenträger
- NICs
- Verfügbarkeitsgruppen 
- Virtuelle Azure-Netzwerke 
- Öffentliche IP-Adressen
- Netzwerksicherheitsgruppen (NSGs)
- Interne und öffentliche Load Balancer 
- Azure SQL-Datenbanken und Pools für elastische Datenbanken


### <a name="can-i-move-resources-across-subscriptions-when-i-move-them-across-regions"></a>Kann ich Ressourcen abonnementübergreifend verschieben, wenn ich sie regionsübergreifend verschiebe?

Sie können das Abonnement nach dem Verschieben von Ressourcen in die Zielregion ändern. [Erfahren Sie mehr](../azure-resource-manager/management/move-resource-group-and-subscription.md) über das Verschieben von Ressourcen in ein anderes Abonnement. 

### <a name="where-is-the-metadata-for-moving-across-regions-stored"></a>Wo werden die Metadaten für regionsübergreifendes Verschieben gespeichert?

Metadaten werden in einer [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md)-Datenbank und in [Azure Blob Storage](../storage/common/storage-service-encryption.md) in einem Microsoft-Abonnement gespeichert. Metadaten werden derzeit in den Regionen „USA, Osten 2“ und „Europa, Norden“ gespeichert. Eine Erweiterung auf andere Regionen ist geplant. Dadurch wird das Verschieben von Ressourcen in öffentlichen Regionen nicht eingeschränkt.

### <a name="is-the-collected-metadata-encrypted"></a>Werden die erfassten Metadaten verschlüsselt?

Ja, sowohl während der Übertragung als auch im Ruhezustand.
- Während der Übertragung werden die Metadaten mithilfe von HTTPS über das Internet sicher an den Resource Mover-Dienst gesendet.
- Die Metadaten werden im Speicher verschlüsselt.

### <a name="how-is-managed-identity-used-in-resource-mover"></a>Wie wird die verwaltete Identität in Resource Mover verwendet?

Eine [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) (früher als Managed Service Identity (MSI) bezeichnet) stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereit.
- Resource Mover verwendet die verwaltete Identität für den Zugriff auf Azure-Abonnements, um Ressourcen regionsübergreifend zu verschieben.
- Eine Verschiebungssammlung benötigt eine vom System zugewiesene Identität mit Zugriff auf das Abonnement, das die zu verschiebenden Ressourcen enthält.

- Wenn Sie Ressourcen regionsübergreifend im Portal verschieben, erfolgt dieser Vorgang automatisch.
- Wenn Sie Ressourcen mithilfe von PowerShell verschieben, führen Sie Cmdlets aus, um der Sammlung eine vom System zugewiesene Identität zuzuweisen. Anschließend weisen Sie dem Identitätsprinzipal eine Rolle mit den richtigen Abonnementberechtigungen zu. 

### <a name="what-managed-identity-permissions-does-resource-mover-need"></a>Welche Berechtigungen für verwaltete Identitäten benötigt Resource Mover?

Für eine verwaltete Identität von Azure Resource Mover werden mindestens die folgenden Berechtigungen benötigt: 

- Berechtigung zum Schreiben/Erstellen von Ressourcen im Benutzerabonnement, verfügbar mit der Rolle *Mitwirkender*. 
- Berechtigung zum Erstellen von Rollenzuweisungen. In der Regel in der Rolle *Besitzer*, der Rolle *Benutzerzugriffsadministrator* sowie benutzerdefinierten Rollen mit zugewiesener Berechtigung *Microsoft.Authorization/role assignments/write* enthalten. Diese Berechtigung ist nicht erforderlich, wenn der verwalteten Identität der Data Share-Ressource bereits Zugriff auf den Azure-Datenspeicher gewährt wurde. 
 
Wenn Sie über das Portal Ressourcen auf dem Resource Mover-Hub hinzufügen, werden Berechtigungen automatisch verarbeitet, sofern der Benutzer über die oben beschriebenen Berechtigungen verfügt. Wenn Sie Ressourcen mit PowerShell hinzufügen, weisen Sie Berechtigungen manuell zu.

> [!IMPORTANT]
> Es wird dringend empfohlen, Identitätsrollenzuweisungen nicht zu ändern oder zu entfernen. 

### <a name="what-should-i-do-if-i-dont-have-permissions-to-assign-role-identity"></a>Wie sollte ich vorgehen, wenn ich keine Berechtigungen zum Zuweisen von Rollenidentitäten habe?

**Mögliche Ursache** | **Empfehlung**
--- | ---
Sie sind kein *Mitwirkender* und *Benutzerzugriffsadministrator* (oder *Besitzer*), wenn Sie eine Ressource zum ersten Mal hinzufügen. | Verwenden Sie ein Konto mit den Berechtigungen *Mitwirkenden* und *Benutzerzugriffsadministrator* (oder *Besitzer*) für das Abonnement.
Die verwaltete Identität von Resource Mover verfügt nicht über die erforderliche Rolle. | Fügen Sie die Rollen „Mitwirkender“ und „Benutzerzugriffsadministrator“ hinzu.
Die verwaltete Identität von Resource Mover wurde auf *Keine* zurückgesetzt. | Aktivieren Sie eine vom System zugewiesene Identität unter „Verschiebungssammlung > **Identität**“ erneut. Alternativ können Sie die Ressource unter **Ressourcen hinzufügen** erneut hinzufügen. Das Ergebnis ist identisch.  
Das Abonnement wurde in einen anderen Mandanten verschoben. | Deaktivieren und aktivieren Sie Verschiebungssammlung.

### <a name="how-can-i-do-multiple-moves-together"></a>Wie kann ich mehrere Verschiebungen gleichzeitig ausführen?

Ändern Sie die Quell-/Zielkombinationen nach Bedarf mithilfe der Änderungsoption im Portal.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](about-move-process.md) über Resource Mover-Komponenten und den Verschiebungsvorgang.
