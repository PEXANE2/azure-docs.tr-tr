---
title: Azure Dosyaları ağ la ilgili hususlar | Microsoft Dokümanlar
description: Azure Dosyaları için ağ seçeneklerine genel bakış.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 02/22/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 383ad5e5063a0a207320a517c34f3b41cc57804a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067162"
---
# <a name="azure-files-networking-considerations"></a>Azure Dosyaları ağ la ilgili hususlar 
Bir Azure dosya paylaşımına iki şekilde bağlanabilirsiniz:

- Doğrudan SMB veya FileREST protokolleri aracılığıyla paylaşıma erişim. Bu erişim deseni, mümkün olduğunca çok şirket içi sunucuyu ortadan kaldırmak için öncelikle kullanılır.
- Azure Dosya Eşitlemi ile şirket içi bir sunucuda Azure dosya paylaşımının önbelleği oluşturma ve kullanım durumunuz için seçtiğiniz protokolle (SMB, NFS, FTPS, vb.) şirket içi sunucudan dosya paylaşımıverilerine erişme. Bu erişim deseni kullanışlıdır, çünkü hem şirket içi performansın hem de bulut ölçeğinin ve Azure Yedekleme gibi sunucusuz eklenebilir hizmetlerin en iyilerini birleştirir.

Bu makalede, kullanım durumunuz Azure Dosya Eşitlemi yerine doğrudan Azure dosya paylaşımına erişmeyi gerektirdiğinde ağ kullanımını nasıl yapılandırılacaya odaklanıldığı açıklanmaktadır. Azure Dosya Eşitleme dağıtımı için ağ la ilgili konular hakkında daha fazla bilgi için Azure [Dosya Eşitleme proxy'si ve güvenlik duvarı ayarlarını yapılandırma](storage-sync-files-firewall-and-proxy.md)konusuna bakın.

Azure dosya paylaşımları için ağ yapılandırması Azure depolama hesabında yapılır. Depolama hesabı, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapısıdır. Depolama hesapları, dosya paylaşımlarınıza ağ erişimini sağlamanıza yardımcı olan birden çok ayarı ortaya çıkarır: ağ uç noktaları, depolama hesabı güvenlik duvarı ayarları ve aktarım sırasında şifreleme. 

Bu kavramsal kılavuzu okumadan önce [Azure Dosyaları dağıtımı için Planlama'yı](storage-files-planning.md) okumanızı öneririz.

## <a name="accessing-your-azure-file-shares"></a>Azure dosya paylaşımlarınıza erişme
Bir depolama hesabında bir Azure dosya paylaşımı dağıttığınızda, dosya paylaşımınıza depolama hesabının genel bitiş noktası üzerinden anında erişilebilir. Bu, bir kullanıcının oturum açma kimliği tarafından yetkilendirilen istekler gibi kimlik doğrulama isteklerinin Azure'un içinden veya dışından güvenli bir şekilde kaynaklanabileceği anlamına gelir. 

Birçok müşteri ortamlarında, Azure VM'lerden gelen bağlar başarılı olsa bile, şirket içi iş istasyonunuzdaki Azure dosya paylaşımının ilk yuvası başarısız olur. Bunun nedeni, birçok kuruluş ve internet servis sağlayıcıları (ISS) SMB iletişim kurmak için kullandığı bağlantı noktası, 445 engellemek olduğunu. Bu uygulama, Kobİ protokolünün eski ve küçümdelenmiş sürümleri yle ilgili güvenlik kılavuzundan kaynaklanır. SMB 3.0 internet güvenli bir protokol olmasına rağmen, SMB eski sürümleri, özellikle SMB 1.0 değildir. Azure dosya paylaşımlarına yalnızca SMB 3.0 ve FileREST protokolü (aynı zamanda internet güvenli protokolü) üzerinden genel bitiş noktası üzerinden dışarıdan erişilebilir.

Azure dosya paylaşımınıza şirket içinde erişmenin en kolay yolu şirket içi ağınızı 445 nolu bağlantı noktasına açmak olduğundan, Microsoft SMB 1.0'ı ortamınızdan kaldırmak için aşağıdaki adımları önerir:

1. SMB 1.0'ın kuruluşunuzun aygıtlarında kaldırıldığından veya devre dışı bırakıldığından emin olun. Şu anda desteklenen Windows ve Windows Server sürümleri, SMB 1.0'ı kaldırmayı veya devre dışı bırakmayı destekler ve Windows 10, sürüm 1709, SMB 1.0 ile başlayarak varsayılan olarak Windows'a yüklenmez. SMB 1.0'ı nasıl devre dışı dışı köttük hakkında daha fazla bilgi edinmek için işletim sistemi yle ilgili sayfalarımıza bakın:
    - [Windows/Windows Server'ı güvenli hale getirme](storage-how-to-use-files-windows.md#securing-windowswindows-server)
    - [Linux'u Güvence Altına Alma](storage-how-to-use-files-linux.md#securing-linux)
2. Kuruluşunuzdaki hiçbir ürünün SMB 1.0 gerektirmediğinden emin olun ve bunu yapanürünleri kaldırın. Microsoft'un SMB 1.0'a ihtiyaç duyduğu bilinen tüm birinci ve üçüncü taraf ürünlerini içeren bir [SMB1 Ürün Takas Evi'ne](https://aka.ms/stillneedssmb1)sahip yiz. 
3. (İsteğe bağlı) SMB 1.0 trafiğinin kuruluş sınırınızı terk etmesini önlemek için kuruluşunuzun şirket içi ağıyla birlikte bir üçüncü taraf güvenlik duvarı kullanın.

Kuruluşunuz 445 bağlantı noktasının ilke veya yönetmelik başına engellenmesini gerektiriyorsa veya kuruluşunuz deterministik bir yolu izlemek için Azure trafiğini gerektiriyorsa, Azure dosya paylaşımlarınıza trafik tüneli açmak için Azure VPN Ağ Geçidi veya ExpressRoute'u kullanabilirsiniz.

> [!Important]  
> Azure dosya paylaşımlarınıza erişmek için alternatif bir yöntem kullanmaya karar verseniz bile, Microsoft yine de SMB 1.0'ı ortamınızdan kaldırmanızı önerir.

### <a name="tunneling-traffic-over-a-virtual-private-network-or-expressroute"></a>Sanal özel ağ veya ExpressRoute üzerinden trafik tünelleme
Şirket içi ağınızla Azure arasında bir ağ tüneli oluşturduğunuzda, şirket içi ağınızı Azure'da bir veya daha fazla sanal ağla karşılarsınız. [Sanal ağ](../../virtual-network/virtual-networks-overview.md)veya VNet, şirket içinde çalıştırabileceğiniz geleneksel ağa benzer. Azure depolama hesabı veya Azure VM gibi, VNet de kaynak grubunda dağıtılan bir Azure kaynağıdır. 

Azure Dosyaları, şirket içi iş istasyonlarınız ve sunucularınız ile Azure arasındaki trafiği tünellemek için aşağıdaki mekanizmaları destekler:

- [Azure VPN Ağ Geçidi](../../vpn-gateway/vpn-gateway-about-vpngateways.md): VPN ağ geçidi, Bir Azure sanal ağı ile internet üzerinden alternatif bir konum (şirket içi gibi) arasında şifreli trafik göndermek için kullanılan belirli bir sanal ağ ağ geçidi türüdür. Azure VPN Ağ Geçidi, bir depolama hesabı veya diğer Azure kaynaklarıyla birlikte bir kaynak grubunda dağıtılabilen bir Azure kaynağıdır. VPN ağ geçitleri iki farklı bağlantı türünü ortaya çıkarır:
    - Azure ile tek bir istemci arasındaki VPN bağlantıları [olan, Site'ye noktalı (P2S) VPN](../../vpn-gateway/point-to-site-about.md) ağ geçidi bağlantıları. Bu çözüm, öncelikle kuruluşunuzun şirket içi ağının bir parçası olmayan cihazlar (örneğin, Azure dosya paylaşımlarını evden, kafeden veya otelden yola çıkarken monte etmek isteyen yolcular gibi) için yararlıdır. Azure Dosyaları ile Bir P2S VPN bağlantısı kullanmak için, bağlanmak isteyen her istemci için bir P2S VPN bağlantısının yapılandırılması gerekir. Bir P2S VPN bağlantısının dağıtımını kolaylaştırmak için, [Azure Dosyaları ile kullanılmak üzere Windows'da Bir Noktadan (P2S) VPN yapılandır'a](storage-files-configure-p2s-vpn-windows.md) bakın ve Azure Dosyaları ile kullanılmak üzere [Linux'ta Bir Noktadan Siteye (P2S) VPN yapılandırın.](storage-files-configure-p2s-vpn-linux.md)
    - Azure ile kuruluşunuzun ağı arasındaki VPN bağlantıları olan [Siteden Siteye (S2S) VPN.](../../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) S2S VPN bağlantısı, Azure dosya paylaşımınıza erişmesi gereken her istemci aygıtı için yapmak yerine, kuruluşunuzun ağında barındırılan bir VPN sunucusu veya aygıt için bir KEZ VPN bağlantısını yapılandırmanızı sağlar. S2S VPN bağlantısının dağıtımını kolaylaştırmak için Azure [Dosyaları'nda kullanılmak üzere Siteden Siteye (S2S) VPN Yapılandır'a](storage-files-configure-s2s-vpn.md)bakın.
- Azure ile internetüzerinden geçmeyen şirket içi ağınız arasında tanımlı bir rota oluşturmanıza olanak tanıyan [ExpressRoute.](../../expressroute/expressroute-introduction.md) ExpressRoute şirket içi veri merkeziniz ile Azure arasında özel bir yol sağladığından, ExpressRoute ağ performansı göz önünde bulundurulduğunda yararlı olabilir. ExpressRoute, kuruluşunuzun ilke veya düzenleyici gereksinimleri buluttaki kaynaklarınız için belirleyici bir yol gerektirdiğinde de iyi bir seçenektir.

Azure dosya paylaşımlarınıza erişmek için hangi tünel yöntemini kullandığınızdan bağımsız olarak, depolama hesabınıza giden trafiğin normal internet bağlantınız yerine tünelden geçtiğini emin olacak bir mekanizmaya ihtiyacınız vardır. Depolama hesapları herhangi bir zamanda depolama kümeleri arasında taşınabileceğinden, depolama hesabının genel bitiş noktasına yönlendirmek teknik olarak mümkündür, ancak bu, bir bölgedeki Azure depolama kümelerinin tüm IP adreslerinin zor kodlanmasını gerektirir. Bu, her zaman yeni kümeler eklenmiştir beri IP adresi eşlemeleri sürekli olarak güncelleştirilmesi ni gerektirir.

Depolama hesaplarınızın IP adresini VPN yönlendirme kurallarınıza sabit kodlamak yerine, depolama hesabınıza bir Azure sanal ağının adres alanından bir IP adresi veren özel uç noktaları kullanmanızı öneririz. Azure'a tünel oluşturmak, şirket içi ağınızla bir veya daha fazla sanal ağ arasında bir bakış oluşturma yı sağladığından, bu durum doğru yönlendirmeyi dayanıklı bir şekilde sağlar.

### <a name="private-endpoints"></a>Özel uç noktalar
Azure Dosyaları, bir depolama hesabı için varsayılan genel bitiş noktasına ek olarak, bir veya daha fazla özel uç noktaya sahip olmak seçeneği sunar. Özel bitiş noktası, yalnızca Bir Azure sanal ağında erişilebilen bir bitiş noktasıdır. Depolama hesabınız için özel bir bitiş noktası oluşturduğunuzda, depolama hesabınız sanal ağınızın adres alanı içinden özel bir IP adresi alır, örneğin şirket içi dosya sunucusunun veya NAS aygıtının ilgili adreste ip adresi alması gibi şirket içi ağınızın alanı. 

Tek bir özel bitiş noktası belirli bir Azure sanal ağ alt ağıyla ilişkilidir. Bir depolama hesabının birden fazla sanal ağda özel uç noktaları olabilir.

Azure Dosyaları ile özel uç noktaları kullanmak şunları yapmanızı sağlar:
- Özel bakışlı vpn veya ExpressRoute bağlantısı kullanarak şirket içi ağlardan Azure dosya paylaşımlarınıza güvenli bir şekilde bağlanın.
- Ortak bitiş noktasındaki tüm bağlantıları engelleyecek şekilde depolama hesabı güvenlik duvarını yapılandırarak Azure dosya paylaşımlarınızı güvence altına alın. Varsayılan olarak, özel bir bitiş noktası oluşturmak ortak bitiş noktasına olan bağlantıları engellemez.
- Sanal ağdan (ve eşleme sınırlarından) veri sızmasını engellemenizi sağlayarak sanal ağ için güvenliği artırın.

Özel bir bitiş noktası oluşturmak için Azure [Dosyaları için özel uç nokta yapılandırma](storage-files-networking-endpoints.md)konusuna bakın.

### <a name="private-endpoints-and-dns"></a>Özel uç noktalar ve DNS
Özel bir bitiş noktası oluşturduğunuzda, varsayılan olarak `privatelink` alt etki alanına karşılık gelen özel bir DNS bölgesi de oluştururuz . Açık konuşmak gerekirse, depolama hesabınız için özel bir bitiş noktası kullanmak için özel bir DNS bölgesi oluşturmak gerekli değildir, ancak Genel olarak önerilir ve Azure dosya payınızı Bir Active Directory kullanıcı ilkesiyle oluştururken veya erişimde açıkça gereklidir FileREST API'den.

> [!Note]  
> Bu makalede, Azure Genel bölgeleri için depolama hesabı DNS soneki kullanır. `core.windows.net` Bu yorum, Azure ABD Devlet bulutu ve Azure Çin bulutu gibi Azure Sovereign bulutları için de geçerlidir - ortamınız için uygun soneklerin yerine geçer. 

Özel DNS bölgenizde, depolama hesabının `storageaccount.privatelink.file.core.windows.net` normal adı için bir A kaydı ve deseni `storageaccount.file.core.windows.net`izleyen bir CNAME kaydı oluştururuz. Azure özel DNS bölgeniz özel bitiş noktasını içeren sanal ağa bağlı olduğundan, PowerShell'den `Resolve-DnsName` cmdlet'i bir Azure VM'de (dönüşümlü olarak `nslookup` Windows ve Linux'ta) arayarak DNS yapılandırmasını gözlemleyebilirsiniz:

```powershell
Resolve-DnsName -Name "storageaccount.file.core.windows.net"
```

Bu örnekte, depolama `storageaccount.file.core.windows.net` hesabı özel bitiş noktasının özel IP adresine `192.168.0.4`gider, bu da .

```Output
Name                              Type   TTL   Section    NameHost
----                              ----   ---   -------    --------
storageaccount.file.core.windows. CNAME  29    Answer     csostoracct.privatelink.file.core.windows.net
net

Name       : storageaccount.privatelink.file.core.windows.net
QueryType  : A
TTL        : 1769
Section    : Answer
IP4Address : 192.168.0.4


Name                   : privatelink.file.core.windows.net
QueryType              : SOA
TTL                    : 269
Section                : Authority
NameAdministrator      : azureprivatedns-host.microsoft.com
SerialNumber           : 1
TimeToZoneRefresh      : 3600
TimeToZoneFailureRetry : 300
TimeToExpiration       : 2419200
DefaultTTL             : 300
```

Aynı komutu şirket içinde çalıştırıyorsanız, aynı depolama hesabı adının depolama hesabının genel IP adresine çözüldüğünü görürsünüz; `storageaccount.file.core.windows.net` depolama hesabını barındıran `storageaccount.privatelink.file.core.windows.net`Azure depolama kümesi için cname kaydı olan bir CNAME kaydıdır:

```Output
Name                              Type   TTL   Section    NameHost
----                              ----   ---   -------    --------
storageaccount.file.core.windows. CNAME  60    Answer     storageaccount.privatelink.file.core.windows.net
net
storageaccount.privatelink.file.c CNAME  60    Answer     file.par20prdstr01a.store.core.windows.net
ore.windows.net

Name       : file.par20prdstr01a.store.core.windows.net
QueryType  : A
TTL        : 60
Section    : Answer
IP4Address : 52.239.194.40
```

Bu, depolama hesabının hem ortak bitiş noktasını hem de bir veya daha fazla özel bitiş noktasını ortaya çıkarabileceği gerçeğini yansıtır. Depolama hesabı adının özel bitiş noktasının özel IP adresine göre çözüldüğünden emin olmak için şirket içi DNS sunucularınızdaki yapılandırmayı değiştirmeniz gerekir. Bu çeşitli şekillerde gerçekleştirilebilir:

- İstenilen özel bitiş noktasının özel IP adresine çözüm bulmak `storageaccount.file.core.windows.net` için istemcilerinizin ana bilgisayar dosyasını değiştirme. Azure dosya paylaşımlarınızı monte etmek isteyen her istemcide bu değişiklikleri yapmanız gerektiğinden ve depolama hesabında veya özel bitiş noktasındayapılan değişiklikler otomatik olarak işlenmediğinden, bu durum üretim ortamları için kesinlikle önerilmez.
- Şirket içi DNS sunucularınızda a `storageaccount.file.core.windows.net` kaydı oluşturma. Bu, şirket içi ortamınızdaki istemcilerin her istemciyi yapılandırmaya gerek kalmadan depolama hesabını otomatik olarak çözebilme avantajına sahiptir, ancak değişiklikler olmadığı için bu çözüm ana bilgisayar dosyasını değiştirmeye benzer şekilde kırılgandır Yansıyan. Bu çözüm kırılgan olmasına rağmen, bazı ortamlar için en iyi seçim olabilir.
- `core.windows.net` Bölgeyi şirket içi DNS sunucularınızdan Azure özel DNS bölgenize iletin. Azure özel DNS ana bilgisayara yalnızca Azure`168.63.129.16`özel DNS bölgesine bağlı sanal ağlarda erişilebilen özel bir IP adresi () üzerinden ulaşılabilir. Bu sınırlamayı geçici olarak çözüme getirmek için, sanal ağınızda `core.windows.net` Azure özel DNS bölgesine iletilecek ek DNS sunucuları çalıştırabilirsiniz. Bu kurulumu basitleştirmek için, Azure sanal ağınızdaki DNS sunucularını otomatik olarak dağıtacak ve bunları istediğimiz şekilde yapılandıracak PowerShell cmdlets sağladık. DNS iletmesini nasıl ayarlayabilirsiniz öğrenmek için [DNS'yi Azure Dosyalarıyla Yapılandırma'ya](storage-files-networking-dns.md)bakın.

## <a name="storage-account-firewall-settings"></a>Depolama hesabı güvenlik duvarı ayarları
Güvenlik duvarı, depolama hesabıiçin ortak bitiş noktasına hangi isteklerin erişmesine izin verildiğini denetleyen bir ağ ilkesidir. Depolama hesabı güvenlik duvarını kullanarak, depolama hesabının genel bitiş noktasına erişimi belirli IP adresleri veya aralıkları veya sanal ağ ile kısıtlayabilirsiniz. Genel olarak, bir depolama hesabı için çoğu güvenlik duvarı ilkeleri, ağ daki bir veya daha fazla sanal ağa erişimi kısıtlar. 

Bir depolama hesabına erişimi sanal ağla sınırlamak için iki yaklaşım vardır:
- Depolama hesabı için bir veya daha fazla özel uç nokta oluşturun ve tüm genel bitiş noktasına erişimi kısıtlayın. Bu, yalnızca istenen sanal ağlar içinden kaynaklanan trafiğin depolama hesabı içindeki Azure dosya paylaşımlarına erişmesini sağlar.
- Ortak bitiş noktasını bir veya daha fazla sanal ağla sınırlandırın. Bu, *hizmet bitiş noktaları*olarak adlandırılan sanal ağın bir yeteneğini kullanarak çalışır. Bir hizmet bitiş noktası aracılığıyla trafiği bir depolama hesabıyla sınırladığınızı, yine de ortak IP adresi üzerinden depolama hesabına erişmeye devam esiniz.

Depolama hesabı güvenlik duvarını nasıl yapılandırılabildiğini öğrenmek için Azure [depolama güvenlik duvarlarını ve sanal ağları yapılandırma](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)ya da yapılandırma bilgisini edinin.

## <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Varsayılan olarak, tüm Azure depolama hesaplarının aktarım özelliği etkin bir şifrelemeye sahiptir. Bu, bir dosya paylaşımını SMB üzerinden bağladığınızveya FileREST protokolü (Azure portalı, PowerShell/CLI veya Azure SDK'ları gibi) aracılığıyla eriştiğınızda, Azure Dosyaları'nın yalnızca şifreleme veya HTTPS ile SMB 3.0+ ile yapıldığında bağlantıya izin vereceği anlamına gelir. SMB 3.0'ı desteklemeyen istemciler veya SMB 3.0'ı destekleyen ancak Kobİ şifrelemesini desteklemeyen istemciler, aktarımda şifreleme etkinse Azure dosya paylaşımını monte edemez. Hangi işletim sistemlerinin SMB 3.0'ı şifrelemeyle desteklediği hakkında daha fazla bilgi için [Windows,](storage-how-to-use-files-windows.md) [macOS](storage-how-to-use-files-mac.md)ve [Linux](storage-how-to-use-files-linux.md)için ayrıntılı belgelerimize bakın. PowerShell, CLI ve SDK'ların tüm geçerli sürümleri HTTPS'yi destekler.  

Bir Azure depolama hesabı için aktarımda şifrelemeyi devre dışı kullanabilirsiniz. Şifreleme devre dışı bırakıldığında, Azure Dosyaları Ayrıca SMB 2.1, şifreleme olmadan SMB 3.0 ve HTTP üzerinden şifrelenmemiş FileREST API aramaları da izin verir. Aktarımsırasında şifrelemeyi devre dışı bilmelidir, windows server 2008 R2 veya daha eski Linux dağıtımı gibi eski bir işletim sisteminde çalıştırılması gereken eski bir uygulamayı desteklemektir. Azure Dosyaları yalnızca Azure dosya paylaşımıyla aynı Azure bölgesinde sMB 2.1 bağlantılarına izin verir; Azure dosya paylaşımının Azure bölgesi nin dışında bulunan (şirket içi veya farklı bir Azure bölgesi gibi) bir SMB 2.1 istemcisi dosya paylaşımına erişemez.

Aktarım sırasında şifreleme hakkında daha fazla bilgi için Azure [depolama alanında güvenli aktarım gerektirmeye](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyalar'a genel bakış](storage-files-introduction.md)
- [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)