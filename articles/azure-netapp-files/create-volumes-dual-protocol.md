---
title: Erstellen eines Volumes mit dualem Protokoll (NFSv3 und SMB) für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie ein Volume erstellen, das ein duales Protokoll aus NFSv3 und SMB mit Unterstützung der LDAP-Benutzerzuordnung verwendet.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 8/11/2020
ms.author: b-juche
ms.openlocfilehash: dcdb3e8ce545227bc11cc60e3885c1a985ed34f4
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89459996"
---
# <a name="create-a-dual-protocol-nfsv3-and-smb-volume-for-azure-netapp-files"></a>Erstellen eines Volumes mit dualem Protokoll (NFSv3 und SMB) für Azure NetApp Files

Azure NetApp Files unterstützt das Erstellen von Volumes mithilfe von NFS (NFSv3 und NFSv4.1), SMBv3 oder dualem Protokoll. In diesem Artikel wird beschrieben, wie Sie ein Volume erstellen, das ein duales Protokoll aus NFSv3 und SMB mit Unterstützung der LDAP-Benutzerzuordnung verwendet.  


## <a name="before-you-begin"></a>Voraussetzungen 

* Sie müssen bereits einen Kapazitätspool eingerichtet haben.  
    Siehe [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md).   
* Ein Subnetz muss an Azure NetApp Files delegiert werden.  
    Siehe [Delegieren eines Subnetzes an Azure NetApp Files](azure-netapp-files-delegate-subnet.md).

## <a name="considerations"></a>Überlegungen

* Achten Sie darauf, die [Anforderungen für Active Directory-Verbindungen](azure-netapp-files-create-volumes-smb.md#requirements-for-active-directory-connections) zu erfüllen. 
* Erstellen Sie eine Reverse-Lookup-Zone auf dem DNS-Server, und fügen Sie dann einen Zeigereintrag (PTR) des AD-Hostcomputers in dieser Reverse-Lookup-Zone hinzu. Andernfalls kann das Volume mit dualem Protokoll nicht erstellt werden.
* Stellen Sie sicher, dass der NFS-Client auf dem neuesten Stand ist, und führen Sie die neuesten Updates für das Betriebssystem aus.

## <a name="create-a-dual-protocol-volume"></a>Erstellen eines Dual-Protokoll-Volumes

1.  Klicken Sie auf dem Blatt „Kapazitätspools“ auf das Blatt **Volumes**. Klicken Sie auf **+ Volume hinzufügen**, um ein Volume zu erstellen. 

    ![Navigieren zu Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png) 

2.  Klicken Sie im Fenster „Volume erstellen“ auf **Erstellen**, und geben Sie auf der Registerkarte „Grundlagen“ Informationen in den folgenden Feldern an:   
    * **Volumename**      
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   

        Ein Volumename muss innerhalb der einzelnen Kapazitätspools eindeutig sein. Er muss mindestens drei Zeichen lang sein. Sie dürfen beliebige alphanumerische Zeichen verwenden.   

        `default` kann nicht als Volumename verwendet werden.

    * **Kapazitätspool**  
        Geben Sie den Kapazitätspool an, in dem das Volume erstellt werden soll.

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.  

    * **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNET) an, von dem aus Sie auf das Volume zugreifen möchten.  

        Das von Ihnen angegebene VNET muss über ein an Azure NetApp Files delegiertes Subnetz verfügen. Auf den Azure NetApp Files-Dienst kann nur vom gleichen VNET aus oder per VNET-Peering von einem VNET aus zugegriffen werden, das sich in der gleichen Region befindet wie das Volume. Sie können auch über ExpressRoute von Ihrem lokalen Netzwerk aus auf das Volume zugreifen.   

    * **Subnetz**  
        Geben Sie das Subnetz an, das Sie für das Volume verwenden möchten.  
        Das von Ihnen angegebene Subnetz muss an Azure NetApp Files delegiert werden. 
        
        Wenn Sie kein Subnetz delegiert haben, klicken Sie auf der Seite „Volume erstellen“ auf **Neu erstellen**. Geben Sie dann auf der Seite „Subnetz erstellen“ die Subnetzinformationen an, und wählen Sie **Microsoft.NetApp/volumes** aus, um das Subnetz für Azure NetApp Files zu delegieren. In jedem VNET kann nur ein Subnetz an Azure NetApp Files delegiert werden.   
 
        ![Erstellen eines Volumes](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Erstellen eines Subnetzes](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Wenn Sie eine vorhandene Momentaufnahmerichtlinie auf das Volume anwenden möchten, klicken Sie auf **Abschnitt „Erweitert“ anzeigen**, um den Bereich zu erweitern, geben Sie an, ob Sie den Momentaufnahmepfad ausblenden möchten, und wählen Sie im Pulldownmenü eine Momentaufnahmenrichtlinie aus. 

        Informationen zum Erstellen einer Momentaufnahmenrichtlinie finden Sie unter [Verwalten von Momentaufnahmenrichtlinien](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

        ![Abschnitt „Erweitert“ anzeigen](../media/azure-netapp-files/volume-create-advanced-selection.png)

3. Klicken Sie auf **Protokoll**, und führen Sie anschließend die folgenden Aktionen aus:  
    * Wählen Sie **Dual-protocol (NFSv3 and SMB)** (Duales Protokoll (NFSv3 und SMB)) als Protokolltyp für das Volume aus.   

    * Wählen Sie in der Dropdownliste die **Active Directory**-Verbindung aus.  
    Die verwendete Active Directory-Verbindung muss über ein Serverzertifikat der Stammzertifizierungsstelle verfügen. 

    * Geben Sie den **Volumepfad** für das Volume an.   
    Der Volumepfad ist der Name des freigegebenen Volumes. Der Name muss mit einem alphabetischen Zeichen beginnen und innerhalb jedes Abonnements und jeder Region eindeutig sein.  

    * Geben Sie den zu verwendenden **Sicherheitsstil** an: NTFS (Standard) oder UNIX.

    * Optional können Sie [die Exportrichtlinie für das Volume konfigurieren](azure-netapp-files-configure-export-policy.md).

    ![Angeben eines dualen Protokolls](../media/azure-netapp-files/create-volume-protocol-dual.png)

4. Klicken Sie auf **Bewerten + erstellen**, um die Volumedetails zu überprüfen. Klicken Sie dann auf **Erstellen**, um das Volume zu erstellen.

    Das von Ihnen erstellte Volume wird auf der Seite „Volumes“ angezeigt. 
 
    Ein Volume erbt Abonnement-, Ressourcengruppen- und Standortattribute aus dem Kapazitätspool. Den Volumebereitstellungsstatus können Sie auf der Benachrichtigungsregisterkarte überwachen.

## <a name="upload-active-directory-certificate-authority-public-root-certificate"></a>Hochladen des öffentlichen Stammzertifikats der Active Directory-Zertifizierungsstelle  

1.  Führen Sie die unter [Installieren der Zertifizierungsstelle](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) angegebenen Schritte zum Installieren und Konfigurieren der AD DS-Zertifizierungsstelle aus. 

2.  Führen Sie die unter [Anzeigen von Zertifikaten mit dem MMC-Snap-In](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) angegebenen Schritte aus, um das MMC-Snap-In und das Tool „Zertifikat-Manager“ zu verwenden.  
    Verwenden Sie das Snap-In „Zertifikat-Manager“, um das Stamm- oder Ausstellerzertifikat für das lokale Gerät zu suchen. Sie sollten die Befehle des Snap-Ins „Zertifikatverwaltung“ über eine der folgenden Einstellungen ausführen:  
    * Windows-basierter Client, der der Domäne hinzugefügt wurde und auf dem das Stammzertifikat installiert ist 
    * Anderer Computer in der Domäne mit dem Stammzertifikat  

3. Exportieren Sie das Stammzertifikat.  
    Achten Sie darauf, dass das Zertifikat im Base64-codierten X.509-Format (.CER) exportiert wird: 

    ![Zertifikatexport-Assistent](../media/azure-netapp-files/certificate-export-wizard.png)

4. Wechseln Sie zum NetApp-Konto des Volumes mit dem dualen Protokoll, klicken Sie auf **Active Directory-Verbindungen**, und laden Sie das Zertifikat der Stammzertifizierungsstelle im Fenster **Active Directory beitreten** hoch:  

    ![Serverzertifikat von Stammzertifizierungsstelle](../media/azure-netapp-files/server-root-ca-certificate.png)

    Stellen Sie sicher, dass der Name der Zertifizierungsstelle durch DNS aufgelöst werden kann. Dieser Name ist das Feld „Ausgestellt von“ oder „Aussteller“ im Zertifikat:  

    ![Zertifikatinformationen](../media/azure-netapp-files/certificate-information.png)

## <a name="manage-ldap-posix-attributes"></a>Verwalten von LDAP-POSIX-Attributen

Sie können POSIX-Attribute wie z. B. UID, Basisverzeichnis und andere Werte über das MMC-Snap-In „Active Directory-Benutzer und -Computer“ verwalten.  Im folgenden Beispiel ist der Active Directory-Attribut-Editor dargestellt:  

![Active Directory-Attribut-Editor](../media/azure-netapp-files/active-directory-attribute-editor.png) 


## <a name="configure-the-nfs-client"></a>Konfigurieren des NFS-Clients 

Befolgen Sie die Anweisungen unter [Konfigurieren eines NFS-Clients für Azure NetApp Files](configure-nfs-clients.md), um den NFS-Client zu konfigurieren.  

## <a name="next-steps"></a>Nächste Schritte  

* [Häufig gestellte Fragen zum dualen Protokoll](azure-netapp-files-faqs.md#dual-protocol-faqs)
* [Konfigurieren eines NFS-Clients für Azure NetApp Files](configure-nfs-clients.md) 
