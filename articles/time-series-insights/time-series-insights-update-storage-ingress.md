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
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: f00529d00312fd6acb045de698590047f991bec7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76714295"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri depolama ve giriş

Bu makalede Azure Time Series Insights önizlemesi için veri depolama ve giriş güncelleştirmeleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve zaman serisi KIMLIĞI özelliğini içerir. Ayrıca temel alınan giriş işlemini, en iyi uygulamaları ve geçerli önizleme sınırlamalarını da açıklar.

## <a name="data-ingress"></a>Veri girişi

Azure Time Series Insights ortamınız, zaman serisi verilerini toplamak, işlemek ve depolamak için bir giriş altyapısı içerir. Ortamınızı planlarken, tüm gelen verilerin işlenmesini sağlamak ve yüksek giriş ölçeğine ulaşmak ve giriş gecikmesini en aza indirmek için göz önünde bulundurmanız gereken bazı noktalar vardır. (TSI tarafından olaydan gelen verileri okumak ve işlemek için geçen süre Kaynak). 

Time Series Insights önizlemede, veri giriş ilkeleri verilerin nereden kaynakta yapılabileceğini ve verilerin ne biçimde olması gerektiğini tespit edebilir.

### <a name="ingress-policies"></a>Giriş ilkeleri

#### <a name="event-sources"></a>Olay kaynakları

Time Series Insights önizlemesi aşağıdaki olay kaynaklarını destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights önizlemesi, örnek başına en fazla iki olay kaynağını destekler.

> [!WARNING] 
> * Önizleme ortamınıza bir olay kaynağı eklerken yüksek gecikme süresi yaşayabilirsiniz. 
> Olay kaynağı gecikmesi, IoT Hub veya Olay Hub 'ınızdaki olay sayısına bağlıdır.
> * Olay kaynak verilerinin ilk kez alındıktan sonra yüksek gecikme süresi alt tarafı olur. Devam eden yüksek gecikme süresi yaşarsanız Azure portal aracılığıyla bir destek bileti göndererek bizimle iletişim kurun.

#### <a name="supported-data-format-and-types"></a>Desteklenen veri biçimi ve türleri

Azure Time Series Insights, Azure IoT Hub veya Azure Event Hubs aracılığıyla gönderilen UTF8 kodlu JSON 'yi destekler. 

Desteklenen veri türlerinin listesi aşağıda verilmiştir.

| Veri türü | Açıklama |
|-----------|------------------|-------------|
| bool      |   İki durumdan birine sahip bir veri türü: true veya false.       |
| Tarih/saat    |   Genellikle günün tarih ve saati olarak ifade edilen bir anlık zamanı temsil eder. DateTimes ISO 8601 biçiminde olmalıdır.      |
| double    |   Çift duyarlıklı 64 bit IEEE 754 kayan nokta
| string    |   Unicode karakterlerinden oluşan metin değerleri.          |

#### <a name="objects-and-arrays"></a>Nesneler ve diziler

Olay yüklerinizin bir parçası olarak nesneler ve diziler gibi karmaşık türler gönderebilirsiniz, ancak verileriniz depolandığında bir düzleştirme işlemini üstlerdir. JSON olaylarınızın yanı sıra karmaşık tür ve iç içe nesne düzleştirme hakkındaki ayrıntıları nasıl şekillendirilebileceğinize ilişkin daha fazla bilgi için bkz. giriş [ve sorgu IÇIN JSON 'u nasıl şekillendirilebileceğinize](./time-series-insights-update-how-to-shape-events.md)ilişkin sayfa.


### <a name="ingress-best-practices"></a>Giriş en iyi uygulamaları

Aşağıdaki en iyi yöntemleri kullanmanızı öneririz:

* Ağ üzerinden oluşan alma gecikmesini azaltmak için aynı bölgedeki Time Series Insights ve IoT Hub ya da Olay Hub 'ını yapılandırın.
* Beklenen alma hızınızı hesaplayarak ve aşağıda listelenen desteklenen oran dahilinde olduğunu doğrulayarak ölçek gereksinimlerinizi planlayın
* Giriş [ve sorgu IÇIN JSON şeklini](./time-series-insights-update-how-to-shape-events.md)okuyarak, JSON verilerinizin ve önizleme aşamasında geçerli sınırlamaların nasıl iyileştirileceği ve şekillendirilmeli olduğunu anlayın.

### <a name="ingress-scale-and-limitations-in-preview"></a>Önizlemede giriş ölçeği ve sınırlamalar

#### <a name="per-environment-limitations"></a>Ortam başına sınırlamalar

Genel olarak giriş fiyatları, kuruluşunuzda bulunan cihazların sayısı, olay egörev sıklığı ve her olayın boyutu olarak görüntülenir:

*  **Cihazların sayısı** × **olay emisi sıklığı** **her olayın boyutu**.

Time Series Insights önizlemesi, varsayılan olarak, **her**bir (MB/sn) bir saniyede 1 megabayta kadar (Mbps) bir hızda gelen verileri alabilir. Gereksinimlerinizi karşılamıyorsa bizimle iletişim kurun Azure portal bir destek bileti göndererek bir ortam için en fazla 16 MBps destekliyoruz.
 
Örnek 1: contoso dağıtımı, dakikada üç kez bir olay sunan 100.000 cihaza sahiptir. Bir olayın boyutu 200 bayttır. Bunlar, TSI olay kaynağı olarak 4 bölümden oluşan bir olay hub 'ı kullanıyor.
TSI ortamları için alım oranı: 100.000 cihaz * 200 bayt/olay * (3/60 olay/sn) = 1 MB/sn olmalıdır.
Bölüm başına alım oranı 0,25 MBps olur.
Contoso sevkiyat alım oranı, önizleme ölçeği kısıtlaması dahilinde olacaktır.
 
Örnek 2: contoso Fleet Analytics, her saniye bir olay sunan 60.000 cihaza sahiptir. Bunlar, TSI olay kaynağı olarak 4 IoT Hub 24 bölüm sayısı kullanıyor. Bir olayın boyutu 200 bayttır.
Ortam alma hızı şu şekilde olacaktır: 20.000 cihaz * 200 bayt/olay * 1 olay/sn = 4 MBps.
Bölüm başına hız 1 MBps olur.
Contoso Fleet analizinin, bu ölçeğe ulaşmak için adanmış bir ortam için Azure portal aracılığıyla TSI 'e istek göndermesi gerekir.

#### <a name="hub-partitions-and-per-partition-limits"></a>Merkez bölümleri ve bölüm sınırları başına

TSI ortamınızı planlarken, TSI 'a bağlanacağınız olay kaynakları yapılandırmasını göz önünde bulundurmanız önemlidir. Hem Azure IoT Hub hem de Event Hubs, olay işleme için yatay ölçeklendirmeyi etkinleştirmek üzere bölümleri kullanır.  Bölüm, bir hub 'da tutulan olayların sıralı dizisidir. Bölüm sayısı, IoT veya Event Hubs ' oluşturma aşamasında ayarlanır ve değiştirilemez. Bölüm sayısını belirleme hakkında daha fazla bilgi için, Event Hubs ' SSS bölümüne kaç bölüme ihtiyacım var? IoT Hub kullanan TSI ortamları için genellikle çoğu IoT Hub 'ı yalnızca 4 bölümden yeterlidir. Örneğin, TSI ortamınız için yeni bir hub oluşturup oluşturmasanız veya var olanı kullanarak, önizleme sınırları dahilinde olup olmadığını anlamak için bölüm başına alma hızınızı hesaplamanız gerekir. TSI Preview şu anda 0,5 MB/sn 'lik **bölüm başına** sınıra sahiptir. Aşağıdaki örnekleri bir başvuru olarak kullanın ve IoT Hub bir Kullanıcı kullanıyorsanız, lütfen aşağıdaki IoT Hub özgü göz önünde bulundurun.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub özgü konular

Bir cihaz IoT Hub oluşturulduğunda bir bölüme atanır ve bölüm ataması değişmez. Bunu yaparak IoT Hub olay sıralamasını garanti edebilir. Ancak, bu, belirli senaryolarda bir aşağı akış okuyucu olarak TSI 'ın etkilerine karşı etkiler. Birden çok cihazdan gelen iletiler hub 'a aynı ağ geçidi cihaz KIMLIĞI kullanılarak iletildiğinde, bu nedenle bölüm başına ölçek sınırlamasını aşacak. 

**Etki**: tek bir bölüm önizleme sınırlaması üzerinden sürekli bir alım oranı yaşıyorsa, IoT Hub veri saklama süresi aşılmadan önce TSI Reader 'ın hiçbir zaman yakalamamasıdır. Bu, veri kaybına neden olabilir.

Şunları öneririz: 

* Çözümünüzü dağıtmaya başlamadan önce ortamınızı ve bölüm başına alma oranını hesaplama
* IoT Hub cihazlarınızın (ve bu nedenle bölümlerinin), mümkün olan en yüksek genişletecek şekilde yük dengelemesi yapıldığından emin olun

> [!WARNING]
> Bir olay kaynağı olarak IoT Hub kullanan ortamlarda, önizlemede bölüm sınırlaması başına 0,5 MBps 'in altına düştüğünü sağlamak için kullanımdaki hub cihazı sayısını kullanarak giriş hızını hesaplayın.

  ![IoT Hub bölüm diyagramı](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

İşleme birimleri ve bölümleri hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [IoT Hub ölçeği](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Olay Hub 'ı ölçeği](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Olay Hub 'ı bölümleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Veri depolama

Bir Time Series Insights Preview Kullandıkça Öde SKU ortamı oluşturduğunuzda iki Azure kaynağı oluşturursunuz:

* İsteğe bağlı olarak, sıcak mağaza yeteneklerini içerebilen bir Time Series Insights önizleme ortamı.
* Soğuk veri depolama için Azure depolama genel amaçlı v1 blob hesabı.

Isınma deponuzdaki veriler yalnızca [zaman serisi sorgusu](./time-series-insights-update-tsq.md) ve [Azure Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)ile kullanılabilir. 

Time Series Insights önizleme, soğuk mağaza verilerinizi [Parquet dosya biçiminde](#parquet-file-format-and-folder-structure)Azure Blob depolamaya kaydeder. Time Series Insights önizlemesi, bu soğuk depolama verilerini özel olarak yönetir, ancak doğrudan standart Parquet dosyaları olarak okumanız için kullanılabilir.

> [!WARNING]
> Soğuk depo verilerinin bulunduğu Azure Blob depolama hesabının sahibi olarak hesaptaki tüm verilere tam erişiminiz vardır. Bu erişim yazma ve silme izinleri içerir. Veri kaybına neden olabileceğinden, önizleme yazmaları Time Series Insights verileri düzenleme veya silme.

### <a name="data-availability"></a>Veri kullanılabilirliği

En iyi sorgu performansı için bölümleri ve dizinleri Time Series Insights görüntüleyin. Veriler dizinlendikten sonra sorgu için kullanılabilir hale gelir. Alınan veri miktarı bu kullanılabilirliği etkileyebilir.

> [!IMPORTANT]
> Önizleme sırasında, veriler kullanılabilir hale gelmeden önce 60 saniyeye kadar bir süre yaşayabilirsiniz. 60 saniyenin ötesinde önemli gecikme yaşınızı yaşıyorsanız lütfen Azure portal bir destek bileti gönderebilirsiniz.

## <a name="azure-storage"></a>Azure Storage

Bu bölümde Azure Time Series Insights önizlemesiyle ilgili Azure depolama ayrıntıları açıklanmaktadır.

Azure Blob depolama alanının kapsamlı bir açıklaması için, [depolama Blobları giriş](../storage/blobs/storage-blobs-introduction.md)' i okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Bir Time Series Insights Preview Kullandıkça Öde ortamı oluşturduğunuzda, uzun vadeli soğuk depolduğunuz için bir Azure depolama genel amaçlı v1 blob hesabı oluşturulur.  

Time Series Insights önizlemesi, Azure Depolama hesabınızdaki her bir olayın en fazla iki kopyasını yayımlar. İlk kopyanın alma zamanına göre sıralanmış olayları vardır ve her zaman korunur, böylece bu hizmete erişmek için diğer hizmetleri kullanabilirsiniz. Ham Parquet dosyalarını işlemek için Spark, Hadoop ve diğer tanıdık araçları kullanabilirsiniz. 

Time Series Insights Preview, Time Series Insights sorgu için iyileştirmek üzere Parquet dosyalarını yeniden bölümlendirir. Verilerin yeniden bölümlenmiş kopyası da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızda süresiz olarak depolanır.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 'ları yazma ve düzenlemekle

Sorgu performansının ve veri kullanılabilirliğinin emin olmak için Time Series Insights önizlemenin oluşturduğu blob 'ları düzenlemeyin veya silmeyin.

#### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Time Series Insights önizlemeye erişme ve verileri dışarı aktarma

Diğer hizmetlerle birlikte kullanmak üzere Time Series Insights önizleme Gezgininde görüntülenen verilere erişmek isteyebilirsiniz. Örneğin, Power BI bir rapor oluşturmak veya Azure Machine Learning Studio kullanarak makine öğrenimi modelini eğitebilmeniz için verilerinizi kullanabilirsiniz. Ya da verilerinizi Jupyıter Not defterlerinizde dönüştürmek, görselleştirmek ve modellemek için kullanabilirsiniz.

Verilerinize üç genel yolla erişebilirsiniz:

* Time Series Insights önizleme Gezgini ' nden. Verileri bir CSV dosyası olarak gezgin 'den dışarı aktarabilirsiniz. Daha fazla bilgi için [Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ni okuyun.
* Olayları Al sorgusunu kullanarak Time Series Insights Preview API. Bu API hakkında daha fazla bilgi edinmek için [zaman serisi sorgusunu](./time-series-insights-update-tsq.md)okuyun.
* Doğrudan bir Azure Storage hesabından. Time Series Insights Preview verilerinize erişmek için kullandığınız hesaba okuma erişiminizin olması gerekir. Daha fazla bilgi için, [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md)konusunu okuyun.

#### <a name="data-deletion"></a>Veri silme

Time Series Insights önizleme dosyalarınızı silmeyin. Yalnızca Time Series Insights Preview içinden ilgili verileri yönetin.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet dosya biçimi ve klasör yapısı

Parquet, verimli depolama ve performans için tasarlanan açık kaynaklı bir sütunlu dosya biçimidir. Time Series Insights önizleme, bu nedenlerle Parquet kullanır. Veri, ölçek sırasında sorgu performansı için zaman serisi KIMLIĞINE göre verileri bölümler.  

Parquet dosya türü hakkında daha fazla bilgi için, [Parquet belgelerini](https://parquet.apache.org/documentation/latest/)okuyun.

Time Series Insights önizlemesi verilerinizin kopyalarını aşağıdaki gibi depolar:

* Birincisi, ilk kopya alma zamanına göre bölümlendirilir ve verileri kabaca gelişme sırasıyla depolar. Veriler `PT=Time` klasöründe bulunur:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* İkinci, yeniden bölümlenmiş kopya, zaman serisi kimliklerinin gruplandırılmasıyla bölümlenir ve `PT=TsId` klasöründe bulunur:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

Her iki durumda da, zaman değerleri blob oluşturulma zamanına karşılık gelir. `PT=Time` klasöründeki veriler korunur. `PT=TsId` klasördeki veriler, zaman içinde sorgu için iyileştirilebilir ve statik olarak kalmaz.

> [!NOTE]
> * `<YYYY>` dört basamaklı bir yıl gösterimine eşlenir.
> * `<MM>` iki basamaklı bir ay gösterimiyle eşlenir.
> * `<YYYYMMDDHHMMSSfff>`, dört basamaklı yıl (`YYYY`), iki basamaklı ay (`MM`), iki basamaklı gün (`DD`), iki basamaklı saat (`HH`), iki basamaklı dakika (`MM`), iki basamaklı saniye (`SS`) ve üç basamaklı milisaniyelik (`fff`) ile zaman damgası gösterimine eşlenir.

Time Series Insights Preview olayları, aşağıdaki gibi, Parquet dosya içeriklerine eşlenir:

* Her olay tek bir satırla eşlenir.
* Her satır, bir olay zaman damgasıyla **zaman damgası** sütunu içerir. Zaman damgası özelliği hiçbir zaman null değildir. Olay kaynağında zaman damgası özelliği belirtilmemişse, **olay sıraya alınan zamanı** varsayılan olarak belirler. Zaman damgası her zaman UTC 'dir.
* Her satır, Time Series Insights ortamı oluşturulduğunda tanımlanan zaman serisi ID sütununu içerir. Özellik adı `_string` sonekini içerir.
* Telemetri verileri olarak gönderilen diğer tüm özellikler, özellik türüne bağlı olarak `_string` (dize), `_bool` (Boolean), `_datetime` (DateTime) veya `_double` (Double) ile biten sütun adlarıyla eşlenir.
* Bu eşleme düzeni, **V = 1**olarak başvurulan dosya biçiminin ilk sürümü için geçerlidir. Bu özellik geliştikçe, ad arttırılabilirler.

## <a name="next-steps"></a>Sonraki adımlar

- Giriş [ve sorgu IÇIN JSON şeklini](./time-series-insights-update-how-to-shape-events.md)okuyun.

- Yeni [veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.
