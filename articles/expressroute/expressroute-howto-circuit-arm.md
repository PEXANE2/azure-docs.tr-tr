---
title: 'Azure ExpressRoute: Bir devreyi değiştirin: PowerShell'
description: Bir ExpressRoute devresi oluşturun, temin edin, doğrulayın, güncelleyin, silin ve yok edin.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 01/08/2020
ms.author: cherylmc
ms.openlocfilehash: ab44d5358154c17622eef68205ac2326e81377a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75770977"
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>PowerShell kullanarak ExpressRoute bağlantı hattını oluşturma ve değiştirme
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-circuit-portal-resource-manager.md)
> * [Powershell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makale, PowerShell cmdlets ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak bir ExpressRoute devresi oluşturmanıza yardımcı olur. Ayrıca durumu denetleyebilir, güncelleyebilir, silebilir veya bir devreyi devreyi de devreyi devreyi devreyi yok edebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute devresi oluşturma ve sağlama
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Desteklenen sağlayıcıların, konumların ve bant genişliklerinin listesini alın
Bir ExpressRoute devresi oluşturmadan önce desteklenen bağlantı sağlayıcılarının, konumların ve bant genişliği seçeneklerinin listesine ihtiyacınız vardır.

PowerShell cmdlet **Get-AzExpressRouteServiceProvider,** daha sonraki adımlarda kullanacağınız bu bilgileri döndürür:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Bağlantı sağlayıcınızın listede olup olmadığını kontrol edin. Bir devre oluştururken daha sonra ihtiyacınız olan aşağıdaki bilgileri not edin:

* Adı
* PeeringLocations
* Bant GenişlikleriTeklif

Artık bir ExpressRoute devresi oluşturmaya hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute devresi oluşturma
Zaten bir kaynak grubunuz yoksa, ExpressRoute devrenizi oluşturmadan önce bir kaynak oluşturmanız gerekir. Bunu aşağıdaki komutu çalıştırarak yapabilirsiniz:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

Aşağıdaki örnek, Silikon Vadisi'ndeki Equinix üzerinden 200 Mbps'lik bir ExpressRoute devresinin nasıl oluşturultu¤unu göstermektedir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, isteğinizi yaparken bu bilgileri değiştirin. Yeni bir hizmet anahtarı istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Doğru SKU katmanını ve SKU ailesini belirttiğinden emin olun:

* SKU katmanı, ExpressRoute devresi [Yerel,](expressroute-faqs.md#expressroute-local)Standart veya [Premium](expressroute-faqs.md#expressroute-premium)olup olmadığını belirler. *Yerel,* *Standart* veya *Premium*belirtebilirsiniz.
* Fatura türünü SKU ailesi belirler. Tarifeli veri planı için *Ölçülü Verileri* ve sınırsız veri planı için Sınırsız *Veri* belirtebilirsiniz. Fatura türünü *Metereddata'dan* *Unlimiteddata'ya*değiştirebilirsiniz, ancak türü *Unlimiteddata'dan* *Metereddata'ya*değiştiremezsiniz. *Yerel* devre her zaman *Unlimiteddata'dır.*

> [!IMPORTANT]
> ExpressRoute devreniz, bir servis anahtarı verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı devreyi sağlamaya hazır olduğunda bu işlemi gerçekleştirdiğinizi sağlayın.
>
>

Yanıt, hizmet anahtarını içerir. Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. Tüm ExpressRoute devrelerini listele
Oluşturduğunuz tüm ExpressRoute devrelerinin listesini almak için **Get-AzExpressRouteCircuit** komutunu çalıştırın:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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

Bu bilgileri `Get-AzExpressRouteCircuit` istediğiniz zaman cmdlet kullanarak alabilirsiniz. Hiçbir parametre olmadan arama yapma tüm devreleri listeler. Hizmet anahtarınız *ServiceKey* alanında listelenir:

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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Hizmet anahtarını sağlama için bağlantı sağlayıcınıza gönderin
*ServiceProviderProvisioningState,* hizmet sağlayıcı tarafında geçerli sağlama durumu hakkında bilgi sağlar. Durum, Microsoft tarafında durumu sağlar. Devre sağlama durumları hakkında daha fazla bilgi için [Bkz. İş Akışları.](expressroute-workflows.md#expressroute-circuit-provisioning-states)

Yeni bir ExpressRoute devresi oluşturduğunuzda, devre aşağıdaki durumdadır:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Bağlantı sağlayıcısı sizin için etkinleştirme sürecindeyken devre aşağıdaki duruma değişir:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Bir ExpressRoute devresini kullanabilmek için aşağıdaki durumda olmalıdır:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Devre anahtarının durumunu ve durumunu periyodik olarak kontrol edin
Devre anahtarının durumunu ve durumunu denetlemek, sağlayıcınızın devrenizi etkinleştirdiğinde size bildirir. Devre yapılandırıldıktan sonra *ServiceProviderProvisioningState,* aşağıdaki örnekte gösterildiği *gibi, Sağlanan*olarak görünür:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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

### <a name="7-create-your-routing-configuration"></a>7. Yönlendirme yapılandırmanızı oluşturun
Adım adım talimatlar için, devre eşlemeleri oluşturmak ve değiştirmek için [ExpressRoute devre yönlendirme yapılandırma](expressroute-howto-routing-arm.md) makalesine bakın.

> [!IMPORTANT]
> Bu yönergeler yalnızca katman 2 bağlantı hizmetleri sunan servis sağlayıcılarla oluşturulan devreler için geçerlidir. Yönetilen katman 3 hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Bir sanal ağı ExpressRoute devresine bağla
Ardından, bir sanal ağı ExpressRoute devrenize bağla. Kaynak Yöneticisi dağıtım modeliyle çalışırken [ExpressRoute devreleri makalesine bağlama sanal ağları](expressroute-howto-linkvnet-arm.md) kullanın.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>ExpressRoute devresinin durumunu alma
**Get-AzExpressRouteCircuit** cmdlet kullanarak istediğiniz zaman bu bilgileri alabilirsiniz. Hiçbir parametre olmadan arama yapma tüm devreleri listeler.

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


Kaynak grubu adını ve devre adını çağrıya parametre olarak geçirerek belirli bir ExpressRoute devresi hakkında bilgi alabilirsiniz:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
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


Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```azurepowershell-interactive
get-help Get-AzExpressRouteCircuit -detailed
```

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme
Bir ExpressRoute devresinin belirli özelliklerini bağlantı etkilemeden değiştirebilirsiniz.

Aşağıdaki görevleri kapalı kalma süresi olmadan yapabilirsiniz:

* ExpressRoute devreniz için ExpressRoute premium eklentisini etkinleştirin veya devre dışı edin.
* Bağlantı noktasında kapasite olması koşuluyla ExpressRoute devrenizin bant genişliğini artırın. Bir devrenin bant genişliğini düşürme desteklenmez.
* Ölçüm planını Tarifeli Verilerden Sınırsız Veri'ye değiştirin. Ölçüm planının Sınırsız Verilerden Ölçülü Verilere değiştirilmesi desteklenmez.
* *Klasik İşlemler'e İzin Ver'i*etkinleştirebilir ve devre dışı kullanabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için [ExpressRoute SSS](expressroute-faqs.md)bölümüne bakın.

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini etkinleştirmek için
Aşağıdaki PowerShell parçacıklarını kullanarak mevcut devreniz için ExpressRoute premium eklentisini etkinleştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Devre artık ExpressRoute premium eklenti özelliklerine sahiptir. Komut başarıyla çalışır çalışmaz, premium eklenti özelliği için size faturalandırmaya başlıyoruz.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini devre dışı atmak için
> [!IMPORTANT]
> Standart devre için izin verilenden daha büyük kaynaklar kullanıyorsanız, bu işlem başarısız olabilir.
>
>

Aşağıdaki bilgileri not edin:

* Premium'dan standarda düşürmeden önce, devreye bağlı sanal ağ sayısının 10'dan az olduğundan emin olmalısınız. Bunu yapmazsanız, güncelleme isteğiniz başarısız olur ve size prim oranlarıyla faturalandırılırız.
* Diğer jeopolitik bölgelerdeki tüm sanal ağların bağlantısını kalmalıdır. Bunu yapmazsanız, güncelleştirme isteğiniz başarısız olur ve size prim oranlarıyla faturalandırılırız.
* Rota tablonuz özel bakış için 4.000'den az rota olmalıdır. Rota tablo boyutunuz 4.000 rotadan büyükse, BGP oturumu düşer ve reklamı yapılan önek sayısı 4.000'in altına inene kadar yeniden etkinleştirilmez.

Aşağıdaki PowerShell cmdlet'i kullanarak varolan devre için ExpressRoute premium eklentisini devre dışı bırakabilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute devre bant genişliğini güncelleştirmek için
Sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS'yi](expressroute-faqs.md)kontrol edin. Mevcut devrenizin boyutundan daha büyük herhangi bir boyutu seçebilirsiniz.

> [!IMPORTANT]
> Varolan bağlantı noktasında yetersiz kapasite varsa ExpressRoute devresini yeniden oluşturmanız gerekebilir. O konumda ek kapasite yoksa devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamazsınız. Bant genişliğini düşürmek, ExpressRoute devresini kaldırmanızı ve ardından yeni bir ExpressRoute devresini yeniden sağlamanızı gerektirir.
>

Hangi boyuta ihtiyacınız olduğuna karar verildikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Devreniz Microsoft tarafında boyutlandırılır. Ardından, bu değişikliği eşleştirmek için kendi tarafındaki yapılandırmaları güncelleştirmek için bağlantı sağlayıcınıza başvurmanız gerekir. Bu bildirimi yaptıktan sonra, güncelleştirilmiş bant genişliği seçeneği için faturalandırmaya başlarız.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU'yu tarifeliden sınırsıza taşımak için
Bir ExpressRoute devresinin SKU'sunu aşağıdaki PowerShell parçacıklarını kullanarak değiştirebilirsiniz:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Kaynak Yöneticisi ortamlarına erişimi denetlemek için
Klasikten [Kaynak Yöneticisi dağıtım modeline Taşıma ExpressRoute devrelerinde](expressroute-howto-move-arm.md)talimatları gözden geçirin.

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme
Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, devreyle bağlantılı sanal ağların olup olmadığını kontrol edin.
* ExpressRoute devre servis sağlayıcısı sağlama durumu **Hükmünde** veya **Provisioned** ise, devreyi kendi taraflarında devreyi devreyi devreyi devreden çıkarmak için servis sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcı devreyi devreyi devreyi tamamlayıp bizi bize iletene kadar kaynak ayırmaya ve size fatura landırmaya devam ediyoruz.
* Hizmet sağlayıcısı devreyi devreyi devreyi devreyi iptal ettiyse (servis sağlayıcı sağlama durumu **değil**olarak ayarlanır), devreyi silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

ExpressRoute devrenizi aşağıdaki komutu çalıştırarak silebilirsiniz:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra aşağıdaki sonraki adımları yaptığınızdan emin olun:

* [ExpressRoute devreniz için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [Sanal ağınızı ExpressRoute devrenize bağla](expressroute-howto-linkvnet-arm.md)
