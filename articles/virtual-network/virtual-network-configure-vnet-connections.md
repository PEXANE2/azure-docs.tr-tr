---
title: Sanal ağ veya VPN bağlantılarını yapılandırma ve doğrulama
description: Çeşitli Azure VPN ve sanal ağ dağıtımlarını yapılandırmak ve doğrulamak için adım adım kılavuz
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 0433a4f4-b5a0-476d-b398-1506c57eafa2
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: dddf402455292e19bf0fcda3c50d9ce10d5888d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71099069"
---
# <a name="configure-and-validate-virtual-network-or-vpn-connections"></a>Sanal ağ veya VPN bağlantılarını yapılandırma ve doğrulama

Bu iz, çeşitli Azure VPN ve sanal ağ dağıtımlarını yapılandırmak ve doğrulamak için adım adım kılavuzluk sağlar. Senaryolar arasında aktarım yönlendirme, ağdan ağa bağlantılar, Kenarlık Ağ Geçidi Protokolü (BGP), çok siteli bağlantılar ve noktadan siteye bağlantılar yer almaktadır.

Azure VPN ağ geçitleri, Azure'da hemen hemen her tür bağlı sanal ağ topolojisini düzenlemede esneklik sağlar. Örneğin, sanal ağları bağlayabilirsiniz:

- Bölgeler arasında.
- Sanal ağ türleri arasında (Azure Kaynak Yöneticisi ile klasik) arasında.
- Azure'da veya şirket içi karma ortam içinde.
- Farklı aboneliklerde. 

## <a name="network-to-network-vpn-connection"></a>Ağdan ağa VPN bağlantısı

Sanal bir ağı VPN üzerinden başka bir sanal ağa (ağdan ağa) bağlamak, sanal ağı şirket içi bir site konumuna bağlamaya benzer. Her iki bağlantı türü de IPsec ve IKE üzerinden güvenli bir tünel sağlamak için bir VPN ağ geçidi kullanır. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler.

![IPsec ile ağdan ağa bağlantı](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Sanal ağlarınız aynı bölgedeyse, sanal ağ eşlemeyi kullanarak bunları bağlamayı düşünebilirsiniz. Sanal ağ eşleme vpn ağ geçidi kullanmaz. İş bilgililiği artırır ve gecikme süresini azaltır. Sanal ağ eşleme bağlantısını yapılandırmak **için, VNet Peering'i yapılandır'ı ve doğrulat'ı**seçin.

Sanal ağlarınız Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulduysa, VPN bağlantısını yapılandırmak **için Kaynak Yöneticisi VNet bağlantısını Yapılandır'ı ve doğrula'yı** seçin.

Sanal ağlardan biri Azure klasik dağıtım modeli aracılığıyla oluşturulduysa ve diğeri Kaynak Yöneticisi aracılığıyla oluşturulduysa, VPN bağlantısını yapılandırmak **için klasik bir VNet'i Kaynak Yöneticisi VNet bağlantısına yapılandır'ı seçin ve doğrulayın.**

### <a name="configure-virtual-network-peering-for-two-virtual-networks-in-the-same-region"></a>Aynı bölgedeki iki sanal ağ için sanal ağ eşlemesini yapılandırma

Azure sanal ağ eşlemesini uygulamaya ve yapılandırmaya başlamadan önce aşağıdaki ön koşulları yerine getirebildiğinizden emin olun:

* Eşlenmiş sanal ağlar aynı Azure bölgesinde bulunmalıdır.
* Eşlenen sanal ağlar, çakışan IP adresi boşluklarına sahip olmalıdır.
* Sanal ağ eşlemesi iki sanal ağ arasında gerçekleşir. Akranlar arasında türemiş geçişli bir ilişki yoktur. Örneğin, VNetA VNetB ile eşlenirve VNetB VNetC ile eşlenirse, VNetA VNetC ile *eşlenmez.*

Gereksinimleri karşılarken, Öğretici: Sanal [ağ la sanal ağ ları birleştirin, azure portalını kullanarak](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) eşlemi oluşturup yapılandırabilirsiniz.

Eşleme yapılandırmasını denetlemek için aşağıdaki yöntemi kullanın:

1. Gerekli [rolleri ve izinleri](virtual-network-manage-peering.md#permissions)olan bir hesap kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Portalın üst kısmındaki metin **Arama kaynaklarını** içeren kutuya **sanal ağlar**yazın. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
3. Görünen **Sanal ağlar** bıçakta, bir eşleme oluşturmak istediğiniz sanal ağı seçin.
4. Sanal ağ için görünen bölmede **Ayarlar** bölümünde **Eşler'i** seçin.
5. Bir görünüm seçin ve yapılandırma sonuçlarını görüntüleyin.

![Sanal ağ eşleme yapılandırmasını denetlemek için seçimler](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
Azure PowerShell için, sanal ağ eşleneme sini elde etmek için [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) komutunu çalıştırın. Bir örneği aşağıda verilmiştir:

```
PS C:\Users\User1> Get-AzureRmVirtualNetworkPeering -VirtualNetworkName Vnet10-01 -ResourceGroupName dev-vnets
Name                             : LinkToVNET10-02
Id                               : /subscriptions/GUID/resourceGroups/dev-vnets/providers/Microsoft.Network/virtualNetworks/VNET10-01/virtualNetworkPeerings/LinkToVNET10-0
2
Etag                             : W/"GUID"
ResourceGroupName                : dev-vnets
VirtualNetworkName               : vnet10-01
ProvisioningState                : Succeeded
RemoteVirtualNetwork             : {
                                  "Id": "/subscriptions/GUID/resourceGroups/DEV-VNET
                                   s/providers/Microsoft.Network/virtualNetworks/VNET10-02"
                                   }
AllowVirtualNetworkAccess        : True
AllowForwardedTraffic            : False
AllowGatewayTransit              : False
UseRemoteGateways                : False
RemoteGateways                   : null
RemoteVirtualNetworkAddressSpace : null
```

### <a name="connect-a-resource-manager-virtual-network-to-another-resource-manager-virtual-network"></a>Kaynak Yöneticisi sanal ağını başka bir Kaynak Yöneticisi sanal ağa bağlama

Bir Kaynak Yöneticisi sanal ağdan doğrudan başka bir Kaynak Yöneticisi sanal ağa bağlantı yapılandırabilirsiniz. Veya IPsec kullanarak bağlantıyı yapılandırabilirsiniz.

### <a name="configure-a-vpn-connection-between-resource-manager-virtual-networks"></a>Kaynak Yöneticisi sanal ağlar arasında VPN bağlantısı yapılandırma

IPsec olmadan Kaynak Yöneticisi sanal ağlar arasında bir bağlantı yapılandırmak için, [Azure portalını kullanarak ağdan ağa VPN ağ geçidi bağlantısını yapılandırma'ya](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)bakın.

İki Kaynak Yöneticisi sanal ağı arasında IPsec ile bağlantı yapılandırmak için, her sanal ağ için [Azure portalında siteden siteye bağlantı oluşturma'da](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) 1 ile 5 arasındaki adımları izleyin.

> [!Note]
> Bu adımlar yalnızca aynı abonelikteki sanal ağlar için çalışır. Sanal ağlarınız farklı aboneliklerdeyse, bağlantıyı yapmak için PowerShell'i kullanmanız gerekir. [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) makalesine bakın.

### <a name="validate-the-vpn-connection-between-resource-manager-virtual-networks"></a>Kaynak Yöneticisi sanal ağlar arasındaki VPN bağlantısını doğrulama

![Azure Kaynak Yöneticisi sanal ağına klasik sanal ağ bağlantısı](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

VPN bağlantınızın doğru yapılandırıldığından kontrol etmek için aşağıdaki yönergeleri izleyin.

> [!Note] 
> Bu adımlardaki sanal ağ bileşenlerinden sonraki sayılar, önceki diyagramdaki sayılara karşılık gelir.

1. Bağlı sanal ağlarda çakışan adres alanları olmadığından emin olun.
2. Azure Kaynak Yöneticisi sanal ağının (1) adres aralığının **Bağlantı nesnesi** örneğinde (4) doğru şekilde tanımlandığını doğrulayın.
3. Azure Kaynak Yöneticisi sanal ağının (6) adres aralığının **Bağlantı nesnesi** örneğinde (3) doğru şekilde tanımlandığını doğrulayın.
4. Önceden paylaşılan anahtarların bağlantı nesnelerinde eşleştindiğini doğrulayın.
5. Azure Kaynak Yöneticisi sanal ağ ağ geçidi VIP'nin (2) **Bağlantı nesnesi** örneğinde (4) doğru şekilde tanımlandığını doğrulayın.
6. Azure Kaynak Yöneticisi sanal ağ ağ geçidi VIP'nin (5) **Bağlantı nesnesi** örneğinde (3) doğru şekilde tanımlandığını doğrulayın.

### <a name="connect-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Klasik bir sanal ağı Kaynak Yöneticisi sanal ağına bağlama

Farklı aboneliklerde ve farklı bölgelerde bulunan sanal ağlar arasında bağlantı oluşturabilirsiniz. Ağ geçidi türünü rota tabanlı olarak yapılandırdığınız sürece, şirket içi ağlara zaten bağlantısı olan sanal ağları da bağlayabilirsiniz.

Klasik bir sanal ağ ile Kaynak Yöneticisi sanal ağı arasındaki bağlantıyı yapılandırmak için [Azure portalını kullanarak farklı dağıtım modellerinden sanal ağları bağlayın'a](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal)bakın.

![Azure Kaynak Yöneticisi sanal ağına klasik sanal ağ bağlantısı](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Klasik bir sanal ağı Azure Kaynak Yöneticisi sanal ağına bağladığınızda yapılandırmayı denetlemek için aşağıdaki yönergeleri izleyin.

> [!Note] 
> Bu adımlardaki sanal ağ bileşenlerinden sonraki sayılar, önceki diyagramdaki sayılara karşılık gelir. 

1. Bağlı sanal ağlarda çakışan adres alanları olmadığından emin olun.
2. Azure Kaynak Yöneticisi sanal ağının (6) adres aralığının klasik yerel ağ tanımında (3) doğru şekilde tanımlandığını doğrulayın.
3. Klasik sanal ağın (1) adres aralığının Azure Kaynak Yöneticisi **Bağlantısı nesne** örneğinde (4) doğru şekilde tanımlandığını doğrulayın.
4. Klasik sanal ağ ağ geçidi VIP'nin (2) Azure Kaynak Yöneticisi **Bağlantısı nesne** örneğinde (4) doğru şekilde tanımlandığını doğrulayın.
5. Azure Kaynak Yöneticisi sanal ağ ağ ağ geçidinin (5) klasik **Yerel Ağ Tanımı** örneğinde (3) doğru şekilde tanımlandığını doğrulayın.
6. Önceden paylaşılan anahtarların bağlı sanal ağlarda eşleşerek eşleştirdiğini doğrulayın:
   - Klasik sanal ağ: **Yerel Ağ Tanımı** (3)
   - Azure Kaynak Yöneticisi sanal ağ: **Bağlantı nesnesi** (4)

## <a name="create-a-point-to-site-vpn-connection"></a>Noktadan siteye VPN bağlantısı oluşturma

Bir noktadan siteye (Aşağıdaki diyagramda*P2S)* yapılandırma, tek bir istemci bilgisayardan sanal ağa güvenli bir bağlantı oluşturmanıza olanak tanır. Sanal ağınıza evden veya konferans gibi uzak bir konumdan bağlanmak istediğinizde, site den siteye bağlantı bağlantıları yararlıdır. Sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda da kullanışlıdır. 

Noktaya işaretVPN bağlantısı istemci bilgisayardan ana Windows VPN istemcisi üzerinden başlatılır. Bağlanan istemciler kimlik doğrulamak için sertifika kullanır.

![Noktadan siteye bağlantılar](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Noktadan siteye bağlantılar vpn aygıtı gerektirmez. Güvenli Soket Tünel Protokolü (SSTP) üzerinden VPN bağlantısı oluştururlar. Çeşitli dağıtım araçları ve dağıtım modelleri kullanarak sanal ağa noktadan siteye bağlantı kurabilirsiniz:

* [Azure portalını kullanarak sanal ağa noktadan siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Azure portalını (klasik) kullanarak sanal ağa noktadan siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [PowerShell'i kullanarak sanal ağa noktadan siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connection"></a>Noktadan siteye bağlantınızı doğrulama

Makale [Sorun Giderme: Azure noktadan siteye bağlantı sorunları,](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) site nin noktasındaki bağlantılarla ilgili sık karşılaşılan sorunlardan geçer.

## <a name="create-a-multisite-vpn-connection"></a>Çok siteli VPN bağlantısı oluşturma

Zaten siteden siteye bağlantısı, noktadan siteye bağlantısı veya ağdan ağa bağlantısı olan sanal ağa siteden siteye (Aşağıdaki diyagramda*S2S)* bağlantı ekleyebilirsiniz. Bu tür bir bağlantı genellikle *çok siteli* yapılandırma olarak adlandırılır. 

![Çok sitebağlantısı](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

Azure şu anda iki dağıtım modeliyle çalışır: Resource Manager ve klasik. İki model birbiriyle tamamen uyumlu değildir. Farklı modellerle çok siteli bir bağlantı yapılandırmak için aşağıdaki makalelere bakın:

* [Varolan VPN ağ geçidi bağlantısına sahip sanal ağa siteden siteye bağlantı ekleme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Varolan VPN ağ geçidi bağlantısına sahip sanal ağa siteden siteye bağlantı ekleme (klasik)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Bu makalelerdeki adımlar Azure ExpressRoute ve siteden siteye birlikte var olan bağlantı yapılandırmaları için geçerli değildir. Daha fazla bilgi için [ExpressRoute ve siteden siteye birlikte var olan bağlantılara](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager)bakın.

## <a name="configure-transit-routing"></a>Transit yönlendirmeyi yapılandırma

Geçiş yönlendirme, papatya zinciri topolojisinde birden çok ağı bağladığınızı gösteren belirli bir yönlendirme senaryosudur. Bu yönlendirme, zincirin her iki ucundaki sanal ağlardaki kaynakların aradaki sanal ağlar aracılığıyla birbirleriyle iletişim kurmasını sağlar. Geçiş yönlendirmesi olmadan, hub üzerinden bakan ağlar veya aygıtlar birbirine ulaşamaz.

### <a name="configure-transit-routing-in-a-point-to-site-connection"></a>Bir noktadan siteye bağlantıda geçiş yönlendirmesini yapılandırma

VNetA ve VNetB arasında siteden siteye VPN bağlantısını yapılandırmak istediğiniz bir senaryo düşünün. Ayrıca, istemcinin VNetA ağ geçidine bağlanması için bir noktadan siteye VPN yapılandırmak istiyorsunuz. Ardından, noktaya işaret istemcilerinin VNetA'dan geçen VNetB'ye bağlanması için geçiş yönlendirmesini etkinleştirmek istiyorsunuz. 

Bu senaryo, VNetA ve VNetB arasındaki siteden siteye VPN'de BGP etkinleştirildiğinde desteklenir. Daha fazla bilgi için bkz: [Sayfadan siteye VPN yönlendirmesi hakkında.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

### <a name="configure-transit-routing-in-an-expressroute-connection"></a>ExpressRoute bağlantısında geçiş rotasını yapılandırma

Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz. Daha fazla bilgi için bkz. [ExpressRoute'a genel bakış](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Azure sanal ağlarına ExpressRoute özel bakış bağlantısı](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

> [!Note]
> VNetA ve VNetB aynı jeopolitik bölgedeyse, geçiş yönlendirmesini yapılandırmak yerine [her iki sanal ağı da ExpressRoute devresine bağlamanızı](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm) öneririz. Sanal ağlarınız farklı jeopolitik bölgelerdeyse, [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium)Premium'unuz varsa doğrudan devrenize de bağlayabilirsiniz. 

ExpressRoute ve siteden siteye birlikte varsanız, geçiş yönlendirmesi desteklenmez. Daha fazla bilgi için [PowerShell'i kullanarak ExpressRoute'u ve siteden siteye yapılandır'a](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager)bakın.

ExpressRoute'un yerel ağlarınızı bir Azure sanal ağına bağlamasını sağladıysanız, geçiş yönlendirmesi yapmak istediğiniz sanal ağlar arasında bakmayı etkinleştirebilirsiniz. Yerel ağlarınızın uzak sanal ağa bağlanmasına izin vermek için [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity)yapılandırmanız gerekir. 

> [!Note]
> Sanal ağ eşlemi yalnızca aynı bölgedeki sanal ağlar için kullanılabilir.

Sanal ağ eşleme için geçiş yönlendirmesini yapılandırıp yapılandırmadığınızı denetlemek için aşağıdaki yönergeleri izleyin:

1. Gerekli [rolleri ve izinleri](virtual-network-manage-peering.md#permissions)olan bir hesap kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Önceki diyagramda gösterildiği gibi [VNetA ve VNetB arasında bir eşleme oluşturun.](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) 
3. Sanal ağ için görünen bölmede **Ayarlar** bölümünde **Eşler'i** seçin.
4. Görüntülemek istediğiniz eşlemi seçin. Ardından, ExpressRoute devresine bağlı VNetA ağında **Ağ Geçidi Geçişine İzin Ver'i** etkinleştirdiğinizi doğrulamak için **Yapılandırma'yı** seçin ve ExpressRoute devresine bağlı olmayan uzak VNetB ağında Uzak Ağ Geçidi'ni **kullanın.**

### <a name="configure-transit-routing-in-a-virtual-network-peering-connection"></a>Sanal ağ eşleme bağlantısında geçiş yönlendirmeyi yapılandırma

Sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Sanal ağ eşlemesinde bir geçiş rotasını yapılandırmak için [Ağdan ağa bağlantılara](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json)bakın.

> [!Note]
> Ağ geçidi geçişi, farklı dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasındaki eşleme ilişkisinde desteklenmez. Eşleme ilişkisindeki her iki sanal ağ da, ağ geçidi nin işe aktarılaması için Kaynak Yöneticisi aracılığıyla oluşturulmuş olmalıdır.

Sanal ağ eşleme için bir geçiş rotası yapılandırıp yapılandırmadığınızı denetlemek için aşağıdaki yönergeleri izleyin:

1. Gerekli [rolleri ve izinleri](virtual-network-manage-peering.md#permissions)olan bir hesap kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Portalın üst kısmındaki metin **Arama kaynaklarını** içeren kutuya **sanal ağlar**yazın. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
3. Görünen **Sanal ağlar** bıçakta, eşleme ayarını denetlemek istediğiniz sanal ağı seçin.
4. Seçtiğiniz sanal ağ için görünen bölmede **Ayarlar** bölümünde **Eşler'i** seçin.
5. Görüntülemek istediğiniz eşlemi seçin. **Ağ geçidi geçişine izin ver** ve **Yapılandırma**altında uzak **ağ geçitlerini kullan'ı** etkinleştirdiğinizi doğrulayın.

![Sanal ağ eşleme için bir geçiş rotası yapılandırdığınızı denetlemek için seçimler](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configure-transit-routing-in-a-network-to-network-connection"></a>Ağdan ağa bağlantıda geçiş yönlendirmesini yapılandırma

Sanal ağlar arasında geçiş yönlendirmesini yapılandırmak için, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak tüm ara ağdan ağa bağlantılarda BGP'yi etkinleştirmeniz gerekir. Talimatlar için [PowerShell'i kullanarak Azure VPN ağ geçitlerinde BGP'yi nasıl yapılandıracağınız](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps)abakın.

Azure VPN ağ geçitleri üzerinden transit trafik klasik dağıtım modeli aracılığıyla mümkündür, ancak bu ağ yapılandırma dosyasındaki statik olarak tanımlanmış adres boşluklarına dayanır. BGP henüz klasik dağıtım modeli aracılığıyla Azure sanal ağları ve VPN ağ geçitleriyle desteklenmemektedir. BGP olmadan, geçiş adresi alanlarını el ile tanımlamak hataya açıktır ve bunu önermiyoruz.

> [!Note]
> Azure klasik portalını kullanarak veya klasik portaldaki bir ağ yapılandırma dosyasını kullanarak klasik ağdan ağa bağlantıları yapılandırabilirsiniz. Azure Kaynak Yöneticisi dağıtım modeli veya Azure portalı aracılığıyla klasik bir sanal ağ oluşturamaz veya değiştiremezsiniz. Klasik sanal ağlar için geçiş yönlendirmesi hakkında daha fazla bilgi için [Microsoft Developer bloguna](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/)bakın.

### <a name="configure-transit-routing-in-a-site-to-site-connection"></a>Siteden siteye bağlantıda geçiş yönlendirmesini yapılandırma

Şirket içi ağınızla siteden siteye bağlantısı olan sanal ağ arasında geçiş yönlendirmesini yapılandırmak için, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak tüm ara site-site bağlantılarında BGP'yi etkinleştirmeniz gerekir. Talimatlar [için PowerShell'i kullanarak Azure VPN ağ geçitlerinde BGP'yi nasıl yapılandıracağınız](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps) abakalım.

Azure VPN ağ geçitleri üzerinden transit trafik klasik dağıtım modeli aracılığıyla mümkündür, ancak bu ağ yapılandırma dosyasındaki statik olarak tanımlanmış adres boşluklarına dayanır. BGP henüz klasik dağıtım modeli aracılığıyla Azure sanal ağları ve VPN ağ geçitleriyle desteklenmemektedir. BGP olmadan, geçiş adresi alanlarını el ile tanımlamak hataya açıktır ve bunu önermiyoruz.

> [!Note]
> Azure klasik portalını kullanarak veya klasik portaldaki bir ağ yapılandırma dosyasını kullanarak klasik siteden siteye bağlantıları yapılandırabilirsiniz. Azure Kaynak Yöneticisi dağıtım modeli veya Azure portalı aracılığıyla klasik bir sanal ağ oluşturamaz veya değiştiremezsiniz. Klasik sanal ağlar için geçiş yönlendirmesi hakkında daha fazla bilgi için [Microsoft Developer bloguna](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/)bakın.

## <a name="configure-bgp-for-a-vpn-gateway"></a>Bir VPN ağ geçidi için BGP yapılandırma

BGP, iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini değiştirmek için internette kullanılan standart yönlendirme protokolüdür. BGP Azure sanal ağları bağlamında kullanıldığında, Azure VPN ağ geçitlerini ve BGP eşarkadaşları veya komşuları olarak bilinen şirket içi VPN aygıtlarınızı etkinleştirirken. Bu öneklerin ağ geçitlerinden veya yönlendiricilerden geçmesi için kullanılabilirlik ve ulaşılabilirlik konusunda her iki ağ geçidini de bilgilendirecek "rotalar" alışverişinde bulunarak geçiş yaparlar. 

BGP ayrıca, bir BGP ağ geçidinin bir BGP eşlerinden diğer tüm BGP eşlerine öğrendiği yolları oluşturarak birden çok ağ arasında geçiş yönlendirmesini de etkinleştirebilir. Daha fazla bilgi için Azure [VPN Ağ Geçidi ile BGP'ye Genel Bakış](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview)bölümüne bakın.

### <a name="configure-bgp-for-a-vpn-connection"></a>VPN bağlantısı için BGP'yi yapılandırın

BGP kullanan bir VPN bağlantısını yapılandırmak [için PowerShell'i kullanarak Azure VPN ağ geçitlerinde BGP'yi nasıl yapılandırılabilirsiniz.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps)

Bunun için özerk bir sistem (AS) numarası oluşturarak sanal ağ ağ ağ geçidinde BGP'yi etkinleştirin. Temel ağ geçitleri BGP'yi desteklemez. Ağ geçidinin SKU'yu denetlemek için Azure portalındaki **VPN Ağ Geçidi** bıçağının Genel **Bakış** bölümüne gidin. SKU **Temel**ise, **VpnGw1**için SKU (ağ [geçidi resizing](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)bakınız) değiştirmek zorunda. 

SKU'nun kontrol edilmesi 20 ila 30 dakikalık bir kapalı kalma süresine neden olur. Ağ geçidi doğru SKU'ya sahip olur olmaz, [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) PowerShell komut izini kullanarak AS numarasını ekleyebilirsiniz. AS numarasını yapılandırdıktan sonra, ağ geçidi için bir BGP eş IP'si otomatik olarak sağlanır.

El ile bir `LocalNetworkGateway` AS numarası ve bir BGP eş adresi sağlamanız gerekir. Değerleri Yeni `ASN` [AzureRmLocalNetworkAğ Ağ Geçidi veya Set-AzureRmLocalNetworkPowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) komut unu kullanarak ayarlayabilirsiniz. [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) `-BgpPeeringAddress` Bazı AS numaraları Azure için ayrılmıştır ve [Azure VPN Ağ Geçidi ile BGP Hakkında'da](../vpn-gateway/vpn-gateway-bgp-overview.md#faq)açıklandığı gibi bunları kullanamazsınız.

Bağlantı nesnesi BGP etkin olmalıdır. `-EnableBGP` Değeri [New-AzureRmVirtualNetworkConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) veya [Set-AzureRmVirtualNetworkAğ Bağlantısı](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) `$True` ile ayarlayabilirsiniz.

### <a name="validate-the-bgp-configuration"></a>BGP yapılandırmasını doğrulama

BGP'nin doğru yapılandırılıp yapılandırılmasını `get-AzureRmVirtualNetworkGateway` denetlemek `get-AzureRmLocalNetworkGateway` için, ve komutları çalıştırabilirsiniz. Sonra `BgpSettingsText` bölümde BGP ile ilgili çıktı fark edeceksiniz. Örnek:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="create-a-highly-available-activeactive-vpn-connection"></a>Son derece kullanılabilir etkin/etkin BIR VPN bağlantısı oluşturma

Etkin/etkin ve etkin/bekleme ağ geçitleri arasındaki temel farklar şunlardır:

* İki genel IP adresine sahip iki ağ geçidi IP yapılandırması oluşturmanız gerekir.
* **EnableActiveActiveFeature** bayrağını ayarlamanız gerekir.
* Ağ geçidi SKU **VpnGw1**olmalıdır , **VpnGw2**, veya **VpnGw3**.

Binalar arası ve ağdan ağ bağlantısı için yüksek kullanılabilirlik elde etmek için, birden çok VPN ağ geçidi dağıtmanız ve ağlarınızla Azure arasında birden çok paralel bağlantı kurmanız gerekir. Bağlantı seçeneklerine ve topolojiye genel bir bakış için, [yüksek kullanılabilir binalar arası ve ağdan ağ bağlantısına](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable)bakın.

Etkin/etkin binalar arası ve ağdan ağ bağlantısı oluşturmak için, etkin/etkin modda bir Azure VPN ağ geçidi ni yapılandırmak için [etkin/etkin S2S VPN bağlantılarını Azure VPN ağ geçitleriyle yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) yönergelerini izleyin.

> [!Note]  
> * When you add addresses to the local network gateway for BGP-enabled active/active mode, *add only the /32 addresses of the BGP peers*. Daha fazla adres eklerseniz, bunlar statik yollar olarak kabul edilir ve BGP rotalarından önce gelir.
> * Azure'a bağlanan şirket içi ağlarınız için farklı BGP AS numaraları kullanmanız gerekir. (Bunlar aynıysa, şirket içi VPN aygıtınız diğer BGP komşularıyla eşlemek için ASN'yi zaten kullanıyorsa sanal ağınızı AS numarasını değiştirmeniz gerekir.)

## <a name="change-an-azure-vpn-gateway-type-after-deployment"></a>Dağıtımdan sonra Azure VPN ağ geçidi türünü değiştirme

Bir Azure sanal ağ ağ geçidi türünü ilke tabanlıdan rota tabanlıya veya doğrudan başka bir şekilde değiştiremezsiniz. Önce ağ geçidini silmeniz gerekir. Bundan sonra, IP adresi ve önceden paylaşılan anahtar korunmaz. Ardından, istediğiniz türde yeni bir ağ geçidi oluşturabilirsiniz. 

Bir ağ geçidini silmek ve oluşturmak için aşağıdaki adımları izleyin:

1. Özgün ağ geçidiyle ilişkili tüm bağlantıları silin.
2. Azure portalını, PowerShell'i veya klasik PowerShell'i kullanarak ağ geçidini silin: 
   * [Azure portalını kullanarak sanal ağ ağ geçidini silme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [PowerShell'i kullanarak sanal ağ ağ ağ geçidini silme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [PowerShell (klasik) kullanarak sanal ağ ağ ağ geçidini silme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. İstenilen türdeki yeni ağ [geçidini](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#VNetGateway) oluşturmak ve VPN kurulumını tamamlamak için VPN ağ geçidioluştur'daki adımları izleyin.

> [!Note]
> Bu işlem yaklaşık 60 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM’leri arasında bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

