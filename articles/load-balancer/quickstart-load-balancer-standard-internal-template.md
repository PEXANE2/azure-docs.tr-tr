---
title: Azure Resource Manager şablonu kullanarak iç yük dengeleyici oluşturma (ARM şablonu)
description: Azure Resource Manager şablonu kullanarak iç Azure yük dengeleyici oluşturmayı öğrenin (ARM şablonu).
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: allensu
ms.date: 09/14/2020
ms.openlocfilehash: 287afc51aa15ed4cadba7e2d6cd389e4869d7d61
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532867"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-by-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak VM 'Lerin yükünü dengelemek için iç yük dengeleyici oluşturma

Bu hızlı başlangıçta, iç Azure yük dengeleyici oluşturmak için bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon, [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/201-2-vms-internal-load-balancer).

:::code language="json" source="~/quickstart-templates/201-2-vms-internal-load-balancer/azuredeploy.json":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Storage/storageaccounts**](/azure/templates/microsoft.storage/storageaccounts): önyükleme tanılaması için sanal makine depolama hesapları.
- [**Microsoft. COMPUTE/Kullanılabilirbilitysets**](/azure/templates/microsoft.compute/availabilitySets): sanal makineler için kullanılabilirlik kümesi.
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualNetworks): yük dengeleyici ve sanal makineler için sanal ağ.
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkInterfaces): sanal makineler için ağ arabirimleri.
- [**Microsoft. Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): iç yük dengeleyici.
- [**Microsoft. COMPUTE/virtualmachines**](/azure/templates/microsoft.compute/virtualMachines): sanal makineler.

Azure Load Balancer ilgili daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Şablonu dağıtma

**Azure CLI**

```azurecli-interactive
read -p "Enter the location (i.e. westcentralus): " location
resourceGroupName="myResourceGroupLB"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json" 

az group create \
--name $resourceGroupName \
--location $location

az deployment group create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol bölmeden **kaynak grupları** ' nı seçin.

3. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı **Myresourcegrouplb**

4. Kaynak grubunda aşağıdaki kaynakların oluşturulduğunu doğrulayın:

:::image type="content" source="media/quickstart-load-balancer-standard-internal-template/verify-creation.png" alt-text="Kaynak oluşturulmasını doğrulamak için Kullanıcı Azure portal." border="true":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive 
  az group delete \
    --name myResourceGroupLB
```

## <a name="next-steps"></a>Sonraki adımlar

Şablon oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için, bkz.:

> [!div class="nextstepaction"]
> [Öğretici: ilk ARM şablonunuzu oluşturma ve dağıtma](/azure/azure-resource-manager/templates/template-tutorial-create-first-template)