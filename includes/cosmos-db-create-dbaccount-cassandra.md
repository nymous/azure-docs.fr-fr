---
title: Fichier Include
description: Fichier Include
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 3ad250ca2e641ebbf1e280e25aa53ff6f0ad6bd6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32198496"
---
1. Dans une nouvelle fenêtre du navigateur, connectez-vous au [portail Azure](https://portal.azure.com/).
2. Cliquez sur **Créer une ressource** > **Bases de données** > **Azure Cosmos DB**.
   
   ![Volet Bases de données du portail Azure](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. Dans la page **Nouveau compte**, entrez les paramètres pour le nouveau compte Azure Cosmos DB. 
 
    Paramètre|Valeur suggérée|Description
    ---|---|---
    ID|*Entrez un nom unique*|Entrez un nom unique pour identifier ce compte Azure Cosmos DB. Étant donné que *cassandra.cosmosdb.azure.com* est ajouté à l’ID que vous fournissez pour créer votre point de contact, utilisez un ID unique, mais identifiable.<br><br>L’ID ne peut contenir que des lettres minuscules, des chiffres et le caractère de trait d’union (-), et doit comporter entre 3 et 50 caractères.
    API|Cassandra|L’API détermine le type de compte à créer. Azure Cosmos DB fournit cinq API pour répondre aux besoins de votre application : SQL (base de données Document), Gremlin (base de données de graphiques), MongoDB (base de données Document), Table Azure et Cassandra, qui nécessitent toutes un compte séparé. <br><br>Sélectionnez **Cassandra**, car dans ce démarrage rapide, vous créez une base de données à colonne large qui peut être interrogée avec la syntaxe CQL.<br><br>Si Cassandra (colonne large) n’est pas affiché dans la liste, vous devez [demander à participer](../articles/cosmos-db/cassandra-introduction.md#sign-up-now) au programme d’évaluation de l’API Cassandra.<br><br> [En savoir plus sur l’API Cassandra](../articles/cosmos-db/cassandra-introduction.md)|
    Abonnement|*Votre abonnement*|Sélectionnez l’abonnement Azure que vous voulez utiliser pour ce compte Azure Cosmos DB. 
    Groupe de ressources|Création<br><br>*Entrez ensuite le même nom unique que celui fourni plus haut dans ID*|Sélectionnez **Créer**, puis entrez le nom du nouveau groupe de ressources pour votre compte. Pour plus de simplicité, vous pouvez utiliser le même nom que votre ID. 
    Lieu|*Sélectionner la région la plus proche de vos utilisateurs*|Sélectionnez l’emplacement géographique où héberger votre compte Azure Cosmos DB. Utilisez l’emplacement le plus proche de vos utilisateurs, pour leur donner l’accès le plus rapide possible aux données.
    Épingler au tableau de bord | Sélectionnez | Cochez cette case pour que votre nouveau compte de base de données soit ajouté à votre tableau de bord du portail pour un accès facilité.

    Cliquez ensuite sur **Créer**.

    ![Page de nouveau compte pour Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. La création du compte prend quelques minutes. Attendez que le portail affiche la page **Félicitations ! Votre compte Azure Cosmos DB a été créé**.

