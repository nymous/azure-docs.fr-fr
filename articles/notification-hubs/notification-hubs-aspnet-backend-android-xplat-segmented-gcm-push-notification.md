---
title: Notifications Push vers des appareils Android spécifiques avec Azure Notification Hubs et Google Cloud Messaging | Microsoft Docs
description: Apprenez à utiliser Notifications Hubs pour envoyer des notifications Push vers des appareils Android spécifiques à l’aide de Azure Notification Hubs et de Google Cloud Messaging.
services: notification-hubs
documentationcenter: android
author: dimazaid
manager: kpiteira
editor: spelluru'
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/06/2018
ms.author: dimazaid
ms.openlocfilehash: 00d6a2de3f51ef8ade50f29b14a4bc944ae5fee6
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33778407"
---
# <a name="tutorial-push-notifications-to-specific-android-devices-using-azure-notification-hubs-and-google-cloud-messaging"></a>Didacticiel : notifications Push vers des appareils Android à l’aide de Azure Notification Hubs et de Google Firebase Cloud Messaging
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Vue d'ensemble
Ce didacticiel vous présente l’utilisation d’Azure Notification Hubs pour diffuser des notifications relatives aux dernières nouvelles vers une application Android. Lorsque vous aurez terminé, vous pourrez vous inscrire aux catégories de dernières nouvelles qui vous intéressent et recevoir uniquement des notifications Push pour ces catégories. Ce scénario est un modèle courant pour de nombreuses applications pour lesquelles des notifications doivent être envoyées à des groupes d’utilisateurs qui ont signalé antérieurement un intérêt, par exemple, lecteur RSS, applications pour fans de musique, etc.

Les scénarios de diffusion sont activés en incluant une ou plusieurs *balises* durant la création d’une inscription dans le hub de notification. Lorsque des notifications sont envoyées à une balise, tous les appareils pour lesquels cette balise est inscrite reçoivent la notification. Les balises étant de simples chaînes, il n’est pas nécessaire de les mettre en service à l’avance. Pour plus d’informations sur les balises, consultez [Routage et expressions de balise Notification Hubs](notification-hubs-tags-segment-push-message.md).

Dans ce didacticiel, vous effectuez les actions suivantes : 

> [!div class="checklist"]
> * Ajout d’une sélection de catégories à l’application mobile.
> * Inscription à des notifications avec balises. 
> * Envoyer des notifications avec balises. 
> * Test de l'application

## <a name="prerequisites"></a>Prérequis

Ce didacticiel s’appuie sur l’application que vous avez créée dans le [didacticiel : notifications Push vers des appareils Android à l’aide de Azure Notification Hubs et de Google Cloud Messaging][get-started]. Avant d’effectuer ce didacticiel, terminez le [didacticiel : notifications Push vers des appareils Android à l’aide de Azure Notification Hubs et de Google Cloud Messaging][get-started].

## <a name="add-category-selection-to-the-app"></a>Ajout d’une sélection de catégories à l’application
La première étape consiste à ajouter des éléments de l’interface utilisateur à l’activité principale existante qui permettent à l’utilisateur de sélectionner des catégories auxquelles s’inscrire. Les catégories sélectionnées par un utilisateur sont stockées sur l'appareil. Lorsque l'application démarre, une inscription d'appareil est créée dans votre Notification Hub avec les catégories sélectionnées sous forme de balises.

1. Ouvrez votre fichier res/layout/activity_main.xml et remplacez le contenu par celui-ci :
   
    ```xml
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context="com.example.breakingnews.MainActivity"
        android:orientation="vertical">

            <CheckBox
                android:id="@+id/worldBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_world" />
            <CheckBox
                android:id="@+id/politicsBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_politics" />
            <CheckBox
                android:id="@+id/businessBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_business" />
            <CheckBox
                android:id="@+id/technologyBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_technology" />
            <CheckBox
                android:id="@+id/scienceBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_science" />
            <CheckBox
                android:id="@+id/sportsBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_sports" />
            <Button
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="subscribe"
                android:text="@string/button_subscribe" />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:id="@+id/text_hello"
            />
    </LinearLayout>
    ```
2. Ouvrez votre fichier res\values\string.xml et ajoutez les lignes suivantes :

    ```xml
    <string name="button_subscribe">Subscribe</string>
    <string name="label_world">World</string>
    <string name="label_politics">Politics</string>
    <string name="label_business">Business</string>
    <string name="label_technology">Technology</string>
    <string name="label_science">Science</string>
    <string name="label_sports">Sports</string>
    ```

    La présentation graphique de votre fichier main_activity.xml doit ressembler à l’image suivante :
   
    ![][A1]
3. Créez une classe `Notifications` dans le même package que votre classe **MainActivity**.

    ```java   
    import java.util.HashSet;
    import java.util.Set;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.os.AsyncTask;
    import android.util.Log;
    import android.widget.Toast;
    import android.view.View;

    import com.google.android.gms.gcm.GoogleCloudMessaging;
    import com.microsoft.windowsazure.messaging.NotificationHub;

    public class Notifications {
        private static final String PREFS_NAME = "BreakingNewsCategories";
        private GoogleCloudMessaging gcm;
        private NotificationHub hub;
        private Context context;
        private String senderId;

        public Notifications(Context context, String senderId, String hubName, 
                                String listenConnectionString) {
            this.context = context;
            this.senderId = senderId;

            gcm = GoogleCloudMessaging.getInstance(context);
            hub = new NotificationHub(hubName, listenConnectionString, context);
        }

        public void storeCategoriesAndSubscribe(Set<String> categories)
        {
            SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
            settings.edit().putStringSet("categories", categories).commit();
            subscribeToCategories(categories);
        }

        public Set<String> retrieveCategories() {
            SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
            return settings.getStringSet("categories", new HashSet<String>());
        }

        public void subscribeToCategories(final Set<String> categories) {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(senderId);

                        String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                        hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                            categories.toArray(new String[categories.size()]));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to register - " + e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    String message = "Subscribed for categories: "
                            + categories.toString();
                    Toast.makeText(context, message,
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

    }
    ```
       
    Cette classe utilise le stockage local pour stocker les catégories de nouvelles que cet appareil doit recevoir. Elle comporte également des méthodes pour s’inscrire à ces catégories.
4. Dans la classe **MainActivity**, supprimez vos champs privés pour **NotificationHub** et **GoogleCloudMessaging**, puis ajoutez un champ pour **Notifications** :

    ```java   
    // private GoogleCloudMessaging gcm;
    // private NotificationHub hub;
    private Notifications notifications;
    ```
5. Ensuite, dans la méthode **onCreate**, supprimez l’initialisation du champ **hub** et de la méthode **registerWithNotificationHubs**. Ajoutez ensuite les lignes suivantes, qui initialisent une instance de la classe **Notifications**. 

    ```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mainActivity = this;

        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId,
                MyHandler.class);

        notifications = new Notifications(this, NotificationSettings.SenderId, NotificationSettings.HubName, NotificationSettings.HubListenConnectionString);

        notifications.subscribeToCategories(notifications.retrieveCategories());
    }
    ```

    Vérifiez que le nom du concentrateur et la chaîne de connexion sont correctement définis dans la classe NotificationSettings.

    > [AZURE.NOTE] Les informations d’identification distribuées avec une application cliente n’étant généralement pas sécurisées, vous ne devez distribuer que la clé d’accès d’écoute avec votre application cliente. L'accès d'écoute permet à votre application de s'inscrire à des notifications, mais les inscriptions existantes ne peuvent pas être modifiées et les notifications ne peuvent pas être envoyées. La clé d’accès complet est utilisée dans un service de serveur principal sécurisé pour l’envoi de notifications et la modification d’inscriptions existantes.
1. Ajoutez ensuite les importations suivantes :

    ```java   
    import android.widget.CheckBox;
    import java.util.HashSet;
    import java.util.Set;
    ```
1. Ajoutez la méthode `subscribe` suivante pour gérer l’événement de clic sur le bouton d’abonnement :        
   
    ```java
    public void subscribe(View sender) {
        final Set<String> categories = new HashSet<String>();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        if (world.isChecked())
            categories.add("world");
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        if (politics.isChecked())
            categories.add("politics");
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        if (business.isChecked())
            categories.add("business");
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        if (technology.isChecked())
            categories.add("technology");
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        if (science.isChecked())
            categories.add("science");
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        if (sports.isChecked())
            categories.add("sports");

        notifications.storeCategoriesAndSubscribe(categories);
    }
    ```
       
    Cette méthode crée une liste de catégories et utilise la classe **Notifications** pour stocker la liste dans le stockage local et inscrire les balises correspondantes auprès du Notification Hub. Lorsque des catégories sont modifiées, l'inscription est à nouveau créée avec les nouvelles catégories.

Votre application peut désormais stocker un ensemble de catégories dans le stockage local sur l’appareil et s’inscrire auprès du hub de notification lorsque l’utilisateur modifie la sélection des catégories.

## <a name="register-for-notifications"></a>Inscription à des notifications
Les étapes suivantes permettent l’inscription auprès du hub de notification au démarrage en utilisant les catégories qui ont été stockées dans le stockage local.

> [!NOTE]
> Comme la valeur de registrationId affectée par Google Cloud Messaging (GCM) peut changer à n’importe quel moment, vous devez vous inscrire fréquemment aux notifications afin d’éviter les défaillances. Cet exemple s'inscrit aux notifications chaque fois que l'application démarre. Pour les applications exécutées fréquemment, plus d'une fois par jour, vous pouvez probablement ignorer l'inscription afin de préserver la bande passante si moins d'un jour s'est écoulé depuis l'inscription précédente.
> 
> 

1. Ajoutez le code suivant à la fin de la méthode **onCreate**, dans la classe **MainActivity** :
   
    ```java
    notifications.subscribeToCategories(notifications.retrieveCategories());
    ```
   
    Ce code garantit que chaque fois que l’application démarre, elle récupère les catégories du stockage local et demande une inscription pour ces catégories. 
2. Puis mettez à jour la méthode `onStart()` de la classe `MainActivity`, comme suit :
   
    ```java
    @Override
    protected void onStart() {
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }
    ```
   
    Ce code met l'activité principale à jour en fonction du statut des catégories enregistrées précédemment.

L'application est désormais terminée et peut stocker un ensemble de catégories dans le stockage local de l'appareil utilisé pour s'inscrire auprès du Notification Hub lorsque l'utilisateur modifie la sélection des catégories. Ensuite, définissez un serveur principal qui peut envoyer des notifications de catégorie à cette application.

## <a name="send-tagged-notifications"></a>Envoyer des notifications avec balises
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="test-the-app"></a>Test de l'application
1. Dans Android Studio, exécutez l’application sur votre appareil ou émulateur Android. L’interface utilisateur de l’application fournit un ensemble de bascules qui vous permet de choisir les catégories auxquelles vous abonner.
2. Activez une ou plusieurs bascules de catégories, puis cliquez sur **S'abonner**. L'application convertit les catégories sélectionnées en balises et demande une nouvelle inscription de l'appareil pour les balises sélectionnées depuis le Notification Hub. Les catégories inscrites sont renvoyées et affichées dans une notification toast.

    ![S’abonner à des catégories](./media/notification-hubs-aspnet-backend-android-breaking-news/subscribe-for-categories.png)
1. Exécutez l’application console .NET qui envoie des notifications pour chaque catégorie. Les notifications pour les catégories sélectionnées apparaissent comme notifications toast.

    ![Notifications de nouvelles technologies](./media/notification-hubs-aspnet-backend-android-breaking-news/technolgy-news-notification.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez envoyé des notifications de diffusion à des appareils Android spécifiques que sont enregistrés pour les catégories. Pour savoir comment envoyer des notifications à des utilisateurs spécifiques, passez au didacticiel suivant : 

> [!div class="nextstepaction"]
>[Notifications Push vers des utilisateurs spécifiques](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md)

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure portal]: https://portal.azure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
