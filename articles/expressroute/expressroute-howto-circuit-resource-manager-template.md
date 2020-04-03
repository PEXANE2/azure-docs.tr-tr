---
title: 'Azure ExpressRoute Şablonu: ExpressRoute devresi oluşturma'
description: Bir ExpressRoute devresi oluşturun, sağlama, silme ve silme.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 11/13/2019
ms.author: charwen
ms.openlocfilehash: ac2fb2f884feb0fbe6fbb1731ebe7c8bdad5114b
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616522"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak ExpressRoute devresi oluşturma

> [!div class="op_single_selector"]
> * [Azure portalı](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Azure PowerShell'i kullanarak bir Azure Kaynak Yöneticisi şablonu dağıtarak ExpressRoute devresi oluşturmayı öğrenin. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için [Kaynak Yöneticisi belgelerine](/azure/azure-resource-manager/) ve [şablon başvurusuna](/azure/templates/microsoft.network/expressroutecircuits)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.
* Yeni ağ kaynakları oluşturmak için izinlere sahip olduğundan emin olun. Doğru izinlere sahip değilseniz hesap yöneticinize başvurun.
* Adımları daha iyi anlamak için videoyu başlamadan önce [görüntüleyebilirsiniz.](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute devresi oluşturma ve sağlama

[Azure Quickstart şablonları,](https://azure.microsoft.com/resources/templates/) iyi bir Kaynak Yöneticisi şablonu koleksiyonuna sahiptir. Bir ExpressRoute [devresi](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) oluşturmak için varolan şablonlardan birini kullanırsınız.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/101-expressroute-circuit-create/azuredeploy.json)]

Daha fazla ilgili şablongörmek için [burayı](https://azure.microsoft.com/resources/templates/?term=expressroute)seçin.

Bir şablon dağıtarak bir ExpressRoute Devresi oluşturmak için:

1. Aşağıdaki kod bloğundan **deneyin'i** seçin ve ardından Azure Bulutu kabuğunda oturum açma yönergelerini izleyin.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * **SKU katmanı,** ExpressRoute devresi [Yerel,](expressroute-faqs.md#expressroute-local)Standart veya [Premium](expressroute-faqs.md#expressroute-premium)olup olmadığını belirler. *Yerel*, *Standart veya *Premium*belirtebilirsiniz.
   * Fatura türünü **SKU ailesi** belirler. Tarifeli veri planı için *Ölçülü Verileri* ve sınırsız veri planı için Sınırsız *Veri* belirtebilirsiniz. Fatura türünü *Metereddata'dan* *Unlimiteddata'ya*değiştirebilirsiniz, ancak türü *Unlimiteddata'dan* *Metereddata'ya*değiştiremezsiniz. *Yerel* devre yalnızca *Sınırsız veridir.*
   * **Eşleyen Konum,** Microsoft ile birlikte bakacağınız fiziksel konumdur.

     > [!IMPORTANT]
     > Eşleyen Konum, Microsoft ile birlikte bakmakta olduğunuz [fiziksel konumu](expressroute-locations.md) gösterir. Bu, Azure Ağ Kaynak Sağlayıcısı'nın bulunduğu coğrafyayı ifade eden "Konum" özelliğiyle bağlantılı **değildir.** Bunlar ilişkili olmasa da, coğrafi olarak devrenin Eşleyen Konumu'na yakın bir Ağ Kaynak Sağlayıcısı seçmek iyi bir uygulamadır.

    Kaynak grubu adı **rg** eklenen servis veri günü ad alanı adıdır.

2. PowerShell komut dosyasını kopyalamak için **Kopyala'yı** seçin.
3. Kabuk konsoluna sağ tıklayın ve ardından **Yapıştır'ı**seçin.

Bir olay merkezi oluşturmak birkaç dakika sürer.

Azure PowerShell bu öğreticide şablonu dağıtmak için kullanılır. Diğer şablon dağıtım yöntemleri için bkz:

* [Azure portalını kullanarak](../azure-resource-manager/templates/deploy-portal.md).
* [Azure CLI'yi kullanarak.](../azure-resource-manager/templates/deploy-cli.md)
* [REST API kullanarak](../azure-resource-manager/templates/deploy-rest.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme

**Delete** simgesini seçerek ExpressRoute devrenizi silebilirsiniz. Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, herhangi bir sanal ağların devreye bağlı olup olmadığını kontrol edin.
* ExpressRoute devre servis sağlayıcısı sağlama durumu **Hükmünde** veya **Provisioned** ise, devreyi kendi taraflarında devreyi devreyi devreyi devreden çıkarmak için servis sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcı devreyi devreyi devreyi tamamlayıp bizi bize iletene kadar kaynak ayırmaya ve size fatura landırmaya devam ediyoruz.
* Hizmet sağlayıcısı devreyi devreyi devreyi devreyi iptal ettiyse (servis sağlayıcı sağlama durumu **değil**olarak ayarlanır), devreyi silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

ExpressRoute devrenizi aşağıdaki PowerShell komutunu çalıştırarak silebilirsiniz:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra aşağıdaki sonraki adımlarla devam edin:

* [ExpressRoute devreniz için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-arm.md)
