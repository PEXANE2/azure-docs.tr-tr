---
title: Azure'da sanal ağdan SAP HANA'ya ayarlanan bağlantı (büyük örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA'yı kullanmak için sanal ağdan ayarlanmış bağlantı (büyük örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f7ac8e69c4e149fdd0f365e19f7a0282a547af43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617194"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Sanal ağı HANA'nın büyük örneklerine bağlayın

Bir Azure sanal ağı oluşturduktan sonra, azure büyük örneklerinde bu ağı SAP HANA'ya bağlayabilirsiniz. Sanal ağda bir Azure ExpressRoute ağ geçidi oluşturun. Bu ağ geçidi, sanal ağı HANA Büyük Örnek damgası üzerindeki müşteri kiracısına bağlanan ExpressRoute devresine bağlamanızı sağlar.

> [!NOTE] 
> Bu adımın tamamlanması 30 dakika kadar sürebilir. Yeni ağ geçidi, belirlenen Azure aboneliğinde oluşturulur ve daha sonra belirtilen Azure sanal ağına bağlanır.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Bir ağ geçidi zaten varsa, expressroute ağ geçidi olup olmadığını kontrol edin. ExpressRoute ağ geçidi değilse, ağ geçidini silin ve ExpressRoute ağ geçidi olarak yeniden oluşturun. Bir ExpressRoute ağ geçidi zaten kurulmuşsa, bu makalenin aşağıdaki bölümüne bakın: "Sanal ağları bağla." 

- Sanal ağınıza bağlı bir ExpressRoute VPN ağ geçidi oluşturmak için [Azure portalını](https://portal.azure.com/) veya PowerShell'i kullanın.
  - Azure portalını kullanıyorsanız, yeni bir **Sanal Ağ Ağ Geçidi**ekleyin ve ardından ağ geçidi türü olarak **ExpressRoute'u** seçin.
  - PowerShell kullanıyorsanız, ilk olarak en son [Azure PowerShell SDK'yı](https://azure.microsoft.com/downloads/)indirip kullanın. 
 
Aşağıdaki komutlar bir ExpressRoute ağ geçidi oluşturur. Bir önceki _$_ metinler, özel bilgilerinizle güncelleştirilmesi gereken kullanıcı tanımlı değişkenlerdir.

```powershell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA large instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Bu örnekte, HighPerformance ağ geçidi SKU kullanılmıştır. Seçenekleriniz, Azure'da SAP HANA için desteklenen tek ağ geçidi SUS'ları olarak HighPerformance veya UltraPerformance'dır (büyük örnekler).

> [!IMPORTANT]
> Tip II sınıfı SKU'nun HANA büyük örnekleri için UltraPerformance Ağ Geçidi SKU'yu kullanmanız gerekir.

## <a name="link-virtual-networks"></a>Sanal ağları birbirine bağla

Azure sanal ağının artık bir ExpressRoute ağ geçidi vardır. ExpressRoute ağ geçidini SAP HANA Büyük Örnekler ExpressRoute devresine bağlamak için Microsoft tarafından sağlanan yetkilendirme bilgilerini kullanın. Azure portalını veya PowerShell'i kullanarak bağlanabilirsiniz. PowerShell talimatları aşağıdaki gibidir. 

Her bağlantı için farklı bir AuthGUID kullanarak her ExpressRoute ağ geçidi için aşağıdaki komutları çalıştırın. Aşağıdaki komut dosyasında gösterilen ilk iki giriş Microsoft tarafından sağlanan bilgilerden gelir. Ayrıca, AuthGUID her sanal ağ ve ağ geçidi için özeldir. Başka bir Azure sanal ağı eklemek istiyorsanız, HANA'nın büyük örneklerini Microsoft'tan Azure'a bağlayan ExpressRoute devreniz için başka bir AuthID almanız gerekir. 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

> [!NOTE]
> New-AzVirtualNetworkGatewayConnection komutundaki son parametre olan **ExpressRouteGatewayBypass,** ExpressRoute Hızlı Yol'u sağlayan yeni bir parametredir. HANA Büyük Örnek birimleriniz ve Azure VM'leriniz arasındaki ağ gecikmesüresini azaltan bir işlev. İşlevsellik Mayıs 2019'da eklendi. Daha fazla ayrıntı için, makale [SAP HANA (Büyük Örnekler) ağ mimarisini kontrol edin.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture) Komutları çalıştırmadan önce PowerShell cmdlets'in en son sürümünü çalıştırdığınızdan emin olun.

Ağ geçidini aboneliğinizle ilişkili birden fazla ExpressRoute devresine bağlamak için bu adımı birden çok kez çalıştırmanız gerekebilir. Örneğin, sanal ağı şirket içi ağınıza bağlayan ExpressRoute devresine aynı sanal ağ ağ geçidini bağlayabilirsiniz.

## <a name="applying-expressroute-fast-path-to-existing-hana-large-instance-expressroute-circuits"></a>Mevcut HANA Büyük Örnek ExpressRoute devrelerine ExpressRoute Hızlı Yol Uygulama
Şimdiye kadarki belgeler, HANA Büyük Örnek dağıtımıyla oluşturulan yeni bir ExpressRoute devresinin Azure sanal ağlarınızdan birinin Azure ExpressRoute ağ geçidine nasıl bağlanılabildiğini açıkladı. Ama birçok müşteri zaten kendi ExpressRoute devreleri kurulum var ve sanal ağları ZATEN HANA Büyük Örnekleri bağlı. Yeni ExpressRoute Hızlı Yolu ağ gecikmesini azalttığından, bu işlevi kullanmak için değişikliği uygulamanız önerilir. Yeni bir ExpreesRoute devresini bağlamak ve varolan bir ExpressRoute Circuit'i değiştirmek için komutlar aynıdır. Sonuç olarak, mevcut bir devreyi kullanmak için değiştirmek için bu PowerShell komutları dizisini çalıştırmanız gerekir 

```powershell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

ExpressRoute Hızlı Yol işlevini etkinleştirmek için yukarıda gösterildiği gibi son parametreyi eklemeniz önemlidir


## <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
İki senaryodan biri veya her ikisi için Global Reach'i etkinleştirmek istediğinizde:

 - Herhangi bir ek yakınlık veya güvenlik duvarı olmadan HANA Sistem Çoğaltma
 - Sistem kopyalarını veya sistem yenilemelerini gerçekleştirmek için iki farklı bölgede HANA Büyük Örnek birimleri arasında yedekleme kopyalama

bunu göz önünde bulundurmanız gerekir:

- Bir /29 adres alanı nın adres alanı aralığısağlamanız gerekir. Bu adres aralığı, HANA Büyük Örnekleri Azure'a bağlayan şimdiye kadar kullandığınız diğer adres alanı aralıklarıyla çakışmayabilir ve Azure'da veya şirket içinde başka bir yerde kullandığınız IP adresi aralıklarının herhangi biriyle çakışmayabilir.
- HANA Büyük Örnekleri'ne şirket içi rotalarınızın reklamını yapmak için kullanılabilecek ASN'lerde (Özerk Sistem Numarası) bir sınırlama vardır. Şirket arkadaşlarınız 65000 - 65020 veya 65515 aralığında özel ASN'ler içeren herhangi bir güzergahın reklamını yapmamalıdır. 
- Şirket içi doğrudan erişimi HANA Büyük örneklerine bağlama senaryosu için, sizi Azure'a bağlayan devre için bir ücret hesaplamanız gerekir. Fiyatlar için [Global Reach Eklentisi](https://azure.microsoft.com/pricing/details/expressroute/)fiyatlarını kontrol edin.

Dağıtımınıza uygulanan senaryolardan birini veya her ikisini almak [için, HANA büyük Örnekleri için bir destek isteği aç'ta](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#open-a-support-request-for-hana-large-instances) açıklandığı gibi Azure ile bir destek iletisi açın

İsteğiniz üzerine yönlendirme ve yürütebilmesi için Microsoft'un kullanması gereken veriler ve kullanmanız gereken anahtar kelimeler şu şekilde görünür:

- Hizmet: SAP HANA Büyük Örnek
- Sorun türü: Yapılandırma ve Kurulum
- Sorun alt türü: Sorunum yukarıda listelenmez
- Konu 'Ağımı Değiştir - Global Reach ekle'
- Ayrıntılar: 'HANA Büyük Örnek'e Global Reach'i HANA Büyük Örnek kiracısına ekleyin veya 'HANA Büyük Örnek kiracısına şirket içi Global Reach ekleyin.
- HANA Büyük Örnek'ten HANA Büyük Örnek'e kiracı durumu için ek ayrıntılar: Bağlanacak iki kiracının bulunduğu **iki Azure bölgesini** tanımlamanız **ve** **/29 IP adres aralığını** göndermeniz gerekir
- HANA Büyük Örnek kiracı kasasına şirket içi için ek ayrıntılar: Doğrudan bağlanmak istediğiniz HANA Büyük Örnek kiracısının dağıtıldığı **Azure Bölgesini** tanımlamanız gerekir. Ayrıca, ExpressRoute devrenizi şirket içi ve Azure arasında oluşturduğunuzda aldığınız **Auth GUID** ve **Circuit Peer KIMLIĞINI** sağlamanız gerekir. Ayrıca, **ASN**adlandırmanız gerekir. Son teslim ExpressRoute Global Reach için **bir /29 IP adres aralığıdır.**

> [!NOTE]
> Her iki servis inin de ele alınmasını istiyorsanız, şimdiye kadar kullanılan diğer IP adresi aralıklarıyla çakışmayan iki farklı /29 IP adresi aralığı sağlamanız gerekir. 




## <a name="next-steps"></a>Sonraki adımlar

- [HLI için ek ağ gereksinimleri](hana-additional-network-requirements.md)
