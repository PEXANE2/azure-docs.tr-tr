---
title: Önizlemede veri depolama ve giriş-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizlemede veri depolama ve giriş hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/27/2020
ms.custom: seodec18
ms.openlocfilehash: d3bfb589ec4c152b136e8e1f432864b719c97d58
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85509328"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri depolama ve giriş

Bu makalede Azure Time Series Insights önizlemesi için veri depolama ve giriş güncelleştirmeleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve zaman serisi KIMLIĞI özelliğini açıklar. Temel alınan giriş işlemi, en iyi uygulamalar ve geçerli önizleme sınırlamaları da açıklanır.

## <a name="data-ingress"></a>Veri girişi

Azure Time Series Insights ortamınız, zaman serisi verilerini toplamak, işlemek ve depolamak için bir giriş *altyapısı* içerir.

Tüm gelen verilerin işlenmesini sağlamak, yüksek giriş ölçeğine ulaşmak ve Alım *gecikmesini* en aza indirmek (Time Series Insights tarafından gerçekleştirilen süre, olay kaynağından verileri okumak ve işlemek için gereken süre), [ortamınızı planlarken](time-series-insights-update-plan.md)göz önünde bulundurmanız gereken bazı önemli noktalar vardır.

Time Series Insights Önizleme verileri giriş ilkeleri, verilerin nereden kaynaklanacağı ve verilerin ne biçimde olması gerektiğini tespit edebilir.

### <a name="ingress-policies"></a>Giriş ilkeleri

*Veri* girişi, verilerin Azure Time Series Insights bir önizleme ortamına nasıl gönderileceğini içerir.

Anahtar yapılandırma, biçimlendirme ve en iyi uygulamalar aşağıda özetlenmiştir.

#### <a name="event-sources"></a>Olay Kaynakları

Azure Time Series Insights önizlemesi aşağıdaki olay kaynaklarını destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Azure Time Series Insights önizlemesi, örnek başına en fazla iki olay kaynağını destekler. Bir olay kaynağını bağladığınızda, TSI ortamınız, en eski olayla başlayarak IoT veya Olay Hub 'ınızda depolanmış olan tüm olayları okur.

> [!IMPORTANT]
>
> * Önizleme ortamınıza bir olay kaynağı eklerken yüksek gecikme süresi yaşayabilirsiniz.
> Olay kaynağı gecikmesi, IoT Hub veya Olay Hub 'ınızdaki olay sayısına bağlıdır.
> * Olay kaynak verilerinin ilk kez alındıktan sonra yüksek gecikme süresi alt tarafı olur. Devam eden yüksek gecikme süresi yaşarsanız Azure portal aracılığıyla bir destek bileti gönderebilirsiniz.

#### <a name="supported-data-format-and-types"></a>Desteklenen veri biçimi ve türleri

Azure Time Series Insights, Azure IoT Hub veya Azure Event Hubs 'den gönderilen UTF-8 kodlu JSON 'yi destekler. 

Desteklenen veri türleri şunlardır:

| Veri türü | Açıklama |
|---|---|
| **bool** | İki durumdan birine sahip bir veri türü: `true` veya `false` . |
| **Hem** | Genellikle günün tarih ve saati olarak ifade edilen bir anlık zamanı temsil eder. [Iso 8601](https://www.iso.org/iso-8601-date-and-time-format.html) biçiminde ifade edilir. |
| **long** | İmzalı 64 bitlik bir tamsayı  |
| **double** | Çift duyarlıklı 64 bitlik [ıeee 754](https://ieeexplore.ieee.org/document/8766229) kayan nokta. |
| **dizisinde** | Unicode karakterlerinden oluşan metin değerleri.          |

> [!IMPORTANT]
>
> * TSI ortamınız kesin olarak belirlenmiş. Cihazlar veya Etiketler hem integral hem de tamsayı olmayan veriler gönderse, cihaz özelliği değerleri iki ayrı çift ve uzun sütunlarda depolanır ve API çağrıları yapılırken ve zaman serisi model değişkeni ifadelerinizi tanımlarken [birleşim () işlevi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) kullanılmalıdır.

#### <a name="objects-and-arrays"></a>Nesneler ve diziler

Olay yüklerinizin bir parçası olarak nesneler ve diziler gibi karmaşık türler gönderebilirsiniz, ancak verileriniz depolandığında bir düzleştirme işlemini üstlerdir.

JSON olaylarınızın nasıl şekillendirilmesi, karmaşık tür gönderme ve iç içe nesne düzleştirme ile ilgili ayrıntılı bilgiler, planlama ve iyileştirmede yardımcı olmak üzere giriş [ve sorgu IÇIN JSON nasıl şekillendirilebileceğinize](./time-series-insights-update-how-to-shape-events.md) sunulmaktadır.

### <a name="ingress-best-practices"></a>Giriş en iyi uygulamaları

Aşağıdaki en iyi yöntemleri kullanmanızı öneririz:

* Olası gecikmeyi azaltmak için aynı bölgedeki Azure Time Series Insights ve IoT Hub ya da Olay Hub 'ını yapılandırın.

* Beklenen alma hızınızı hesaplayarak ve aşağıda listelenen desteklenen oran dahilinde olduğunu doğrulayarak [Ölçek gereksinimlerinizi planlayın](time-series-insights-update-plan.md) .

* Giriş [ve sorgu IÇIN JSON şeklini](./time-series-insights-update-how-to-shape-events.md)okuyarak, JSON verilerinizin ve önizleme aşamasında geçerli sınırlamaların nasıl iyileştirileceği ve şekillendirilmeli olduğunu anlayın.

* Yalnızca neredeyse gerçek zamanlı ve en son veriler için akış alımı kullanın, geçmiş verileri akışa alma desteklenmez.

#### <a name="historical-data-ingestion"></a>Geçmiş veri alımı

Geçmiş verileri içeri aktarmak için akış işlem hattının kullanılması şu anda Azure Time Series Insights önizlemede desteklenmiyor. Eski verileri ortamınıza aktarmanız gerekiyorsa aşağıdaki yönergeleri izleyin:

* Canlı ve geçmiş verileri paralel olarak akışmayın. Verilerin dışına inmek, sorgu performansının düşmesine neden olur.
* En iyi performansı elde etmek için geçmiş verileri zaman düzenli olarak alma.
* Aşağıdaki alma işlemi hız sınırları dahilinde kalır.
* Veriler, sıcak mağaza saklama süresinden daha eskiyse, sıcak mağazayı devre dışı bırakın.

### <a name="ingress-scale-and-preview-limitations"></a>Giriş ölçeği ve önizleme sınırlamaları

Azure Time Series Insights önizleme giriş sınırlamaları aşağıda açıklanmıştır.

> [!TIP]
> Önizleme [ortamınızı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits) , tüm önizleme limitlerinin kapsamlı bir listesi için planlayın.

#### <a name="per-environment-limitations"></a>Ortam başına sınırlamalar

Genel olarak giriş fiyatları, kuruluşunuzda bulunan cihazların sayısı, olay egörev sıklığı ve her olayın boyutu olarak görüntülenir:

*  **Cihazların sayısı** × **olay emisi sıklığı** **her olayın boyutu**.

Time Series Insights önizlemesi, varsayılan olarak, **Time Series Insights ortamları başına 1 megabaylarca (Mbps)** bir hızda gelen verileri alabilir. [Hub bölümü başına](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#hub-partitions-and-per-partition-limits)ek sınırlamalar vardır.

> [!TIP]
>
> * 16 MBps 'e kadar hızlara kadar olan ortam desteği istek tarafından sağlanarak temin edilebilir.
> * Azure portal aracılığıyla bir destek bileti göndererek daha yüksek aktarım hızı gerekiyorsa bizimle iletişim kurun.
 
* **Örnek 1:**

    Contoso Shipping, dakikada üç kez bir olay sunan 100.000 cihaza sahiptir. Bir olayın boyutu 200 bayttır. Time Series Insights olay kaynağı olarak dört bölümden oluşan bir IoT Hub kullanıyor.

    * Time Series Insights ortamları için alım oranı: **100.000 cihaz * 200 bayt/olay * (3/60 olay/sn) = 1 MB**/sn olmalıdır.
    * Bölüm başına alım oranı 0,25 MBps olur.
    * Contoso sevkiyat alım oranı, önizleme ölçeği kısıtlaması dahilinde olacaktır.

* **Örnek 2:**

    Contoso Fleet Analytics, her saniye bir olay sunan 60.000 cihaza sahiptir. Time Series Insights olay kaynağı olarak 4 bölüm sayısıyla bir olay hub 'ı kullanıyor. Bir olayın boyutu 200 bayttır.

    * Ortam alma hızı şu şekilde olacaktır: **60.000 cihaz * 200 bayt/olay * 1 olay/sn = 12 Mbps**.
    * Bölüm başına hız 3 MBps olur.
    * Contoso Fleet Analizi ' alım oranı, ortam ve bölüm sınırlarının üzerinde. Time Series Insights, ortamları için alma hızını artırmak üzere Azure portal aracılığıyla bir istek gönderebilirler ve önizleme sınırları dahilinde daha fazla bölüme sahip bir olay hub 'ı oluşturabilir.

#### <a name="hub-partitions-and-per-partition-limits"></a>Merkez bölümleri ve bölüm sınırları başına

Time Series Insights ortamınızı planlarken, Time Series Insights bağlanacağınız olay kaynakları 'nın yapılandırılmasını göz önünde bulundurmanız önemlidir. Hem Azure IoT Hub hem de Event Hubs, olay işleme için yatay ölçeklendirmeyi etkinleştirmek üzere bölümleri kullanır. 

*Bölüm* , bir hub 'da tutulan olayların sıralı dizisidir. Bölüm sayısı, hub oluşturma aşamasında ayarlanır ve değiştirilemez.

Event Hubs bölümlendirme en iyi uygulamaları için [kaç bölümden Ihtiyacım olduğunu](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need) gözden geçirin.

> [!NOTE]
> Azure Time Series Insights en çok kullanılan IoT Hub 'Larının yalnızca dört bölüm olması gerekir.

Time Series Insights ortamınız için yeni bir hub oluşturuyor veya var olanı kullanarak, önizleme sınırları dahilinde olup olmadığını öğrenmek için bölüm başına alma hızınızı hesaplamanız gerekir. 

Azure Time Series Insights Preview şu anda **0,5 Mbps 'lik bölüm sınırına göre**genel bir.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub özgü konular

Bir cihaz IoT Hub oluşturulduğunda, kalıcı olarak bir bölüme atanır. Bunu yaparken, IoT Hub olay sıralamasını garanti edebilir (atama hiçbir şekilde değişmeyeceğinden).

Sabit bölüm ataması Ayrıca, IoT Hub aşağı akış 'dan gönderilen verilerin Time Series Insights örnekleri de etkiler. Birden çok cihazdan gelen iletiler hub 'a aynı ağ geçidi cihaz KIMLIĞI kullanılarak iletildiğinde, bu, bölüm başına ölçek limitlerini aşan büyük olasılıkla aynı anda aynı bölüme ulaşabilirler.

**Etki**:

* Tek bir bölüm önizleme sınırı üzerinden sürekli alma oranı yaşıyorsa, IoT Hub veri saklama süresi aşılmadan önce Time Series Insights tüm cihaz telemetrisini eşitlememesi mümkündür. Sonuç olarak, giriş sınırları sürekli olarak aşılırsa gönderilen veriler kaybolabilir.

Bu durumda, aşağıdaki en iyi yöntemleri öneririz:

* Çözümünüzü dağıtmaya başlamadan önce ortamınızı ve bölüm başına giriş oranlarını hesaplayın.
* IoT Hub cihazlarınızın mümkün olan en yüksek ölçüde yük dengelemesi yapıldığından emin olun.

> [!IMPORTANT]
> Bir olay kaynağı olarak IoT Hub kullanan ortamlarda, önizlemede bölüm sınırlaması başına 0,5 MBps 'in altına düştüğünü sağlamak için kullanımdaki hub cihazı sayısını kullanarak giriş hızını hesaplayın.
>
> * Aynı anda birkaç olay geldikçe bile, önizleme sınırı aşılmaz.

  ![IoT Hub bölüm diyagramı](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Hub aktarım hızını ve bölümleri iyileştirme hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [IoT Hub ölçeği](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Olay Hub 'ı ölçeği](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Olay Hub 'ı bölümleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Veri depolama

Bir Time Series Insights Preview *Kullandıkça Öde* (PAYG) SKU ortamı oluşturduğunuzda iki Azure kaynağı oluşturursunuz:

* Normal veri depolama için yapılandırılabilecek bir Azure Time Series Insights önizleme ortamı.
* Soğuk veri depolama için Azure depolama genel amaçlı v1 blob hesabı.

Isınma deponuzdaki veriler yalnızca [zaman serisi sorgusu](./time-series-insights-update-tsq.md) ve [Azure Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)ile kullanılabilir. Isınma depoluıza Time Series Insights ortamı oluşturulurken seçilen [Bekletme dönemi](./time-series-insights-update-plan.md#the-preview-environment) içinde son veriler yer alacak.

Time Series Insights önizleme, soğuk mağaza verilerinizi [Parquet dosya biçiminde](#parquet-file-format-and-folder-structure)Azure Blob depolamaya kaydeder. Time Series Insights önizlemesi, bu soğuk depolama verilerini özel olarak yönetir, ancak doğrudan standart Parquet dosyaları olarak okumanız için kullanılabilir.

> [!WARNING]
> Soğuk depo verilerinin bulunduğu Azure Blob depolama hesabının sahibi olarak hesaptaki tüm verilere tam erişiminiz vardır. Bu erişim yazma ve silme izinleri içerir. Veri kaybına neden olabileceğinden, önizleme yazmaları Time Series Insights verileri düzenleme veya silme.

### <a name="data-availability"></a>Veri kullanılabilirliği

En iyi sorgu performansı için bölümleri ve dizinleri Azure Time Series Insights görüntüleyin. Veriler, Dizin oluşturulduktan sonra hem normal (etkinse) hem de soğuk depodan sorgu için kullanılabilir hale gelir. Alınan veri miktarı bu kullanılabilirliği etkileyebilir.

> [!IMPORTANT]
> Önizleme sırasında, veriler kullanılabilir hale gelmeden önce 60 saniyeye kadar bir süre yaşayabilirsiniz. 60 saniyenin ötesinde önemli gecikme yaşınızı yaşıyorsanız lütfen Azure portal bir destek bileti gönderebilirsiniz.

## <a name="azure-storage"></a>Azure Depolama

Bu bölümde Azure Time Series Insights önizlemesiyle ilgili Azure depolama ayrıntıları açıklanmaktadır.

Azure Blob depolama alanının kapsamlı bir açıklaması için, [depolama Blobları giriş](../storage/blobs/storage-blobs-introduction.md)' i okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Azure Time Series Insights Preview PAYG ortamı oluşturduğunuzda, uzun vadeli soğuk depolduğunuz için bir Azure depolama genel amaçlı v1 blob hesabı oluşturulur.  

Azure Time Series Insights önizleme, Azure Depolama hesabınızdaki her bir olayın en fazla iki kopyasını tutar. Bir kopya alma zamanına göre sıralanmış olayları depolar, her zaman bir zaman sıralı dizideki olaylara erişime izin verir. Zaman içinde Time Series Insights önizlemesi, performansı iyileştirmek için verilerin yeniden bölümlendirilmiş bir kopyasını da oluşturur Time Series Insights.

Genel Önizleme sırasında veriler Azure depolama hesabınızda süresiz olarak depolanır.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 'ları yazma ve düzenlemekle

Sorgu performansının ve veri kullanılabilirliğinin emin olmak için Time Series Insights önizlemenin oluşturduğu blob 'ları düzenlemeyin veya silmeyin.

#### <a name="accessing-time-series-insights-preview-cold-store-data"></a>Time Series Insights önizleme soğuk mağaza verilerine erişme

[Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md) ve [zaman serisi sorgusundan](./time-series-insights-update-tsq.md)verilerinize erişmenin yanı sıra, soğuk depoda depolanan Parquet dosyalarından verilerinize doğrudan de erişmek isteyebilirsiniz. Örneğin, bir Jupyter not defterindeki verileri okuyabilir, dönüştürebilir ve temizleyebilir, ardından Azure Machine Learning modelinizi aynı Spark iş akışında eğitebilmeniz için bunu kullanabilirsiniz.

Verilere doğrudan Azure Storage hesabınızdan erişmek için, Time Series Insights Preview verilerinizi depolamak için kullanılan hesaba okuma erişiminizin olması gerekir. Daha sonra seçili verileri, aşağıda açıklanan Parquet dosyasının oluşturulma zamanına göre, `PT=Time` [Parquet dosya biçimi](#parquet-file-format-and-folder-structure) bölümünde bulabilirsiniz.  Depolama Hesabınıza yönelik okuma erişiminin etkinleştirilmesi hakkında daha fazla bilgi için bkz. [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Veri silme

Time Series Insights önizleme dosyalarınızı silmeyin. Yalnızca Time Series Insights Preview içinden ilgili verileri yönetin.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet dosya biçimi ve klasör yapısı

Parquet, verimli depolama ve performans için tasarlanan açık kaynaklı bir sütunlu dosya biçimidir. Time Series Insights önizlemesi, zaman serisi KIMLIK tabanlı sorgu performansını ölçekte etkinleştirmek için Parquet kullanır.  

Parquet dosya türü hakkında daha fazla bilgi için, [Parquet belgelerini](https://parquet.apache.org/documentation/latest/)okuyun.

Time Series Insights önizlemesi verilerinizin kopyalarını aşağıdaki gibi depolar:

* Birincisi, ilk kopya alma zamanına göre bölümlendirilir ve verileri kabaca gelişme sırasıyla depolar. Bu veriler şu klasörde bulunur `PT=Time` :

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* İkinci, yeniden bölümlenmiş kopya zaman serisi kimliklerine göre gruplandırılır ve `PT=TsId` klasöründe bulunur:

  `V=1/PT=TsId/<TSI_INTERNAL_STRUCTURE>/<TSI_INTERNAL_NAME>.parquet`

Klasördeki Blobların adlarındaki zaman damgası, `PT=Time` VERILERI TSI (olayların zaman damgasına değil) için varış zamanına karşılık gelir.

`PT=TsId`Klasördeki veriler, zaman içinde sorgu için iyileştirilebilir ve statik değildir. Yeniden bölümleme sırasında, aynı olaylar birden fazla blobda bulunabilir. Ayrıca, Blobların adlandırılması gelecekte değişebilir.

> [!NOTE]
>
> * `<YYYY>`dört basamaklı bir yıl gösterimine eşlenir.
> * `<MM>`iki basamaklı bir ay gösterimiyle eşlenir.
> * `<YYYYMMDDHHMMSSfff>`dört basamaklı yıl ( `YYYY` ), iki basamaklı ay (), iki basamaklı gün (), iki basamaklı `MM` `DD` saat ( `HH` ), iki basamaklı dakika ( `MM` ), iki basamaklı saniye ( `SS` ) ve üç basamaklı milisaniyelik ( `fff` ) ile zaman damgası gösterimine eşler.

Time Series Insights Preview olayları, aşağıdaki gibi, Parquet dosya içeriklerine eşlenir:

* Her olay tek bir satırla eşlenir.
* Her satır, bir olay zaman damgasıyla **zaman damgası** sütunu içerir. Zaman damgası özelliği hiçbir zaman null değildir. Olay kaynağında zaman damgası özelliği belirtilmemişse, varsayılan olarak **olay sıraya alınan zaman** . Depolanan zaman damgası her zaman UTC olarak kullanılır.
* Her satır, Time Series Insights ortamı oluşturulduğunda tanımlanan zaman serisi KIMLIĞI (TSıD) sütununu içerir. TSıD Özellik adı, soneki içerir `_string` .
* Telemetri verileri olarak gönderilen diğer tüm özellikler `_string` , özellik türüne bağlı olarak, (dize), ( `_bool` Boolean), `_datetime` (DateTime) veya `_double` (Double) ile biten sütun adlarıyla eşleştirilir.
* Bu eşleme şeması, **V = 1** olarak başvurulan ve aynı ada sahip temel klasörde depolanan dosya biçiminin ilk sürümü için geçerlidir. Bu özellik geliştikçe, bu eşleme şeması değişebilir ve başvuru adı artar.

## <a name="next-steps"></a>Sonraki adımlar

- Giriş [ve sorgu IÇIN JSON şeklini](./time-series-insights-update-how-to-shape-events.md)okuyun.

- Yeni [veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.
