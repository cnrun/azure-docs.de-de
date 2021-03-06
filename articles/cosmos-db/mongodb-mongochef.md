---
title: Verwenden von Studio 3T zum Herstellen einer Verbindung mit der API für MongoDB von Azure Cosmos DB
description: Erfahren Sie, wie Sie eine Verbindung mit einer Azure Cosmos DB-API für MongoDB mithilfe von Studio 3T herstellen.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 18a9f97d2cee9dd17345a1c8c0ae0efe442d79a4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85799393"
---
# <a name="connect-to-an-azure-cosmos-account-using-studio-3t"></a>Herstellen einer Verbindung mit einem Azure Cosmos-Konto mithilfe von Studio 3T

Um eine Verbindung mit einer Azure Cosmos DB-API für MongoDB mithilfe von Studio 3T herzustellen, müssen Sie:

* [Studio 3T](https://studio3t.com/) herunterladen und installieren.
* Rufen Sie die Informationen zur [Verbindungszeichenfolge](connect-mongodb-account.md) für Ihr Azure Cosmos-Konto ab.

## <a name="create-the-connection-in-studio-3t"></a>Sie müssen die Verbindung in Studio 3T herstellen

Führen Sie die folgenden Schritte aus, um dem Studio 3T-Verbindungs-Manager Ihrem Azure Cosmos-Konto hinzuzufügen:

1. Rufen Sie die Verbindungsinformationen Ihrer Azure Cosmos DB-API für das MongoDB-Konto ab, indem Sie den Anweisungen im Artikel [Verbinden einer MongoDB-Anwendung mit Azure Cosmos DB](connect-mongodb-account.md) folgen.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionStringBlade.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::

2. Klicken Sie auf **Connect** (Verbinden), um den Verbindungs-Manager zu öffnen, und klicken Sie anschließend auf **New Connection** (Neue Verbindung).

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManager.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
3. Geben Sie im Fenster **Neue Verbindung** auf der Registerkarte **Server** den HOST (FQDN) des Azure Cosmos-Kontos sowie den PORT ein.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerServerTab.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
4. Wählen Sie im Fenster **New Connection** (Neue Verbindung) auf der Registerkarte **Authentication** (Authentifizierung) den Authentifizierungsmodus **Standard (MONGODB-CR or SCARM-SHA-1)** (Standard [MONGODB-CR oder SCARM-SHA-1]) aus, und geben Sie Benutzername und Kennwort ein.  Übernehmen Sie den Standardwert für Authentifizierungs-DB („admin“), oder geben Sie einen eigenen Wert ein.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
5. Aktivieren Sie im Fenster **New Connection** (Neue Verbindung) auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol to connect** (Verbindung über SSL-Protokoll herstellen) und das Optionsfeld **Accept server self-signed SSL certificates** (Selbstsignierte SSL-Serverzertifikate akzeptieren).

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerSSLTab.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
6. Klicken Sie auf die Schaltfläche **Test Connection** (Verbindung testen), um die Verbindungsinformationen zu überprüfen. Klicken Sie auf **OK**, um zum Fenster für neue Verbindungen zurückzukehren, und klicken Sie anschließend auf **Save** (Speichern).

    :::image type="content" source="./media/mongodb-mongochef/TestConnectionResults.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Verwenden von Studio 3T zum Erstellen von Datenbanken, Sammlungen und Dokumenten
Führen Sie die folgenden Schritte aus, um Datenbanken, Sammlungen und Dokumente mithilfe von Studio 3T zu erstellen:

1. Markieren Sie die Verbindung im **Verbindungs-Manager**, und klicken Sie auf **Connect** (Verbinden).

    :::image type="content" source="./media/mongodb-mongochef/ConnectToAccount.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
2. Klicken Sie mit der rechten Maustaste auf den Host, und wählen Sie **Add Database**(Datenbank hinzufügen) aus.  Geben Sie einen Datenbanknamen an, und klicken Sie auf **OK**.

    :::image type="content" source="./media/mongodb-mongochef/AddDatabase1.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
3. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Add Collection**(Sammlung hinzufügen) aus.  Geben Sie einen Sammlungsnamen an, und klicken Sie auf **Create**(Erstellen).

    :::image type="content" source="./media/mongodb-mongochef/AddCollection.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::
4. Klicken Sie auf das Menüelement **Collection** (Sammlung) und anschließend auf **Add Document** (Dokument hinzufügen).

    :::image type="content" source="./media/mongodb-mongochef/AddDocument1.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“" },
        "isRegistered": false
    }
    ```

7. Führen Sie eine Beispielabfrage aus. Suchen Sie z.B. nach Familien mit dem Nachnamen „Andersen“, und geben Sie übergeordnete Felder und Felder mit Angaben zum Staat zurück.

    :::image type="content" source="./media/mongodb-mongochef/QueryDocument1.png" alt-text="Screenshot der Seite „Verbindungszeichenfolge“":::

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Robo 3T](mongodb-robomongo.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Untersuchen Sie MongoDB-[Beispiele](mongodb-samples.md) mit der API für MongoDB von Azure Cosmos DB.
