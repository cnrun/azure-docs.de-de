---
title: REST-API zum Abfragen von Apache HBase in Azure HDInsight
description: ''
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/08/2020
ms.openlocfilehash: 3bf63aa08ec4c1deff2551cfcc0cf188a75261bc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82515972"
---
# <a name="rest-api-to-query-apache-hbase-in-azure-hdinsight"></a>REST-API zum Abfragen von Apache HBase in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Verwendung der Apache HBase-REST-Schnittstelle zum Abfragen einer Tabelle in einem anderen Namespace als dem Standard-Namespace führt zu einem Laufzeitfehler (HTTP-Status „500“).

## <a name="cause"></a>Ursache

Die HBase-REST-API wird nur unterstützt, wenn der Standard-Namespace verwendet wird. Dies ist ein bekanntes Problem im Hinblick auf die Verwendung von HBase-Namespaces oder die Durchführung von Aufrufen, die auf bestimmte GETS in Spalten mit Spaltenfamilien (REST-Server in HDInsight) verweisen. Der Grund: Ein Sicherheitsproblem bei HDInsight Gateway. Wenn Sie die-API zum Erstellen einer Tabelle mit einem Namespace verwenden, indem Sie über Spaltenfamilien auf Spalten zugreifen, müssen Sie das Zeichen `:` angeben. Dies wird als Sicherheitsproblem im IIS Gateway-Modul betrachtet.

## <a name="mitigation"></a>Minderung

Umgehen Sie das Gateway/den REST-Server, indem Sie Ihre Anwendung auf einer VM bereitstellen, die sich in demselben Azure-VNet wie der HDInsight-Cluster befindet. Anschließend können Sie mit HBase entweder direkt über RPC (vollständige Umgehung des REST-Servers) oder auf einzelnen REST-Servern kommunizieren, die auf Workerknoten ausgeführt werden (Umgehung von HDInsight-Gateways).

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
