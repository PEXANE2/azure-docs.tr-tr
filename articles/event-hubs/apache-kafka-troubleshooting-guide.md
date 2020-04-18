---
title: Apache Kafka için Azure Etkinlik Hub'larıyla ilgili sorun giderme sorunları
description: Bu makalede, Apache Kafka için Azure Etkinlik Hub'ları ile ilgili sorunları nasıl gidereceğiniz gösterilmektedir
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2020
ms.author: shvija
ms.openlocfilehash: 12ddc5fa74b7a1b42bbd64fde9ec3410b1c1e425
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606736"
---
# <a name="apache-kafka-troubleshooting-guide-for-event-hubs"></a>Etkinlik Hub'ları için Apache Kafka sorun giderme kılavuzu
Bu makalede, Apache Kafka için Olay Hub'ları kullanırken karşılaşabileceğiniz sorunlar için sorun giderme ipuçları verilmektedir. 

## <a name="server-busy-exception"></a>Sunucu Meşgul özel durumu
Kafka azaltma nedeniyle Server Busy özel durumu alabilirsiniz. AMQP istemcileri ile Olay Hub'ları, hizmet azaltma üzerine sunucu **meşgul** özel durumunu hemen döndürür. "Daha sonra tekrar dene" iletisine eşdeğerdir. Kafka'da mesajlar tamamlanmadan önce geciktirilir. Gecikme uzunluğu, produce/fetch `throttle_time_ms` yanıtında olduğu gibi milisaniye cinsinden döndürülür. Çoğu durumda, bu gecikmiş istekler Olay Hub'ları panolarında ServerBusy özel durumları olarak günlüğe kaydedilmez. Bunun yerine, yanıtın `throttle_time_ms` değeri, girdinin sağlanan kotayı aştığını gösteren bir gösterge olarak kullanılmalıdır.

Trafik aşırıysa, hizmet aşağıdaki davranışa sahiptir:

- Ürün isteğinin gecikmesi istek zaman aşımını aşıyorsa, Olay Hub'ları **İlke İhlali** hata kodunu döndürür.
- Alma isteğinin gecikmesi istek zaman aşımını aşıyorsa, Olay Hub'ları isteği daraltılmış olarak kaydeder ve boş kayıt kümesiyle yanıt verir ve hata kodu yoktur.

[Adanmış kümelerin](event-hubs-dedicated-overview.md) azaltma mekanizmaları yoktur. Tüm küme kaynaklarınızı tüketebilirsiniz.

## <a name="no-records-received"></a>Kayıt alınmaz
Tüketicilerin herhangi bir kayıt almadığınızı ve sürekli olarak yeniden dengelemelerini görebilirsiniz. Bu senaryoda, tüketiciler herhangi bir kayıt alamadım ve sürekli yeniden dengeleme. Bu olduğunda bir istisna veya hata yoktur, ancak Kafka günlükleri tüketicilerin gruba yeniden katılmaya ve bölümler atamaya çalıştıklarını gösterir. Birkaç olası nedeni vardır:

- Sizin `request.timeout.ms` en az 60000 önerilen değeri ve `session.timeout.ms` en az 30000 önerilen değeri olduğundan emin olun. Bu ayarların çok düşük olması tüketici zaman ayarı neden olabilir, daha sonra yeniden dengelemeneden (daha sonra daha fazla zaman ayarı neden, daha fazla dengeleme neden, vb. 
- Yapılandırmanız bu önerilen değerlerle eşleşiyorsa ve sürekli yeniden dengeleme görmeye devam ediyorsanız, bir sorunu açmakta çekinmeyin (hata ayıklamanıza yardımcı olabileceğimiz için tüm yapılandırmanızı soruna ekleyin)

## <a name="compressionmessage-format-version-issue"></a>Sıkıştırma/İleti biçimi sürüm sorunu
Kafka sıkıştırmayı destekler ve Kafka için Etkinlik Hub'ları şu anda desteklemez. İleti biçiminde bir sürümden bahseden `The message format version on the broker does not support the request.`hatalar (örneğin, bir istemci aracılarımıza sıkıştırılmış Kafka mesajları göndermeye çalıştığında) neden olur.

Sıkıştırılmış veri gerekiyorsa, verilerinizi aracılara göndermeden önce sıkıştırmak ve aldıktan sonra sıkıştırmak geçerli bir geçici çözümdür. İleti gövdesi hizmete yalnızca bir bayt dizisidir, bu nedenle istemci tarafı sıkıştırma/dekompresyon herhangi bir soruna neden olmaz.

## <a name="unknownserverexception"></a>UnknownServerException
Aşağıdaki örneğe benzer Kafka istemci kitaplıklarından bir UnknownServerException alabilirsiniz: 

```
org.apache.kafka.common.errors.UnknownServerException: The server experienced an unexpected error when processing the request
```

Microsoft desteği yle bir bilet açın.  UTC'deki hata ayıklama düzeyinde günlük ve özel durum zaman damgaları, sorunun hata ayıklanmasında yararlıdır. 

## <a name="other-issues"></a>Diğer sorunlar
Etkinlik Hub'larında Kafka kullanırken sorunlar görürseniz aşağıdaki öğeleri kontrol edin.

- **Güvenlik duvarı trafiği engelleme** - **9093** bağlantı noktasının güvenlik duvarınız tarafından engellenmediğinden emin olun.
- **TopicAuthorizationException** - Bu özel durum en yaygın nedenleri şunlardır:
    - Yapılandırma dosyanızdaki bağlantı dizesinde bir yazım hatası veya
    - Temel katman ad alanında Kafka için Olay Hub'larını kullanmaya çalışıyorum. Kafka için Etkinlik Hub'ları [yalnızca Standart ve Özel katman ad alanları için desteklenir.](https://azure.microsoft.com/pricing/details/event-hubs/)
- **Kafka sürüm uyumsuzluğu** - Kafka Ecosystems için Etkinlik Hub'ları Kafka versiyonları 1.0 ve sonrası sürümlerini destekler. Kafka sürüm 0.10 ve daha sonra kafka protokolünün geriye dönük uyumluluğu nedeniyle zaman zaman işe yada bilen bazı uygulamalar, eski API sürümlerini kullanmamanızı şiddetle tavsiye ediyoruz. Kafka sürümleri 0.9 ve önceki sürümleri gerekli SASL protokollerini desteklemez ve Olay Hub'larına bağlanamaz.
- **Kafka ile tüketirken AMQP üstbilgilerinde garip kodlamalar** - amqp üzerinden bir olay hub'ına olay hub'ına gönderirken, amqp yük başlıkları AMQP kodlamasında seri hale getirilmiştir. Kafka tüketicileri üstbilgileri AMQP'dan deserialize etmezler. Üstbilgi değerlerini okumak için, AMQP üstbilgilerini el ile çöz. Alternatif olarak, Kafka protokolü ile tüketeceğinizi biliyorsanız, AMQP üstbilgilerini kullanmaktan kaçınabilirsiniz. Daha fazla bilgi için [bu GitHub sorununa](https://github.com/Azure/azure-event-hubs-for-kafka/issues/56)bakın.
- **SASL kimlik doğrulaması** - Çerçevenizin Event Hub'larının gerektirdiği SASL kimlik doğrulama protokolüyle işbirliği yapmak, göze çarptırılmaktan daha zor olabilir. SASL kimlik doğrulama üzerinde çerçevenizin kaynaklarını kullanarak yapılandırmayı giderip gidermediğinize bakın. 

## <a name="limits"></a>Sınırlar
Apache Kafka vs Olay Hubs Kafka. Çoğunlukla Kafka Ekosistemleri için Etkinlik Hub'ları, Apache Kafka'nın yaptığı gibi varsayılanlara, özelliklere, hata kodlarına ve genel davranışa sahiptir. Bu ikisinin açıkça farklı olduğu (veya Olay Hub'larının Kafka'nın uygulamadığı bir sınır uyguladığı) durumlar aşağıda listelenmiştir:

- Özelliğin `group.id` maksimum uzunluğu 256 karakterdir
- Maksimum boyutu `offset.metadata.max.bytes` 1024 bayt
- Ofset taahhütleri, maksimum dahili günlük boyutu 1 MB olan bölüm başına 4 arama/saniye ile daraltılır


## <a name="next-steps"></a>Sonraki adımlar
Kafka için Etkinlik Hub'ları ve Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu](apache-kafka-developer-guide.md)
- [Etkinlik Hub'ları için Apache Kafka geçiş kılavuzu](apache-kafka-migration-guide.md)
- [Sık sorulan sorular - Apache Kafka için Etkinlik Hub'ları](apache-kafka-frequently-asked-questions.md)
- [Önerilen yapılandırmalar](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)
