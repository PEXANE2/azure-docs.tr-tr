---
title: 'Hızlı başlangıç: NAT Gateway-Kaynak Yöneticisi şablonu oluşturma'
titleSuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıçta, Azure Resource Manager şablonunu kullanarak bir NAT ağ geçidinin nasıl oluşturulacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway by using an Azure Resource Manager template so that I can provide outbound connectivity for my virtual machines.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/09/2020
ms.author: allensu
ms.custom: subject-armqs
ms.openlocfilehash: 051b1bd22b2e04f92c02b63a416160d824d1be10
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79216998"
---
# <a name="quickstart-create-a-nat-gateway---resource-manager-template"></a>Hızlı başlangıç: NAT Gateway-Kaynak Yöneticisi şablonu oluşturma

Azure Resource Manager şablonu kullanarak sanal ağ NAT ile çalışmaya başlayın.  Bu şablon bir sanal ağ, bir NAT ağ geçidi ve Ubuntu sanal makinesi dağıtır. Ubuntu sanal makinesi, NAT ağ geçidiyle ilişkili bir alt ağa dağıtılır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-nat-gateway-and-supporting-resources"></a>NAT ağ geçidi ve destekleyici kaynaklar oluşturma

Bu şablon, bir 

* Sanal ağ 
* NAT ağ geçidi kaynağı
* Ubuntu sanal makinesi

Ubuntu VM, NAT ağ geçidi kaynağıyla ilişkili bir alt ağa dağıtılır.

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-nat-gateway-1-vm/azuredeploy.json" range="1-335" highlight="256-282":::

Şablonda dokuz Azure kaynağı tanımlanmıştır:

**Microsoft. Network**

* **[Microsoft. Network/natgateway](https://docs.microsoft.com/azure/templates/microsoft.network/natgateways)** : bir NAT ağ geçidi kaynağı oluşturur.

* **[Microsoft. Network/networkSecurityGroups](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)** : bir ağ güvenlik grubu oluşturur.

    * **[Microsoft. Network/networkSecurityGroups/securityRules](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups/securityrules)** : bir güvenlik kuralı oluşturur.

* **[Microsoft. Network/Publicıpaddresses](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)** : genel bir IP adresi oluşturur.

* **[Microsoft. Network/Publicıpprefix](https://docs.microsoft.com/azure/templates/microsoft.network/publicipprefixes)** : genel bir IP öneki oluşturur.

* **[Microsoft. Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)** : bir sanal ağ oluşturur.

    * **[Microsoft. Network/virtualNetworks/alt ağları](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)** : bir sanal ağ alt ağı oluşturur.

* **[Microsoft. Network/NetworkInterfaces](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)** : bir ağ arabirimi oluşturur.

**Microsoft. COMPUTE**

* **[Microsoft. COMPUTE/virtualMachines](https://docs.microsoft.com/azure/templates/Microsoft.Compute/virtualMachines)** : bir sanal makine oluşturur.

### <a name="deploy-the-template"></a>Şablonu dağıtma

**Azure CLI**

```azurecli-interactive
read -p "Enter the location (i.e. westcentralus): " location
resourceGroupName="myResourceGroupNAT"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az group deployment create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

**Azure PowerShell**

```azurepowershell-interactive
$location = Read-Host -Prompt "Enter the location (i.e. westcentralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json"

$resourceGroupName = "myResourceGroupNAT"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri
```

**Azure portalında**

[![Azure’a dağıtma](./media/quick-create-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-nat-gateway-1-vm%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmeden **kaynak grupları** ' nı seçin.

3. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı **Myresourcegroupnat** ' dır

4. Kaynak grubunda aşağıdaki kaynakların oluşturulduğunu doğrulayın:

    ![Sanal ağ NAT kaynak grubu](./media/quick-create-template/nat-gateway-template-rg.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Azure CLI**

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

**Azure PowerShell**

Artık gerekli değilse, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

**Azure portalında**

Artık gerekli olmadığında kaynak grubunu, NAT ağ geçidini ve tüm ilgili kaynakları silin. NAT ağ geçidini içeren **Myresourcegroupnat** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunu oluşturdunuz:

* NAT ağ geçidi kaynağı
* Sanal ağ
* Ubuntu sanal makinesi

Sanal makine, NAT ağ geçidiyle ilişkili bir sanal ağ alt ağına dağıtılır. 

Sanal ağ NAT ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

* [Sanal ağ NAT 'ye genel bakış](nat-overview.md) konusunu okuyun
* [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* [Azure Resource Manager](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
