---
title: 'Schnellstart: Erstellen eines Profils und eines Endpunkts – Resource Manager-Vorlage'
titleSuffix: Azure Content Delivery Network
description: Informationen zum Erstellen eines Azure Content Delivery Network-Profils und -Endpunkts mithilfe einer Resource Manager-Vorlage
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 06/25/2020
ms.author: allensu
ms.openlocfilehash: aca3c9c52260d3ea2ab8def37566377fa67e712d
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88705927"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint---arm-template"></a>Schnellstart: Erstellen eines Azure CDN-Profils und -Endpunkts – ARM-Vorlage

Hier finden Sie Informationen zu den ersten Schritten mit Azure Content Delivery Network (CDN) unter Verwendung einer Azure Resource Manager-Vorlage (ARM-Vorlage). Mit der Vorlage werden ein Profil und ein Endpunkt bereitgestellt.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cdn-with-custom-origin%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-cdn-with-custom-origin/).

Diese Vorlage ist für die Erstellung der folgenden Elemente konfiguriert:

* Profil
* Endpunkt

:::code language="json" source="~/quickstart-templates/101-cdn-with-custom-origin/azuredeploy.json":::

In der Vorlage ist eine einzelne Azure-Ressource definiert:

* **[Microsoft.Cdn/profiles](https://docs.microsoft.com/azure/templates/microsoft.cdn/profiles)**

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
read -p "Enter the location (i.e. eastus): " location
resourceGroupName="myResourceGroupCDN"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cdn-with-custom-origin/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az group deployment create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$location = Read-Host -Prompt "Enter the location (i.e. eastus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cdn-with-custom-origin/azuredeploy.json"

$resourceGroupName = "myResourceGroupCDN"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri
```

### <a name="portal"></a>Portal

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cdn-with-custom-origin%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie im linken Bereich **Ressourcengruppen** aus.

3. Wählen Sie die Ressourcengruppe aus, die Sie im vorherigen Abschnitt erstellt haben. Der Ressourcengruppenname lautet standardmäßig **myResourceGroupCDN**.

4. Vergewissern Sie sich, dass die folgenden Ressourcen in der Ressourcengruppe erstellt wurden:

    :::image type="content" source="media/create-profile-endpoint-template/cdn-profile-template-rg.png" alt-text="Azure CDN-Ressourcengruppe" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

### <a name="azure-cli"></a>Azure CLI

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#az-group-delete) entfernen.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupCDN
```

### <a name="powershell"></a>PowerShell

Wenn die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) entfernen.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupCDN
```

### <a name="portal"></a>Portal

Löschen Sie die Ressourcengruppe, das CDN-Profil und alle zugehörigen Ressourcen, wenn Sie diese nicht mehr benötigen. Wählen Sie die Ressourcengruppe **myResourceGroupCDN** aus, die das CDN-Profil und den CDN-Endpunkt enthält, und wählen Sie dann **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes erstellt:

* CDN-Profil
* Endpunkt

Weitere Informationen zu Azure CDN und Azure Resource Manager finden Sie in den folgenden Artikeln.

* Lesen Sie die [Übersicht über Azure CDN](cdn-overview.md).
* Lesen Sie weitere Informationen zu [Azure Resource Manager](../azure-resource-manager/management/overview.md).