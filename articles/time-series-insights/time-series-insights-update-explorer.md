---
title: Önizleme gezgininde verileri görselleştir - Azure Time Series Insights | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizleme kaşifinde bulunan özellikler ve seçenekler hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: seodec18
ms.openlocfilehash: c117510749abcf997e414371faa6dea1dc79ea7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75861770"
---
# <a name="azure-time-series-insights-preview-explorer"></a>Azure Zaman Serisi Öngörüleri Önizleme kaşifi

Bu makalede, Azure Zaman Serisi Öngörüleri Önizleme [demo web uygulamasında](https://insights.timeseries.azure.com/preview/demo)bulunan çeşitli özellikler ve seçenekler açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Azure Zaman Serisi Öngörüleri Önizleme gezginine başlamak için şunları

* Bir Time Series Insights ortamının sağlanmasını sağlar. [Azure Zaman Serisi Öngörüleri Önizleme](./time-series-insights-update-create-environment.md) öğreticisini okuyarak bir örneği sağlama hakkında daha fazla bilgi edinin.
* Hesap için oluşturduğunuz Time Series Insights ortamına [veri erişimi sağlayın.](./time-series-insights-data-access.md) Başkalarına ve kendinize erişim sağlayabilirsiniz.
* Verileri ortama itmek için Zaman Serisi Öngörüleri ortamına bir olay kaynağı ekleyin:
  * [Etkinlik merkezine nasıl bağlanırılamayı](./time-series-insights-how-to-add-an-event-source-eventhub.md) öğrenin 
  * [Bir IoT hub'ına nasıl bağlanılalıyorum](./time-series-insights-how-to-add-an-event-source-iothub.md) öğrenin

## <a name="explore-the-time-series-insights-preview-explorer"></a>Zaman Serisi Öngörüleri Önizleme kaşifini keşfedin

Azure Zaman Serisi Öngörüleri Önizleme gezgini aşağıdaki yedi öğeden oluşur:

[![Zaman Serisi Öngörüleri Önizleme explorer'a genel bakış](media/v2-update-explorer/preview-explorer-overview.png)](media/v2-update-explorer/preview-explorer-overview.png#lightbox)

1. [Çevre paneli](#1-environment-panel): Tüm Azure Time Series Öngörüleri ortamlarınızı görüntüler.
1. [Gezinme çubuğu](#2-navigation-bar): **Çözümle** ve **Model** sayfaları arasında geçiş yapmanızı sağlar.
1. [Hiyerarşi ağacı ve arama paneli](#3-hierarchy-tree-and-search-panel): Grafiklenecek belirli veri öğelerini seçmenizi ve aramanızı sağlar.
1. [Zaman serisi iyi](#4-time-series-well): Şu anda seçtiğiniz tüm veri öğelerini gösterir.
1. [Grafik paneli](#5-chart-panel): Geçerli çalışma grafiğinizi görüntüler.
1. [Zaman Çizelgesi](#6-time-editor-panel): Çalışma sürenizi değiştirmenizi sağlar.
1. [Uygulama çubuğu](#7-app-bar): Kullanıcı yönetim seçeneklerinizi (geçerli kiracı gibi) içerir ve bunları ve dil ayarlarını değiştirmenize olanak tanır.


## <a name="1-environment-panel"></a>1. Çevre paneli

Ortam paneli, erişebildiğiniz tüm Time Series Insights ortamlarını görüntüler. Liste, istediğiniz kadar öde (Önizleme) ortamlarının yanı sıra S1/S2 ortamlarını (Genel Kullanılabilirlik) içerir. Hemen oraya götürülmek için kullanmak istediğiniz Zaman Serisi Öngörüleri ortamını seçmeniz yeterlidir.

1. Görüntülenen ortamınızın yanındaki açılır ok'u seçin.

   [![Çevre paneli](media/v2-update-explorer/environment-panel.png)](media/v2-update-explorer/environment-panel.png#lightbox)

1. Ardından, istediğiniz ortamı seçin.

## <a name="2-navigation-bar"></a>2. Navigasyon çubuğu

  [![Gezinti çubuğu](media/v2-update-explorer/tsi-preview-navigation-bar.png)](media/v2-update-explorer/tsi-preview-navigation-bar.png#lightbox)

İki görünüm arasında seçim yapmak için gezinti çubuğunu kullanın:

* **Analiz**: Modellenmiş veya modellenmemiş zaman serisi verilerinizde zengin analizleri grafiklemek ve gerçekleştirmek için kullanın.
* **Model**: Zaman Serisi Öngörüleri modelinize yeni Zaman Serisi Öngörüleri Önizleme türleri, hiyerarşiler ve örnekler itmek için kullanın.

### <a name="model-authoring"></a>Model yazma

Azure Zaman Serisi Öngörüleri Önizleme, Zaman Serisi Modelinizde tam oluşturma, okuma, güncelleme ve silme (CRUD) işlemlerini destekler.

[![Model arama paneli](media/v2-update-explorer/model-search-panel.png)](media/v2-update-explorer/model-search-panel.png#lightbox)

* **Zaman Serisi Model türü**: Hesaplama yapmak için değişkenleri veya formülleri tanımlamak için Time Series Insights türlerini kullanabilirsiniz. Belirli bir Zaman Serisi Öngörüleri örneğiyle ilişkilidirler. Bir türbir veya daha fazla değişken olabilir.
* **Zaman Serisi Model hiyerarşisi**: Hiyerarşiler verilerinizin sistematik organizasyonlarıdır. Hiyerarşiler, Time Series Insights verilerinizde farklı varlıklar arasındaki ilişkileri betimlenir.
* **Zaman Serisi Model örneği**: Örnekler zaman serilerinin ta kendisidir. Çoğu durumda, bunlar **deviceid** veya **AssetID**'dir , bu da varlığın ortamdaki benzersiz tanımlayıcısıdır.

Zaman Serisi Modeli hakkında daha fazla bilgi edinmek için [Times Series Modelleri'ni](./time-series-insights-update-tsm.md)okuyun.

## <a name="3-hierarchy-tree-and-search-panel"></a>3. Hiyerarşi ağacı ve arama paneli

Hiyerarşi ağacı ve arama paneli, grafiğinizde görüntülemek istediğiniz belirli zaman serisi örneklerini bulmak için [Zaman Serisi Model](./time-series-insights-update-tsm.md) hiyerarşinizde kolayca arama ve gezinme olanağı sağlar. Örneklerinizi seçtiğinizde, bunlar yalnızca geçerli grafiğe eklenmez, aynı zamanda verilere de eklenir. 

[![Hiyerarşi ağacı ve arama paneli](media/v2-update-explorer/tsi-preview-explorer-hierarchy-search.png)](media/v2-update-explorer/tsi-preview-explorer-hierarchy-search.png#lightbox)

Arama sonuçları bölmesi, sonuçlarınızı bir hiyerarşi görünümünde veya liste görünümünde görüntülemenize olanak sağlayarak görüntülemek istediğiniz örnekleri bulmanızı kolaylaştırır.
 
## <a name="4-time-series-well"></a>4. Zaman serisi iyi

Kuyu, örnek alanlarını ve seçili Time Series Öngörüleri örnekleriyle ilişkili diğer meta verileri görüntüler. Sağ taraftaki onay kutularını seçerek, geçerli grafikten belirli örnekleri gizleyebilir veya görüntüleyebilirsiniz. 

  [![Önizleme iyi](media/v2-update-explorer/tsi-preview-ui-explorer-well.png)](media/v2-update-explorer/tsi-preview-ui-explorer-well.png#lightbox)

Öğenin sol tarafındaki kırmızı **Sil** (çöp kutusu) denetimini seçerek geçerli verilerinizden belirli veri öğelerini kaldırabilirsiniz. Kuyu ayrıca her öğenin grafikte nasıl görüntüleneceğini denetlemenize olanak tanır. Min/max gölgeler, veri noktaları eklemeyi, öğeyi zaman içinde kaydırmayı ve örneği adım adımlı bir şekilde görselleştirmeyi seçebilirsiniz. 

Ayrıca, keşifler kontrolü kolayca zaman kaymaları ve dağılım arsalar oluşturmanıza olanak sağlar.  

  [![Iyi düzen seçenekleri](media/v2-update-explorer/well-layout-options.png)](media/v2-update-explorer/well-layout-options.png#lightbox)

> [!NOTE]
> Aşağıdaki ileti görünürse, örnekte seçilen zaman aralığı nda veri yoktur. Sorunu gidermek için, süreyi artırın veya örneğin verileri ittiğini onaylayın.
>
> ![Veri bildirimi yok](media/v2-update-explorer/tsi-preview-no-data-warning.png)

## <a name="5-chart-panel"></a>5. Grafik paneli

Grafik, zaman serisi örneklerini satır olarak görüntülemenizi sağlar. Grafiği büyütmek için web denetimlerini tıklatarak ortam panelini, veri modelini ve zaman aralığı denetim panelini daraltabilirsiniz. 

  [![Önizleme grafiğine genel bakış](media/v2-update-explorer/tsi-preview-chart-overview.png)](media/v2-update-explorer/tsi-preview-chart-overview.png#lightbox)

1. **Grafik türü**: Görselleştirme için hangi veri elemanlarının kullanılabilerini denetler.

1. **Aralık boyutu**: Interval boyutu kaydırıcı aracı, aynı zaman aralığında aralıkları yakınlaştırmanızı ve uzaklaştırmanızı sağlar. Bu, düzgün eğilimleri milisaniye kadar küçük dilimlere kadar gösteren büyük zaman dilimleri arasındaki hareketin daha hassas bir şekilde kontrol altına alet edilerek verilerinizin parçalı, yüksek çözünürlüklü kesimlerini gözden geçirmenize olanak sağlar. Kaydırıcının varsayılan başlangıç noktası, seçiminizdeki verilerin en uygun görünümü olarak ayarlanır; çözünürlük, sorgu hızı ve parçalılık dengeleme.

1. **Yakınlaştırma ve kaydırma**: Grafiği yakınlaştırmak ve kaydırmak için bu denetimi seçin.

1. **Y ekseni denetimi**: Kullanılabilir y ekseni görünüm seçenekleri ile döngüler:

    * `Stacked`: Her satırın ayrı bir Y ekseni vardır.
    * `Overlap`: Y ekseni verisi seçili çizgiye göre değişirken, aynı Y ekseninde birden çok satırı yığına bürünmek için kullanın.
    * `Shared`: Tüm Y ekseni verileri birlikte görüntülenir.

1. **İşaretleyici öğesi**: Şu anda seçili veri öğesi ve ilişkili ayrıntıları.

Fareyi basılı tutarken geçerli grafikteki bir veri noktasını **sol** tıklatarak ve seçili alanı seçtiğiniz bitiş noktasına sürükleyerek belirli bir veri dilimini daha da delebilirsiniz. Mavi, seçili alana **sağ tıklayın** ve aşağıda gösterildiği gibi **Yakınlaştır'ı** seçin. Ayrıca seçili zaman bölmesinde telemetri olaylarını görüntüleyebilir ve indirebilirsiniz.

  [![Grafik yakınlaştırmayı önizleme](media/v2-update-explorer/preview-chart-zoom.png)](media/v2-update-explorer/preview-chart-zoom.png#lightbox)

**Yakınlaştırma** eylemini gerçekleştirmenizden sonra, seçtiğiniz veri kümesi görüntülenir. Time Series Insights verilerinizin üç y ekseni gösterimi arasında geçiş yapmak için biçim denetimini seçin.

  [![Önizleme grafiği y ekseni](media/v2-update-explorer/tsi-preview-explorer-standard-chart.png)](media/v2-update-explorer/tsi-preview-explorer-standard-chart.png#lightbox)

Burada, **çakışan** bir grafik örneği verilmiştir:

  [![Çakışan grafik seçeneği](media/v2-update-explorer/tsi-preview-explorer-overlapping-chart.png)](media/v2-update-explorer/tsi-preview-explorer-overlapping-chart.png#lightbox)

**Daha fazla eylem** düğmesi **CSV olarak İndir**görüntülemek için genişletir , Güç BI **bağlanın**, **tablo olarak grafik verilerini göster**, ve ham **olayları keşfedin** seçenekleri.

  [![Daha fazla eylem seçeneği](media/v2-update-explorer/more-actions-icon.png)](media/v2-update-explorer/more-actions-icon.png#lightbox)

[Time Series Insights yerel Power BI konektöründe](concepts-power-bi.md) **Power BI'ye Bağlan** seçeneği hakkında daha fazla bilgi edinin.

## <a name="6-time-editor-panel"></a>6. Zaman editörü paneli

Time Series Insights ile çalışırken önce bir zaman aralığı seçeceksiniz. Seçili zaman aralığı, Time Series Insights güncelleştirme widget'ları ile manipülasyon alabilen veri kümesini denetler.

  [![Zaman seçim paneli](media/v2-update-explorer/tsi-preview-explorer-timeline-element.png)](media/v2-update-explorer/tsi-preview-explorer-timeline-element.png#lightbox)

> [!TIP]
> Zaman çizelgesinin bir bölümü, sıcak depoda bulunan verilerin açıkliğini belirtmek için kehribar veya turuncu renkte vurgulanır.

Çalışma sürenizi seçmek için Time Series Insights güncellemesinde aşağıdaki web denetimleri mevcuttur. 

  [![Keşif iyi kontrol](media/v2-update-explorer/exploration-well-control.png)](media/v2-update-explorer/exploration-well-control.png#lightbox)

1. **İç tarih aralığı kaydırıcı sıcağı denetimi**: İki uç nokta denetimini istediğiniz zaman aralığında sürükleyerek kullanın. Bu iç tarih aralığı dış tarih aralığı kaydırıcı kontrolü ile sınırlandırılmıştır.

1. **Tarih aralığı düğmelerini artırın ve azaltın**: İstediğiniz aralık için düğmelerden birini seçerek zaman aralığınızı artırın veya azaltın.

1. **Zaman aralığı daraltma kontrolü**: Bu web denetimi, iç tarih aralığı kaydırıcı aracı dışında tüm denetimleri gizlemenizi sağlar.

1. **Dış tarih aralığı kaydırıcı sıcası denetimi**: İç tarih aralığı denetiminiz için kullanılabilecek dış tarih aralığını seçmek için bitiş noktası denetimlerini kullanın.

1. **Zaman aralığı kaydırıcı sıcağı denetimi**: Son **30 dakika,** **son 12 saat**veya **özel**aralık gibi önceden ayarlanmış zaman aralığı seçimleri arasında hızlı bir şekilde geçiş yapmak için kullanın. Bu değeri değiştirmek, aralık boyutu kaydırıcı aracında tartışılan kullanılabilir aralık aralıklarını da değiştirir.

   [![Seçim paneline ve seçim panelinden](media/v2-update-explorer/to-and-from-element.png)](media/v2-update-explorer/to-and-from-element.png#lightbox)

## <a name="7-app-bar"></a>7. Uygulama çubuğu

Time Series Insights Preview navigasyon paneli, Time Series Insights uygulamanızın en üstünde görünür. Aşağıdaki işlevleri sağlar:

### <a name="current-session-share-link-control"></a>Geçerli oturum paylaşım bağlantısı denetimi

  [![Simgeyi paylaş](media/v2-update-explorer/tsi-preview-explorer-share-icon.png)](media/v2-update-explorer/tsi-preview-explorer-share-icon.png#lightbox)

Ekibinizle bir URL bağlantısını paylaşmak için yeni **Paylaş** simgesini seçin.

  [![Örnek URL'nizi paylaşın](media/v2-update-explorer/tsi-preview-explorer-share-your-view.png)](media/v2-update-explorer/tsi-preview-explorer-share-your-view.png#lightbox)

### <a name="tenant-section"></a>Kiracı bölümü

  [![Kiracı seçimi](media/v2-update-explorer/tsi-preview-explorer-tenant-selection.png)](media/v2-update-explorer/tsi-preview-explorer-tenant-selection.png#lightbox)

* Geçerli Time Series Insights oturum açma hesap bilgilerinizi görüntüler.
* Kullanılabilir Time Series Insights temaları arasında geçiş yapmak için kullanın.
* Önizleme demo web [uygulamasını](https://insights.timeseries.azure.com/preview/demo)görüntülemek için kullanın.

### <a name="theme-selection"></a>Tema seçimi

Yeni bir tema seçmek için sağ üst köşede bulunan profil simgenizi seçin. Ardından, **Tema Değiştir'i**seçin.

  [![Tema seçimi](media/v2-update-explorer/tsi-preview-theme-selection.png)](media/v2-update-explorer/tsi-preview-theme-selection.png#lightbox)

> [!TIP]
> Profil simgenizi seçerek dil seçimi de kullanılabilir.

Azure Zaman Serisi Öngörüleri Önizleme seçimi iki temanı destekler:

* **Işık teması**: Bu belge boyunca gösterilen varsayılan tema.
* **Karanlık tema**: Burada gösterildiği gibi explorer işler:

  [![Seçilen karanlık tema](media/v2-update-explorer/tsi-preview-dark-theme-selected.png)](media/v2-update-explorer/tsi-preview-dark-theme-selected.png#lightbox)

## <a name="s1s2-environment-controls"></a>S1/S2 ortam kontrolleri

### <a name="preview-terms-panel"></a>Önizleme terimleri paneli

Bu bölüm yalnızca güncelleştirilmiş kullanıcı arabirimi'nde gezgini kullanmaya çalışan varolan S1/S2 ortamları için geçerlidir. Genel olarak kullanılabilen ürünü ve Önizleme'yi birlikte kullanmak isteyebilirsiniz. Güncelleştirilmiş gezgine mevcut Kullanıcı Arabirimi'nden bazı işlevler ekledik, ancak mevcut Zaman Serisi Öngörüleri gezgininde S1/S2 ortamı için tam kullanıcı arabirimi deneyimini elde edebilirsiniz. 

Hiyerarşi yerine, Zaman Serisi Öngörüler terimleri paneli görüntülenir. Terimler paneli, ortamınızdaki sorguları tanımlamanıza olanak tanır. Bir yükleme dayalı verileri filtrelemek için de kullanın.

  [![Nerede sorgu paneli](media/v2-update-explorer/s1-s2-preview-query.png)](media/v2-update-explorer/s1-s2-preview-query.png#lightbox)

Zaman Serisi Öngörüler Önizleme terimleri düzenleyici paneli aşağıdaki parametreleri alır:

**Where**: Aşağıdaki tabloda listelenen operands kümesini kullanarak olaylarınızı hızlı bir şekilde filtrelemek için where yan tümcesini kullanın. Bir operand seçerek bir arama yaparsanız, yüklem bu aramaya göre otomatik olarak güncelleştirilir. Desteklenen operand türleri şunlardır:

| İşlem | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Çift, Datetime, Timespan | |
| `=`, `!=`, `<>` | String, Bool, Çift, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Çift, DateTime, TimeSpan, NULL | Tüm operands aynı tip veya NULL sabit olmalıdır. |
| `HAS` | Dize | Sadece sabit dize literals sağ tarafta izin verilir. Boş dize ve NULL izin verilmez. |

Desteklenen sorgu işlemleri ve veri türleri hakkında daha fazla bilgi edinmek için [Zaman Serisi İfadesi'ni (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)okuyun.

### <a name="examples-of-where-clauses"></a>Nerede yan tümceörnekleri

  [![Nerede yan tümce örnekleri](media/v2-update-explorer/tsi-preview-example-queries.png)](media/v2-update-explorer/tsi-preview-example-queries.png#lightbox)

**Ölçü**: Geçerli grafiğiniz için öğe olarak kullanabileceğiniz tüm sayısal**sütunları (Doubles)** görüntüleyen açılır liste.

**Split by**: Bu açılır liste, verilerinizi gruplaştırabileceğiniz modelinizde bulunan tüm kategorik sütunları (Strings) görüntüler. Aynı x ekseninde görüntülemek için en fazla beş terim ekleyebilirsiniz. İstediğiniz parametreleri girin ve sonra yeni bir terim eklemek için **Ekle'yi** seçin.

  [![Sorgulanan ve filtre uygulanmış görünüm](media/v2-update-explorer/s1-s2-preview-filtered-view.png)](media/v2-update-explorer/s1-s2-preview-filtered-view.png#lightbox)

Aşağıdaki resimde gösterildiği gibi görünür simgeyi seçerek grafik panelindeki öğeleri gösterip gizleyebilirsiniz. Sorguları tamamen kaldırmak için kırmızı **X'i**seçin.

  [![Sorgulanan ve filtre uygulanmış bir seçeneği iptal etme](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png)](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Zaman Serisi Öngörüleri Önizlemesinde [depolama ve giriş](./time-series-insights-update-storage-ingress.md) hakkında bilgi edinin.

- Veri modelleme ile ilgili Zaman Serisi Öngörüleri Önizleme [belgesini](./time-series-insights-update-tsm.md)okuyun.

- Zaman Serisi Öngörüleri örneğini [nasıl tanılayıp sorun gidereceklerini](./time-series-insights-update-how-to-troubleshoot.md) öğrenin.
