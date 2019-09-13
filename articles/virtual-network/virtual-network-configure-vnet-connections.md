---
title: VNet veya VPN bağlantılarını yapılandırma ve doğrulama
description: Çeşitli Azure VPN ve VNet dağıtımlarını yapılandırma ve doğrulamaya yönelik adım adım yönergeler
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
ms.openlocfilehash: fc4b649ce8d082d8d854c4c19b617c088ff3141c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901899"
---
# <a name="configure-and-validate-vnet-or-vpn-connections"></a>VNet veya VPN bağlantılarını yapılandırma ve doğrulama

Bu Kılavuzlu izlenecek yol, geçiş yönlendirme, VNet-VNet, BGP, çok siteli, Noktadan siteye ve benzeri senaryolarda çeşitli Azure VPN ve VNet dağıtımlarını yapılandırmak ve doğrulamak için adım adım yönergeler sağlar.

Azure VPN ağ geçitleri, Azure 'da neredeyse her türlü bağlı sanal ağ (VNet) topolojisini düzenleme esnekliği sağlar: VNet türleri arasında (Azure Resource Manager vs.) VNET 'leri bağlayabilirsiniz. Klasik), Azure 'da veya şirket içi karma ortamla, farklı aboneliklerde vb. olarak devam eder. 

## <a name="scenario-1-vnet-to-vnet-vpn-connection"></a>Senaryo 1: VNet-VNet VPN bağlantısı

VPN aracılığıyla bir sanal ağı başka bir sanal ağa (VNet-VNet) bağlamak, VNet 'i şirket içi site konumuna bağlamaya benzer. Her iki bağlantı türü de **IPSec/IKE**kullanarak güvenli bir tünel sağlamak IÇIN bir VPN ağ geçidi kullanır. Sanal ağlar aynı ya da farklı bölgelerde ve aynı ya da farklı aboneliklerde bulunuyor olabilirler.

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Şekil 1-IPSec ile VNET-VNet

Sanal ağlarınız aynı bölgedeyse, bir VPN ağ geçidi kullanmayan, aktarım hızını artıran ve gecikmeyi azaltan VNET eşlemesi kullanarak bunları bağlamayı düşünebilirsiniz, VNET eşlemesini yapılandırmak için **VNET eşlemesini Yapılandır ve Doğrula** ' yı seçin bağlanma.

Sanal ağlarınız her ikisi de Azure Resource Manager dağıtım modeli kullanılarak oluşturulduysa, VPN bağlantısını yapılandırmak için bir **Kaynak Yöneticisi VNET 'i Kaynak Yöneticisi VNET bağlantısına Yapılandır ve Doğrula** ' yı seçin.

Sanal ağlardan biri Azure klasik dağıtım modeli kullanılarak oluşturulduysa, diğeri Kaynak Yöneticisi tarafından oluşturulur, bir VPN bağlantısını yapılandırmak için **Klasik VNET 'i Kaynak Yöneticisi VNET bağlantısına Yapılandır ve Doğrula** ' yı seçin.

### <a name="configuration-1-configure-vnet-peering-for-two-vnets-in-the-same-region"></a>Yapılandırma 1: Aynı bölgedeki iki sanal ağ için VNet eşlemesini yapılandırma

Azure VNet eşlemesini uygulamaya başlamadan önce, VNet eşlemesini yapılandırmak için aşağıdaki önkoşulları karşıladığınızdan emin olun:

* Eşlenmiş sanal ağlar aynı Azure bölgesinde bulunmalıdır.
* Eşlenen sanal ağlarda çakışmayan IP adresi alanları olmalıdır.
* Sanal ağ eşlemesi iki sanal ağ arasında gerçekleşir. Eşlemeler arasında türetilmiş geçişli ilişki yoktur. Örneğin, SanalAğA ile SanalAğB eşlenir ve SanalAğB ile SanalAğC eşlenirse, SanalAğA ile SanalAğC *eşlenmez*.

Gereksinimleri karşıladığınızda, VNet eşlemesi oluşturmak ve yapılandırmak için [sanal ağ eşlemesi oluşturma öğreticisini](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) izleyebilirsiniz.

VNet eşleme yapılandırmasını denetlemek için aşağıdaki yöntemleri kullanın:

1. [Azure Portal](https://portal.azure.com/) , gerekli [Roller ve izinlere](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions)sahip bir hesapla oturum açın.
2. Azure portal en üstünde bulunan metin *arama kaynaklarını* içeren kutuya *sanal ağlar*yazın. Arama sonuçlarında **sanal ağlar** göründüğünde, buna tıklayın.
3. Görüntülenen **sanal ağlar** dikey penceresinde, eşleme oluşturmak istediğiniz sanal ağa tıklayın.
4. Seçtiğiniz sanal ağ için görüntülenen bölmede, **Ayarlar** bölümünde **eşlemeler ' e** tıklayın.
5. Yapılandırmayı denetlemek istediğiniz eşlemeye tıklayın.

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
Azure PowerShell 'i kullanarak, sanal ağ eşlemesini almak için [Get-Azurermvirtualnetworkeşleme](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) komutunu çalıştırın, örneğin:

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

### <a name="connection-type-1-connect-a-resource-manager-vnet-to-anther-resource-manager-vnet-azure-resource-manager-to-azure-resource-manager"></a>Bağlantı türü 1: Kaynak Yöneticisi VNET 'i oluşturulduğu kaynak yöneticisi VNET 'e bağlama (Azure Resource Manager Azure Resource Manager)

Bir Kaynak Yöneticisi VNet 'ten başka bir Kaynak Yöneticisi VNet 'e doğrudan bağlantı yapılandırabilir veya bağlantıyı IPSec ile yapılandırabilirsiniz.

### <a name="configuration-2-configure-vpn-connection-between-resource-manager-vnets"></a>Yapılandırma 2: Kaynak Yöneticisi sanal ağlar arasında VPN bağlantısını yapılandırma

IPSec olmadan Kaynak Yöneticisi sanal ağlar arasında bağlantı yapılandırmak için, bkz. [Azure Portal kullanarak VNET-VNET VPN Gateway bağlantısı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal).

İki Kaynak Yöneticisi sanal ağ arasında IPSec ile bir bağlantı yapılandırmak için, her VNet için [Azure Portal siteden siteye bağlantı oluşturma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) içindeki 1-5 adımlarını izleyin.

> [!Note]
> Bu adımlar yalnızca aynı abonelikteki sanal ağlar için geçerlidir. Sanal ağlar farklı aboneliklerdeyse, bağlantıyı kurmak için PowerShell kullanmanız gerekir. [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) makalesine bakın.

### <a name="validate-vpn-connection-between-resource-manager-vnets"></a>Kaynak Yöneticisi sanal ağlar arasında VPN bağlantısını doğrulama

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

Şekil 4-Azure Resource Manager VNet 'e klasik VNet bağlantısı

VPN bağlantınızın doğru şekilde yapılandırıldığını denetlemek için yönergeleri izleyin:

> [!Note]
> Aşağıdaki adımlarda bulunan "VNET 1" gibi sanal ağ bileşenlerinden sonraki sayı Şekil 4 ' teki sayılara karşılık gelir.

1. Bağlı VNET 'lerdeki çakışan adres alanlarını denetleyin.

   > [!Note]
   > VNET 1 ve VNET 'in 6 ' da çakışan adres alanları olamaz. 

2. Azure Resource Manager VNET 1 adres aralığının **bağlantı nesnesi** 4 ' te doğru şekilde tanımlandığından emin olun.
3. Azure Resource Manager VNET 6 adres aralığının **bağlantı nesnesi** 3 ' te doğru şekilde tanımlandığından emin olun.
4. Önceden paylaşılan anahtarların hem bağlantı nesnelerinde 3 ve 4 ' te eşleştirdiğini doğrulayın.
5. Azure Resource Manager VNET Gateway 2 VIP 'nin **bağlantı nesnesi** 4 ' te doğru şekilde tanımlandığını doğrulayın.
6. Azure Resource Manager VNET Gateway 5 VIP 'nin **bağlantı nesnesi** 3 ' te doğru şekilde tanımlandığını doğrulayın.

### <a name="connection-type-2-connect-a-classic-vnet-to-a-resource-manager-vnet"></a>Bağlantı türü 2: Klasik Sanal Ağ ile Resource Manager Sanal Ağı arasında bağlantı kurma

Farklı aboneliklerde ve farklı bölgelerde bulunan sanal ağlar arasında bir bağlantı oluşturabilirsiniz. Ağ Geçidi türünü rota tabanlı olarak yapılandırdığınız sürece, zaten şirket içi ağlarda bağlantıları olan VNET 'leri de bağlayabilirsiniz.

Klasik VNet ve Kaynak Yöneticisi VNet arasında bağlantı yapılandırmak için, bkz. daha fazla bilgi için [Azure Portal kullanarak farklı dağıtım modellerindeki sanal ağları bağlama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal) .

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Şekil 5-Azure Resource Manager VNet 'e klasik VNet bağlantısı

Klasik sanal ağı bir Azure Resource Manager VNet 'e bağladığınızda yapılandırmayı denetlemek için şu yönergeleri izleyin:

> [!Note]
> Aşağıdaki adımlarda "VNET 1" gibi sanal ağ bileşenlerinden sonraki sayı Şekil 5 ' teki numaralara karşılık gelir.

1. Bağlı VNET 'lerdeki çakışan adres alanlarını denetleyin.

   > [!Note]
   > VNET 1 ve sanal ağ 6 ' da çakışan adres alanları olamaz

2. Azure Resource Manager VNet 6 adres aralığının Klasik yerel ağ tanımı 3 ' te doğru bir şekilde tanımlandığından emin olun.
3. Klasik VNet 1 adres aralığının Azure Resource Manager **bağlantı nesnesi** 4 ' te doğru şekilde tanımlandığını doğrulayın.
4. Klasik VNet Gateway 2 VIP 'nin Azure Resource Manager **bağlantı nesnesi** 4 ' te doğru şekilde tanımlandığını doğrulayın.
5. Azure Resource Manager VNet Gateway 5 VIP 'nin klasik **yerel ağ tanımı** 3 ' te doğru bir şekilde tanımlandığından emin olun.
6. Önceden paylaşılan anahtarların hem bağlı VNET 'lerde eşleştirdiğini doğrulayın:
   1. Klasik VNET-yerel ağ tanımı 3
   2. Azure Resource Manager VNET-bağlantı nesnesi 4

## <a name="scenario-2-point-to-site-vpn-connection"></a>Senaryo 2: Noktadan siteye VPN bağlantısı

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda Noktadan Siteye bağlantıları kullanışlıdır. P2S VPN bağlantısı, yerel Windows VPN istemcisi kullanılarak istemci bilgisayardan başlatılır. Bağlanan istemciler kimlik doğrulamak için sertifika kullanır.

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Şekil 2-Noktadan siteye bağlantı

Noktadan siteye bağlantılar için bir VPN cihazı gerekmez. P2S'de, VPN bağlantısı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden oluşturulur. Farklı dağıtım araçları ve dağıtım modelleri kullanarak bir sanal ağa Noktadan siteye bağlantı bağlayabilirsiniz:

* [Azure portal’ı kullanarak bir sanal ağa yönelik Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Azure portal kullanarak bir sanal ağa Noktadan siteye bağlantı yapılandırma (klasik)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [PowerShell'i kullanarak bir sanal ağa yönelik bir Noktadan Siteye bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connections"></a>Noktadan siteye bağlantılarınızı doğrulama

Sorun giderme [makalesi: Azure Noktadan siteye bağlantı sorunları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) , Noktadan siteye bağlantılar ile ilgili yaygın sorunları açıklar.

## <a name="scenario-3-multi-site-vpn-connection"></a>Senaryo 3: Çok siteli VPN bağlantısı

Zaten bir S2S bağlantısı, Noktadan siteye bağlantısı veya VNet-VNet bağlantısına sahip olan bir VNet 'e siteden siteye (S2S) bağlantısı ekleyebilirsiniz, bu tür bir bağlantı genellikle **çok** siteli bir yapılandırma olarak bilinir. 

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

Şekil 3-çok siteli bağlantı

Azure Şu anda iki dağıtım modeli ile birlikte çalışıyor: Kaynak Yöneticisi ve klasik. İki model birbirleriyle tamamen uyumlu değildir. Farklı modellerle **çok siteli** bağlantıyı yapılandırmak için aşağıdaki makalelere bakın:

* [Mevcut bir VPN Ağ Geçidi bağlantısı ile bir VNet 'e siteden siteye bağlantı ekleme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Mevcut bir VPN Ağ Geçidi bağlantısı ile bir VNet 'e siteden siteye bağlantı ekleme (klasik)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Bu adımlar, ExpressRoute ve siteden siteye birlikte var olan bağlantı yapılandırmalarına yönelik olarak geçerli değildir. Birlikte bulunan bağlantılar hakkında daha fazla bilgi için bkz. [ExpressRoute/S2S birlikte var bağlantıları](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

## <a name="scenario-4-configure-transit-routing"></a>Senaryo 4: Geçiş yönlendirmeyi yapılandırma

Geçişli yönlendirme, ' zincirleme zinciri ' topolojisinde birden çok ağı bağladığınızda belirli bir yönlendirme senaryosudur. Bu yönlendirme, ' zincirin ' ın her iki ucundaki VNET 'lerdeki kaynakların, içindeki VNET 'ler aracılığıyla birbirleriyle iletişim kurmasını sağlar. Geçişli yönlendirme olmadan, bir hub aracılığıyla eşlenen ağlar veya cihazlar birbiriyle iletişime geçemez.

### <a name="configuration-1-configure-transit-routing-in-a-point-to-site-connection"></a>Yapılandırma 1: Noktadan siteye bağlantıda geçiş yönlendirmeyi yapılandırma

Bu senaryoda, VNetA ve VNetB arasında bir siteden siteye VPN bağlantısı yapılandırırsınız ve ayrıca, istemcinin VNetA ağ geçidine bağlanması için bir noktadan siteye VPN 'yi yapılandırın. Ardından, Noktadan siteye istemcilerin VNetA aracılığıyla geçen VNetB 'ye bağlanması için geçiş yönlendirmeyi etkinleştirmek istersiniz. Bu senaryo, siteden VNetA ve VNetB arasında siteden siteye VPN üzerinde BGP etkinleştirildiğinde desteklenir. Daha fazla bilgi için bkz. [noktadan sıteye VPN yönlendirmesi hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

### <a name="configuration-2-configure-transit-routing-in-an-expressroute-connection"></a>Yapılandırma 2: ExpressRoute bağlantısında geçiş yönlendirmeyi yapılandırma

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  Daha fazla bilgi için bkz. [ExpressRoute 'a genel bakış](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

Şekil 6-ExpressRoute ' özel eşleme ' Azure sanal ağlarına bağlantı

> [!Note]
> VNetA ve VNetB 'nin, geçişli yönlendirmeyi yapılandırmak yerine [her Iki VNET 'i de ExpressRoute devresine bağlayabileceğiniz](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm) aynı coğrafi bölgede olması önerilir. Sanal ağlarınız farklı coğrafi bölgeler halinde ise, [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium)'a sahipseniz, bu öğeleri doğrudan bağlantı hattına da bağlayabilirsiniz. 

ExpressRoute ve siteden siteye birlikte bulunma varsa, geçiş yönlendirmesi desteklenmez. Daha fazla bilgi için bkz. [daha fazla bilgi için bkz. ExpressRoute ve siteden siteye birlikte bağlantıları yapılandırma](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

Yerel ağlarınızı bir Azure sanal ağına bağlamak için ExpressRoute 'u etkinleştirdiyseniz, geçişli yönlendirmeye sahip olmak istediğiniz sanal ağlar arasında VNet eşlemesini etkinleştirebilirsiniz. Yerel ağlarınızın uzak VNet 'e bağlanmasına izin vermek için [sanal ağ eşlemesini](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity)yapılandırmanız gerekir. 

> [!Note]
> VNet eşlemesi yalnızca aynı bölgedeki sanal ağlar için kullanılabilir.

VNet eşlemesi için geçiş rotası yapılandırılıp yapılandırılmadığını denetlemek için yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/) , gerekli [Roller ve izinlere](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions)sahip bir hesapla oturum açın.
2. Daha önce diyagramda olduğu gibi [VNET a ve B arasında bir eşleme oluşturun](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) (Şekil 6). 
3. Seçtiğiniz sanal ağ için görüntülenen bölmede, **Ayarlar** bölümünde **eşlemeler ' e** tıklayın.
4. ExpressRoute bağlantı hattına bağlı VNetA 'da **ağ geçidi aktarımına Izin ver** ' i etkinleştirdiğini doğrulamak ve ExpressRoute 'a bağlı olmayan uzak VNetB üzerinde **uzak ağ geçidini kullanmak** Için, görüntülemek ve **yapılandırma** ' ya tıklayın. bilgisayarına.

### <a name="configuration-3-configure-transit-routing-in-a-vnet-peering-connection"></a>Yapılandırma 3: VNet eşleme bağlantısında geçiş yönlendirmeyi yapılandırma

Sanal ağlar eşlendiğinde, eşlenmiş sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. VNet eşlemesindeki geçiş yolunu yapılandırmak için [sanal ağdan sanal ağa bağlantıları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json)denetleyin.

> [!Note]
> Farklı dağıtım modelleriyle oluşturulan sanal ağlar arasındaki eşleme ilişkisinde ağ geçidi geçişi desteklenmez. Bir ağ geçidi geçişinin çalışması için eşleme ilişkisindeki her iki sanal ağ da Resource Manager ile oluşturulmuş olmalıdır.

VNet eşlemesi için geçiş rotası yapılandırılıp yapılandırılmadığını denetlemek için yönergeleri izleyin:

1. [Azure Portal](https://portal.azure.com/) , gerekli [Roller ve izinlere](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions)sahip bir hesapla oturum açın.
2. Azure portal en üstünde bulunan metin arama kaynaklarını içeren kutuya *sanal ağlar*yazın. Arama sonuçlarında **sanal ağlar** göründüğünde, buna tıklayın.
3. Görüntülenen **sanal ağlar** dikey penceresinde, eşleme ayarını denetlemek istediğiniz sanal ağa tıklayın.
4. Seçtiğiniz sanal ağ için görüntülenen bölmede, **Ayarlar** bölümünde **eşlemeler ' e** tıklayın.
5. Görüntülemek istediğiniz eşlemeye tıklayın ve **ağ geçidi aktarımına Izin ver** ' i etkinleştirdiğini ve **yapılandırma**altında **uzak ağ geçidini kullanmayı** doğrulayın.

![GÖRÜNTÜYLE](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configuration-4-configure-transit-routing-in-a-vnet-to-vnet-connection"></a>Yapılandırma 4: VNet-VNet bağlantısında geçiş yönlendirmeyi yapılandırma

Sanal ağlar arasında geçiş yönlendirmeyi yapılandırmak için, Kaynak Yöneticisi dağıtım modelini ve PowerShell 'i kullanarak tüm ara VNet-VNet bağlantılarında BGP 'yi etkinleştirmelisiniz. Yönergeler için bkz. [PowerShell kullanarak Azure VPN ağ geçitlerinde BGP yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Klasik dağıtım modeli kullanılarak Azure VPN Gateway aracılığıyla geçiş trafiği mümkündür, ancak ağ yapılandırma dosyasında statik olarak tanımlanan adres alanlarını kullanır. Azure sanal ağları ve VPN ağ geçitleri, klasik dağıtım modeli kullanılarak henüz desteklenmez. BGP olmadan, transit adres alanlarını el ile tanımlamak hataya açıktır ve önerilmez.

> [!Note]
> Klasik VNet-VNet bağlantıları, Azure portal (klasik) kullanılarak veya klasik portalda bir ağ yapılandırma dosyası kullanılarak yapılandırılır. Azure Resource Manager dağıtım modeli veya Azure portal aracılığıyla klasik bir sanal ağ oluşturamaz veya değiştiremezsiniz. Klasik VNET 'ler için geçiş yönlendirme hakkında daha fazla bilgi için bkz. [VPN (v1) kullanarak Azure ARM 'de Hub & bağlı bileşen, zincirleme ve tam ağ VNET topolojileri](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

### <a name="configuration-5-configure-transit-routing-in-a-site-to-site-connection"></a>Yapılandırma 5: Siteden siteye bağlantıda geçiş yönlendirmeyi yapılandırma

Şirket içi ağınız ile VNet arasında siteden siteye bağlantısı olan geçiş yönlendirmeyi yapılandırmak için, Kaynak Yöneticisi dağıtım modelini ve PowerShell 'i kullanarak tüm ara siteden siteye bağlantılar üzerinde BGP 'yi etkinleştirmeniz gerekir, bkz. [nasıl yapılandırılır Yönergeler için PowerShell 'i kullanarak Azure VPN ağ geçitlerinde BGP](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps) .

Klasik dağıtım modeli kullanılarak Azure VPN Gateway aracılığıyla geçiş trafiği mümkündür, ancak ağ yapılandırma dosyasında statik olarak tanımlanan adres alanlarını kullanır. Azure sanal ağları ve VPN ağ geçitleri, klasik dağıtım modeli kullanılarak henüz desteklenmez. BGP olmadan, transit adres alanlarını el ile tanımlamak hataya açıktır ve önerilmez.

> [!Note]
> Klasik siteden siteye bağlantılar Azure portal (klasik) kullanılarak veya klasik portalda bir ağ yapılandırma dosyası kullanılarak yapılandırılır. Azure Resource Manager dağıtım modeli veya Azure portal aracılığıyla klasik bir sanal ağ oluşturamaz veya değiştiremezsiniz. Klasik VNET 'ler için geçiş yönlendirme hakkında daha fazla bilgi için bkz. [VPN (v1) kullanarak Azure ARM 'de Hub & bağlı bileşen, zincirleme ve tam ağ VNET topolojileri](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

## <a name="scenario-5-configure-bgp-for-a-vpn-gateway"></a>Senaryo 5: Bir VPN ağ geçidi için BGP yapılandırma

BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini değiş tokuş etmek için Internet 'te kullanılan standart yönlendirme protokolüdür. BGP, Azure sanal ağları bağlamında kullanıldığında BGP, Azure VPN ağ geçitlerini ve BGP eşleri veya komşuları olarak bilinen şirket içi VPN cihazlarınızı etkinleştirilir. Her iki ağ geçidini, bu öneklerdeki ağ geçitleri veya yönlendiriciler arasında gidip ulaşılabilmesi için kullanılabilirlik ve ulaşılabilirlik konusunda bilgilendiren "rotalar" şeklinde değiş tokuş ederler. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir. Daha fazla bilgi için bkz. [Azure VPN ağ geçitleri Ile BGP 'ye genel bakış](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

### <a name="configure-bgp-for-a-vpn-connection"></a>VPN bağlantısı için BGP 'yi yapılandırma

BGP kullanan bir VPN bağlantısı yapılandırmak için bkz. [PowerShell kullanarak Azure VPN ağ geçitlerinde BGP yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

> [!Note]
> 1. Sanal ağ geçidinde BGP 'yi bir AS numarası oluşturarak etkinleştirin. Temel ağ geçitleri BGP 'yi desteklemez. Ağ geçidinin SKU 'sunu denetlemek için Azure portal VPN Gateway dikey penceresinin Genel Bakış bölümüne gidin. SKU 'nuzu **temel**Ise, SKU 'yu ([ağ geçidini yeniden boyutlandırma](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)) **VpnGw1** SKU 'su olarak değiştirmeniz gerekir. SKU 'nun denetlenmesi, 20-30 dakika kapalı kalma süresine neden olur. Ağ geçidinin doğru SKU 'SU varsa, AS [set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) PowerShell komutu-Let aracılığıyla eklenebilir. AS numarasını yapılandırdıktan sonra, ağ geçidi için bir BGP eşi IP 'si otomatik olarak sunulacaktır.
> 2. LocalNetworkGateway, AS numarası ve BGP eş adresiyle **el ile** sağlanmalıdır. **ASN** ve **-bgppeeringaddress** değerlerini [New-azurermlocalnetworkgateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) veya [set-azurermlocalnetworkgateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) PowerShell komutunu-Let kullanarak ayarlayabilirsiniz. Bazı AS numaraları Azure için ayrılmıştır ve [azure VPN Gateway Ile BGP hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview#bgp-faq)bölümünde açıklandığı gibi bunları kullanamazsınız.
> 3. Son olarak, bağlantı nesnesinin BGP etkin olması gerekir. **-Enablebgp** değerini `$True` [New-azurermvirtualnetworkgatewayconnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) veya [set-azurermvirtualnetworkgatewayconnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0)aracılığıyla olarak ayarlayabilirsiniz.

### <a name="validate-the-bgp-configuration"></a>BGP yapılandırmasını doğrulama

BGP 'nin doğru şekilde yapılandırılıp yapılandırılmadığını denetlemek için cmdlet 'ini `get-AzureRmVirtualNetworkGateway` çalıştırabilirsiniz ve `get-AzureRmLocalNetworkGateway`sonra bgpsettingstext bölümünde BGP ile ilgili çıktıyı fark edeceksiniz. Örneğin: BgpSettingsText:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="scenario-6-highly-available-active-active-vpn-connection"></a>Senaryo 6: Yüksek oranda kullanılabilir etkin-etkin VPN bağlantısı

Etkin-etkin ve etkin bekleme geçitleri arasındaki temel farklılıklar:

* İki genel IP adresi ile iki ağ geçidi IP yapılandırması oluşturmanız gerekir
* *Enableactiveactivefeature* bayrağını ayarlamanız gerekir
* Ağ Geçidi SKU 'SU VpnGw1, VpnGw2, VpnGw3 olmalıdır

Şirketler arası ve VNet 'ten VNet 'e bağlantı için yüksek kullanılabilirlik elde etmek üzere, birden fazla VPN ağ geçidi dağıtmanız ve ağlarınız ile Azure arasında birden çok paralel bağlantı oluşturmanız gerekir. Bağlantı seçeneklerine ve topolojisine genel bir bakış için bkz. [yüksek oranda kullanılabilir şirket içi ve VNET 'Ten VNET 'e bağlantı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable).

Etkin-etkin şirketler arası ve VNet-VNet bağlantıları oluşturmak için Azure VPN ağ [geçitleriyle etkin-ETKIN S2S VPN bağlantılarını yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) bölümündeki yönergeleri IZLEYEREK Azure VPN Gateway 'i etkin/etkin modda yapılandırın.

> [!Note]  
> 1. BGP etkin için yerel ağ geçidine adres eklediğinizde, etkin-etkin modu *yalnızca BGP eşlerinin/32 adreslerini ekleyin*. Daha fazla adres eklerseniz, bu adresler statik yollar olarak değerlendirilir ve BGP rotalarıyla önceliklidir.
> 2. Azure 'a bağlanan şirket içi ağlarınız için farklı BGP 'ler kullanmanız gerekir. (Aynı ise, şirket içi VPN cihazınız zaten diğer BGP komşuları ile ASN 'yi kullanıyorsa VNet ASN 'nizi değiştirmeniz gerekir.)

## <a name="scenario-7-change-an-azure-vpn-gateway-type-after-deployment"></a>Senaryo 7: Dağıtımdan sonra bir Azure VPN ağ geçidi türünü değiştirme

Bir Azure VNet ağ geçidi türünü, ilke temelli olarak doğrudan rota tabanlı veya başka bir yöntemle değiştiremezsiniz. IP adresi ve önceden paylaşılan anahtar (PSK) herhangi bir şekilde korunduktan sonra ağ geçidini silmeniz gerekir. Daha sonra, istenen türde yeni bir ağ geçidi oluşturabilirsiniz. Bir ağ geçidini silmek ve oluşturmak için aşağıdaki adımları izleyin:

1. Özgün ağ geçidiyle ilişkili tüm bağlantıları silin.
2. Azure portal, PowerShell veya klasik PowerShell kullanarak ağ geçidini silin: 
   * [Azure portal kullanarak bir sanal ağ geçidini silme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [PowerShell kullanarak bir sanal ağ geçidini silme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [PowerShell kullanarak bir sanal ağ geçidini silme (klasik)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. İstenen türde yeni ağ geçidini oluşturmak için [VPN ağ geçidini oluşturma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#a-namevnetgatewaya4-create-the-vpn-gateway) bölümündeki adımları IZLEYIN ve VPN kurulumunu tamamlayın.

> [!Note]
> Bu işlem yaklaşık 60 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM’leri arasında bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

