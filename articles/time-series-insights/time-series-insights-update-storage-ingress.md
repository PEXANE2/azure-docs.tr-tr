---
title: Azure Time Series Insights Preview 'da veri depolama ve giriş Microsoft Docs
description: Azure Time Series Insights önizlemede veri depolamayı ve girişi anlama.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/23/2019
ms.custom: seodec18
ms.openlocfilehash: 0b61e194bdea5fd8272ffc0fc9e16a2d80d3cf60
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72989702"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri depolama ve giriş

Bu makalede Azure Time Series Insights önizlemesi için veri depolama ve giriş güncelleştirmeleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve zaman serisi KIMLIĞI özelliğini içerir. Ayrıca temel alınan giriş işlemini, üretilen işi ve sınırlamaları da açıklar.

## <a name="data-ingress"></a>Veri girişi

Time Series Insights önizlemede, veri giriş ilkeleri verilerin nereden kaynakta yapılabileceğini ve verilerin ne biçimde olması gerektiğini tespit edebilir.

[![zaman serisi modeline genel bakış](media/v2-update-storage-ingress/tsi-data-ingress.png)](media/v2-update-storage-ingress/tsi-data-ingress.png#lightbox)

### <a name="ingress-policies"></a>Giriş ilkeleri

Time Series Insights önizlemesi, Time Series Insights Şu anda desteklediği olay kaynaklarını destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights önizlemesi, örnek başına en fazla iki olay kaynağını destekler.
  
Azure Time Series Insights Azure IoT Hub veya Azure Event Hubs aracılığıyla gönderilen JSON 'yi destekler. IoT JSON verilerinizi iyileştirmek için [JSON şeklini nasıl şekilleyeceğinizi](./time-series-insights-send-events.md#supported-json-shapes)öğrenin.

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
> Time Series Insights genel kullanılabilirlik (GA) sürümü, verilerin olay kaynağından okunduktan sonra 60 saniye içinde kullanılabilir hale gelir. Önizleme sırasında, veriler kullanılabilir hale gelmeden önce daha uzun bir süre yaşayabilirsiniz. 60 saniyenin ötesinde önemli gecikme yaşınızı yaşıyorsanız lütfen bizimle iletişime geçin.

### <a name="scale"></a>Ölçeklendirme

Time Series Insights önizleme, varsayılan olarak, ortam başına en fazla 1 megabayt (MB/sn) kadar bir başlangıç giriş ölçeğini destekler. İhtiyacınız varsa 16 MB/sn 'ye kadar aktarım hızı kullanılabilir. Gelişmiş ölçeklendirme desteğine ihtiyacınız varsa lütfen bizimle iletişime geçin.

Olay kaynağı için ek giriş ve ölçeklendirme özellikleri edinebilirsiniz:

* [IoT Hub’ı](../iot-hub/iot-hub-scaling.md)
* [Event Hubs](../event-hubs/event-hubs-scalability.md)

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

* Time Series Insights önizleme Gezgini ' nden. Verileri bir CSV dosyası olarak gezgin 'den dışarı aktarabilirsiniz. Daha fazla bilgi için [Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ne bakın.
* Time Series Insights Preview API 'sinden. API uç noktasına `/getRecorded`adresinden ulaşabilirsiniz. Bu API hakkında daha fazla bilgi edinmek için bkz. [zaman serisi sorgusu](./time-series-insights-update-tsq.md).
* Doğrudan bir Azure Storage hesabından. Time Series Insights Preview verilerinize erişmek için kullandığınız hesaba okuma erişiminizin olması gerekir. Daha fazla bilgi için bkz. [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

### <a name="data-deletion"></a>Veri silme

Time Series Insights önizleme dosyalarınızı silmeyin. İlgili verileri yalnızca Time Series Insights Preview içinden yönetmeniz gerekir.

## <a name="parquet-file-format-and-folder-structure"></a>Parquet dosya biçimi ve klasör yapısı

Parquet, verimli depolama ve performans için tasarlanan açık kaynaklı bir sütunlu dosya biçimidir. Time Series Insights önizleme, bu nedenlerle Parquet kullanır. Veri, ölçek sırasında sorgu performansı için zaman serisi KIMLIĞINE göre verileri bölümler.  

Parquet dosya türü hakkında daha fazla bilgi için, bkz. [Parquet belgeleri](https://parquet.apache.org/documentation/latest/).

Time Series Insights önizlemesi verilerinizin kopyalarını aşağıdaki gibi depolar:

* Birincisi, ilk kopya alma zamanına göre bölümlendirilir ve verileri kabaca gelişme sırasıyla depolar. Veriler `PT=Time` klasöründe bulunur:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* İkinci, yeniden bölümlenmiş kopya, zaman serisi kimliklerinin gruplandırılmasıyla bölümlenir ve `PT=TsId` klasöründe bulunur:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

Her iki durumda da, zaman değerleri blob oluşturulma zamanına karşılık gelir. `PT=Time` klasöründeki veriler korunur. `PT=TsId` klasördeki veriler, zaman içinde sorgu için iyileştirilebilir ve statik olarak kalmaz.

> [!NOTE]
> * `<YYYY>` dört basamaklı bir yıl gösterimine eşlenir.
> * `<MM>` iki basamaklı bir ay gösterimiyle eşlenir.
> * `<YYYYMMDDHHMMSSfff>`, dört basamaklı yıl (`YYYY`), iki basamaklı ay (`MM`), iki basamaklı gün (`DD`), iki basamaklı bir saat (`HH`), iki basamaklı dakika (`MM`), iki basamaklı saniye (`SS`) ve üç basamaklı milisaniyeye sahip zaman damgası gösterimine eşlenir (`fff`).

Time Series Insights Preview olayları, aşağıdaki gibi, Parquet dosya içeriklerine eşlenir:

* Her olay tek bir satırla eşlenir.
* Her satır, bir olay zaman damgasıyla **zaman damgası** sütunu içerir. Zaman damgası özelliği hiçbir zaman null değildir. Olay kaynağında zaman damgası özelliği belirtilmemişse, **olay sıraya alınan zamanı** varsayılan olarak belirler. Zaman damgası her zaman UTC 'dir.
* Her satır, Time Series Insights ortamı oluşturulduğunda tanımlanan zaman serisi ID sütununu içerir. Özellik adı `_string` sonekini içerir.
* Telemetri verileri olarak gönderilen diğer tüm özellikler, özellik türüne bağlı olarak `_string` (dize), `_bool` (Boolean), `_datetime` (DateTime) veya `_double` (Double) ile biten sütun adlarıyla eşlenir.
* Bu eşleme düzeni, **V = 1**olarak başvurulan dosya biçiminin ilk sürümü için geçerlidir. Bu özellik geliştikçe, ad arttırılabilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Time Series Insights önizleme depolama ve](./time-series-insights-update-storage-ingress.md)giriş makalesini okuyun.

- Yeni [veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.
