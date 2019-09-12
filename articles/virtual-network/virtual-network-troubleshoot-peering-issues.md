---
title: Sanal ağ eşleme sorunlarını giderme
description: Birçok sanal ağ eşleme sorununu çözmeye yardımcı olacak adımlar.
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 1a3d1e84-f793-41b4-aa04-774a7e8f7719
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: fc01a20a077a1c624ed490600db919fe3197556c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901769"
---
# <a name="troubleshoot-virtual-network-peering-issues"></a>Sanal ağ eşleme sorunlarını giderme

Bu sorun giderme kılavuzu, çoğu [sanal ağ eşleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) sorununu çözmenize yardımcı olacak adımları sağlar.

![GÖRÜNTÜYLE](./media/virtual-network-troubleshoot-peering-issues/4489538_en_1.png)

## <a name="scenario-1-configure-virtual-network-peering-between-two-virtual-networks"></a>Senaryo 1: İki sanal ağ arasında sanal ağ eşlemesi yapılandırma

Sanal ağlar aynı abonelikte veya farklı aboneliklerde mı?

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-subscription"></a>Bağlantı türü 1: Sanal ağlar aynı abonelikte

Aynı abonelikte bulunan sanal ağlar için sanal ağ eşlemesini yapılandırmak üzere aşağıdaki makalelerde sunulan yöntemleri uygun şekilde kullanın:

* Sanal ağlar **aynı bölgedeyse**, [aynı abonelikte bulunan sanal ağlar için eşleme oluşturma](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#create-a-peering)adımlarını izleyin.
* Sanal ağlar **farklı bölgelerde**ise, [Genel sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)ayarlama adımlarını izleyin.  

> [!Note]
> Bağlantı, aşağıdaki kaynaklar için küresel VNet eşlemesi üzerinden çalışmaz: 
>
> * Temel ıLB SKU 'SU arkasındaki VM 'Ler
> * Redis Cache (temel ıLB SKU 'SU kullanır)
> * Application Gateway (temel ıLB SKU 'SU kullanır)
> * Ölçek Kümeleri (temel ıLB SKU 'SU kullanır)
> * Service Fabric kümeleri (temel ıLB SKU 'SU kullanır)
> * SQL her zaman açık (temel ıLB SKU 'SU kullanır)
> * App Service ortamları (Ao) (temel ıLB SKU 'SU kullanır)
> * API Management (temel ıLB SKU 'SU kullanır)
> * Azure Active Directory etki alanı hizmeti (ekler) (temel ıLB SKU 'SU kullanır)

Daha fazla bilgi için bkz. genel eşleme [gereksinimleri ve kısıtlamaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) .

### <a name="connection-type-2-the-virtual-networks-are-in-different-subscriptions-or-ad-tenants"></a>Bağlantı türü 2: Sanal ağlar farklı aboneliklerde veya AD kiracılarında

Farklı aboneliklerdeki veya Active Directory kiracılardaki sanal ağların sanal ağ eşlemesini yapılandırmak için [Azure CLI için farklı aboneliklerde eşleme oluşturma](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli)' daki adımları izleyin.

> [!Note]
> Ağ eşlemesini yapılandırmak için her iki abonelik için de **ağ katılımcısı** izinlerinizin olması gerekir. Daha fazla bilgi için bkz. [eşleme izinleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering%23permissions).

## <a name="scenario-2-configure-virtual-network-peering-with-hub-spoke-topology-that-uses-on-premises-resources"></a>Senaryo 2: Şirket içi kaynakları kullanan hub-ışınsal topolojisi ile sanal ağ eşlemesini yapılandırma

![GÖRÜNTÜYLE](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1a.png)

### <a name="connection-type-1-for-site-to-site-connection-or-expressroute-connection"></a>Bağlantı türü 1: Siteden siteye bağlantı veya ExpressRoute bağlantısı için

İçindeki adımları izleyin: [VPN ağ geçidi geçişi 'ni sanal ağ eşlemesi Için yapılandırın](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).

### <a name="connection-type-2-for-point-to-site-connections"></a>Bağlantı türü 2: Noktadan siteye bağlantılar için

1. İçindeki adımları izleyin: [VPN ağ geçidi geçişi 'ni sanal ağ eşlemesi Için yapılandırın](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json).
2. Sanal ağ eşlemesi kurulduktan veya değiştirildikten sonra, Noktadan siteye istemcilerinin, bağlı olan sanal ağa güncelleştirilmiş yolları alması için Noktadan siteye paketinin yeniden indirilmesi ve yeniden yüklenmesi gerekir.

## <a name="scenario-3-configure-virtual-network-peering-with-hub-spoke-topology-for-azure-virtual-network"></a>Senaryo 3: Azure sanal ağ için hub-ışınsal topolojisi ile sanal ağ eşlemeyi yapılandırma

![GÖRÜNTÜYLE](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1b.png)

### <a name="connection-type-1-the-virtual-networks-are-in-the-same-region"></a>Bağlantı türü 1: Sanal ağlar aynı bölgedeyse

Hub sanal ağında bir ağ sanal gereci (NVA) yapılandırmanız ve bağlı bileşen sanal ağlarında bir sonraki atlamaya "ağ sanal gereci" uygulanmış Kullanıcı tanımlı yollara sahip olmanız gerekir. Daha fazla bilgi için bkz. [hizmet zincirleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining).

> [!Note]
> Bir NVA ayarlamak için yardıma ihtiyacınız varsa [NVA satıcısına başvurun](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines).

NVA cihaz kurulumu ve yönlendirme sorunlarını gidermeye yönelik yardım için bkz. [Azure 'Da ağ sanal gereç sorunları](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva).

### <a name="connection-type-2-the-virtual-networks-are-in-different-regions"></a>Bağlantı türü 2: Sanal ağlar farklı bölgelerde

Genel VNet eşlemesi üzerinden aktarım artık desteklenmektedir. Bağlantı, aşağıdaki kaynaklar için genel VNet eşlemesi üzerinden çalışmaz:

* Temel ıLB SKU 'SU arkasındaki VM 'Ler
* Redis Cache (temel ıLB SKU 'SU kullanır)
* Application Gateway (temel ıLB SKU 'SU kullanır)
* Ölçek Kümeleri (temel ıLB SKU 'SU kullanır)
* Service Fabric kümeleri (temel ıLB SKU 'SU kullanır)
* SQL her zaman açık (temel ıLB SKU 'SU kullanır)
* App Service ortamları (Ao) (temel ıLB SKU 'SU kullanır)
* API Management (temel ıLB SKU 'SU kullanır)
* Azure Active Directory etki alanı hizmeti (ekler) (temel ıLB SKU 'SU kullanır)

Genel eşleme gereksinimleri ve depoları hakkında daha fazla bilgi edinmek için bkz. [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints).

## <a name="scenario-4-i-have-a-connectivity-issue-between-two-peered-virtual-networks"></a>Senaryo 4: Eşlenen iki sanal ağ arasında bağlantı sorunu yaşıyorum

[Azure Portal](https://portal.azure.com/) , gerekli [Roller ve izinlere](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions)sahip bir hesapla oturum açın. Sanal ağı seçin, **eşleme**' yi seçin ve ardından **durum** alanını denetleyin. Durum nedir?

### <a name="connection-type-1-the-peering-status-shows-connected"></a>Bağlantı türü 1: Eşleme durumu ' Connected ' gösteriyor

Sorunu gidermek için şu adımları izleyin:

1. Ağ trafiği akışlarını denetleyin:

   Trafik akışlarında girişim hatasına neden olan bir NSG veya UDR olup olmadığını öğrenmek için [bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-overview) ve kaynak VM 'den hedef sanal makineye [IP akışı doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) ' yı kullanın.

   Bir güvenlik duvarı veya NVA gereci kullanıyorsanız, şu adımları izleyin: 
   1. Bu adım tamamlandıktan sonra dosyaları geri yükleyebilmek için UDR parametrelerini belgeleyin.
   2. Bir sonraki atlama olarak NVA 'ya işaret eden kaynak VM alt ağından veya NIC 'den UDR 'yi kaldırın. Kaynak VM 'den, NVA 'yi atlayarak hedefe doğrudan bağlantıyı doğrulayın. Bu adım çalışırsa, [NVA sorun gidericisine](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)bakın.

2. Ağ izlemesi yapın: 
   1. Hedef VM 'de bir ağ izlemesi başlatın. Windows için **netsh**' i kullanabilirsiniz. Linux için **TCPDump**kullanın.
   2. Kaynaktan hedef IP 'ye **Tcing veya Psping** çalıştırın.

   * Bu, bir **tcma** komutunun örneğidir:`tcping64.exe -t <destination VM address> 3389`

   3. **Tcme** işlemi tamamlandıktan sonra, hedefteki ağ izlemesini durdurun.
   4. Paketler kaynaktan ulaştığında, ağ sorunu yoktur. Yapılandırma sorununun yerini bulmak için hem VM Güvenlik duvarını hem de bu bağlantı noktasını dinleyen uygulamayı inceleyin.

   > [!Note]
   > Genel sanal ağ eşlemesi (farklı bölgelerdeki sanal ağlar) üzerinden aşağıdaki kaynak türlerine bağlanamazsınız:
   >
   > * Temel ıLB SKU 'SU arkasındaki VM 'Ler
   > * Redis Cache (temel ıLB SKU 'SU kullanır)
   > * Application Gateway (temel ıLB SKU 'SU kullanır)
   > * Ölçek Kümeleri (temel ıLB SKU 'SU kullanır)
   > * Service Fabric kümeleri (temel ıLB SKU 'SU kullanır)
   > * SQL her zaman açık (temel ıLB SKU 'SU kullanır)
   > * App Service ortamları (Ao) (temel ıLB SKU 'SU kullanır)
   > * API Management (temel ıLB SKU 'SU kullanır)
   > * Azure Active Directory etki alanı hizmeti (ekler) (temel ıLB SKU 'SU kullanır)

Daha fazla bilgi için bkz. genel eşleme [gereksinimleri ve kısıtlamaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) .

### <a name="connection-type-2-the-peering-status-shows-disconnected"></a>Bağlantı türü 2: Eşleme durumu ' bağlantısız ' gösterir

Her iki VNET 'ten de eş öğeleri silmeniz ve bunları yeniden oluşturmanız gerekir.

## <a name="scenario-5-i-have-a-connectivity-issue-between-a-hub-spoke-virtual-network-and-on-premises-resource"></a>Senaryo 5: Hub-bağlı bir sanal ağ ile şirket içi kaynak arasında bir bağlantı sorunu yaşıyorum

Üçüncü taraf bir NVA veya VPN Gateway mi kullanıyorsunuz?

### <a name="connection-type-1-my-network-uses-a-third-party-nva-or-vpn-gateway"></a>Bağlantı türü 1: Ağımdaki bir üçüncü taraf NVA veya VPN ağ geçidi kullanılıyor

Üçüncü taraf bir NVA veya VPN ağ geçidini etkileyen bağlantı sorunlarını gidermek için aşağıdaki makalelere bakın:

* [NVA sorun giderici](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)
* [Hizmet zincirleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)

### <a name="connection-type-2-my-network-does-not-a-third-party-nva-or-vpn-gateway"></a>Bağlantı türü 2: Ağımdaki bir üçüncü taraf NVA veya VPN ağ geçidi değil

Hub ve bağlı bileşen sanal ağlarının VPN Gateway 'i olması gerekir mi?

#### <a name="both-the-hub-and-spoke-virtual-networks-have-a-vpn-gateway"></a>Hub ve bağlı bileşen sanal ağlarının VPN Gateway 'i vardır

Uzak ağ geçidi kullanılması desteklenmez.

VNet eşleme sınırlaması nedeniyle, bağlı olan VNet 'in zaten bir VPN ağ geçidi varsa, **uzak ağ geçidi** , bağlı olan VNET 'te desteklenmez.

#### <a name="both-the-hub-and-spoke-virtual-networks-dont-have-a-vpn-gateway"></a>Hub ve bağlı bileşen sanal ağlarının VPN ağ geçidi yok

Siteden siteye veya ExpressRoute bağlantıları için, şirket içi sanal ağa yönelik bağlantı sorunlarının bu birincil nedenlerini denetleyin.

* Ağ geçidine sahip sanal ağ üzerinde **iletilen trafiğe Izin ver** onay kutusunun seçili olduğundan emin olun.
* Ağ geçidine sahip olmayan sanal ağ üzerinde **uzak ağ geçidini kullan** onay kutusunun seçili olduğunu doğrulayın.
* Ağ yöneticinizin, tüm uzak sanal ağ adresi alanı eklenmiş olduklarını doğrulamak için şirket içi cihazlarınızı denetlemesini sağlayabilirsiniz.

Noktadan siteye bağlantılar için:

* Ağ geçidine sahip sanal ağ üzerinde **iletilen trafiğe Izin ver** onay kutusunun seçili olduğundan emin olun.
* Ağ geçidine sahip olmayan sanal ağ üzerinde **uzak ağ geçidini kullan** onay kutusunun seçili olduğunu doğrulayın.
* Noktadan siteye istemci paketini indirin ve yeniden yükleyin. Yeni eşlenen sanal ağ yolları Noktadan siteye istemcilere otomatik olarak yollar eklemez.

## <a name="scenario-6-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-the-same-region"></a>Senaryo 6: Aynı bölgedeki bağlı olan sanal ağlar arasında hub-bağlı ağ bağlantısı sorunu var

Bir hub ağında bir NVA olmalıdır, bir sonraki atlama olarak ayarlanmış bir NVA 'ya sahip olan ve hub sanal ağında **Iletilen trafiğe Izin ver** seçeneğini etkinleştirmeniz gerekir.

Daha fazla bilgi için bkz. [hizmet zincirleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)ve bu gereksinimleri tercih ettiğiniz [NVA satıcısı](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines) ile tartışın.

## <a name="scenario-7-i-have-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-different-regions"></a>Senaryo 7: Farklı bölgelerde bağlı olan sanal ağlar arasında hub-bağlı bir ağ bağlantısı sorunu var

Genel VNet eşlemesi üzerinden aktarım artık desteklenmektedir. Bağlantı, aşağıdaki kaynaklar için küresel VNet eşlemesi üzerinden çalışmaz:

* Temel ıLB SKU 'SU arkasındaki VM 'Ler
* Redis Cache (temel ıLB SKU 'SU kullanır)
* Application Gateway (temel ıLB SKU 'SU kullanır)
* Ölçek Kümeleri (temel ıLB SKU 'SU kullanır)
* Service Fabric kümeleri (temel ıLB SKU 'SU kullanır)
* SQL her zaman açık (temel ıLB SKU 'SU kullanır)
* App Service ortamları (Ao) (temel ıLB SKU 'SU kullanır)
* API Management (temel ıLB SKU 'SU kullanır)
* Azure Active Directory etki alanı hizmeti (ekler) (temel ıLB SKU 'SU kullanır)

Daha fazla bilgi için bkz. genel eşleme [gereksinimleri ve kısıtlamaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) ve [farklı VPN topolojileri](https://blogs.msdn.microsoft.com/igorpag/2016/02/11/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-v2/).

## <a name="scenario-8-i-have-a-hub-spoke-network-connectivity-issue-between-a-web-app-and-the-spoke-virtual-network"></a>Senaryo 8: Bir Web uygulaması ve bağlı bileşen sanal ağı arasında hub-bağlı ağ bağlantısı sorunu var

Bu sorunu gidermek için şu adımları izleyin:

1. Azure portal oturum açın. Web uygulamasına gidin, **ağ**' ı seçin ve ardından **VNET tümleştirmesi**' ni seçin.
2. Uzak sanal ağı görüp görmeyeceğinizi denetleyin. Uzak sanal ağ adres alanını el ile girin (**ağa eşitleyin** ve **yollar ekleyin**).

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Uygulamanızı bir Azure sanal ağıyla tümleştirin](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
* [Noktadan siteye VPN yönlendirmesi hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

## <a name="scenario-9-i-receive-an-error-when-configuring-virtual-network-peering"></a>Senaryo 9: Sanal Ağ eşlemesini yapılandırırken bir hata alıyorum

### <a name="error-1-current-tenant-tenant-id-isnt-authorized-to-access-linked-subscription"></a>Hata 1: Geçerli kiracının `<TENANT ID>` bağlantılı aboneliğe erişme yetkisi yok

Bu sorunu çözmek için, [eşleme oluşturma-Azure CLI](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli)' daki adımları izleyin.

### <a name="error-2-not-connected"></a>Hata 2: Bağlanılmadı

Her iki VNET 'ten de eş öğeleri silmeniz ve yeniden oluşturmanız gerekir.

### <a name="error-3-failed-to-peer-a-databricks-virtual-network"></a>Hata 3: Databricks sanal ağının eşi yapılamadı

Bu sorunu çözmek için **Azure Databricks** dikey penceresinden sanal ağ eşlemesini yapılandırın ve ardından **kaynak kimliği**' ni kullanarak hedef sanal ağı belirtin. Daha fazla bilgi için bkz. [peer a Databricks sanal ağını uzak bir sanal ağa](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#id2).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM’leri arasında bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)