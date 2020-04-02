---
title: Sanal ağ eşleme sorunlarını giderme
description: Çoğu sanal ağ izleme sorununu çözmeye yardımcı olacak adımlar.
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-network
ms.assetid: 1a3d1e84-f793-41b4-aa04-774a7e8f7719
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: 662619e101b45d1dd8b34ea97e31f214b254124a
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521880"
---
# <a name="troubleshoot-virtual-network-peering-issues"></a>Sanal ağ eşleme sorunlarını giderme

Bu sorun giderme kılavuzu, çoğu [sanal ağ izleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) sorunlarını çözmenize yardımcı olacak adımlar sağlar.

![Sanal ağ eşleme diyagramı](./media/virtual-network-troubleshoot-peering-issues/4489538_en_1.png)

## <a name="configure-virtual-network-peering-between-two-virtual-networks"></a>Sanal ağ eşlemeyi iki sanal ağ arasında yapılandırma

Sanal ağlar aynı abonelikte mi yoksa farklı aboneliklerde mi?

### <a name="the-virtual-networks-are-in-the-same-subscription"></a>Sanal ağlar aynı abonelikte

Aynı abonelikte bulunan sanal ağlar için sanal ağ eşlemi yapılandırmak için aşağıdaki makalelerdeki yöntemleri kullanın:

* Sanal ağlar *aynı bölgedeyse,* [bkz.](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#create-a-peering)
* Sanal ağlar *farklı bölgelerdeyse,* [Sanal ağ eşleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)sini görün. 

> [!Note]
> Bağlantı, aşağıdaki kaynaklar için genel sanal ağ eşleme üzerinde çalışmaz: 
>
> * Temel dahili yük dengeleyicisi (ILB) SKU arkasında sanal makineler (VM)
> * Redis önbelleği (Temel ILB SKU kullanır)
> * Uygulama ağ geçidi (Temel ILB SKU kullanır)
> * Sanal makine ölçek setleri (Temel ILB SKU kullanır)
> * Azure Hizmet Kumaş kümeleri (Temel ILB SKU kullanır)
> * SQL Server Always On (Temel ILB SKU kullanır)
> * PowerApps için Azure Uygulama Hizmet Ortamı (Temel ILB SKU kullanır)
> * Azure API Yönetimi (Temel ILB SKU kullanır)
> * Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) (Temel ILB SKU kullanır)

Daha fazla bilgi için, genel bakış gereksinimlerini [ve kısıtlamalarına](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) bakın.

### <a name="the-virtual-networks-are-in-different-subscriptions-or-active-directory-tenants"></a>Sanal ağlar farklı aboneliklerde veya Active Directory kiracılarında

Farklı aboneliklerde veya Active Directory kiracılarında sanal ağlar için sanal ağ eşlemesini yapılandırmak [için](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli)bkz.

> [!Note]
> Ağ eşlemesi yapılandırmak için, her iki abonelikte de **Ağ Katılımcısı** izinlerine sahip olmalısınız. Daha fazla bilgi için [Bkz. Akran İzni.](virtual-network-manage-peering.md#permissions)

## <a name="configure-virtual-network-peering-with-hub-spoke-topology-that-uses-on-premises-resources"></a>Şirket içi kaynakları kullanan hub ile konuşan topoloji ile sanal ağ eşlemelerini yapılandırma

![Şirket içi konuşan sanal ağ diyagramı](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1a.png)

### <a name="for-a-site-to-site-connection-or-an-expressroute-connection"></a>Siteden siteye bağlantı veya ExpressRoute bağlantısı için

Adımları izleyin: [Sanal ağ eşleme için VPN ağ geçidi geçişini yapılandırın.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json)

### <a name="for-point-to-site-connections"></a>Noktadan siteye bağlantılar için

1. Adımları izleyin: [Sanal ağ eşleme için VPN ağ geçidi geçişini yapılandırın.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit?toc=/azure/virtual-network/toc.json)
2. Sanal ağ eşlemi kurulduktan veya değiştirildikten sonra, siteden siteye gelen istemcilerin güncelleştirilmiş yolları konuşan sanal ağa ulaştırabilmesi için siteden siteye paketi indirin ve yeniden yükleyin.

## <a name="configure-virtual-network-peering-with-hub-spoke-topology-virtual-network"></a>Hub ile konuşan topoloji sanal ağ ile sanal ağ eşleme yapılandırma

![Sanal ağ ile sanal ağ izleme diyagramı konuştu](./media/virtual-network-troubleshoot-peering-issues/4488712_en_1b.png)

### <a name="the-virtual-networks-are-in-the-same-region"></a>Sanal ağlar aynı bölgede


1. Hub sanal ağda, bir ağ sanal cihaz (NVA) yapılandırın.
1. Konuşan sanal ağlarda, bir sonraki hop türü "ağ sanal cihaz" uygulanan kullanıcı tanımlı yolları var.

Daha fazla bilgi için [Hizmet zincirine](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)bakın.

> [!Note]
> Bir NVA ayarlamak için yardıma ihtiyacınız varsa, [NVA satıcısına başvurun.](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines)

NVA aygıt kurulumu ve yönlendirme sorununa yardımcı olmak için [Azure'daki Ağ sanal cihaz sorunlarına](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)bakın.

### <a name="the-virtual-networks-are-in-different-regions"></a>Sanal ağlar farklı bölgelerde

Küresel sanal ağ izleme üzerinden geçiş artık desteklenir. Bağlantı, aşağıdaki kaynaklar için genel sanal ağ eşleme üzerinde çalışmaz:

* Temel ILB SKU arkasında VMs
* Redis önbelleği (Temel ILB SKU kullanır)
* Uygulama ağ geçidi (Temel ILB SKU kullanır)
* Ölçek kümeleri (Temel ILB SKU kullanır)
* Servis Kumaş kümeleri (Temel ILB SKU kullanır)
* SQL Server Always On (Temel ILB SKU kullanır)
* Uygulama Hizmet Ortamı (Temel ILB SKU kullanır)
* API Yönetimi (Temel ILB SKU kullanır)
* Azure AD DS (Temel ILB SKU kullanır)

Genel bakış gereksinimleri ve kısıtlamalar hakkında daha fazla bilgi edinmek için [Sanal ağ eşleme'sine](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints)bakın.

## <a name="troubleshoot-a-connectivity-issue-between-two-peered-virtual-networks"></a>İki eşlenen sanal ağ arasındaki bağlantı sorununu giderme

Gerekli [rolleri ve izinleri](virtual-network-manage-peering.md#permissions)içeren bir hesapla [Azure portalında](https://portal.azure.com/) oturum açın. Sanal ağı seçin, **Eşleme'yi**seçin ve ardından **Durum** alanını denetleyin. Durum nedir?

### <a name="the-peering-status-is-connected"></a>Eşleme durumu "Bağlı"

Bu sorunu gidermek için:

1. Ağ trafiği akışlarını kontrol edin:

   Trafik akışlarında parazite neden olan bir NSG veya UDR olup olmadığını belirlemek için [Bağlantı Sorun Giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-overview) ve IP akışını kaynağıvm'dan hedef VM'ye [doğrulayın.](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview)

   Güvenlik duvarı veya NVA kullanıyorsanız: 
   1. Bu adım tamamlandıktan sonra geri yükleyebilmeniz için UDR parametrelerini belgeleyin.
   2. UDR'yi bir sonraki atlama olarak NVA'yı işaret eden vm alt netinden veya NIC'den kaldırın. Kaynak VM'den nva'yı atlayan hedefe doğrudan bağlantı doğrulayın. Bu adım işe yaramazsa, [NVA sorun gidericibakın.](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)

2. Ağ izleme alın: 
   1. Hedef VM'de bir ağ izleme başlatın. Windows için **Netsh'i**kullanabilirsiniz. Linux için **TCPDump'ı**kullanın.
   2. **TcpPing** veya **PsPing'i** kaynaktan hedef IP'ye çalıştırın.

      Bu, **Bir TcpPing** komutu örneğidir:`tcping64.exe -t <destination VM address> 3389`

   3. **TcpPing** tamamlandıktan sonra, hedefteki ağ izini durdurun.
   4. Paketler kaynaktan geliyorsa, ağ sorunu yoktur. Yapılandırma sorununu bulmak için hem VM güvenlik duvarını hem de bu bağlantı noktasında dinleyen uygulamayı inceleyin.

   > [!Note]
   > Genel sanal ağ eşleme (farklı bölgelerdeki sanal ağlar) üzerinden aşağıdaki kaynak türlerine bağlanamazsınız:
   >
   > * Temel ILB SKU arkasında VMs
   > * Redis önbelleği (Temel ILB SKU kullanır)
   > * Uygulama ağ geçidi (Temel ILB SKU kullanır)
   > * Ölçek kümeleri (Temel ILB SKU kullanır)
   > * Servis Kumaş kümeleri (Temel ILB SKU kullanır)
   > * SQL Server Always On (Temel ILB SKU kullanır)
   > * Uygulama Hizmet Ortamı (Temel ILB SKU kullanır)
   > * API Yönetimi (Temel ILB SKU kullanır)
   > * Azure AD DS (Temel ILB SKU kullanır)

Daha fazla bilgi için, genel bakış gereksinimlerini [ve kısıtlamalarına](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) bakın.

### <a name="the-peering-status-is-disconnected"></a>Eşleme durumu "Kesildi"

Bu sorunu gidermek için, her iki sanal ağdan da eşlemi silin ve yeniden oluşturun.

## <a name="troubleshoot-a-connectivity-issue-between-a-hub-spoke-virtual-network-and-an-on-premises-resource"></a>Hub ile konuşan sanal ağ ve şirket içi kaynak arasındaki bağlantı sorununu giderme

Ağınız bir üçüncü taraf NVA veya VPN ağ geçidi kullanıyor mu?

### <a name="my-network-uses-a-third-party-nva-or-vpn-gateway"></a>Ağım üçüncü taraf NVA veya VPN ağ geçidi kullanır

Üçüncü taraf NVA veya VPN ağ geçidini etkileyen bağlantı sorunlarını gidermek için aşağıdaki makalelere bakın:

* [NVA sorun giderici](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-nva)
* [Servis zincirleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)

### <a name="my-network-does-not-use-a-third-party-nva-or-vpn-gateway"></a>Ağım üçüncü taraf NVA veya VPN ağ geçidi kullanmıyor

Hub sanal ağ ve konuşan sanal ağ vpn ağ geçidi var mı?

#### <a name="both-the-hub-virtual-network-and-the-spoke-virtual-network-have-a-vpn-gateway"></a>Hem hub sanal ağ ve kollu sanal ağ bir VPN ağ geçidi var

Uzak bir ağ geçidi kullanmak desteklenmez.

Konuşan sanal ağın zaten bir VPN ağ geçidi varsa, **uzaktan ağ geçidi kullan** seçeneği konuşan sanal ağda desteklenmez. Bunun nedeni sanal ağ izleme sınırlamasıdır.

#### <a name="both-the-hub-virtual-network-and-the-spoke-virtual-network-do-not-have-a-vpn-gateway"></a>Hem hub sanal ağ ve kollu sanal ağ vpn ağ geçidi yok

Siteden siteye veya Azure ExpressRoute bağlantılarında, şirket içinden uzak sanal ağa bağlantı sorunlarının aşağıdaki birincil nedenlerini kontrol edin:

* Ağ geçidi olan sanal ağda, **iletilen trafik** onay kutusunun seçildiğini doğrulayın.
* Ağ geçidi olmayan sanal ağda, **uzak ağ geçidi kullan** onay kutusunun seçili olduğunu doğrulayın.
* Ağ yöneticinizin, hepsinin uzak sanal ağ adresi alanı eklettiğini doğrulamak için şirket içi aygıtlarınızı kontrol etmesini sağlayın.

Noktadan siteye bağlantılar için:

* Ağ geçidi olan sanal ağda, **iletilen trafik** onay kutusunun seçildiğini doğrulayın.
* Ağ geçidi olmayan sanal ağda, **uzak ağ geçidi kullan** onay kutusunun seçili olduğunu doğrulayın.
* Noktaya işaret istemci paketini indirin ve yeniden yükleyin. Yeni bakan sanal ağ yolları, otomatik olarak siteden siteye istemcilere rota eklemez.

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-the-same-region"></a>Aynı bölgedeki konuşan sanal ağlar arasında hub ile konuşan ağ bağlantısı sorununu giderme

Hub ağı bir NVA içermelidir. Bir sonraki atlama olarak NVA ayarlanmış sözcülerde UdR'ları yapılandırın ve hub sanal ağında **iletilen trafiğe izin ver'i** etkinleştirin.

Daha fazla bilgi için [Hizmet zinciri bölümüne](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#service-chaining)bakın ve bu gereksinimleri seçtiğiniz [NVA satıcısıyla](https://support.microsoft.com/help/2984655/support-for-azure-market-place-for-virtual-machines) görüşün.

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-spoke-virtual-networks-in-different-regions"></a>Farklı bölgelerdeki konuşan sanal ağlar arasında hub destekli ağ bağlantısı sorununu giderme

Küresel sanal ağ izleme üzerinden geçiş artık desteklenir. Bağlantı, aşağıdaki kaynaklar için genel sanal ağ eşleme üzerinde çalışmaz:

* Temel ILB SKU arkasında VMs
* Redis önbelleği (Temel ILB SKU kullanır)
* Uygulama ağ geçidi (Temel ILB SKU kullanır)
* Ölçek kümeleri (Temel ILB SKU kullanır)
* Servis Kumaş kümeleri (Temel ILB SKU kullanır)
* SQL Server Always On (Temel ILB SKU kullanır)
* Uygulama Hizmet Ortamı (Temel ILB SKU kullanır)
* API Yönetimi (Temel ILB SKU kullanır)
* Azure AD DS (Temel ILB SKU kullanır)

Daha fazla bilgi için, küresel akran ve [Farklı VPN Topolojileri](https://blogs.msdn.microsoft.com/igorpag/2016/02/11/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-v2/) [gereksinimleri ve kısıtlamaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#requirements-and-constraints) bakın.

## <a name="troubleshoot-a-hub-spoke-network-connectivity-issue-between-a-web-app-and-the-spoke-virtual-network"></a>Bir web uygulaması ile konuşan sanal ağ arasındaki hub destekli ağ bağlantısı sorununu giderme

Bu sorunu gidermek için:

1. Azure Portal’da oturum açın. 
1. Web uygulamasında, **ağ seçeneğini**seçin ve ardından **VNet Tümleştirme'yi**seçin.
1. Uzak sanal ağı görüp göremeyeceğinizi kontrol edin. Uzak sanal ağ adres alanını el ile girin (**Ağ Eşitle** ve **Rota Ekle**).

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Uygulamanızı azure sanal ağıyla tümleştirme](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)
* [Noktadan Siteye VPN yönlendirme hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing)

## <a name="troubleshoot-a-virtual-network-peering-configuration-error-message"></a>Sanal ağ eşleme yapılandırma hata iletisi sorun giderme 

### <a name="current-tenant-tenant-id-isnt-authorized-to-access-linked-subscription"></a>Geçerli `<TENANT ID>` kiracının bağlantılı aboneye erişme yetkisi yok

Bu sorunu gidermek için [bkz.](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions#cli)

### <a name="not-connected"></a>Bağlı değil

Bu sorunu gidermek için, her iki sanal ağdan da eşlemi silin ve sonra yeniden oluşturun.

### <a name="failed-to-peer-a-databricks-virtual-network"></a>Databricks sanal ağına eş başarısız oldu

Bu sorunu gidermek için, Azure Veri **Tuğlaları**altında sanal ağ eşlemesini yapılandırın ve **kaynak kimliği**kullanarak hedef sanal ağı belirtin. Daha fazla bilgi için, [uzak bir sanal ağa Peer a Databricks sanal ağ](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-peering.html#id2)bakın.

### <a name="the-remote-virtual-network-lacks-a-gateway"></a>Uzak sanal ağ da ağ geçidinden yoksundur

Bu sorun, farklı kiracılardan sanal ağlar eş ve daha `Use Remote Gateways`sonra yapılandırmak istediğinizde oluşur. Azure portalının bir sınırlaması, başka bir kiracının sanal ağında sanal ağ geçidinin varlığını doğrulayamaabilmesidir.

Sorunu çözmenin iki yolu vardır:

 * Yeni bir eşleme oluşturduğunuzda eşlemeleri silin ve `Use Remote Gateways` seçeneği etkinleştirin.
 * Etkinleştirmek `Use Remote Gateways`için Azure portalı yerine PowerShell veya CLI'yi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM’leri arasında bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)
