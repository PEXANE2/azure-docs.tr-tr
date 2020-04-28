---
title: 'Azure ExpressRoute: bir devreyi değiştirme: PowerShell'
description: ExpressRoute bağlantı hattı oluşturma, sağlama, doğrulama, güncelleştirme, silme ve sağlamayı kaldırma.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 01/08/2020
ms.author: cherylmc
ms.openlocfilehash: ab44d5358154c17622eef68205ac2326e81377a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75770977"
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>PowerShell kullanarak ExpressRoute bağlantı hattını oluşturma ve değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makale, PowerShell cmdlet 'lerini ve Azure Resource Manager dağıtım modelini kullanarak bir ExpressRoute bağlantı hattı oluşturmanıza yardımcı olur. Ayrıca, durumu denetleyebilir, güncelleştirebilir, silebilir veya devre sağlamayı kaldırabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce, yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

### <a name="working-with-azure-powershell"></a>Azure PowerShell çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute bağlantı hattı oluşturma ve sağlama
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. desteklenen sağlayıcıların, konumların ve bant genişliklerinin listesini alın
ExpressRoute bağlantı hattı oluşturmadan önce, desteklenen bağlantı sağlayıcıları, konumlar ve bant genişliği seçeneklerinin listesine ihtiyacınız vardır.

**Get-AzExpressRouteServiceProvider** PowerShell cmdlet 'i, sonraki adımlarda kullanacağınız bu bilgileri döndürür:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Bağlantı sağlayıcınızın orada listelenip listelenmediğini denetleyin. Daha sonra bir devre oluşturduğunuzda ihtiyacınız olan aşağıdaki bilgileri unutmayın:

* Adı
* PeeringLocations
* BandwidthsOffered

Artık bir ExpressRoute devresi oluşturmaya hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. bir ExpressRoute bağlantı hattı oluşturma
Henüz bir kaynak grubunuz yoksa, ExpressRoute bağlantı hattını oluşturmadan önce bir tane oluşturmanız gerekir. Şu komutu çalıştırarak bunu yapabilirsiniz:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

Aşağıdaki örnek, bir 200 Mbps ExpressRoute bağlantı hattının, Silicon Valley içinde Equinx aracılığıyla nasıl oluşturulacağını göstermektedir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız isteğinizi yaparken bu bilgileri yerine koyun. Yeni bir hizmet anahtarı istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Doğru SKU katmanını ve SKU ailesini belirttiğinizden emin olun:

* SKU katmanı, bir ExpressRoute devresinin [Yerel](expressroute-faqs.md#expressroute-local), standart veya [Premium](expressroute-faqs.md#expressroute-premium)olup olmadığını belirler. *Yerel*, *Standart* veya *Premium*belirtebilirsiniz.
* SKU ailesi Faturalandırma türünü belirler. Tarifeli veri planı için *Metereddata* , sınırsız bir veri planı Için de *unlimiteddata* belirtebilirsiniz. Faturalandırma türünü *Metereddata* 'Den *unlimiteddata*olarak değiştirebilirsiniz, ancak türü *Unlimiteddata* iken *Metereddata*olarak değiştiremezsiniz. *Yerel* bir bağlantı hattı her zaman *ayrıcalıklıya verisi*değildir.

> [!IMPORTANT]
> ExpressRoute bağlantı hattı, hizmet anahtarının verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı devre sağlamaya hazırsa bu işlemi gerçekleştirdiğinizden emin olun.
>
>

Yanıt, hizmet anahtarını içerir. Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. tüm ExpressRoute devreleri listeleyin
Oluşturduğunuz tüm ExpressRoute devrelerinin listesini almak için **Get-Azexpressroutedevresi** komutunu çalıştırın:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Yanıt aşağıdaki örneğe benzer şekilde görünür:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Bu bilgileri, `Get-AzExpressRouteCircuit` cmdlet 'ini kullanarak istediğiniz zaman alabilirsiniz. Hiçbir parametre olmadan çağrının yapılması tüm devreleri listeler. Hizmet anahtarınız *ServiceKey* alanında listelenir:

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Yanıt aşağıdaki örneğe benzer şekilde görünür:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. sağlama için hizmet anahtarını bağlantı sağlayıcınıza gönderin
*Serviceproviderprovisioningstate* , hizmet sağlayıcı tarafında sağlama durumunun geçerli durumu hakkında bilgi sağlar. Durum, Microsoft tarafında durumu sağlar. Devre sağlama durumları hakkında daha fazla bilgi için bkz. [Iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Yeni bir ExpressRoute bağlantı hattı oluşturduğunuzda, devre aşağıdaki durumda olur:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Bağlantı sağlayıcısı sizin için etkinleştirme işleminde olduğunda devre aşağıdaki duruma geçer:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

ExpressRoute bağlantı hattını kullanabilmeniz için aşağıdaki durumda olmalıdır:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. her düzenli olarak devre anahtarı durumunu ve durumunu kontrol edin
Devre anahtarının durumunu ve durumunu denetlemek, sağlayıcınız devrenizi etkinleştirdiğinizde emin olmanızı sağlar. Devre yapılandırıldıktan sonra, aşağıdaki örnekte gösterildiği gibi *Serviceproviderprovisioningstate* *sağlandı*olarak görünür:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Yanıt aşağıdaki örneğe benzer şekilde görünür:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. yönlendirme yapılandırmanızı oluşturun
Adım adım yönergeler için, bkz. bağlantı hattı eşlemelerini oluşturmak ve değiştirmek için [ExpressRoute devrouting yapılandırma](expressroute-howto-routing-arm.md) makalesi.

> [!IMPORTANT]
> Bu yönergeler yalnızca, katman 2 bağlantı hizmetleri sunan hizmet sağlayıcılarıyla oluşturulan devrelere uygulanır. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi yapılandırır ve yönetir.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. bir ExpressRoute devresine bir sanal ağ bağlama
Ardından, bir sanal ağı ExpressRoute bağlantı hattına bağlayın. Kaynak Yöneticisi dağıtım modeliyle çalışırken [sanal ağları ExpressRoute devrelerine bağlama](expressroute-howto-linkvnet-arm.md) makalesini kullanın.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>ExpressRoute devresinin durumunu alma
**Get-Azexpressroutedevresi** cmdlet 'ini kullanarak bu bilgileri istediğiniz zaman alabilirsiniz. Hiçbir parametre olmadan çağrının yapılması tüm devreleri listeler.

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Yanıt aşağıdaki örneğe benzer:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Kaynak grubu adını ve devre adını çağrıya bir parametre olarak geçirerek belirli bir ExpressRoute devresi hakkında bilgi alabilirsiniz:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Yanıt aşağıdaki örneğe benzer şekilde görünür:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```azurepowershell-interactive
get-help Get-AzExpressRouteCircuit -detailed
```

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme
Bağlantıyı etkilemeden bir ExpressRoute devresine ait belirli özellikleri değiştirebilirsiniz.

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* ExpressRoute devreniz için bir ExpressRoute Premium eklentisini etkinleştirin veya devre dışı bırakın.
* Bağlantı noktasında kullanılabilir kapasite bulunduğundan ExpressRoute devreniz bant genişliğini artırın. Bağlantı hattının devre dışı olması desteklenmiyor.
* Ölçüm planını ölçülen verilerden sınırsız veriye değiştirin. Ölçüm planının sınırsız verilerden ölçülen verilerle değiştirilmesi desteklenmez.
* *Klasik Işlemlere Izin ver*' i etkinleştirebilir ve devre dışı bırakabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini etkinleştirmek için
Aşağıdaki PowerShell kod parçacığını kullanarak mevcut bağlantı hattınızı ExpressRoute Premium eklentisi ' ni etkinleştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Devre dışı bırakıldı ExpressRoute Premium eklenti özellikleri etkinleştirilmiştir. Komut başarıyla çalıştırıldığında, Premium eklenti özelliği için faturalandırmaya başlayacağız.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini devre dışı bırakmak için
> [!IMPORTANT]
> Standart devre için izin verilen miktardan daha büyük kaynaklar kullanıyorsanız, bu işlem başarısız olabilir.
>
>

Aşağıdaki bilgileri not edin:

* Premium 'dan standart sürümüne düşürme yapmadan önce, bağlantı hattına bağlı sanal ağların sayısının 10 ' dan küçük olduğundan emin olmanız gerekir. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve size Premium ücretler üzerinden faturalandırırız.
* Diğer geopolitik bölgelerdeki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve size Premium ücretler üzerinden faturalandırırız.
* Yol tablonuz, özel eşleme için 4.000 rotadan az olmalıdır. Yol tablonuzun boyutu 4.000 rotadan büyükse BGP oturumu, tanıtılan ön eklerin sayısı 4.000 altına çıkana kadar yeniden etkinleştirilmez.

Aşağıdaki PowerShell cmdlet 'ini kullanarak mevcut devrenin ExpressRoute Premium eklentisini devre dışı bırakabilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için
Sağlayıcınız için desteklenen bant genişliği seçenekleri için bkz. [ExpressRoute SSS](expressroute-faqs.md). Mevcut devrenin boyutundan daha büyük bir boyut seçebilirsiniz.

> [!IMPORTANT]
> Mevcut bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. Bu konumda kullanılabilir ek kapasite yoksa, devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamaz. Bant genişliğini düşürme işlemi, ExpressRoute bağlantı hattının sağlanması ve ardından yeni bir ExpressRoute devresine yeniden sağlanması gerektirir.
>

Hangi boyuta ihtiyacınız olduğuna karar verdikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Devreniz Microsoft tarafında olacak. Daha sonra bu değişikliği eşleştirmek üzere kendi taraflarındaki yapılandırmaların güncelleştirilmesini sağlamak için bağlantı sağlayıcınızla iletişim kurmanız gerekir. Bu bildirimi yaptıktan sonra, güncelleştirilmiş bant genişliği seçeneği için faturalandırmaya başlayacağız.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU 'YU tarifeli 'ten sınırsız 'e taşımak için
Aşağıdaki PowerShell kod parçacığını kullanarak bir ExpressRoute devresine ait SKU 'sunu değiştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Kaynak Yöneticisi ortamlarına erişimi denetlemek için
[ExpressRoute devrelerini klasik Kaynak Yöneticisi dağıtım modeline taşıma](expressroute-howto-move-arm.md)bölümündeki yönergeleri gözden geçirin.

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme
Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, devline bağlı sanal ağların olup olmadığını denetleyin.
* ExpressRoute bağlantı hattı hizmeti sağlayıcısı sağlama **durumu sağlandıysa veya** sağlanmışsa **Provisioned** , devre dışı bırakmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcısı, devre sağlamasını kaldırmayı ve bize haber verene kadar, kaynakları ayırmaya ve sizi faturalandırmaya devam ediyoruz.
* Hizmet sağlayıcı devre sağlamasını kaldırmışsa (hizmet sağlayıcı sağlama durumu **sağlanmadı**olarak ayarlandıysa), devresini silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

Aşağıdaki komutu çalıştırarak ExpressRoute devrenizi silebilirsiniz:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra, aşağıdaki adımları gerçekleştirdiğinizden emin olun:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağınızı ExpressRoute devrenizi ile bağlama](expressroute-howto-linkvnet-arm.md)
