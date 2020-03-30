---
title: Azure Ağ İzleyicisi ile paket denetimi | Microsoft Dokümanlar
description: Bu makalede, Bir VM toplanan derin paket denetimi gerçekleştirmek için Ağ İzleyicisi nasıl kullanılacağı açıklanmaktadır
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 7d32043ca73e9cf810b3eab5e65cb4b42b599d18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152933"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Azure Ağ İzleyicisi ile paket denetimi

Network Watcher'ın paket yakalama özelliğini kullanarak, Azure VM'lerinizdeki yakalama oturumlarını PowerShell, CLI portalından ve Programlı olarak SDK ve REST API'den başlatabilir ve yönetebilirsiniz. Paket yakalama, bilgileri kolayca kullanılabilir bir biçimde sağlayarak paket düzeyi verileri gerektiren senaryoları ele almanızı sağlar. Verileri incelemek için serbestçe kullanılabilen araçlardan yararlanarak, Sanal Bilgisayar'larınıza gönderilen ve bu ülkelerden gönderilen iletişimleri inceleyebilir ve ağ trafiğiniz hakkında bilgi edinebilirsiniz. Paket yakalama verilerinin bazı örnek kullanımları şunlardır: ağ veya uygulama sorunlarını araştırmak, ağ kötüye kullanımını ve izinsiz giriş girişimlerini algılamak veya mevzuata uygunluğu korumak. Bu makalede, popüler bir açık kaynak aracı kullanarak Network Watcher tarafından sağlanan bir paket yakalama dosyasının nasıl açılacağını gösteriyoruz. Ayrıca, bağlantı gecikmesi nasıl hesaplanır, anormal trafiği tanımlamak ve ağ istatistiklerini incelemek için nasıl örnekler sağlarız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, daha önce çalıştırılan bir paket yakalama bazı önceden yapılandırılmış senaryolar geçer. Bu senaryolar, paket yakalama gözden geçirerek erişilebilen yetenekleri gösterir. Bu senaryo, paket yakalama incelemek için [WireShark](https://www.wireshark.org/) kullanır.

Bu senaryo, sanal bir makinede zaten bir paket yakalama çalıştırdığınızı varsayar. Paket yakalama ziyaretinin nasıl oluşturulabildiğini öğrenmek için PAKET [Yakalamaları'nı](network-watcher-packet-capture-manage-rest.md)REST API ile Yönetme'yi ziyaret ederek portalla veya REST [ile paket yakalamaları yönetin.](network-watcher-packet-capture-manage-portal.md)

## <a name="scenario"></a>Senaryo

Bu senaryoda, şunları

* Paket yakalamayı gözden geçirme

## <a name="calculate-network-latency"></a>Ağ gecikmesi hesaplama

Bu senaryoda, iki uç nokta arasında gerçekleşen Iletim Denetim Protokolü (TCP) konuşmasının ilk Gidiş-Dönüş Saatini (RTT) nasıl görüntüleyebileceğimizi gösteririz.

Bir TCP bağlantısı kurulduğunda, bağlantıda gönderilen ilk üç paket, genellikle üç yönlü el sıkışma olarak adlandırılan bir deseni izler. Bu el sıkışma, istemciden gelen ilk istek ve sunucudan gelen bir yanıt la gönderilen ilk iki paketi inceleyerek, bu bağlantı kurulduğunda gecikme süresini hesaplayabiliriz. Bu gecikme süresine Gidiş-Dönüş Saati (RTT) adı verilir. TCP protokolü ve üç yönlü el sıkışma hakkında daha fazla bilgi için aşağıdaki kaynağa bakın. [https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip](https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip)

### <a name="step-1"></a>1. Adım

WireShark başlat

### <a name="step-2"></a>2. Adım

Paket yakalamanızdan **.cap** dosyasını yükleyin. Bu dosya, nasıl yapılandırdığınıza bağlı olarak, yerel olarak sanal makineye kaydedilen blob bulunabilir.

### <a name="step-3"></a>3. Adım

TCP konuşmalarındaki ilk Gidiş Dönüş Saatini (RTT) görüntülemek için, yalnızca TCP el sıkışmasındaki ilk iki pakete bakıyor olacağız. İlk iki paketi üç yönlü el sıkışmada kullanacağız, bunlar [SYN], [SYN, ACK] paketleridir. Bunlar, TCP üstbilgisinde ayarlanan bayraklar için adlandırılır. El sıkışmadaki son paket olan [ACK] paketi bu senaryoda kullanılmaz. [SYN] paketi istemci tarafından gönderilir. Sunucu alındıktan sonra [ACK] paketini istemciden SYN alma bildirimi olarak gönderir. Sunucunun yanıtının çok az ek yükü gerektirdiğinden, [SYN, ACK] paketinin istemci tarafından [SYN] paketinin istemci tarafından gönderildiği zamana kadar alındığı zamanı çıkararak RTT'yi hesaplıyoruz.

WireShark kullanarak bu değer bizim için hesaplanır.

TCP üç yönlü el sıkışma ilk iki paket daha kolay görüntülemek için, WireShark tarafından sağlanan filtreleme yeteneği ni kullanır.

Filtreyi WireShark'da uygulamak için, yakalamada [SYN] paketinin "İletim Kontrol Protokolü" Segmentini genişletin ve TCP üstbilgisinde ayarlanan bayrakları inceleyin.

Tüm [SYN] ve [SYN, ACK] paketlerini filtrelemek istediğimizden, bayraklar altında Syn bitinin 1 olarak ayarlı olduğunu onaylıyoruz, ardından Syn biti -> Filtre olarak uygula -> Seçili olarak sağ tıklayın.

![şekil 7][7]

### <a name="step-4"></a>4. Adım

Artık pencereyi yalnızca [SYN] bit setine sahip paketleri görmek için filtrelediğinize göre, ilk RTT'yi görüntülemek için ilgilendiğiniz konuşmaları kolayca seçebilirsiniz. WireShark RTT görüntülemek için basit bir yol sadece "SEQ / ACK" analizi işaretli açılır tıklayın. Daha sonra RTT görüntülenen göreceksiniz. Bu durumda RTT 0.00221114 saniye veya 2.211 ms idi.

![şekil 8][8]

## <a name="unwanted-protocols"></a>İstenmeyen protokoller

Azure'da dağıttığınız sanal makine örneğinde çalışan birçok uygulamanız olabilir. Bu uygulamaların çoğu, belki de açık izniniz olmadan ağ üzerinden iletişim kurar. Ağ iletişimini depolamak için paket yakalamayı kullanarak, uygulamanın ağda nasıl konuştuğunu araştırabilir ve herhangi bir sorun alabiliyoruz.

Bu örnekte, makinenizde çalışan bir uygulamanın yetkisiz iletişimini gösterebilecek istenmeyen protokoller için önceki bir çalıştırılmış paket yakalamayı gözden geçiriyoruz.

### <a name="step-1"></a>1. Adım

Önceki senaryoda aynı yakalamayı kullanarak **İstatistik** > **Protokolü Hiyerarşisi'ni** tıklatın

![protokol hiyerarşisi menüsü][2]

Protokol hiyerarşisi penceresi görüntülenir. Bu görünüm, yakalama oturumu sırasında kullanılan tüm protokollerin ve protokoller kullanılarak iletilen ve alınan paket sayısının bir listesini sağlar. Bu görünüm, sanal makinelerinizde veya ağınızda istenmeyen ağ trafiğini bulmak için yararlı olabilir.

![protokol hiyerarşisi açıldı][3]

Aşağıdaki ekran yakalama gördüğünüz gibi, eşler arası dosya paylaşımı için kullanılan BitTorrent protokolü kullanarak trafik vardı. Bir yönetici olarak bu özel sanal makinelerde BitTorrent trafik görmeyi beklemiyoruz. Artık bu trafiğin farkındasınız, bu sanal makineye yüklenen eşler arası yazılımı kaldırabilir veya Ağ Güvenlik Grupları veya Güvenlik Duvarı kullanarak trafiği engelleyebilirsiniz. Ayrıca, sanal makinelerinizdeki protokol kullanımını düzenli olarak gözden geçirebilmeniz için paket yakalamaları bir zamanlamaya göre çalıştırmayı seçebilirsiniz. Azure'da ağ görevlerini otomatikleştirme hakkında bir örnek için [azure otomasyonu ile ağ kaynaklarını izleyin](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>En iyi hedefleri ve bağlantı noktalarını bulma

Ağınızdaki uygulamaları ve kaynakları izlerken veya sorun giderirken trafik türlerini, uç noktaları ve iletişim bağlantı noktalarının anlaşılması önemlidir. Yukarıdan bir paket yakalama dosyası kullanarak, vm iletişim ve kullanılan limanları ile en iyi hedefleri hızlı bir şekilde öğrenebilirsiniz.

### <a name="step-1"></a>1. Adım

Önceki senaryoda aynı yakalamayı kullanarak **İstatistiki** > **IPv4 İstatistik** > **Hedefleri ve Bağlantı Noktaları'nı** tıklatın

![paket yakalama penceresi][4]

### <a name="step-2"></a>2. Adım

Sonuçlara baktığımızda bir hat göze çarpıyor, bağlantı noktası 111'de birden fazla bağlantı vardı. En çok kullanılan bağlantı noktası 3389, uzak masaüstü ve kalan RPC dinamik bağlantı noktalarıdır.

Bu trafik hiçbir şey ifade etmese de, birçok bağlantı için kullanılan ve yönetici tarafından bilinmeyen bir bağlantı noktasıdır.

![şekil 5][5]

### <a name="step-3"></a>3. Adım

Artık yer dışı bir bağlantı noktası belirlediğimize göre, bağlantı noktasına göre çekimimizi filtreleyebiliriz.

Bu senaryodaki filtre:

```
tcp.port == 111
```

Filtre metin kutusunun üstten filtre metnini giriyoruz ve enter tuşuna basıyoruz.

![şekil 6][6]

Sonuçlardan, tüm trafiğin aynı alt ağdaki yerel bir sanal makineden geldiğini görebiliyoruz. Bu trafiğin neden oluştuğunu hala anlayamıyorsak, 111 portunda neden bu aramaları yaptığını belirlemek için paketleri daha fazla inceleyebiliriz. Bu bilgilerle gerekli önlemleri alabiliriz.

## <a name="next-steps"></a>Sonraki adımlar

Azure ağ izleme genel görünümünü ziyaret ederek Ağ İzleyicisinin diğer [tanılama](network-watcher-monitoring-overview.md) özellikleri hakkında bilgi edinin

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













