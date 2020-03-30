---
title: Verileri Azure Akış Analitiği'ne girdi olarak aktarın
description: Azure Akış Analizi'nde veri bağlantısı kurma hakkında bilgi edinin. Girişler, olaylardan bir veri akışı ve ayrıca referans verileri içerir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 388f43fec9242f6a4b448483d9486aa4413d2612
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254475"
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Verileri Akış Analizi'ne girdi olarak aktarın

Akış Analizi, üç tür kaynaktan gelen girdiler olarak Azure veri akışlarıyla birinci sınıf tümleştirmeye sahiptir:

- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/) 

Bu giriş kaynakları, Akış Analizi işinizle veya farklı bir abonelikle aynı Azure aboneliğinde yaşayabilir.

### <a name="compression"></a>Sıkıştırma

Akış Analizi, tüm veri akışı giriş kaynaklarında sıkıştırmayı destekler. Desteklenen sıkıştırma türleri şunlardır: Yok, GZip ve Deflate sıkıştırma. Başvuru verileri için sıkıştırma desteği kullanılamıyor. Giriş biçimi sıkıştırılmış Avro verileriyse, saydam olarak işlenir. Avro serileştirme ile sıkıştırma türünü belirtmeniz gerekmez. 

## <a name="create-edit-or-test-inputs"></a>Giriş oluşturma, yeniden oluşturma veya test

Akış işinizdeki varolan girişleri eklemek ve görüntülemek veya bunları yeniden oluşturmak için [Azure portalını,](stream-analytics-quick-create-portal.md) [Visual Studio](stream-analytics-quick-create-vs.md)ve Visual [Studio Kodunu](quick-create-vs-code.md) kullanabilirsiniz. Ayrıca Azure portalı, [Visual Studio](stream-analytics-vs-tools-local-run.md)ve [Visual Studio Kodu'ndan](visual-studio-code-local-run.md)örnek verilerden giriş bağlantılarını ve [sorguları test](stream-analytics-manage-job.md#test-your-query) edebilirsiniz. Bir sorgu yazdığınızda, FROM yan tümcesindeki girişi listelersiniz. Kullanılabilir girişlerin listesini portaldaki **Sorgu** sayfasından alabilirsiniz. Birden çok giriş kullanmak isterseniz, `JOIN` bunları yapabilir `SELECT` veya birden çok sorgu yazabilirsiniz.


## <a name="stream-data-from-event-hubs"></a>Event Hubs’dan veri akışı sağlama

Azure Etkinlik Hub'ları, yüksek oranda ölçeklenebilir yayımlama-abone olay sindiricileri sağlar. Bir olay hub'ı, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen büyük miktardaver'i işleyip analiz edebilmeniz için saniyede milyonlarca olay toplayabilir. Etkinlik Hub'ları ve Akış Analizi birlikte gerçek zamanlı analizler için uçtan uca bir çözüm sağlar. Etkinlik Hub'ları etkinlikleri Azure'da gerçek zamanlı olarak beslemenize olanak tanır ve Akış Analizi işleri bu etkinlikleri gerçek zamanlı olarak işleyebilir. Örneğin, Web tıklamaları, sensör okumaları veya çevrimiçi günlük etkinliklerini Olay Hub'larına gönderebilirsiniz. Daha sonra, gerçek zamanlı filtreleme, toplama ve korelasyon için giriş veri akışları olarak Olay Hub'larını kullanmak için Akış Analizi işleri oluşturabilirsiniz.

`EventEnqueuedUtcTime`Bir etkinliğin bir etkinlik hub'ına gelişinin zaman damgasıdır ve Olay Hub'larından Stream Analytics'e gelen olayların varsayılan zaman damgasi. Olay yükünde bir zaman damgası kullanarak verileri akış olarak işlemek için [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) anahtar sözcük lerini kullanmanız gerekir.

### <a name="event-hubs-consumer-groups"></a>Etkinlik Hub'ları Tüketici grupları

Her Akış Analizi etkinlik merkezi girdisini kendi tüketici grubuna sahip olacak şekilde yapılandırmanız gerekir. Bir iş kendi kendine birleştirme içeriyorsa veya birden çok girişi varsa, bazı girişler akış aşağı birden fazla okuyucu tarafından okunabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyuculuk Olay Hub'ları sınırını aşmamak için, her Akış Analizi işi için bir tüketici grubu belirlemek en iyi yöntemdir. Standart katman etkinlik merkezi için 20 tüketici grubu sınırı da vardır. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Akış Analizi girişleri.](stream-analytics-troubleshoot-input.md)

### <a name="create-an-input-from-event-hubs"></a>Olay Hub'larından giriş oluşturma

Aşağıdaki tablo, bir olay merkezinden veri girişi akışı sağlamak için Azure portalındaki **Yeni giriş** sayfasındaki her özelliği açıklar:

| Özellik | Açıklama |
| --- | --- |
| **Girdi diğer adı** |Bu girişe başvurmak için işin sorgusunda kullandığınız kolay bir ad. |
| **Abonelik** | Etkinlik hub kaynağının bulunduğu aboneliği seçin. | 
| **Olay Hub ad alanı** | Olay Hub ad alanı, ileti varlıkları kümesi için bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, ad alanını da oluşturursunuz. |
| **Olay Hub'ı adı** | Giriş olarak kullanılacak olay hub'ının adı. |
| **Olay Hub'ı ilke adı** | Olay Hub'ına erişim sağlayan paylaşılan erişim ilkesi. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. Olay Hub ayarlarını el ile sağlama seçeneğini seçmediğiniz sürece, bu seçenek otomatik olarak doldurulur.|
| **Olay Hub tüketici grubu** (önerilir) | Her Stream Analytics işi için ayrı bir tüketici grubu kullanılması önerilir. Bu dize, olay merkezinden veri almak için kullanılacak tüketici grubunu tanımlar. Tüketici grubu belirtilmemişse, Stream Analytics işi $Default tüketici grubunu kullanır.  |
| **Bölüm anahtarı** | Girdiniz bir özellik tarafından bölümlenmişse, bu özelliğin adını ekleyebilirsiniz. Bölüm tuşları isteğe bağlıdır ve bu özellik üzerinde bir PARTITION BY veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [Diğer (Protobuf, XML, tescilli...) ).](custom-deserializer.md)  JSON biçiminin belirtimle hizalandığından ve ondalık sayılar için satır aralığı 0 içermediğinden emin olun. |
| **Encoding** | UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| **Olay sıkıştırma türü** | Gelen veri akışını okumak için kullanılan None (varsayılan), GZip veya Deflate gibi sıkıştırma türü. |

Verileriniz bir Event Hub akışı girişinden geldiğinde, Akış Analizi sorgunuzda aşağıdaki meta veri alanlarına erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **OlayProsesUtcTime** |Etkinliğin Stream Analytics tarafından işlendiği tarih ve saat. |
| **EventEnqueuedUtcTime** |Etkinliğin Olay Hub'ları tarafından alındığı tarih ve saat. |
| **Partitionıd** |Giriş bağdaştırıcısı için sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnek gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> IoT Hub Yolları için bir bitiş noktası olarak Olay Hub'ını kullanırken, [GetMetadataPropertyValue işlevini](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue)kullanarak IoT Hub meta verilerine erişebilirsiniz.
> 

## <a name="stream-data-from-iot-hub"></a>IoT Hub'ından veri akışı

Azure IoT Hub, IoT senaryoları için optimize edilmiş yüksek ölçeklenebilir bir yayımlama-abone olay yutulur.

Stream Analytics'teki bir IoT Hub'ından gelen olayların varsayılan zaman damgası, etkinliğin IoT `EventEnqueuedUtcTime`Hub'ına geldiği zaman damgasIdır. Olay yükünde bir zaman damgası kullanarak verileri akış olarak işlemek için [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) anahtar sözcük lerini kullanmanız gerekir.

### <a name="iot-hub-consumer-groups"></a>Iot Hub Tüketici grupları

Her Akış Analizi IoT Hub girdisini kendi tüketici grubuna sahip olacak şekilde yapılandırmanız gerekir. Bir iş kendi kendine birleşin veya birden çok girişi olduğunda, bazı girdiler birden fazla okuyucu tarafından okunabilir. Bu durum, tek bir tüketici grubundaki okuyucu sayısını etkiler. Bölüm başına tüketici grubu başına beş okuyuculuk Azure IoT Hub sınırını aşmamak için, her Akış Analizi işi için bir tüketici grubu belirlemek en iyi yöntemdir.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Veri akışı girişi olarak bir IoT Hub'ını yapılandırma

Aşağıdaki tablo, bir IoT Hub'ını akış girişi olarak yapılandırırken Azure portalındaki **Yeni giriş** sayfasındaki her özelliği açıklar.

| Özellik | Açıklama |
| --- | --- |
| **Girdi diğer adı** | Bu girişe başvurmak için işin sorgusunda kullandığınız kolay bir ad.|
| **Abonelik** | IoT Hub kaynağının bulunduğu aboneliği seçin. | 
| **IoT Hub** | Giriş olarak kullanılacak IoT Hub'ının adı. |
| **Uç Nokta** | IoT Hub için bitiş noktası.|
| **Paylaşılan erişim ilkesi adı** | IoT Hub'ına erişim sağlayan paylaşılan erişim ilkesi. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. |
| **Paylaşılan erişim ilkesi anahtarı** | IoT Hub'ına erişimi yetkilendirmek için kullanılan paylaşılan erişim anahtarı.  Iot Hub ayarlarını el ile sağlama seçeneğini seçmediğiniz sürece bu seçenek otomatik olarak doldurulur. |
| **Tüketici grubu** | Her Stream Analytics işi için farklı bir tüketici grubu kullanmanız önerilir. Tüketici grubu, IoT Hub'ından veri almak için kullanılır. Akış Analizi, aksini belirtmediğiniz sürece $Default tüketici grubunu kullanır.  |
| **Bölüm anahtarı** | Girdiniz bir özellik tarafından bölümlenmişse, bu özelliğin adını ekleyebilirsiniz. Bölüm tuşları isteğe bağlıdır ve bu özellik üzerinde bir PARTITION BY veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [Diğer (Protobuf, XML, tescilli...) ).](custom-deserializer.md)  JSON biçiminin belirtimle hizalandığından ve ondalık sayılar için satır aralığı 0 içermediğinden emin olun. |
| **Encoding** | UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| **Olay sıkıştırma türü** | Gelen veri akışını okumak için kullanılan None (varsayılan), GZip veya Deflate gibi sıkıştırma türü. |


Bir IoT Hub'ından akış verilerini kullandığınızda, Akış Analizi sorgunuzda aşağıdaki meta veri alanlarına erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **OlayProsesUtcTime** | Olayın işlendiği tarih ve saat. |
| **EventEnqueuedUtcTime** | Etkinliğin IoT Hub tarafından alındığı tarih ve saat. |
| **Partitionıd** | Giriş bağdaştırıcısı için sıfır tabanlı bölüm kimliği. |
| **IoTHub.MessageId** | IoT Hub'da çift yönlü iletişimi ilişkilendirmek için kullanılan bir kimlik. |
| **IoTHub.CorrelationId** | IoT Hub'da ileti yanıtlarında ve geri bildirimde kullanılan bir kimlik. |
| **IoTHub.ConnectionDeviceId** | Bu iletiyi göndermek için kullanılan kimlik doğrulama kimliği. Bu değer, IoT Hub tarafından servise bağlı iletilere damgalanır. |
| **IoTHub.ConnectionDeviceGenerationId** | Bu iletiyi göndermek için kullanılan kimlik doğrulamalı aygıtın nesil kimliği. Bu değer, IoT Hub tarafından servise bağlı iletilere damgalanır. |
| **IoTHub.EnqueuedTime** | İletinin IoT Hub tarafından alındığı saat. |


## <a name="stream-data-from-blob-storage"></a>Blob depolamadan veri akışı
Azure Blob depolama, bulutta depolanması gereken büyük miktarlarda yapılandırılmamış veri içeren senaryolar için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob depolamadaki veriler genellikle istirahatte veri olarak kabul edilir; ancak, blob verileri Stream Analytics tarafından veri akışı olarak işlenebilir. 

Günlük işleme, Stream Analytics ile Blob depolama girişlerini kullanmak için yaygın olarak kullanılan bir senaryodur. Bu senaryoda, telemetri veri dosyaları bir sistemden yakalanmış ve anlamlı veri ayıklamak için ayrışdırılmış ve işlenmesi gerekir.

Stream Analytics'teki Blob depolama olaylarının varsayılan zaman damgası, blob'un `BlobLastModifiedUtcTime`en son değiştirildiği zaman damgasI'dır. Bir blob saat 13:00'te bir depolama hesabına yüklenirse ve Azure Akışı Analytics işi *Şimdi* 13:01 seçeneğini kullanmaya başlarsa, değiştirilen süre iş çalıştırma süresinin dışına düştüğünden, blob alınmaz.

Bir blob saat 13:00'te bir depolama hesabı kapsayıcısına yüklenirse ve Azure Akışı Analytics işi 13:00 veya daha erken saatlerde *Özel Saat* kullanılarak başlatılırsa, değiştirilen süre iş çalıştırma süresine denk geldiğinde niçin blob alınır.

Bir Azure Akışı Analytics işi saat 13:00'te *Şimdi'i* kullanmaya başlarsa ve saat 13:01'de depolama hesabı konteynerine bir blob yüklenirse, Azure Akış Analytics blob'u alır.

Olay yükünde bir zaman damgası kullanarak verileri akış olarak işlemek için [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) anahtar sözcük lerini kullanmanız gerekir. Bir Akış Analizi işi, blob dosyası varsa, Azure Blob depolama girişinden her saniye veri çeker. Blob dosyası kullanılamıyorsa, en fazla 90 saniye lik bir gecikmeyle üstel bir geri dönüş vardır.

CSV biçimli girişler, veri kümesi için alanları tanımlamak için bir üstbilgi satırı gerektirir ve tüm üstbilgi satır alanları benzersiz olmalıdır.

> [!NOTE]
> Akış Analizi, varolan bir blob dosyasına içerik eklemeyi desteklemez. Akış Analizi her dosyayı yalnızca bir kez görüntületir ve iş verileri okuduktan sonra dosyada meydana gelen değişiklikler işlenmez. En iyi yöntem, bir blob dosyası için tüm verileri aynı anda yüklemek ve sonra farklı, yeni bir blob dosyasına ek yeni olaylar eklemektir.

Aynı anda çok sayıda blob yüklemek, Stream Analytics'in nadir durumlarda birkaç blob okumayı atlamasına neden olabilir. Blob depolama alanına en az 2 saniye arayla blob yüklemesi önerilir. Bu seçenek uygun değilse, büyük hacimli olayları akışiçin Olay Hub'larını kullanabilirsiniz. 

### <a name="configure-blob-storage-as-a-stream-input"></a>Blob depolamayı akış girişi olarak yapılandırma 

Aşağıdaki tablo, Blob depolama alanını akış girişi olarak yapılandırdığınızda Azure portalındaki **Yeni giriş** sayfasındaki her özelliği açıklar.

| Özellik | Açıklama |
| --- | --- |
| **Girdi diğer adı** | Bu girişe başvurmak için işin sorgusunda kullandığınız kolay bir ad. |
| **Abonelik** | IoT Hub kaynağının bulunduğu aboneliği seçin. | 
| **Depolama hesabı** | Blob dosyalarının bulunduğu depolama hesabının adı. |
| **Depolama hesabı anahtarı** | Depolama hesabıyla ilişkili gizli anahtar. Blob depolama ayarlarını el ile sağlama seçeneğini seçmediğiniz sürece bu seçenek otomatik olarak doldurulur. |
| **Kapsayıcı** | Blob girişi için konteyner. Kapsayıcılar, Microsoft Azure Blob hizmetinde depolanan blob'lar için mantıksal bir gruplandırma sağlar. Azure Blob depolama hizmetine bir blob yüklediğinizde, bu blob için bir kapsayıcı belirtmeniz gerekir. Yeni bir kapsayıcı oluşturmak için **varolan** kapsayıcıyı kullan veya **yeni oluştur'u** seçebilirsiniz.|
| **Yol deseni** (isteğe bağlı) | Belirtilen kapsayıcı içindeki lekeleri bulmak için kullanılan dosya yolu. Kapsayıcının kökünden lekeleri okumak istiyorsanız, bir yol deseni ayarlamayın. Yol içinde, aşağıdaki üç değişkenin bir veya daha fazla `{date}` `{time}`örneğini belirtebilirsiniz: , veya`{partition}`<br/><br/>Örnek 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Örnek 2:`cluster1/logs/{date}`<br/><br/>Karakter `*` yol öneki için izin verilen bir değer değildir. Yalnızca geçerli <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob karakterlerine</a> izin verilir. Kapsayıcı adlarını veya dosya adlarını eklemeyin. |
| **Tarih biçimi** (isteğe bağlı) | Yoldaki tarih değişkenini kullanırsanız, dosyaların düzenlendiği tarih biçimi. Örnek: `YYYY/MM/DD` |
| **Zaman biçimi** (isteğe bağlı) |  Yoldaki zaman değişkenini kullanırsanız, dosyaların düzenlendiği zaman biçimi. Şu anda desteklenen `HH` tek değer saatler içindir. |
| **Bölüm anahtarı** | Girdiniz bir özellik tarafından bölümlenmişse, bu özelliğin adını ekleyebilirsiniz. Bölüm tuşları isteğe bağlıdır ve bu özellik üzerinde bir PARTITION BY veya GROUP BY yan tümcesi içeriyorsa sorgunuzun performansını artırmak için kullanılır. |
| **Olay serileştirme biçimi** | Gelen veri akışının serileştirme biçimi (JSON, CSV, Avro veya [Diğer (Protobuf, XML, tescilli...) ).](custom-deserializer.md)  JSON biçiminin belirtimle hizalandığından ve ondalık sayılar için satır aralığı 0 içermediğinden emin olun. |
| **Encoding** | CSV ve JSON için UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| **Sıkıştırma** | Gelen veri akışını okumak için kullanılan None (varsayılan), GZip veya Deflate gibi sıkıştırma türü. |

Verileriniz bir Blob depolama kaynağından geldiğinde, Akış Analizi sorgunuzda aşağıdaki meta veri alanlarına erişebilirsiniz:

| Özellik | Açıklama |
| --- | --- |
| **BlobName** |Olayın geldiği giriş blobunun adı. |
| **OlayProsesUtcTime** |Etkinliğin Stream Analytics tarafından işlendiği tarih ve saat. |
| **BlobLastModiModiUtcTime** |Lekenin en son değiştirilme tarihi ve saati. |
| **Partitionıd** |Giriş bağdaştırıcısı için sıfır tabanlı bölüm kimliği. |

Örneğin, bu alanları kullanarak, aşağıdaki örnek gibi bir sorgu yazabilirsiniz:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
