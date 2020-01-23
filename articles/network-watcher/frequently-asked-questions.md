---
title: Azure ağ Izleyicisi hakkında sık sorulan sorular (SSS) | Microsoft Docs
description: Bu makalede, Azure ağ Izleyicisi hizmeti hakkında sık sorulan sorular yanıtlanmaktadır.
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
ms.openlocfilehash: 856c249b72e9e0ff8667d10821ad14b3432b0775
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76509198"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Azure ağ Izleyicisi hakkında sık sorulan sorular (SSS)
[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) hizmeti, bir Azure sanal ağındaki kaynaklara yönelik günlükleri izlemeye, tanılamaya, görüntülemeye ve etkinleştirmeye ve devre dışı bırakacak bir araç paketi sağlar. Bu makalede hizmetle ilgili yaygın soruların yanıtları vardır.

## <a name="general"></a>Genel

### <a name="what-is-network-watcher"></a>Ağ İzleyicisi nedir?
Ağ Izleyicisi, sanal makineler, sanal ağlar, uygulama ağ geçitleri, yük dengeleyiciler ve bir Azure sanal ağındaki diğer kaynakları içeren IaaS (hizmet olarak altyapı) bileşenlerinin ağ durumunu izlemek ve onarmak üzere tasarlanmıştır. PaaS (hizmet olarak platform) altyapısını izlemeye veya Web/Mobil Analiz almaya yönelik bir çözüm değildir.

### <a name="what-tools-does-network-watcher-provide"></a>Ağ Izleyicisi hangi araçları sağlar?
Ağ Izleyicisi üç önemli özellik kümesi sağlar
* İzleme
  * [Topoloji görünümü](https://docs.microsoft.com/azure/network-watcher/view-network-topology) , sanal ağınızdaki kaynakları ve bunlar arasındaki ilişkileri gösterir.
  * [Bağlantı İzleyicisi](https://docs.microsoft.com/azure/network-watcher/connection-monitor) , bir VM ile başka bir ağ kaynağı arasındaki bağlantıyı ve gecikme süresini izlemenizi sağlar.
  * [Ağ performansı İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) , karma ağ mimarilerinde, ExpressRoute devrelerine ve hizmet/uygulama uç noktalarında bağlantıyı ve gecikme sürelerini izlemenize olanak sağlar.  
* Tanılama
  * [IP akışı doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) , bir VM düzeyinde trafik filtreleme sorunlarını algılamanıza olanak tanır.
  * [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) , trafik yollarını doğrulamanıza ve yönlendirme sorunlarını tespit etmenize yardımcı olur.
  * [Bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) , bir VM ile başka bir ağ kaynağı arasında tek seferlik bir bağlantı ve gecikme denetimi sunar.
  * [Paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) , sanal AĞıNıZDAKI bir VM 'deki tüm trafiği yakalamanızı sağlar.
  * [VPN sorun giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) , VPN ağ geçitleriniz ve bağlantılarında hata ayıklamanıza yardımcı olan birden çok tanılama denetimi çalıştırır.
* Günlüğe kaydetme
  * [NSG akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) , [ağ güvenlik gruplarındaki (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) tüm trafiği günlüğe kaydetmenize olanak tanır
  * [Trafik Analizi](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) NSG akış günlüğü verilerinizi işleyerek ağ trafiğinizi görselleştirmenizi, sorgulamanızı, çözümlemenize ve anlamanıza olanak tanır.


Daha ayrıntılı bilgi için bkz. [Ağ İzleyicisi 'ne genel bakış sayfası](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).


### <a name="how-does-network-watcher-pricing-work"></a>Ağ Izleyicisi fiyatlandırması nasıl çalışır?
Ağ Izleyicisi bileşenleri için [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/network-watcher/) ve bunların fiyatlandırmasını ziyaret edin.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>Ağ Izleyicisi hangi bölgelerde desteklenir/kullanılabilir?
[Azure hizmet kullanılabilirliği sayfasında](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher) en son bölgesel kullanılabilirliği görüntüleyebilirsiniz

### <a name="what-are-resource-limits-on-network-watcher"></a>Ağ izleyicisinden kaynak sınırları nelerdir?
Tüm sınırlar için [hizmet limitleri](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#network-watcher-limits) sayfasına bakın.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Bölge başına yalnızca bir ağ Izleyicisi örneğine izin veriliyor mu?
Bu özelliğin çalışması için bir abonelik için ağ Izleyicisi 'nin etkinleştirilmesi yeterlidir, bu bir hizmet sınırı değildir.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>Neden ağ Izleyicisi uzantısını yüklemem gerekir? 
Ağ Izleyicisi uzantısı, bir VM 'den trafik oluşturması veya bu trafiği ele almak için gereken tüm özellikler için gereklidir. 

### <a name="which-features-require-the-network-watcher-extension"></a>Hangi özellikler ağ Izleyicisi uzantısını gerektiriyor?
Yalnızca paket yakalama, bağlantı sorunlarını giderme ve bağlantı Izleyicisi için ağ Izleyicisi uzantısının mevcut olması gerekir.

## <a name="nsg-flow-logs"></a>NSG akış günlükleri

### <a name="what-does-nsg-flow-logs-do"></a>NSG akış günlükleri ne yapar?
Azure ağ kaynakları, [ağ güvenlik grupları (NSG 'ler)](https://docs.microsoft.com/azure/virtual-network/security-overview)ile birleştirilebilir ve yönetilebilir. NSG akış günlükleri, NSG 'larınız aracılığıyla tüm trafikle ilgili 5 demet akış bilgilerini günlüğe Kaydetetkinleştirmenizi sağlar. Ham akış günlükleri, gerektikçe daha fazla işlenebileceği, çözümlenebildiği, sorgulanan veya verilebilecekleri bir Azure depolama hesabına yazılır.

### <a name="how-do-i-use-nsg-flow-logs-on-a-storage-account-with-a-firewall"></a>Nasıl yaparım? NSG akış günlüklerini güvenlik duvarıyla bir depolama hesabında kullanmak mı istiyorsunuz?

Bir güvenlik duvarıyla depolama hesabı kullanmak için, güvenilen Microsoft hizmetlerinin depolama hesabınıza erişmesi için bir özel durum sağlamanız gerekir:

* Portala veya [depolama hesapları sayfasından](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) depolama hesabının adını yazarak depolama hesabına gidin.
* **AYARLAR** bölümünün altında **Güvenlik duvarları ve sanal ağlar**'ı seçin
* "Erişime Izin ver" bölümünde **Seçili ağlar**' ı seçin. Ardından, **özel durumlar**altında **"Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver"** seçeneğinin yanındaki kutuyu işaret edin. 
* Zaten seçiliyse, hiçbir değişiklik yapmanız gerekmez.  
* [NSG akış günlüklerine Genel Bakış sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) hedef NSG 'nizi bulun ve NSG akış günlüklerini yukarıdaki depolama hesabı seçiliyken etkinleştirin.

Birkaç dakika sonra depolama günlüklerini denetleyebilirsiniz; güncelleştirilmiş bir TimeStamp veya yeni oluşturulmuş bir JSON dosyası görmelisiniz.

### <a name="how-do-i-use-nsg-flow-logs-with-service-endpoints-for-storage"></a>Depolama için hizmet uç noktaları ile NSG akış günlüklerini Nasıl yaparım? mi kullanıyorsunuz?

Lütfen [hizmet uç noktalarını etkinleştirme öğreticisine](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources#enable-a-service-endpoint)bakın. 


### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Akış günlükleri sürümleri 1 & 2 arasındaki fark nedir?
Akış günlükleri sürüm 2, *akış durumu* kavramını tanıtır & aktarılan bayt ve paketler hakkında bilgi depolar. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Sonraki Adımlar
 - Ağ izleyicisine başlamanıza yönelik bazı öğreticiler için [belgelerimize genel bakış sayfasına](https://docs.microsoft.com/azure/network-watcher/) gidin.
