---
title: Azure ağ Izleyicisi ile paket incelemesi | Microsoft Docs
description: Bu makalede bir VM 'den toplanan derin paket incelemesi gerçekleştirmek için ağ Izleyicisi 'nin nasıl kullanılacağı açıklanır.
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
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152933"
---
# <a name="packet-inspection-with-azure-network-watcher"></a>Azure ağ Izleyicisi ile paket incelemesi

Ağ Izleyicisi 'nin paket yakalama özelliğini kullanarak portal, PowerShell, CLı ve aracılığıyla Azure sanal makinelerinizdeki yakalama oturumlarını, SDK ve REST API aracılığıyla başlatabilir ve yönetebilirsiniz. Paket yakalama, bilgileri kullanıma hazır bir biçimde sağlayarak paket düzeyi veri gerektiren senaryolara olanak tanır. Verileri incelemek için serbestçe sunulan araçları kullanarak, VM 'lerinizden gelen ve giden iletişimleri inceleyebilir ve ağ trafiğiniz hakkında öngörüler elde edebilirsiniz. Paket yakalama verilerinin bazı örnek kullanımları şunlardır: ağ veya uygulama sorunlarını araştırma, ağ kötüye kullanımı ve yetkisiz giriş girişimlerini algılama veya yasal uyumluluğu koruma. Bu makalede, popüler bir açık kaynak Aracı kullanılarak ağ Izleyicisi tarafından sunulan bir paket yakalama dosyasının nasıl açılacağı gösterilmektedir. Ayrıca, bir bağlantı gecikmesini hesaplamayı, anormal trafiği belirlemeyi ve ağ istatistiklerini incelemenizi gösteren örnekler de sunuyoruz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, daha önce çalıştırılan bir paket yakalamadaki bazı önceden yapılandırılmış senaryolara karşı gider. Bu senaryolarda, bir paket yakalama gözden geçirilerek erişilebilen yetenekler gösterilmektedir. Bu senaryo, paket yakalamayı denetlemek için [Wireshark](https://www.wireshark.org/) kullanır.

Bu senaryo, bir sanal makinede zaten bir paket yakalama çalıştırdığınızı varsayar. Paket yakalama oluşturmayı öğrenmek için bkz. paket [yakalamalarını Portal Ile yönetme](network-watcher-packet-capture-manage-portal.md) veya geri alma Ile [paket yakalamalarını yönetme REST API](network-watcher-packet-capture-manage-rest.md).

## <a name="scenario"></a>Senaryo

Bu senaryoda şunları yapabilirsiniz:

* Paket yakalamayı gözden geçirme

## <a name="calculate-network-latency"></a>Ağ gecikmesini hesapla

Bu senaryoda, iki uç nokta arasında gerçekleşen bir Iletim Denetim Protokolü (TCP) görüşmesinin ilk gidiş dönüş süresini (RTT) görüntülemeyi göstereceğiz.

TCP bağlantısı oluşturulduğunda, bağlantıda gönderilen ilk üç paket, genellikle üç yönlü el sıkışma olarak adlandırılan bir düzene uyar. Bu el sıkışma içinde gönderilen ilk iki paket, istemciden gelen bir başlangıç isteği ve sunucudan bir yanıt inceleyerek, bu bağlantı oluşturulduğunda gecikme süresini hesaplayabiliriz. Bu gecikme süresi, gidiş dönüş süresi (RTT) olarak adlandırılır. TCP protokolü ve üç yönlü el sıkışma hakkında daha fazla bilgi için aşağıdaki kaynağa bakın. [https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip](https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip)

### <a name="step-1"></a>1\. Adım

WireShark Başlat

### <a name="step-2"></a>2\. Adım

Paket yakalamadan **. Cap** dosyasını yükleyin. Bu dosya, nasıl yapılandırdığınıza bağlı olarak, sanal makinede yerel olarak kaydedildiği blob 'da bulunabilir.

### <a name="step-3"></a>3\. Adım

TCP konuşmalarında ilk gidiş dönüş süresini (RTT) görüntülemek için, yalnızca TCP el sıkışmasına sahip olan ilk iki pakete bakacağız. Üç yönlü el sıkışma ([SYN], [SYN, ACK] paketleri) ile ilk iki paketi kullanacağız. TCP üstbilgisinde ayarlanan bayraklar için adlandırılır. Bu senaryoda, [ACK] paketi, el sıkışma içindeki son paket kullanılmayacak. [SYN] paketi istemci tarafından gönderilir. Sunucu alındıktan sonra sunucu [ACK] paketini istemciden SYN alma bildirimi olarak gönderir. Sunucu yanıtının çok az ek yük gerektirmesi durumunda, [SYN, ACK] paketinin istemci tarafından, istemci tarafından gönderilen zaman tarafından alındığı süreyi çıkararak RTT 'yi hesapladık.

WireShark kullanma bu değer bizim için hesaplanır.

TCP üç yönlü el sıkışmasının ilk iki paketini daha kolay bir şekilde görüntülemek için, WireShark tarafından sunulan filtreleme özelliğini kullanacağız.

Filtreyi WireShark içinde uygulamak için, yakalamadaki bir [SYN] paketinin "Iletim Denetim Protokolü" segmentini genişletin ve TCP üstbilgisinde ayarlanan bayrakları inceleyin.

Tüm [SYN] ve [SYN, ACK] paketlerine filtre uygulamayı aradığımızdan, bayraklar altında SYN bitinin 1 olarak ayarlandığını onaylayın ve sonra SYN bit > Apply as Filter-> olarak Uygula ' ya sağ tıklayın.

![Şekil 7][7]

### <a name="step-4"></a>4\. Adım

Pencereyi yalnızca [SYN] bit kümesiyle yalnızca paketleri görmek üzere filtreleyerek, ilk RTT 'yi görüntülemek için ilgilendiğiniz konuşmaları kolayca seçebilirsiniz. WireShark ' de RTT görüntülemenin basit bir yolu, "SEQ/ACK" analizinde işaretlenen açılan listeye tıklamanız yeterlidir. Daha sonra RTT görüntülendiğini görürsünüz. Bu durumda, RTT 0,0022114 saniye veya 2,211 MS idi.

![Şekil 8][8]

## <a name="unwanted-protocols"></a>İstenmeyen protokoller

Azure 'da dağıttığınız bir sanal makine örneği üzerinde çalışan birçok uygulama olabilir. Bu uygulamaların birçoğu, büyük olasılıkla açık izniniz olmadan ağ üzerinden iletişim kurar. Ağ iletişimini depolamak için paket yakalama 'yı kullanarak, uygulamanın ağ üzerinde nasıl konuştudığını araştırabilir ve herhangi bir sorun araması yapabilirsiniz.

Bu örnekte, makinenizde çalışan bir uygulamadan yetkisiz iletişimi gösterebilen istenmeyen protokoller için önceki bir çalıştırılan paket yakalamayı gözden geçiririz.

### <a name="step-1"></a>1\. Adım

Önceki senaryoda aynı yakalamanın kullanılması **, > ** **protokol hiyerarşisi** ' ne tıklayın.

![protokol hiyerarşisi menüsü][2]

Protokol hiyerarşisi penceresi görüntülenir. Bu görünüm, yakalama oturumu sırasında kullanılmakta olan tüm protokollerin listesini ve protokolleri kullanarak aktarılan ve alınan paketlerin sayısını sağlar. Bu görünüm, sanal makinelerinizde veya ağınızda istenmeyen ağ trafiğini bulmak için yararlı olabilir.

![protokol hiyerarşisi açıldı][3]

Aşağıdaki ekran yakalamada görebileceğiniz gibi, eşler arası dosya paylaşımı için kullanılan Bittorkiralık protokolünü kullanan trafik vardı. Yönetici olarak, bu belirli sanal makinelerde Bitukiralık trafiği görmeyi beklememeniz gerekmez. Artık bu trafiği öğrenolduğunuza göre, bu sanal makinede yüklü olan eşler arası yazılımı kaldırabilir veya ağ güvenlik grupları veya güvenlik duvarı kullanarak trafiği engelleyebilirsiniz. Ayrıca, paket yakalamalarını bir zamanlamaya göre çalıştırmayı tercih edebilirsiniz, böylece sanal makinelerinizdeki protokol kullanımını düzenli olarak inceleyebilirsiniz. Azure 'da ağ görevlerini otomatikleştirme hakkında bir örnek için bkz. [Azure Otomasyonu ile ağ kaynaklarını izleme](network-watcher-monitor-with-azure-automation.md)

## <a name="finding-top-destinations-and-ports"></a>En üst hedefleri ve bağlantı noktalarını bulma

Trafik türlerini, uç noktaları ve üzerinden iletilen bağlantı noktalarını anlamak, ağınızdaki uygulamaları ve kaynakları izlerken veya sorunlarını giderirken önemli bir öneme sahiptir. Yukarıdaki bir paket yakalama dosyasını kullanarak, sanal makinenizin iletişim kurduğu en üst hedefleri ve kullanılan bağlantı noktalarını hızlıca öğreniyoruz.

### <a name="step-1"></a>1\. Adım

Önceki senaryoda aynı yakalamanın kullanılması, **istatistikler** > **IPv4 istatistikleri** > **hedefler ve bağlantı noktaları** ' na tıklayın

![paket yakalama penceresi][4]

### <a name="step-2"></a>2\. Adım

Bir satırın temsil edildiği sonuçlara baktığımızda, 111 numaralı bağlantı noktası üzerinde birden fazla bağlantı vardı. En çok kullanılan bağlantı noktası, uzak masaüstü olan 3389 ve kalan RPC dinamik bağlantı noktalardır.

Bu trafik hiçbir şey anlamına gelirken, çok sayıda bağlantı için kullanılan ve yönetici tarafından bilinmeyen bir bağlantı noktasıdır.

![Şekil 5][5]

### <a name="step-3"></a>3\. Adım

Artık bir yerinde bağlantı noktası belirlediğimizden, yakaımızı bağlantı noktasına göre filtreleyebiliriz.

Bu senaryodaki filtre şöyle olacaktır:

```
tcp.port == 111
```

Filtre metni ' nden filtre metin kutusuna girer ve ENTER tuşuna basın.

![Şekil 6][6]

Sonuçlardan, tüm trafiğin aynı alt ağdaki yerel bir sanal makineden geldiğini görebiliriz. Bu trafiğin neden gerçekleştiğini anlamıyorsanız, 111 numaralı bağlantı noktasında neden bu çağrıları yaptığını tespit etmek için paketleri daha fazla inceleyebilirsiniz. Bu bilgilerle ilgili eylemi gerçekleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure ağ izlemeye genel bakış ' ı](network-watcher-monitoring-overview.md) ziyaret ederek Ağ İzleyicisi 'nin diğer tanılama özellikleri hakkında bilgi edinin

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













