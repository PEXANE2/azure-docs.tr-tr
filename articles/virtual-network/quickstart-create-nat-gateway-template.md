---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma - Kaynak Yöneticisi Şablonu'
titleSuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıç, Azure Kaynak Yöneticisi şablonu kullanılarak nasıl bir NAT ağ geçidi oluşturulacak gösteriş gösterir.
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
ms.openlocfilehash: 6b3dee698e63ed2b0a44f05b593b8840d1cd23ee
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80066411"
---
# <a name="quickstart-create-a-nat-gateway---resource-manager-template"></a>Hızlı başlangıç: NAT ağ geçidi oluşturma - Kaynak Yöneticisi şablonu

Azure Kaynak Yöneticisi Şablonu kullanarak Sanal Ağ NAT ile başlayın.  Bu şablon, sanal ağ, bir NAT ağ geçidi kaynağı ve Ubuntu sanal makine dağıtır. Ubuntu sanal makine NAT ağ geçidi kaynağı ile ilişkili bir alt net dağıtılır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-nat-gateway-and-supporting-resources"></a>NAT ağ geçidi oluşturma ve kaynakları destekleme

Bu şablon, bir 

* Sanal ağ 
* NAT ağ geçidi kaynağı
* Ubuntu sanal makine

Ubuntu VM, NAT ağ geçidi kaynağıyla ilişkili bir alt ağa dağıtılır.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarından](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-nat-gateway-1-vm/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-nat-gateway-1-vm/azuredeploy.json" range="1-335" highlight="256-282":::

Şablonda dokuz Azure kaynağı tanımlanır:

**Microsoft.Network**

* **[Microsoft.Network/natGateways](https://docs.microsoft.com/azure/templates/microsoft.network/natgateways)**: NAT ağ geçidi kaynağı oluşturur.

* **[Microsoft.Network/networkSecurityGroups](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups)**: Ağ güvenlik grubu oluşturur.

    * **[Microsoft.Network/networkSecurityGroups/securityRules](https://docs.microsoft.com/azure/templates/microsoft.network/networksecuritygroups/securityrules)**: Bir güvenlik kuralı oluşturur.

* **[Microsoft.Network/publicIPAddresses](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)**: Herkese açık bir IP adresi oluşturur.

* **[Microsoft.Network/publicIPPrefixes](https://docs.microsoft.com/azure/templates/microsoft.network/publicipprefixes)**: Genel IP öneki oluşturur.

* **[Microsoft.Network/virtualNetworks](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)**: Sanal ağ oluşturur.

    * **[Microsoft.Network/virtualNetworks/subnets](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks/subnets)**: Sanal ağ alt ağı oluşturur.

* **[Microsoft.Network/networkinterfaces](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)**: Ağ arabirimi oluşturur.

**Microsoft.Compute**

* **[Microsoft.Compute/virtualMachines](https://docs.microsoft.com/azure/templates/Microsoft.Compute/virtualMachines)**: Sanal bir makine oluşturur.

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

[![Azure'a Dağıt](./media/quick-create-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-nat-gateway-1-vm%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Sol bölmeden **Kaynak gruplarını** seçin.

3. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grup adı **myResourceGroupNAT**

4. Kaynak grubunda aşağıdaki kaynakların oluşturulduğunu doğrulayın:

    ![Sanal Ağ NAT kaynak grubu](./media/quick-create-template/nat-gateway-template-rg.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Azure CLI**

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme](/cli/azure/group#az-group-delete) komutunu kullanabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupNAT
```

**Azure PowerShell**

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

**Azure portalında**

Artık gerekmediğinde, kaynak grubunu, NAT ağ geçidini ve ilgili tüm kaynakları silin. NAT ağ geçidini içeren **myResourceGroupNAT** kaynak grubunu seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, şunları oluşturdunuz:

* NAT ağ geçidi kaynağı
* Sanal ağ
* Ubuntu sanal makine

Sanal makine, NAT ağ geçidiyle ilişkili bir sanal ağ alt ağına dağıtılır. 

Sanal Ağ NAT ve Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

* Sanal [Ağ NAT'ye Genel Bakış](nat-overview.md)
* [NAT Ağ Geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* [Azure Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
