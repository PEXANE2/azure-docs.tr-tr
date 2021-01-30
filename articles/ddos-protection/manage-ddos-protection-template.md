---
title: Azure Resource Manager şablonu kullanarak bir Azure DDoS koruma planı oluşturun ve etkinleştirin (ARM şablonu).
description: Azure Resource Manager şablonu kullanarak Azure DDoS koruma planı oluşturma ve etkinleştirme hakkında bilgi edinin (ARM şablonu).
services: ddos-protection
documentationcenter: na
author: mumian
ms.service: ddos-protection
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.custom: subject-armqs
ms.author: jgao
ms.date: 01/14/2021
ms.openlocfilehash: 75d6c484a0f1d6325aaa7894d8902ff78cadbd74
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99092509"
---
# <a name="quickstart-create-an-azure-ddos-protection-standard-using-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak Azure DDoS koruma standardı oluşturma

Bu hızlı başlangıçta Dağıtılmış bir hizmet reddi (DDoS) koruma planı ve sanal ağ (VNet) oluşturmak için bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı açıklanır ve ardından VNet için koruma planı etkinleştirilir. Azure DDoS koruması standart planı, abonelikler arasında DDoS koruması etkinleştirilmiş bir sanal ağ kümesini tanımlar. Kuruluşunuz için bir DDoS koruma planı yapılandırabilir ve sanal ağları birden çok abonelikten aynı plana bağlayabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-create-and-enable-ddos-protection-plans%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-create-and-enable-ddos-protection-plans) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-create-and-enable-ddos-protection-plans/azuredeploy.json":::

Şablon iki kaynağı tanımlar:

- [Microsoft. Network/Ddosprotectionplanlar](/azure/templates/microsoft.network/ddosprotectionplans)
- [Microsoft. Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bu örnekte, şablon yeni bir kaynak grubu, DDoS koruma planı ve sanal ağ oluşturur.

1. Azure 'da oturum açmak ve şablonu açmak için **Azure 'A dağıt** düğmesini seçin.

    [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-create-and-enable-ddos-protection-plans%2Fazuredeploy.json)

1. Yeni bir kaynak grubu, DDoS koruma planı ve VNet adı oluşturmak için değerleri girin.

    :::image type="content" source="media/manage-ddos-protection-template/ddos-template.png" alt-text="DDoS hızlı başlangıç şablonu.":::

    - **Abonelik**: kaynakların dağıtılacağı Azure aboneliğinin adı.
    - **Kaynak grubu**: var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturun.
    - **Bölge**: kaynak grubunun dağıtıldığı bölge, örneğin Doğu ABD.
    - **DDoS koruma planı adı**: yeni DDoS koruma planının adı.
    - **Sanal ağ adı**: yeni VNET için bir ad oluşturur.
    - **Konum**: kaynak dağıtımı için kaynak grubuyla aynı bölgeyi kullanan işlev.
    - **VNET adresi ön eki**: varsayılan değeri kullanın veya VNET adresinizi girin.
    - **Alt ağ ön eki**: varsayılan değeri kullanın veya VNET alt ağını girin.
    - **DDoS koruma planı etkin**: varsayılan `true` DDoS koruma planını etkinleştirmektir.

1. **Gözden geçir ve oluştur**’u seçin.
1. Şablon doğrulamanın başarılı olduğunu doğrulayın ve dağıtımı başlatmak için **Oluştur** ' u seçin.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure CLı veya Azure PowerShell komutunu kopyalamak için **Kopyala** düğmesini seçin. Komutu çalıştırmak için Azure Cloud Shell **deneyin** düğmesi açılır.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az network ddos-protection show \
  --resource-group MyResourceGroup \
  --name MyDdosProtectionPlan
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzDdosProtectionPlan -ResourceGroupName 'MyResourceGroup' -Name 'MyDdosProtectionPlan'
```

---

Çıktı yeni kaynakları gösterir.

# <a name="cli"></a>[CLI](#tab/CLI)

```Output
{
  "etag": "W/\"abcdefgh-1111-2222-bbbb-987654321098\"",
  "id": "/subscriptions/b1111111-2222-3333-aaaa-012345678912/resourceGroups/MyResourceGroup/providers/Microsoft.Network/ddosProtectionPlans/MyDdosProtectionPlan",
  "location": "eastus",
  "name": "MyDdosProtectionPlan",
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "resourceGuid": null,
  "tags": null,
  "type": "Microsoft.Network/ddosProtectionPlans",
  "virtualNetworks": [
    {
      "id": "/subscriptions/b1111111-2222-3333-aaaa-012345678912/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet",
      "resourceGroup": "MyResourceGroup"
    }
  ]
}
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```Output
Name              : MyDdosProtectionPlan
Id                : /subscriptions/b1111111-2222-3333-aaaa-012345678912/resourceGroups/MyResourceGroup/providers/Microsoft.Network/ddosProtectionPlans/MyDdosProtectionPlan
Etag              : W/"abcdefgh-1111-2222-bbbb-987654321098"
ProvisioningState : Succeeded
VirtualNetworks   : [
                      {
                        "Id": "/subscriptions/b1111111-2222-3333-aaaa-012345678912/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet"
                      }
                    ]
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde kaynakları silebilirsiniz. Komut, kaynak grubunu ve içerdiği tüm kaynakları siler.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az group delete --name MyResourceGroup
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Remove-AzResourceGroup -Name 'MyResourceGroup'
```

---

## <a name="next-steps"></a>Sonraki adımlar

DDoS koruma planınız için telemetri görüntüleme ve yapılandırma hakkında bilgi edinmek için öğreticilere devam edin.

> [!div class="nextstepaction"]
> [DDoS konuma telemetrisini görüntüleme ve yapılandırma](telemetry.md)
