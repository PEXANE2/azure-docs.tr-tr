---
title: "Azure ExpressRoute: ExpressRoute Direct 'i yapılandırma"
description: Azure ExpressRoute Direct 'in dünya genelindeki eşleme konumlarında doğrudan Microsoft Global Network 'e bağlanması için nasıl Azure PowerShell kullanacağınızı öğrenin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 01/22/2020
ms.author: duau
ms.openlocfilehash: c4ce764f50f85ef9979d5a14235759c16228f6b7
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89396038"
---
# <a name="how-to-configure-expressroute-direct"></a>ExpressRoute Direct 'i yapılandırma

ExpressRoute Direct, doğrudan Microsoft 'un Dünya genelinde dağıtılmış olan eşleme konumlarında doğrudan Microsoft 'un küresel ağına bağlanmasına olanak sağlar. Daha fazla bilgi için bkz. [ExpressRoute Direct Hakkında](expressroute-erdirect-about.md).

## <a name="create-the-resource"></a><a name="resources"></a>Kaynağı oluşturma

1. Azure 'da oturum açın ve aboneliği seçin. ExpressRoute doğrudan kaynağı ve ExpressRoute devreleri aynı abonelikte olmalıdır.

   ```powershell
   Connect-AzAccount 

   Select-AzSubscription -Subscription "<SubscriptionID or SubscriptionName>"
   ```
   
2. Expressrouteportslocation ve expressrouteport API 'Lerine erişmek için aboneliğinizi Microsoft. Network 'e yeniden kaydedin.

   ```powershell
   Register-AzResourceProvider -ProviderNameSpace "Microsoft.Network"
   ```   
3. ExpressRoute Direct 'in desteklendiği tüm konumları listeleyin.
  
   ```powershell
   Get-AzExpressRoutePortsLocation
   ```

   **Örnek çıkış**
  
   ```powershell
   Name                : Equinix-Ashburn-DC2
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-D
                        C2
   ProvisioningState   : Succeeded
   Address             : 21715 Filigree Court, DC2, Building F, Ashburn, VA 20147
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-Dallas-DA3
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA
                        3
   ProvisioningState   : Succeeded
   Address             : 1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207
   Contact             : support@equinix.com
   AvailableBandwidths : []

   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : []
   ```
4. Yukarıda listelenen bir konumun kullanılabilir bant genişliğine sahip olup olmadığını belirleme

   ```powershell
   Get-AzExpressRoutePortsLocation -LocationName "Equinix-San-Jose-SV1"
   ```

   **Örnek çıkış**

   ```powershell
   Name                : Equinix-San-Jose-SV1
   Id                  : /subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-
                        SV1
   ProvisioningState   : Succeeded
   Address             : 11 Great Oaks Blvd, SV1, San Jose, CA 95119
   Contact             : support@equinix.com
   AvailableBandwidths : [
                          {
                            "OfferName": "100 Gbps",
                            "ValueInGbps": 100
                          }
                        ]
   ```
5. Yukarıda seçilen konuma göre bir ExpressRoute doğrudan kaynağı oluşturun

   ExpressRoute Direct, QinQ ve Dot1Q kapsüllemeyi destekler. QinQ seçilirse, her bir ExpressRoute devresine dinamik olarak bir S etiketi atanır ve ExpressRoute doğrudan kaynağında benzersiz olacaktır. Devrendeki her C etiketi, Devroute Direct üzerinde değil, devre üzerinde benzersiz olmalıdır.  

   Dot1Q kapsülleme seçilirse, tüm ExpressRoute doğrudan kaynağı genelinde C-Tag (VLAN) öğesinin benzersizliğini yönetmeniz gerekir.  

   > [!IMPORTANT]
   > ExpressRoute Direct yalnızca bir kapsülleme türü olabilir. ExpressRoute doğrudan oluşturulduktan sonra kapsülleme değiştirilemez.
   > 
 
   ```powershell 
   $ERDirect = New-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName -PeeringLocation $PeeringLocationName -BandwidthInGbps 100.0 -Encapsulation QinQ | Dot1Q -Location $AzureRegion
   ```

   > [!NOTE]
   > Kapsülleme özniteliği de Dot1Q olarak ayarlanabilir. 
   >

   **Örnek çıktı:**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                               ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                                 {
                                   "Name": "link1",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link1",
                                   "RouterName": "tst-09xgmr-cis-1",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "link2",
                                   "Etag": "W/\"<etagnumber>\"",
                                   "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                               Network/expressRoutePorts/Contoso-Direct/links/link2",
                                   "RouterName": "tst-09xgmr-cis-2",
                                   "InterfaceName": "HundredGigE2/2/2",
                                   "PatchPanelId": "PPID",
                                   "RackId": "RackID",
                                   "ConnectorType": "SC",
                                   "AdminState": "Disabled",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
   Circuits                   : []
   ```

## <a name="change-admin-state-of-links"></a><a name="state"></a>Bağlantıların yönetici durumunu değiştirme

  Bu işlem, katman 1 testi yürütmek için kullanılmalıdır, her bir çapraz bağlantının, birincil ve ikincil için her bir yönlendiriciye düzgün bir şekilde düzeltme eki olmasını sağlar.
1. ExpressRoute doğrudan ayrıntılarını alın.

   ```powershell
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   ```
2. Bağlantıyı etkin olarak ayarlayın. Her bağlantıyı etkin olarak ayarlamak için bu adımı tekrarlayın.

   [0] bağlantıları birincil bağlantı noktasıdır ve [1] bağlantıları ikincil bağlantı noktasıdır.

   ```powershell
   $ERDirect.Links[0].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   $ERDirect = Get-AzExpressRoutePort -Name $Name -ResourceGroupName $ResourceGroupName
   $ERDirect.Links[1].AdminState = "Enabled"
   Set-AzExpressRoutePort -ExpressRoutePort $ERDirect
   ```
   **Örnek çıktı:**

   ```powershell
   Name                       : Contoso-Direct
   ResourceGroupName          : Contoso-Direct-rg
   Location                   : westcentralus
   Id                         : /subscriptions/<number>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/exp
                             ressRoutePorts/Contoso-Direct
   Etag                       : W/"<etagnumber> "
   ResourceGuid               : <number>
   ProvisioningState          : Succeeded
   PeeringLocation            : Equinix-Seattle-SE2
   BandwidthInGbps            : 100
   ProvisionedBandwidthInGbps : 0
   Encapsulation              : QinQ
   Mtu                        : 1500
   EtherType                  : 0x8100
   AllocationDate             : Saturday, September 1, 2018
   Links                      : [
                               {
                                 "Name": "link1",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link1",
                                 "RouterName": "tst-09xgmr-cis-1",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               },
                               {
                                 "Name": "link2",
                                 "Etag": "W/\"<etagnumber>\"",
                                 "Id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.
                             Network/expressRoutePorts/Contoso-Direct/links/link2",
                                 "RouterName": "tst-09xgmr-cis-2",
                                 "InterfaceName": "HundredGigE2/2/2",
                                 "PatchPanelId": "PPID",
                                 "RackId": "RackID",
                                 "ConnectorType": "SC",
                                 "AdminState": "Enabled",
                                 "ProvisioningState": "Succeeded"
                               }
                             ]
   Circuits                   : []
   ```

   `AdminState = "Disabled"`Bağlantı noktalarını kapatmak için ile aynı yordamı kullanın.

## <a name="create-a-circuit"></a><a name="circuit"></a>Bağlantı hattı oluşturma

Varsayılan olarak, abonelikte ExpressRoute doğrudan kaynağının olduğu 10 devre oluşturabilirsiniz. Bu, destek aracılığıyla artırılabilir. Hem sağlanan hem de kullanılan bant genişliğini izlemeden sorumlusunuz. Sağlanan bant genişliği, ExpressRoute doğrudan kaynağında bulunan tüm devrelerin bant genişliğinin toplamıdır ve kullanılan bant genişliği, temel alınan fiziksel arabirimlerin fiziksel kullanımdır.

Yalnızca yukarıda özetlenen senaryoları desteklemek üzere ExpressRoute Direct üzerinde kullanılabilen ek devre bant genişlikleri vardır. Bunlar: 40Gbps ve 100Gbps.

**Skutier** yerel, standart veya Premium olabilir.

**Skufamily** , doğrudan ExpressRoute Direct üzerinde desteklenmediği için MeteredData olmalıdır.

ExpressRoute doğrudan kaynağında bir devre oluşturun.

  ```powershell
  New-AzExpressRouteCircuit -Name $Name -ResourceGroupName $ResourceGroupName -ExpressRoutePort $ERDirect -BandwidthinGbps 100.0  -Location $AzureRegion -SkuTier Premium -SkuFamily MeteredData 
  ```

  Diğer bant genişlikleri şunlardır: 5,0, 10,0 ve 40,0

  **Örnek çıktı:**

  ```powershell
  Name                             : ExpressRoute-Direct-ckt
  ResourceGroupName                : Contoso-Direct-rg
  Location                         : westcentralus
  Id                               : /subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Netwo
                                   rk/expressRouteCircuits/ExpressRoute-Direct-ckt
  Etag                             : W/"<etagnumber>"
  ProvisioningState                : Succeeded
  Sku                              : {
                                     "Name": "Premium_MeteredData",
                                     "Tier": "Premium",
                                     "Family": "MeteredData"
                                   }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
    ServiceProviderProperties        : null
  ExpressRoutePort                 : {
                                     "Id": "/subscriptions/<subscriptionID>n/resourceGroups/Contoso-Direct-rg/providers/Micros
                                   oft.Network/expressRoutePorts/Contoso-Direct"
                                   }
  BandwidthInGbps                  : 10
  Stag                             : 2
  ServiceKey                       : <number>
  Peerings                         : []
  Authorizations                   : []
  AllowClassicOperations           : False
  GatewayManagerEtag     
  ```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute Direct hakkında daha fazla bilgi için bkz. [genel bakış](expressroute-erdirect-about.md).
