---
title: Sichern und Wiederherstellen von verschlüsselten virtuellen Azure-Computern
description: Beschreibt, wie verschlüsselte virtuelle Azure-Computer (VMs) mit dem Azure Backup-Dienst gesichert und wiederhergestellt werden.
ms.topic: conceptual
ms.date: 08/18/2020
ms.openlocfilehash: 6ce0068203c91d9d2031ce2f8735cccf94172dd8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89014913"
---
# <a name="back-up-and-restore-encrypted-azure-virtual-machines"></a>Sichern und Wiederherstellen verschlüsselter virtueller Azure-Computer

In diesem Artikel wird beschrieben, wie Sie virtuelle Azure-Computer (VMs) unter Windows oder Linux mit verschlüsselten Datenträgern mithilfe des Diensts [Azure Backup](backup-overview.md) sichern und wiederherstellen. Weitere Informationen finden Sie unter [Verschlüsseln von Azure-VM-Sicherungen](backup-azure-vms-introduction.md#encryption-of-azure-vm-backups).

## <a name="encryption-using-platform-managed-keys"></a>Verschlüsselung mithilfe plattformseitig verwalteter Schlüssel

Standardmäßig werden alle Datenträger in Ihren VMs automatisch im Ruhezustand mit plattformseitig verwalteten Schlüsseln (PMK) verschlüsselt, die [Speicherdienstverschlüsselung (SSE)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) verwenden. Sie können diese VMs mit Azure Backup sichern, ohne dass spezifische Aktionen zur Unterstützung der Verschlüsselung auf Ihrer Seite erforderlich sind. Weitere Informationen zur Verschlüsselung mit plattformseitig verwalteten Schlüsseln [finden Sie in diesem Artikel](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#platform-managed-keys).

![Verschlüsselte Datenträger](./media/backup-encryption/encrypted-disks.png)

## <a name="encryption-using-customer-managed-keys"></a>Verschlüsselung mithilfe kundenseitig verwalteter Schlüssel

Wenn Sie Datenträger mit kundenseitig verwalteten Schlüsseln (CMK) verschlüsseln, wird der für die Verschlüsselung der Datenträger verwendete Schlüssel im Azure Key Vault gespeichert und von Ihnen verwaltet. Speicherdienstverschlüsselung (SSE) mit CMK unterscheidet sich von der Azure Disk Encryption (ADE)-Verschlüsselung. ADE verwendet die Verschlüsselungstools des Betriebssystems. SSE verschlüsselt Daten im Speicherdienst, sodass Sie beliebige Betriebssysteme oder Images für Ihre VMs verwenden können. Weitere Informationen zur Verschlüsselung verwalteter Datenträger mit kundenseitig verwalteten Schlüsseln [finden Sie in diesem Artikel](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys).

## <a name="encryption-support-using-ade"></a>Verschlüsselungsunterstützung mit ADE

Azure Backup unterstützt die Sicherung von Azure-VMs, deren Betriebssystem/Datenträger mit Azure Disk Encryption (ADE) verschlüsselt wurde(n). ADE verwendet BitLocker für die Verschlüsselung von Windows-VMs und die Funktion „dm-crypt“ für Linux-VMs. ADE ist in Azure Key Vault integriert, um die Verwaltung von Datenträger-Verschlüsselungsschlüsseln und Geheimnissen zu erleichtern. Key Vault-Schlüssel für die Schlüsselverschlüsselung (Key Encryption Keys, KEKs) können verwendet werden, um eine zusätzliche Sicherungsebene hinzuzufügen. Sie dient zum Verschlüsseln von Verschlüsselungsgeheimnissen, bevor sie in Key Vault geschrieben werden.

Wie in der nachstehenden Tabelle zusammengefasst, kann Azure Backup Azure-VMs mithilfe von ADE mit und ohne die Azure AD-App sichern und wiederherstellen.

**VM-Datenträgertyp** | **ADE (BEK/dm-crypt)** | **ADE und KEK**
--- | --- | ---
**Nicht verwaltet** | Ja | Ja
**Verwaltet**  | Ja | Ja

- Weitere Informationen zu [ADE](../security/fundamentals/azure-disk-encryption-vms-vmss.md), [Key Vault](../key-vault/general/overview.md) und [KEKs](../virtual-machine-scale-sets/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek).
- Lesen Sie [Häufig gestellte Fragen](../security/fundamentals/azure-disk-encryption-vms-vmss.md) zur Datenträgerverschlüsselung für virtuelle Azure-Computer.

### <a name="limitations"></a>Einschränkungen

- Sie können verschlüsselte VMs innerhalb desselben Abonnements und derselben Region sichern und wiederherstellen.
- Azure Backup unterstützt mit eigenständigen Schlüsseln verschlüsselte VMs. Ein Schlüssel, der Teil eines Zertifikats ist, das zum Verschlüsseln eines virtuellen Computers verwendet wurde, wird derzeit nicht unterstützt.
- Sie können verschlüsselte VMs innerhalb desselben Abonnements und derselben Region als Recovery Services-Sicherungstresor sichern und wiederherstellen.
- Verschlüsselte virtuelle Computer können nicht auf Datei- oder Ordnerebene wiederhergestellt werden. Sie müssen den gesamten virtuellen Computer wiederherstellen, damit Dateien und Ordner wiederhergestellt werden.
- Beim Wiederherstellen eines virtuellen Computers können Sie die Option [Vorhandenen virtuellen Computer ersetzen](backup-azure-arm-restore-vms.md#restore-options) für verschlüsselte virtuelle Computer nicht verwenden. Diese Option wird nur bei unverschlüsselten verwalteten Datenträgern unterstützt.

## <a name="before-you-start"></a>Vorbereitung

Führen Sie zunächst folgende Schritte aus:

1. Stellen Sie sicher, dass Sie über einen oder mehrere virtuelle [Windows](../virtual-machines/linux/disk-encryption-overview.md)- oder [Linux](../virtual-machines/linux/disk-encryption-overview.md)-Computer mit aktiviertem ADE verfügen.
2. Sehen Sie sich die [Unterstützungsmatrix](backup-support-matrix-iaas.md) für die Sicherung virtueller Azure-Computer an.
3. [Erstellen Sie](backup-create-rs-vault.md) einen Recovery Services-Sicherungstresor, wenn Sie noch keinen haben.
4. Wenn Sie die Verschlüsselung für VMs aktivieren, die bereits für Sicherung aktiviert wurden, müssen Sie Backup einfach Berechtigungen für den Zugriff auf den Key Vault gewähren, damit Sicherungen ohne Unterbrechung fortgesetzt werden können. [Erfahren Sie mehr](#provide-permissions) zum Zuweisen dieser Berechtigungen.

Darüber hinaus gibt es einige Schritte, die Sie in bestimmten Fällen möglicherweise ausführen müssen:

- **Installieren des VM-Agents auf dem virtuellen Computer:** Azure Backup sichert Azure-VMs durch die Installation einer Erweiterung für den Azure-VM-Agent auf dem Computer. Wenn Ihre VM aus einem Azure Marketplace-Image erstellt wurde, ist der Agent installiert und aktiv. Wenn Sie eine benutzerdefinierte VM erstellen oder einen lokalen Computer migrieren, müssen Sie möglicherweise [den Agent manuell installieren](backup-azure-arm-vms-prepare.md#install-the-vm-agent).

## <a name="configure-a-backup-policy"></a>Konfigurieren einer Sicherungsrichtlinie

1. Wenn Sie noch keinen Recovery Services-Sicherungstresor erstellt haben, folgen Sie [diesen Anweisungen](backup-create-rs-vault.md).
1. Öffnen Sie den Tresor im Portal, und wählen Sie im Abschnitt **Übersicht** die Option **Sicherung** aus.

    ![Bereich „Sicherung“](./media/backup-azure-vms-encryption/select-backup.png)

1. Wählen Sie unter **Sicherungsziel** > **Wo wird Ihre Workload ausgeführt?** den Eintrag **Azure** aus.
1. Wählen Sie für **Was möchten Sie sichern?** die Option **Virtueller Computer** aus. Wählen Sie dann **Sichern** aus.

      ![Szenariobereich](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

1. Wählen Sie unter **Sicherungsrichtlinie** > **Sicherungsrichtlinie auswählen** die Richtlinie aus, die Sie dem Tresor zuordnen möchten. Klicken Sie anschließend auf **OK**.
    - Eine Sicherungsrichtlinie gibt an, wann Sicherungen erstellt und wie lange sie gespeichert werden.
    - Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

1. Wenn Sie nicht die Standardrichtlinie verwenden möchten, wählen Sie **Neu erstellen** und [Benutzerdefinierte Richtlinie erstellen](backup-azure-arm-vms-prepare.md#create-a-custom-policy) aus.

1. Wählen Sie in unter **Virtuelle Computer** die Option **Hinzufügen** aus.

    ![Virtuelle Computer hinzufügen](./media/backup-azure-vms-encryption/add-virtual-machines.png)

1. Wählen Sie die verschlüsselten VMs, die Sie mit der ausgewählten Richtlinie sichern möchten, und dann **OK** aus.

      ![Auswählen von verschlüsselten VMs](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

1. Wenn Sie Azure Key Vault verwenden, werden Sie auf der Tresorseite in einer Meldung informiert, dass Azure Backup schreibgeschützten Zugriff auf die Schlüssel und Geheimnisse im Key Vault benötigt.

    - Wenn diese Meldung angezeigt wird, ist keine Aktion erforderlich.

        ![Zugriff OK](./media/backup-azure-vms-encryption/access-ok.png)

    - Wenn diese Meldung angezeigt wird, müssen Sie Berechtigungen entsprechend der Beschreibung im [Verfahren unten](#provide-permissions) festlegen.

        ![Zugriff Warnung](./media/backup-azure-vms-encryption/access-warning.png)

1. Wählen Sie **Sicherung aktivieren** aus, um die Sicherungsrichtlinie im Tresor bereitzustellen, und aktivieren Sie die Sicherung für die ausgewählten VMs.

## <a name="trigger-a-backup-job"></a>Auslösen eines Sicherungsauftrags

Die erste Sicherung wird entsprechend dem festgelegten Zeitplan ausgeführt; Sie können sie aber auch mit den folgenden Schritten sofort ausführen:

1. Wählen Sie im Tresormenü die Option **Sicherungselemente** aus.
2. Wählen Sie unter **Sicherungselemente** die Option **Virtueller Azure-Computer** aus.
3. Wählen Sie in der Liste **Sicherungselemente** das Auslassungszeichen (...) aus.
4. Wählen Sie **Jetzt sichern** aus.
5. Verwenden Sie unter **Jetzt sichern** den Kalender, um den letzten Tag zur Beibehaltung des Wiederherstellungspunkts auszuwählen. Klicken Sie anschließend auf **OK**.
6. Überwachen Sie die Portalbenachrichtigungen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > **In Bearbeitung** überwachen. Je nach Größe Ihrer VM kann das Erstellen der ersten Sicherung einige Zeit dauern.

## <a name="provide-permissions"></a>Gewähren von Berechtigungen

Azure Backup benötigt schreibgeschützten Zugriff, um die Schlüssel und Geheimnisse zusammen mit den zugeordneten VMs zu sichern.

- Ihr Key Vault ist dem Azure AD-Mandanten des Azure-Abonnements zugeordnet. Wenn Sie ein **Mitgliedsbenutzer** sind, erhält Azure Backup ohne weitere Aktion Zugriff auf den Key Vault.
- Wenn Sie ein **Gastbenutzer** sind, müssen Sie Berechtigungen gewähren, damit Azure Backup auf den Schlüsseltresor zugreifen kann.

So legen Sie Berechtigungen fest:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Schlüsseltresore**.
1. Wählen Sie den Schlüsseltresor aus, der dem verschlüsselten virtuellen Computer zugeordnet ist, den Sie sichern möchten.
1. Wählen Sie **Zugriffsrichtlinien** > **Zugriffsrichtlinie hinzufügen** aus.

    ![Zugriffsrichtlinie hinzufügen](./media/backup-azure-vms-encryption/add-access-policy.png)

1. Wählen Sie unter **Zugriffsrichtlinie hinzufügen** > **Anhand einer Vorlage konfigurieren (optional)** die Option **Azure Backup** aus.
    - Unter **Schlüsselberechtigungen** und **Berechtigungen für Geheimnis** sind bereits die erforderlichen Berechtigungen angegeben.
    - Wenn Ihr virtueller Computer mithilfe von **Nur BEK** verschlüsselt ist, entfernen Sie die Auswahl für **Schlüsselberechtigungen**, da Sie Berechtigungen nur für Geheimnisse benötigen.

    ![Auswählen von Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Wählen Sie **Hinzufügen**. **Sicherungsverwaltungsdienst** wird zu **Zugriffsrichtlinien** hinzugefügt.

    ![Zugriffsrichtlinien](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Wählen Sie **Speichern** aus, um Azure Backup die Berechtigungen zu erteilen.

## <a name="restore-an-encrypted-vm"></a>Wiederherstellen eines verschlüsselten virtuellen Computers

Verschlüsselte virtuelle Computer können nur durch Wiederherstellen des VM-Datenträgers wiederhergestellt werden, wie weiter unten erläutert. Die Funktionen **Vorhandene ersetzen** und **Virtuellen Computer wiederherstellen** werden nicht unterstützt.

Stellen Sie verschlüsselte virtuelle Computer wie folgt wieder her:

1. [Stellen Sie den VM-Datenträger wieder her](backup-azure-arm-restore-vms.md#restore-disks).
2. Erstellen Sie die VM-Instanz neu, indem Sie eine der folgenden Aktionen ausführen:
    1. Verwenden Sie die Vorlage, die während des Wiederherstellungsvorgangs generiert wurde, um VM-Einstellungen anzupassen und die Bereitstellung der VM auszulösen. [Weitere Informationen](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm)
    2. Erstellen Sie mithilfe von PowerShell eine neue VM aus den wiederhergestellten Datenträgern. [Weitere Informationen](backup-azure-vms-automation.md#create-a-vm-from-restored-disks)
3. Installieren Sie für virtuelle Linux-Computer die ADE-Erweiterung neu, damit die Datenträger offen und eingebunden sind.

## <a name="next-steps"></a>Nächste Schritte

Sollten Probleme auftreten, sehen Sie sich die folgenden Artikel an:

- [Häufige Fehler](backup-azure-vms-troubleshoot.md) beim Sichern und Wiederherstellen von verschlüsselten virtuellen Azure-Computern.
- Probleme im Zusammenhang mit [Azure VM-Agent/Backup-Erweiterung](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md).
