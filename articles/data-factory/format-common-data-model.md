---
title: Common Data Model-Format
description: Transformieren von Daten mit dem Common Data Model-Metadatensystem
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: daperlov
ms.openlocfilehash: 483e26cf4044b909c8d7923cfd74bd6fcf871e2a
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905284"
---
# <a name="common-data-model-format-in-azure-data-factory"></a>Common Data Model-Format in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Das Common Data Model-Metadatensystem ermöglicht es, dass Daten und deren Bedeutung problemlos von allen Anwendungen und Geschäftsprozessen gemeinsam genutzt werden können. Weitere Informationen finden Sie in der Übersicht zum [Common Data Model](https://docs.microsoft.com/common-data-model/).

In Azure Data Factory können Benutzer Daten aus CDM-Entitäten sowohl in MODEL.JSON-Dateien als auch in Manifestdateien transformieren, die in [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) (ADLS Gen2) mithilfe von Zuordnungsdatenflüssen gespeichert werden. Sie können Daten mithilfe von Verweisen auf CDM-Entitäten auch als Senken in das CDM-Format transformieren. Dann werden diese im CSV- oder PARQUET-Format in partitionierten Ordnern gespeichert. 

> [!NOTE]
> Der Common Data Model-Formatconnector für ADF-Datenflüsse ist zurzeit als öffentliche Vorschauversion verfügbar.

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Zuordnungsdatenflüssen

Das Common Data Model (CDM) ist als [Inlinedataset](data-flow-source.md#inline-datasets) in Zuordnungsdatenflüssen sowohl als Quelle als auch als Senke verfügbar.

> [!NOTE]
> Beim Schreiben von CDM-Entitäten muss bereits eine CDM-Entitätsdefinition (Metadatenschema) vorhanden sein, die zur Verwendung als Verweis definiert ist. Die ADF-Datenflusssenke liest diese CDM-Entitätsdatei und importiert das Schema in die Senke für die Feldzuordnung.

### <a name="source-properties"></a>Quelleigenschaften

In der folgenden Tabelle sind die von einer CDM-Quelle unterstützten Eigenschaften aufgeführt. Sie können diese Eigenschaften auf der Registerkarte **Quelloptionen** bearbeiten.

| Name | BESCHREIBUNG | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `cdm` sein. | ja | `cdm` | format |
| Metadatenformat | Gibt an, wo sich der Entitätsverweis auf die Daten befindet. Bei Verwendung von CDM-Version 1.0 wählen Sie „manifest“ aus. Bei Verwendung einer früheren CDM-Version als 1.0 wählen Sie „model.json“ aus. | Ja | `'manifest'` oder `'model'` | manifestType |
| Stammspeicherort: Container | Der Containername des CDM-Ordners. | ja | String | fileSystem |
| Stammspeicherort: Ordnerpfad | Der Stammspeicherort des CDM-Ordners. | ja | String | folderPath |
| Manifestdatei: Entitätspfad | Der Ordnerpfad der Entität innerhalb des Stammordners. | nein | String | entityPath |
| Manifestdatei: Manifestname | Der Name der Manifestdatei. Der Standardwert lautet „default“.  | Nein | String | manifestName |
| Nach der letzten Änderung filtern | Filtern Sie Dateien nach dem Zeitpunkt ihrer letzten Änderung. | nein | Timestamp | modifiedAfter <br> modifiedBefore | 
| Mit dem Schema verknüpfter Dienst | Der verknüpfte Dienst, in dem sich der Korpus befindet. | Ja, wenn Manifest verwendet wird | `'adlsgen2'` oder `'github'` | corpusStore | 
| Entitätsverweis-Container | Der Containerkorpus ist in | Ja, wenn Manifest und Korpus in ADLS Gen2 verwendet werden | String | adlsgen2_fileSystem |
| Entitätsverweis-Repository | Der Name des GitHub-Repositorys. | Ja, wenn Manifest und Korpus in GitHub verwendet werden | String | github_repository |
| Entitätsverweis-Verzweigung | Die Verzweigung des GitHub-Repositorys. | Ja, wenn Manifest und Korpus in GitHub verwendet werden | String |  github_branch |
| Korpusordner | Der Stammspeicherort des Korpus. | Ja, wenn Manifest verwendet wird | String | corpusPath |
| Korpusentität | Der Pfad zum Entitätsverweis. | ja | String | entity |
| Finden keiner Dateien zulässig | „true“ gibt an, dass kein Fehler ausgelöst wird, wenn keine Dateien gefunden werden. | nein | `true` oder `false` | ignoreNoFilesFound |

### <a name="sink-settings"></a>Senkeneinstellungen

* Zeigen Sie auf die Verweisdatei für die CDM-Entität, die die Definition der Entität enthält, die Sie schreiben möchten.

![Entitätseinstellungen](media/data-flow/common-data-model-111.png "Entitätsverweis")

* Definieren Sie den Partitionspfad und das Format der Ausgabedateien, die ADF zum Schreiben von Entitäten verwenden soll.

![Entitätsformat](media/data-flow/common-data-model-222.png "Entitätsformat")

* Legen Sie den Speicherort der Ausgabedatei sowie den Speicherort und den Namen der Manifestdatei fest.

![CDM-Speicherort](media/data-flow/common-data-model-333.png "CDM-Speicherort")


#### <a name="import-schema"></a>Importieren des Schemas

CDM ist nur als Inlinedataset verfügbar und weist standardmäßig kein zugeordnetes Schema auf. Zum Abrufen von Spaltenmetadaten klicken Sie auf der Registerkarte **Projektion** auf die Schaltfläche **Schema importieren**. Auf diese Weise können Sie auf die Spaltennamen und Datentypen verweisen, die durch den Korpus angegeben sind. Zum Importieren des Schemas muss eine [Datenfluss-Debugsitzung](concepts-data-flow-debug-mode.md) aktiv sein, und es muss eine CDM-Entitätsdefinitionsdatei vorhanden sein, auf die verwiesen werden kann.

Wenn Sie Entitätseigenschaften in der Senkentransformation Spalten für Zuordnungsdatenflüsse zuordnen möchten, klicken Sie erst auf die Registerkarte „Zuordnen“ und dann auf „Schema importieren“. ADF liest den Entitätsverweis, auf den Sie in den Senkenoptionen verwiesen haben, und ermöglicht es Ihnen, das CDM-Zielschema zuzuordnen.

![CDM-Senkeneinstellungen](media/data-flow/common-data-model-444.png "CDM-Zuordnung")

> [!NOTE]
>  Wenn Sie den Quelltyp „model.json“ verwenden, der aus Power BI- oder Power Platform-Datenflüssen stammt, können Fehler vom Typ „Korpuspfad ist NULL oder leer“ aus der Quelltransformation auftreten. Dies ist wahrscheinlich auf Formatierungsprobleme beim Speicherortpfad der Partition in der Datei „model.json“ zurückzuführen. Führen Sie zur Behebung die folgenden Schritte aus: 

1. Öffnen Sie die Datei „model.json“ in einem Text-Editor.
2. Suchen Sie nach der Eigenschaft „partitions.Location“. 
3. Ändern Sie „blob.core.windows.net“ in „dfs.core.windows.net“.
4. Korrigieren Sie eine „%2F“-Codierung in der URL in „/“.
 

### <a name="cdm-source-data-flow-script-example"></a>Beispiel eines Datenflussskripts für eine CDM-Quelle

```
source(output(
        ProductSizeId as integer,
        ProductColor as integer,
        CustomerId as string,
        Note as string,
        LastModifiedDate as timestamp
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    manifestType: 'manifest',
    manifestName: 'ProductManifest',
    entityPath: 'Product',
    corpusPath: 'Products',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    folderPath: 'ProductData',
    fileSystem: 'data') ~> CDMSource
```

### <a name="sink-properties"></a>Senkeneigenschaften

In der folgenden Tabelle sind die von einer CDM-Senke unterstützten Eigenschaften aufgeführt. Sie können diese Eigenschaften auf der Registerkarte **Einstellungen** bearbeiten.

| Name | BESCHREIBUNG | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `cdm` sein | ja | `cdm` | format |
| Stammspeicherort: Container | Der Containername des CDM-Ordners. | ja | String | fileSystem |
| Stammspeicherort: Ordnerpfad | Der Stammspeicherort des CDM-Ordners. | ja | String | folderPath |
| Manifestdatei: Entitätspfad | Der Ordnerpfad der Entität innerhalb des Stammordners. | nein | String | entityPath |
| Manifestdatei: Manifestname | Der Name der Manifestdatei. Der Standardwert lautet „default“. | Nein | String | manifestName |
| Mit dem Schema verknüpfter Dienst | Der verknüpfte Dienst, in dem sich der Korpus befindet. | ja | `'adlsgen2'` oder `'github'` | corpusStore | 
| Entitätsverweis-Container | Der Containerkorpus ist in | Ja, wenn der Korpus in ADLS Gen2 ist | String | adlsgen2_fileSystem |
| Entitätsverweis-Repository | Der Name des GitHub-Repositorys. | Ja, wenn der Korpus in GitHub ist | String | github_repository |
| Entitätsverweis-Verzweigung | Die Verzweigung des GitHub-Repositorys. | Ja, wenn der Korpus in GitHub ist | String |  github_branch |
| Korpusordner | Der Stammspeicherort des Korpus. | ja | String | corpusPath |
| Korpusentität | Der Pfad zum Entitätsverweis. | ja | String | Entität |
| Partitionspfad | Der Speicherort, an den die Partition geschrieben wird. | nein | String | partitionPath |
| Ordner löschen | Wenn der Zielordner vor dem Schreiben gelöscht wird. | nein | `true` oder `false` | truncate |
| Formattyp | Hiermit wird das Parquet-Format ausgewählt. | nein | `parquet`, wenn angegeben | subformat |
| Spaltentrennzeichen | Hiermit wird beim Schreiben in „DelimitedText“ das Trennen von Spalten angegeben. | Ja, wenn in „DelimitedText“ geschrieben wird | String | columnDelimiter |
| Erste Zeile als Kopfzeile | Hiermit wird bei Verwendung von „DelimitedText“ angegeben, ob die Spaltennamen als Kopfzeile hinzugefügt werden. | nein | `true` oder `false` | columnNamesAsHeader |

### <a name="cdm-sink-data-flow-script-example"></a>Beispiel eines Datenflussskripts für eine CDM-Senke

Das zugehörige Datenflussskript ist:

```
CDMSource sink(allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    entityPath: 'ProductSize',
    manifestName: 'ProductSizeManifest',
    corpusPath: 'Products',
    partitionPath: 'adf',
    folderPath: 'ProductSizeData',
    fileSystem: 'cdm',
    subformat: 'parquet',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    truncate: true,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> CDMSink

```

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Quelltransformation](data-flow-source.md) in einem Zuordnungsdatenfluss.
