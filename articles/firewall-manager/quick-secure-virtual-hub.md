---
title: "Hızlı başlangıç: Azure Güvenlik Duvarı Yöneticisi 'Ni Kaynak Yöneticisi şablonu kullanarak güvenli sanal hub"
description: Azure Güvenlik Duvarı Yöneticisi 'Ni kullanarak sanal hub 'ınızı güvenli hale getirme hakkında bilgi edinin.
services: firewall-manager
author: vhorne
ms.service: firewall
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 6641cc29025d39ddff33e706dd9b1b0da517b884
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563699"
---
# <a name="quickstart-secure-your-virtual-hub-using-azure-firewall-manager---resource-manager-template"></a>Hızlı başlangıç: Azure Güvenlik Duvarı Yöneticisi 'Ni Kaynak Yöneticisi şablonu kullanarak sanal hub 'ınızı güvenli hale getirme

Bu hızlı başlangıçta, Azure Güvenlik Duvarı Yöneticisi 'Ni kullanarak sanal hub 'ınızı güvenli hale getirmek için bir Kaynak Yöneticisi şablonu kullanırsınız. Dağıtılan güvenlik duvarında bağlantılara izin veren bir uygulama kuralı vardır `www.microsoft.com` . Güvenlik duvarını test etmek için iki Windows Server 2019 sanal makine dağıtılır. Bir geçiş sunucusu, iş yükü sunucusuna bağlanmak için kullanılır. İş yükü sunucusundan yalnızca ' a bağlanabilirsiniz `www.microsoft.com` .

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure Güvenlik Duvarı Yöneticisi hakkında daha fazla bilgi için bkz. [Azure Güvenlik Duvarı Yöneticisi nedir?](overview.md).

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-secured-virtual-hub"></a>Güvenli sanal hub oluşturma

Bu şablon, senaryoyu desteklemek için gerekli kaynaklarla birlikte Azure Güvenlik Duvarı Yöneticisi 'ni kullanarak güvenli bir sanal hub oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/fwm-docs-qs/) alınmıştır.

:::code language="json" source="~/quickstart-templates/fwm-docs-qs/azuredeploy.json" range="001-477" highlight="47-76":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/Virtualwan 'Lar**](/azure/templates/microsoft.network/virtualWans)
- [**Microsoft. Network/Virtualhub 'Ları**](/azure/templates/microsoft.network/virtualHubs)
- [**Microsoft. Network/firewallPolicies**](/azure/templates/microsoft.network/firewallPolicies)
- [**Microsoft. Network/azureFirewalls**](/azure/templates/microsoft.network/azureFirewalls)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines)
- [**Microsoft. Storage/storageAccounts**](/azure/templates/microsoft.storage/storageAccounts)
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.network/routeTables)

### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon bir Azure Güvenlik Duvarı, sanal bir WAN ve sanal hub, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Ffwm-docs-qs%2Fazuredeploy.json)

2. Portalda, **güvenli sanal hub 'lar** sayfasında, aşağıdaki değerleri yazın veya seçin:
   - Abonelik: mevcut abonelikler arasından seçim yapın 
   - Kaynak grubu: mevcut kaynak gruplarından seçim yapın veya **Yeni oluştur**' u seçin ve **Tamam**' ı seçin.
   - Konum: bir konum seçin
   - Yönetici Kullanıcı adı: yönetici kullanıcı hesabı için Kullanıcı adı yazın 
   - Yönetici parolası: yönetici parolasını veya anahtarını yazın

3. **Gözden Geçir + oluştur**’u ve sonra da **Oluştur**’u seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Şimdi, beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı kurallarını test edin.

1. Azure portal, **Iş yükü-SRV** sanal makinesi için ağ ayarlarını gözden geçirin ve özel IP adresini aklınızda edin.
2. Uzak bir masaüstünü, **Atsrv** sanal makinesine bağlayın ve oturum açın. Buradan, **Iş yükü-SRV** özel IP adresine bir Uzak Masaüstü bağlantısı açın.

3. Internet Explorer'ı açın ve `www.microsoft.com` adresine gidin.
4. **OK**  >  Internet Explorer güvenlik uyarıları 'nda Tamam**Kapat** ' ı seçin.

   Microsoft giriş sayfasını görmeniz gerekir.

5. `www.google.com` adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Artık Güvenlik Duvarı kurallarının çalıştığını doğruladınız:

* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarıyla oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, güvenlik duvarını ve tüm ilgili kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenlik iş ortağı sağlayıcıları hakkında bilgi edinin](trusted-security-partners.md)
