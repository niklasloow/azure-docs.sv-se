---
title: Skapa en SQL-pool med hjälp av Azure Resource Manager mall
description: Lär dig hur du skapar en SQL-pool för Azure Synapse Analytics med hjälp av Azure Resource Manager mall.
services: azure-resource-manager
author: julieMSFT
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: jrasnick
ms.date: 06/09/2020
ms.openlocfilehash: 29d4e4d696b34aa493714c870ebb466f491c47fe
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88641882"
---
# <a name="quickstart-create-an-azure-synapse-analytics-sql-pool-by-using-an-arm-template"></a>Snabb start: skapa en SQL-pool för Azure Synapse Analytics med en ARM-mall

Med den här Azure Resource Manager mallen (ARM-mallen) skapas en Azure Synapse Analytics SQL-pool med transparent datakryptering aktiverat. Synapse SQL-pool syftar på de företags data lager funktioner som är allmänt tillgängliga i Azure Synapse.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Om din miljö uppfyller förhandskraven och du är van att använda ARM-mallar väljer du knappen **Distribuera till Azure**. Mallen öppnas på Azure-portalen.

[![Distribuera till Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Krav

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="review-the-template"></a>Granska mallen

Mallen som används i den här snabbstarten kommer från [Azure-snabbstartsmallar](https://azure.microsoft.com/resources/templates/201-sql-data-warehouse-transparent-encryption-create/).

:::code language="json" source="~/quickstart-templates/201-sql-data-warehouse-transparent-encryption-create/azuredeploy.json":::

Mallen definierar en resurs:

- [Microsoft. SQL/Servers](/azure/templates/microsoft.sql/servers)

## <a name="deploy-the-template"></a>Distribuera mallen

1. Välj följande bild för att logga in på Azure och öppna mallen. Den här mallen skapar en Synapse SQL-pool.
   
   [![Distribuera till Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-sql-data-warehouse-transparent-encryption-create%2Fazuredeploy.json)

1. Ange eller uppdatera följande värden:

   * **Prenumeration**: Välj en Azure-prenumeration.
   * **Resurs grupp**: Välj **Skapa ny** och ange ett unikt namn för resurs gruppen och välj **OK**. En ny resurs grupp gör det lättare att rensa resursen.
   * **Region**: Välj en region.  Välj till exempel **USA, centrala**.
   * **SQL Server namn**: acceptera standard namnet eller ange ett namn för SQL Server namnet.
   * **SQL-administratör inloggning**: Ange administratörs användar namnet för SQL Server.
   * **SQL-administratörs lösen ord**: Ange administratörs lösen ordet för SQL Server.
   * **Data lager namn**: Ange ett namn på en SQL-pool.
   * **Transparent datakryptering**: Godkänn standardinställningen, aktive rad. 
   * **Service nivå mål**: acceptera standard-DW400c.
   * **Plats**: acceptera standard platsen för resurs gruppen.
   * **Granska och skapa**: Välj.
   * **Skapa**: Välj.

## <a name="review-deployed-resources"></a>Granska distribuerade resurser

Du kan antingen använda Azure Portal för att kontrol lera de distribuerade resurserna eller använda Azure CLI eller Azure PowerShell skript för att visa en lista över distribuerade resurser.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the resource group where your Synapse SQL pool exists:" &&
read resourcegroupName &&
az resource list --resource-group $resourcegroupName 
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name where your SQL pool account exists"
(Get-AzResource -ResourceType "Microsoft.Sql/servers/databases" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Rensa resurser

När de inte längre behövs tar du bort resurs gruppen med hjälp av Azure CLI eller Azure PowerShell:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Nästa steg

I den här snabb starten skapade du en Azure Synapse Analytics SQL-pool med en ARM-mall och validerat distributionen. Om du vill veta mer om Azure Synapse Analytics och Azure Resource Manager kan du fortsätta till artiklarna nedan.

- Läs en [Översikt över Azure Synapse Analytics](sql-data-warehouse-overview-what-is.md)
- Läs mer om [Azure Resource Manager](../../azure-resource-manager/management/overview.md)
- [Skapa och distribuera din första ARM-mall](../../azure-resource-manager/templates/template-tutorial-create-first-template.md)
