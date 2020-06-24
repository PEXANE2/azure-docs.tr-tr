---
title: 'ExpressRoute bağlantı hattı oluşturma ve değiştirme: Azure CLı'
description: Bu makalede, CLı kullanarak bir ExpressRoute bağlantı hattı oluşturma, sağlama, doğrulama, güncelleştirme, silme ve kaldırma işlemlerinin nasıl yapılacağı gösterilir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: how-to
ms.date: 11/13/2019
ms.author: cherylmc
ms.openlocfilehash: 56af984ef83d2e237f0aa05af5cfef4dd6205256
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84738337"
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>CLI kullanarak ExpressRoute bağlantı hattını oluşturma ve değiştirme


Bu makalede komut satırı arabirimi (CLı) kullanılarak Azure ExpressRoute bağlantı hattı oluşturma açıklanır. Bu makalede ayrıca, durumu denetleme, güncelleştirme veya silme ve devre sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir. ExpressRoute devreleri ile çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden makaleyi seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

## <a name="before-you-begin"></a>Başlamadan önce

* Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).
* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute bağlantı hattı oluşturma ve sağlama

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanızı başlatmak için Azure hesabınızda oturum açın. CloudShell "TRY It" kullanırsanız, otomatik olarak oturumunuz açıldı. Bağlanmanıza yardımcı olması için aşağıdaki örnekleri kullanın:

```azurecli-interactive
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli-interactive
az account list
```

ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. desteklenen sağlayıcıların, konumların ve bant genişliklerinin listesini alın

ExpressRoute bağlantı hattı oluşturmadan önce, desteklenen bağlantı sağlayıcıları, konumlar ve bant genişliği seçeneklerinin listesine ihtiyacınız vardır. CLı komutu `az network express-route list-service-providers` , sonraki adımlarda kullanacağınız bu bilgileri döndürür:

```azurecli-interactive
az network express-route list-service-providers
```

Yanıt aşağıdaki örneğe benzer:

```output
[
  {
    "bandwidthsOffered": [
      {
        "offerName": "50Mbps",
        "valueInMbps": 50
      },
      {
        "offerName": "100Mbps",
        "valueInMbps": 100
      },
      {
        "offerName": "200Mbps",
        "valueInMbps": 200
      },
      {
        "offerName": "500Mbps",
        "valueInMbps": 500
      },
      {
        "offerName": "1Gbps",
        "valueInMbps": 1000
      },
      {
        "offerName": "2Gbps",
        "valueInMbps": 2000
      },
      {
        "offerName": "5Gbps",
        "valueInMbps": 5000
      },
      {
        "offerName": "10Gbps",
        "valueInMbps": 10000
      }
    ],
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/expressRouteServiceProviders/",
    "location": null,
    "name": "AARNet",
    "peeringLocations": [
      "Melbourne",
      "Sydney"
    ],
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "tags": null,
    "type": "Microsoft.Network/expressRouteServiceProviders"
  },
```

Bağlantı sağlayıcınızın listelenip listelenmediğini görmek için yanıtı denetleyin. Bir devre oluşturduğunuzda ihtiyacınız olacak aşağıdaki bilgileri unutmayın:

* Name
* PeeringLocations
* BandwidthsOffered

Artık bir ExpressRoute devresi oluşturmaya hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. bir ExpressRoute bağlantı hattı oluşturma

> [!IMPORTANT]
> ExpressRoute bağlantı hattı, hizmet anahtarının verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı, devreyi sağlamaya hazırsa bu işlemi gerçekleştirin.
>
>

Henüz bir kaynak grubunuz yoksa, ExpressRoute bağlantı hattını oluşturmadan önce bir tane oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bir kaynak grubu oluşturabilirsiniz:

```azurecli-interactive
az group create -n ExpressRouteResourceGroup -l "West US"
```

Aşağıdaki örnek, bir 200 Mbps ExpressRoute bağlantı hattının, Silicon Valley içinde Equinx aracılığıyla nasıl oluşturulacağını göstermektedir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız isteğinizi yaparken bu bilgileri yerine koyun.

Doğru SKU katmanını ve SKU ailesini belirttiğinizden emin olun:

* SKU katmanı, bir ExpressRoute devresinin [Yerel](expressroute-faqs.md#expressroute-local), standart veya [Premium](expressroute-faqs.md#expressroute-premium)olup olmadığını belirler. *Yerel*, *Standart* veya *Premium*belirtebilirsiniz.
* SKU ailesi Faturalandırma türünü belirler. Tarifeli veri planı için *Metereddata* , sınırsız bir veri planı Için de *unlimiteddata* belirtebilirsiniz. Faturalandırma türünü *Metereddata* 'Den *unlimiteddata*olarak değiştirebilirsiniz, ancak türü *Unlimiteddata* iken *Metereddata*olarak değiştiremezsiniz. *Yerel* devre yalnızca *limiteddata* ' dır.


ExpressRoute bağlantı hattı, hizmet anahtarının verildiği andan itibaren faturalandırılır. Aşağıdaki örnek, yeni bir hizmet anahtarı isteği örneğidir:

```azurecli-interactive
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Yanıt, hizmet anahtarını içerir.

### <a name="4-list-all-expressroute-circuits"></a>4. tüm ExpressRoute devreleri listeleyin

Oluşturduğunuz tüm ExpressRoute devrelerinin listesini almak için `az network express-route list` komutunu çalıştırın. Bu bilgileri, bu komutu kullanarak istediğiniz zaman alabilirsiniz. Tüm devreleri listelemek için, hiçbir parametre olmadan çağrıyı yapın.

```azurecli-interactive
az network express-route list
```

Hizmet anahtarınız, yanıtın *ServiceKey* alanında listelenir.

```output
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

'-H ' parametresini kullanarak komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz.

```azurecli-interactive
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. sağlama için hizmet anahtarını bağlantı sağlayıcınıza gönderin

' ServiceProviderProvisioningState ', hizmet sağlayıcı tarafında sağlama durumunun geçerli durumu hakkında bilgi sağlar. Durum, Microsoft tarafında durumu sağlar. Daha fazla bilgi için bkz. [Iş akışları makalesi](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Yeni bir ExpressRoute bağlantı hattı oluşturduğunuzda, devre aşağıdaki durumda olur:

```output
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Bağlantı sağlayıcısı sizin için etkinleştirme işleminde olduğunda devre aşağıdaki duruma geçer:

```output
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

ExpressRoute bağlantı hattını kullanabilmeniz için aşağıdaki durumda olmalıdır:

```output
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. her düzenli olarak devre anahtarı durumunu ve durumunu kontrol edin

Devre anahtarının durumunu ve durumunu denetlemek, sağlayıcınız devrenizi etkinleştirdiğinizde emin olmanızı sağlar. Devre yapılandırıldıktan sonra, aşağıdaki örnekte gösterildiği gibi ' ServiceProviderProvisioningState ' ' sağlandı ' olarak görünür:

```azurecli-interactive
az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
```

Yanıt aşağıdaki örneğe benzer:

```output
"allowClassicOperations": false,
"authorizations": [],
"circuitProvisioningState": "Enabled",
"etag": "W/\"1262c492-ffef-4a63-95a8-a6002736b8c4\"",
"gatewayManagerEtag": null,
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit",
"location": "westus",
"name": "MyCircuit",
"peerings": [],
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup",
"serviceKey": "1d05cf70-1db5-419f-ad86-1ca62c3c125b",
"serviceProviderNotes": null,
"serviceProviderProperties": {
  "bandwidthInMbps": 200,
  "peeringLocation": "Silicon Valley",
  "serviceProviderName": "Equinix"
},
"serviceProviderProvisioningState": "NotProvisioned",
"sku": {
  "family": "UnlimitedData",
  "name": "Standard_MeteredData",
  "tier": "Standard"
},
"tags": null,
"type": "Microsoft.Network/expressRouteCircuits]
```

### <a name="7-create-your-routing-configuration"></a>7. yönlendirme yapılandırmanızı oluşturun

Adım adım yönergeler için, bkz. bağlantı hattı eşlemelerini oluşturmak ve değiştirmek için [ExpressRoute devrouting yapılandırma](howto-routing-cli.md) makalesi.

> [!IMPORTANT]
> Bu yönergeler yalnızca, katman 2 bağlantı hizmetleri sunan hizmet sağlayıcılarıyla oluşturulan devrelere uygulanır. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi yapılandırır ve yönetir.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. bir ExpressRoute devresine bir sanal ağ bağlama

Ardından, bir sanal ağı ExpressRoute bağlantı hattına bağlayın. [Sanal ağları ExpressRoute devresine bağlama](howto-linkvnet-cli.md) makalesini kullanın.

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme

Bağlantıyı etkilemeden bir ExpressRoute devresine ait belirli özellikleri değiştirebilirsiniz. Kapalı kalma süresi olmadan aşağıdaki değişiklikleri yapabilirsiniz:

* ExpressRoute devreniz için bir ExpressRoute Premium eklentisini etkinleştirebilir veya devre dışı bırakabilirsiniz.
* Bağlantı noktasında kullanılabilir kapasite bulunduğundan ExpressRoute devrenizi için bant genişliğini artırabilirsiniz. Ancak, bir bağlantı hattının devre dışı bir şekilde düşürülmesi desteklenmez.
* Ölçüm planını ölçülen verilerden sınırsız veriye dönüştürebilirsiniz. Ancak, ölçüm planının sınırsız verilerden ölçülen verilerle değiştirilmesi desteklenmez.
* *Klasik Işlemlere Izin ver*' i etkinleştirebilir ve devre dışı bırakabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz. [ExpressRoute SSS](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini etkinleştirmek için

Aşağıdaki komutu kullanarak mevcut bağlantı hattınızı ExpressRoute Premium eklentisi ' ni etkinleştirebilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Devre dışı bırakıldı ExpressRoute Premium eklenti özellikleri etkinleştirilmiştir. Komut başarıyla çalıştırıldığında, Premium eklenti özelliği için faturalandırmaya başlayacağız.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute Premium eklentisini devre dışı bırakmak için

> [!IMPORTANT]
> Standart devre için izin verilen miktardan daha büyük kaynaklar kullanıyorsanız bu işlem başarısız olabilir.
>
>

ExpressRoute Premium eklentisini devre dışı bırakmadan önce aşağıdaki ölçütleri anlayın:

* Premium 'dan standart sürümüne düşürme yapmadan önce, devresiyle bağlantılı 10 ' dan az sanal ağa sahip olduğunuzdan emin olmanız gerekir. 10 ' dan fazla 'niz varsa, güncelleştirme isteğiniz başarısız olur ve size Premium ücretler üzerinden faturalandırırız.
* Diğer geopolitik bölgelerdeki tüm sanal ağların bağlantısını kaldırmanız gerekir. Tüm sanal ağlarınızın bağlantısını kaldırırsanız, güncelleştirme isteğiniz başarısız olur ve size Premium ücretler üzerinden faturalandırırız.
* Yol tablonuz, özel eşleme için 4.000 rotadan az olmalıdır. Yol tablonuzun boyutu 4.000 rotadan büyükse BGP oturumu düşer. Tanıtılan ön ek sayısı 4.000 altına çıkana kadar oturum yeniden etkinleştirilemez.

Aşağıdaki örneği kullanarak mevcut devre için ExpressRoute Premium eklentisini devre dışı bırakabilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için

Sağlayıcınız için desteklenen bant genişliği seçenekleri için bkz. [ExpressRoute SSS](expressroute-faqs.md). Mevcut devrenin boyutundan daha büyük bir boyut seçebilirsiniz.

> [!IMPORTANT]
> Mevcut bağlantı noktasında yetersiz kapasite varsa, ExpressRoute devresini yeniden oluşturmanız gerekebilir. Bu konumda kullanılabilir ek kapasite yoksa, devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamaz. Bant genişliğini düşürme, ExpressRoute bağlantı hattının sağlanması ve ardından yeni bir ExpressRoute bağlantı hattı sağlamanız gerekir.
>

İhtiyacınız olan boyuta karar verdikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Devreniz Microsoft tarafında boyutlandırılır. Daha sonra, bu değişikliği eşleştirmek üzere kendi taraflarındaki yapılandırmaların güncelleştirilmesini sağlamak için bağlantı sağlayıcınızla iletişim kurmanız gerekir. Bu bildirimi yaptıktan sonra, güncelleştirilmiş bant genişliği seçeneği için faturalandırmaya başlayacağız.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU 'YU tarifeli 'ten sınırsız 'e taşımak için

Aşağıdaki örneği kullanarak bir ExpressRoute devresine ait SKU 'sunu değiştirebilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Kaynak Yöneticisi ortamlarına erişimi denetlemek için

[ExpressRoute devrelerini klasik Kaynak Yöneticisi dağıtım modeline taşıma](expressroute-howto-move-arm.md)bölümündeki yönergeleri gözden geçirin.

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme

Bir ExpressRoute devresini sağlamayı ve silmeyi kaldırmak için aşağıdaki ölçütleri anladığınızdan emin olun:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, devline bağlı sanal ağların olup olmadığını denetleyin.
* ExpressRoute bağlantı hattı hizmet sağlayıcısı sağlama **durumu sağlandıysa veya** sağlanmışsa **, devre**dışı bırakmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Hizmet sağlayıcısı, devre sağlamasını kaldırmayı ve bize haber verene kadar, kaynakları ayırmaya ve sizi faturalandırmaya devam ediyoruz.
* Hizmet sağlayıcısı devresini hazırlaymışsa, devresini silebilirsiniz. Devre dışı bırakıldığında, hizmet sağlayıcısı sağlama durumu **sağlanmadı**olarak ayarlanır. Bu durumda bağlantı hattının faturalandırılması durdurulur.

Aşağıdaki komutu çalıştırarak ExpressRoute devrenizi silebilirsiniz:

```azurecli-interactive
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra, aşağıdaki görevleri yaptığınızdan emin olun:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](howto-routing-cli.md)
* [Sanal ağınızı ExpressRoute devrenizi ile bağlama](howto-linkvnet-cli.md)
