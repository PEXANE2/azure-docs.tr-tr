---
title: 'Hızlı başlangıç: Azure Güvenlik Duvarı ve IP Grupları Oluşturma - Kaynak Yöneticisi şablonu'
description: Azure Güvenlik Duvarı ve IP Grupları oluşturmak için Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.date: 04/06/2020
ms.author: victorh
ms.openlocfilehash: 767e91af7fb5ff3de517a79ac4df55af5057c76f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679838"
---
# <a name="quickstart-create-an-azure-firewall-and-ip-groups---resource-manager-template"></a>Hızlı başlangıç: Azure Güvenlik Duvarı ve IP Grupları Oluşturma - Kaynak Yöneticisi şablonu

Bu hızlı başlatmada, ağ kuralı ve uygulama kuralında kullanılan örnek IP Gruplarıyla birlikte bir Azure Güvenlik Duvarı dağıtmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

IP Grubu, IP adreslerini, aralıklarını ve alt ağları tek bir nesnede tanımlamanızı ve gruplandırmanızı sağlayan üst düzey bir kaynaktır. Bu, Azure Güvenlik Duvarı kurallarında IP adreslerini yönetmek için yararlıdır. IP adreslerini el ile girebilir veya bir dosyadan içe aktarabilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="create-an-azure-firewall-and-ip-groups"></a>Azure Güvenlik Duvarı ve IP Grupları Oluşturma

Bu şablon, Azure Güvenlik Duvarı'nı desteklemek için gerekli kaynakların yanı sıra bir Azure Güvenlik Duvarı ve IP Grupları oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-azurefirewall-create-with-ipgroups-and-linux-jumpbox/azuredeploy.json" range="001-512" highlight="118-141":::

Şablonda birden çok Azure kaynağı tanımlanır:

- [**Microsoft.Network/ipGroups**](/azure/templates/microsoft.network/ipGroups)
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

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurefirewall-create-with-ipgroups-and-linux-jumpbox%2Fazuredeploy.json"><img src="./media/quick-create-ipgroup-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Portalda, **IpGroups sayfasıyla bir Azure Güvenlik Duvarı Oluştur** sayfasında aşağıdaki değerleri yazın veya seçin:
   - Abonelik: Mevcut aboneliklerden seçim 
   - Kaynak grubu: Varolan kaynak gruplarından seçin veya **yeni oluştur'u**seçin ve **Tamam'ı**seçin.
   - Konum: Konum seçin
   - Sanal Ağ Adı: Yeni sanal ağ (VNet) için bir ad yazın 
   - IP Grup Adı 1: IP Grubu 1'in türü adı 
   - IP Grup Adı 2: IP Grubu 2'nin türü adı 
   - Yönetici Kullanıcı Adı: Yönetici kullanıcı hesabı için kullanıcı adı yazın 
   - Kimlik doğrulama: sshPublicKey veya şifre seçin 
   - Yönetici Şifresi: Yönetici parolası veya anahtar yazın

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** seçin ve ardından Satın **Alma'yı**seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Azure portalında, dağıtılan kaynakları, özellikle IP Grupları'nı kullanan güvenlik duvarı kurallarını gözden geçirin.

:::image type="content" source="media/quick-create-ipgroup-template/ipgroups.png" alt-text="IP Grupları.":::

:::image type="content" source="media/quick-create-ipgroup-template/network-rule.png" alt-text="Ağ kuralları.":::

Şablondaki bir güvenlik duvarının JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Microsoft.Network azureFirewalls şablonu başvurusuna](https://docs.microsoft.com/azure/templates/Microsoft.Network/2019-11-01/azureFirewalls)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarıyla oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Bu, güvenlik duvarını ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet'i arayın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını karma ağda dağıtma ve yapılandırma](tutorial-hybrid-portal.md)