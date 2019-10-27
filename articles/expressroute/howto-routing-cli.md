---
title: 'Bir devre dışı bırakma için eşleme yapılandırma: Azure CLı | Microsoft Docs'
description: Bu makale, bir ExpressRoute devresine ait özel, genel ve Microsoft eşlemesi oluşturmanıza ve sağlamanıza yardımcı olur. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 8d6b361bf7e1b18c44719a8cdbe2e958ac63006d
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72965762"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-cli"></a>CLı kullanarak bir ExpressRoute bağlantı hattı için eşleme oluşturma ve değiştirme

Bu makale, CLı kullanarak Kaynak Yöneticisi dağıtım modelinde bir ExpressRoute devresi için Yönlendirme yapılandırması/eşlemesi oluşturmanıza ve yönetmenize yardımcı olur. Ayrıca, bir ExpressRoute bağlantı hattı için durum, güncelleştirme veya silme ve yinelenenleri kaldırma ve sağlama ayarlarını da denetleyebilirsiniz. Devrenize çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçin:

> [!div class="op_single_selector"]
> * [Azure portalda](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video-özel eşleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video-genel eşleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video-Microsoft eşlemesi](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Başlamadan önce, CLI komutlarının en son sürümünü (2.0 veya üzeri) yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli).
* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışı](expressroute-workflows.md) sayfalarını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](howto-circuit-cli.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. ExpressRoute bağlantı hattının, bu makaledeki komutları çalıştırabilmeniz için sağlanan ve etkinleştirilmiş durumda olması gerekir.

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir ıPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırıp yönetecektir.

Bir ExpressRoute bağlantı hattı için bir, iki veya üç eş (Azure Private, Azure genel ve Microsoft) yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeler hakkında daha fazla bilgi için bkz. [ExpressRoute yönlendirme etki alanları](expressroute-circuit-peerings.md).

## <a name="msft"></a>Microsoft eşlemesi

Bu bölüm, bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmesine yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yönlendirme filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini de alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır. Daha fazla bilgi için bkz. [Microsoft eşlemesi için rota filtresi yapılandırma](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. Azure CLı 'nın en son sürümünü yükler. Azure komut satırı arabiriminin (CLı) en son sürümünü kullanın. * yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [Iş akışlarını](expressroute-workflows.md) gözden geçirin.

   ```azurecli-interactive
   az login
   ```

   ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

   ```azurecli-interactive
   az account set --subscription "<subscription ID>"
   ```
2. ExpressRoute bağlantı hattı oluşturun. Bir [ExpressRoute bağlantı hattı](howto-circuit-cli.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Microsoft eşlemesini sizin için etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra sonraki adımları kullanarak yapılandırmanıza devam edin. 

3. Sağlanmış ve ayrıca etkinleştirildiğinden emin olmak için ExpressRoute devresini denetleyin. Şu örneği kullanın:

   ```azurecli-interactive
   az network express-route list
   ```

   Yanıt aşağıdaki örneğe benzer:

   ```azurecli
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
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

4. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca genel IP adresi önekleri kabul edilir. Bir önek kümesi gönderilmesini planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * **Isteğe bağlı-** Müşteri ASN: eşleme numarasına kayıtlı olmayan ön ekler varsa, bunların kaydedildiği AS numarasını belirtebilirsiniz.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.

   Devreniz için Microsoft eşlemesini yapılandırmak üzere aşağıdaki örneği çalıştırın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 123.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 123.0.0.4/30 --vlan-id 300 --peering-type MicrosoftPeering --advertised-public-prefixes 123.1.0.0/24
   ```

### <a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Aşağıdaki örneği kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```azurecli-interactive
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzureMicrosoftPeering
```
> [!IMPORTANT]
> Microsoft, belirtilen ' tanıtılan genel ön eklerin ' ve ' eşdüzey ASN ' (veya ' müşteri ASN ') tarafından Internet yönlendirme kayıt defterinde size atanıp atanmadığını doğrular. Diğer bir varlıktan ortak ön ekleri alıyorsanız ve atama, yönlendirme kayıt defteriyle birlikte kaydedilmetiyse, otomatik doğrulama tamamlanmaz ve el ile doğrulama gerektirir. Otomatik doğrulama başarısız olursa yukarıdaki komutun çıkışında ' AdvertisedPublicPrefixesState ' öğesini ' doğrulama gerekli ' olarak görürsünüz. 
> 
> ' Doğrulama gerekli ' iletisini görürseniz, genel önekleri gösteren belge (ler) i, yönlendirme kayıt defterindeki ön eklerin sahibi olarak listelenen varlık tarafından kuruluşunuza atanır ve bu belgeleri el ile doğrulama için aşağıda gösterildiği gibi bir destek bileti açılıyor. 
> 
>

Çıktı aşağıdaki örneğe benzer:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzureMicrosoftPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": {
    "advertisedPublicPrefixes": [
        ""
      ],
     "advertisedPublicPrefixesState": "",
     "customerASN": ,
     "routingRegistryName": ""
  }
  "name": "AzureMicrosoftPeering",
  "peerAsn": ,
  "peeringType": "AzureMicrosoftPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updatemsft"></a>Microsoft eşleme yapılandırmasını güncelleştirmek için

Yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Devrenin tanıtılan ön ekleri aşağıdaki örnekte 123.1.0.0/24 ile 124.1.0.0/24 arasında güncelleştiriliyor:

```azurecli-interactive
az network express-route peering update --circuit-name MyCircuit -g ExpressRouteResourceGroup --peering-type MicrosoftPeering --advertised-public-prefixes 124.1.0.0/24
```

### <a name="addIPv6msft"></a>Mevcut bir IPv4 yapılandırmasına IPv6 Microsoft eşleme ayarları eklemek için

```azurecli-interactive
az network express-route peering update -g ExpressRouteResourceGroup --circuit-name MyCircuit --peering-type MicrosoftPeering --ip-version ipv6 --primary-peer-subnet 2002:db00::/126 --secondary-peer-subnet 2003:db00::/126 --advertised-public-prefixes 2002:db00::/126
```

### <a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki örneği çalıştırarak, eşleme yapılandırmanızı kaldırabilirsiniz:

```azurecli-interactive
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name MicrosoftPeering
```

## <a name="private"></a>Azure özel eşleme

Bu bölüm, bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. Azure CLı 'nın en son sürümünü yükler. Azure komut satırı arabiriminin (CLı) en son sürümünü kullanmanız gerekir. * yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [Iş akışlarını](expressroute-workflows.md) gözden geçirin.

   ```azurecli-interactive
   az login
   ```

   ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin

   ```azurecli-interactive
   az account set --subscription "<subscription ID>"
   ```
2. ExpressRoute bağlantı hattı oluşturun. Bir [ExpressRoute bağlantı hattı](howto-circuit-cli.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra sonraki adımları kullanarak yapılandırmanıza devam edin.

3. Sağlanmış ve ayrıca etkinleştirildiğinden emin olmak için ExpressRoute devresini denetleyin. Şu örneği kullanın:

   ```azurecli-interactive
   az network express-route show --resource-group ExpressRouteResourceGroup --name MyCircuit
   ```

   Yanıt aşağıdaki örneğe benzer:

   ```azurecli
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
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

4. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

   * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515’i kullanmadığınızdan emin olun.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.

   Devreniz için Azure özel eşlemesini yapılandırmak üzere aşağıdaki örneği kullanın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering
   ```

   Bir MD5 karması kullanmayı seçerseniz, aşağıdaki örneği kullanın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 10.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 10.0.0.4/30 --vlan-id 200 --peering-type AzurePrivatePeering --SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
   > 
   > 

### <a name="getprivate"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Aşağıdaki örneği kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```azurecli-interactive
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

Çıktı aşağıdaki örneğe benzer:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePrivatePeering",
  "ipv6PeeringConfig": null,
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePrivatePeering",
  "peerAsn": 7671,
  "peeringType": "AzurePrivatePeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki örneği kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, devrenin VLAN KIMLIĞI 100 ' den 500 ' e güncelleştiriliyor.

```azurecli-interactive
az network express-route peering update --vlan-id 500 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

### <a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki örneği çalıştırarak, eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağların ve ExpressRoute Global Reach bağlantılarının kaldırıldığından emin olmanız gerekir. 
> 
> 

```azurecli-interactive
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePrivatePeering
```

## <a name="public"></a>Azure genel eşleme

Bu bölüm, bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

> [!Note]
> Azure genel eşlemesi yeni devreler için kullanım dışıdır. Daha fazla bilgi için bkz. [ExpressRoute eşlemesi](expressroute-circuit-peerings.md).
>

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için

1. Azure CLı 'nın en son sürümünü yükler. Azure komut satırı arabiriminin (CLı) en son sürümünü kullanmanız gerekir. * yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md) ve [Iş akışlarını](expressroute-workflows.md) gözden geçirin.

   ```azurecli-interactive
   az login
   ```

   ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

   ```azurecli-interactive
   az account set --subscription "<subscription ID>"
   ```
2. ExpressRoute bağlantı hattı oluşturun.  Bir [ExpressRoute bağlantı hattı](howto-circuit-cli.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure genel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra sonraki adımları kullanarak yapılandırmanıza devam edin.

3. Sağlanan ve ayrıca etkinleştirildiğinden emin olmak için ExpressRoute bağlantı hattını denetleyin. Şu örneği kullanın:

   ```azurecli-interactive
   az network express-route list
   ```

   Yanıt aşağıdaki örneğe benzer:

   ```azurecli
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
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

4. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.

   Devreniz için Azure ortak eşlemesini yapılandırmak üzere aşağıdaki örneği çalıştırın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
   ```

   Bir MD5 karması kullanmayı seçerseniz, aşağıdaki örneği kullanın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.

### <a name="getpublic"></a>Azure genel eşleme ayrıntılarını görüntülemek için

Yapılandırma ayrıntılarını aşağıdaki örneği kullanarak edinebilirsiniz:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

Çıktı aşağıdaki örneğe benzer:

```azurecli
{
  "azureAsn": 12076,
  "etag": "W/\"2e97be83-a684-4f29-bf3c-96191e270666\"",
  "gatewayManagerEtag": "18",
  "id": "/subscriptions/9a0c2943-e0c2-4608-876c-e0ddffd1211b/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/peerings/AzurePublicPeering",
  "lastModifiedBy": "Customer",
  "microsoftPeeringConfig": null,
  "name": "AzurePublicPeering",
  "peerAsn": 7671,
  "peeringType": "AzurePublicPeering",
  "primaryAzurePort": "",
  "primaryPeerAddressPrefix": "",
  "provisioningState": "Succeeded",
  "resourceGroup": "ExpressRouteResourceGroup",
  "routeFilter": null,
  "secondaryAzurePort": "",
  "secondaryPeerAddressPrefix": "",
  "sharedKey": null,
  "state": "Enabled",
  "stats": null,
  "vlanId": 100
}
```

### <a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki örneği kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, devrenin VLAN KIMLIĞI 200 ' den 600 ' e güncelleştiriliyor.

```azurecli-interactive
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Aşağıdaki örneği çalıştırarak, eşleme yapılandırmanızı kaldırabilirsiniz:

```azurecli-interactive
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [ExpressRoute bağlantı hattına bir VNet bağlama](howto-linkvnet-cli.md).

* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
