---
title: Azure ön kapılarında ölçümleri ve günlükleri izleme | Microsoft Docs
description: Bu makalede, Azure ön kapısının desteklediği farklı ölçümler ve erişim günlükleri açıklanmaktadır
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 7939daf97309d5026fd24732cd922e47c2b16812
ms.sourcegitcommit: 24f31287b6a526e23ff5b5469113522d1ccd4467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84743635"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door"></a>Azure ön kapılarında ölçümleri ve günlükleri izleme

Azure ön kapısını kullanarak kaynakları aşağıdaki yollarla izleyebilirsiniz:

- **Ölçümler**. Azure ön kapısının Şu anda performans sayaçlarını görüntülemek için yedi ölçümü vardır.
- **Günlükler**. Etkinlik ve tanılama günlükleri, izleme amacıyla bir kaynaktan performans, erişim ve diğer verilerin kaydedilmesine veya kullanılmasına olanak tanır.

### <a name="metrics"></a>Ölçümler

Ölçümler, portalda performans sayaçlarını görüntülemenize olanak tanıyan belirli Azure kaynaklarına yönelik bir özelliktir. Aşağıda kullanılabilir ön kapı ölçümleri verilmiştir:

| Ölçüm | Ölçüm görünen adı | Birim | Boyutlar | Description |
| --- | --- | --- | --- | --- |
| Istek sayısı | İstek Sayısı | Sayı | Http durumu</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Ön kapı tarafından sunulan istemci isteklerinin sayısı.  |
| RequestSize | İstek boyutu | Bayt | Http durumu</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemcilerden ön kapıya istek olarak gönderilen bayt sayısı. |
| Yanıt boyutu | Yanıt boyutu | Bayt | Http durumu</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Ön kapıdan istemcilere yanıt olarak gönderilen bayt sayısı. |
| TotalLatency | Toplam gecikme süresi | Mayacak | Http durumu</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemci, ön kapıdan gelen son yanıt baytını kabul edene kadar ön kapıdan alınan istemci isteğinden hesaplanan süredir. |
| BackendRequestCount | Arka uç Istek sayısı | Sayı | Http durumu</br>HttpStatusGroup</br>Arka uç | Ön kapıdan arka uçlara gönderilen isteklerin sayısı. |
| BackendRequestLatency | Arka uç Isteği gecikmesi | Mayacak | Arka uç | Ön kapı arka uca Son Yanıt baytı aldığından, isteğin ön uca gönderildiği süre. |
| BackendHealthPercentage | Arka uç sistem durumu yüzdesi | Yüzde | Arka uç</br>BackendPool | Ön kapıdan arka uçlara yapılan başarılı sistem durumu araştırmalarının yüzdesi. |
| WebApplicationFirewallRequestCount | Web uygulaması güvenlik duvarı Istek sayısı | Sayı | PolicyName</br>RuleName</br>Eylem | Ön kapısının uygulama katmanı güvenliği tarafından işlenen istemci isteklerinin sayısı. |

## <a name="activity-logs"></a><a name="activity-log"></a>Etkinlik günlükleri

Etkinlik günlükleri, ön kapıda yapılan işlemler hakkında bilgi sağlar. Bunlar ayrıca, ön kapıda herhangi bir yazma işlemi (put, post veya delete) için ne zaman, kim ve ne zaman alındığını da saptayacaktır.

>[!NOTE]
>Etkinlik günlükleri okuma (Get) işlemlerini içermez. Ayrıca, Azure portal veya orijinal yönetim API 'sini kullanarak gerçekleştirdiğiniz işlemleri de içermez.

Ön kapıdaki etkinlik günlüklerine veya Azure Izleyici 'deki Azure kaynaklarınızın tüm günlüklerine erişin. Etkinlik günlüklerini görüntülemek için:

1. Ön kapı örneğinizi seçin.
2. **Etkinlik günlüğü**' nü seçin.

    ![Etkinlik günlüğü](./media/front-door-diagnostics/activity-log.png)

3. Bir filtreleme kapsamı seçin ve ardından **Uygula**' yı seçin.

## <a name="diagnostic-logs"></a><a name="diagnostic-logging"></a>Tanılama günlükleri
Tanılama günlükleri, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır.

Etkinlik günlükleri, Azure kaynakları üzerinde gerçekleştirilen işlemlere yönelik öngörüler sağlar. Tanılama günlükleri, kaynağınızın gerçekleştirdiği işlemlere ilişkin öngörüler sağlar. Daha fazla bilgi için bkz. [Azure izleyici tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md).

![Tanılama günlükleri](./media/front-door-diagnostics/diagnostic-log.png)

Ön kapılarınız için tanılama günlüklerini yapılandırmak için:

1. Azure ön Kapıınızı seçin.

2. **Tanılama ayarları**' nı seçin.

3. **Tanılamayı aç '** ı seçin. Tanılama günlüklerini bir depolama hesabı ölçümleriyle birlikte arşivleyin, bunları bir olay hub 'ına akıtır veya Azure Izleyici günlüklerine gönderin.

Ön kapı şu anda tanılama günlükleri sağlıyor (toplu saatlik). Tanılama günlükleri, her giriş için aşağıdaki şemaya sahip tek API istekleri sağlar:

| Özellik  | Açıklama |
| ------------- | ------------- |
| BackendHostname | İstek bir arka uca iletilirse Bu alan, arka ucun ana bilgisayar adını temsil eder. Bu alan, istek yeniden yönlendirildiyse veya bir bölgesel önbelleğe iletilirse (yönlendirme kuralı için önbelleğe alma etkinleştirildiğinde) boş olur. |
| CacheStatus | Önbelleğe alma senaryolarında bu alan, POP 'ta önbelleğin isabet/isabetsizlik sını tanımlar |
| ClientIp | İsteği yapan istemcinin IP adresi. İstekte bir X-Iletilmiş-for üstbilgisi varsa, Istemci IP 'si aynı üzerinden çekilir. |
| Istemci bağlantı noktası | İsteği yapan istemcinin IP bağlantı noktası. |
| HttpMethod | İstek tarafından kullanılan HTTP yöntemi. |
| HttpStatusCode | Proxy 'den döndürülen HTTP durum kodu. |
| HttpStatusDetails | İstek üzerine sonuçtaki durum. Bu dize değerinin anlamı, bir durum başvuru tablosunda bulunabilir. |
| HttpVersion | İstek veya bağlantı türü. |
| POP | İsteğin bulunduğu kenarın kısa adı. |
| RequestBytes | İstek üst bilgileri ve istek gövdesi dahil olmak üzere HTTP isteği iletisinin bayt cinsinden boyutu. |
| RequestUri | Alınan isteğin URI 'SI. |
| ResponseBytes | Yanıt olarak arka uç sunucusu tarafından gönderilen bayt.  |
| RoutingRuleName | İsteğin eşleştiği yönlendirme kuralının adı. |
| SecurityProtocol | İstek tarafından kullanılan TLS/SSL protokol sürümü veya şifreleme yoksa null. |
| SentToOriginShield | İlk ortamda bir önbellek isabetsizlik olup olmadığını ve isteğin bölgesel önbelleğe gönderilip gönderilmediğini temsil eden Boole alanı. Yönlendirme kuralı bir yeniden yönlendirme veya ne zaman önbelleğe alma etkin değilse bu alanı yoksayın. |
| TimeTaken | İşlemin gerçekleştiği sürenin milisaniye cinsinden uzunluğu. |
| TrackingReference | Ön kapıya yönelik olarak sunulan ve istemciye X-Azure-ref üst bilgisi olarak gönderilen bir isteği tanımlayan benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerindeki ayrıntıları aramak için gereklidir. |
| Kullanıcı | İstemcinin kullandığı tarayıcı türü. |

**Note:** Çeşitli yönlendirme yapılandırmalarının ve trafik davranışları için backendHostname, cacheStatus, sentToOriginShield ve POP alanı gibi bazı alanlar farklı değerlerle yanıt verebilir. Aşağıdaki tabloda farklı değerler açıklanmakta, bu alanlar çeşitli senaryolar için olacaktır:

| Senaryolar | Günlük girişi sayısı | POP | BackendHostname | SentToOriginShield | CacheStatus |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Önbelleğe alma etkin olmayan yönlendirme kuralı | 1 | Edge POP kodu | İsteğin iletildiği arka uç | False | CONFIG_NOCACHE |
| Önbelleğe alma özelliği etkin yönlendirme kuralı. Uç POP 'ta isabetli önbellek okuması | 1 | Edge POP kodu | Olmamalıdır | False | SıNAMASıNA |
| Önbelleğe alma özelliği etkin yönlendirme kuralı. Uç POP 'ta önbellek isabetsizlik, ancak üst önbellek AÇıSıNı önbellekte isabet | 2 | 1. Edge POP kodu</br>2. üst önbellek POP kodu | 1. üst önbellek POP ana bilgisayar adı</br>2. boş | 1. doğru</br>2. yanlış | 1. ISABETSIZLIK</br>2. PARTIAL_HIT |
| Önbelleğe alma özelliği etkin yönlendirme kuralı. Hem kenar hem de üst önbellek AÇıSıNı önbellekte isabetsiz önbellek | 2 | 1. Edge POP kodu</br>2. üst önbellek POP kodu | 1. üst önbellek POP ana bilgisayar adı</br>2. önbelleğin doldurulmaya yardımcı olan arka uç | 1. doğru</br>2. yanlış | 1. ISABETSIZLIK</br>2. ISABETSIZLIK |


## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı profili oluşturma](quickstart-create-front-door.md)
- [Ön kapı nasıl kullanılır?](front-door-routing-architecture.md)
