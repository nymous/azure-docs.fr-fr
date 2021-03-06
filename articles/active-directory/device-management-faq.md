---
title: FAQ sur la gestion des appareils Azure Active Directory | Documents Microsoft
description: FAQ sur la gestion des appareils Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: c8b0529b0ae45d7bcee5574991551a424c13ba70
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713862"
---
# <a name="azure-active-directory-device-management-faq"></a>FAQ sur la gestion des appareils Azure Active Directory



**Q : Comment puis-je inscrire un appareil macOS ?**

**R :** Pour inscrire un appareil macOS :

1.  [Créez une stratégie de conformité](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Définissez une stratégie d’accès conditionnel pour les appareils macOS](active-directory-conditional-access-azure-portal.md) 

**Remarques :**

- Les utilisateurs qui sont inclus dans votre stratégie d’accès conditionnel ont besoin d’une [version d’Office pour macOS prise en charge](active-directory-conditional-access-technical-reference.md#client-apps-condition) pour accéder aux ressources. 

- Lors de la première tentative d’accès, vos utilisateurs sont invités à inscrire l’appareil par l’intermédiaire du portail d’entreprise.

---

**Q : J’ai enregistré récemment l’appareil. Pourquoi ne puis-je pas voir l’appareil sous mes informations d’utilisateur dans le portail Azure ?**

**R :** Les appareils Windows 10 qui sont joints à Azure AD hybride ne s’affichent pas en tant qu’appareils UTILISATEUR.
Vous devez utiliser PowerShell pour afficher tous les appareils. 

Seuls les appareils suivants sont répertoriés en tant qu’appareils UTILISATEUR :

- Tous les appareils personnels qui ne sont pas joints à Azure AD hybride. 
- Tous les appareils non Windows 10 / Windows Server 2016.
- Tous les appareils non Windows 

---

**Q : Pourquoi ne puis-je pas voir tous les appareils inscrits auprès d’Azure Active Directory dans le portail Azure ?** 

**R :** Vous pouvez maintenant les voir dans le menu Azure AD Directory -> tous les appareils. Vous pouvez aussi utiliser Azure PowerShell pour rechercher tous les appareils. Pour plus d’informations, consultez l’applet de commande [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0).

--- 

**Q : Comment puis-je connaître l’état de l’inscription d’appareils du client ?**

**R :** Pour les appareils Windows 10 et Windows Server 2016 ou versions ultérieures, exécutez dsregcmd.exe /status.

Pour les versions de système d’exploitation de niveau inférieur, exécutez « %programFiles%\Microsoft Workplace Join\autoworkplace.exe »

---

**Q : Pourquoi un appareil que j’ai supprimé dans le portail Azure ou à l’aide de Windows PowerShell est-il toujours répertorié comme inscrit ?**

**R :** Il s’agit du comportement par défaut. L’appareil n’aura pas accès aux ressources dans le cloud. Si vous souhaitez inscrire à nouveau l’appareil, vous devez effectuer une action manuelle sur celui-ci. 

Pour effacer l’état de jointure des appareils Windows 10 et Windows Server 2016 sur site et joints à un domaine AD :

1.  Ouvrez une invite de commandes en tant qu’administrateur.

2.  Saisissez `dsregcmd.exe /debug /leave`

3.  Déconnectez-vous, puis reconnectez-vous pour déclencher la tâche planifiée qui inscrit à nouveau l’appareil auprès d’Azure AD. 

Pour les versions de système d’exploitation Windows de niveau inférieur des appareils sur site et joints à un domaine AD :

1.  Ouvrez une invite de commandes en tant qu’administrateur.
2.  Saisissez `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Saisissez `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**Q : Pourquoi le portail Azure affiche-t-il des entrées d’appareils dupliquées ?**

**R :**

-   Pour Windows 10 et Windows Server 2016, en cas de tentatives répétées visant à disjoindre et à joindre à nouveau le même appareil, des entrées dupliquées peuvent s’afficher. 

-   Si vous avez utilisé « Ajouter un compte professionnel ou scolaire », chaque utilisateur Windows qui utilise « Ajouter un compte professionnel ou scolaire » créera un nouvel enregistrement d’appareil avec le même nom d’appareil.

-   Pour les versions de système d’exploitation Windows de niveau inférieur des appareils sur site et joints à un domaine AD, l’inscription automatique permettra de créer un nouvel enregistrement d’appareil avec le même nom d’appareil pour chaque utilisateur du domaine qui se connecte à l’appareil. 

-   Une machine jointe Azure AD qui a été réinitialisée, réinstallée et jointe à nouveau avec le même nom s’affichera en tant que nouvel enregistrement avec le même nom d’appareil.

---

**Q : Pourquoi un utilisateur peut-il toujours accéder aux ressources à partir d’un appareil que j’ai désactivé dans le portail Azure ?**

**R :** Une opération de révocation peut prendre jusqu’à une heure pour être entièrement appliquée.

>[!Note] 
>Pour les appareils inscrits, nous vous recommandons de réinitialiser l’appareil pour vous assurer que les utilisateurs ne puissent pas accéder aux ressources. Pour plus d’informations, consultez [Inscrire des appareils pour la gestion dans Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Q : Pourquoi mes utilisateurs voient-ils s’afficher « Vous ne pouvez pas accéder à cet emplacement à partir d’ici » ?**

**R :** Si vous avez configuré certaines règles d’accès conditionnel pour exiger un état d’appareil spécifique et que l’appareil ne respecte pas les critères, les utilisateurs sont bloqués et ce message s’affiche. Veuillez évaluer les règles de la stratégie d’accès conditionnel et vous assurer que l’appareil est en mesure de respecter les critères pour éviter l’affichage de ce message.

---


**Q : Je vois l’enregistrement d’appareil sous les informations UTILISATEUR dans le portail Azure, ainsi que l’état en tant qu’inscrit sur le client. Ma configuration est-elle correcte pour l’utilisation de l’accès conditionnel ?**

**R :** L’état de jointure de l’appareil, reflété par l’ID d’appareil, doit correspondre à celui d’Azure AD et répondre à tous les critères d’évaluation pour l’accès conditionnel. Pour plus d’informations, consultez [Bien démarrer avec le service Azure Active Directory Device Registration](active-directory-device-registration.md).

---

**Q : Pourquoi est-ce que je reçois le message « nom d’utilisateur ou mot de passe incorrect » pour un appareil que je viens juste de joindre à Azure AD ?**

**R :** Les raisons les plus courantes sont les suivantes :

- Vos informations d’identification ne sont plus valides.

- Votre ordinateur n’est pas en mesure de communiquer avec Azure Active Directory. Vérifiez les éventuels problèmes de connectivité réseau.

- Les conditions préalables pour la jonction à Azure AD n’ont pas été respectées. Veuillez vous assurer que vous avez respecté les étapes de la section [Extension des fonctionnalités du cloud aux appareils Windows 10 via Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Les connexions fédérées nécessitent que votre serveur de fédération prenne en charge un point de terminaison WS-Trust actif. 

---

**Q : Pourquoi la boîte de dialogue « Désolé... une erreur s’est produite ! » s’affiche-t-elle lorsque j’essaye de joindre mon ordinateur à Azure AD ?**

**R :** Cela résulte de la configuration de l’inscription Azure Active Directory avec Intune. Assurez-vous que l’utilisateur qui tente de créer la jointure Azure AD dispose de la licence Intune appropriée. Pour plus d’informations, consultez [Configurer la gestion des appareils Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Q : Pourquoi ma tentative d’inscription d’un ordinateur a-t-elle échoué alors que je n’ai reçu aucune information d’erreur ?**

**R :** Une cause possible est que l’utilisateur est connecté à l’appareil à l’aide du compte administrateur intégré. Créez un compte local distinct avant d’utiliser Azure Active Directory Join pour terminer la configuration. 

---

**Q : Où puis-je trouver des instructions pour la configuration de l’inscription automatique  d’appareils ?**

**R :** Pour plus d’instructions, consultez [Configuration de l’inscription automatique auprès d’Azure Active Directory d’appareils Windows joints à un domaine](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**Q : Où puis-je trouver des informations de résolution des problèmes concernant l’inscription d’appareils automatique ?**

**R :** Pour obtenir des informations de résolution des problèmes, consultez :

- [Résolution des problèmes de l’inscription automatique des ordinateurs joints au domaine à Azure AD – Windows 10 et Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Résolution des problèmes de l’inscription automatique des ordinateurs joints au domaine à Azure AD pour les clients de bas niveau Windows](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

