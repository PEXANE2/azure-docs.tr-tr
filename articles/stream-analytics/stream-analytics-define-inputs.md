---
title: Azure Stream Analytics giriş olarak Stream veri
description: Azure Stream analytics'te bir veri bağlantısı kurma bilgi edinin. Giriş olayları veri akışından içerir ve ayrıca verilere başvurur.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 388f43fec9242f6a4b448483d9486aa4413d2612
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79254475"
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Stream Analytics giriş olarak Stream veri

Stream Analytics, Azure veri akışları ile birinci sınıf tümleştirme giriş kaynakları üç tür olarak sahiptir:

- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, Stream Analytics işinizi aynı Azure aboneliğinde veya farklı bir abonelik Canlı çalıştırabilirsiniz.

### <a name="compression"></a>Sıkıştırma

Stream Analytics, tüm veri akışı giriş kaynaklarında sıkıştırma destekler. Desteklenen sıkıştırma türleri şunlardır: None, GZip ve söndür sıkıştırma. Sıkıştırma desteğine başvuru verileri için kullanılabilir değil. Giriş biçimi, sıkıştırılmış Avro veri olması durumunda saydam bir şekilde ele alınır. Avro serileştirme ile sıkıştırma türünü belirtmeniz gerekmez. 

## <a name="create-edit-or-test-inputs"></a>Oluşturma, düzenleme veya test girişleri

[Azure Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md)ve [Visual Studio Code](quick-create-vs-code.md) kullanarak akış işinizdeki mevcut girişleri ekleyebilir ve bunları görüntüleyebilir veya düzenleyebilirsiniz. Ayrıca, Azure portal, [Visual Studio](stream-analytics-vs-tools-local-run.md)ve [Visual Studio Code](visual-studio-code-local-run.md)örnek verilerden giriş bağlantılarını ve [Test sorgularını](stream-analytics-manage-job.md#test-your-query) test edebilirsiniz. Bir sorgu yazdığınızda FROM yan tümcesindeki girişi listeleyin. Portalda **sorgu** sayfasından kullanılabilir girişlerin listesini alabilirsiniz. Birden çok giriş kullanmak istiyorsanız, bunları `JOIN` veya birden çok `SELECT` sorgu yazabilirsiniz.


## <a name="stream-data-from-event-hubs"></a>Event Hubs’dan veri akışı sağlama

Azure Event Hubs yüksek oranda ölçeklenebilir sağlar yayımlama-abonelik olay ingestors. Bir olay hub 'ı saniyede milyonlarca olay toplayabilir, böylece bağlı cihazlarınız ve uygulamalarınız tarafından üretilen büyük miktarda veriyi işleyebilir ve analiz edebilirsiniz. Birlikte, Event Hubs ve Stream Analytics bir uçtan uca çözüm için gerçek zamanlı analizler sağlar. Olay hub'ları sağlar, olayları Azure'da içine akış gerçek zamanlı olarak ve Stream Analytics işleri, gerçek zamanlı olayları işleyebilir. Örneğin, Event Hubs'a web tıklama, sensör okumaları veya çevrimiçi günlüğü olaylarını gönderebilir. Ardından, Event Hubs, gerçek zamanlı filtreleyerek, toplayarak ve bağıntı için giriş veri akışları olarak kullanmak için Stream Analytics işleri de oluşturabilirsiniz.

`EventEnqueuedUtcTime` bir olay hub 'ında bir olayın varış zamanıdır ve Event Hubs gelen olayların zaman damgasıdır Stream Analytics. Verileri olay yükünde zaman damgası kullanarak bir akış olarak işlemek için anahtar sözcük [Ile zaman damgasını](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) kullanmanız gerekir.

### <a name="event-hubs-consumer-groups"></a>Event Hubs tüketici grupları

Kendi tüketici grubu için giriş her Stream Analytics olay hub'ı yapılandırmanız gerekir. Bir işin ne zaman kendi kendine birleşme içeriyor veya sahip birden fazla giriş, bazı girişler aşağı yönde birden fazla okuyucu tarafından okunmaması. Bu durum tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucular Event Hubs sınırını aşmamak için her bir Stream Analytics işine ilişkin bir tüketici grubu tanımlamak için en iyi uygulama olan. Ayrıca Standart katman Olay Hub 'ı için 20 Tüketici grubu sınırı vardır. Daha fazla bilgi için bkz. [Azure Stream Analytics girdileri sorun giderme](stream-analytics-troubleshoot-input.md).

### <a name="create-an-input-from-event-hubs"></a>Event Hubs bir giriş oluşturun

Aşağıdaki tabloda, bir olay hub 'ından veri girişi akışı için Azure portal **yeni giriş** sayfasında her bir özellik açıklanmaktadır:

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** |Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad. |
| **Abonelik** | Olay hub'ı kaynağını bulunduğu aboneliği seçin. | 
| **Olay Hub 'ı ad alanı** | Olay hub'ı ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, ad alanı oluşturabilir. |
| **Olay Hub 'ı adı** | Giriş olarak kullanmak için olay hub'ının adı. |
| **Olay Hub 'ı ilke adı** | Olay Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. Olay hub'ı ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek otomatik olarak, doldurulur el ile.|
| **Olay Hub 'ı Tüketici grubu** (önerilir) | Her bir Stream Analytics işi için ayrı bir tüketici grubu kullanmak için önerilir. Bu dize, olay hub'ından veri alma işlemini tüketici grubu tanımlar. Henüz hiç tüketici grubu belirtilirse, Stream Analytics işi $Default tüketici grubu kullanır.  |
| **Bölüm anahtarı** | Giriş bir özellik tarafından bölümlenmiş ise, bu özelliğin adını ekleyebilirsiniz. Bölüm anahtarları isteğe bağlıdır ve bu özellikte bir bölüm veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [diğer (prototip, XML, özel...)](custom-deserializer.md)).  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | UTF-8, şu anda desteklenen tek kodlama biçimi aşamasındadır. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Olay hub'ı stream girdi verilerinizi söz konusu olduğunda, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** |Stream Analytics tarafından işlendiğinde olay saat ve tarihi. |
| **EventEnqueuedUtcTime** |Olay, olay hub'ları tarafından alındı saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnekte olduğu gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Olay Hub 'ını IoT Hub yollar için uç nokta olarak kullanırken, [Getmetadatapropertyvalue işlevini](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue)kullanarak IoT Hub meta verilerine erişebilirsiniz.
> 

## <a name="stream-data-from-iot-hub"></a>IOT hub'ı Stream verileri

Azure IoT Hub, IoT senaryoları için iyileştirilmiş, yüksek düzeyde ölçeklenebilir bir yayınla-abone ol olay alma olayıdır.

Stream Analytics bir IoT Hub gelen olayların varsayılan zaman damgası, `EventEnqueuedUtcTime`olan IoT Hub olayın ulaştığı zaman damgasıdır. Verileri olay yükünde zaman damgası kullanarak bir akış olarak işlemek için anahtar sözcük [Ile zaman damgasını](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) kullanmanız gerekir.

### <a name="iot-hub-consumer-groups"></a>IoT Hub 'ı tüketici grupları

Her Stream Analytics IOT Hub'ı tüketici grubu için giriş yapılandırmanız gerekir. Bir işi kendi kendine birleşme içerdiğinde ya da birden fazla giriş varsa, bazı giriş aşağı yönde birden fazla okuyucu tarafından okunmaması. Bu durum tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyucular Azure IOT Hub sınırını aşmamak için her bir Stream Analytics işine ilişkin bir tüketici grubu tanımlamak için en iyi uygulama olan.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Giriş veri akışı IOT hub'ı yapılandırma

Aşağıdaki tabloda, bir IoT Hub akış girişi olarak yapılandırdığınızda Azure portal **yeni giriş** sayfasında her bir özellik açıklanmaktadır.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad.|
| **Abonelik** | IOT hub'ı kaynak bulunduğu aboneliği seçin. | 
| **IoT Hub’ı** | Giriş olarak kullanmak için IOT hub'ının adı. |
| **Bkz** | IOT hub'ının uç noktası.|
| **Paylaşılan erişim ilkesi adı** | IOT Hub'ına erişimi sağlayan paylaşılan erişim ilkesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| **Paylaşılan erişim ilkesi anahtarı** | IOT hub'ına erişim yetkisi vermek için kullanılan paylaşılan erişim anahtarı.  IOT hub'ının ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **Tüketici grubu** | Her bir Stream Analytics iş için farklı bir tüketici grubu kullanmanızı önemle tavsiye edilir. Tüketici grubu, IOT Hub'ından veri almak için kullanılır. Stream Analytics, aksi belirtilmedikçe $Default tüketici grubu kullanır.  |
| **Bölüm anahtarı** | Giriş bir özellik tarafından bölümlenmiş ise, bu özelliğin adını ekleyebilirsiniz. Bölüm anahtarları isteğe bağlıdır ve bu özellikte bir bölüm veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [diğer (prototip, XML, özel...)](custom-deserializer.md)).  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | UTF-8, şu anda desteklenen tek kodlama biçimi aşamasındadır. |
| **Olay sıkıştırma türü** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |


Bir IOT Hub'ından veri akışı kullandığınızda, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **EventProcessedUtcTime** | Olay işlendi saat ve tarihi. |
| **EventEnqueuedUtcTime** | IOT Hub tarafından olayı alındı saat ve tarihi. |
| **PartitionID** | Giriş bağdaştırıcısı sıfır bölüm kimliği. |
| **Iothub. MessageId** | IOT hub'ında iki yönlü iletişim ilişkilendirmek için kullanılan bir kimliği. |
| **Iothub. CorrelationId** | IOT hub'ında ileti yanıtlar ve kullanılan bir kimliği. |
| **Iothub. Connectiondeviceıd** | Bu ileti göndermek için kullanılan kimlik doğrulama kimliği. Bu değer servicebound iletileri IOT Hub tarafından damgalandı. |
| **Iothub. Connectiondevicegenerationıd** | Bu ileti göndermek için kullanılan kimliği doğrulanmış cihaz oluşturma kimliği. Bu değer servicebound iletileri IOT Hub tarafından damgalandı. |
| **Iothub. EnqueuedTime** | İleti IOT Hub tarafından ne zaman alındığı zamanı. |


## <a name="stream-data-from-blob-storage"></a>Blob depolama alanındaki verilerin Stream
Azure Blob Depolama, büyük miktarlarda yapılandırılmamış veriyi bulutta depolayabilir ile senaryoları için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob depolama alanındaki verilerin genellikle bekleyen veri olarak değerlendirilir. Ancak, blob verilerini bir veri akışı Stream Analytics tarafından işlenebilir. 

Günlük işleme, Stream Analytics ile Blob Depolama girişlerini kullanarak için yaygın olarak kullanılan bir senaryodur. Bu senaryoda, telemetri veri dosyaları bir sisteminden yakalanan ve ayrıştırılması ve anlamlı verileri ayıklamak için işlenen gerekir.

Stream Analytics blob Storage olaylarının varsayılan zaman damgası, blob 'un en son değiştirildiği zaman damgasıdır ve bu `BlobLastModifiedUtcTime`. Blob, 13:00 konumundaki bir depolama hesabına yüklenirse ve Azure Stream Analytics işi *Şu* anda 13:01 ' de seçeneği kullanılarak başlatılırsa, değiştirilen süre iş çalışma döneminin dışında kaldığında blob alınmaz.

Bir blob, 13:00 konumundaki bir depolama hesabı kapsayıcısına yüklenirse ve Azure Stream Analytics işi 13:00 veya daha önceki bir tarihte *özel süre* kullanılarak başlatılırsa, blob, değiştirilen süre iş çalışma döneminin içinde yer aldığından alınır.

Bir Azure Stream Analytics işi *Şu* anda 13:00 ' de kullanılarak başlatılırsa ve bir BLOB depolama hesabı kapsayıcısına 13:01 ' de yüklenirse, Azure Stream Analytics blobu seçer.

Verileri olay yükünde zaman damgası kullanarak bir akış olarak işlemek için anahtar sözcük [Ile zaman damgasını](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanmanız gerekir. Blob dosya varsa bir Stream Analytics işi saniyede Azure Blob Depolama giriş veri çeker. Blob dosya kullanılamıyorsa bir üstel geri alma bir gecikme süresiyle en fazla 90 saniyelik yoktur.

CSV biçimli girişler, veri kümesi alanlarını tanımlamak için bir başlık satırı gerektirir ve tüm üstbilgi satırı alanları benzersiz olmalıdır.

> [!NOTE]
> Stream Analytics, mevcut bir blob dosyasına ekleyerek içeriği desteklemiyor. Stream Analytics her dosyanın yalnızca bir kez görüntüleyeceği ve dosyayı iş verileri okuma sonra gerçekleşen değişikliklerin işlenmez. En iyi blob dosyası için tüm verileri tek seferde karşıya yükleme ve ardından ek yeni olaylar farklı, yeni blob dosyasını eklemektir.

Tek seferde çok fazla sayıda blob yüklemek Stream Analytics nadir durumlarda birkaç blob 'un okunmasını atlamasına neden olabilir. BLOB depolama alanına blob 'ları en az 2 saniye karşıya yüklemeniz önerilir. Bu seçenek uygun değilse, büyük hacimde olayları akışa almak için Event Hubs kullanabilirsiniz. 

### <a name="configure-blob-storage-as-a-stream-input"></a>Giriş akışı olarak BLOB depolamayı yapılandırma 

Aşağıdaki tabloda, blob depolamayı bir akış girişi olarak yapılandırdığınızda Azure portal **yeni giriş** sayfasında her bir özellik açıklanmaktadır.

| Özellik | Açıklama |
| --- | --- |
| **Giriş diğer adı** | Bu giriş başvurmak için işin sorgusunda kullandığınız kolay ad. |
| **Abonelik** | IOT hub'ı kaynak bulunduğu aboneliği seçin. | 
| **Depolama hesabı** | Blob dosyaların bulunduğu depolama hesabının adıdır. |
| **Depolama hesabı anahtarı** | Depolama hesabı ile ilişkili gizli anahtar. Blob Depolama alanı ayarlarını sağlama seçeneği seçmediğiniz sürece bu seçenek, otomatik olarak doldurulur el ile. |
| **Kapsayıcı** | Giriş blob kapsayıcısı. Kapsayıcıları Microsoft Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırmasını sağlar. İçin Azure Blob Depolama hizmetinin blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir. Yeni bir kapsayıcının oluşturulmasını sağlamak için **Mevcut kapsayıcıyı kullan** veya **Yeni oluştur** seçeneklerinden birini belirleyebilirsiniz.|
| **Yol kalıbı** (isteğe bağlı) | Belirtilen kapsayıcı içindeki blobları bulmak için kullanılan dosya yolu. Kapsayıcının kökünden blob 'ları okumak istiyorsanız, bir yol kalıbı ayarlamayın. Yol içinde, aşağıdaki üç değişkenin bir veya daha fazla örneğini belirtebilirsiniz: `{date}`, `{time}`veya `{partition}`<br/><br/>Örnek 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Örnek 2: `cluster1/logs/{date}`<br/><br/>`*` karakteri yol ön eki için izin verilen bir değer değil. Yalnızca geçerli <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure Blob karakterlerine</a> izin verilir. Kapsayıcı adlarını veya dosya adlarını eklemeyin. |
| **Tarih biçimi** (isteğe bağlı) | Yolda bir tarih değişkeniyle kullanırsanız, tarihi biçimlendirmek dosyaları düzenlenir. Örnek: `YYYY/MM/DD` |
| **Saat biçimi** (isteğe bağlı) |  Yolda zaman değişken kullanırsanız, biçiminde zamanı dosyaları düzenlenir. Şu anda desteklenen tek değer saat `HH`. |
| **Bölüm anahtarı** | Giriş bir özellik tarafından bölümlenmiş ise, bu özelliğin adını ekleyebilirsiniz. Bölüm anahtarları isteğe bağlıdır ve bu özellikte bir bölüm veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [diğer (prototip, XML, özel...)](custom-deserializer.md)).  JSON biçimini belirtimiyle uyumludur ve ondalık sayılar için önde gelen 0 içermez emin olun. |
| **Kodlama** | CSV ve JSON, UTF-8, şu anda desteklenen tek kodlama biçimi içindir. |
| **Masıyla** | Hiçbiri (varsayılan), GZip veya Deflate gibi gelen veri akışını okumak için kullanılan sıkıştırma türü. |

Verilerinizi bir Blob Depolama kaynaktan geldiğinde, aşağıdaki meta verileri alanları Stream Analytics sorgunuzda erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **BlobName** |Olay geldiği giriş blob adı. |
| **EventProcessedUtcTime** |Stream Analytics tarafından işlendiğinde olay saat ve tarihi. |
| **BlobLastModifiedUtcTime** |Blob, son değiştirilme zamanı saat ve tarihi. |
| **PartitionID** |Giriş bağdaştırıcısı sıfır bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnekte olduğu gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portal kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
