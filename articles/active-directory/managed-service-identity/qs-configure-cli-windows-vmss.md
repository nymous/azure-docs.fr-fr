---
title: Guide pratique pour configurer des identités attribuées au système et à l’utilisateur sur un groupe de machines virtuelles identiques Azure à l’aide d’Azure CLI
description: Instructions étape par étape pour configurer des identités attribuées au système et à l’utilisateur sur un groupe de machines virtuelles identiques Azure à l’aide d’Azure CLI.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: a0e05543734ae0604149d18564ae1bc1eff1892b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714627"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Configurer l’identité du service administré (MSI) d’un groupe de machines virtuelles identiques à l’aide d’Azure CLI

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

L’identité du service administré fournit des services Azure avec une identité gérée automatiquement dans Azure Active Directory. Vous pouvez utiliser cette identité pour vous authentifier sur n’importe quel service prenant en charge l’authentification Azure AD, sans avoir d’informations d’identification dans votre code. 

Dans cet article, vous allez découvrir comment effectuer les opérations Managed Service Identity suivantes sur un groupe de machines virtuelles identiques Azure à l’aide d’Azure CLI :
- Activer et désactiver l’identité attribuée au système sur un groupe de machines virtuelles identiques Azure
- Ajouter et supprimer une identité attribuée par l’utilisateur sur un groupe de machines virtuelles identiques Azure


## <a name="prerequisites"></a>Prérequis

- Si vous ne connaissez pas MSI, consultez la [section Vue d’ensemble](overview.md). **Veillez à lire [la différence entre les identités attribuées au système et celles attribuées à l’utilisateur](overview.md#how-does-it-work)**.
- Si vous n’avez pas encore de compte Azure, [inscrivez-vous à un essai gratuit](https://azure.microsoft.com/free/) avant de continuer.

Pour exécuter les exemples de script d’Azure CLI, vous disposez de trois options :

- Utilisez [Azure Cloud Shell](../../cloud-shell/overview.md) à partir du portail Azure (voir section suivante).
- Utilisez l’interface intégrée Azure Cloud Shell via le bouton « Essayer », situé dans le coin supérieur droit de chaque bloc de code.
- [Installez la dernière version de CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou version ultérieure) si vous préférez utiliser une console CLI locale. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Identité attribuée au système

Dans cette section, vous allez découvrir comment activer et désactiver l’identité attribuée au système pour un groupe de machines virtuelles identiques Azure à l’aide d’Azure CLI.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Activer une identité attribuée au système lors de la création d’un groupe de machines virtuelles identiques Azure

Pour créer un groupe de machines virtuelles identiques avec l’identité attribuée au système activée

1. Si vous utilisez l’interface de ligne de commande Azure dans une console locale, commencez par vous connecter à Azure avec [az login](/cli/azure/reference-index#az_login). Utilisez un compte associé à l’abonnement Azure sur lequel vous souhaitez déployer le groupe de machines virtuelles identiques :

   ```azurecli-interactive
   az login
   ```

2. Créez un [groupe de ressources](../../azure-resource-manager/resource-group-overview.md#terminology) pour l’imbrication et le déploiement de votre groupe de machines virtuelles identiques et de ses ressources connexes, à l’aide de la commande [az group create](/cli/azure/group/#az_group_create). Vous pouvez ignorer cette étape si vous disposez déjà d’un groupe de ressources que vous souhaitez utiliser à la place :

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Créez un groupe de machines virtuelles identiques avec la commande [az vmss create](/cli/azure/vmss/#az_vmss_create). L’exemple suivant crée un groupe de machines virtuelles identiques nommé *myVMSS* avec une identité attribuée au système, comme le demande le paramètre `--assign-identity`. Les paramètres `--admin-username` et `--admin-password` spécifient le nom d’utilisateur et le mot de passe d’administration du compte pour la connexion à la machine virtuelle. Mettez à jour ces valeurs en fonction de votre environnement : 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Activer une identité attribuée au système sur un groupe de machines virtuelles identiques Azure existant

Si vous devez activer l’identité attribuée au système sur un groupe de machines virtuelles identiques Azure existant :

1. Si vous utilisez l’interface de ligne de commande Azure dans une console locale, commencez par vous connecter à Azure avec [az login](/cli/azure/reference-index#az_login). Utilisez un compte associé à l’abonnement Azure qui contient le groupe de machines virtuelles identiques.

   ```azurecli-interactive
   az login
   ```

2. Utilisez la commande [az vm identity assign](/cli/azure/vmss/identity/#az_vmss_identity_assign) pour activer l’identité attribuée au système sur une machine virtuelle existante :

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Désactiver une identité attribuée au système à partir d’un groupe de machines virtuelles identiques Azure

> [!NOTE]
> La désactivation de Managed Service Identity à partir d’un groupe de machines virtuelles identiques n’est pas prise en charge actuellement. En attendant, vous pouvez basculer entre des identités attribuées au système et des identités attribuées à l’utilisateur. Revenez ultérieurement pour des mises à jour.

Si vous avez un groupe de machines virtuelles identiques qui n’a plus besoin d’identité attribuée au système mais qui a toujours besoin d’identités attribuées à l’utilisateur, exécutez la commande suivante :

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='UserAssigned' 
```

Pour supprimer l’extension de machine virtuelle MSI, exécutez la commande [az vmss identity remove](/cli/azure/vmss/identity/#az_vmss_remove_identity) pour supprimer l’identité attribuée à l’utilisateur d’un groupe de machines virtuelles identiques :

   ```azurecli-interactive
   az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
   ```

## <a name="user-assigned-identity"></a>Identité attribuée à l’utilisateur

Dans cette section, vous allez découvrir comment activer et supprimer une identité attribuée à l’utilisateur à l’aide d’Azure CLI.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Attribuer une identité attribuée à l’utilisateur lors de la création d’un groupe de machines virtuelles identiques

Cette section explique en détail comment créer un groupe de machines virtuelles identiques et lui attribuer une identité attribuée à l’utilisateur. Si vous disposez déjà d’un groupe de machines virtuelles identiques que vous souhaitez utiliser, ignorez cette section et passez à la suivante.

1. Vous pouvez ignorer cette étape si vous disposez déjà d’un groupe de ressources que vous souhaitez utiliser. Créez un [groupe de ressources](~/articles/azure-resource-manager/resource-group-overview.md#terminology) pour contenir et déployer votre identité attribuée à l’utilisateur en exécutant la commande [az group create](/cli/azure/group/#az_group_create). N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>` et `<LOCATION>` par vos propres valeurs. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Créez une identité attribuée à l’utilisateur avec la commande [az identity create](/cli/azure/identity#az-identity-create).  Le paramètre `-g` spécifie le groupe de ressources où l’identité attribuée à l’utilisateur est créée, et le paramètre `-n` spécifie son nom. N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>` et `<USER ASSIGNED IDENTITY NAME>` par vos propres valeurs :

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
La réponse contient les détails de l’identité attribuée à l’utilisateur qui a été créée, comme dans l’exemple suivant. La valeur d’`id` de ressource attribuée à l’identité attribuée à l’utilisateur est utilisée à l’étape suivante.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Créez un groupe de machines virtuelles identiques à l’aide de [az vmss create](/cli/azure/vmss/#az-vmss-create). L’exemple suivant crée un groupe de machines virtuelles identiques associé à la nouvelle identité attribuée à l’utilisateur, tel que spécifié par le paramètre `--assign-identity`. N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` et `<USER ASSIGNED IDENTITY ID>` par vos propres valeurs. Pour `<USER ASSIGNED IDENTITY ID>`, utilisez la propriété `id` de la ressource de l’identité attribuée à l’utilisateur créée à l’étape précédente : 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Attribuer une identité attribuée à l’utilisateur à une machine virtuelle Azure existante

1. Créez une identité attribuée à l’utilisateur avec la commande [az identity create](/cli/azure/identity#az-identity-create).  Le paramètre `-g` spécifie le groupe de ressources où l’identité attribuée à l’utilisateur est créée, et le paramètre `-n` spécifie son nom. N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>` et `<USER ASSIGNED IDENTITY NAME>` par vos propres valeurs :

    > [!IMPORTANT]
    > La création d’identités attribuées à l’utilisateur avec des caractères spéciaux (tels qu’un trait de soulignement) dans le nom n’est pas prise en charge actuellement. Utilisez des caractères alphanumériques. Revenez ultérieurement pour des mises à jour.  Pour plus d’informations, consultez [FAQ et problèmes connus](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
La réponse contient les détails de l’identité attribuée à l’utilisateur qui a été créée, comme dans l’exemple suivant. La valeur d’`id` de ressource attribuée à l’identité attribuée à l’utilisateur est utilisée à l’étape suivante.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Attribuez l’identité attribuée à l’utilisateur à votre un groupe de machines virtuelles identiques à l’aide de la commande [az vmss identity assign](/cli/azure/vmss/identity#az_vm_assign_identity). N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>` et `<VMSS NAME>` par vos propres valeurs. `<USER ASSIGNED IDENTITY ID>` sera la propriété `id` de ressource de l’identité attribuée à l’utilisateur, tel que créée à l’étape précédente :

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Supprimer une identité attribuée à l’utilisateur d’un groupe de machines virtuelles identiques Azure

> [!NOTE]
>  La suppression de toutes les identités attribuées à l’utilisateur d’un groupe de machines virtuelles identiques n’est actuellement pas prise en charge, sauf si vous avez une identité attribuée au système. 

Si votre groupe de machines virtuelles identiques a plusieurs identités attribuées à l’utilisateur, vous pouvez les supprimer toutes sauf la dernière à l’aide de la commande [az vmss identity remove](/cli/azure/vmss/identity#az-vmss-identity-remove). N’oubliez pas de remplacer les valeurs des paramètres `<RESOURCE GROUP>` et `<VMSS NAME>` par vos propres valeurs. `<MSI NAME>` est la propriété de nom de l’identité attribuée à l’utilisateur, qui est accessible dans la section d’identité de la machine virtuelle en exécutant la commande `az vm show` :

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```
Si votre groupe de machines virtuelles identiques dispose à la fois d’identités attribuées par l’utilisateur et d’identités attribuées par le système, vous pouvez supprimer toutes les identités attribuées par l’utilisateur en choisissant de n’utiliser que des identités attribuées par le système. Utilisez la commande suivante : 

```azurecli-interactive
az vmss update -n <VMSS NAME> -g <RESOURCE GROUP> --set identity.type='SystemAssigned' identity.identityIds=null
```

## <a name="next-steps"></a>Étapes suivantes

- [Vue d’ensemble de l’identité du service administré](overview.md)
- Pour le démarrage rapide de la création d’un groupe de machines virtuelles identiques Azure, consultez : 

  - [Créer un groupe de machines virtuelles identiques avec CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















