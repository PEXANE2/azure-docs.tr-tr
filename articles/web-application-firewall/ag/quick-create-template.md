---
title: "Hızlı başlangıç: Uygulama Ağ Geçidi'nde Azure WAF v2 oluşturma - Kaynak Yöneticisi şablonu"
titleSuffix: Azure Application Gateway
description: Azure Uygulama Ağ Geçidi'nde Bir Web Uygulaması Güvenlik Duvarı v2 oluşturmak için Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: quickstart
ms.date: 04/02/2020
ms.author: victorh
ms.openlocfilehash: 6759071e73adfd3af4ac780da6db3a0e6e967ea1
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617986"
---
# <a name="quickstart-create-an-azure-waf-v2-on-application-gateway---resource-manager-template"></a>Hızlı başlangıç: Uygulama Ağ Geçidi'nde Azure WAF v2 oluşturma - Kaynak Yöneticisi şablonu

Bu hızlı başlatmada, Uygulama Ağ Geçidi'nde Bir Azure Web Uygulaması Güvenlik Duvarı v2 oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="create-a-web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı Oluşturma

Bu şablon, Azure Uygulama Ağ Geçidi'nde basit bir Web Uygulaması Güvenlik Duvarı v2 oluşturur. Buna halka açık bir IP frontend IP adresi, HTTP ayarları, bağlantı noktası 80'de temel dinleyiciiçeren bir kural ve bir arka uç havuzu dahildir. Bir IP adresi eşleşme türüne bağlı olarak arka uç havuzuna gelen trafiği engellemek için özel bir kurala sahip bir WAF ilkesi oluşturulur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarından](https://github.com/Azure/azure-quickstart-templates/blob/master/ag-docs-wafv2/azuredeploy.json)

:::code language="json" source="~/quickstart-templates/ag-docs-wafv2/azuredeploy.json" range="001-404" highlight="314-358":::

Şablonda birden çok Azure kaynağı tanımlanır:

- [**Microsoft.Network/applicationgateways**](/azure/templates/microsoft.network/applicationgateways)
- [**Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies**](/azure/templates/microsoft.network/ApplicationGatewayWebApplicationFirewallPolicies)
- [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses) : uygulama ağ geçidi için bir ve sanal makineler için iki.
- [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups)
- [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks)
- [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines) : iki sanal makine
- [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) : sanal makineler için iki
- [**Microsoft.Compute/virtualMachine/extensions**](/azure/templates/microsoft.compute/virtualmachines/extensions) : IIS ve web sayfalarını yapılandırmak için

### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure'a dağıtın:

1. Azure'da oturum açmak ve şablonu açmak için **Azure'a Dağıt'ı** seçin. Şablon, IIS çalıştıran arka uç havuzunda bir uygulama ağ geçidi, ağ altyapısı ve iki sanal makine oluşturur.

   [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fag-docs-wafv2%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun.
3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** seçin ve ardından Satın **Alma'yı**seçin. Dağıtımın tamamlanması 10 dakika veya daha uzun sürebilir.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Uygulama ağ geçidini oluşturmak için IIS gerekmese de, Azure'un uygulama ağ geçidinde başarılı bir WAF v2 oluşturmuş olup olmadığını doğrulamak için arka uç sunucularına yüklenir. 

Uygulama ağ geçidini test etmek için IIS'yi kullanın:

1. **Genel Bakış** sayfasında uygulama ağ geçidinin genel IP adresini bulun. ![Kayıt uygulama ağ](../../application-gateway/media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png) geçidi ortak IP adresi Veya, **Tüm kaynakları**seçebilir, arama kutusuna *myAGPublicIPAddress* girin ve sonra arama sonuçlarında seçin. Azure, **Genel Bakış** sayfasında genel IP adresini görüntüler.
2. Herkese açık IP adresini kopyalayın ve bu IP adresine göz atmak için tarayıcınızın adres çubuğuna yapıştırın.
3. Yanıtı kontrol edin. **403 Yasak** yanıt, WAF'ın başarıyla oluşturulduğunu ve arka uç havuzuna bağlantıları engellediğini doğrular.
4. Özel kuralı **trafiğe izin vermek**için değiştirin.
  Kaynak grup adınızı değiştirmek için aşağıdaki Azure PowerShell komut dosyasını çalıştırın:
   ```azurepowershell
   $rg = "<your resource group name>"
   $AppGW = Get-AzApplicationGateway -Name myAppGateway -ResourceGroupName $rg
   $pol = Get-AzApplicationGatewayFirewallPolicy -Name WafPol01 -ResourceGroupName $rg
   $pol[0].customrules[0].action = "allow"
   $rule = $pol.CustomRules
   Set-AzApplicationGatewayFirewallPolicy -Name WafPol01 -ResourceGroupName $rg -CustomRule $rule
   $AppGW.FirewallPolicy = $pol
   Set-AzApplicationGateway -ApplicationGateway $AppGW
   ```

   Tarayıcınızı birden çok kez yenileyin ve hem myVM1 hem de myVM2 bağlantılarını görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidinde oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Bu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet'i arayın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name "<your resource group name>"
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure portalını kullanarak Web Uygulaması Güvenlik Duvarı ile bir uygulama ağ geçidi oluşturma](application-gateway-web-application-firewall-portal.md)