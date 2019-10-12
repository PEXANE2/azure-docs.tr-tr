---
title: Azure Time Series Insights Preview 'da veri depolama ve giriş Microsoft Docs
description: Azure Time Series Insights önizlemede veri depolamayı ve girişi anlama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/26/2019
ms.custom: seodec18
ms.openlocfilehash: 9af53728ee038a6511c434aeedfdb9afdab6d04b
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273877"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri depolama ve giriş

Bu makalede, Azure Time Series Insights önizlemeden veri depolama ve giriş değişiklikleri açıklanmaktadır. Temel depolama yapısını, dosya biçimini ve zaman serisi KIMLIĞI özelliğini içerir. Makalede ayrıca temel alınan giriş işlemi, üretilen iş ve sınırlamalar ele alınmaktadır.

## <a name="data-ingress"></a>Veri girişi

Azure Time Series Insights veri giriş ilkeleri, verilerin nereden ve hangi biçimde kaynak olarak yapılabileceğini belirlemek için kullanılır.

[![ zaman serisi modeline genel bakış](media/v2-update-storage-ingress/tsi-data-ingress.png)](media/v2-update-storage-ingress/tsi-data-ingress.png#lightbox)

### <a name="ingress-policies"></a>Giriş ilkeleri

Time Series Insights önizlemesi, Time Series Insights Şu anda desteklediği olay kaynaklarını ve dosya türlerini destekler:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)
  
Azure Time Series Insights Azure IoT Hub veya Azure Event Hubs aracılığıyla gönderilen JSON 'yi destekler. IoT JSON verilerinizi iyileştirmek için [JSON şeklini nasıl şekilleyeceğinizi](./time-series-insights-send-events.md#supported-json-shapes)öğrenin.

### <a name="data-storage"></a>Veri depolama

Bir Time Series Insights Preview Kullandıkça Öde SKU ortamı oluşturduğunuzda iki kaynak oluşturursunuz:

* Time Series Insights ortamı.
* Verilerin depolanacağı bir Azure depolama genel amaçlı v1 hesabı.

Time Series Insights önizlemesi, Parquet dosya türüyle Azure Blob depolamayı kullanır. Time Series Insights, Azure depolama hesabındaki blob 'lar oluşturma, dizin oluşturma ve verileri bölümleme dahil tüm veri işlemlerini yönetir. Bu Blobları bir Azure depolama hesabı kullanarak oluşturursunuz.

Diğer Azure depolama Blobları gibi Time Series Insights oluşturulan blob 'lar gibi çeşitli Tümleştirme senaryolarını desteklemek için bunları okuyup yazmanızı sağlar.

### <a name="data-availability"></a>Veri kullanılabilirliği

Time Series Insights önizlemesi, blob boyut iyileştirme stratejisi kullanarak verileri dizinler. Veriler, Dizin oluşturulduktan sonra sorgu için kullanılabilir hale gelir ve bu, ne kadar veri geldiğini, ne kadar hızlanır olduğunu temel alır.

> [!IMPORTANT]
> * Time Series Insights genel kullanılabilirlik (GA) sürümü, bir olay kaynağına ulaşan verileri 60 saniye içinde kullanılabilir hale getirir.
> * Önizleme sırasında, veriler kullanılabilir hale gelmeden önce daha uzun bir süre beklemeniz gerekir.
> * Önemli bir gecikmeyle karşılaşırsanız bizimle iletişime geçerek emin olun.

### <a name="scale"></a>Ölçeklendirme

Time Series Insights önizlemesi, ortam başına en fazla 1 mega bayt (MB/sn) kadar bir başlangıç giriş ölçeğini destekler. Gelişmiş ölçeklendirme desteği devam etmektedir. Bu geliştirmeleri yansıtacak şekilde belgelerimizi güncelleştirmeyi planlıyoruz.

## <a name="parquet-file-format"></a>Parquet dosya biçimi

Parquet, için tasarlanan sütun odaklı bir veri dosyası biçimidir:

* Birlikte çalışabilirlik
* Alan verimliliği
* Sorgu verimliliği

Time Series Insights, karmaşık verileri toplu olarak işleyebilen gelişmiş performansa sahip verimli veri sıkıştırma ve kodlama şemaları sağladığından Parquet 'i seçti.

Parquet dosya türü hakkında daha fazla bilgi için, [Parquet belgelerine](https://parquet.apache.org/documentation/latest/)başvurun.

Azure 'da Parquet dosya biçimi hakkında daha fazla bilgi için bkz. [Azure Storage 'Da desteklenen dosya türleri](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#parquet-format).

### <a name="event-structure-in-parquet"></a>Parquet 'de olay yapısı

Time Series Insights Blobların kopyalarını aşağıdaki iki biçimde oluşturur ve depolar:

1. İlk, ilk kopya varış süresine göre bölümlenmiştir:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Varış süresine göre bölümlenen blob 'lar için blob oluşturma süresi.

1. İkinci, yeniden bölümlenmiş kopya, zaman serisi KIMLIĞININ dinamik bir gruplandırmasına göre bölümlenmiştir:

    * `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Zaman serisi KIMLIĞINE göre bölümlenmiş Bloblar için blobdaki en küçük olay zaman damgası.

> [!NOTE]
> * `<YYYY>`, 4 basamaklı bir yıl gösterimine eşlenir.
> * `<MM>`, 2 basamaklı bir ay gösterimine eşlenir.
> * `<YYYYMMDDHHMMSSfff>`, 4 basamaklı yıl (`YYYY`) ile zaman damgası gösterimine eşlenir, 2 basamaklı ay (`MM`), 2 basamaklı gün (`DD`), 2 basamaklı saat (`HH`), 2 basamaklı dakika (`MM`), 2 basamaklı saniye (`SS`) ve 3 basamaklı milisaniyelik (`fff`).

Time Series Insights olaylar, aşağıdaki gibi, Parquet dosya içeriklerine eşlenir:

* Her olay tek bir satırla eşlenir.
* Bir olay zaman damgasıyla yerleşik **zaman damgası** sütunu. Timestamp özelliği hiçbir zaman null değildir. Olay kaynağında zaman damgası özelliği belirtilmemişse **olay kaynağı sıraya alınan zaman** varsayılan olarak bu olur. Zaman damgası UTC 'dir. 
* Sütunlara eşlenen diğer tüm özellikler `_string` (dize), `_bool` (Boole), `_datetime` (DateTime) ve `_double` (Double) ile biter ve özellik türüne bağlıdır.
* Bu, dosya biçiminin ilk sürümü için, **V = 1**olarak adlandırdığımız eşleme şemadır. Bu özellik geliştikçe, ad **v = 2**, **v = 3**vb. olarak artırılır.

## <a name="azure-storage"></a>Azure Depolama

Bu bölümde Azure Time Series Insights ilgili Azure depolama ayrıntıları açıklanmaktadır.

Kapsamlı bir Azure Blob depolama hizmeti açıklaması için, [depolama Blobları giriş](../storage/blobs/storage-blobs-introduction.md)' i okuyun.

### <a name="your-storage-account"></a>Depolama hesabınız

Bir Time Series Insights Kullandıkça Öde ortamı oluşturduğunuzda iki kaynak oluşturursunuz: bir Time Series Insights ortamı ve verilerin depolanacağı bir Azure depolama genel amaçlı v1 hesabı. Azure depolama genel amaçlı v1 'yi, birlikte çalışabilirlik, Fiyat ve performans nedeniyle varsayılan kaynak olarak yapmayı seçtik.

Time Series Insights, Azure Depolama hesabınızdaki her bir olayın en fazla iki kopyasını yayımlar. İlk kopya, diğer hizmetleri kullanarak hızlı bir şekilde sorgulanabilmeniz için her zaman korunur. Bu altyapılar temel dosya adı filtrelemesini destekledikleri için, ham Parquet dosyaları üzerinden zaman serisi kimlikleri genelinde Spark, Hadoop ve diğer tanıdık araçları kolayca kullanabilirsiniz. Blobları yıla ve aya göre gruplandırmak, özel bir iş için belirli bir zaman aralığı içinde Blobları listelemek için kullanışlı bir yoldur.

Ayrıca, Time Series Insights API 'Leri için iyileştirmek üzere Parquet dosyalarını yeniden bölümlendirir Time Series Insights. En son yeniden bölümlenmiş dosya da kaydedilir.

Genel Önizleme sırasında veriler Azure depolama hesabınızda süresiz olarak depolanır.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 'ları yazma ve düzenlemekle

Sorgu performansı ve veri kullanılabilirliği sağlamak için, Time Series Insights tarafından oluşturulan blob 'ları düzenlemeyin veya silmeyin.

> [!TIP]
> Blob 'ları çok sık okuyup veya yazarsanız, Time Series Insights performans olumsuz etkilenebilir.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Time Series Insights önizlemeye erişme ve verileri dışarı aktarma

Diğer hizmetlerle birlikte kullanmak üzere Time Series Insights önizleme Gezgininde depolanan verilere erişmek isteyebilirsiniz. Örneğin, Power BI raporlamak, Azure Machine Learning Studio kullanarak makine öğrenimi gerçekleştirmek veya Jupyter not defterlerine sahip bir not defteri uygulamasında kullanmak için verilerinizi kullanmak isteyebilirsiniz.

Verilerinize üç genel yolla erişebilirsiniz:

* Time Series Insights önizleme Gezgini 'nden: verileri bir CSV dosyası olarak Time Series Insights önizleme Gezgini ' nden dışarı aktarabilirsiniz. Daha fazla bilgi için [Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ne bakın.
* Time Series Insights Preview API 'Lerinde: API uç noktasına `/getRecorded` ' a ulaşılabilir. Bu API hakkında daha fazla bilgi edinmek için bkz. [zaman serisi sorgusu](./time-series-insights-update-tsq.md).
* Doğrudan bir Azure Storage hesabından (aşağıda).

#### <a name="from-an-azure-storage-account"></a>Bir Azure depolama hesabından

* Time Series Insights verilerinize erişmek için kullandığınız hesaba okuma erişiminizin olması gerekir. Daha fazla bilgi için bkz. [depolama hesabı kaynaklarınıza erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).
* Azure Blob depolama alanından verileri okumanın doğrudan yolları hakkında daha fazla bilgi için bkz. [veri aktarımı için bir Azure çözümü seçme](../storage/common/storage-choose-data-transfer-solution.md).
* Azure depolama hesabından verileri dışarı aktarmak için:
    * İlk olarak, hesabınızın verileri dışarı aktarmak için gereken gereksinimleri karşıladığından emin olun. Daha fazla bilgi için bkz. [depolama içeri ve dışarı aktarma gereksinimleri](../storage/common/storage-import-export-requirements.md).
    * Azure depolama hesabınızdan veri vermenin diğer yolları hakkında bilgi edinmek için bkz. [bloblardan veri alma ve dışarı aktarma](../storage/common/storage-import-export-data-from-blobs.md).

### <a name="data-deletion"></a>Veri silme

Blob 'ları silmeyin. Yalnızca verilerinizin kaydını denetleme ve sürdürme konusunda yararlı değildir, Time Series Insights önizlemesi her blob içinde blob meta verilerini korur.

## <a name="partitions"></a>Bölümler

Her Time Series Insights önizleme ortamında bir **zaman SERISI kimliği** özelliği ve onu benzersiz şekilde tanımlayan bir **zaman damgası** özelliği olmalıdır. Zaman serisi KIMLIĞINIZ verileriniz için bir mantıksal Bölüm görevi görür ve Time Series Insights önizleme ortamına, verileri fiziksel bölümler arasında dağıtmaya yönelik doğal bir sınır sağlar. Fiziksel bölümler, Azure depolama hesabında Time Series Insights önizlemesi tarafından yönetilir.

Time Series Insights, bölümleri bırakıp yeniden oluşturarak depolamayı ve sorgu performansını iyileştirmek için dinamik bölümlendirme kullanır. Time Series Insights Preview dinamik bölümlendirme algoritması, tek bir fiziksel bölümün birden çok, farklı, mantıksal bölüm için veri olmasına engel olmaya çalışır. Diğer bir deyişle, bölümleme algoritması, diğer zaman serisi kimlikleri ile araya girmeden, tek bir zaman serisi KIMLIĞINE özgü tüm verileri yalnızca Parquet dosyalarında yer almıyor şekilde tutar. Dinamik Bölümlendirme algoritması, olayların tek bir zaman serisi KIMLIĞI içindeki orijinal sırasını korumayı da dener.

Başlangıçta, giriş zamanında veriler zaman damgasıyla bölümlenir, böylece belirli bir zaman aralığı içindeki tek bir mantıksal bölüm birden fazla fiziksel bölüme yayılabilecek. Tek bir fiziksel bölüm ayrıca birçok veya daha fazla mantıksal bölüm içerebilir. BLOB boyutu sınırlamaları nedeniyle, en iyi bölümlendirme ile birlikte, tek bir mantıksal bölüm birden fazla fiziksel bölümden oluşabilir.

> [!NOTE]
> Varsayılan olarak, zaman damgası değeri, yapılandırılan olay kaynağınızda *sıraya alınmış bir süredir* .

Geçmiş verileri veya Batch iletilerini karşıya yüklüyorsanız, verilerinize depolamak istediğiniz değeri uygun zaman damgasıyla eşlenen zaman damgası özelliğine atayın. Timestamp özelliği büyük/küçük harfe duyarlıdır. Daha fazla bilgi için bkz. [zaman serisi modeli](./time-series-insights-update-tsm.md).

### <a name="physical-partitions"></a>Fiziksel bölümler

Fiziksel bölüm, depolama hesabınızda depolanan bir blok bloğudur. Boyut, gönderim hızına bağlı olduğundan, Blobların gerçek boyutu farklılık gösterebilir. Ancak, blob 'ların boyutunun yaklaşık 20 MB ila 50 MB olması beklenir. Bu, sorgu performansını iyileştirmek için Time Series Insights ekibinin boyut olarak 20 MB seçmesini sağlar. Bu boyut, dosya boyutuna ve veri girişi hızına bağlı olarak zaman içinde değişebilir.

> [!NOTE]
> * Bloblar 20 MB olarak boyutlandırılır.
> * Azure Blob 'ları, daha iyi performans için zaman zaman yeniden bölümlenir ve yeniden oluşturulur.
> * Ayrıca, aynı Time Series Insights verileri iki veya daha fazla blobda bulunabilir.

### <a name="logical-partitions"></a>Mantıksal bölümler

Mantıksal bölüm, tek bir bölüm anahtarı değeriyle ilişkili tüm verileri depolayan fiziksel bir bölüm içindeki bir bölümdür. Time Series Insights önizlemesi her bir Blobun iki özelliği temel alarak mantıksal olarak bölümler:

* **Zaman SERISI kimliği**: olay akışı ve model içindeki tüm Time Series Insights verileri için bölüm anahtarı.
* **Zaman damgası**: ilk giriş temelli zaman.

Time Series Insights önizlemesi, bu iki özelliği temel alan performanslı sorgular sağlar. Bu iki özellik ayrıca Time Series Insights verilerini hızlıca teslim etmek için en etkili yöntemi sağlar.

Sabit bir özellik olduğundan uygun bir zaman serisi KIMLIĞI seçmeniz önemlidir. Daha fazla bilgi için bkz. [zaman serisi kimliklerini seçme](./time-series-insights-update-how-to-id.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Time Series Insights önizleme depolama ve](./time-series-insights-update-storage-ingress.md)giriş makalesini okuyun.

- Yeni [veri modelleme](./time-series-insights-update-tsm.md)hakkında bilgi edinin.
