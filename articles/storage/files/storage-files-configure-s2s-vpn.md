---
title: Azure Dosyaları ile kullanılmak üzere Siteden Siteye (S2S) VPN yapılandırın | Microsoft Dokümanlar
description: Azure Dosyaları ile kullanılmak üzere Siteden Siteye (S2S) VPN nasıl yapılandırılasınız?
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ae3d38d92990d7a1af4146c25b017286ebd29352
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061034"
---
# <a name="configure-a-site-to-site-vpn-for-use-with-azure-files"></a>Azure Dosyaları ile kullanılmak üzere Siteden Siteye VPN yapılandırma
445 bağlantı noktasını açmadan Azure dosya paylaşımlarınızı şirket içi ağınızdan Kobİ üzerinden monte etmek için Siteden Siteye (S2S) VPN bağlantısı kullanabilirsiniz. VPN hizmetleri sunan bir Azure kaynağı olan ve depolama hesapları veya diğer Azure kaynaklarının yanı sıra bir kaynak grubunda dağıtılan [Azure VPN Ağ Geçidi'ni](../../vpn-gateway/vpn-gateway-about-vpngateways.md)kullanarak Siteden Siteye VPN ayarlayabilirsiniz.

![Bir Azure dosyası paylaşımını s2S VPN kullanarak şirket içi bir siteye bağlayan bir Azure VPN ağ geçidinin topolojisini gösteren bir topoloji grafiği](media/storage-files-configure-s2s-vpn/s2s-topology.png)

Azure Dosyaları için kullanılabilir ağ seçeneklerinin tam olarak tartışılması için makaleye devam etmeden önce [Azure Dosyaları ağ genel görünümünü](storage-files-networking-overview.md) okumanızı öneririz.

Makalede, Azure dosya paylaşımlarını doğrudan şirket içinde monte etmek için Siteden Siteye VPN'i yapılandırma adımları ayrıntılı olarak anlatılıyor. Azure Dosya Eşitleme trafiğini Site'den Siteye VPN üzerinden yönlendirmek istiyorsanız, lütfen [Azure Dosya Eşitleme proxy'sini ve güvenlik duvarı ayarlarını yapılandırmaya](storage-sync-files-firewall-and-proxy.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
- Şirket içinde monte etmek istediğiniz bir Azure dosya paylaşımı. Azure dosya paylaşımları, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapıları olan depolama hesaplarında dağıtılır. Azure dosya paylaşımı oluştur'da Azure dosya paylaşımlarını ve depolama hesaplarını nasıl dağıtabileceğiniz hakkında daha fazla bilgi [edinebilirsiniz.](storage-how-to-create-file-share.md)

- Şirket içinde monte etmek istediğiniz Azure dosya paylaşımını içeren depolama hesabı için özel bir bitiş noktası. Özel bir bitiş noktası oluşturma hakkında daha fazla bilgi edinmek için Azure [Dosyaları ağ uç noktalarını yapılandırma](storage-files-networking-endpoints.md?tabs=azure-portal)konusuna bakın. 

- Şirket içi veri merkezinizde Azure VPN Ağ Geçidi ile uyumlu bir ağ cihazı veya sunucu. Azure Files, seçilen şirket içi ağ aygıtının agnostikidir, ancak Azure VPN Ağ Geçidi [test edilmiş aygıtların](../../vpn-gateway/vpn-gateway-about-vpn-devices.md)bir listesini tutar. Farklı ağ cihazları farklı özellikler, performans özellikleri ve yönetim işlevleri sunar, bu nedenle bir ağ cihazı seçerken bunları göz önünde bulundurun.

    Varolan bir ağ aygıtınız yoksa, Windows Server yerleşik Sunucu Rolü, Yönlendirme ve Uzaktan Erişim (RRAS) içerir ve bu cihaz şirket içi ağ aygıtı olarak kullanılabilir. Windows Server'da Yönlendirme ve Uzaktan Erişim'i yapılandırma hakkında daha fazla bilgi edinmek için [RAS Ağ Geçidi'ne](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)bakın.

## <a name="add-storage-account-to-vnet"></a>VNet'e depolama hesabı ekleme
Azure portalında, şirket içinde monte etmek istediğiniz Azure dosya paylaşımını içeren depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Güvenlik Duvarları ve sanal ağlar** girişini seçin. Oluşturduğunuzda depolama hesabınıza sanal bir ağ eklemediğiniz sürece, ortaya çıkan bölmede **tüm ağlar** için **radyodan erişime izin** verme düğmesi olmalıdır.

Depolama hesabınızı istenilen sanal ağa eklemek için **Seçili ağları**seçin. Sanal **ağlar** alt başlığı altında, + **Varolan sanal ağ ekle** veya istenen duruma bağlı olarak **+Yeni sanal ağ ekle'yi** tıklatın. Yeni bir sanal ağ oluşturmak, yeni bir Azure kaynağının oluşturulmasına neden olur. Yeni veya varolan VNet kaynağının depolama hesabıyla aynı kaynak grubunda veya abonelikte olması gerekmez, ancak depolama hesabı ve VNet'inizi dağıttığınız kaynak grubu ve aboneliğin aynı bölgede olması gerekir. VPN Ağ Geçidinizi dağıtın. 

![Depolama hesabına varolan veya yeni bir sanal ağ ekleme seçeneği veren Azure portalının ekran görüntüsü](media/storage-files-configure-s2s-vpn/add-vnet-1.png)

Varolan sanal ağı eklerseniz, depolama hesabının eklenmesi gereken sanal ağın bir veya daha fazla alt ağını seçmeniz istenir. Yeni bir sanal ağ seçerseniz, sanal ağın oluşturulmasının bir parçası olarak bir alt ağ oluşturursunuz ve sanal ağ için ortaya çıkan Azure kaynağı aracılığıyla daha sonra ekleyebilirsiniz.

Aboneliğinize daha önce bir depolama hesabı eklemediyseniz, Microsoft.Storage hizmet bitiş noktasının sanal ağa eklenmesi gerekir. Bu işlem biraz zaman alabilir ve bu işlem tamamlanana kadar VPN bağlantısı da dahil olmak üzere bu depolama hesabındaki Azure dosya paylaşımlarına erişemeyeceksiniz. 

## <a name="deploy-an-azure-vpn-gateway"></a>Azure VPN Ağ Geçidi dağıtma
Azure portalının içindekiler tablosunda **yeni bir kaynak oluştur'u** ve *Sanal ağ ağ ağ geçidini*arayın'ı seçin. Sanal ağ ağ ağ geçidiniz, önceki adımda dağıttığınız sanal ağla aynı abonelikte, Azure bölgesinde ve kaynak grubunda olmalıdır (sanal ağ seçildiğinde kaynak grubunun otomatik olarak seçildiğini unutmayın). 

Azure VPN Ağ Geçidi dağıtmak amacıyla aşağıdaki alanları doldurmanız gerekir:

- **Ad**: VPN Ağ Geçidi için Azure kaynağının adı. Bu ad, yönetiminiz için yararlı bulduğunuz herhangi bir ad olabilir.
- **Bölge**: VPN Ağ Geçidinin dağıtılanacağı bölge.
- **Ağ Geçidi türü**: Siteden Siteye VPN dağıtmak amacıyla **VPN'i**seçmeniz gerekir.
- **VPN türü**: VPN cihazınıza bağlı olarak *Rota tabanlı** veya **Politika tabanlı** birini seçebilirsiniz. Rota tabanlı VPN'ler IKEv2'yi desteklerken, politika tabanlı VPN'ler yalnızca IKEv1'i destekler. İki VPN ağ geçidi türü hakkında daha fazla bilgi edinmek [için, politika tabanlı ve rota tabanlı VPN ağ geçitleri hakkında](../../vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md#about) bilgi edinin
- **SKU**: SKU, izin verilen Site-To-Site tünellerinin sayısını ve VPN'in istenilen performansını kontrol eder. Kullanım durumunuz için uygun SKU'yu seçmek için [Ağ Geçidi SKU](../../vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) listesine başvurun. GEREKIRSE VPN Ağ Geçidi'nin SKU'su daha sonra değiştirilebilir.
- **Sanal ağ**: Önceki adımda oluşturduğunuz sanal ağ.
- **Genel IP adresi**: Internet'e açık olacak VPN Ağ Geçidi'nin IP adresidir. Büyük olasılıkla, yeni bir IP adresi oluşturmanız gerekir, ancak uygunsa mevcut kullanılmayan bir IP adresi de kullanabilirsiniz. Yeni , yeni bir IP adresi **oluşturmayı**seçerseniz, VPN Ağ Geçidi ile aynı kaynak grubunda yeni bir IP adresi oluşturulacaktır ve yeni oluşturulan IP adresinin **adı Genel IP adresi** nin adı olacaktır. **Varolan Kullan'ı**seçerseniz, varolan kullanılmayan IP adresini seçmeniz gerekir.
- **Etkin etkin modu etkinleştir**: Yalnızca etkin etkin ağ geçidi yapılandırması oluşturuyorsanız **Etkin etkinleştir'i** seçin, aksi takdirde **Devre Dışı** bırakılmış olarak seçilsin. Etkin-etkin mod hakkında daha fazla bilgi edinmek için, [yüksek kullanılabilir binalar arası ve VNet'ten VNet'e bağlantı](../../vpn-gateway/vpn-gateway-highlyavailable.md)ya da
- **BGP ASN'yi yapılandırın**: Yalnızca yapılandırmanız özellikle bu ayarı gerektiriyorsa **Etkin'i** seçin. Bu ayar hakkında daha fazla bilgi edinmek için [Azure VPN Ağ Geçidi ile BGP Hakkında'ya](../../vpn-gateway/vpn-gateway-bgp-overview.md)bakın.

VPN Ağ Geçidi'ni oluşturmak için **Gözden Geçir + oluştur'u** seçin. VPN Ağ Geçidi'nin tam olarak oluşturulması ve dağıtılması 45 dakika kadar sürebilir.

### <a name="create-a-local-network-gateway-for-your-on-premises-gateway"></a>Şirket içi ağ geçidiniz için yerel ağ ağ geçidi oluşturma 
Yerel ağ ağ geçidi, şirket içi ağ cihazınızı temsil eden bir Azure kaynağıdır. Azure portalının içindekiler tablosunda **yeni bir kaynak oluştur'u** ve *yerel ağ ağ ağ geçidini arayın'ı*seçin. Yerel ağ ağ ağ geçidi, depolama hesabınız, sanal ağınız ve VPN Ağ Geçidinizin yanında dağıtılacak, ancak depolama hesabıyla aynı kaynak grubunda veya abonelikte olması gerekmeyen bir Azure kaynağıdır. 

Yerel ağ ağ geçidi kaynağını dağıtmak amacıyla aşağıdaki alanları doldurmanız gerekir:

- **Ad**: Yerel ağ ağ geçidi için Azure kaynağının adı. Bu ad, yönetiminiz için yararlı bulduğunuz herhangi bir ad olabilir.
- **IP adresi**: Yerel ağ geçidinizin genel IP adresi.
- **Adres alanı**: Bu yerel ağ ağ geçidinin temsil eder ağ için adres aralıkları. Birden çok adres alanı aralığı ekleyebilirsiniz, ancak burada belirttiğiniz aralıkların bağlanmak istediğiniz diğer ağ aralıklarıyla örtüşmediğinden emin olun. 
- **BGP ayarlarını yapılandırın**: Yalnızca yapılandırmanız bu ayarı gerektiriyorsa BGP ayarlarını yapılandırın. Bu ayar hakkında daha fazla bilgi edinmek için [Azure VPN Ağ Geçidi ile BGP Hakkında'ya](../../vpn-gateway/vpn-gateway-bgp-overview.md)bakın.
- **Abonelik**: İstenilen abonelik. Bunun VPN Ağ Geçidi veya depolama hesabı için kullanılan abonelikle eşleşmesi gerekmez.
- **Kaynak grubu**: İstenilen kaynak grubu. Bunun VPN Ağ Geçidi veya depolama hesabı için kullanılan kaynak grubuyla eşleşmesi gerekmez.
- **Konum**: Azure Bölgesi yerel ağ ağ geçidi kaynağı oluşturulmalıdır. Bu, VPN Ağ Geçidi ve depolama hesabı için seçtiğiniz bölgeyle eşleşmelidir.

Yerel ağ ağ geçidi kaynağını oluşturmak için **Oluştur'u** seçin.  

## <a name="configure-on-premises-network-appliance"></a>Şirket içi ağ cihazını yapılandırma
Şirket içi ağ cihazınızı yapılandırmak için belirli adımlar, kuruluşunuzun seçtiği ağ cihazına bağlıdır. Kuruluşunuzun seçtiği aygıta bağlı olarak, [test edilen aygıtlar listesinde](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) Azure VPN Ağ Geçidi ile yapılandırma için aygıt satıcınızın talimatlarına bir bağlantı bulunabilir.

## <a name="create-the-site-to-site-connection"></a>Siteden Siteye bağlantı oluşturma
Bir S2S VPN dağıtımıtamamlamak için, şirket içi ağ cihazınız (yerel ağ ağ geçidi kaynağı tarafından temsil edilir) ve VPN Ağ Geçidi arasında bir bağlantı oluşturmanız gerekir. Bunu yapmak için, yukarıda oluşturduğunuz VPN Ağ Geçidi'ne gidin. VPN Ağ Geçidi'nin içindekiler tablosunda **Bağlantılar'ı**seçin ve **Ekle'yi**tıklatın. Ortaya çıkan **Bağlantı Ekle** bölmesi aşağıdaki alanları gerektirir:

- **Adı**: Bağlantının adı. VPN Ağ Geçidi birden çok bağlantı barındırabilir, bu nedenle yönetiminiz için bu bağlantıyı ayırt edecek yararlı bir ad seçin.
- **Bağlantı türü**: Bu bir S2S bağlantısı olduğundan, açılan listede **Site'den siteye (IPSec)** seçeneğini belirleyin.
- **Sanal ağ ağ geçidi**: Bu alan, bağlantı yaptığınız VPN Ağ Geçidi'ne otomatik olarak seçilir ve değiştirilemez.
- **Yerel ağ ağ geçidi**: VPN Ağ Geçidi'ne bağlanmak istediğiniz yerel ağ ağ geçididir. Ortaya çıkan seçim bölmesi, yukarıda oluşturduğunuz yerel ağ ağ geçidinin adını almalıdır.
- **Paylaşılan anahtar (PSK)**: Bağlantı için şifreleme oluşturmak için kullanılan harf ve sayıların karışımıdır. Aynı paylaşılan anahtar hem sanal ağda hem de yerel ağ ağ geçitlerinde kullanılmalıdır. Ağ geçidi aygıtınız bir tane sağlamazsa, buradan bir tane yapabilir ve cihazınıza sağlayabilirsiniz.

Bağlantıyı oluşturmak için **Tamam'ı** seçin. **Bağlantının Bağlantılar** sayfasından başarıyla yapıldığını doğrulayabilirsiniz.

## <a name="mount-azure-file-share"></a>Azure dosya paylaşımını başlat 
Bir S2S VPN'i yapılandırmanın son adımı Azure Dosyaları için çalıştığını doğrulamaktır. Bunu, Azure dosya paylaşımınızı tercih ettiğiniz işletim sistemiyle şirket içinde monte ederek yapabilirsiniz. İşletim sistemi ile monte etmek için talimatlara buradan bakın:

- [Windows](storage-how-to-use-files-windows.md)
- [Macos](storage-how-to-use-files-mac.md)
- [Linux](storage-how-to-use-files-linux.md)

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları ağına genel bakış](storage-files-networking-overview.md)
- [Azure Dosyaları ile kullanılmak üzere Windows'da Bir Noktaya Sayfa (P2S) VPN'i yapılandırma](storage-files-configure-p2s-vpn-windows.md)
- [Azure Dosyaları ile kullanılmak üzere Linux'ta Bir Noktadan Siteye (P2S) VPN yapılandırın](storage-files-configure-p2s-vpn-linux.md)