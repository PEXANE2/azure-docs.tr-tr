---
title: "Azure ExpressRoute: Bir VNet'i devreye bağla: CLI"
description: Bu makalede, Kaynak Yöneticisi dağıtım modelini ve CLI'yi kullanarak sanal ağları (VNet) ExpressRoute devrelerine nasıl bağlayacaklarınız gösterilmektedir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: fdd809bcba703dbd8f9ee1e7c18185fd20e4586f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476143"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>CLI kullanarak sanal ağı ExpressRoute devresine bağlayın

Bu makale, Sanal ağları (VNets) CLI kullanarak Azure ExpressRoute devrelerine bağlamanıza yardımcı olur. Azure CLI kullanarak bağlantı sağlamak için, sanal ağların Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulması gerekir. Bunlar aynı abonelikte veya başka bir aboneliğin parçası olabilir. VNet'inizi bir ExpressRoute devresine bağlamak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Powershell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Komut satırı arabiriminin (CLI) en son sürümüne ihtiyacınız vardır. Daha fazla bilgi için azure [cli'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirmeniz gerekir.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * [Bir ExpressRoute devresi oluşturmak](howto-circuit-cli.md) ve devreyi bağlantı sağlayıcınız tarafından etkinleştirmek için yönergeleri izleyin. 
  * Devreniz için Azure özel eşlemeyapılı olduğundan emin olun. Yönlendirme yönergeleri için [yapılandırma yönlendirme](howto-routing-cli.md) makalesine bakın. 
  * Azure özel eşlemenin yapılandırıldığından emin olun. Ağınızdan microsoft'a kadar bgp eşlemesi, uçuça bağlantı yı etkinleştirebilmeniz için olmalıdır.
  * Sanal bir ağa ve sanal ağ ağ geçidinin oluşturulduğundan ve tam olarak sağlanmış olduğundan emin olun. [ExpressRoute için sanal ağ ağ ağ geçidiyapılandırmak için](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli)yönergeleri izleyin. Kullandığınızdan `--gateway-type ExpressRoute`emin olun.

* Standart bir ExpressRoute devresine en fazla 10 sanal ağ bağlayabilirsiniz. Standart bir ExpressRoute devresi kullanırken tüm sanal ağlar aynı jeopolitik bölgede olmalıdır. 

* Tek bir VNet en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute devresi için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin bir karışımında olabilir.

* ExpressRoute premium eklentisini etkinleştiriseniz, Bir sanal ağı ExpressRoute devresinin jeopolitik bölgesinin dışına bağlayabilir veya daha fazla sayıda sanal ağı ExpressRoute devrenize bağlayabilirsiniz. Premium eklenti hakkında daha fazla bilgi için [SSS](expressroute-faqs.md)bölümüne bakın.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Aynı abonelikteki sanal ağı bir devreye bağlama

Örneği kullanarak bir sanal ağ ağ ağ geçidini ExpressRoute devresine bağlayabilirsiniz. Sanal ağ ağ geçidinin oluşturulduğundan ve komutu çalıştırmadan önce bağlanmaya hazır olduğundan emin olun.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama

Bir ExpressRoute devresi'ni birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekil, expressroute devreleri için birden çok abonelik arasında paylaşımın nasıl çalıştığını gösteren basit bir şema dır.

Büyük bulutiçindeki küçük bulutların her biri, kuruluştaki farklı bölümlere ait abonelikleri temsil etmek için kullanılır. Kuruluş içindeki departmanların her biri, hizmetlerini dağıtmak için kendi aboneliklerini kullanabilir, ancak şirket içi ağınıza bağlanmak için tek bir ExpressRoute devresini paylaşabilir. Tek bir departman (bu örnekte: BT) ExpressRoute devresi sahibi olabilir. Kuruluş içindeki diğer abonelikler ExpressRoute devresini kullanabilir.

> [!NOTE]
> Özel devre için bağlantı ve bant genişliği ücretleri ExpressRoute Circuit Sahibine uygulanacaktır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim - Devre Sahipleri ve Devre Kullanıcıları

'Devre Sahibi' ExpressRoute devre kaynağının yetkili bir Güç Kullanıcısıdır. Devre Sahibi, 'Devre Kullanıcıları' tarafından kullanılabilir yetkilendirmeler oluşturabilir. Devre Kullanıcıları, ExpressRoute devresi ile aynı abonelik içinde olmayan sanal ağ ağ ağ geçitlerinin sahipleridir. Devre Kullanıcıları yetkilendirmeleri kullanabilir (sanal ağ başına bir yetkilendirme).

Devre Sahibi, yetkilendirmeleri istediği zaman değiştirme ve iptal etme yetkisine sahiptir. Yetkilendirme iptal edildiğinde, erişimi iptal edilen abonelikten tüm bağlantı bağlantıları silinir.

### <a name="circuit-owner-operations"></a>Devre Sahibi işlemleri

**Yetkilendirme oluşturmak için**

Devre Sahibi, bir Devre Kullanıcısı tarafından sanal ağ ağ ağ geçitlerini ExpressRoute devresine bağlamak için kullanılabilecek bir yetkilendirme anahtarı oluşturan bir yetkilendirme oluşturur. Yetkilendirme yalnızca bir bağlantı için geçerlidir.

Aşağıdaki örnekte, yetkilendirme nin nasıl oluşturulabildiğini gösterilmektedir:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

Yanıt, yetkilendirme anahtarını ve durumunu içerir:

```output
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**Yetkilendirmeleri gözden geçirmek için**

Devre Sahibi aşağıdaki örneği çalıştırarak belirli bir devrede verilen tüm yetkilendirmeleri gözden geçirebilirsiniz:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Yetkilendirme eklemek için**

Devre Sahibi aşağıdaki örneği kullanarak yetkilendirmeler ekleyebilir:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Yetkilendirmeleri silmek için**

Devre Sahibi aşağıdaki örneği çalıştırarak kullanıcıya yetkilendirmeleri iptal edebilir/silebilir:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Devre Kullanıcı işlemleri

Devre Kullanıcısı'nın eş kimliğine ve Devre Sahibinden bir yetkilendirme anahtarına ihtiyacı vardır. Yetkilendirme anahtarı bir GUID'dir.

```powershell
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bağlantı yetkisini kullanmak için**

Devre Kullanıcısı, bağlantı yetkilendirmesini kullanmak için aşağıdaki örneği çalıştırabilir:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bağlantı yetkilendirmesi serbest bırakmak için**

ExpressRoute devresini sanal ağa bağlayan bağlantıyı silerek yetkilendirme bırakabilirsiniz.

## <a name="modify-a-virtual-network-connection"></a>Sanal ağ bağlantısını değiştirme
Sanal ağ bağlantısının belirli özelliklerini güncelleştirebilirsiniz. 

**Bağlantı ağırlığını güncelleştirmek için**

Sanal ağınız birden çok ExpressRoute devresine bağlanabilir. Aynı öneki birden fazla ExpressRoute devresinden alabilirsiniz. Bu önek için hangi bağlantıyı göndereceğinizi seçmek için, bir bağlantının *RoutingWeight'ini* değiştirebilirsiniz. Trafik en yüksek *RoutingWeight*ile bağlantı gönderilecektir.

```azurecli
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --routing-weight 100
```

*RoutingWeight* aralığı 0-32000 arasındadır. Varsayılan değer 0’dır.

## <a name="configure-expressroute-fastpath"></a>ExpressRoute FastPath'i yapılandır 
ExpressRoute devreniz [ExpressRoute](expressroute-erdirect-about.md) Direct'teyse ve sanal newtork ağ geçidiniz Ultra Performance veya ErGw3AZ ise [ExpressRoute FastPath'i](expressroute-about-virtual-network-gateways.md) etkinleştirebilirsiniz. FastPath, şirket içi ağınızla sanal ağınız arasındaki saniyedeki paketler ve bağlantılar gibi veri yolu preformance'ını geliştirir. 

**FastPath'i yeni bir bağlantıda yapılandırma**

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

**FastPath'i etkinleştirmek için varolan bağlantıyı güncelleştirme**

```azurecli
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true
```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
