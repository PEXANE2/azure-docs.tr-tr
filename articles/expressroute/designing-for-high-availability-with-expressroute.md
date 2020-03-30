---
title: 'Azure ExpressRoute: Yüksek kullanılabilirlik için tasarım'
description: Bu sayfa, Azure ExpressRoute kullanırken yüksek kullanılabilirlik için mimari öneriler sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 06/28/2019
ms.author: rambala
ms.openlocfilehash: 4c3c6ae5fbdd91e6e44438be7fef2a3a91564a34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74076674"
---
# <a name="designing-for-high-availability-with-expressroute"></a>ExpressRoute ile yüksek kullanılabilirlik için tasarım

ExpressRoute, Microsoft kaynaklarına operatör sınıfı özel ağ bağlantısı sağlamak için yüksek kullanılabilirlik için tasarlanmıştır. Başka bir deyişle, Microsoft ağındaki ExpressRoute yolunda tek bir hata noktası yoktur. Kullanılabilirliği en üst düzeye çıkarmak için, ExpressRoute devrenizin müşteri ve servis sağlayıcı segmenti de yüksek kullanılabilirlik için tasarlanmalıdır. Bu makalede, önce ExpressRoute kullanarak sağlam ağ bağlantısı oluşturmak için ağ mimarisinin gözlerini inceleyelim, sonra ExpressRoute devrenizin yüksek kullanılabilirliğini geliştirmenize yardımcı olan ince ayar özelliklerine bakalım.


## <a name="architecture-considerations"></a>Mimari hususlar

Aşağıdaki şekil, bir ExpressRoute devresinin kullanılabilirliğini en üst düzeye çıkarmak için bir ExpressRoute devresi kullanarak bağlanmanın önerilen yolunu göstermektedir.

 [![1.1.2.2.]][1]

Yüksek kullanılabilirlik için, ExpressRoute devresinin uçtan uca ağ boyunca fazlalığını korumak önemlidir. Başka bir deyişle, şirket içi ağınızda fazlalık korumanız gerekir ve hizmet sağlayıcı ağınızdaki fazlalığınızdan ödün vermemelidir. Artıklığı en az düzeyde korumak, tek bir ağ hatası noktasından kaçınmak anlamına gelir. Ağ aygıtları için gereksiz güç ve soğutmaya sahip olmak yüksek kullanılabilirliği daha da artıracaktır.

### <a name="first-mile-physical-layer-design-considerations"></a>Birinci mil fiziksel katman tasarım konuları

 Aynı Müşteri Dayanak Donanımı 'ndaki (CPE) bir ExpressRoute devresinin hem birincil hem de ikincil bağlantılarını sonlandırırsanız, şirket içi ağınızdaki yüksek kullanılabilirliği tehlikeye atmış olursunuz. Ayrıca, hem birincil hem de ikincil bağlantıları bir CPE'nin aynı bağlantı noktası üzerinden yapılandırıyorsanız (iki bağlantıyı farklı alt arabirimler altında sonlandırarak veya iş ortağı ağı içindeki iki bağlantıyı birleştirerek), iş ortağını zorlamış oluyorsunuz ağ segmentinde de yüksek kullanılabilirlik tehlikeye atmak için. Bu uzlaşma aşağıdaki şekilde gösterilmiştir.

[![2 2 2.000]][2]

Diğer taraftan, farklı coğrafi konumlardaki bir ExpressRoute devresinin birincil ve ikincil bağlantılarını sonlandırırsanız, bağlantının ağ performansından ödün verebilirsiniz. Trafik, farklı coğrafi konumlarda sonlandırılan birincil ve ikincil bağlantılar arasında etkin bir şekilde yüklenirse, iki yol arasındaki ağ gecikmesi arasındaki olası önemli fark, optimal olmayan ağ Performans. 

Coğrafi gereksiz tasarım konuları için [ExpressRoute ile olağanüstü durum kurtarma için tasarım][DR]konusuna bakın.

### <a name="active-active-connections"></a>Etkin-etkin bağlantılar

Microsoft ağı, ExpressRoute devrelerinin birincil ve ikincil bağlantılarını etkin-etkin modda çalışacak şekilde yapılandırılmıştır. Ancak, rota reklamlarınız aracılığıyla, bir ExpressRoute devresinin gereksiz bağlantılarını etkin-pasif modda çalışmaya zorlayabilirsiniz. Reklam daha özel yolları ve BGP AS yol prepending bir yol diğerine tercih yapmak için kullanılan ortak tekniklerdir.

Yüksek kullanılabilirliği artırmak için, bir ExpressRoute devresinin her iki bağlantısını da etkin-etkin modda çalıştırmak önerilir. Bağlantıların etkin modda çalışmasına izin ederseniz, Microsoft ağı akışları başına olarak bağlantılar arasındaki trafiği yükler.

Bir ExpressRoute devresinin birincil ve ikincil bağlantılarını etkin-pasif modda çalıştırmak, her iki bağlantının da etkin yolda bir hata sonrasında arızalanması riskiile karşı karşıyadır. Geçiş başarısızlığının yaygın nedenleri pasif bağlantının etkin yönetimi olmaması ve pasif bağlantı reklam bayat yollarıdır.

Alternatif olarak, bir ExpressRoute devresinin birincil ve ikincil bağlantılarını etkin-etkin modda çalıştırmak, ExpressRoute bağlantı hatası sonrasında akışların yalnızca yarısının arızalanmasına ve yeniden yönlendirilmemesine neden olur. Böylece, etkin-etkin mod önemli ölçüde Ortalama Zaman Kurtarma (MTTR) geliştirmeye yardımcı olacaktır.

### <a name="nat-for-microsoft-peering"></a>Microsoft bakış için NAT 

Microsoft bakış, ortak uç noktalar arasındaki iletişim için tasarlanmıştır. Bu nedenle, şirket içi özel uç noktalar, Microsoft'un bakışları üzerinden iletişim kurmadan önce müşteri veya iş ortağı ağında genel IP ile birlikte Ağ Adresi Çevrilmiştir (NATed) dir. Hem birincil hem de ikincil bağlantıları etkin etkin modda kullandığınızı varsayarsak, NAT'nin ExpressRoute bağlantılarından birinde bir hatadan sonra ne kadar hızlı toparlandığınızı ve nasıl kurtardığınızı nasıl etkilediğiniz. İki farklı NAT seçeneği aşağıdaki şekilde gösterilmiştir:

[![3]][3]

Seçenek 1'de, NAT, trafiği ExpressRoute'un birincil ve ikincil bağlantıları arasında böldükten sonra uygulanır. NAT'nin devlet gereksinimlerini karşılamak için, birincil ve ikincil aygıtlar arasında bağımsız NAT havuzları kullanılır, böylece dönüş trafiği akış tan hangi şekilde aktı aynı kenar aygıtına ulaşır.

Seçenek 2'de, trafiği ExpressRoute'un birincil ve ikincil bağlantıları arasında bölmeden önce ortak bir NAT havuzu kullanılır. Trafiği bölmeden önce ortak NAT havuzunun tek noktalı hata lar getirerek yüksek kullanılabilirlikten ödün vermek anlamına gelmediğini belirtmek önemlidir.

1 seçeneği yle, ExpressRoute bağlantı arızası sonrasında, ilgili NAT havuzuna ulaşma yeteneği kırılır. Bu nedenle, tüm kırık akışları tcp veya ilgili pencere zaman ayarı aşağıdaki uygulama katmanı tarafından yeniden kurulması gerekir. NAT havuzlarından herhangi biri şirket içi sunuculardan herhangi birini ön planda kullanmak için kullanılırsa ve ilgili bağlantı başarısız olursa, bağlantı düzeltilene kadar şirket içi sunuculara Azure'dan erişilemez.

2 seçeneği ile ise, NAT bir birincil veya ikincil bağlantı hatası sonra bile ulaşılabilir. Bu nedenle, ağ katmanının kendisi paketleri yeniden yönlendirebilir ve hatadan sonra daha hızlı kurtarma yardımcı olabilir. 

> [!NOTE]
> NAT seçeneğini 1 'i (birincil ve ikincil ExpressRoute bağlantıları için bağımsız NAT havuzları) kullanır ve NAT havuzundan bir IP adresinin bağlantı noktasını şirket içi sunucuya eşlerseniz, ilgili sunucu bağlantı başarısız olur.
> 

## <a name="fine-tuning-features-for-private-peering"></a>Özel eşleme için ince ayar özellikleri

Bu bölümde, ExpressRoute devrenizin yüksek kullanılabilirliğini artırmaya yardımcı olan isteğe bağlı (Azure dağıtımınıza ve MTTR'ye ne kadar hassas olduğunuza bağlı olarak) özellikleri gözden geçirelim. Özellikle, ExpressRoute sanal ağ ağ ağ geçitlerinin bölge tarafından bilinçli dağıtımını ve Çift Yönlü Yönlendirme Algılamasını (BFD) gözden geçirelim.

### <a name="availability-zone-aware-expressroute-virtual-network-gateways"></a>Kullanılabilirlik Bölgesi farkında ExpressRoute sanal ağ ağ geçitleri

Azure bölgesindeki Kullanılabilirlik Bölgesi, hata etki alanı ve güncelleştirme etki alanının birleşimidir. Bölge yedekli Azure IaaS dağıtımını tercih ederseniz, ExpressRoute özel eşlemesine son veren bölge yedekli sanal ağ ağ ağ geçitlerini de yapılandırmak isteyebilirsiniz. Daha fazla bilgi edinmek için Azure [Kullanılabilirlik Bölgelerinde ki bölge artıkları][zone redundant vgw]gereksiz sanal ağ ağ geçitleri hakkında bilgi edinin. Bölge yedekli sanal ağ ağ ağ geçidini yapılandırmak için bkz. [Azure Kullanılabilirlik Bölgelerinde bölge yedekli sanal ağ ağ geçidi oluştur'][conf zone redundant vgw]a bakın.

### <a name="improving-failure-detection-time"></a>Hata algılama süresini artırma

ExpressRoute, özel bakışlar üzerinden BFD'yi destekler. BFD, Microsoft Enterprise Edge (MSEEs) ve şirket içi taraftaki BGP komşuları arasındaki Katman 2 ağı üzerindeki hata algılama süresini yaklaşık 3 dakikadan (varsayılan olarak) saniyenin daha kısasına indirir. Hızlı arıza algılama süresi, hata kurtarmayı hızlandırır. Daha fazla bilgi için Bkz. [ExpressRoute üzerinden BFD Yapılandır.][BFD]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ExpressRoute devre bağlantısının yüksek kullanılabilirliği için nasıl tasarlanabileceğimizi tartıştık. Bir ExpressRoute devresi hedefleme noktası coğrafi bir konuma sabitlenir ve bu nedenle tüm konumu etkileyen yıkıcı bir arızadan etkilenebilir. 

Tüm bölgeyi etkileyen felaket hatalarına dayanabilecek Microsoft omurgasına coğrafi gereksiz ağ bağlantısı oluşturmak için tasarım konuları için [ExpressRoute özel bakışları ile olağanüstü durum kurtarma tasarımı][DR]konusuna bakın.

<!--Image References-->
[1]: ./media/designing-for-high-availability-with-expressroute/exr-reco.png "ExpressRoute kullanarak bağlanmanın önerilen yolu"
[2]: ./media/designing-for-high-availability-with-expressroute/suboptimal-lastmile-connectivity.png "Suboptimal son mil bağlantısı"
[3]: ./media/designing-for-high-availability-with-expressroute/nat-options.png "NAT seçeneği"


<!--Link References-->
[zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/about-zone-redundant-vnet-gateways
[conf zone redundant vgw]: https://docs.microsoft.com/azure/vpn-gateway/create-zone-redundant-vnet-gateway
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[DR]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering




