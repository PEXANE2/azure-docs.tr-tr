---
title: 'Azure ExpressRoute: Eşleme yapılandırma: klasik'
description: Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: cherylmc
ms.openlocfilehash: 05602538f206032d924b39a7dd8f4325c48a5224
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931385"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>ExpressRoute devresi (klasik) için eşleme oluşturma ve değiştirme
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-routing-portal-resource-manager.md)
> * [Powershell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - Özel akran](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - Genel bakış](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft'a bakış](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

Bu makalede, PowerShell ve klasik dağıtım modelini kullanarak bir ExpressRoute devresi için eşleme/yönlendirme yapılandırması oluşturma ve yönetme adımları size iletilmesi. Aşağıdaki adımlarda ayrıca bir ExpressRoute bağlantı hattının durumunu denetleme, güncelleştirme veya bağlantı hattını silme ve eşlemelerin sağlamasını kaldırma işlemleri de anlatılmaktadır. Bir ExpressRoute devresi için bir, iki veya üç eşlemenin (Azure özel, Azure geneli ve Microsoft) yapılandırAbilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan servis sağlayıcılarla oluşturulan devreler için geçerlidir. Yönetilen Katman 3 hizmetleri (genellikle MPLS gibi bir IPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandıracak ve yönetecektir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. [Bir ExpressRoute devresi oluşturmak](expressroute-howto-circuit-classic.md) için yönergeleri izleyin ve devam etmeden önce devrebağlantı sağlayıcınız tarafından etkinleştirin. Aşağıda açıklanan cmdlet’leri çalıştırmanız için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir.

### <a name="download-the-latest-powershell-cmdlets"></a>En son PowerShell cmdlets indirin

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

## <a name="azure-private-peering"></a>Azure özel eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. **Bir ExpressRoute devresi oluşturun.**

   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **Verilen olduğundan emin olmak için ExpressRoute devresini kontrol edin.**
   
   ExpressRoute devresinin Sağlanıp Sağlanıp Sağlanmadı ve aynı zamanda etkin olup olmadığını denetleyin.

   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Şunu döndürür:

   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Devrenin Sağlanan ve Etkin olarak gösterdiğinden emin olun. Değilse, devrenizi gerekli duruma ve duruma getirmek için bağlantı sağlayıcınızla birlikte çalışın.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
3. **Bağlantı hattı için Azure özel eşlemesini yapılandırın.**

   Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:
   
   * Birincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Devredeki başka hiçbir eşlemenin aynı VLAN KIMLIĞINI kullanmadığını doğrulayın.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515 kullanmadığınızı doğrulayın.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **İsteğe bağlı**.
     
   Devreniz için Azure özel eşlemi yapılandırmak için aşağıdaki örneği kullanabilirsiniz:

   ```powershell
   New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100
   ```    

   BIR MD5 karma kullanmak istiyorsanız, devreniz için özel eşlemi yapılandırmak için aşağıdaki örneği kullanın:

   ```powershell
   New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"
   ```
     
   > [!IMPORTANT]
   > AS numaranızı müşteri ASN değil, asn'yi bakan ASN olarak belirttiğinizi doğrulayın.
   > 

### <a name="to-view-azure-private-peering-details"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Yapılandırma ayrıntılarını aşağıdaki cmdlet'i kullanarak görüntüleyebilirsiniz:

```powershell
Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

Şunu döndürür:

```
AdvertisedPublicPrefixes       : 
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 10.0.0.0/30
RoutingRegistryName            : 
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 10.0.0.4/30
State                          : Enabled
VlanId                         : 100
```

### <a name="to-update-azure-private-peering-configuration"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Aşağıdaki örnekte, devrenin VLAN kimliği 100'den 500'e güncellenmektedir.

```powershell
Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"
```

### <a name="to-delete-azure-private-peering"></a>Azure özel eşlemesini silmek için

Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz. Bu cmdlet'i çalıştırmadan önce tüm sanal ağların ExpressRoute devresinden bağlantısız olduğundan emin olmalısınız.

```powershell
Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
```

## <a name="azure-public-peering"></a>Azure ortak eşleme

Bu bölümde bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır.

> [!NOTE]
> Azure genel bakışları yeni devreler için amortismana hazırdır.
>

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için

1. **ExpressRoute bağlantı hattı oluşturma**

   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure ortak eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **Sağlanmış olduğunu doğrulamak için ExpressRoute devresini kontrol edin**

   Önce ExpressRoute ağ geçidinin Sağlandığından ve Etkin durumda olduğundan emin olmanız gerekir.

   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Şunu döndürür:

   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Devrenin Sağlanan ve Etkin olarak gösteriş yaptığından doğrulayın. Değilse, devrenizi gerekli duruma ve duruma getirmek için bağlantı sağlayıcınızla birlikte çalışın.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
4. **Devre için Azure ortak eşlemesini yapılandırma**
   
   Devam etmeden önce aşağıdaki bilgilere sahip olduğundan emin olun:
   
   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Devredeki başka hiçbir eşlemenin aynı VLAN KIMLIĞINI kullanmadığını doğrulayın.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **İsteğe bağlı**.

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN değil, AS n'ye bakan ASN olarak belirttiğinizden emin olun.
   >  
     
   Devreniz için Azure genel eşlemi yapılandırmak için aşağıdaki örneği kullanabilirsiniz:

   ```powershell
   New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200
   ```
     
   MD5 karma kullanmak istiyorsanız, devrenizi yapılandırmak için aşağıdaki örneği kullanın:
     
   ```powershell
   New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"
   ```
     
### <a name="to-view-azure-public-peering-details"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Yapılandırma ayrıntılarını görüntülemek için aşağıdaki cmdlet'i kullanın:

```powershell
Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

Şunu döndürür:

```powershell
AdvertisedPublicPrefixes       : 
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 131.107.0.0/30
RoutingRegistryName            : 
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 131.107.0.4/30
State                          : Enabled
VlanId                         : 200
```

### <a name="to-update-azure-public-peering-configuration"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki cmdlet'i kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, devrenin VLAN kimliği 200'den 600'e güncellenmektedir.

```powershell
Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"
```

Devrenin Sağlanan ve Etkin olarak gösteriş yaptığından doğrulayın. 
### <a name="to-delete-azure-public-peering"></a>Azure ortak eşlemesini silmek için

Aşağıdaki cmdlet çalıştırarak akran yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
```

## <a name="microsoft-peering"></a>Microsoft eşlemesi

Bu bölümde bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturma, alma, güncelleştirme ve silme hakkında yönergeler açıklanmaktadır. 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. **ExpressRoute bağlantı hattı oluşturma**
  
   Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-classic.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra aşağıdaki yönergeleri izleyin.
2. **Sağlanmış olduğunu doğrulamak için ExpressRoute devresini kontrol edin**

   Devrenin Sağlanan ve Etkin olarak gösteriş yaptığından doğrulayın. 
   
   ```powershell
   Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   ```

   Şunu döndürür:
   
   ```powershell
   Bandwidth                        : 200
   CircuitName                      : MyTestCircuit
   Location                         : Silicon Valley
   ServiceKey                       : *********************************
   ServiceProviderName              : equinix
   ServiceProviderProvisioningState : Provisioned
   Sku                              : Standard
   Status                           : Enabled
   ```
   
   Devrenin Sağlanan ve Etkin olarak gösteriş yaptığından doğrulayın. Değilse, devrenizi gerekli duruma ve duruma getirmek için bağlantı sağlayıcınızla birlikte çalışın.

   ```powershell
   ServiceProviderProvisioningState : Provisioned
   Status                           : Enabled
   ```
3. **Devre için Microsoft eşleme yapılandırma**
   
    Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.
   
   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Devredeki başka hiçbir eşlemenin aynı VLAN KIMLIĞINI kullanmadığını doğrulayın.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir önek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * Müşteri ASN’si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz. **İsteğe bağlı**.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * Kullanmayı seçerseniz bir MD5 karma değeri. **Isteğe bağlı.**
     
   Devreniz için Microsoft'u yapılandırmak için aşağıdaki cmdlet'i çalıştırın:
 
   ```powershell
   New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
   ```

### <a name="to-view-microsoft-peering-details"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Yapılandırma ayrıntılarını aşağıdaki cmdlet'i kullanarak görüntüleyebilirsiniz:

```powershell
Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```
Şunu döndürür:

```powershell
AdvertisedPublicPrefixes       : 123.0.0.0/30
AdvertisedPublicPrefixesState  : Configured
AzureAsn                       : 12076
CustomerAutonomousSystemNumber : 2245
PeerAsn                        : 1234
PrimaryAzurePort               : 
PrimaryPeerSubnet              : 10.0.0.0/30
RoutingRegistryName            : ARIN
SecondaryAzurePort             : 
SecondaryPeerSubnet            : 10.0.0.4/30
State                          : Enabled
VlanId                         : 300
```

### <a name="to-update-microsoft-peering-configuration"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Yapılandırmanın herhangi bir bölümünü aşağıdaki cmdlet'i kullanarak güncelleyebilirsiniz:

```powershell
Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"
```

### <a name="to-delete-microsoft-peering"></a>Microsoft eşlemesini silmek için

Aşağıdaki cmdlet çalıştırarak akran yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
```

## <a name="next-steps"></a>Sonraki adımlar

Ardından, [Bir VNet'i ExpressRoute devresine bağla.](expressroute-howto-linkvnet-classic.md)

* İş akışları hakkında daha fazla bilgi için [ExpressRoute iş akışlarına](expressroute-workflows.md)bakın.
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
