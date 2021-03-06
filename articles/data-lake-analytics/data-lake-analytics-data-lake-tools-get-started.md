---
title: Prise en main d’Azure Data Lake Analytics à l’aide de Visual Studio | Microsoft Docs
description: Découvrez comment installer Data Lake Tools pour Visual Studio et développer et tester des scripts U-SQL.
services: data-lake-analytics
documentationcenter: ''
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/02/2018
ms.author: saveenr, yanacai
ms.openlocfilehash: d0974e3258e0def09fe12d348180dcedf216401c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Développer des scripts U-SQL à l’aide des outils Data Lake pour Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Découvrez comment utiliser Visual Studio pour créer des comptes Azure Data Lake Analytics, définir des travaux dans [U-SQL](data-lake-analytics-u-sql-get-started.md) et envoyer des travaux au service Data Lake Analytics. Pour plus d’informations sur Analytique Data Lake, consultez [Présentation d’Analytique Data Lake Azure](data-lake-analytics-overview.md).

>[!IMPORTANT]
>
>En prévision du nouveau Règlement général sur la protection des données (RGPD) qui entrera en vigueur le 25 mai 2018, il est recommandé que les utilisateurs d’Azure Data Lake Tools pour Visual Studio passent à la version 2.3.3000.4 ou une version ultérieure. Cette version inclut des modifications basées sur des exigences de protection des données plus récentes. Notez que les versions précédentes ne sont plus disponibles au téléchargement et sont dépréciées. 
>
>**Que dois-je faire ?**
>
>1. Veillez à utiliser une version d’Azure Data Lake Tools pour Visual Studio antérieure à 2.3.3000.4. 
>   
>   ![Vérifier la version de l’outil](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
> 
>2. Si votre version est antérieure à 2.3.3000.4, mettez à jour Azure Data Lake Tools pour Visual Studio par le biais du centre de téléchargement : 
>    - [Pour Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [Pour Visual Studio 2013 et 2015](https://www.microsoft.com/en-us/download/details.aspx?id=49504)


## <a name="prerequisites"></a>Prérequis


* **Visual Studio** : toutes les éditions sauf Express sont prises en charge.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Kit SDK Microsoft Azure pour .NET** version 2.7.1 ou ultérieure.  Installez-le à l’aide de [Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx).
* Un compte**Data Lake Analytics**. Pour créer un compte, consultez [Prise en main d’Azure Data Lake Analytics à l’aide du portail Azure](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Installer Azure Data Lake Tools pour Visual Studio

### <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Installer Azure Data Lake Tools pour Visual Studio 2017

Azure Data Lake Tools pour Visual Studio est pris en charge dans Visual Studio 2017 15.3 ou version supérieure. L’outil fait partie des charges de travail **Développement Azure** et **Stockage et traitement des données** dans Visual Studio Installer. Activez l’une de ces deux charges de travail dans le cadre de votre installation de Visual Studio.  

Activez la charge de travail **Stockage et traitement des données** comme indiqué : ![Activer la charge de travail Stockage et traitement des données](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Activez la charge de travail **Développement Azure** comme indiqué : ![Activer la charge de travail Développement Azure](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

### <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Installer Azure Data Lake Tools pour Visual Studio 2013 et 2015

Téléchargez et installez Azure Data Lake Tools pour Visual Studio [à partir du centre de téléchargement](http://aka.ms/adltoolsvs). Après l’installation, notez les points suivants :
* Le nœud **Server Explorer** > **Azure** contient un nœud **Data Lake Analytics**. 
* Le menu **Outils** a un élément **Data Lake**.

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Se connecter à un compte Azure Data Lake Analytics

1. Ouvrez Visual Studio.
2. Ouvrez l’Explorateur de serveurs en sélectionnant **Afficher** > **Explorateur de serveurs**.
3. Cliquez avec le bouton droit sur **Azure**. Puis, sélectionnez **Se connecter à un abonnement Microsoft Azure**, puis suivez les instructions.
4. Dans l’Explorateur de serveurs, sélectionnez **Azure** > **Data Lake Analytics**. Une liste des comptes Data Lake Analytics s’affiche.


## <a name="write-your-first-u-sql-script"></a>Rédiger son premier script U-SQL

Le texte suivant est un script U-SQL simple. Il définit un petit jeu de données et l’écrit dans le Data Lake Store par défaut sous la forme d’un fichier appelé `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Envoyer le travail Analytique Data Lake

1. Sélectionnez **Fichier** > **Nouveau** > **Projet**.

2. Sélectionnez le type **Projet U-SQL**, puis cliquez sur **OK**. Visual Studio crée une solution avec un fichier **Script.usql**.

3. Collez le script précédent dans la fenêtre **Script.usql**.

4. Dans le coin supérieur gauche de la fenêtre **Script.usql**, spécifiez le compte Data Lake Analytics.

    ![Soumettre un projet U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. Dans le coin supérieur gauche de la fenêtre **Script.usql**, sélectionnez **Envoyer**.
6. Vérifiez le **compte Analytics**, puis sélectionnez **Envoyer**. Les résultats de l’envoi sont disponibles dans la fenêtre Résultats de Data Lake Tools pour Visual Studio à l’issue de l’envoi.

    ![Soumettre un projet U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. Cliquez sur le bouton **Actualiser** pour afficher le dernier état du travail et actualiser l’écran. Si le travail réussit, celui-ci affiche le **Graphique des travaux**, les **Opérations sur les métadonnées**, l’**Historique de l’état** et les **Diagnostics** :

    ![Graphique des performances du travail U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Résumé du travail** montre le résumé du travail.   
   * **Détails du travail** montre des informations plus spécifiques sur le travail, y compris le script, les ressources et les vertex.
   * **Graphique des travaux** permet de visualiser la progression du travail.
   * **Opérations sur les métadonnées** indique toutes les actions qui ont été effectuées sur le catalogue U-SQL.
   * **Données** indique toutes les entrées et sorties.
   * **Diagnostics** fournit une analyse avancée pour l’optimisation des performances et de l’exécution des travaux.

### <a name="to-check-job-state"></a>Pour vérifier l’état des travaux

1. Dans l’Explorateur de serveurs, sélectionnez **Azure** > **Data Lake Analytics**. 
2. Développez le nom du compte Data Lake Analytics.
3. Double-cliquez sur **Travaux**.
4. Sélectionnez le travail que vous avez déjà envoyé.

### <a name="to-see-the-output-of-a-job"></a>Pour voir le résultat d’un travail

1. Dans l’Explorateur de serveurs, accédez au travail que vous avez envoyé.
2. Cliquez sur l'onglet **Données** .
3. Dans l’onglet **Sorties du travail**, sélectionnez le fichier `"/data.csv"`.

## <a name="next-steps"></a>Étapes suivantes

* [Run U-SQL scripts on your own workstation for testing and debugging (Exécuter des scripts U-SQL sur votre station de travail pour les tests et le débogage)](data-lake-analytics-data-lake-tools-local-run.md)
* [Débogage du code C# dans les travaux U-SQL à l’aide d’Azure Data Lake Tools pour Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Utilisation d’Azure Data Lake Tools pour Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
