---
title: Depolamaya genel bakış-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights 'de veri depolama hakkında bilgi edinin.
author: esung22
ms.author: elsung
manager: diegoviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: fc6b6b42293b4f2028f1a12af950e96e28febc87
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86085574"
---
# <a name="data-storage"></a>Veri Depolama

Bir Time Series Insights *Kullandıkça Öde* (PAYG) SKU ortamı oluşturduğunuzda iki Azure kaynağı oluşturursunuz:

* Normal veri depolama için yapılandırılabilecek bir Azure Time Series Insights ortamı.
* Soğuk veri depolaması için bir Azure depolama hesabı.

Isınma deponuzdaki veriler yalnızca [zaman serisi sorgusu](./time-series-insights-update-tsq.md) ve [Azure Time Series Insights Gezgini](./time-series-insights-update-explorer.md)ile kullanılabilir. Isınma depoluıza Time Series Insights ortamı oluşturulurken seçilen [Bekletme dönemi](./time-series-insights-update-plan.md#the-preview-environment) içinde son veriler yer alacak.

Azure Time Series Insights, soğuk mağaza verilerinizi [Parquet dosya biçiminde](#parquet-file-format-and-folder-structure)Azure Blob depolamaya kaydeder. Time Series Insights bu soğuk depolama verilerini özel olarak yönetir, ancak doğrudan standart Parquet dosyaları olarak okumanız için kullanılabilir.

> [!WARNING]
> Soğuk depo verilerinin bulunduğu Azure Blob depolama hesabının sahibi olarak hesaptaki tüm verilere tam erişiminiz vardır. Bu erişim yazma ve silme izinleri içerir. Veri kaybına neden olabileceğinden yazma Azure Time Series Insights verileri düzenleme veya silme.

## <a name="data-availability"></a>Veri kullanılabilirliği

En iyi sorgu performansı için bölümler ve dizinler verileri Azure Time Series Insights. Veriler, Dizin oluşturulduktan sonra hem normal (etkinse) hem de soğuk depodan sorgu için kullanılabilir hale gelir. Alınan veri miktarı bu kullanılabilirliği etkileyebilir.

> [!IMPORTANT]
> Veriler kullanılabilir hale gelmeden önce 60 saniyeye kadar bir süre yaşayabilirsiniz. 60 saniyenin ötesinde önemli gecikme yaşınızı yaşıyorsanız lütfen Azure portal bir destek bileti gönderebilirsiniz.

## <a name="azure-storage"></a>Azure Storage

Bu bölümde Azure Time Series Insights ilgili Azure depolama ayrıntıları açıklanmaktadır.

Azure Blob depolama alanının kapsamlı bir açıklaması için, [depolama Blobları giriş](../storage/blobs/storage-blobs-introduction.md)' i okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Bir Azure Time Series Insights PAYG ortamı oluşturduğunuzda, uzun süreli soğuk depolduğunuz için bir Azure depolama hesabı oluşturulur.  

Azure Time Series Insights, Azure Depolama hesabınızdaki her bir olayın en fazla iki kopyasını tutar. Bir kopya alma zamanına göre sıralanmış olayları depolar, her zaman bir zaman sıralı dizideki olaylara erişime izin verir. Zaman içinde Time Series Insights ayrıca, performans Time Series Insights sorgusu için iyileştirmek üzere verilerin yeniden bölümlendirilmiş bir kopyasını oluşturur.

Tüm verileriniz, Azure depolama hesabınızda süresiz olarak depolanır.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 'ları yazma ve düzenlemekle

Sorgu performansının ve veri kullanılabilirliğinin emin olmak için Time Series Insights oluşturduğu blob 'ları düzenlemeyin veya silmeyin.

#### <a name="accessing-time-series-insights-cold-store-data"></a>Time Series Insights soğuk mağaza verilerine erişme

[Time Series Insights Gezgini](./time-series-insights-update-explorer.md) ve [zaman serisi sorgusundan](./time-series-insights-update-tsq.md)verilerinize erişmenin yanı sıra, verilerinize doğrudan soğuk Store 'Da depolanan Parquet dosyalarından de erişmek isteyebilirsiniz. Örneğin, bir Jupyter not defterindeki verileri okuyabilir, dönüştürebilir ve temizleyebilir, ardından Azure Machine Learning modelinizi aynı Spark iş akışında eğitebilmeniz için bunu kullanabilirsiniz.

Verilere doğrudan Azure Storage hesabınızdan erişmek için, Time Series Insights Preview verilerinizi depolamak için kullanılan hesaba okuma erişiminizin olması gerekir. Daha sonra seçili verileri, aşağıda açıklanan Parquet dosyasının oluşturulma zamanına göre, `PT=Time` [Parquet dosya biçimi](#parquet-file-format-and-folder-structure) bölümünde bulabilirsiniz.  Depolama Hesabınıza yönelik okuma erişiminin etkinleştirilmesi hakkında daha fazla bilgi için bkz. [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

#### <a name="data-deletion"></a>Veri silme

Azure Time Series Insights dosyalarınızı silmeyin. Yalnızca Azure Time Series Insights içinden ilgili verileri yönetin.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet dosya biçimi ve klasör yapısı

Parquet, verimli depolama ve performans için tasarlanan açık kaynaklı bir sütunlu dosya biçimidir. Azure Time Series Insights, ölçekte zaman serisi KIMLIĞI tabanlı sorgu performansını etkinleştirmek için Parquet kullanır.  

Parquet dosya türü hakkında daha fazla bilgi için, [Parquet belgelerini](https://parquet.apache.org/documentation/latest/)okuyun.

Azure Time Series Insights verilerinizin kopyalarını aşağıdaki gibi depolar:

* Birincisi, ilk kopya alma zamanına göre bölümlendirilir ve verileri kabaca gelişme sırasıyla depolar. Bu veriler şu klasörde bulunur `PT=Time` :

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* İkinci, yeniden bölümlenmiş kopya zaman serisi kimliklerine göre gruplandırılır ve `PT=TsId` klasöründe bulunur:

  `V=1/PT=TsId/<TSI_INTERNAL_NAME>.parquet`

Klasördeki blob adlarındaki zaman damgası, `PT=Time` olayların zaman damgasına değil, Azure Time Series Insights verilerin varış süresine karşılık gelir.

`PT=TsId`Klasördeki veriler, zaman içinde sorgu için iyileştirilebilir ve statik değildir. Yeniden bölümleme sırasında bazı olaylar birden fazla blobda bulunabilir. Bu klasördeki Blobların adlandırılmasının sağlanması garanti edilmez. 

Genel olarak, Parquet dosyaları aracılığıyla verilere doğrudan erişmeniz gerekiyorsa `PT=Time` klasörünü kullanın.  Gelecekteki işlevler, klasöre verimli erişim sağlayacak `PT=TsId` . 

> [!NOTE]
>
> * `<YYYY>`dört basamaklı bir yıl gösterimine eşlenir.
> * `<MM>`iki basamaklı bir ay gösterimiyle eşlenir.
> * `<YYYYMMDDHHMMSSfff>`dört basamaklı yıl ( `YYYY` ), iki basamaklı ay (), iki basamaklı gün (), iki basamaklı `MM` `DD` saat ( `HH` ), iki basamaklı dakika ( `MM` ), iki basamaklı saniye ( `SS` ) ve üç basamaklı milisaniyelik ( `fff` ) ile zaman damgası gösterimine eşler.

Time Series Insights Preview olayları, aşağıdaki gibi, Parquet dosya içeriklerine eşlenir:

* Her olay tek bir satırla eşlenir.
* Her satır, bir olay zaman damgasıyla **zaman damgası** sütunu içerir. Zaman damgası özelliği hiçbir zaman null değildir. Olay kaynağında zaman damgası özelliği belirtilmemişse, varsayılan olarak **olay sıraya alınan zaman** . Depolanan zaman damgası her zaman UTC olarak kullanılır.
* Her satır, Time Series Insights ortamı oluşturulduğunda tanımlanan zaman serisi KIMLIĞI (TSıD) sütununu içerir. TSıD Özellik adı, soneki içerir `_string` .
* Telemetri verileri olarak gönderilen diğer tüm özellikler, `_bool` özellik türüne bağlı olarak, (Boolean), `_datetime` (zaman damgası), `_long` (Long), ( `_double` Double), `_string` (dize) veya `dynamic` (dinamik) ile biten sütun adlarıyla eşlenir.  Daha fazla bilgi için [desteklenen veri türleri](./concepts-supported-data-types.md)hakkında bilgi edinin.
* Bu eşleme şeması, **V = 1** olarak başvurulan ve aynı ada sahip temel klasörde depolanan dosya biçiminin ilk sürümü için geçerlidir. Bu özellik geliştikçe, bu eşleme şeması değişebilir ve başvuru adı artar.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.

* [Azure Time Series Insights Preview ortamınızı](./time-series-insights-update-plan.md)planlayın.
