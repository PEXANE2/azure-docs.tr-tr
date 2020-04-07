---
title: Önizlemede veri depolama ve giriş - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri Önizleme'de veri depolama ve giriş hakkında bilgi edinin.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/10/2020
ms.custom: seodec18
ms.openlocfilehash: 95a579cacc339360295f5f25fa6415ab29cd68ff
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673909"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Öngörüleri Önizlemesinde veri depolama ve giriş

Bu makalede, Azure Zaman Serisi Öngörüleri Önizlemesi için veri depolama ve giriş güncelleştirmeleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve Time Series ID özelliğini açıklar. Temel giriş işlemi, en iyi uygulamalar ve geçerli önizleme sınırlamaları da açıklanmıştır.

## <a name="data-ingress"></a>Veri girişi

Azure Zaman Serisi Öngörüleri ortamınız, zaman serisi verilerini toplamak, işlemek ve depolamak için bir *yutma motoru* içerir. 

[Çevrenizi planlarken](time-series-insights-update-plan.md)tüm gelen verilerin işlenmesini sağlamak, yüksek giriş ölçeğine ulaşmak ve *yutma gecikmesüresini* en aza indirmek için (Etkinlik kaynağından verileri okumak ve işlemek için Time Series Insights tarafından alınan süre) dikkat edilmesi gereken bazı noktalar vardır.

Zaman Serisi Öngörüleri Önizleme veri girişi ilkeleri, verilerin nereden alınabileceğini ve verilerin hangi biçimde olması gerektiğini belirler.

### <a name="ingress-policies"></a>Giriş ilkeleri

*Veri girişi,* verilerin Azure Zaman Serisi Öngörüleri Önizleme ortamına nasıl gönderildiğini içerir. 

Anahtar yapılandırma, biçimlendirme ve en iyi uygulamalar aşağıda özetlenmiştir.

#### <a name="event-sources"></a>Olay Kaynakları

Azure Zaman Serisi Öngörüleri Önizleme aşağıdaki etkinlik kaynaklarını destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Azure Zaman Serisi Öngörüleri Önizleme, her örnek için en fazla iki olay kaynağını destekler. Bir olay kaynağını bağladığınızda, TSI ortamınız en eski olaydan başlayarak Şu anda Iot veya Event Hub'ınızda depolanan tüm olayları okur. 

> [!IMPORTANT] 
> * Önizleme ortamınıza bir olay kaynağı takarken yüksek başlangıç gecikmesi yaşayabilirsiniz. 
> Olay kaynağı gecikmesi, IoT Hub'ınızda veya Olay Hub'ınızda şu anda bulunan olayların sayısına bağlıdır.
> * Olay kaynağı verileri ilk olarak yutulduktan sonra yüksek gecikme oranı azalır. Sürekli yüksek gecikme cezası yla karşılaşırsanız, Azure portalı üzerinden bir destek bileti gönderin.

#### <a name="supported-data-format-and-types"></a>Desteklenen veri biçimi ve türleri

Azure Zaman Serisi Öngörüleri, Azure IoT Hub veya Azure Etkinlik Hub'larından gönderilen UTF-8 kodlu JSON'u destekler. 

Desteklenen veri türleri şunlardır:

| Veri türü | Açıklama |
|---|---|
| **bool** | İki durumdan birine sahip `true` bir `false`veri türü: veya . |
| **Datetime** | Genellikle günün tarihi ve saati olarak ifade edilen bir zamanı temsil eder. [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) formatında ifade edilir. |
| **double** | Çift duyarlıklı 64 bit [IEEE 754](https://ieeexplore.ieee.org/document/8766229) yüzer nokta. |
| **Dize** | Unicode karakterlerinden oluşan metin değerleri.          |

#### <a name="objects-and-arrays"></a>Nesneler ve diziler

Olay yükünüzün bir parçası olarak nesneler ve diziler gibi karmaşık türler gönderebilirsiniz, ancak verileriniz depolandığında düzleme işleminden geçer. 

JSON etkinliklerinizi nasıl şekillendireceklerini, karmaşık tür de göndererek ve iç içe içe olan nesne düzleştirmeyi açıklayan ayrıntılı bilgiler, planlama ve optimizasyona yardımcı olmak [için giriş ve sorgu için JSON'u nasıl şekillendirecek?](./time-series-insights-update-how-to-shape-events.md)

### <a name="ingress-best-practices"></a>Ingress en iyi uygulamaları

Aşağıdaki en iyi uygulamaları kullanmanızı öneririz:

* Azure Zaman Serisi Öngörülerini ve aynı bölgedeki herhangi bir IoT Hub veya Etkinlik Hub'ını, olası gecikme süresini azaltmak için yapılandırın.

* Beklenen alım oranınızı hesaplayarak ve aşağıda listelenen desteklenen orana denk geldiğini doğrulayarak [ölçek ihtiyaçlarınız](time-series-insights-update-plan.md) için plan yapın.

* Giriş ve [sorgu için JSON'u nasıl şekillendirirken json](./time-series-insights-update-how-to-shape-events.md)verilerinizi ve önizlemedeki geçerli sınırlamaları nasıl optimize edip şekillendireceklerinizi anlayın.

### <a name="ingress-scale-and-preview-limitations"></a>Giriş ölçeği ve Önizleme sınırlamaları 

Azure Zaman Serisi Öngörüleri Önizleme sınırlamaları aşağıda açıklanmıştır.

> [!TIP]
> Tüm Önizleme sınırlarının kapsamlı bir listesi için [Önizleme ortamınızı](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-plan#review-preview-limits) planlayın'ı okuyun.

#### <a name="per-environment-limitations"></a>Çevre sınırlamaları başına

Genel olarak, giriş oranları kuruluşunuzdaki aygıt sayısı, olay emisyon sıklığı ve her olayın boyutu faktörü olarak görülebilir:

*  **Cihaz sayısı** × **Olay emisyon frekansı** × **Her olayın boyutu**.

Varsayılan olarak, Time Series Insights önizlemesi gelen verileri **Time Series Insights ortamı başına saniyede en fazla 1 megabayt (MBp)** oranında yutabilir. Hub bölümü [başına](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-storage-ingress#hub-partitions-and-per-partition-limits)ek sınırlamalar vardır.

> [!TIP] 
> * 16 MBps'ye kadar olan hızların alınması için çevre desteği istek üzerine sağlanabilir.
> * Azure portalı üzerinden bir destek bileti göndererek daha yüksek iş bilgili yseniz bize ulaşın.
 
* **Örnek 1:**

    Contoso Shipping dakikada üç kez bir olay yontmak 100.000 cihazlar vardır. Bir olayın boyutu 200 bayttır. Time Series Insights etkinlik kaynağı olarak dört bölümlü bir Iot Hub kullanıyorlar.

    * Onların Time Series Insights ortamı için yutma oranı olacaktır: **100.000 cihazlar * 200 bayt / olay * (3/60 olay / sn) = 1 MBps**.
    * Bölüm başına yutma oranı 0,25 MBps olacaktır.
    * Contoso Shipping'in yutma oranı önizleme ölçeği sınırlaması dahilinde olacaktır.

* **Örnek 2:**

    Contoso Fleet Analytics'te her saniye bir etkinlik yalayan 60.000 cihaz bulunmaktadır. Zaman Serisi Öngörüleri etkinlik kaynağı olarak bölüm sayısı 4 olan bir Olay Hub'ı kullanıyorlar. Bir olayın boyutu 200 bayttır.

    * Çevre yeme oranı olacaktır: **60.000 cihazlar * 200 bayt/olay * 1 olay/sn = 12 MBps**.
    * Bölüm başına oranı 3 MBps olacaktır.
    * Contoso Fleet Analytics'in yutma oranı çevre ve bölüm sınırlarının üzerindedir. Ortamları için alım oranını artırmak için Azure portalı aracılığıyla Time Series Öngörüleri'ne bir istek gönderebilir ve Önizleme sınırları içinde daha fazla bölüm içeren bir Etkinlik Hub'ı oluşturabilirler.

#### <a name="hub-partitions-and-per-partition-limits"></a>Hub bölümleri ve bölüm başına sınırları

Time Series Insights ortamınızı planlarken, Time Series Insights'a bağlanacağınız etkinlik kaynağının yapılandırmasını göz önünde bulundurmanız önemlidir. Hem Azure IoT Hub'ı hem de Olay Hub'ları, olay işleme için yatay ölçeklendirme sağlamak için bölümler kullanır. 

*Bölüm,* hub'da tutulan olayların sıralı dizisidir. Bölüm sayısı hub oluşturma aşamasında ayarlanır ve değiştirilemez. 

En iyi uygulamaları bölümleyen Olay Hub'ları için [kaç bölüme ihtiyacım olduğunu](https://docs.microsoft.com/azure/event-hubs/event-hubs-faq#how-many-partitions-do-i-need) gözden geçirin?

> [!NOTE]
> Azure Zaman Serisi Öngörüleri ile kullanılan Çoğu IoT Hub'ın yalnızca dört bölüme ihtiyacı vardır.

İster Time Series Öngörüleri ortamınız için yeni bir hub oluşturuyor olun ister varolan bir hub kullanıyor olun, önizleme sınırları içinde olup olmadığını belirlemek için bölüm başına alım oranınızı hesaplamanız gerekir. 

Azure Zaman Serisi Öngörüleri Önizlemeşu anda **bölüm başına genel bir 0,5 MBps sınırına**sahiptir.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub'a özel hususlar

Bir aygıt IoT Hub'ında oluşturulduğunda, kalıcı olarak bir bölüme atanır. Bunu yaparken, IoT Hub olay siparişi garanti edebiliyor (atama asla değişmeyeceğiiçin).

Sabit bir bölüm ataması, IoT Hub'dan gönderilen verileri sindiren Time Series Insights örneklerini de etkiler. Aynı ağ geçidi aygıt kimliği kullanılarak birden çok cihazdan gelen iletiler hub'a iletildiğinde, aynı bölüme aynı anda gelebilirler ve bölüm başına ölçek sınırlarını aşma potansiyeline sahiptir. 

**Etki**:

* Tek bir bölüm Önizleme sınırı üzerinde sürekli bir alım oranı yla karşılanırsa, Zaman Serisi Öngörüleri IoT Hub veri saklama süresi aşılmadan önce tüm aygıt telemetrilerini eşitlemez. Sonuç olarak, yutma limitleri sürekli aşılırsa gönderilen veriler kaybedilebilir.

Bu durumu azaltmak için aşağıdaki en iyi uygulamaları öneririz:

* Çözümünüzü dağıtmadan önce ortam ve bölüm başına alım oranlarınızı hesaplayın.
* IoT Hub aygıtlarınızın mümkün olan en uzak ölçüde yük dengeli olduğundan emin olun.

> [!IMPORTANT]
> IoT Hub'ı olay kaynağı olarak kullanan ortamlar için, oranın önizlemedeki bölüm sınırlaması başına 0,5 MBps'nin altına düştüğünden emin olmak için kullanılan hub aygıtlarının sayısını kullanarak yutma oranını hesaplayın.
> * Aynı anda birden fazla olay gelse bile, Önizleme sınırı aşılmaz.

  ![IoT Hub Bölüm Diyagramı](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

Hub iş ve bölümlerini en iyi duruma getirmek hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [IoT Hub Ölçeği](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Olay Hub Ölçeği](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Olay Hub Bölümleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Veri depolama

Bir Zaman Serisi Öngörüleri *Önizlemesi istediğiniz kadar öde* (PAYG) SKU ortamı oluşturduğunuzda, iki Azure kaynağı oluşturursunuz:

* Sıcak veri depolama için yapılandırılabilen Bir Azure Zaman Serisi Öngörüler Önizleme ortamı.
* Soğuk veri depolama için Azure Depolama genel amaçlı V1 blob hesabı.

Sıcak mağazanızdaki veriler yalnızca [Zaman Serisi Sorgusu](./time-series-insights-update-tsq.md) ve [Azure Zaman Serisi Öngörüleri Önizleme gezgini](./time-series-insights-update-explorer.md)aracılığıyla kullanılabilir. Sıcak mağazanız, Time Series Öngörüleri ortamını oluştururken seçilen [bekletme süresi](./time-series-insights-update-plan.md#the-preview-environment) içinde son verileri içerir.

Zaman Serisi Öngörüleri Önizleme, soğuk depo verilerinizi [Parke dosya biçimindeki](#parquet-file-format-and-folder-structure)Azure Blob depolamaalanına kaydeder. Zaman Serisi Öngörüler Önizleme bu soğuk mağaza verilerini yalnızca yönetir, ancak doğrudan standart Parke dosyaları olarak okumanız için kullanılabilir.

> [!WARNING]
> Soğuk depo verilerinin bulunduğu Azure Blob depolama hesabının sahibi olarak, hesaptaki tüm verilere tam erişime sahip siniz. Bu erişim, yazma ve silme izinlerini içerir. Zaman Serisi Öngörüleri Önizleme'nin yazdığı verileri düzenlemayın veya silmeyin, çünkü bu veri kaybına neden olabilir.

### <a name="data-availability"></a>Veri kullanılabilirliği

Azure Zaman Serisi Öngörüleri Optimum sorgu performansı için verileri önizleme ve dizinler. Veriler, dizine alındıktan sonra hem sıcak (etkinse) hem de soğuk depodan sorgu için kullanılabilir hale gelir. Yutulan veri miktarı bu kullanılabilirliği etkileyebilir.

> [!IMPORTANT]
> Önizleme sırasında, verilerin kullanılabilir hale gelmesinden önce 60 saniyeye kadar bir süre yaşayabilirsiniz. 60 saniyenin üzerinde önemli bir gecikme gecikmesi yaşıyorsanız, lütfen Azure portalı üzerinden bir destek bileti gönderin.

## <a name="azure-storage"></a>Azure Storage

Bu bölümde, Azure Zaman Serisi Öngörüleri Önizlemesi ile ilgili Azure Depolama ayrıntıları açıklanmaktadır.

Azure Blob depolama alanının ayrıntılı bir açıklaması için [Depolama blobs girişini](../storage/blobs/storage-blobs-introduction.md)okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Bir Azure Zaman Serisi Öngörüleri PayG Önizleme ortamı oluşturduğunuzda, uzun süreli soğuk mağazanız olarak Azure Depolama genel amaçlı bir V1 blob hesabı oluşturulur.  

Azure Zaman Serisi Öngörüleri Önizleme, Azure Depolama hesabınızdaki her etkinliğin en fazla iki kopyasını saklar. Bir kopya, her zaman zaman sıralı bir sırada olaylara erişim sağlayan, yutma süresi tarafından sıralanan olayları depolar. Zaman içinde, Zaman Serisi Öngörüler Önizleme de performant Time Series Insights sorgu için optimize etmek için verilerin yeniden bölümlenmiş bir kopyasını oluşturur. 

Genel Önizleme sırasında veriler Azure Depolama hesabınızda süresiz olarak depolanır.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Yazma ve düzenleme Time Series Insights blobs

Sorgu performansı ve veri kullanılabilirliği sağlamak için, Zaman Serisi Öngörüleri Önizleme'nin oluşturduğu lekeleri düzenlemayın veya silmeyin.

#### <a name="accessing-time-series-insights-preview-cold-store-data"></a>Erişim Zaman Serisi Öngörüleri Soğuk mağaza verilerini önizleme 

[Zaman Serisi Öngörüleri Önizleme kaşifi](./time-series-insights-update-explorer.md) ve [Zaman Serisi Sorgusu'ndan](./time-series-insights-update-tsq.md)verilerinize erişmenize ek olarak, verilerinize soğuk depoda depolanan Parke dosyalarından doğrudan erişmek de isteyebilirsiniz. Örneğin, bir Jupyter dizüstü bilgisayarındaki verileri okuyabilir, dönüştürebilir ve temizleyebilir, ardından Azure Machine Learning modelinizi aynı Spark iş akışında eğitmek için kullanabilirsiniz.

Verilere doğrudan Azure Depolama hesabınızdan erişmek için, Zaman Serisi Öngörüleri Önizleme verilerinizi depolamak için kullanılan hesaba okuma erişimi yapmanız gerekir. Daha sonra `PT=Time` [parke dosya formatı](#parquet-file-format-and-folder-structure) bölümünde aşağıda açıklanan klasörde bulunan Parke dosyasının oluşturma süresine göre seçili verileri okuyabilirsiniz.  Depolama hesabınıza okuma erişimini etkinleştirme hakkında daha fazla bilgi için [bkz.](../storage/blobs/storage-manage-access-to-resources.md)

#### <a name="data-deletion"></a>Veri silme

Zaman Serisi Öngörüleri Önizleme dosyalarınızı silmeyin. Yalnızca Time Series Öngörüleri Önizleme'den ilgili verileri yönetin.

### <a name="parquet-file-format-and-folder-structure"></a>Parke dosya biçimi ve klasör yapısı

Parke, verimli depolama ve performans için tasarlanmış açık kaynak kodlu bir sütun dosyası biçimidir. Zaman Serisi Öngörüleri Önizleme ölçekte Zaman Serisi KIMLIK tabanlı sorgu performansını etkinleştirmek için Parke kullanır.  

Parke dosya türü hakkında daha fazla bilgi için [Parke belgelerini](https://parquet.apache.org/documentation/latest/)okuyun.

Zaman Serisi Öngörüleri Önizleme, verilerinizin kopyalarını aşağıdaki gibi saklar:

* İlk, ilk kopya yutma süresine göre bölümlenir ve verileri varış sırasına göre kabaca depolar. Bu veriler `PT=Time` klasörde bulunur:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* İkinci, yeniden bölümlenen kopya Time Series dislerine göre gruplandırılır ve `PT=TsId` klasörde bulunur:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

Her iki durumda da, Parke dosyasının zaman özelliği blob oluşturma süresine karşılık gelir. Klasördeki `PT=Time` veriler, dosyaya yazıldıktan sonra hiçbir değişiklik olmadan korunur. Klasördeki `PT=TsId` veriler zaman içinde sorgu için en iyi duruma getirilir ve statik değildir.

> [!NOTE]
> * `<YYYY>`dört basamaklı bir yıl gösterimi için haritalar.
> * `<MM>`iki basamaklı ay gösterimi için eşler.
> * `<YYYYMMDDHHMMSSfff>`dört basamaklı`YYYY`yıl (), iki basamaklı ay (`MM`), iki basamaklı saat`DD`( ), iki`HH`basamaklı dakika (`MM`), iki basamaklı`SS`dakika ( ), iki`fff`basamaklı saniye ( ), ve üç basamaklı milisaniye ( ) ile bir zaman damgası gösterimi için haritalar .

Zaman Serisi Öngörüleri Önizleme etkinlikleri Parke dosya içeriğine aşağıdaki şekilde eşlenir:

* Her olay tek bir satırla eşler.
* Her satırda olay zaman damgası içeren **zaman damgası** sütunu içerir. Zaman damgası özelliği asla hükümsüz değildir. Zaman damgası özelliği olay kaynağında belirtilmemişse, olay sırasına göre varsayılan **olarak olay süresine** göre belirlenir. Saklanan zaman damgası her zaman UTC'dedir.
* Her satır, Zaman Serisi Öngörüleri ortamı oluşturulduğunda tanımlandığı gibi Zaman Serisi Kimliği (TSID) sütununa (s) içerir. TSID özellik adı `_string` sonek içerir.
* Telemetri verisi olarak gönderilen diğer tüm özellikler, `_string` özellik türüne bağlı olarak (string), (Boolean), `_bool` `_datetime` (datetime) veya `_double` (çift) ile biten sütun adlarına eşlenir.
* Bu eşleme şeması, **V=1** olarak başvurulan ve aynı adı taşıyan temel klasörde depolanan dosya biçiminin ilk sürümü için geçerlidir. Bu özellik geliştikçe, bu eşleme şeması değişebilir ve başvuru adı artabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Giriş ve sorgu için JSON'u nasıl şekillendireceklerini](./time-series-insights-update-how-to-shape-events.md)okuyun.

- Yeni veri [modelleme](./time-series-insights-update-tsm.md)si hakkında bilgi edinin.
