---
title: Redis için Azure Cache istemci tarafı sorunlarını giderme
description: Redis istemci bellek basıncı, trafik patlaması, yüksek CPU, sınırlı bant genişliği, büyük istekler veya büyük yanıt boyutu gibi Redis için Azure Önbelleği ile sık karşılaşılan istemci tarafındaki sorunları nasıl çözeceğinizi öğrenin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: troubleshooting
ms.date: 10/18/2019
ms.openlocfilehash: ace953fcb278604cb64eef463753f0f2622d3d24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277953"
---
# <a name="troubleshoot-azure-cache-for-redis-client-side-issues"></a>Redis için Azure Cache istemci tarafı sorunlarını giderme

Bu bölümde, uygulamanızın kullandığı Redis istemcisindeki bir durum nedeniyle oluşan sorun giderme sorunları tartışılır.

- [Redis istemcisi üzerinde bellek baskısı](#memory-pressure-on-redis-client)
- [Trafik patlaması](#traffic-burst)
- [Yüksek istemci CPU kullanımı](#high-client-cpu-usage)
- [İstemci tarafı bant genişliği sınırlaması](#client-side-bandwidth-limitation)
- [Büyük istek veya yanıt boyutu](#large-request-or-response-size)

## <a name="memory-pressure-on-redis-client"></a>Redis istemcisi üzerinde bellek baskısı

İstemci makinesiüzerindeki bellek basıncı, önbellekteki yanıtların işlenmesini geciktirebilecek her türlü performans sorununa yol açar. Bellek basıncı çarptığında, sistem verileri diske sayfalayabilir. Bu _sayfa hatası,_ sistemin önemli ölçüde yavaşlamasına neden olur.

İstemci üzerindeki bellek basıncını algılamak için:

- Kullanılabilir belleği aşmadığından emin olmak için makinedeki bellek kullanımını izleyin.
- Müşterinin `Page Faults/Sec` performans sayacını izleyin. Normal çalışma sırasında, çoğu sistem bazı sayfa hataları var. İstek zaman ekmeleriyle karşılık gelen sayfa hatalarında ani artışlar bellek basıncını gösterebilir.

İstemci üzerindeki yüksek bellek basıncı çeşitli şekillerde azaltılabilir:

- İstemcinin bellek tüketimini azaltmak için bellek kullanım kalıplarınızı araştırın.
- İstemcinizin VM'sini daha fazla bellekle daha büyük bir boyuta yükseltin.

## <a name="traffic-burst"></a>Trafik patlaması

Kötü `ThreadPool` ayarlarla birlikte trafik patlamaları, Redis Server tarafından gönderilen ancak henüz istemci tarafında tüketilmese de verilerin işlenmesinde gecikmelere neden olabilir.

Bir `ThreadPool` [örneği `ThreadPoolLogger` ](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs)kullanarak istatistiklerinizin zaman içinde nasıl değiştiğini izleyin. StackExchange.Redis'ten gelen iletileri daha fazla araştırmak için aşağıdaki gibi kullanabilirsiniz: `TimeoutException`

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Önceki özel durum, ilginç çeşitli sorunlar vardır:

- `IOCP` Bölümde ve `WORKER` bölümde `Min` değerden daha `Busy` büyük bir değere sahip olduğunuza dikkat edin. Bu fark, `ThreadPool` ayarlarınızın ayarlanması gerektiği anlamına gelir.
- Ayrıca görebilirsiniz. `in: 64221` Bu değer, istemcinin çekirdek soketi katmanından 64.211 bayt alındığını, ancak uygulama tarafından okunmadığını gösterir. Bu fark genellikle uygulamanızın (örneğin StackExchange.Redis) ağdaki verileri sunucunun size gönderdiği kadar hızlı okumadığı anlamına gelir.

İş parçacığı havuzunuzun seri çözüm senaryoları altında hızla ölçeklendirildidiğinden emin olmak için [Ayarlarınızı `ThreadPool` yapılandırabilirsiniz.](cache-faq.md#important-details-about-threadpool-growth)

## <a name="high-client-cpu-usage"></a>Yüksek istemci CPU kullanımı

Yüksek istemci CPU kullanımı, sistemin yapması istenen işe ayak uyduramediğini gösterir. Önbellek yanıtı hızlı bir şekilde gönderse de, istemci yanıtı zamanında işlemeyi başaramayabilir.

Azure portalında bulunan ölçümleri veya makinedeki performans sayaçlarını kullanarak istemcinin sistem genelindeki CPU kullanımını izleyin. Tek bir işlem düşük CPU kullanımına sahip olabilir, ancak sistem genelinde CPU yüksek olabilir, çünkü *işlem* CPU izlemek için dikkatli olun. Zaman ekmelerine karşılık gelen CPU kullanımındaki ani artışlara dikkat edin. Yüksek CPU, Trafik `in: XXX` `TimeoutException` [patlaması](#traffic-burst) bölümünde açıklandığı gibi hata iletilerinde yüksek değerlere de neden olabilir.

> [!NOTE]
> StackExchange.Redis 1.1.603 ve `local-cpu` daha `TimeoutException` sonra hata iletileri metrik içerir. [StackExchange.Redis NuGet paketinin](https://www.nuget.org/packages/StackExchange.Redis/)en son sürümünü kullandığınızdan emin olun. En son sürüme sahip olmak önemlidir, bu yüzden zaman ekmeleri daha sağlam hale getirmek için kod sürekli sabit hatalar vardır.
>

İstemciden yüksek CPU kullanımını azaltmak için:

- CPU artışlarına neyin neden olduğunu araştırın.
- Daha fazla CPU kapasitesiyle istemcinizi daha büyük bir VM boyutuna yükseltin.

## <a name="client-side-bandwidth-limitation"></a>İstemci tarafı bant genişliği sınırlaması

İstemci makinelerinin mimarisine bağlı olarak, kullanılabilir ağ bant genişliğine ne kadar sahip oldukları konusunda sınırlamalar olabilir. İstemci, ağ kapasitesini aşırı yükleyerek kullanılabilir bant genişliğini aşarsa, veriler sunucunun gönderdiği anda istemci tarafında işlenmez. Bu durum zaman adabına yol açabilir.

[Bir örneği `BandwidthLogger` ](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs)kullanarak Bant Genişliği kullanımınızın zaman içinde nasıl değiştiğini izleyin. Bu kod, kısıtlı izinlere sahip bazı ortamlarda (Azure web siteleri gibi) başarılı bir şekilde çalıştırılamayabilir.

Azaltmak için ağ bant genişliği tüketimini azaltın veya istemci VM boyutunu daha fazla ağ kapasitesine sahip bire yükseltin.

## <a name="large-request-or-response-size"></a>Büyük istek veya yanıt Boyutu

Büyük bir istek/yanıt zaman adabına neden olabilir. Örnek olarak, istemcinizde yapılandırılan zaman aşan değerinizin 1 saniye olduğunu varsayalım. Uygulamanız aynı anda (aynı fiziksel ağ bağlantısını kullanarak) iki anahtar (örneğin, 'A' ve 'B') ister. Çoğu istemci, yanıtlarını beklemeden her iki isteğin de birbiri ardına gönderildiği "pipelining" isteğini destekler. Sunucu yanıtları aynı sırada geri gönderir. 'A' yanıtı büyükse, daha sonraki istekler için zaman anına kadar yiyebilir.

Aşağıdaki örnekte, 'A' ve 'B' isteği sunucuya hızlı bir şekilde gönderilir. Sunucu hızlı bir şekilde 'A' ve 'B' yanıtları göndermeye başlar. Veri aktarım süreleri nedeniyle, sunucu hızlı bir şekilde yanıt vermesine rağmen 'B' yanıtı 'A' kez geride beklemelidir.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Bu istek/yanıt ölçmek için zor bir tanesidir. Büyük istek ve yanıtları izlemek için istemci kodunuzu kullanabilirsin.

Büyük yanıt boyutları için çözünürlükler çeşitlidir, ancak şunları içerir:

1. Uygulamanızı birkaç büyük değer yerine çok sayıda küçük değer için optimize edin.
    - Tercih edilen çözüm, verilerinizi ilgili daha küçük değerlere ayırmaktır.
    - Yazıyı görün [Redis için ideal değer boyutu aralığı nedir? 100 KB çok mu büyük?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) neden daha küçük değerlerin tavsiye edilir ayrıntıları için.
1. Daha yüksek bant genişliği özellikleri elde etmek için VM'nizin boyutunu artırın
    - İstemciveya sunucuVM üzerinde daha fazla bant genişliği daha büyük yanıtlar için veri aktarım sürelerini azaltabilir.
    - Her iki makinedeki mevcut ağ kullanımınızı geçerli VM boyutunuzla karşılaştırın. Yalnızca sunucuda veya yalnızca istemcide daha fazla bant genişliği yeterli olmayabilir.
1. Uygulamanızın kullandığı bağlantı nesnesi sayısını artırın.
    - Farklı bağlantı nesneleri üzerinden istekte bulunmak için bir round-robin yaklaşımı kullanın.

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Cache sunucu tarafı sorunlarını giderme](cache-troubleshoot-server.md)
- [Önbelleğimin performansını nasıl kıyaslayabilir ve test edebilirim?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
