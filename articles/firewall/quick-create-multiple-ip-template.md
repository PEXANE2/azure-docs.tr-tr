---
title: 'Hızlı başlangıç: birden çok genel IP adresi ile Azure Güvenlik Duvarı oluşturma-Kaynak Yöneticisi şablonu'
description: Birden çok genel IP adresi olan bir Azure Güvenlik duvarı oluşturmak için Azure Resource Manager şablonu (ARM şablonu) kullanmayı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 04/14/2020
ms.author: victorh
ms.openlocfilehash: 41190cd52e29b187182589f93e9d494d05943cf8
ms.sourcegitcommit: e0785ea4f2926f944ff4d65a96cee05b6dcdb792
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88705444"
---
# <a name="quickstart-create-an-azure-firewall-with-multiple-public-ip-addresses---arm-template"></a>Hızlı başlangıç: birden çok genel IP adresi-ARM şablonuyla Azure Güvenlik Duvarı oluşturma

Bu hızlı başlangıçta, birden çok genel IP adresi ile bir Azure Güvenlik Duvarı dağıtmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Dağıtılan güvenlik duvarında, iki Windows Server 2019 sanal makinesine RDP bağlantılarına izin veren NAT kural koleksiyonu kuralları vardır.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Birden çok genel IP adresiyle Azure Güvenlik Duvarı hakkında daha fazla bilgi için, bkz. [Azure PowerShell kullanarak birden çok genel IP adresi Ile Azure Güvenlik Duvarı dağıtma](deploy-multi-public-ip-powershell.md).

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffw-docs-qs%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu şablon, Azure Güvenlik duvarını desteklemek için gerekli kaynaklarla birlikte iki genel IP adresi ile bir Azure Güvenlik duvarı oluşturur.

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/fw-docs-qs) alınmıştır.

:::code language="json" source="~/quickstart-templates/fw-docs-qs/azuredeploy.json":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)

## <a name="deploy-the-template"></a>Şablonu dağıtma

ARM şablonunu Azure 'a dağıtma:

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