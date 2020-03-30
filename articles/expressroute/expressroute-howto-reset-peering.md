---
title: 'Azure ExpressRoute: Devre eşlemi sıfırlama'
description: ExpressRoute devre si izlemelerini devre dışı bırakıp etkinleştirme.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 01/13/2018
ms.author: charwen
ms.openlocfilehash: 9f32eb439872de9e4687d046745c03bafd86b2fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941749"
---
# <a name="reset-expressroute-circuit-peerings"></a>ExpressRoute devre eşlemelerini sıfırla

Bu makalede, PowerShell kullanarak bir ExpressRoute devresinin eşlemelerinin nasıl devre dışı sürülebileceği ve etkinleştirilen anlatılmaktadır. Bir eşlemi devre dışı kaldığınız zaman, ExpressRoute devrenizin hem birincil bağlantısı hem de ikincil bağlantısındaki BGP oturumu kapatılır. Microsoft'a bu bakış la bağlantı kaybedersiniz. Bir eşlemi etkinleştirdiğinizde, ExpressRoute devrenizin hem birincil bağlantısı hem de ikincil bağlantısı ndaki BGP oturumu açılır. Microsoft'a bu bakışla bağlantıyı yeniden kazanırsınız. Bir ExpressRoute devresi üzerinde Microsoft Peering ve Azure Özel Eşleme'yi bağımsız olarak etkinleştirebilir ve devre dışı kullanabilirsiniz. ExpressRoute devrenizdeki eşlemeleri ilk yapılandırırken, eşlemeler varsayılan olarak etkinleştirilir.

ExpressRoute bakışlarınızı sıfırlamayı yararlı bulabileceğiniz birkaç senaryo vardır.
* Olağanüstü durum kurtarma tasarımınızı ve uygulamanızı test edin. Örneğin, iki ExpressRoute devreniz vardır. Bir devrenin eşlemelerini devre dışı bırakıp ağ trafiğinizi diğer devreüzerinde başarısız olmaya zorlayabilirsiniz.
* Azure Özel Bakışveya ExpressRoute devrenizin Microsoft Peering'inde Çift Yönlü Yönlendirme Algılamasını (BFD) etkinleştirin. ExpressRoute devreniz 1 Ağustos 2018'den sonra oluşturulursa Azure Özel Eşleme'de ve ExpressRoute devreniz 10 Ocak 2020'den sonra oluşturulacaksa Microsoft Peering'de VARSAYıLAN olarak BFD etkinleştirilir. Devreniz bundan önce oluşturulduysa, BFD etkinleştirilememişti. BFD'yi, eşlemesini devre dışı bırakarak ve yeniden etkinleştirerek etkinleştirebilirsiniz. 

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="reset-a-peering"></a>Bir eşleme sıfırlama

1. PowerShell'i yerel olarak çalıştırıyorsanız, PowerShell konsolunuzu yüksek ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```azurepowershell
   Connect-AzAccount
   ```
2. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
3. Kullanmak istediğiniz aboneliği belirtin.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```
4. ExpressRoute devrenizi almak için aşağıdaki komutları çalıştırın.

   ```azurepowershell-interactive
   $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```
5. Devre dışı kalmak veya etkinleştirmek istediğiniz eşlemi tanımlayın. *Peerings* bir dizidir. Aşağıdaki örnekte, Peerings[0] Azure Özel Eşleme ve Peerings[1] Microsoft Peering olduğunu.

   ```azurepowershell-interactive
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
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
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
   ServiceKey                       : ########-####-####-####-############
   Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
   Authorizations                   : []
   AllowClassicOperations           : False
   GatewayManagerEtag               :
   ```
6. Bakanların durumunu değiştirmek için aşağıdaki komutları çalıştırın.

   ```azurepowershell-interactive
   $ckt.Peerings[0].State = "Disabled"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```
   Bakış, belirlediğiniz bir durumda olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar
Bir ExpressRoute sorununu gidermek için yardıma ihtiyacınız varsa, aşağıdaki makalelere göz atın:
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Ağ performansı sorunlarını giderme](expressroute-troubleshooting-network-performance.md)
