---
title: Azure Stream Analytics Service Bus kuyruk çıktısı
description: Bu makalede, Azure Stream Analytics için çıkış olarak Service Bus kuyrukları açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: aa9ec5454935816e733c1607e006ba1c30476cac
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88875896"
---
# <a name="service-bus-queues-output-from-azure-stream-analytics"></a>Azure Stream Analytics Service Bus kuyruk çıktısı

[Service Bus kuyruklar](../service-bus-messaging/service-bus-queues-topics-subscriptions.md) , bir veya daha fazla rakip TÜKETICIYE bir FIFO ileti teslimi sunar. Genellikle, iletiler kuyruğa eklendikleri zamana bağlı sırada alıcılar tarafından alınır ve işlenir. Her ileti yalnızca bir ileti tüketicisi tarafından alınır ve işlenir.

[Uyumluluk düzeyi 1,2](stream-analytics-compatibility-level.md)' de Azure Stream Analytics, Service Bus kuyruklara ve konulara yazmak Için [Gelişmiş ileti sırası Protokolü (AMQP)](../service-bus-messaging/service-bus-amqp-overview.md) mesajlaşma protokolünü kullanır. AMQP, açık bir standart protokol kullanarak platformlar arası karma uygulamalar oluşturmanıza olanak sağlar.

## <a name="output-configuration"></a>Çıkış yapılandırması

Aşağıdaki tabloda, bir kuyruk çıkışı oluşturmaya yönelik özellik adları ve açıklamaları listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıkışını bu Service Bus kuyruğuna yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Service Bus ad alanı |Bir mesajlaşma varlıkları kümesi için kapsayıcı. |
| Kuyruk adı |Service Bus kuyruğunun adı. |
| Kuyruk ilkesi adı |Bir kuyruk oluşturduğunuzda, sıranın **Yapılandır** sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin adı, sizin ayarladığınız izinler ve anahtarlara erişim vardır. |
| Kuyruk İlkesi anahtarı |Service Bus ad alanına erişimin kimliğini doğrulamak için kullanılan paylaşılan erişim anahtarı. |
| Olay serileştirme biçimi |Çıkış verileri için serileştirme biçimi. JSON, CSV ve avro desteklenir. |
| Encoding |CSV ve JSON için, UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı |Yalnızca CSV serileştirme için geçerlidir. Stream Analytics CSV biçimindeki verileri serileştirmek için bir dizi ortak sınırlayıcıları destekler. Desteklenen değerler virgül, noktalı virgül, boşluk, sekme ve dikey çubuklardır. |
| Biçimlendir |Yalnızca JSON türü için geçerlidir. **Satır ayrımı** , çıktının her bir JSON nesnesine yeni bir satırla ayrılmış şekilde biçimlendirildiğini belirtir. **Satır ayrılmış**' i SEÇERSENIZ, JSON tek seferde bir nesne okur. Tüm içerik geçerli bir JSON değil. **Dizi** , çıktının JSON nesneleri dizisi olarak biçimlendirildiğini belirtir. |
| Özellik sütunları | İsteğe bağlı. Yük yerine giden iletinin Kullanıcı özellikleri olarak eklenmesi gereken virgülle ayrılmış sütunlar. Bu özellik hakkında daha fazla bilgi, [Çıkış Için özel meta veri özellikleri](#custom-metadata-properties-for-output)bölümünde bulunur. |
| Sistem özelliği sütunları | İsteğe bağlı. Sistem özelliklerinin anahtar değer çiftleri ve yük yerine giden iletiye eklenmesi gereken karşılık gelen sütun adları.  |

Bölüm sayısı [SERVICE Bus SKU 'sunu ve boyutunu temel alır](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir.

## <a name="partitioning"></a>Bölümleme

Bölümlendirme otomatik olarak seçilir. Bölüm sayısı [SERVICE Bus SKU 'sunu ve boyutunu](../service-bus-messaging/service-bus-partitioning.md)temel alır. Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir. Çıktı yazıcılarının sayısı, çıkış sırasındaki bölüm sayısıyla aynıdır.

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

En büyük ileti boyutu, Standart katman için ileti başına 256 KB ve Premium katman için 1 MB 'tır. Daha fazla bilgi için bkz. [Service Bus sınırları](../service-bus-messaging/service-bus-quotas.md). İyileştirmek için ileti başına tek bir olay kullanın.

## <a name="custom-metadata-properties-for-output"></a>Çıkış için özel meta veri özellikleri

Sorgu sütunlarını, giden iletilerinize Kullanıcı özellikleri olarak ekleyebilirsiniz. Bu sütunlar yük içine gitmez. Özellikler, çıkış iletisindeki bir sözlük biçiminde bulunur. *Anahtar* , sütun adı ve *değer* Özellikler sözlüğündeki sütun değeridir. Tüm Stream Analytics veri türleri, kayıt ve dizi dışında desteklenir.

## <a name="system-properties"></a>Sistem özellikleri

Sorgu sütunlarını, giden hizmet veri yolu kuyruğunuza veya konu iletilerinize [Sistem Özellikleri](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) olarak iliştirebilirsiniz.

Bu sütunlar, karşılık gelen BrokeredMessage [sistem özelliği](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) sorgu sütunu değerleriyle doldurulduğundan yük içine gitmez.
Bu sistem özellikleri desteklenir- `MessageId, ContentType, Label, PartitionKey, ReplyTo, SessionId, CorrelationId, To, ForcePersistence, TimeToLive, ScheduledEnqueueTimeUtc` .

Bu sütunların dize değerleri karşılık gelen sistem özelliği değer türü olarak ayrıştırılır ve Ayrıştırma hataları veri hatası olarak değerlendirilir.
Bu alan JSON nesne biçimi olarak sağlanır. Bu biçimle ilgili ayrıntılar aşağıdaki gibidir:

* Küme ayraçları ile çevrelenmiş {} .
* Anahtar/değer çiftlerinde yazılmıştır.
* Anahtarlar ve değerler dize olmalıdır.
* Anahtar, sistem özelliği adıdır ve değer sorgu sütunu adıdır.
* Anahtarlar ve değerler iki nokta üst üste ile ayrılır.
* Her anahtar/değer çifti virgülle ayrılır.

Bu özelliğin nasıl kullanılacağını gösterir –

* Sorgulayamadı `select *, column1, column2 INTO queueOutput FROM iotHubInput`
* Sistem özelliği sütunları: `{ "MessageId": "column1", "PartitionKey": "column2"}`

Bu, `MessageId` Service Bus kuyruğu iletilerinin `column1` değerlerini ve partitionkey değerini ile ayarlanmış olarak ayarlar `column2` .

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
* [Hızlı başlangıç: Azure CLı kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-cli.md)
* [Hızlı başlangıç: ARM şablonu kullanarak Azure Stream Analytics işi oluşturma](quick-create-azure-resource-manager.md)
* [Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics iş oluşturma](quick-create-vs-code.md)
