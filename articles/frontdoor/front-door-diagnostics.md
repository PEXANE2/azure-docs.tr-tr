---
title: Azure Ön Kapı'daki ölçümleri ve günlükleri izleme| Microsoft Dokümanlar
description: Bu makalede, Azure Ön Kapı'nın desteklediği farklı ölçümler ve erişim günlükleri açıklanmaktadır
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: b935355cce36a6e26b168db286ab40248f8f0f68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471736"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door"></a>Azure Ön Kapı'da ölçümleri ve günlükleri izleme

Azure Ön Kapı'yı kullanarak kaynakları aşağıdaki yollarla izleyebilirsiniz:

- **Ölçümler**. Azure Ön Kapı şu anda performans sayaçlarını görüntülemek için yedi ölçüme sahiptir.
- **Günlükler**. Etkinlik ve tanılama günlükleri, performans, erişim ve diğer verilerin izleme amacıyla bir kaynaktan kaydedilmesine veya tüketilmesine olanak sağlar.

### <a name="metrics"></a>Ölçümler

Ölçümler, portaldaki performans sayaçlarını görüntülemenize olanak tanıyan belirli Azure kaynakları için bir özelliktir. Aşağıdaki ön kapı ölçümleri mevcuttur:

| Ölçüm | Metrik Görüntü Adı | Birim | Boyutlar | Açıklama |
| --- | --- | --- | --- | --- |
| İstek Sayısı | İstek Sayısı | Sayı | httpstatus</br>httpstatusgroup</br>Müşteri Bölgesi</br>Müşteri Ülke | Front Door tarafından sunulan istemci isteklerinin sayısı.  |
| İstek Boyutu | İstek Boyutu | Bayt | httpstatus</br>httpstatusgroup</br>Müşteri Bölgesi</br>Müşteri Ülke | İstemcilerden Front Door'a istek olarak gönderilen bayt sayısı. |
| YanıtBoyutu | Yanıt Boyutu | Bayt | httpstatus</br>httpstatusgroup</br>Müşteri Bölgesi</br>Müşteri Ülke | Ön Kapı'dan istemcilere yanıt olarak gönderilen bayt sayısı. |
| TotalLatency | Toplam Gecikme | Milisaniye | httpstatus</br>httpstatusgroup</br>Müşteri Bölgesi</br>Müşteri Ülke | Müşteri Ön Kapı'dan son yanıt baytını kabul edene kadar Ön Kapı tarafından alınan istemci isteğinden hesaplanan süre. |
| BackendRequestCount | Arka Uç İstek Sayısı | Sayı | httpstatus</br>httpstatusgroup</br>Arka uç | Ön Kapıdan arka uçlara gönderilen istek sayısı. |
| BackendRequestLatency | Arka Uç İstek GecikmeSi | Milisaniye | Arka uç | Ön Kapı arka uçtan son yanıt bayt ı alana kadar isteğin Ön Kapı tarafından arka uca gönderildiği tarihten hesaplanan süre. |
| BackendHealthPercentage | Arka Uç Sağlık Yüzdesi | Yüzde | Arka uç</br>Arka uçHavuz | Front Door'dan arka uçlara kadar başarılı sağlık sondalarının yüzdesi. |
| WebApplicationFirewallRequestCount | Web Uygulaması Güvenlik Duvarı İstek Sayısı | Sayı | PolicyName</br>Rulename</br>Eylem | Ön Kapı uygulama katmanı güvenliği tarafından işlenen istemci isteklerinin sayısı. |

## <a name="activity-logs"></a><a name="activity-log"></a>Etkinlik günlükleri

Etkinlik günlükleri Ön Kapı'da yapılan işlemler hakkında bilgi sağlar. Ayrıca, ön kapıda alınan herhangi bir yazma işleminin (put, post veya delete) ne zaman, kim olduğunu ve ne zaman alındığını da belirlerler.

>[!NOTE]
>Etkinlik günlükleri okuma (al) işlemlerini içermez. Ayrıca, Azure portalını veya orijinal Yönetim API'sini kullanarak gerçekleştirdiğiniz işlemleri de içermezler.

Azure Monitor'da Ön Kapınızda veya Azure kaynaklarınızın tüm günlüklerine erişin. Etkinlik günlüklerini görüntülemek için:

1. Ön Kapı örneğini seçin.
2. **Etkinlik günlükünü**seçin.

    ![Etkinlik günlüğü](./media/front-door-diagnostics/activity-log.png)

3. Bir filtreleme kapsamı seçin ve sonra **Uygula'yı**seçin.

## <a name="diagnostic-logs"></a><a name="diagnostic-logging"></a>Tanılama günlükleri
Tanılama günlükleri, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri etkinlik günlüklerinden farklıdır.

Etkinlik günlükleri, Azure kaynaklarında yapılan işlemlerle ilgili öngörüler sağlar. Tanılama günlükleri, kaynağınızın gerçekleştirdiği işlemlerhakkında bilgi sağlar. Daha fazla bilgi için [Azure Monitörtanı günlüklerine](../azure-monitor/platform/platform-logs-overview.md)bakın.

![Tanılama günlükleri](./media/front-door-diagnostics/diagnostic-log.png)

Ön Kapınız için tanılama günlüklerini yapılandırmak için:

1. Azure Ön Kapınızı seçin.

2. **Tanılama ayarlarını**seçin.

3. **Tanılamayı Aç'ı**seçin. Tanılama günlüklerini bir depolama hesabına arşivleyin, bir etkinlik merkezine aktarın veya Azure Monitor günlüklerine gönderin.

Ön Kapı şu anda tanılama günlükleri (toplu saatlik) sağlar. Tanılama günlükleri, her girişte aşağıdaki şemaiçeren ayrı API istekleri sağlar:

| Özellik  | Açıklama |
| ------------- | ------------- |
| BackendHostname | İstek bir arka uca iletiliyorsa, bu alan arka uç ana bilgisayar adını temsil eder. İstek yeniden yönlendirildiyse veya bölgesel bir önbelleğe iledilirse (yönlendirme kuralı için önbelleğe alma etkinleştirildiğinde) bu alan boş olacaktır. |
| Önbellek Durumu | Önbelleğe alma senaryoları için bu alan POP'taki önbellek isabetini/kaçırdığını tanımlar |
| ClientIp | İstekte bulundu müşterinin IP adresi. İstekte Bir X-Forwarded-For başlığı varsa, istemci IP aynı seçilir. |
| ClientPort | İstemi yapan istemcinin IP bağlantı noktası. |
| Http Yöntemi | İstek tarafından kullanılan HTTP yöntemi. |
| httpstatuscode | HTTP durum kodu proxy'den döndürülür. |
| httpStatusDetails | İstek te ortaya çıkan durum. Bu dize değerinin anlamı durum başvuru tablosunda bulunabilir. |
| httpVersion | İstek veya bağlantı türü. |
| POP | İsteğin indiği kenarın kısa adı. |
| İstekBaytlar | İstek üstbilgisi ve istek gövdesi de dahil olmak üzere baytlar daki HTTP istek iletisinin boyutu. |
| Requesturi | Uri alınan istek. |
| YanıtBayt | Yanıt olarak arka uç sunucusu tarafından gönderilen baytlar.  |
| YönlendirmeKuralName | İsteğin eşolduğu yönlendirme kuralının adı. |
| Securityprotocol | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null. |
| SenttoOriginShield | Boolean alanı ilk ortamda bir önbellek miss olup olmadığını temsil eden ve istek bölgesel önbelleğe gönderildi. Yönlendirme kuralı yeniden yönlendirme yse veya önbelleğe alma etkin olmadığında bu alanı yoksayın. |
| Zaman Dilimi | Eylemin milisaniye cinsinden aldığı süre. |
| İzlemeReferans | Ön Kapı tarafından sunulan bir isteği tanımlayan ve istemciye X-Azure-Ref üstbilgisi olarak gönderilen benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerinde arama ayrıntıları için gereklidir. |
| Useragent | İstemcinin kullandığı tarayıcı türü. |

**Not:** Çeşitli yönlendirme yapılandırmaları ve trafik davranışları için, backendHostname, cacheStatus, sentToOriginShield ve POP alanı gibi alanlardan bazıları farklı değerlerle yanıt verebilir. Aşağıdaki tabloda farklı değerler açıklanmaktadır, bu alanlar çeşitli senaryolar için olacaktır:

| Senaryolar | Günlük girişlerinin sayısı | POP | BackendHostname | SenttoOriginShield | Önbellek Durumu |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Önbelleğe almadan yönlendirme kuralı etkin | 1 | Kenar POP kodu | İsteğin iletildiği arka uç | False | CONFIG_NOCACHE |
| Önbelleğe alma etkin olan yönlendirme kuralı. Önbellek kenarında POP vurdu | 1 | Kenar POP kodu | Boş | False | Vurmak |
| Önbelleğe alma etkin olan yönlendirme kuralı. Önbellek kenar POP özledim ama önbellek üst önbellek POP isabet | 2 | 1. Kenar POP kodu</br>2. Üst önbellek POP kodu | 1. Üst önbellek POP hostname</br>2. Boş | 1. Doğru</br>2. Yanlış | 1. MISS</br>2. PARTIAL_HIT |
| Önbelleğe alma etkin olan yönlendirme kuralı. Önbellek hem kenar hem de üst önbellek POP özledim | 2 | 1. Kenar POP kodu</br>2. Üst önbellek POP kodu | 1. Üst önbellek POP hostname</br>2. Önbelleği doldurmaya yardımcı olan arka uç | 1. Doğru</br>2. Yanlış | 1. MISS</br>2. MISS |


## <a name="next-steps"></a>Sonraki adımlar

- [Ön Kapı profili oluşturma](quickstart-create-front-door.md)
- [Ön Kapı nasıl çalışır?](front-door-routing-architecture.md)
