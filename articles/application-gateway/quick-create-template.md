---
title: 'Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Bir arka uç havuzundaki sanal makinelere Web trafiğini yönlendiren bir Azure Application Gateway oluşturmak için Kaynak Yöneticisi şablonunu nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 05/28/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 963f728db55f587f7ae72aec702a099882cf401a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86249385"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---arm-template"></a>Hızlı başlangıç: Azure Application Gateway ARM şablonuyla doğrudan web trafiği

Bu hızlı başlangıçta, bir Azure Application Gateway oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız. Ardından, doğru çalıştığından emin olmak için uygulama ağ geçidini test edersiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](quick-create-portal.md), [Azure POWERSHELL](quick-create-powershell.md)veya [Azure CLI](quick-create-cli.md)kullanarak da tamamlayabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure 'A dağıt** düğmesini seçin. Şablon Azure portal açılır.

[![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-qs%2Fazuredeploy.json)

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Kolaylık sağlaması için bu şablon, genel ön uç IP 'si olan basit bir kurulum ve uygulama ağ geçidinde tek bir siteyi barındırmak için temel bir dinleyici, temel bir istek yönlendirme kuralı ve arka uç havuzundaki iki sanal makine oluşturur.

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/ag-docs-qs/)

:::code language="json" source="~/quickstart-templates/ag-docs-qs/azuredeploy.json" range="001-343" highlight="197-297":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/applicationgateway 'ler**](/azure/templates/microsoft.network/applicationgateways)
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses) : uygulama ağ geçidi için bir tane ve sanal makineler için iki.
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft. COMPUTE/virtualmachines**](/azure/templates/microsoft.compute/virtualmachines) : iki sanal makine
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) : sanal makineler için iki
- [**Microsoft. COMPUTE/virtualMachine/Extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) : IIS ve Web sayfalarını yapılandırmak için

## <a name="deploy-the-template"></a>Şablonu dağıtma

ARM şablonunu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon, IIS çalıştıran arka uç havuzunda bir uygulama ağ geçidi, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-qs%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun, sanal makine yöneticisinin Kullanıcı adını ve parolasını yazın.
3. **Gözden geçir + oluştur** ' u ve ardından **Oluştur**' u seçin.

   Dağıtımın tamamlanması 20 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Uygulama ağ geçidi oluşturmak için IIS gerekli olmasa da, Azure 'ın uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak üzere yüklenir. Uygulama ağ geçidini test etmek için IIS kullanın:

1. Uygulama ağ geçidinin genel IP adresini **genel bakış** sayfasında bulabilirsiniz. ![ Uygulama Ağ Geçidi genel IP adresini kaydedin ](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png) veya **tüm kaynaklar**' ı seçip Arama kutusuna *Myagpublicıpaddress* girebilir ve arama sonuçlarında bunu seçebilirsiniz. Azure **genel bakış** SAYFASıNDA genel IP adresini görüntüler.
2. Genel IP adresini kopyalayın ve ardından bu IP adresine gözatabilmek için tarayıcınızın adres çubuğuna yapıştırın.
3. Yanıtı denetleyin. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uca başarıyla bağlanıp bağlanabileceğini doğrular.

   ![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

   Tarayıcıyı birden çok kez yenileyin ve hem myVM1 hem de myVM2 bağlantıları görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, uygulama ağ geçidini ve tüm ilgili kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
