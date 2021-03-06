---
title: Exemples Azure Event Hubs | Microsoft Docs
description: Exemples Azure Event Hubs
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: sethm
ms.openlocfilehash: a581b7039a3631b7f1dc35816175242f892bd7dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
ms.locfileid: "29802785"
---
# <a name="event-hubs-samples"></a>Exemples de hubs d’événements 

Les exemples Azure Event Hubs présentent des fonctionnalités clés de [Azure Event Hubs](/azure/event-hubs/). Cette article attribue une catégorie et décrit les exemples disponibles, avec des liens vers chacun d’eux.

Au moment de la rédaction de cet article, les exemples de hubs d’événements se trouvent dans différents emplacements :

- [Exemples de code développeur MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)
- [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples)

Pour plus d’informations sur les différentes versions de .NET Framework, consultez [Infrastructures et cibles](/dotnet/articles/standard/frameworks).

D’autres exemples vont être ajoutés au fil du temps, alors consultez régulièrement cette page pour vérifier les mises à jour.

## <a name="net-standard"></a>.NET Standard

Les exemples suivants montrent comment envoyer et recevoir des événements à l’aide du [client Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet/blob/master/readme.md) pour la [bibliothèque .NET Standard](/dotnet/articles/standard/library).

### <a name="send-events"></a>Envoyer des événements 

L’exemple de [prise en main de l’envoi](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) montre comment écrire une application console .NET Core qui envoie des événements vers un Event Hub.

### <a name="receive-events"></a>Recevoir des événements 

L’exemple de [prise en main de la réception avec l’hôte du processeur d’événements](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) est une application console .NET Core qui reçoit des messages à partir d’un concentrateur d’événements à l’aide de l’hôte du processeur d’événements.

## <a name="net-framework"></a>.NET Framework   

Ces exemples présentent d’autres fonctionnalités d’Azure Event Hubs, ciblant la [bibliothèque .NET Framework](/dotnet/framework/index).
 
### <a name="notify-users-of-events-received"></a>Avertir les utilisateurs de la réception d’événements

L’exemple [AppToNotifyUsers](https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications) avertit les utilisateurs de données reçues à partir des capteurs ou d’autres systèmes.

### <a name="get-started-with-event-hubs"></a>Prise en main des hubs d’événements 

L’exemple de [prise en main des Event Hub](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097) montre les capacités de base des hubs d’événements, telles que la création d’un hub d’événements, l’envoi d’événements vers un Event Hub et la consommation d’événements à l’aide de [l’hôte du processeur d’événements](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).

### <a name="scale-out-event-processing"></a>Traitement d’événement mis à l’échelle 

L’exemple de [traitement d’événement mis à l’échelle](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) montre comment utiliser [l’hôte du processeur d’événements](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) pour distribuer la charge de travail de la consommation de flux de hubs d’événements. Il montre comment implémenter les objets **EventProcessor** et **EventProcessorFactory** pour gérer le flux d’événements. 

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les versions de .NET Framework, visitez les liens suivants :

- [Infrastructures et cibles](/dotnet/articles/standard/frameworks)
- [.NET Framework 4.6 et 4.5](/dotnet/framework/index)

Pour plus d’informations sur les hubs d’événements, consultez les articles suivants :

- [Vue d'ensemble d’Event Hubs](event-hubs-what-is-event-hubs.md)
- [Fonctionnalités Event Hubs](event-hubs-features.md)
- [FAQ sur les hubs d'événements](event-hubs-faq.md)