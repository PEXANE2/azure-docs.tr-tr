---
title: 'Hızlı başlangıç: Birden çok genel IP adresiyle bir Azure Güvenlik Duvarı oluşturma - Kaynak Yöneticisi şablonu'
description: Birden çok genel IP adresine sahip bir Azure Güvenlik Duvarı oluşturmak için Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 76211014f96aee864412fd19686d58d0338ec5c2
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310249"
---
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---resource-manager-template"></a>Hızlı başlangıç: Birden çok genel IP adresiyle bir Azure Güvenlik Duvarı oluşturma - Kaynak Yöneticisi şablonu

Bu hızlı başlatmada, birden çok genel IP adresine sahip bir Azure Güvenlik Duvarı dağıtmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

Dağıtılan güvenlik duvarı, iki Windows Server 2019 sanal makinesine RDP bağlantısı sağlayan NAT kural toplama kurallarına sahiptir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Birden çok genel IP adresine sahip Azure Güvenlik Duvarı hakkında daha fazla bilgi [için](deploy-multi-public-ip-powershell.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="create-an-azure-firewall"></a>Azure Güvenlik Duvarı Oluşturma

Bu şablon, Azure Güvenlik Duvarı'nı desteklemek için gerekli kaynakların yanı sıra iki genel IP adresine sahip bir Azure Güvenlik Duvarı oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/fw-docs-qs/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/fw-docs-qs/azuredeploy.json" range="001-391" highlight="238-370":::

Şablonda birden çok Azure kaynağı tanımlanır:

- [**Microsoft.Network/publicIPAdresleri**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft.Storage/storageHesapları**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft.Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft.Network/routeTables**](/azure/templates/microsoft.network/routeTables)


### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure'a dağıtın:

1. Azure'da oturum açmak ve şablonu açmak için **Azure'a Dağıt'ı** seçin. Şablon bir Azure Güvenlik Duvarı, ağ altyapısı ve iki sanal makine oluşturur.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json"><img src="./media/quick-create-multiple-ip-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Portalda, birden **çok IP ortak adresi olan bir Azure Güvenlik Duvarı Oluştur'da** aşağıdaki değerleri yazın veya seçin:
   - Abonelik: Mevcut aboneliklerden seçim 
   - Kaynak grubu: Varolan kaynak gruplarından seçin veya **yeni oluştur'u**seçin ve **Tamam'ı**seçin.
   - Konum: Konum seçin
   - Yönetici Kullanıcı Adı: Yönetici kullanıcı hesabı için kullanıcı adı yazın 
   - Yönetici Şifresi: Yönetici parolası veya anahtar yazın

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** seçin ve ardından Satın **Alma'yı**seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Azure portalında dağıtılan kaynakları gözden geçirin. Güvenlik duvarı genel IP adreslerine dikkat edin.  

Güvenlik duvarı genel IP adreslerine bağlanmak için Uzak Masaüstü Bağlantısı'nı kullanın. Başarılı bağlantılar, arka uç sunucularına bağlantı sağlayan güvenlik duvarı NAT kurallarını gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarıyla oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Bu, güvenlik duvarını ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet'i arayın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)