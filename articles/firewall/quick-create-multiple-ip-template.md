---
title: 'Hızlı başlangıç: birden çok genel IP adresi ile Azure Güvenlik Duvarı oluşturma-Kaynak Yöneticisi şablonu'
description: Birden çok genel IP adresi olan bir Azure Güvenlik duvarı oluşturmak için Kaynak Yöneticisi şablonunu nasıl kullanacağınızı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 3d58173d239e7a9249b588ff038ea46cfedb27a3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81605201"
---
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---resource-manager-template"></a>Hızlı başlangıç: birden çok genel IP adresi ile Azure Güvenlik Duvarı oluşturma-Kaynak Yöneticisi şablonu

Bu hızlı başlangıçta, birden çok genel IP adresi ile bir Azure Güvenlik Duvarı dağıtmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

Dağıtılan güvenlik duvarında, iki Windows Server 2019 sanal makinesine RDP bağlantılarına izin veren NAT kural koleksiyonu kuralları vardır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Birden çok genel IP adresiyle Azure Güvenlik Duvarı hakkında daha fazla bilgi için, bkz. [Azure PowerShell kullanarak birden çok genel IP adresi Ile Azure Güvenlik Duvarı dağıtma](deploy-multi-public-ip-powershell.md).

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-an-azure-firewall"></a>Azure Güvenlik Duvarı oluşturma

Bu şablon, Azure Güvenlik duvarını desteklemek için gerekli kaynaklarla birlikte iki genel IP adresi ile bir Azure Güvenlik duvarı oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/fw-docs-qs/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/fw-docs-qs/azuredeploy.json" range="001-391" highlight="238-370":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)


### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon bir Azure Güvenlik Duvarı, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

2. Portalda, **birden çok IP ortak adresi olan bir Azure Güvenlik duvarı oluşturun** sayfasında, aşağıdaki değerleri yazın veya seçin:
   - Abonelik: mevcut abonelikler arasından seçim yapın 
   - Kaynak grubu: mevcut kaynak gruplarından seçim yapın veya **Yeni oluştur**' u seçin ve **Tamam**' ı seçin.
   - Konum: bir konum seçin
   - Yönetici Kullanıcı adı: yönetici kullanıcı hesabı için Kullanıcı adı yazın 
   - Yönetici parolası: yönetici parolasını veya anahtarını yazın

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** ' u seçin ve ardından **satın al**' ı seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Azure portal dağıtılan kaynakları gözden geçirin. Güvenlik Duvarı genel IP adreslerini aklınızda edin.  

Güvenlik Duvarı genel IP adreslerine bağlanmak için Uzak Masaüstü Bağlantısı kullanın. Başarılı bağlantılar, arka uç sunucularıyla bağlantıya izin veren güvenlik duvarı NAT kurallarını gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarıyla oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, güvenlik duvarını ve tüm ilgili kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)