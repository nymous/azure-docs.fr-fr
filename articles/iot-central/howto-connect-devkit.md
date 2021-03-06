---
title: Connecter un appareil DevKit à votre application Azure IoT Central | Microsoft Docs
description: En tant que développeur d’appareils, apprenez à connecter un appareil DevKit IoT MXChip à votre application Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d7b92359e8875c281fd460f1f5307a7941c11c1f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261574"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Connecter un appareil DevKit IoT MXChip à votre application Azure IoT Central

Cet article vous explique comment, en tant que développeur d’appareils, vous pouvez connecter un appareil DevKit IoT MXChip (DevKit) à votre application Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Avant de commencer

Pour effectuer les étapes de cet article, vous avez besoin des éléments suivants :

1. Une application Azure IoT Central créée à partir du modèle d’application **Exemples de Devkits**. Pour plus d’informations, consultez [Créer votre application Azure IoT Central](howto-create-application.md).
1. Un appareil DevKit. Pour acheter un appareil DevKit, accédez à [MXChip IoT DevKit](http://mxchip.com/az3166).

Une application créée à partir du modèle d’application **Exemples de Devkits** comprend un modèle d’appareil **MXChip** qui présente les caractéristiques suivantes :

### <a name="measurements"></a>Mesures

#### <a name="telemetry"></a>Télémétrie 

| Nom du champ     | Units  | Minimale | Maximale | Nombre de décimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humidity       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1 260    | 0              |
| magnetometerX  | mgauss | -1 000   | 1 000    | 0              |
| magnetometerY  | mgauss | -1 000   | 1 000    | 0              |
| magnetometerZ  | mgauss | -1 000   | 1 000    | 0              |
| accelerometerX | mg     | -2 000   | 2000    | 0              |
| accelerometerY | mg     | -2 000   | 2000    | 0              |
| accelerometerZ | mg     | -2 000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2 000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2 000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2 000   | 2000    | 0              |

#### <a name="states"></a>États 

| NOM          | Nom complet   | NORMAL | AVERTISSEMENT | DANGER | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | État de l’appareil   | Vert  | Orange  | Rouge    | 

#### <a name="events"></a>Événements 

| NOM             | Nom complet      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Bouton B enfoncé  | 



### <a name="settings"></a>Paramètres

Paramètres numériques

| Nom complet | Nom du champ | Units | Nombre de décimales | Minimale | Maximale | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage      | setVoltage | Volts | 0              | 0       | 240     | 0       |
| Current      | setCurrent | Amps  | 0              | 0       | 100     | 0       |
| Vitesse du ventilateur    | fanSpeed   | TR/MIN   | 0              | 0       | 1 000    | 0       |

Paramètres de bascule

| Nom complet | Nom du champ | Texte pour Activé | Texte pour Désactivé | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ACTIVÉ      | ÉTEINT      | Off     |

### <a name="properties"></a>properties

| type            | Nom complet | Nom du champ | Type de données |
| --------------- | ------------ | ---------- | --------- |
| Propriété d’appareil | Numéro gravé   | dieNumber  | number    |
| Texte            | Lieu     | location   | N/A       |


### <a name="add-a-real-device"></a>Ajouter un appareil réel

Dans votre application Azure IoT Central, ajoutez un appareil réel à partir du modèle d’appareil **MXChip** et notez la chaîne de connexion de l’appareil. Pour plus d’informations, consultez [Ajouter un appareil réel à votre application Azure IoT Central](tutorial-add-device.md).

## <a name="prepare-the-devkit-device"></a>Préparer l’appareil DevKit

> [!TIP]
> Pour obtenir des conseils de dépannage sur les appareils DevKit, consultez [IoT DevKit get started](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/).

Pour préparer l’appareil DevKit :

1. Téléchargez la dernière version du microprogramme Azure IoT Central prédéfini pour MXChip à partir de la page [Releases](https://github.com/Azure/iot-central-firmware/releases) de GitHub. Voici comment se présente le nom du fichier de téléchargement dans la page Releases : `AZ3166-IoT-Central-X.X.X.bin`.

1. Connectez l’appareil DevKit à votre machine de développement à l’aide d’un câble USB. Dans Windows, une fenêtre Explorateur de fichiers s’ouvre sur un lecteur mappé au stockage de l’appareil DevKit. Par exemple, le lecteur peut s’appeler **AZ3166 (d)**.

1. Faites glisser le fichier **iotCentral.bin** jusqu’à la fenêtre du lecteur. Une fois la copie effectuée, l’appareil redémarre avec le nouveau microprogramme.

1. Pendant le redémarrage de l’appareil DevKit, voici l’écran que vous obtenez :

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Si cet écran affiche autre chose, appuyez sur le bouton de **réinitialisation** de l’appareil. 

1. L’appareil est maintenant en mode AP (point d’accès). Vous pouvez vous connecter à ce point d’accès Wi-Fi à partir de votre ordinateur ou appareil mobile.

1. Sur votre ordinateur, téléphone ou tablette, connectez-vous au réseau Wi-Fi dont le nom est affiché sur l’écran de l’appareil. Quand vous vous connectez à ce réseau, vous n’avez pas accès à Internet. Il s’agit d’un état normal. Vous êtes connecté uniquement à ce réseau pendant un bref laps de temps, le temps de configurer l’appareil.

1. Ouvrez votre navigateur web et accédez à [http://192.168.0.1/start](http://192.168.0.1/start). La page web suivante s’affiche à l’écran :

    ![Page de configuration de l’appareil](media/howto-connect-devkit/configpage.png)

    Dans la page web : 
    - ajoutez le nom de votre réseau Wi-Fi 
    - le mot de passe de votre réseau Wi-Fi 
    - le code PIN (PIN CODE) indiqué sur l’écran de l’appareil 
    - la chaîne de connexion de votre appareil. 
      Vous pouvez trouver la chaîne de connexion à l’emplacement suivant : `https://apps.iotcentral.com` -> `Device Explorer` -> `Device` -> `Select or Create a new Real Device` -> `Connect this device` (en haut à droite) 
    - Sélectionnez toutes les mesures de télémétrie disponibles. 

1. Après avoir choisi **Configurer l’appareil**, cette page apparaît :

    ![Appareil configuré](media/howto-connect-devkit/deviceconfigured.png)

1. Appuyez sur le bouton de **réinitialisation** de votre appareil.

> [!NOTE]
> Pour reconfigurer l’appareil de façon à utiliser un autre réseau Wi-Fi, une autre chaîne de connexion ou une autre mesure de télémétrie, appuyez simultanément sur les boutons **A** et **B** de la carte. Si cela ne fonctionne pas, appuyez sur le bouton de **réinitialisation**, puis réessayez. 

## <a name="view-the-telemetry"></a>Afficher les données de télémétrie

Pendant le redémarrage de l’appareil DevKit, l’écran de l’appareil affiche les informations suivantes :

* Nombre de messages de télémétrie envoyés.
* Nombre d’échecs.
* Nombre de propriétés souhaitées reçues et nombre de propriétés signalés envoyées.

Le fait de secouer l’appareil a pour effet d’augmenter le nombre de propriétés signalées envoyées. L’appareil envoie un nombre aléatoire pour la propriété de l’appareil **Numéro gravé**.

Vous pouvez consulter les mesures de télémétrie et les valeurs des propriétés signalées et configurer les paramètres dans Azure IoT Central :

1. Utilisez l’**Explorateur d’appareils** pour accéder à la page **Mesures** pour l’appareil MXChip réel que vous avez ajouté :

    ![Accéder à l’appareil réel](media/howto-connect-devkit/realdevice.png)

1. Dans la page **Mesures**, vous pouvez examiner les données de télémétrie en provenance de l’appareil MXChip :

    ![Afficher les données de télémétrie de l’appareil réel](media/howto-connect-devkit/realtelemetry.png)

1. Dans la page **Propriétés**, vous pouvez voir le dernier numéro gravé signalé par l’appareil :

    ![Afficher les propriétés de l’appareil](media/howto-connect-devkit/deviceproperties.png)

1. Dans la page **Propriétés**, vous pouvez mettre à jour les paramètres de l’appareil MXChip :

    ![Afficher les paramètres de l’appareil](media/howto-connect-devkit/settings.png)

## <a name="download-the-source-code"></a>Télécharger le code source

Si vous souhaitez explorer et modifier le code de l’appareil, vous pouvez le télécharger à partir de GitHub. Si vous envisagez de modifier le code, suivez ces instructions pour [préparer l’environnement de développement](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) du système d’exploitation de votre ordinateur.

Pour télécharger le code source, exécutez la commande suivante sur votre ordinateur de bureau :

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

La commande précédente télécharge le code source dans un dossier appelé `iot-central-firmware`. 

> [!NOTE]
> Si **git** n’est pas installé dans votre environnement de développement, vous pouvez le télécharger à partir de [https://git-scm.com/download](https://git-scm.com/download).

## <a name="review-the-code"></a>Vérifier le code

Utilisez Visual Studio Code, qui a été installé pendant la préparation de votre environnement de développement, pour ouvrir le dossier `AZ3166` à l’intérieur du dossier `iot-central-firmware` : 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Pour voir comment les données de télémétrie sont envoyées à l’application Azure IoT Central, ouvrez le fichier **main_telemetry.cpp** dans le dossier source.

La fonction `buildTelemetryPayload` crée la charge utile de télémétrie JSON en utilisant les données issues des capteurs de l’appareil.

La fonction `sendTelemetryPayload` appelle `sendTelemetry` dans **iotHubClient.cpp** pour envoyer la charge utile JSON au hub IoT qu’utilise votre application Azure IoT Central.

Pour voir comment les valeurs des propriétés sont signalées à l’application Azure IoT Central, ouvrez le fichier **main_telemetry.cpp** dans le dossier source.

La fonction `telemetryLoop` envoie la propriété signalée **doubleTap** quand l’accéléromètre détecte un double appui. Elle utilise la fonction `sendReportedProperty` dans le fichier source **iotHubClient.cpp**.

Le code du fichier source **iotHubClient.cpp** utilise les fonctions des [kits SDK Microsoft Azure IoT et des bibliothèques pour C](https://github.com/Azure/azure-iot-sdk-c) pour interagir avec IoT Hub.

Pour plus d’informations sur la façon de modifier, générer et charger l’exemple de code sur votre appareil, consultez le fichier **readme.md** dans le dossier`AZ3166`.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous savez comment connecter un appareil DevKit à votre application Azure IoT Central, voici les étapes suivantes suggérées :

* [Préparer et connecter un Raspberry Pi](howto-connect-raspberry-pi-python.md)