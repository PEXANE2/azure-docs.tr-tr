---
title: 'Hızlı başlangıç: profil ve uç nokta Kaynak Yöneticisi şablonu oluşturma'
titleSuffix: Azure Content Delivery Network
description: Bir Azure Content Delivery Network profili ve uç nokta Kaynak Yöneticisi şablonu oluşturmayı öğrenin
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: allensu
ms.openlocfilehash: bdb30fed4dadef84fe012c11893661b8d9d70325
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85483049"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint---resource-manager-template"></a>Hızlı başlangıç: Azure CDN profili ve uç nokta Kaynak Yöneticisi şablonu oluşturma

Azure Resource Manager şablonu kullanarak Azure CDN kullanmaya başlayın.  Bu şablon bir profil ve uç nokta dağıtır.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-cdn-profile-and-endpoint"></a>CDN profili ve uç noktası oluşturma

Bu şablon, şunu oluşturacak şekilde yapılandırıldı:

* Profil
* Uç Nokta

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cdn-with-custom-origin/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cdn-with-custom-origin/azuredeploy.json" range="1-125" highlight="46-117":::


Şablonda bir Azure kaynağı tanımlanmıştır:

**Microsoft. CDN**

* **[Microsoft. CDN/profiller](https://docs.microsoft.com/azure/templates/microsoft.cdn/profiles)**

### <a name="deploy-the-template"></a>Şablonu dağıtma

**Azure CLI**

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

**Azure PowerShell**

```azurepowershell-interactive
$location = Read-Host -Prompt "Enter the location (i.e. eastus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cdn-with-custom-origin/azuredeploy.json"

$resourceGroupName = "myResourceGroupCDN"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri
```

**Azure portal**

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cdn-with-custom-origin%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmeden **kaynak grupları** ' nı seçin.

3. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı **Myresourcegroupcdn**

4. Kaynak grubunda aşağıdaki kaynakların oluşturulduğunu doğrulayın:

    :::image type="content" source="media/create-profile-endpoint-template/cdn-profile-template-rg.png" alt-text="Azure CDN kaynak grubu" border="true":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Azure CLI**

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupCDN
```

**Azure PowerShell**

Artık gerekli değilse, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupCDN
```

**Azure portal**

Artık gerekli olmadığında kaynak grubunu, CDN profilini ve tüm ilgili kaynakları silin. CDN profili ve uç noktası içeren **Myresourcegroupcdn** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunu oluşturdunuz:

* CDN profili
* Uç Nokta

Azure CDN ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

* [Azure CDN genel bakışını](cdn-overview.md) okuyun
* [Azure Resource Manager](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin