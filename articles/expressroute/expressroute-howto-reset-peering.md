---
title: 'Azure ExpressRoute: devre eşlemesini sıfırlama'
description: ExpressRoute devre eşayarlarını devre dışı bırakma ve etkinleştirme.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 01/13/2018
ms.author: charwen
ms.openlocfilehash: cc4266a92d510ac84118922f7db09c99234045b3
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84735260"
---
# <a name="reset-expressroute-circuit-peerings"></a>ExpressRoute devre eşayarlarını sıfırlama

Bu makalede, PowerShell kullanarak bir ExpressRoute bağlantı hattının eşayarlarını devre dışı bırakma ve etkinleştirme açıklanmaktadır. Bir eşlemeyi devre dışı bıraktığınızda, hem birincil bağlantıda BGP oturumu hem de ExpressRoute devreniz için ikincil bağlantı kapatılır. Bu eşleme ile Microsoft 'a bağlantıyı kaybedeceksiniz. Bir eşlemeyi etkinleştirdiğinizde, hem birincil bağlantıda BGP oturumu hem de ExpressRoute devrenizin ikincil bağlantısı oluşturulur. Bağlantıyı bu eşleme ile Microsoft 'a geri kazanacaksınız. ExpressRoute bağlantı hattındaki Microsoft eşlemesini ve Azure özel eşlemesini bağımsız olarak etkinleştirebilir ve devre dışı bırakabilirsiniz. ExpressRoute bağlantı hattınızı ilk kez yapılandırdığınızda, eşlemeler varsayılan olarak etkindir.

ExpressRoute eşlerinizi sıfırlamanızı yararlı bulabileceğiniz birkaç senaryo vardır.
* Olağanüstü durum kurtarma tasarımınızı ve uygulamanızı test edin. Örneğin, iki ExpressRoute devreniz vardır. Bir devrenin eşayarlarını devre dışı bırakabilir ve ağ trafiğinizi diğer devreye devretmek için zorlayabilirsiniz.
* Azure özel eşleme veya ExpressRoute devrenizin Microsoft eşlemesi üzerinde çift yönlü Iletme algılamasını (BFD) etkinleştirin. ExpressRoute bağlantı hattı 10 2020 ' den sonra, ExpressRoute bağlantı hattı 1 2018 Ağustos 'Tan sonra oluşturulduysa, BFD varsayılan olarak Azure özel eşleme 'de etkinleştirilir. Devreniz daha önce oluşturulduysa BFD etkin değildi. Eşlemeyi devre dışı bırakarak ve yeniden etkinleştirerek BFD 'yi etkinleştirebilirsiniz. 

### <a name="working-with-azure-powershell"></a>Azure PowerShell çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="reset-a-peering"></a>Eşlemeyi sıfırlama

1. PowerShell 'i yerel olarak çalıştırıyorsanız, PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

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
5. Devre dışı bırakmak veya etkinleştirmek istediğiniz eşlemeyi belirler. *Peerings* Eşlemeler bir dizidir. Aşağıdaki örnekte, [0] eşlemeleri Azure özel eşleme ve eşleme [1] Microsoft eşlemesi.

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
6. Eşleme durumunu değiştirmek için aşağıdaki komutları çalıştırın.

   ```azurepowershell-interactive
   $ckt.Peerings[0].State = "Disabled"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```
   Eşleme, ayarladığınız bir durumda olmalıdır. 

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute sorununu gidermeye yönelik yardıma ihtiyacınız varsa aşağıdaki makalelere göz atın:
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Ağ performansı sorunlarını giderme](expressroute-troubleshooting-network-performance.md)
