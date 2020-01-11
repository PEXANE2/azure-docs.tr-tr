---
title: Önizlemede veri depolama ve giriş-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizlemede veri depolama ve giriş hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: 1deca696ba576849701eb8719de7fbaa7895a26a
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861413"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri depolama ve giriş

Bu makalede Azure Time Series Insights önizlemesi için veri depolama ve giriş güncelleştirmeleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve zaman serisi KIMLIĞI özelliğini içerir. Ayrıca temel alınan giriş işlemini, en iyi uygulamaları ve geçerli önizleme sınırlamalarını da açıklar.

## <a name="data-ingress"></a>Veri girişi

Azure Time Series Insights ortamınız, zaman serisi verilerini toplamak, işlemek ve depolamak için bir giriş altyapısı içerir. Ortamınızı planlarken, tüm gelen verilerin işlenmesini sağlamak ve yüksek giriş ölçeğine ulaşmak ve giriş gecikmesini en aza indirmek için göz önünde bulundurmanız gereken bazı noktalar vardır. (TSI tarafından olaydan gelen verileri okumak ve işlemek için geçen süre Kaynak). 

Time Series Insights önizlemede, veri giriş ilkeleri verilerin nereden kaynakta yapılabileceğini ve verilerin ne biçimde olması gerektiğini tespit edebilir.

### <a name="ingress-policies"></a>Giriş ilkeleri

Time Series Insights önizlemesi aşağıdaki olay kaynaklarını destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights önizlemesi, örnek başına en fazla iki olay kaynağını destekler. Azure Time Series Insights Azure IoT Hub veya Azure Event Hubs aracılığıyla gönderilen JSON 'yi destekler.

> [!WARNING] 
> * Önizleme ortamınıza bir olay kaynağı eklerken yüksek gecikme süresi yaşayabilirsiniz. 
> Olay kaynağı gecikmesi, IoT Hub veya Olay Hub 'ınızdaki olay sayısına bağlıdır.
> * Olay kaynak verilerinin ilk kez alındıktan sonra yüksek gecikme süresi alt tarafı olur. Devam eden yüksek gecikme süresi yaşıyorsanız, lütfen Azure portal aracılığıyla bir destek bileti göndererek bizimle iletişim kurun.

## <a name="ingress-best-practices"></a>Giriş en iyi uygulamaları

Aşağıdaki en iyi yöntemleri kullanmanızı öneririz:

* Time Series Insights ve bir IoT Hub veya Olay Hub 'ını aynı bölgede yapılandırın. Bu, ağ nedeniyle oluşan gecikme süresini azaltır.
* Beklenen alma hızınızı hesaplayarak ve aşağıda listelenen desteklenen oran dahilinde olduğunu doğrulayarak ölçek gereksinimlerinizi planlayın
* Giriş [ve sorgu IÇIN JSON şeklini](./time-series-insights-update-how-to-shape-events.md)okuyarak, JSON verilerinizin ve önizleme aşamasında geçerli sınırlamaların nasıl iyileştirileceği ve şekillendirilmeli olduğunu anlayın.

### <a name="ingress-scale-and-limitations-in-preview"></a>Önizlemede giriş ölçeği ve sınırlamalar

Varsayılan olarak, önizleme ortamları **ortam başına en fazla 1 megabayt (MB/sn)** hızda giriş hızını destekler. Müşteriler, gerekirse, önizleme ortamlarını **16 MB/sn** aktarım hızına kadar ölçeklendirebilir.
Bölüm başına **0,5 MB/sn**sınırı da vardır. 

Bölüm başına sınırın IoT Hub kullanan müşterilere etkileri vardır. Özellikle, IoT Hub bir cihaz ve bölüm arasındaki benzeşim verildiğinde. Bir ağ geçidi cihazının, kendi cihaz KIMLIĞINI ve bağlantı dizesini kullanarak iletileri hub 'a iletmesi durumunda, olay yükü farklı zaman serisi kimliklerini belirtse bile, iletilerin tek bir bölüme ulaştığını belirten 0,5 MB/s sınırına ulaşmaya yönelik tehlikeniz vardır. 

Genel olarak giriş fiyatları, kuruluşunuzda bulunan cihazların sayısı, olay egörev sıklığı ve her olayın boyutu olarak görüntülenir:

*  **Cihazların sayısı** × **olay emisi sıklığı** **her olayın boyutu**.

> [!TIP]
> Bir olay kaynağı olarak IoT Hub kullanan ortamlarda, kullanımdaki veya kuruluştaki toplam cihaz yerine, kullanımdaki Hub bağlantısı sayısını kullanarak giriş oranını hesaplayın.

Üretilen iş birimleri, sınırlar ve bölümler hakkında daha fazla bilgi için:

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
> Time Series Insights gelecek genel kullanılabilirlik (GA) sürümü, verilerin olay kaynağından okunduktan sonra 60 saniye içinde kullanılabilir hale gelir. Önizleme sırasında, veriler kullanılabilir hale gelmeden önce daha uzun bir süre yaşayabilirsiniz. 60 saniyenin ötesinde önemli gecikme yaşınızı yaşıyorsanız lütfen Azure portal bir destek bileti gönderebilirsiniz.

## <a name="azure-storage"></a>Azure Depolama

Bu bölümde Azure Time Series Insights önizlemesiyle ilgili Azure depolama ayrıntıları açıklanmaktadır.

Azure Blob depolama alanının kapsamlı bir açıklaması için, [depolama Blobları giriş](../storage/blobs/storage-blobs-introduction.md)' i okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Bir Time Series Insights Preview Kullandıkça Öde ortamı oluşturduğunuzda, uzun vadeli soğuk depolduğunuz için bir Azure depolama genel amaçlı v1 blob hesabı oluşturulur.  

Time Series Insights önizlemesi, Azure Depolama hesabınızdaki her bir olayın en fazla iki kopyasını yayımlar. İlk kopyanın alma zamanına göre sıralanmış olayları vardır ve her zaman korunur, böylece bu hizmete erişmek için diğer hizmetleri kullanabilirsiniz. Ham Parquet dosyalarını işlemek için Spark, Hadoop ve diğer tanıdık araçları kullanabilirsiniz. 

Time Series Insights Preview, Time Series Insights sorgu için iyileştirmek üzere Parquet dosyalarını yeniden bölümlendirir. Verilerin yeniden bölümlenmiş kopyası da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızda süresiz olarak depolanır.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 'ları yazma ve düzenlemekle

Sorgu performansının ve veri kullanılabilirliğinin emin olmak için Time Series Insights önizlemenin oluşturduğu blob 'ları düzenlemeyin veya silmeyin.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Time Series Insights önizlemeye erişme ve verileri dışarı aktarma

Diğer hizmetlerle birlikte kullanmak üzere Time Series Insights önizleme Gezgininde görüntülenen verilere erişmek isteyebilirsiniz. Örneğin, Power BI bir rapor oluşturmak veya Azure Machine Learning Studio kullanarak makine öğrenimi modelini eğitebilmeniz için verilerinizi kullanabilirsiniz. Ya da verilerinizi Jupyıter Not defterlerinizde dönüştürmek, görselleştirmek ve modellemek için kullanabilirsiniz.

Verilerinize üç genel yolla erişebilirsiniz:

* Time Series Insights önizleme Gezgini ' nden. Verileri bir CSV dosyası olarak gezgin 'den dışarı aktarabilirsiniz. Daha fazla bilgi için [Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ni okuyun.
* Time Series Insights Preview API 'sinden. API uç noktasına `/getRecorded`adresinden ulaşabilirsiniz. Bu API hakkında daha fazla bilgi edinmek için [zaman serisi sorgusunu](./time-series-insights-update-tsq.md)okuyun.
* Doğrudan bir Azure Storage hesabından. Time Series Insights Preview verilerinize erişmek için kullandığınız hesaba okuma erişiminizin olması gerekir. Daha fazla bilgi için, [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md)konusunu okuyun.

### <a name="data-deletion"></a>Veri silme

Time Series Insights önizleme dosyalarınızı silmeyin. Yalnızca Time Series Insights Preview içinden ilgili verileri yönetin.

## <a name="parquet-file-format-and-folder-structure"></a>Parquet dosya biçimi ve klasör yapısı

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

- [Azure Time Series Insights önizleme depolama ve](./time-series-insights-update-storage-ingress.md)giriş makalesini okuyun.

- Yeni [veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.
