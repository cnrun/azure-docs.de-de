---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d03579f704879bd8d012bb0bb326659d1f778dee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84793291"
---
[Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) in der REST-API gibt den Integritätsstatus der Endpunkte sowie den letzten bekannten Fehler an, um den Grund zu nennen, warum ein Endpunkt nicht fehlerfrei ist. In der folgenden Tabelle werden die häufigsten Fehler aufgeführt.

|Letzter bekannter Fehler|Beschreibung/Auftreten|Mögliche Entschärfung|
|-----|-----|-----|
|Kurzlebig|Ein vorübergehender Fehler ist aufgetreten, und IoT Hub wiederholt den Vorgang.|Untersuchen Sie [Diagnoseprotokolle für Routen](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes).|
|InternalError|Fehler bei der Übermittlung einer Nachricht an einen Endpunkt.|Dies ist eine interne Ausnahme, untersuchen Sie aber auch die [Diagnoseprotokolle](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health#routes) für Routen.|
|Nicht autorisiert|IoT Hub ist nicht zum Senden von Nachrichten an den angegebenen Endpunkt autorisiert.|Überprüfen Sie, ob die Verbindungszeichenfolge für den Endpunkt auf dem neuesten Stand ist. Wenn sie sich geändert hat, sollten Sie ein Update für Ihre IoT Hub in Erwägung ziehen. Wenn der Endpunkt eine verwaltete Identität verwendet, überprüfen Sie, ob der IoT Hub-Prinzipal über die erforderlichen Berechtigungen für das Ziel verfügt.|
|Gedrosselt|IoT Hub wird beim Schreiben von Nachrichten in den Endpunkt gedrosselt.|Überprüfen Sie die Drosselungslimits für den betroffenen Endpunkt. Ändern Sie die Konfigurationen, damit der Endpunkt bei Bedarf hochskaliert wird.|
|Timeout|Timeout bei Vorgang.|Wiederholen Sie den Vorgang.|
|Nicht gefunden|Die Zielressource ist nicht vorhanden.|Stellen Sie sicher, dass die Zielressource vorhanden ist.|
|Container nicht gefunden|Der Speichercontainer ist nicht vorhanden.|Stellen Sie sicher, dass der Speichercontainer vorhanden ist.|
|Container deaktiviert|Der Speichercontainer ist deaktiviert.|Stellen Sie sicher, dass der Speichercontainer aktiviert ist.|
|MaxMessageSizeExceeded|Das Nachrichtenrouting weist eine Nachrichtengrößenbeschränkung von 256 KB auf. Die Nachrichtengröße, die weitergeleitet wird, hat diesen Grenzwert überschritten.|Überprüfen Sie, ob die Größe der Nachricht verringert werden kann, indem Sie weniger Anwendungseigenschaften oder weniger Nachrichtenergänzungen verwenden.|
|PartitioningAndDuplicateDetectionNotSupported|Für Service Bus ist Duplikaterkennung möglicherweise nicht aktiviert.|Deaktivieren Sie Duplikaterkennung in Service Bus, oder verwenden Sie eine Entität ohne Duplikaterkennung.|
|SessionfulEntityNotSupported|Für Service Bus sind Sitzungen möglicherweise nicht aktiviert.|Deaktivieren Sie die Sitzung in Service Bus, oder verwenden Sie eine Entität ohne Sitzungen.|
|NoMatchingSubscriptionsForMessage|Es ist kein Abonnement zum Schreiben von Nachrichten für das Service Bus-Thema vorhanden.|Erstellen Sie ein Abonnement für IoT Hub- Nachrichten, an die die Weiterleitung erfolgen soll.|
|EndpointExternallyDisabled|Der Endpunkt befindet sich nicht in einem aktiven Zustand, IoT Hub kann daher keine Nachrichten an ihn senden.|Aktivieren Sie den Endpunkt, um ihn erneut in den aktiven Zustand zu versetzen.|
|DeviceMaximumQueueDepthExceeded|Die maximale Service Bus-Größe wurde erreicht.|Entfernen Sie Nachrichten aus den Ziel-Event Hubs, damit neue Nachrichten in den Event Hubs erfasst werden können.|