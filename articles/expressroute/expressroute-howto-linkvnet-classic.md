---
title: "Azure ExpressRoute: Bir VNet'i bir devreye bağla: klasik"
description: Bu belge, klasik dağıtım modelini ve PowerShell'i kullanarak sanal ağların (VNet) ExpressRoute devrelerine nasıl bağlanılabağlanane genel bir bakış sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: cherylmc
ms.openlocfilehash: 53c200b01dfa6bce09cfc058dc24ab8e38d253a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930027"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>PowerShell (klasik) kullanarak bir ExpressRoute devresine sanal ağ bağlayın
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Powershell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makale, PowerShell kullanarak sanal ağları (VNets) Azure ExpressRoute devrelerine bağlamanıza yardımcı olur. Tek bir VNet en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute devresine yeni bir bağlantı oluşturmak için bu makaledeki adımları kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin bir karışımında olabilir. Bu makale, klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar için geçerlidir.

Bir ExpressRoute devresine en fazla 10 sanal ağ bağlayabilirsiniz. Tüm sanal ağlar aynı jeopolitik bölgede olmalıdır. ExpressRoute premium eklentisini etkinleştirirseniz, çok sayıda sanal ağı ExpressRoute devrenize bağlayabilir veya diğer jeopolitik bölgelerdeki sanal ağları bağlayabilirsiniz. Premium eklenti hakkında daha fazla bilgi için [SSS'yi](expressroute-faqs.md) kontrol edin.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
   * [Bir ExpressRoute devresi oluşturmak](expressroute-howto-circuit-classic.md) ve bağlantı sağlayıcınızın devreyi etkinleştirmesini sağlamak için yönergeleri izleyin.
   * Devreniz için Azure özel eşlemeyapılı olduğundan emin olun. Yönlendirme yönergeleri için [Yapılandırma yönlendirme](expressroute-howto-routing-classic.md) makalesine bakın.
   * Azure özel eşlemenin yapılandırıldığından ve ağınızdan Microsoft'a bgp eşlemesi doldu böylece uçuca bağlantı sağlayabilirsiniz.
   * Bir sanal ağ ve sanal ağ ağ geçidi oluşturulmuş ve tam olarak sağlanan olmalıdır. [ExpressRoute için bir sanal ağ yapılandırmak için](expressroute-howto-vnet-portal-classic.md)yönergeleri izleyin.

### <a name="download-the-latest-powershell-cmdlets"></a>En son PowerShell cmdlets indirin

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Aynı abonelikteki sanal ağı bir devreye bağlama
Aşağıdaki cmdlet'i kullanarak sanal ağı ExpressRoute devresine bağlayabilirsiniz. Cmdlet'i çalıştırmadan önce sanal ağ ağ geçidinin oluşturulduğundan ve bağlanmaya hazır olduğundan emin olun.

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
Provisioned
```
    
## <a name="remove-a-virtual-network-link-to-a-circuit"></a>Bir devreye sanal ağ bağlantısını kaldırma
Aşağıdaki cmdlet'i kullanarak ExpressRoute devresine giden sanal ağ bağlantısını kaldırabilirsiniz. Verilen sanal ağ için geçerli aboneliğin seçildiğinden emin olun. 

```powershell
Remove-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```
 

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir ExpressRoute devresi'ni birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekil, expressroute devreleri için birden çok abonelik te paylaşımın nasıl çalıştığını gösteren basit bir şema dır.

Büyük bulutiçindeki küçük bulutların her biri, kuruluştaki farklı bölümlere ait abonelikleri temsil etmek için kullanılır. Kuruluş içindeki departmanların her biri hizmetlerini dağıtmak için kendi aboneliklerini kullanabilir, ancak departmanlar şirket içi ağınıza bağlanmak için tek bir ExpressRoute devresini paylaşabilir. Tek bir departman (bu örnekte: BT) ExpressRoute devresi sahibi olabilir. Kuruluş içindeki diğer abonelikler ExpressRoute devresini kullanabilir.

> [!NOTE]
> Özel devre için bağlantı ve bant genişliği ücretleri ExpressRoute devre sahibine uygulanacaktır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Yönetim
*Devre sahibi,* ExpressRoute devresinin oluşturulduğu aboneliğin yöneticisi/yardımcı yöneticisidir. Devre sahibi, *devre kullanıcıları*olarak adlandırılan diğer aboneliklerin yöneticilerini/yardımcı yöneticilerini, sahip oldukları özel devreyi kullanma yetkisi verebilir. Kuruluşun ExpressRoute devresini kullanma yetkisine sahibi olan devre kullanıcıları, aboneliklerindeki sanal ağı, yetkilendikten sonra ExpressRoute devresine bağlayabilirler.

Devre sahibi, yetkilendirmeleri istediği zaman değiştirme ve iptal etme yetkisine sahiptir. Yetkilendirmenin iptaledilmesi, erişimi iptal edilen abonelikten tüm bağlantıların silinmelerine neden olur.

### <a name="circuit-owner-operations"></a>Devre sahibi işlemleri

**Yetkilendirme oluşturma**

Devre sahibi, diğer aboneliklerin yöneticilerine belirtilen devreyi kullanma yetkisi verebsidir. Aşağıdaki örnekte, devrenin yöneticisi (Contoso IT) başka bir aboneliğin (Dev-Test) yöneticisinin devreye en fazla iki sanal ağ bağlamasını sağlar. Contoso BT yöneticisi, Dev-Test Microsoft Kimliğini belirterek bunu sağlar. Cmdlet, belirtilen Microsoft Kimliği'ne e-posta göndermez. Devre sahibinin yetkilendirmenin tamamladığını diğer abonelik sahibine açıkça bildirmesi gerekir.

```powershell
New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'
```

  Şunu döndürür:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 2
  LinkAuthorizationId : **********************************
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Yetkilendirmeleri gözden geçirme**

Devre sahibi aşağıdaki cmdlet çalıştırarak belirli bir devre üzerinde verilen tüm yetkileri gözden geçirebilirsiniz:

```powershell
Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"
```
  Şunu döndürür:

  ```powershell
  Description         : EngineeringTeam
  Limit               : 3
  LinkAuthorizationId : ####################################
  MicrosoftIds        : engadmin@contoso.com
  Used                : 1

  Description         : MarketingTeam
  Limit               : 1
  LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
  MicrosoftIds        : marketingadmin@contoso.com
  Used                : 0

  Description         : Dev-Test Links
  Limit               : 2
  LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  MicrosoftIds        : salesadmin@contoso.com
  Used                : 2
  ```

**Yetkilendirmeleri güncelleştirme**

Devre sahibi aşağıdaki cmdlet kullanarak yetkilendirmeleri değiştirebilir:

```powershell
Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5
```

  Şunu döndürür:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 5
  LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Yetkilendirmeleri silme**

Devre sahibi aşağıdaki cmdlet çalıştırarak kullanıcıya yetkileri iptal /silme:

```powershell
Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"
```

### <a name="circuit-user-operations"></a>Devre kullanıcı işlemleri

**Yetkilendirmeleri gözden geçirme**

Devre kullanıcısı aşağıdaki cmdlet'i kullanarak yetkilendirmeleri gözden geçirebilirsiniz:

```powershell
Get-AzureAuthorizedDedicatedCircuit
```

  Şunu döndürür:

  ```powershell
  Bandwidth                        : 200
  CircuitName                      : ContosoIT
  Location                         : Washington DC
  MaximumAllowedLinks              : 2
  ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  UsedLinks                        : 0
  ```

**Bağlantı yetkisini kullanma**

Devre kullanıcısı, bağlantı yetkilendirmesini kullanmak için aşağıdaki cmdlet'i çalıştırabilir:

```powershell
New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'
```

  Şunu döndürür:

  ```powershell
  State VnetName
  ----- --------
  Provisioned SalesVNET1
  ```

Bu komutu sanal ağ için yeni bağlanan abonelikte çalıştırın:

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
