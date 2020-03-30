---
title: Azure Ağ İzleyicisi hakkında sık sorulan sorular (SSS) | Microsoft Dokümanlar
description: Bu makalede, Azure Ağ İzleyicisi hizmeti hakkında sık sorulan sorular yanıtlanmaktadır.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: b48aab918b477f5c689a50ca476b0b1336642f0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471865"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Azure Ağ İzleyicisi hakkında sık sorulan sorular (SSS)
[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) hizmeti, bir Azure sanal ağındaki kaynakların günlüklerini izlemek, tanılamak, görüntülemek ve etkinleştirmek veya devre dışı etmek için bir araç paketi sağlar. Bu makalede, hizmet le ilgili sık sorulan soruları yanıtlar.

## <a name="general"></a>Genel

### <a name="what-is-network-watcher"></a>Ağ İzleyicisi nedir?
Ağ İzleyicisi, Bir Azure sanal ağındasanal makineler, Sanal Ağlar, Uygulama Ağ Geçitleri, Yük dengeleyicileri ve diğer kaynakları içeren IaaS (Hizmet Olarak Altyapı) bileşenlerinin ağ durumunu izlemek ve onarmak için tasarlanmıştır. PaaS (Hizmet Olarak Platform) altyapılarını izlemek veya web/mobil analitiği almak için bir çözüm değildir.

### <a name="what-tools-does-network-watcher-provide"></a>Ağ İzleyicisi hangi araçları sağlar?
Ağ İzleyicisi üç ana yetenek kümesi sağlar
* İzleme
  * [Topoloji görünümü,](https://docs.microsoft.com/azure/network-watcher/view-network-topology) sanal ağınızdaki kaynakları ve bunlar arasındaki ilişkileri gösterir.
  * [Bağlantı İzleyicisi,](https://docs.microsoft.com/azure/network-watcher/connection-monitor) bir VM ile başka bir ağ kaynağı arasındaki bağlantıyı ve gecikmeyi izlemenize olanak tanır.
  * [Ağ performans monitörü,](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) hibrit ağ mimarileri, Expressroute devreleri ve hizmet/uygulama bitiş noktaları arasında bağlantı ve gecikme süreleri izlemenize olanak tanır.  
* Tanılama
  * [IP Akış Doğrulama,](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) trafik filtreleme sorunlarını VM düzeyinde algılamanızı sağlar.
  * [Next Hop,](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) trafik rotalarını doğrulamanıza ve yönlendirme sorunlarını algılamanıza yardımcı olur.
  * [Bağlantı Sorunu Giderme,](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) VM ile başka bir ağ kaynağı arasında bir kerelik bağlantı ve gecikme süresi denetimi sağlar.
  * [Packet Capture,](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) sanal ağınızdaki tüm VM trafiğini yakalamanızı sağlar.
  * [VPN Sorun Giderme,](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) hata ayıklama sorunlarına yardımcı olmak için VPN ağ geçitlerinizde ve bağlantılarınızda birden çok tanılama denetimi çalıştırın.
* Günlüğe Kaydetme
  * [NSG Akış Günlükleri,](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) Ağ Güvenlik [Gruplarınızdaki (NSG' ler)](https://docs.microsoft.com/azure/virtual-network/security-overview) tüm trafiği kaydetmenizi sağlar
  * [Trafik Analitiği,](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) NSG Akış Günlüğü verilerinizi işleyerek ağ trafiğinizi görselleştirmenizi, sorgulamanızı, analiz etmenizi ve anlamanızı sağlar.


Daha ayrıntılı bilgi için [Ağ İzleyicisi genel bakış sayfasına](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)bakın.


### <a name="how-does-network-watcher-pricing-work"></a>Network Watcher fiyatlandırması nasıl çalışır?
Ağ İzleyicisi bileşenleri ve bunların fiyatlandırması için [Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/network-watcher/) ziyaret edin.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>Ağ İzleyicisi hangi bölgelerde desteklenir/kullanılabilir?
[Azure Hizmeti kullanılabilirlik sayfasında](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher) en son bölgesel kullanılabilirliği görüntüleyebilirsiniz

### <a name="which-permissions-are-needed-to-use-network-watcher"></a>Ağ İzleyicisi kullanmak için hangi izinler gereklidir?
[Ağ İzleyicisi'ni kullanmak için gereken RBAC izinlerinin listesine](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions)bakın. Kaynakları dağıtmak için NetworkWatcherRG'ye katkıda bulunan izinlere ihtiyacınız vardır (aşağıya bakın).

### <a name="how-do-i-enable-network-watcher"></a>Ağ İzleyicisi'ni nasıl etkinleştirebilirim?
Ağ İzleyicisi hizmeti her abonelik için [otomatik olarak etkinleştirilir.](https://azure.microsoft.com/updates/azure-network-watcher-will-be-enabled-by-default-for-subscriptions-containing-virtual-networks/)

### <a name="what-is-the-network-watcher-deployment-model"></a>Ağ İzleyicisi dağıtım modeli nedir?
Ağ İzleyicisi üst kaynağı, her bölgede benzersiz bir örnekle dağıtılır. Adlandırma biçimi: NetworkWatcher_RegionName. Örnek: NetworkWatcher_centralus "Orta ABD" bölgesi için Ağ İzleyicisi kaynağıdır.

### <a name="what-is-the-networkwatcherrg"></a>NetworkWatcherRG nedir?
Ağ İzleyicisi kaynakları, otomatik olarak oluşturulan gizli **NetworkWatcherRG** kaynak grubunda yer alır. Örneğin, NSG Akış Günlükleri kaynağı Ağ İzleyicisi'nin alt kaynağıdır ve NetworkWatcherRG'de etkindir.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>Ağ İzleyicisi uzantısını neden yüklemem gerekiyor? 
Ağ İzleyicisi uzantısı, bir VM'den trafik oluşturması veya engellemesi gereken herhangi bir özellik için gereklidir. 

### <a name="which-features-require-the-network-watcher-extension"></a>Hangi özellikler Ağ İzleyicisi uzantısı gerektirir?
Paket Yakalama, Bağlantı Sorun Giderme ve Bağlantı İzleyicisi özelliklerinin bulunması için Ağ İzleyicisi uzantısı gerekir.

### <a name="what-are-resource-limits-on-network-watcher"></a>Ağ İzleyicisi'ndeki kaynak sınırları nelerdir?
Tüm sınırlar için [Hizmet sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#network-watcher-limits) sayfasına bakın.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Neden bölge başına Ağ İzleyicisi'nin yalnızca bir örneğine izin veriliyor? 
Ağ İzleyicisi sadece bir kez bu özellikleri çalışması için bir abonelik için etkin olması gerekir, bu bir hizmet sınırı değildir.

### <a name="how-can-i-manage-the-network-watcher-resource"></a>Ağ İzleyicisi Kaynağını nasıl yönetebilirim? 
Ağ İzleyicisi kaynağı, Ağ İzleyicisi için arka uç hizmetini temsil eder ve azure tarafından tamamen yönetilir. Müşterilerin yönetmesine gerek yoktur. Taşıma gibi işlemler kaynakta desteklenmez. Ancak, [kaynak silinebilir.](https://docs.microsoft.com/azure/network-watcher/network-watcher-create#delete-a-network-watcher-in-the-portal) 

## <a name="nsg-flow-logs"></a>NSG Akış Günlükleri

### <a name="what-does-nsg-flow-logs-do"></a>NSG Akış Günlükleri ne yapar?
Azure ağ kaynakları [Ağ Güvenlik Grupları (NSG' ler)](https://docs.microsoft.com/azure/virtual-network/security-overview)aracılığıyla birleştirilebilir ve yönetilebilir. NSG Akış Günlükleri, NSG'leriniz aracılığıyla tüm trafikle ilgili 5 tuple akış bilgilerini kaydetmenizi sağlar. Ham akış günlükleri, gerektiğinde daha fazla işlenebilecekleri, analiz edilebildikleri, sorgulanabilecekleri veya dışa aktarılabildiği bir Azure Depolama hesabına yazılır.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-firewall"></a>Güvenlik duvarının arkasındaki Depolama hesabıyla NSG Akış Günlüklerini nasıl kullanırım?

Güvenlik duvarının arkasında bir Depolama hesabı kullanmak için, Güvenilen Microsoft Hizmetleri'nin depolama hesabınıza erişebilmesi için bir özel durum sağlamanız gerekir:

* Portaldaki genel aramada veya [Depolama Hesapları sayfasından](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) depolama hesabının adını yazarak depolama hesabına gidin
* **AYARLAR** bölümünün altında **Güvenlik duvarları ve sanal ağlar**'ı seçin
* "Erişime izin ver"de **Seçili ağları**seçin. Ardından **Özel Durumlar**altında , **"Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver"** kutusunu işaretleyin 
* Zaten seçiliyse, hiçbir değişiklik yapmanız gerekmez.  
* Hedef NSG'nizi [NSG Akış Günlükleri genel bakış sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) bulun ve yukarıdaki depolama hesabı seçilerek NSG Akış Günlüklerini etkinleştirin.

Birkaç dakika sonra depolama günlüklerini denetleyebilirsiniz; güncelleştirilmiş bir TimeStamp veya yeni oluşturulmuş bir JSON dosyası görmelisiniz.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-service-endpoint"></a>Hizmet Bitiş Noktası'nın arkasındaki Depolama hesabıyla NSG Akış Günlüklerini nasıl kullanırım?

NSG Akış Günlükleri, herhangi bir ekstra yapılandırma gerektirmeden Servis Bitiş Noktaları ile uyumludur. Lütfen sanal ağınızdaki [Hizmet Bitiş Noktalarını etkinleştirme öğreticisine](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources#enable-a-service-endpoint) bakın.


### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Akış günlükleri sürümleri 1 & 2 arasındaki fark nedir?
Akış Günlükleri sürüm *2, aktarılan* baytlar ve paketler hakkında bilgi depolayarak Akış Durumu kavramını & tanıtıyor. [Devamını oku](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Sonraki Adımlar
 - Network Watcher ile işe başlamanız için bazı eğitimler için [belgelere genel bakış sayfamıza](https://docs.microsoft.com/azure/network-watcher/) gidin.
