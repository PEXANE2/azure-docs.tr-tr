---
title: Azure ExpressRoute genel bakış oluşturma ve yönetme
description: Azure genel ekiylemeyi öğrenin ve yönetin
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: cherylmc
ms.openlocfilehash: 8c1afac834fb9abb2cbf82f16f046a1624b251f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481142"
---
# <a name="create-and-manage-expressroute-public-peering"></a>ExpressRoute genel bakış oluşturma ve yönetme

> [!div class="op_single_selector"]
> * [Madde - Kamu bakan](about-public-peering.md)
> * [Video - Genel bakış](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Madde - Microsoft bakan](expressroute-circuit-peerings.md#microsoftpeering)
>

Bu makale, ExpressRoute devresi için genel eşleme yönlendirme yapılandırması oluşturmanıza ve yönetmenize yardımcı olur. Ayrıca durumu denetleyebilir, güncelleyebilir veya silebilir ve eşleri yok edebilirsiniz. Bu makale, kamu hakemliği amortismana gitmeden önce oluşturulan Kaynak Yöneticisi devreleri için geçerlidir. Önceden varolan bir devreniz varsa (genel bakışlar amortismana alınmadan önce oluşturulmuşsa), [Azure PowerShell](#powershell), [Azure CLI](#cli)ve [Azure portalını](#portal)kullanarak genel bakışları yönetebilir/yapılandırabilirsiniz.

>[!NOTE]
>Kamu yada bakmak küçümsüyor. Yeni ExpressRoute devrelerinde herkese açık bir bakış oluşturamazsınız. Bunun yerine yeni bir ExpressRoute devreniz varsa, Azure hizmetleriniz için [Microsoft'u gözden](expressroute-circuit-peerings.md#microsoftpeering) kullanabilirsiniz.
>

## <a name="connectivity"></a>Bağlantı

Bağlantı her zaman WAN'ınızdan Microsoft Azure hizmetlerine başlatılır. Microsoft Azure hizmetleri, bu yönlendirme etki alanı üzerinden ağınıza bağlantı başlatamaz. ExpressRoute devreniz Azure genel bakışları için etkinleştirildiyse, devre üzerinden [Azure'da kullanılan genel IP aralıklarına](../virtual-network/virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) erişebilirsiniz.

Herkese açık bir şekilde bakma özelliği etkinleştirildikten sonra, çoğu Azure hizmetine bağlanabilirsiniz. Rotaların reklamını yaptığımız hizmetleri seçici olarak seçmene izin vermiyoruz.

* Azure Depolama, SQL Veritabanları ve Web Siteleri gibi hizmetler genel IP adreslerinde sunulur.
* Genel eşleme yönlendirme etki alanı aracılığıyla, bulut hizmetlerinizin VIP'leri de dahil olmak üzere genel IP adreslerinde barındırılan hizmetlere özel olarak bağlanabilirsiniz.
* Ortak bakış alan adını DMZ'nize bağlayabilir ve internet üzerinden bağlanmak zorunda kalmadan WAN'ınızdaki herkese açık IP adreslerindeki tüm Azure hizmetlerine bağlanabilirsiniz.

## <a name="services"></a><a name="services"></a>Hizmetleri

Bu bölümde, genel bakış üzerinden kullanılabilir hizmetleri gösterir. Genel bakışlar amortismana uygun olduğundan, genel eşlemelere yeni veya ek hizmetler ekleme planı yoktur. Herkese açık bir bakış kullanıyorsanız ve kullanmak istediğiniz hizmet yalnızca Microsoft'un bakışları üzerinden desteklenmişse, Microsoft'a geçmeniz gerekir. Desteklenen hizmetlerin listesi için [Microsoft'un bakışını](expressroute-faqs.md#microsoft-peering) görün.

**Desteklenen:**

* Power BI
* Azure hizmetlerinin çoğu desteklenir. Desteği doğrulamak için kullanmak istediğiniz hizmetle doğrudan kontrol edin.

**Desteklenmiyor:**
  * CDN
  * Azure Front Door
  * Çok faktörlü Kimlik Doğrulama Sunucusu (eski)
  * Traffic Manager

Belirli bir hizmetin kullanılabilirliğini doğrulamak için, bu hizmet için ayrılmış bir aralık olup olmadığını görmek için bu hizmetin belgelerini denetleyebilirsiniz. Daha sonra hedef hizmetin IP aralıklarına bakabilir [ve Azure IP Aralıkları ve Hizmet Etiketleri – Public Cloud XML dosyasında](https://www.microsoft.com/download/details.aspx?id=56519)listelenen aralıklarla karşılaştırabilirsiniz. Alternatif olarak, açıklama için söz konusu hizmet için bir destek bileti açabilirsiniz.

## <a name="peering-comparison"></a><a name="compare"></a>Eşleme karşılaştırması

[!INCLUDE [peering comparison](../../includes/expressroute-peering-comparison.md)]

> [!NOTE]
> Azure genel bakışlarının her BGP oturumuyla ilişkili 1 NAT IP adresi vardır. 2'den fazla NAT IP adresi için Microsoft'a bakın. Microsoft'un bakışları, kendi NAT ayırmalarınızı yapılandırmanıza ve seçici önek reklamlar için rota filtrelerini kullanmanıza olanak tanır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/expressroute/how-to-move-peering)
>

## <a name="custom-route-filters"></a>Özel rota filtreleri

Yalnızca gereksinim duyduğunuz yolları tüketmek için ağınızdaözel rota filtreleri tanımlayabilirsiniz. Yönlendirme yapılandırması hakkında ayrıntılı bilgi için [Yönlendirme](expressroute-routing.md) sayfasına bakın.

## <a name="azure-powershell-steps"></a><a name="powershell"></a>Azure PowerShell adımları


[!INCLUDE [CloudShell](../../includes/expressroute-cloudshell-powershell-about.md)]

Genel bakışlar amortismana hazır olduğundan, yeni bir ExpressRoute devresi üzerinde genel bakışları yapılandıramazsınız.

1. Sağlanan ve aynı zamanda etkin bir ExpressRoute devreniz olduğunu doğrulayın. Şu örneği kullanın:

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   Yanıt aşağıdaki örneğe benzer:

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
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
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
2. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Daha fazla ilerlemeden önce aşağıdaki bilgilere sahip olduğundan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * İsteğe bağlı:
   * Kullanmayı seçerseniz bir MD5 karma değeri.

   Devreniz için Azure genel eşlemi yapılandırmak için aşağıdaki örneği çalıştırın

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   BIR MD5 karma kullanmayı seçerseniz, aşağıdaki örneği kullanın:

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
   > 
   >

### <a name="to-get-azure-public-peering-details"></a><a name="getpublic"></a>Azure genel bakış ayrıntılarını elde etmek için

Aşağıdaki cmdlet kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```azurepowershell-interactive
  $ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="to-update-azure-public-peering-configuration"></a><a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Yapılandırmanın herhangi bir bölümünü aşağıdaki örneği kullanarak güncelleştirebilirsiniz. Bu örnekte, devrenin VLAN kimliği 200'den 600'e güncellenmektedir.

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-public-peering"></a><a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Aşağıdaki örneği çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-cli-steps"></a><a name="cli"></a>Azure CLI adımları


[!INCLUDE [CloudShell](../../includes/expressroute-cloudshell-powershell-about.md)]

1. Sağlandığından ve etkinleştirildiğinden emin olmak için ExpressRoute devresini kontrol edin. Şu örneği kullanın:

   ```azurecli-interactive
   az network express-route list
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
   "serviceProviderProvisioningState": "Provisioned",
   "sku": {
    "family": "UnlimitedData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
   },
   "tags": null,
   "type": "Microsoft.Network/expressRouteCircuits]
   ```

2. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Daha fazla ilerlemeden önce aşağıdaki bilgilere sahip olduğundan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * **İsteğe bağlı -** Bir kullanmayı seçerseniz bir MD5 karma.

   Devreniz için Azure genel eşlemi yapılandırmak için aşağıdaki örneği çalıştırın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering
   ```

   BIR MD5 karma kullanmayı seçerseniz, aşağıdaki örneği kullanın:

   ```azurecli-interactive
   az network express-route peering create --circuit-name MyCircuit --peer-asn 100 --primary-peer-subnet 12.0.0.0/30 -g ExpressRouteResourceGroup --secondary-peer-subnet 12.0.0.4/30 --vlan-id 200 --peering-type AzurePublicPeering --SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.

### <a name="to-view-azure-public-peering-details"></a><a name="getpublic"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Aşağıdaki örneği kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```azurecli
az network express-route peering show -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

Çıktı aşağıdaki örneğe benzer:

```output
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

### <a name="to-update-azure-public-peering-configuration"></a><a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Yapılandırmanın herhangi bir bölümünü aşağıdaki örneği kullanarak güncelleştirebilirsiniz. Bu örnekte, devrenin VLAN kimliği 200'den 600'e güncellenmektedir.

```azurecli-interactive
az network express-route peering update --vlan-id 600 -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

### <a name="to-delete-azure-public-peering"></a><a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Aşağıdaki örneği çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```azurecli-interactive
az network express-route peering delete -g ExpressRouteResourceGroup --circuit-name MyCircuit --name AzurePublicPeering
```

## <a name="azure-portal-steps"></a><a name="portal"></a>Azure portal adımları

Eşlemi yapılandırmak için bu makalede yer alan PowerShell veya CLI adımlarını kullanın. Bir eşlemi yönetmek için aşağıdaki bölümleri kullanabilirsiniz. Başvuru için, bu adımlar [portalda](expressroute-howto-routing-portal-resource-manager.md#msft)bir Microsoft bakan yönetme benzer.

### <a name="to-view-azure-public-peering-details"></a><a name="get"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Portaldaki eşlemi seçerek Azure genel bakış özelliklerini görüntüleyin.

### <a name="to-update-azure-public-peering-configuration"></a><a name="update"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçin ve ardından bakan özellikleri değiştirin.

### <a name="to-delete-azure-public-peering"></a><a name="delete"></a>Azure ortak eşlemesini silmek için

Silme simgesini seçerek eşleme yapılandırmanızı kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [Bir ExpressRoute devresine sanal ağ bağla.](expressroute-howto-linkvnet-arm.md)

* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).