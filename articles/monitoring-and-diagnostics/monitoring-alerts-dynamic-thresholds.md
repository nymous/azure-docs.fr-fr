---
title: Création d’alertes avec des seuils dynamiques dans Azure Monitor | Microsoft Docs
description: Créer des alertes avec des seuils dynamiques basés sur un apprentissage automatique
author: antonfrMSFT
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: antonfr;mbullwin
ms.openlocfilehash: 45d2f39ff7fb2e5a22cda74cd0c794db28e351f1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33943295"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-public-preview"></a>Alertes avec des seuils dynamiques dans Azure Monitor (préversion publique limitée)

Les alertes avec des seuils dynamiques sont une amélioration apportée aux Alertes Métrique dans Azure Monitor. Elles tirent parti des capacités avancées de Machine Learning pour apprendre le comportement historique des métriques afin de calculer automatiquement des points de référence utilisables en tant que les seuils d’alerte.

Les avantages de l’utilisation de seuils dynamiques sont les suivants :

- Ils vous épargnent les tracas associés à la définition d’une limite rigide prédéfinie lorsque le moniteur apprend automatiquement les performances historiques des métriques et applique des algorithmes de Machine Learning pour déterminer des seuils d’alerte.
- Ils peuvent identifier un comportement saisonnier et n’alerter qu’en cas d’écarts par rapport à un comportement saisonnier attendu. Les alertes Métrique avec des seuils dynamiques ne se déclenchent pas si votre service est régulièrement inactif durant les week-ends, et présente un pic chaque lundi. Actuellement prises en charge : saisonnalités horaire, quotidienne et hebdomadaire.
- Ils permettent d’apprendre les performances de métriques et de s’adapter aux modifications de métriques en continu.

Des alertes basées sur des seuils dynamiques sont disponibles pour toutes les sources de métriques basées sur Azure Monitor répertoriées dans cet [article](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>S’inscrire pour accéder à la préversion

Pour évaluer cette fonctionnalité, [inscrivez-vous à la préversion](http://aka.ms/DynamicThresholdMetricAlerts). Comme toujours, votre avis est important pour nous. Continuez à envoyer vos commentaires à [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com).

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Comment configurer des alertes avec des seuils dynamiques

Vous pouvez configurer des alertes avec des seuils dynamiques via Alertes dans Azure Monitor.

![Préversion d’Alertes](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Création d’une règle d’alerte avec des seuils dynamiques

1. Dans le volet Alertes sous Surveiller, sélectionnez le bouton **Nouvelle règle d’alerte** pour créer une alerte dans Azure.

   ![Nouvelle règle d’alerte](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. La section Créer une règle s’affiche avec ses trois parties : _Définir la condition de l’alerte_, _Définir les détails de l’alerte_ et _Définir un groupe d’actions_. Commencez par la section _Définir la condition de l’alerte_. Utilisez le lien **Sélectionner la cible** pour spécifier la cible en choisissant une ressource. Après avoir choisi une ressource appropriée, cliquez sur le bouton Terminé.

   ![Sélectionner la cible](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Utilisez ensuite le bouton **Ajouter d’autres critères** pour afficher la liste des options de signal disponibles pour la ressource, puis choisissez une option de **métrique** appropriée (par exemple Pourcentage UC).

   ![Ajouter des critères](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. Sur l’écran Configurer la logique du signal, dans la section Logique d’alerte, vous pouvez basculer la condition sur un type Dynamique qui génère automatiquement les Seuils dynamiques (lignes rouges) à côté de la métrique (ligne bleue).

   ![Dynamique](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. Les seuils figurant dans le graphique sont calculés sur la base des données historiques des sept derniers jours. Une fois qu’une alerte est créée, les seuils dynamiques acquièrent de nouvelles données historiques supplémentaires, qu’ils apprennent en continu afin d’améliorer leur précision.

6. Paramètres de logique d’alerte supplémentaires :
   - Condition : vous pouvez choisir l’alerte à déclencher dans l’une des trois conditions suivantes :
       - Supérieur au seuil supérieur ou inférieur au seuil inférieur (par défaut)
       - Supérieur au seuil supérieur
       - Inférieur au seuil inférieur
   - Agrégation de temps : moyenne (par défaut), somme, min max.
   - Critère de diffusion des alertes :
       - Haut : génère davantage d’alertes, celles-ci étant déclenchées sur le plus petit l’écart.
       - Moyen : moins sensible que le seuil Haut, génère moins d’alertes que celui-ci (par défaut)
       - Faible : seuil le moins sensible.

    ![Paramètres de la logique d’alerte](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Évaluées sur la base de :
    -  Durée pendant laquelle l’alerte doit rechercher la condition spécifiée par le choix de la **Période**.

    ![Évaluées sur la base de](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Valeurs de période prises en charge : 5 minutes, 10 minutes, 30 minutes et 1 heure.

   Pour réduire le bruit des alertes générée par des pics temporaires, nous vous recommandons d’utiliser les paramètres « Nombre de violations pour déclencher l’alerte ». Cette fonctionnalité vous permet d’obtenir une alerte uniquement si le seuil a été violé X fois de suite, ou Y fois au cours des Z dernières périodes. Par exemple : 

    Pour déclencher une alerte quand le problème perdure pendant 15 minutes, 3 fois de suite au cours d’une période de 5 minutes, utilisez les paramètres suivants :

   ![Évaluées sur la base de](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Pour déclencher une alerte en cas de violation d’un seuil dynamique pendant 15 minutes sur les 30 dernières minutes avec une période de 5 minutes, utilisez les paramètres suivants :

   ![Évaluées sur la base de](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Actuellement, les utilisateurs peuvent obtenir des alertes avec un critère de seuil dynamique en tant que critère unique.

   ![Créer une règle](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>Questions et réponses

- Q : une métrique qui change lentement au fil du temps déclenchera-t-elle une alerte avec des seuils dynamiques ?

- R : Probablement pas. Les seuils dynamiques sont appropriés pour détecter des écarts significatifs plutôt que des problèmes se manifestant lentement.

- Q : Puis-je configurer des seuils dynamiques via une API ?

- R : Nous y travaillons.
