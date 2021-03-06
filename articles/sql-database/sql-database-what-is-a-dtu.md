---
title: 'Base de données SQL : Qu’est-ce qu’une DTU ? | Microsoft Docs'
description: Compréhension du rôle d’une unité de transaction de base de données SQL.
keywords: options de base de données,performances de base de données
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 50b12de2400f0db90108e3eeb158357039ac92ec
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649820"
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Unités de transaction de base de données (DTU) et des unités de transaction de base de données élastique (eDTU)
Cet article explique les unités de transaction de base de données (DTU) et les unités de transaction de base de données élastique (eDTU) et ce qu’il se passe lorsque vous avez atteint le nombre maximal de DTU et d’eDTU. Pour plus d’informations sur la tarification, consultez [Tarifs Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="what-are-database-transaction-units-dtus"></a>Définition des unités de transaction de base de données (DTU)
Microsoft garantit un certain niveau de ressources pour une seule base de données Azure SQL Database à un niveau de performances spécifique au sein d’un [niveau de service](sql-database-single-database-resources.md) (indépendamment de toute autre base de données dans le cloud Azure), fournissant un niveau de performances prévisible. Le volume de ressources est calculé sous forme d’unités de transactions de base de données ou DTU. Il s’agit d’une mesure groupée de calcul, de stockage et de ressources d’E/S. Le ratio entre ces ressources a été déterminé à l’origine par une [charge de travail d’évaluation OLTP](sql-database-benchmark-overview.md), conforme aux charges de travail OLTP réelles standard. Quand votre charge de travail dépasse la quantité de ces ressources, le débit est limité, ce qui entraîne un ralentissement des performances et des délais d’attente. Les ressources utilisées par votre charge de travail n’impactent pas les ressources disponibles pour les autres bases de données SQL dans le cloud Azure, et les ressources utilisées par d’autres charges de travail n’impactent pas les ressources disponibles pour votre base de données SQL.

![cadre englobant](./media/sql-database-what-is-a-dtu/bounding-box.png)

Les DTU sont particulièrement utiles pour comprendre la quantité relative de ressources entre les bases de données Azure SQL Database à différents niveaux de performances et de service. Par exemple, le fait de doubler les DTU en augmentant le niveau de performances d’une base de données revient à doubler l’ensemble des ressources disponibles pour cette base de données. Par exemple, une base de données Premium P11 comprenant 1 750 DTU fournit une puissance de calcul DTU 350 fois plus importante qu’une base de données de base comprenant 5 DTU.  

Pour avoir une idée plus précise de la consommation de ressources (DTU) de votre charge de travail, utilisez [Query Performance Insight pour Azure SQL Database ](sql-database-query-performance.md) pour :

- Identifier les principales requêtes par UC/durée/nombre d’exécutions qui peuvent être réglées pour améliorer les performances. Par exemple, une requête utilisant beaucoup d’E/S peut tirer profit de l’utilisation de [techniques d’optimisation en mémoire](sql-database-in-memory.md) pour mieux utiliser la mémoire disponible à un certain niveau de service et de performances.
- Connaître les détails d’une requête, afficher son texte et l’historique d’utilisation des ressources.
- Accéder aux recommandations de réglage des performances qui indiquent les actions effectuées par [SQL Database Advisor](sql-database-advisor.md).

Vous pouvez changer les [niveaux de service des DTU](sql-database-service-tiers-dtu.md) à tout moment avec un temps d’arrêt minimal de votre application (généralement sous les quatre secondes environ). Pour de nombreuses entreprises et applications, la possibilité de créer des bases de données et d’augmenter ou ralentir les performances à la demande se révèle suffisante, surtout si les modèles d’utilisation sont relativement prévisibles. Mais si vous avez des modèles d'utilisation imprévisibles, il peut être difficile de gérer les coûts et votre modèle commercial. Pour ce scénario, vous utilisez un pool élastique avec un certain nombre d’eDTU qui sont partagées entre plusieurs bases de données dans le pool.

![Introduction à la base de données SQL : DTU de base de données unique par couche et niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Définition des unités de transaction de base de données élastique (DTU)
Plutôt que de fournir un ensemble de ressources (DTU) dédié qui peut ne pas s’avérer toujours nécessaire pour une base de données SQL Database qui est toujours disponible, vous pouvez placer les bases de données dans un [pool élastique](sql-database-elastic-pool.md) sur un serveur SQL Database qui partage un pool de ressources entre ces bases de données. Les ressources partagées dans un pool élastique sont mesurées en eDTU (elastic Database Transaction Unit). Les pools élastiques offrent une solution simple et économique pour gérer les objectifs de performance de plusieurs bases de données ayant des modèles d’utilisation variables et non prévisibles. Un pool élastique garantit que les ressources ne peuvent pas être consommées par une base de données dans le pool, mais également que chaque base de données dans le pool dispose toujours d’une quantité de ressources minimale. 

![Introduction à la base de données SQL : eDTU par couche et niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Un pool bénéficie d’un nombre défini d’eDTU pour un prix donné. Au sein du pool élastique, les différentes bases de données peuvent s’adapter facilement et automatiquement aux limites définies. Une base de données soumise à une charge plus élevée consomme plus d’eDTU pour répondre à la demande. Les bases de données soumises à des charges plus légères consomment moins d’eDTU. Les bases de données qui ne subissent aucune charge ne consomment aucune eDTU. Le provisionnement des ressources pour l’ensemble du pool, plutôt que pour chaque base de données, simplifie les tâches de gestion, fournissant un aperçu du budget nécessaire pour le pool.

Vous pouvez ajouter des eDTU à un pool existant sans que les bases de données du pool ne connaissent de temps d’arrêt et ne soient affectées. De même, si les eDTU supplémentaires ne sont plus nécessaires, elles peuvent être supprimées à partir d’un pool existant à tout moment. Vous pouvez ajouter des bases de données dans le pool ou en supprimer. Vous pouvez également limiter la quantité d’eDTU qu’une base de données peut utiliser en cas de charge importante pour réserver des eDTU pour les autres bases de données. Si vous prévoyez qu’une base de données sous-utilise des ressources, vous pouvez la déplacer hors du pool et la configurer en tant que base de données unique avec une quantité prévisible des ressources nécessaires.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Comment puis-je déterminer le nombre de DTU requises par ma charge de travail ?
Si vous cherchez à migrer une charge de travail de machine virtuelle SQL Server ou locale existante vers une base de données SQL Azure, vous pouvez utiliser l’outil [DTU Calculator](http://dtucalculator.azurewebsites.net/) pour évaluer approximativement le nombre de DTU requises. Dans le cas d’une charge de travail de base de données SQL Azure existante, vous pouvez utiliser [Query Performance Insight pour base de données SQL](sql-database-query-performance.md) pour comprendre votre consommation des ressources de la base de données (DTU) et obtenir un insight plus approfondi pour l’optimisation de votre charge de travail. Vous pouvez également utiliser la DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) pour voir la consommation des ressources au cours de la dernière heure. Sinon, la vue de catalogue [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) affiche la consommation des ressources pour les 14 derniers jours, mais avec une précision inférieure (moyennes de cinq minutes).

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Comment savoir si je peux tirer parti d’un pool élastique de ressources ?
Les pools sont idéaux dans le cas de nombreuses bases de données avec des modèles d’utilisation spécifiques. Pour une base de données indiquée, ce modèle se caractérise par une moyenne d’utilisation faible avec des pics d’utilisation relativement rares. La base de données SQL évalue automatiquement l’historique d’utilisation en ressources des bases de données dans un serveur de base de données SQL existant et recommande la configuration de pool appropriée dans le portail Azure. Pour plus d’informations, consultez l’article [Quand utiliser un pool élastique ?](sql-database-elastic-pool.md).

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Que se passe-t-il lorsque j’atteins le nombre maximal de DTU ?
Les niveaux de performances sont étalonnés et régis pour fournir les ressources nécessaires à l’exécution de la charge de travail de votre base de données dans les limites maximales autorisées pour le niveau de service/niveau de performances sélectionné. Si votre charge de travail atteint une des limites UC, E/S de données ou E/S de journal, vous bénéficiez toujours du niveau maximal de ressources autorisé, mais les latences de requête risquent de s’accroître. Ces limites ne génèrent pas d’erreur, plutôt un ralentissement de la charge de travail, sauf si le ralentissement s’accentue au point que les requêtes arrivent à expiration. Si vous atteignez le nombre maximal autorisé de sessions/demandes utilisateur simultanées (threads de travail), vous voyez des erreurs explicites. Pour plus d’informations sur les limites de ressource qui ne concernent pas l’unité centrale, la mémoire, les E/S de données ou les E/S du journal des transactions, consultez [Limites de ressources d’Azure SQL Database]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached).

## <a name="next-steps"></a>Étapes suivantes
* Consultez [Modèle d’achat basé sur les DTU](sql-database-service-tiers-dtu.md) pour plus d’informations sur les DTU et eDTU disponibles pour les bases de données uniques et les pools élastiques, ainsi que pour connaître les limites sur les ressources autres que celles d’unité centrale, de mémoire, d’E/S de données et d’E/S de journaux de transactions.
* Consultez [Modèle d’achat vCore (préversion)](sql-database-service-tiers-vcore.md) pour plus d’informations sur les niveaux de service et d’allocation de ressources vCore. 
* Pour comprendre votre consommation de DTU, consultez [Query Performance Insight pour base de données SQL](sql-database-query-performance.md) .
* Pour comprendre la méthodologie sous-jacente à la charge de travail d’évaluation OLTP utilisée pour déterminer la fusion DTU, consultez [Vue d’ensemble du test d’évaluation de la base de données SQL](sql-database-benchmark-overview.md) .
