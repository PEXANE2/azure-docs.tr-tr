---
title: 'Bir ExpressRoute devresi oluşturma ve değiştirme: Azure CLI'
description: Bu makalede, CLI kullanarak bir ExpressRoute devresi nasıl oluşturulup, hükmetmek, doğrulamak, güncelleştirmek, silmek ve deprovision nasıl gösterilmektedir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: cherylmc
ms.openlocfilehash: b967e1d8751a9c6a5214fef5241d57e954ad9f17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476160"
---
# <a name="create-and-modify-an-expressroute-circuit-using-cli"></a>CLI kullanarak ExpressRoute bağlantı hattını oluşturma ve değiştirme


Bu makalede, Komut Satırı Arabirimi (CLI) kullanarak bir Azure ExpressRoute devresi nasıl oluşturulacak açıklanmaktadır. Bu makalede, bir devrenin durumunu nasıl denetleyip, güncelleştirip sildiğinizi ve nasıl yok edilebildiğinizi de gösterir. ExpressRoute devreleriyle çalışmak için farklı bir yöntem kullanmak istiyorsanız, makaleyi aşağıdaki listeden seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-circuit-portal-resource-manager.md)
> * [Powershell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

## <a name="before-you-begin"></a>Başlamadan önce

* Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli) ve [Azure CLI’yi Kullanmaya Başlama](/cli/azure/get-started-with-azure-cli).
* Yapılandırmaya başlamadan önce [ön koşulları](expressroute-prerequisites.md) ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>ExpressRoute devresi oluşturma ve sağlama

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanıza başlamak için Azure hesabınızda oturum açın. CloudShell "Try It" kullanırsanız, otomatik olarak oturum açmış olursunuz. Bağlanmanıza yardımcı olmak için aşağıdaki örnekleri kullanın:

```azurecli-interactive
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli-interactive
az account list
```

ExpressRoute devresi oluşturmak istediğiniz aboneliği seçin.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Desteklenen sağlayıcıların, konumların ve bant genişliklerinin listesini alın

Bir ExpressRoute devresi oluşturmadan önce desteklenen bağlantı sağlayıcılarının, konumların ve bant genişliği seçeneklerinin listesine ihtiyacınız vardır. CLI komutu, `az network express-route list-service-providers` daha sonraki adımlarda kullanacağınız bu bilgileri döndürür:

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

Bağlantı sağlayıcınızın listeli olup olmadığını görmek için yanıtı denetleyin. Bir devre oluştururken ihtiyaç duymadığınız aşağıdaki bilgileri not edin:

* Adı
* PeeringLocations
* Bant GenişlikleriTeklif

Artık bir ExpressRoute devresi oluşturmaya hazırsınız.

### <a name="3-create-an-expressroute-circuit"></a>3. ExpressRoute devresi oluşturma

> [!IMPORTANT]
> ExpressRoute devreniz, bir servis anahtarı verildiği andan itibaren faturalandırılır. Bağlantı sağlayıcısı devreyi sağlamaya hazır olduğunda bu işlemi gerçekleştirin.
>
>

Zaten bir kaynak grubunuz yoksa, ExpressRoute devrenizi oluşturmadan önce bir kaynak oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bir kaynak grubu oluşturabilirsiniz:

```azurecli-interactive
az group create -n ExpressRouteResourceGroup -l "West US"
```

Aşağıdaki örnek, Silikon Vadisi'ndeki Equinix üzerinden 200 Mbps'lik bir ExpressRoute devresinin nasıl oluşturultu¤unu göstermektedir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, isteğinizi yaparken bu bilgileri değiştirin.

Doğru SKU katmanını ve SKU ailesini belirttiğinden emin olun:

* SKU katmanı, ExpressRoute devresi [Yerel,](expressroute-faqs.md#expressroute-local)Standart veya [Premium](expressroute-faqs.md#expressroute-premium)olup olmadığını belirler. *Yerel,* *Standart* veya *Premium*belirtebilirsiniz.
* Fatura türünü SKU ailesi belirler. Tarifeli veri planı için *Ölçülü Verileri* ve sınırsız veri planı için Sınırsız *Veri* belirtebilirsiniz. Fatura türünü *Metereddata'dan* *Unlimiteddata'ya*değiştirebilirsiniz, ancak türü *Unlimiteddata'dan* *Metereddata'ya*değiştiremezsiniz. *Yerel* devre yalnızca *Sınırsız veridir.*


ExpressRoute devreniz, bir servis anahtarı verildiği andan itibaren faturalandırılır. Aşağıdaki örnek, yeni bir hizmet anahtarı isteğidir:

```azurecli-interactive
az network express-route create --bandwidth 200 -n MyCircuit --peering-location "Silicon Valley" -g ExpressRouteResourceGroup --provider "Equinix" -l "West US" --sku-family MeteredData --sku-tier Standard
```

Yanıt, hizmet anahtarını içerir.

### <a name="4-list-all-expressroute-circuits"></a>4. Tüm ExpressRoute devrelerini listele

Oluşturduğunuz tüm ExpressRoute devrelerinin listesini almak için `az network express-route list` komutu çalıştırın. Bu komutu kullanarak istediğiniz zaman bu bilgileri alabilirsiniz. Tüm devreleri listelemek için, aramayı parametre olmadan yapın.

```azurecli-interactive
az network express-route list
```

Hizmet anahtarınız yanıtın *ServiceKey* alanında listelenir.

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

'-h' parametresini kullanarak komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz.

```azurecli-interactive
az network express-route list -h
```

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Hizmet anahtarını sağlama için bağlantı sağlayıcınıza gönderin

'ServiceProviderProvisioningState', hizmet sağlayıcı tarafında geçerli sağlama durumu hakkında bilgi sağlar. Durum, Microsoft tarafında durumu sağlar. Daha fazla bilgi için [İş Akışları makalesine](expressroute-workflows.md#expressroute-circuit-provisioning-states)bakın.

Yeni bir ExpressRoute devresi oluşturduğunuzda, devre aşağıdaki durumdadır:

```output
"serviceProviderProvisioningState": "NotProvisioned"
"circuitProvisioningState": "Enabled"
```

Bağlantı sağlayıcısı sizin için etkinleştirme sürecindeyken devre aşağıdaki duruma değişir:

```output
"serviceProviderProvisioningState": "Provisioning"
"circuitProvisioningState": "Enabled"
```

Bir ExpressRoute devresini kullanabilmek için aşağıdaki durumda olmalıdır:

```output
"serviceProviderProvisioningState": "Provisioned"
"circuitProvisioningState": "Enabled
```

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Devre anahtarının durumunu ve durumunu periyodik olarak kontrol edin

Devre anahtarının durumunu ve durumunu denetlemek, sağlayıcınızın devrenizi etkinleştirdiğinde size bildirir. Devre yapılandırıldıktan sonra aşağıdaki örnekte gösterildiği gibi 'ServiceProviderProvisioningState' 'Provisioned' olarak görünür:

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

### <a name="7-create-your-routing-configuration"></a>7. Yönlendirme yapılandırmanızı oluşturun

Adım adım talimatlar için, devre eşlemeleri oluşturmak ve değiştirmek için [ExpressRoute devre yönlendirme yapılandırma](howto-routing-cli.md) makalesine bakın.

> [!IMPORTANT]
> Bu yönergeler yalnızca katman 2 bağlantı hizmetleri sunan servis sağlayıcılarla oluşturulan devreler için geçerlidir. Yönetilen katman 3 hizmetleri (genellikle MPLS gibi bir IP VPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Bir sanal ağı ExpressRoute devresine bağla

Ardından, bir sanal ağı ExpressRoute devrenize bağla. [ExpressRoute devreleri makalesine bağlama sanal ağları](howto-linkvnet-cli.md) kullanın.

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>ExpressRoute devresini değiştirme

Bir ExpressRoute devresinin belirli özelliklerini bağlantı etkilemeden değiştirebilirsiniz. Aşağıdaki değişiklikleri hiçbir kesinti olmadan yapabilirsiniz:

* ExpressRoute devreniz için ExpressRoute premium eklentisini etkinleştirebilir veya devre dışı kullanabilirsiniz.
* Bağlantı noktasında kapasite olması koşuluyla ExpressRoute devrenizin bant genişliğini artırabilirsiniz. Ancak, bir devrenin bant genişliğinin küçültülmesi desteklenmez.
* Ölçüm planını Tarifeli Verilerden Sınırsız Veri'ye değiştirebilirsiniz. Ancak, ölçüm planının Sınırsız Verilerden Ölçülü Verilere değiştirilmesi desteklenmez.
* *Klasik İşlemler'e İzin Ver'i*etkinleştirebilir ve devre dışı kullanabilirsiniz.

Sınırlar ve sınırlamalar hakkında daha fazla bilgi için [ExpressRoute SSS](expressroute-faqs.md)bölümüne bakın.

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini etkinleştirmek için

Aşağıdaki komutu kullanarak mevcut devreniz için ExpressRoute premium eklentisini etkinleştirebilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Premium
```

Devre artık ExpressRoute premium eklenti özelliklerine sahiptir. Komut başarıyla çalışır çalışmaz, premium eklenti özelliği için size faturalandırmaya başlıyoruz.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisini devre dışı atmak için

> [!IMPORTANT]
> Standart devre için izin verilenden daha büyük kaynaklar kullanıyorsanız, bu işlem başarısız olabilir.
>
>

ExpressRoute premium eklentisini devre dışı bırakmadan önce aşağıdaki ölçütleri anlayın:

* Premium'dan standarda düşürmeden önce, devreye bağlı 10'dan az sanal ağınız olduğundan emin olmalısınız. 10'dan fazla talebiniz varsa, güncelleştirme isteğiniz başarısız olur ve size prim oranlarıyla faturalandırılırız.
* Diğer jeopolitik bölgelerdeki tüm sanal ağların bağlantısını kalmalıdır. Tüm sanal ağlarınızın bağlantısını kesemezseniz, güncelleme isteğiniz başarısız olur ve size prim fiyatlarıyla fatura kesiyoruz.
* Rota tablonuz özel bakış için 4.000'den az rota olmalıdır. Rota tablo boyutunuz 4.000 rotadan büyükse, BGP oturumu düşer. Oturum, reklamı yapılan önek sayısı 4.000'in altında olana kadar yeniden etkinleştirilmez.

Aşağıdaki örneği kullanarak varolan devre için ExpressRoute premium eklentisini devre dışı kullanabilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-tier Standard
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute devre bant genişliğini güncelleştirmek için

Sağlayıcınız için desteklenen bant genişliği seçenekleri için [ExpressRoute SSS'yi](expressroute-faqs.md)kontrol edin. Mevcut devrenizin boyutundan daha büyük herhangi bir boyutu seçebilirsiniz.

> [!IMPORTANT]
> Varolan bağlantı noktasında yeterli kapasite yoksa, ExpressRoute devresini yeniden oluşturmanız gerekebilir. O konumda ek kapasite yoksa devreyi yükseltemezsiniz.
>
> Bir ExpressRoute devresinin bant genişliğini kesintiye uğramadan azaltamazsınız. Bant genişliğini düşürmek, ExpressRoute devresini kaldırmanızı ve ardından yeni bir ExpressRoute devresini yeniden sağlamanızı gerektirir.
>

İhtiyacınız olan boyuta karar verildikten sonra, devrenizi yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --bandwidth 1000
```

Devreniz Microsoft tarafında boyutlandırılır. Ardından, bu değişikliği eşleştirmek için kendi tarafındaki yapılandırmaları güncelleştirmek için bağlantı sağlayıcınıza başvurmanız gerekir. Bu bildirimi yaptıktan sonra, güncelleştirilmiş bant genişliği seçeneği için faturalandırmaya başlarız.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>SKU'yu tarifeliden sınırsıza taşımak için

Bir ExpressRoute devresinin SKU'sını aşağıdaki örneği kullanarak değiştirebilirsiniz:

```azurecli-interactive
az network express-route update -n MyCircuit -g ExpressRouteResourceGroup --sku-family UnlimitedData
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klasik ve Kaynak Yöneticisi ortamlarına erişimi denetlemek için

Klasikten [Kaynak Yöneticisi dağıtım modeline Taşıma ExpressRoute devrelerinde](expressroute-howto-move-arm.md)talimatları gözden geçirin.

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Bir ExpressRoute bağlantı hattının sağlamasını kaldırma ve silme

Bir ExpressRoute devresini deprovision ve silmek için aşağıdaki ölçütleri anladığınızdan emin olun:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, devreyle bağlantılı sanal ağların olup olmadığını kontrol edin.
* ExpressRoute devre servis sağlayıcısı sağlama durumu **Hükmünde** veya **Provisioned**ise, onların tarafında devre deprovision için servis sağlayıcı ile birlikte çalışmanız gerekir. Hizmet sağlayıcı devreyi devreyi devreyi tamamlayıp bizi bize iletene kadar kaynak ayırmaya ve size fatura landırmaya devam ediyoruz.
* Servis sağlayıcı devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi devreyi silebilirsiniz. Bir devre deprovisioned olduğunda, hizmet sağlayıcı sağlama durumu **değil hükmü**olarak ayarlanır. Bu durumda bağlantı hattının faturalandırılması durdurulur.

ExpressRoute devrenizi aşağıdaki komutu çalıştırarak silebilirsiniz:

```azurecli-interactive
az network express-route delete  -n MyCircuit -g ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Devrenizi oluşturduktan sonra aşağıdaki görevleri yaptığınızdan emin olun:

* [ExpressRoute devreniz için yönlendirme oluşturma ve değiştirme](howto-routing-cli.md)
* [Sanal ağınızı ExpressRoute devrenize bağla](howto-linkvnet-cli.md)
