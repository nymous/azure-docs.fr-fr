---
title: Envoyer des notifications vers des applications de plateforme Windows universelle à l’aide d’Azure Notification Hubs | Microsoft Docs
description: Dans ce didacticiel, vous découvrirez comment utiliser Azure Notification Hubs pour envoyer des notifications Push à une application de plateforme Windows universelle.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: c3bb170800508d5a546573850f445b2a8991ea8c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777060"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-by-using-azure-notification-hubs"></a>Didacticiel : envoyer des notifications vers des applications de plateforme Windows universelle à l’aide d’Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Dans ce didacticiel, vous créez un hub de notification pour envoyer des notifications Push à une application de plateforme Windows universelle (UWP). Vous créez une application Windows Store vide qui reçoit des notifications Push au moyen du Service de notifications Windows Push (WNS). Vous pouvez ensuite utiliser votre hub de notification pour diffuser des notifications Push sur tous les appareils exécutant votre application.

> [!NOTE]
> Vous pouvez trouver le code complet de ce didacticiel sur [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

Dans ce didacticiel, vous effectuez les étapes suivantes :

> [!div class="checklist"]
> * Créer une application dans Windows Store
> * Créer un hub de notification
> * Créer un exemple d’application Windows
> * Envoyer des notifications de test


## <a name="prerequisites"></a>Prérequis

- **Abonnement Azure**. Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.
- [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) ou version ultérieure.
- [Outils de développement d’applications UWP installés](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Un compte Windows Store actif

Vous devez terminer ce didacticiel avant de pouvoir suivre tous les autres didacticiels Notification Hubs pour les applications UWP.

## <a name="create-an-app-in-windows-store"></a>Créer une application dans Windows Store
Pour envoyer des notifications Push à des applications UWP, associez votre application au Windows Store. Ensuite, configurez votre Notification Hub pour l’intégrer à WNS.

1. Accédez à la page [Centre de développement Windows](https://dev.windows.com/overview), connectez-vous avec votre compte Microsoft, puis sélectionnez **Créer une application**.

    ![Bouton Nouvelle application](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
1. Saisissez un nom pour votre application et sélectionnez **Réserver le nom de produit**. La nouvelle inscription au Windows Store pour votre application est alors créée.

    ![Stocker le nom de l'application](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
1. Développez **Gestion des applications**, sélectionnez **WNS/MPNS**, sélectionnez **WNS/MPNS**, puis sélectionnez **Services Microsoft Live**. vous connecter à votre compte Microsoft ; Le **portail d’inscription des applications** s’ouvre dans un nouvel onglet. Vous pouvez également naviguer directement jusqu’au [portail d’inscription des applications](http://apps.dev.microsoft.com), sélectionnez le nom de votre application pour accéder à cette page.

    ![Page de WNS MPNS](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
1.   Notez le mot de passe **Clé secrète d’application** et la valeur **Identificateur de sécurité (SID) du package**.

        >[!WARNING]
        >Le secret d’application et le SID du package sont des informations d'identification de sécurité importantes. Ne partagez pas ces valeurs avec quiconque et ne les distribuez pas avec votre application.

## <a name="create-a-notification-hub"></a>Création d’un hub de notifications
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


### <a name="configure-wns-settings-for-the-hub"></a>Configurer les paramètres WNS le hub

1. Sélectionnez **Windows (GCM)** dans la catégorie **PARAMÈTRES DE NOTIFICATION**. 
2. Entrez les valeurs pour **SID de package** et **Clé de sécurité** notées à la section précédente. 
3. Sélectionnez **Enregistrer** dans la barre d’outils.

    ![Les zones SID du Package et Clé de sécurité](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Votre Notification Hub est désormais configuré pour fonctionner avec WNS. Vous disposez des chaînes de connexion pour inscrire votre application et envoyer des notifications.

## <a name="create-a-sample-windows-app"></a>Créer un exemple d’application Windows
1. Dans Visual Studio, sélectionnez **Fichier**, pointez sur **Nouveau**, puis sélectionnez **Projet**. 
2. Dans la boîte de dialogue **Nouveau projet**, procédez comme suit : 

    1. Développez **Visual C#**.
    2. Sélectionnez **Windows Universel**. 
    3. Sélectionnez **Application vide (Universal Windows)**. 
    4. Entrez un **nom** pour le projet. 
    5. Sélectionnez **OK**. 

        ![Boîte de dialogue Nouveau projet](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)
1. Acceptez les valeurs par défaut pour les versions de plateforme **cible** et **minimale** puis sélectionnez **OK**. 
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet d’application Windows Store, sélectionnez **Store**, puis **Associer l’application au Windows Store**. L'Assistant **Associer votre application au Windows Store** s'affiche.
3. Dans l’Assistant, connectez-vous avec votre compte Microsoft.
4. Sélectionnez l’application inscrite à l’étape 2, puis sélectionnez **Suivant** et **Associer**. Cela ajoute les informations d’inscription Windows Store requises au manifeste de l’application.
5. Dans Visual Studio, cliquez avec le bouton droit sur la solution, puis sélectionnez **Gérer les packages NuGet**. La fenêtre **Gérer les packages NuGet** s’ouvre.
6. Dans la zone de recherche, saisissez **WindowsAzure.Messaging.Managed**, sélectionnez **Installer**et acceptez les conditions d’utilisation.
   
    ![La fenêtre Gérer les packages NuGet][20]
   
    Cette action télécharge, installe et ajoute une référence à la bibliothèque Azure Notification Hubs pour Windows à l’aide du [package NuGet Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs).

3. Ouvrez le fichier projet App.xaml.cs et ajoutez les instructions `using` qui suivent : 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. Dans le fichier App.xaml.cs, ajoutez également la définition de méthode **InitNotificationsAsync** suivante à la classe **App** :
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Ce code récupère l’URI de canal pour l’application dans WNS et l’inscrit avec votre Notification Hub.
   
    >[!NOTE]
    >* Remplacez l’espace réservé **nom de hub** par le nom du hub de notification affiché sur le Portail Azure. 
    >* Remplacez également l’espace réservé de la chaîne de connexion par la chaîne de connexion **DefaultListenSharedAccessSignature** que vous avez obtenue sur la page **Stratégies d’accès** de votre hub de notification dans une section précédente.
   > 
   > 
5. En haut du gestionnaire d'événements **OnLaunched** dans App.xaml.cs, ajoutez l'appel suivant à la nouvelle méthode **InitNotificationsAsync** :
   
        InitNotificationsAsync();
   
    Cette action garantit l’inscription de l’URI de canal dans votre Notification Hub chaque fois que l’application est lancée.

6. Pour lancer l’application, appuyez sur **F5**. Une boîte de dialogue s’affiche avec la clé d’inscription. Cliquez sur **OK** pour fermer la boîte de dialogue. 

    ![Inscription réussie](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

Votre application est maintenant prête à recevoir des notifications toast.

## <a name="send-test-notifications"></a>Envoyer des notifications de test
Vous pouvez tester rapidement la réception de notifications dans votre application en envoyant des notifications dans le [Portail Azure](https://portal.azure.com/). 

1. Dans le portail Azure, basculez vers l’onglet Vue d’ensemble, puis sélectionnez **Test d’envoi** sur la barre d’outils.     

    ![Bouton de test d’envoi](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. Dans la fenêtre **Test d’envoi**, effectuez les actions suivantes : 
    1. Pour **Plateformes**, sélectionnez **Windows**.
    2. Pour **Type de Notification**, sélectionnez **Toast**. 
    3. Sélectionnez **Envoyer**. 
    
        ![Le volet du test d’envoi](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Examinez le résultat de l’opération d’envoi dans la liste **Résultat** au bas de la fenêtre. Vous voyez également un message d’alerte. 

    ![Résultat de l’opération d’envoi](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
1. Vous voyez le message de notification : **message de test** sur votre bureau. 

    ![Message de notification](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)


## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez envoyé des notifications à tous vos appareils Windows en utilisant le portail ou une application de console. Pour savoir comment envoyer des notifications à des appareils spécifiques, passez au didacticiel suivant : 

> [!div class="nextstepaction"]
>[Notifications Push vers des appareils spécifiques](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)


<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 
