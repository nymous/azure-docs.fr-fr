---
title: Options IoT Microsoft Azure | Microsoft Docs
description: Choisissez la mise en œuvre de votre solution IoT utilisant les accélérateurs de solution Azure IoT, Azure IoT Central ou Azure IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 56a19e9cbeb6e7d30171d23ff918a34ba423d2f2
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725579"
---
# <a name="compare-azure-iot-options"></a>Comparer les options d’Azure IoT

L’article [Azure et l’Internet des objets](iot-accelerators-what-is-azure-iot.md) décrit une architecture de solution IoT standard avec les couches suivantes :

* Connectivité et gestion des appareils
* Traitement et analyse des données
* Présentation et intégration d’entreprise

Pour implémenter cette architecture, Azure IoT offre plusieurs options, adaptées à différents ensembles d’exigences du client :

* Les [accélérateurs de solution Azure IoT](../active-directory-domain-services/index.yml) représentent une collection de niveau entreprise d’[accélérateurs de solution](iot-accelerators-what-are-solution-accelerators.md) basés sur la plateforme en tant que service (PaaS) Azure et qui vous permettent d’accélérer le développement de solutions IoT personnalisées.

* [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions) est une solution SaaS (logiciel en tant que service) qui utilise une approche basée sur les modèles pour vous permettre de créer des solutions IoT de niveau entreprise sans nécessiter de compétences en développement de solutions cloud.

## <a name="azure-iot-hub"></a>Azure IoT Hub

Azure IoT Hub est la plateforme en tant que service (PaaS) de base utilisée par Microsoft IoT Central et les accélérateurs de solution Azure IoT. IoT Hub permet des communications bidirectionnelles fiables et sécurisées entre des millions d’appareils IoT et une solution cloud. IoT Hub vous aide à répondre aux défis de mise en œuvre de l’IoT telles que :

* Connectivité et gestion de haut volume d’appareils.
* Ingestion de télémétrie de haut volume.
* Commande et contrôle des appareils.
* Application de la sécurité des appareils.

## <a name="compare-azure-iot-solution-accelerators-and-azure-iot-central"></a>Comparaison entre les accélérateurs de solution Azure IoT et Azure IoT Central

Le choix de votre produit Azure IoT est une étape critique de la planification de votre solution IoT. IoT Hub est un service Azure individuel qui seul n’offre pas une solution IoT de bout en bout. IoT Hub peut être utilisé comme point de départ pour une solution IoT, et vous n’avez pas besoin d’utiliser les accélérateurs de solution Azure IoT ou Azure IoT Central pour l’utiliser. Les accélérateurs de solution Azure IoT et Azure IoT Central, ainsi que d’autres services Azure utilisent IoT Hub. Le tableau suivant résume les principales différences entre les accélérateurs de solution Azure IoT et Azure IoT Central pour vous aider à choisir ce qui convient le mieux à vos besoins :

|                        | Accélérateurs de solution Azure IoT | Azure IoT Central |
| ---------------------- | --------- | ----------- |
| Utilisation principale | Accélérer le développement d’une solution IoT personnalisée nécessitant une flexibilité maximale. | Accélérer la mise sur le marché de solutions IoT simples qui ne nécessitent pas une personnalisation de service complète. |
| Accès aux services PaaS sous-jacents          | Vous avez accès aux services Azure sous-jacents en vue de les gérer ou de les remplacer en fonction des besoins. | SaaS. Solution entièrement gérée, les services sous-jacents ne sont pas exposés. |
| Flexibilité            | Élevée. Le code des microservices est open source, et vous pouvez le modifier comme vous le souhaitez. En outre, vous pouvez personnaliser l’infrastructure de déploiement.| Moyenne. Vous pouvez utiliser l’expérience utilisateur intégrée basée sur un navigateur pour personnaliser le modèle de la solution et les aspects de l’interface utilisateur. L’infrastructure n’est pas personnalisable, car les différents composants ne sont pas exposés.|
| Niveau de compétence                 | Moyenne-élevée. Des compétences en Java ou en .NET sont nécessaires pour personnaliser le back end de la solution. Des compétences en JavaScript sont nécessaires pour personnaliser la visualisation. | Faible. Des compétences en modélisation sont nécessaires pour personnaliser la solution. Aucune compétence en codage n’est requise. |
| Expérience de démarrage | Les accélérateurs de solution mettent en œuvre des scénarios IoT courants. Déploiement possible en quelques minutes. | Les modèles d’application et les modèles de périphériques fournissent des modèles prédéfinis. Déploiement possible en quelques minutes. |
| Tarifs                | Vous pouvez affiner les services pour contrôler le coût. | Structure de tarification simple et prévisible. |

Le choix du produit à utiliser pour créer votre solution IoT est finalement déterminé par :

* Vos exigences métier.
* Le type de solution que vous souhaitez créer
* Les compétences de votre organisation pour créer et maintenir la solution à long terme.

## <a name="next-steps"></a>Étapes suivantes

Les étapes suggérées en fonction du produit et de l’approche choisis, sont :

* **Accélérateurs de solution Azure IoT** : [que sont les accélérateurs de solution Azure IoT ?](iot-accelerators-what-are-solution-accelerators.md)
* **Azure IoT Central** : [Azure IoT Central](https://www.microsoft.com/internet-of-things/iot-central-saas-solutions).
* **IoT Hub** : [Présentation du service Azure IoT Hub](../iot-hub/iot-hub-what-is-iot-hub.md).
