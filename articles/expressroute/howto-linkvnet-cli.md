---
title: 'Azure ExpressRoute: sanal ağı devre dışı bağlantı: CLı'
description: Bu makalede, Kaynak Yöneticisi dağıtım modeli ve CLı kullanarak sanal ağları (VNet) ExpressRoute devrelerine nasıl bağlayabileceğiniz gösterilmektedir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: fdd809bcba703dbd8f9ee1e7c18185fd20e4586f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79476143"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>CLı kullanarak bir ExpressRoute devresine sanal ağ bağlama

Bu makale, CLı kullanarak sanal ağları (VNet) Azure ExpressRoute devrelerine bağlamanıza yardımcı olur. Azure CLı kullanarak bağlantı için, sanal ağların Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulması gerekir. Aynı abonelikte ya da başka bir aboneliğin parçası olabilir. VNet 'nizi bir ExpressRoute devresine bağlamak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* En son komut satırı arabirimi (CLı) sürümüne ihtiyacınız vardır. Daha fazla bilgi için bkz. [Azure CLI 'Yı yüklerken](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirmeniz gerekir.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * [ExpressRoute](howto-circuit-cli.md) bağlantı hattı oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı. 
  * Devreniz için Azure özel eşlemesi 'nin yapılandırıldığından emin olun. Yönlendirme yönergeleri için [yönlendirmeyi yapılandırma](howto-routing-cli.md) makalesine bakın. 
  * Azure özel eşlemesinin yapılandırıldığından emin olun. Uçtan uca bağlantıyı etkinleştirebilmeniz için ağınız ve Microsoft arasındaki BGP eşlemesi olmalıdır.
  * Oluşturulmuş ve tam olarak sağlanmış bir sanal ağa ve sanal ağ geçidine sahip olduğunuzdan emin olun. [ExpressRoute için sanal ağ geçidi yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli)yönergelerini izleyin. ' İ kullandığınızdan emin `--gateway-type ExpressRoute`olun.

* 10 adede kadar sanal ağı standart bir ExpressRoute devresine bağlayabilirsiniz. Standart bir ExpressRoute bağlantı hattı kullanılırken tüm sanal ağların aynı geopolitik bölgede olması gerekir. 

* Tek bir sanal ağ, en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin karışımı olabilir.

* ExpressRoute Premium eklentisini etkinleştirirseniz bir sanal ağı ExpressRoute bağlantı hattının geopolitik bölgesinin dışında bağlayabilir veya ExpressRoute devrenizin daha fazla sayıda sanal ağ bağlayabilirsiniz. Premium eklentisi hakkında daha fazla bilgi için bkz. [SSS](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Aynı abonelikteki bir sanal ağı bir devreye bağlama

Örneği kullanarak bir ExpressRoute devresine bir sanal ağ geçidini bağlayabilirsiniz. Komutu çalıştırmadan önce, sanal ağ geçidinin oluşturulduğundan ve bağlamaya hazırlandığınızdan emin olun.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama

Bir ExpressRoute devresini birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekilde, birden çok aboneliğin ExpressRoute devreleri için paylaşımın nasıl çalıştığı basit bir şematik gösterilmektedir.

Büyük buluttaki küçük bulutların her biri, bir kuruluştaki farklı departmanlara ait olan abonelikleri temsil etmek için kullanılır. Kuruluştaki bölümlerin her biri, hizmetlerini dağıtmak için kendi aboneliğini kullanabilir. ancak, şirket içi ağınıza geri bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşabilir. Tek bir departman (Bu örnekte:) ExpressRoute devresine sahip olabilir. Kuruluştaki diğer abonelikler ExpressRoute devresini kullanabilir.

> [!NOTE]
> Adanmış devre için bağlantı ve bant genişliği ücretleri ExpressRoute bağlantı hattı sahibine uygulanır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim-devre sahipleri ve devre kullanıcıları

' Devowner ', ExpressRoute bağlantı hattı kaynağının yetkili bir uzman kullanıcısı. Devre sahibi, ' devre kullanıcıları ' tarafından kullanılabilecek yetkilendirmeler oluşturabilir. Devre kullanıcıları ExpressRoute bağlantı hattı ile aynı abonelikte yer alan sanal ağ geçitlerinin sahiplerinden oluşur. Devre kullanıcıları yetkilendirmeleri (sanal ağ başına bir yetkilendirme) kullanabilir.

Devre sahibinin, her zaman yetkilendirmeleri değiştirme ve iptal etme gücü vardır. Yetkilendirme iptal edildiğinde, tüm bağlantı bağlantıları, erişimi iptal edilen abonelikten silinir.

### <a name="circuit-owner-operations"></a>Devre sahibi işlemleri

**Yetkilendirme oluşturmak için**

Devre sahibi, sanal ağ geçitlerini ExpressRoute devresine bağlamak için bir devre kullanıcısı tarafından kullanılabilen bir yetkilendirme anahtarı oluşturan bir yetkilendirme oluşturur. Yetkilendirme yalnızca bir bağlantı için geçerlidir.

Aşağıdaki örnek nasıl bir yetkilendirme oluşturulacağını göstermektedir:

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

Devre sahibi, aşağıdaki örneği çalıştırarak belirli bir devreye göre verilen tüm yetkilendirmeleri gözden geçirebilir:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Yetkilendirmeler eklemek için**

Devre sahibi aşağıdaki örneği kullanarak yetkilendirmeler ekleyebilir:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Yetkilendirmeleri silmek için**

Devre sahibi, aşağıdaki örneği çalıştırarak kullanıcıya yetkilendirmeleri iptal edebilir/silebilir:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Devre Kullanıcı işlemleri

Devre kullanıcısının, devre sahibinden eş KIMLIĞI ve yetkilendirme anahtarı olması gerekir. Yetkilendirme anahtarı bir GUID 'dir.

```powershell
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bir bağlantı yetkilendirmesini kullanma**

Devre kullanıcısı bir bağlantı yetkilendirmesi kullanmak için aşağıdaki örneği çalıştırabilir:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bir bağlantı yetkilendirmesini serbest bırakmak için**

ExpressRoute bağlantı hattını sanal ağa bağlayan bağlantıyı silerek bir yetkilendirmeyi serbest bırakabilirsiniz.

## <a name="modify-a-virtual-network-connection"></a>Bir sanal ağ bağlantısını değiştirme
Bir sanal ağ bağlantısının belirli özelliklerini güncelleştirebilirsiniz. 

**Bağlantı ağırlığını güncelleştirmek için**

Sanal ağınız, birden fazla ExpressRoute devresine bağlanabilir. Birden fazla ExpressRoute devreninden aynı öneki alabilirsiniz. Bu önek için hangi trafiğin gönderileceğini seçmek üzere bir bağlantının *Routingweight* değerini değiştirebilirsiniz. Trafik en yüksek *Routingweight*bağlantısı üzerinden gönderilir.

```azurecli
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --routing-weight 100
```

*Routingweight* aralığı 0 ile 32000 arasındadır. Varsayılan değer 0’dır.

## <a name="configure-expressroute-fastpath"></a>ExpressRoute FastPath yapılandırma 
ExpressRoute bağlantı hattınızı [ExpressRoute Direct](expressroute-erdirect-about.md) üzerinde ise ve sanal ağ seçin ağ geçidiniz Ultra Performance veya ErGw3AZ ise [ExpressRoute FastPath](expressroute-about-virtual-network-gateways.md) 'i etkinleştirebilirsiniz. FastPath, şirket içi ağınız ve sanal ağınız arasında saniyedeki paket sayısı ve saniye başına bağlantı gibi veri yolu preformance geliştirir. 

**Yeni bir bağlantıda FastPath yapılandırma**

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

**FastPath 'i etkinleştirmek için mevcut bir bağlantı güncelleştiriliyor**

```azurecli
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true
```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
