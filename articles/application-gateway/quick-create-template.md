---
title: 'Hızlı başlatma: Kaynak Yöneticisi şablonu kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Uygulama Ağ Geçidi oluşturmak için Kaynak Yöneticisi şablonunun nasıl kullanılacağını öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/23/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: cce3ef20a93c6d7a24bfa312501d2f8cc8ed9273
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604894"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---resource-manager-template"></a>Hızlı başlangıç: Azure Application Gateway ile doğrudan web trafiği - Kaynak Yöneticisi şablonu

Bu hızlı başlangıçta, Azure Uygulama Ağ Geçidi oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız. Daha sonra doğru çalıştığından emin olmak için uygulama ağ geçidini sınarsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlatmayı Azure [portalı](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md)veya [Azure CLI'yi](quick-create-cli.md)kullanarak da tamamlayabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Basitlik adına, bu şablon, genel bir ön uç IP'si, uygulama ağ geçidinde tek bir siteyi barındıracak temel bir dinleyici, temel bir istek yönlendirme kuralı ve arka uç havuzunda iki sanal makine içeren basit bir kurulum oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/ag-docs-qs/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/ag-docs-qs/azuredeploy.json" range="001-343" highlight="197-297":::

Şablonda birden çok Azure kaynağı tanımlanır:

- [**Microsoft.Network/applicationgateways**](/azure/templates/microsoft.network/applicationgateways)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses) : uygulama ağ geçidi için bir ve sanal makineler için iki.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines) : iki sanal makine
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) : sanal makineler için iki
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) : IIS ve web sayfalarını yapılandırmak için


### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure'a dağıtın:

1. Azure'da oturum açmak ve şablonu açmak için **Azure'a Dağıt'ı** seçin. Şablon, IIS çalıştıran arka uç havuzunda bir uygulama ağ geçidi, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-qs%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun, sanal makine yöneticisi kullanıcı adını ve parolasını yazın.
3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** seçin ve ardından Satın **Alma'yı**seçin. Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Uygulama ağ geçidini oluşturmak için IIS gerekmese de, Azure'un uygulama ağ geçidini başarıyla oluşturmuş olup olmadığını doğrulamak için yüklenir. Uygulama ağ geçidini test etmek için IIS'yi kullanın:

1. **Genel Bakış** sayfasında uygulama ağ geçidinin genel IP adresini bulun. ![Kayıt uygulama ağ](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png) geçidi ortak IP adresi Veya, **Tüm kaynakları**seçebilir, arama kutusuna *myAGPublicIPAddress* girin ve sonra arama sonuçlarında seçin. Azure, **Genel Bakış** sayfasında genel IP adresini görüntüler.
2. Herkese açık IP adresini kopyalayın ve bu IP adresine göz atmak için tarayıcınızın adres çubuğuna yapıştırın.
3. Yanıtı kontrol edin. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uçla başarılı bir şekilde bağlanabileceğini doğrular.

   ![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

   Tarayıcıyı birden çok kez yenileyin ve hem myVM1 hem de myVM2 bağlantılarını görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidinde oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Bu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet'i arayın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
