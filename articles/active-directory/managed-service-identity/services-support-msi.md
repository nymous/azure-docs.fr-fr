---
title: Services Azure qui prennent en charge l’identité du service administré
description: Liste des services qui prennent en charge l’identité du service administré et l’authentification Azure AD
services: active-directory
author: daveba
ms.author: daveba
ms.date: 03/28/2018
ms.topic: reference
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 2d896b11ae94355eb2bdcfa8bc3a647f96fd8caf
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723760"
---
# <a name="services-that-support-managed-service-identity"></a>Services qui prennent en charge l’identité du service administré 

L’identité du service administré fournit des services Azure avec une identité gérée automatiquement dans Azure Active Directory. Avec une identité gérée, vous pouvez vous authentifier sur n’importe quel service prenant en charge l’authentification Azure AD, sans avoir d’informations d’identification dans votre code. Nous sommes en train d’intégrer l’identité de service administré et l’authentification Azure AD sur Azure. Vérifiez régulièrement cette page si des mises à jour sont disponibles.

## <a name="azure-services-that-support-managed-service-identity"></a>Services Azure qui prennent en charge l’identité du service administré

Les services Azure suivants prennent en charge l’identité du service administré.

| de diffusion en continu | Statut | Date | Configuration | Obtention d’un jeton |
| ------- | ------ | ---- | --------- | ----------- |
| Machines virtuelles Azure | VERSION PRÉLIMINAIRE | Septembre 2017 | [Portail Azure](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[interface de ligne de commande Azure](qs-configure-cli-windows-vm.md)<br>[Modèles Microsoft Azure Resource Manager](qs-configure-template-windows-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Azure App Service | VERSION PRÉLIMINAIRE | Septembre 2017 | [Portail Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Modèle Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Functions | VERSION PRÉLIMINAIRE | Septembre 2017 | [Portail Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Modèle Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) |
| Azure Data Factory V2 | VERSION PRÉLIMINAIRE | Novembre 2017 | [Portail Azure](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[Foundation](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Services Azure qui prennent en charge l’authentification Azure AD

Les services suivants prennent en charge l’authentification Azure AD et ont été testés avec les services clients qui utilisent l’identité du service administré.

| de diffusion en continu | ID de ressource | Statut | Date | Attribuer l’accès |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Disponible | Septembre 2017 | [Portail Azure](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[interface de ligne de commande Azure](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Disponible | Septembre 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Disponible | Septembre 2017 | |
| Azure SQL | https://database.windows.net/ | Disponible | Octobre 2017 | |
| Hubs d'événements Azure | https://eventhubs.azure.net | Disponible | Décembre 2017 | |
| Azure Service Bus | https://servicebus.azure.net | Disponible | Décembre 2017 | |
| Stockage Azure | https://storage.azure.com/ | VERSION PRÉLIMINAIRE | Mai 2018 | |
