---
title: Apache Kafka istemcileri için önerilen Yapılandırma-Azure Event Hubs
description: Bu makalede, Azure Event Hubs Apache Kafka için etkileşim kuran istemciler için önerilen Apache Kafka konfigürasyonları sunulmaktadır.
ms.topic: reference
ms.date: 07/20/2020
ms.openlocfilehash: f9a03d1d3433461a575b32cd69893408a8b0ef97
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87099717"
---
# <a name="recommended-configurations-for-apache-kafka-clients"></a>Apache Kafka istemcileri için önerilen yapılandırma
Apache Kafka istemci uygulamalarından Azure Event Hubs kullanmak için önerilen yapılandırma aşağıda verilmiştir. 

## <a name="java-client-configuration-properties"></a>Java istemci yapılandırma özellikleri

### <a name="producer-and-consumer-configurations"></a>Üretici ve tüketici yapılandırması

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|-----:|---
`metadata.max.age.ms` | 180000 (yaklaşık) | < 240000 | Meta veri değişikliklerinin daha erken çekilmesi için düşürülemez.
`connections.max.idle.ms`   | 180000 | < 240000 | Azure, gelen TCP boşta > 240.000 MS 'yi kapatır ve bu da ölü bağlantılara (gönderme zaman aşımı nedeniyle süresi doldu olarak gösterilir) neden olabilir.

### <a name="producer-configurations-only"></a>Yalnızca üretici yapılandırması
Üretici yapılandırması [burada](https://kafka.apache.org/documentation/#producerconfigs)bulunabilir.

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|---:|---
`max.request.size` | 1000000 | < 1046528 | 1.046.528 bayttan daha büyük istekler gönderildiğinde hizmet bağlantıları kapatacak.  *Bu değer **değiştirilmelidir** ve yüksek verimlilik üretme senaryolarında soruna neden olur.*
`retries` | > 0 | | Delivery.timeout.ms değerini artırmayı gerektirebilir, bkz. belgeler.
`request.timeout.ms` | 30000.. 60000 | > 20000| EH, varsayılan olarak en az 20.000 MS için varsayılan değer olarak kullanılır.  *Düşük zaman aşımı değerleri olan istekler kabul edilirken, istemci davranışı garanti edilmez.*
`metadata.max.idle.ms` | 180000 | > 5000 | Producer 'ın boşta olan bir konu için meta verileri ne kadar süreyle önbelleğe alınacağını denetler. Bir konunun en son üretilmesinden beri geçen süre meta veri boşta kalma süresini aşarsa, konunun meta verileri unutuldu ve buna bir sonraki erişim, meta veri getirme isteğine zorlayacaktır.
`linger.ms` | > 0 | | Yüksek aktarım hızı senaryolarında, iş üretimini avantajlarından yararlanmak için, lzil değeri en yüksek toleranyabilecek değere eşit olmalıdır.
`delivery.timeout.ms` | | | ( `request.timeout.ms`  +  `linger.ms` ) * Formülüne göre ayarlayın `retries` .
`enable.idempotence` | yanlış | | Idempotan desteklenmiyor.
`compression.type` | `none` | | Sıkıştırma Şu anda desteklenmiyor..

### <a name="consumer-configurations-only"></a>Yalnızca tüketici yapılandırması
[Burada](https://kafka.apache.org/documentation/#consumerconfigs)tüketici yapılandırması bulunabilir.

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|-----:|---
`heartbeat.interval.ms` | 3000 | | 3000 varsayılan değerdir ve değiştirilmemelidir.
`session.timeout.ms` | 30000 |6000.. 300000| 30000 ile başlayın, eksik sinyaller nedeniyle sık sık yeniden dengeleyerek artış yapın.


## <a name="librdkafka-configuration-properties"></a>librdkafka yapılandırma özellikleri
Ana `librdkafka` yapılandırma dosyası ([bağlantı](https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md)) aşağıdaki özellikler için genişletilmiş açıklamalar içerir.

### <a name="producer-and-consumer-configurations"></a>Üretici ve tüketici yapılandırması

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|-----:|---
`socket.keepalive.enable` | true | | Bağlantının boşta olması bekleniyorsa gereklidir.  Azure, 240.000 MS gelen TCP boşta > kapatır.
`metadata.max.age.ms` | ~ 180000| < 240000 | Meta veri değişikliklerinin daha erken çekilmesi için düşürülemez.

### <a name="producer-configurations-only"></a>Yalnızca üretici yapılandırması

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|-----:|---
`retries` | > 0 | | Varsayılan değer 2 ' dir. Bu değeri tutmanız önerilir. 
`request.timeout.ms` | 30000.. 60000 | > 20000| EH, varsayılan olarak en az 20.000 MS için varsayılan değer olarak kullanılır.  `librdkafka`Varsayılan değer, sorunlu olabilecek 5000 ' dir. *Düşük zaman aşımı değerleri olan istekler kabul edilirken, istemci davranışı garanti edilmez.*
`partitioner` | `consistent_random` | Bkz. librdkafka belgeleri | `consistent_random`Varsayılan ve en iyisidir.  Çoğu durumda boş ve null anahtarlar ideal olarak işlenir.
`enable.idempotence` | yanlış | | Idempotan desteklenmiyor.
`compression.codec` | `none` || Sıkıştırma Şu anda desteklenmiyor.

### <a name="consumer-configurations-only"></a>Yalnızca tüketici yapılandırması

Özellik | Önerilen değerler | İzin verilen Aralık | Notlar
---|---:|-----:|---
`heartbeat.interval.ms` | 3000 || 3000 varsayılan değerdir ve değiştirilmemelidir.
`session.timeout.ms` | 30000 |6000.. 300000| 30000 ile başlayın, eksik sinyaller nedeniyle sık sık yeniden dengeleyerek artış yapın.


## <a name="further-notes"></a>Daha fazla Not

Aşağıdaki yapılandırma ile ilgili ortak hata senaryoları tablosunu inceleyin.

Belirtiler | Sorun | Çözüm
----|---|-----
Yeniden dengeleme nedeniyle işleme başarısızlıklarını dengelemek | Tüketicinizin yoklama () çağrıları arasında çok uzun bekleme süresini bekliyor ve hizmet tüketiciye Grup dışına ıyor. | Birkaç seçeneğiniz vardır: <ul><li>oturum zaman aşımını artırma</li><li>işlemeyi hızlandırmak için ileti toplu iş boyutunu azaltın</li><li>tüketici. yoklama () engellemesini önlemek için paralelleştirme işlemeyi geliştirme</li></ul> Üçünün bir birleşimini uygulamak büyük olasılıkla wisest.
Yüksek üretim üretilen iş üzerinde ağ özel durumları | Java istemcisi + varsayılan maks. istek. size mi kullanıyorsunuz?  İstekleriniz çok büyük olabilir. | Yukarıdaki Java yapılandırmalarını konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
Tüm Azure hizmetlerinin kotaları ve limitleri için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](..//azure-resource-manager/management/azure-subscription-service-limits.md) . 
