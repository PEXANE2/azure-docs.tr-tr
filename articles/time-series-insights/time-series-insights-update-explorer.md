---
title: Önizleme Gezgini 'nde verileri görselleştirme-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizleme Gezgini ' nde kullanılabilen özellikler ve seçenekler hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: seodec18
ms.openlocfilehash: 06f235b7f1c20681beee1f5be4c18de60d53a690
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037316"
---
# <a name="azure-time-series-insights-preview-explorer"></a>Azure Time Series Insights önizlemesi Gezgini

Bu makalede Azure Time Series Insights Preview [demo Web uygulamasında](https://insights.timeseries.azure.com/preview/demo)kullanılabilen çeşitli özellikler ve seçenekler açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Azure Time Series Insights önizleme Gezginini kullanmaya başlamak için şunları yapmanız gerekir:

* Sağlanmış bir Time Series Insights ortamı vardır. [Azure Time Series Insights önizleme](./time-series-insights-update-create-environment.md) öğreticisini okuyarak bir örnek sağlama hakkında daha fazla bilgi edinin.
* Hesap için oluşturduğunuz Time Series Insights ortamına [veri erişimi sağlar](./time-series-insights-data-access.md) . Başkalarının yanı sıra kendinize de erişim sağlayabilirsiniz.
* Ortama veri göndermek için Time Series Insights ortamına bir olay kaynağı ekleyin:
  * [Bir olay hub 'ına nasıl bağlanacağınızı](./time-series-insights-how-to-add-an-event-source-eventhub.md) öğrenin 
  * [IoT Hub 'ına nasıl bağlanacağınızı](./time-series-insights-how-to-add-an-event-source-iothub.md) öğrenin

## <a name="explore-the-time-series-insights-preview-explorer"></a>Time Series Insights önizleme Gezginini keşfet

Azure Time Series Insights önizleme Gezgini aşağıdaki yedi öğeden oluşur:

[![Time Series Insights Preview Explorer 'a genel bakış](media/v2-update-explorer/preview-explorer-overview.png)](media/v2-update-explorer/preview-explorer-overview.png#lightbox)

1. [Ortam paneli](#1-environment-panel): tüm Azure Time Series Insights ortamlarınızı görüntüler.
1. [Gezinti çubuğu](#2-navigation-bar): **analiz** ve **model** sayfaları arasında geçiş yapmanızı sağlar.
1. [Hiyerarşi ağacı ve arama paneli](#3-hierarchy-tree-and-search-panel): grafiklenen belirli veri öğelerini seçmenizi ve aramanızı sağlar.
1. [Zaman serisi iyi](#4-time-series-well): Şu anda seçili olan tüm veri öğelerinizi gösterir.
1. [Grafik paneli](#5-chart-panel): geçerli çalışma grafiğinizi görüntüler.
1. [Zaman çizelgesi](#6-time-editor-panel): çalışma süresi yayılımını değiştirmenize izin verir.
1. [Uygulama çubuğu](#7-app-bar): Kullanıcı yönetim seçeneklerinizi (geçerli kiracı gibi) içerir ve bunları ve dil ayarlarını değiştirmenize izin verir.


## <a name="1-environment-panel"></a>1. ortam paneli

Ortam paneli, erişiminiz olan tüm Time Series Insights ortamları görüntüler. Liste, Kullandıkça Öde (Önizleme) ortamlarının yanı sıra S1/S2 ortamlarını (genel kullanılabilirlik) içerir. Hemen burada almak için kullanmak istediğiniz Time Series Insights ortamı seçmeniz yeterlidir.

1. Görüntülenmiş ortamınızın yanındaki aşağı açılan oku seçin.

   [![Ortam paneli](media/v2-update-explorer/environment-panel.png)](media/v2-update-explorer/environment-panel.png#lightbox)

1. Ardından istediğiniz ortamı seçin.

## <a name="2-navigation-bar"></a>2. gezinti çubuğu

  [![Gezinti çubuğu](media/v2-update-explorer/tsi-preview-navigation-bar.png)](media/v2-update-explorer/tsi-preview-navigation-bar.png#lightbox)

İki görünüm arasından seçim yapmak için gezinti çubuğunu kullanın:

* **Çözümle**: modellenen veya Modellenmemiş zaman serisi verilerinizde zengin analizler gerçekleştirmek için bunu kullanın.
* **Model**: yeni Time Series Insights önizleme türlerini, hiyerarşileri ve örnekleri Time Series Insights modelinize göndermek için bunu kullanın.

### <a name="model-authoring"></a>Model yazma

Azure Time Series Insights önizlemesi, zaman serisi modelinizdeki tam oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini destekler.

[![Model arama paneli](media/v2-update-explorer/model-search-panel.png)](media/v2-update-explorer/model-search-panel.png#lightbox)

* **Zaman serisi model türü**: hesaplamalar yapmak için değişkenler veya formüller tanımlamak üzere Time Series Insights türlerini kullanabilirsiniz. Bunlar, belirli bir Time Series Insights örneğiyle ilişkilendirilir. Bir tür bir veya daha fazla değişkene sahip olabilir.
* **Zaman serisi model hiyerarşisi**: hiyerarşiler verilerinizin sistematik kuruluşlardır. Hiyerarşiler Time Series Insights verilerinizde farklı varlıklar arasındaki ilişkileri düzenler.
* **Zaman serisi model örneği**: örnekler zaman serisidir. Çoğu durumda, bu, ortamdaki varlığın benzersiz tanımlayıcısı olan **DeviceID** veya **AssetID**.

Zaman serisi modeli hakkında daha fazla bilgi edinmek için okuma [zamanları dizi modelleri](./concepts-model-overview.md).

## <a name="3-hierarchy-tree-and-search-panel"></a>3. hiyerarşi ağacı ve arama paneli

Hiyerarşi ağacı ve arama paneli, grafiğinizde görüntülenmesini istediğiniz belirli zaman serisi örneklerini bulmak için [zaman serisi modeli](./concepts-model-overview.md) hiyerarşinizde kolayca arama yapmanıza ve gezinmenize izin verir. Örneklerinizi seçtiğinizde, bunlar yalnızca geçerli grafiğe eklenmez, ancak veri kutusuna da eklenir. 

[![Hiyerarşi ağacı ve arama paneli](media/v2-update-explorer/tsi-preview-explorer-hierarchy-search.png)](media/v2-update-explorer/tsi-preview-explorer-hierarchy-search.png#lightbox)

Arama sonuçları bölmesi, göstermek istediğiniz örnekleri bulmayı kolaylaştıran bir hiyerarşi görünümünde veya liste görünümünde sonuçlarınızı görüntülemenize imkan tanır.
 
## <a name="4-time-series-well"></a>4. zaman serisi iyi

İyi, örnek alanlarını ve seçili Time Series Insights örneklerle ilişkili diğer meta verileri görüntüler. Sağ taraftaki onay kutularını seçerek geçerli grafikten belirli örnekleri gizleyebilir veya görüntüleyebilirsiniz. 

  [![Önizleme kutusu](media/v2-update-explorer/tsi-preview-ui-explorer-well.png)](media/v2-update-explorer/tsi-preview-ui-explorer-well.png#lightbox)

Öğenin sol tarafındaki kırmızı **silme** (çöp kutusu) denetimini seçerek, geçerli verilerdeki belirli veri öğelerini kaldırabilirsiniz. Ayrıca, her bir öğenin grafikte nasıl görüntülendiğini denetlemenize de olanak tanır. En az/en yüksek gölgeler, veri noktaları ekleyebilir, öğeyi zamanında kaydırabileceğiniz örnek bir şekilde görselleştirmeyi seçebilirsiniz. 

Ayrıca, araştırma denetimi zaman vardiyalarını ve dağılım çizimlerini kolayca oluşturmanızı sağlar.  

  [![İyi düzen seçenekleri](media/v2-update-explorer/well-layout-options.png)](media/v2-update-explorer/well-layout-options.png#lightbox)

> [!NOTE]
> Aşağıdaki ileti görüntülenirse, örnek seçilen zaman aralığı boyunca hiçbir veri içermez. Sorunu çözmek için, zaman aralığını artırın veya örneğin veri iletme süresini onaylayın.
>
> ![Veri bildirimi yok](media/v2-update-explorer/tsi-preview-no-data-warning.png)

## <a name="5-chart-panel"></a>5. Grafik bölmesi

Grafik, zaman serisi örneklerini satır olarak görüntülemenizi sağlar. Grafiği daha büyük hale getirmek için Web denetimlerine tıklayarak ortam paneli, veri modeli ve zaman aralığı denetim masasını daraltabilirsiniz. 

  [![Önizleme grafiğine genel bakış](media/v2-update-explorer/tsi-preview-chart-overview.png)](media/v2-update-explorer/tsi-preview-chart-overview.png#lightbox)

1. **Grafik türü**: görselleştirme için hangi veri öğelerinin kullanılabilir olduğunu denetler.

1. **Aralık boyutu**: Aralık boyutu kaydırıcı Aracı, aynı zaman dilimi boyunca aralıkların yakınlaştırmasını ve ölçeğini görmenizi sağlar. Bu, çok büyük bir zaman dilimi arasında hareket konusunda daha kesin bir denetim sağlar ve bu sayede, verilerinizin ayrıntılı, yüksek çözünürlüklü bir şekilde kesişimlerini gözden geçirmenize olanak tanır. Kaydırıcının varsayılan başlangıç noktası, Seçiminizdeki verilerin en uygun görünümü olarak ayarlanır; dengeleme çözümü, sorgu hızı ve ayrıntı düzeyi.

1. **Yakınlaştır ve Kaydır**: grafiği yakınlaştırmak ve kaydırmak için bu denetimi seçin.

1. **Y ekseni denetimi**: kullanılabilir Y ekseni görünümü seçeneklerinde geçiş yapar:

    * `Stacked`: Her satırın tek bir Y ekseni vardır.
    * `Overlap`: Seçili satırı temel alarak Y ekseni verileri değiştirilerek aynı Y ekseninde birden çok satırı yığmak için kullanın.
    * `Shared`: Tüm Y ekseni verileri birlikte gösterilir.

1. **İşaretleyici öğesi**: Şu anda seçili olan veri öğesi ve ilgili ayrıntıları.

Mevcut grafikteki bir veri noktasına **sağ tıklayıp** fare tuşunu basılı tutup seçili alanı seçtiğiniz uç noktaya sürükleyerek belirli bir veri dilimine daha fazla ayrıntıya gidebilirsiniz. Mavi, seçili alana sağ tıklayın ve ardından aşağıda gösterildiği gibi **Yakınlaştır** **' ı** seçin. Ayrıca, telemetri olaylarını seçili TimeSpan içinde görüntüleyebilir ve indirebilirsiniz.

  [![Grafik yakınlaştırmasını Önizle](media/v2-update-explorer/preview-chart-zoom.png)](media/v2-update-explorer/preview-chart-zoom.png#lightbox)

**Yakınlaştırma** eylemini gerçekleştirdikten sonra, seçtiğiniz veri kümesi görüntülenir. Time Series Insights verilerinizin üç y ekseni gösterimlerine göre geçiş yapmak için biçim denetimini seçin.

  [![Grafik y ekseni önizlemesi](media/v2-update-explorer/tsi-preview-explorer-standard-chart.png)](media/v2-update-explorer/tsi-preview-explorer-standard-chart.png#lightbox)

Burada, **Çakışan bir grafiğe** örnek verilmiştir:

  [![Çakışan grafik seçeneği](media/v2-update-explorer/tsi-preview-explorer-overlapping-chart.png)](media/v2-update-explorer/tsi-preview-explorer-overlapping-chart.png#lightbox)

**Diğer eylemler** düğmesi, **indirmeyi CSV olarak**görüntülemek, **Power BI bağlanmak**, **grafik verilerini tablo olarak göstermek**ve **Ham olaylar seçeneklerini araştırmak** için genişler.

  [![Diğer eylemler seçeneği](media/v2-update-explorer/more-actions-icon.png)](media/v2-update-explorer/more-actions-icon.png#lightbox)

[Time Series Insights yerel Power BI Bağlayıcısı](concepts-power-bi.md)'Nda **Power BI Bağlan** seçeneği hakkında daha fazla bilgi edinin.

## <a name="6-time-editor-panel"></a>6. zaman Düzenleyicisi bölmesi

Time Series Insights ile çalışırken ilk olarak bir zaman aralığı seçersiniz. Seçilen zaman aralığı Time Series Insights güncelleştirme pencere öğeleri ile düzenleme için kullanılabilen veri kümesini denetler.

  [![Zaman seçim paneli](media/v2-update-explorer/tsi-preview-explorer-timeline-element.png)](media/v2-update-explorer/tsi-preview-explorer-timeline-element.png#lightbox)

> [!TIP]
> Zaman çizelgesinin bir kısmı, ısınma veya turuncu renkte vurgulanır ve bu da, normal mağazada kullanılabilir olan verilerin yayılmasını gösterir.

Çalışma zamanı aralığını seçmek için Time Series Insights güncelleştirmesinde aşağıdaki Web denetimleri mevcuttur. 

  [![Araştırma iyi denetimi](media/v2-update-explorer/exploration-well-control.png)](media/v2-update-explorer/exploration-well-control.png#lightbox)

1. **İç tarih aralığı kaydırıcı denetimi**: iki uç nokta denetimini istediğiniz zaman aralığında sürükleyerek kullanın. Bu iç tarih aralığı, dış tarih aralığı kaydırıcı denetimi tarafından sınırlandırılır.

1. **Tarih aralığı düğmelerini artırma ve azaltma**: istediğiniz Aralık için herhangi bir düğmeyi seçerek zaman aralığınızı artırın veya azaltın.

1. **Zaman aralığı daraltma denetimi**: Bu Web denetimi, iç tarih aralığı kaydırıcı aracı dışında tüm denetimleri gizlemenizi sağlar.

1. **Dış tarih aralığı kaydırıcı denetimi**: bitiş tarihi aralığını seçmek için uç nokta denetimlerini kullanın. Bu, iç tarih aralığı denetiminizdeki kullanılabilir olacak.

1. **Zaman aralığı kaydırıcı denetimi**: son **30 dakika**, **son 12 saat**veya **özel bir Aralık**gibi önceden ayarlanmış zaman aralığı seçimleri arasında hızlıca geçiş yapmak için bunu kullanın. Bu değerin değiştirilmesi, Aralık boyutu kaydırıcı aracında ele alınan kullanılabilir Aralık aralıklarını de değiştirir.

   [![Seçim paneline ve](media/v2-update-explorer/to-and-from-element.png)](media/v2-update-explorer/to-and-from-element.png#lightbox)

## <a name="7-app-bar"></a>7. uygulama çubuğu

Time Series Insights önizlemesi gezinti paneli Time Series Insights uygulamanızın en üstünde görünür. Aşağıdaki işlevleri sağlar:

### <a name="current-session-share-link-control"></a>Geçerli oturum paylaşma bağlantı denetimi

  [![Paylaş simgesi](media/v2-update-explorer/tsi-preview-explorer-share-icon.png)](media/v2-update-explorer/tsi-preview-explorer-share-icon.png#lightbox)

Ekibinizle bir URL bağlantısı paylaşmak için yeni **paylaşma** simgesini seçin.

  [![Örnek URL 'nizi paylaşma](media/v2-update-explorer/tsi-preview-explorer-share-your-view.png)](media/v2-update-explorer/tsi-preview-explorer-share-your-view.png#lightbox)

### <a name="tenant-section"></a>Kiracı bölümü

  [![Kiracı seçimi](media/v2-update-explorer/tsi-preview-explorer-tenant-selection.png)](media/v2-update-explorer/tsi-preview-explorer-tenant-selection.png#lightbox)

* Geçerli Time Series Insights oturum açma hesabı bilgilerini görüntüler.
* Kullanılabilir Time Series Insights temaları arasında geçiş yapmak için bunu kullanın.
* Önizleme [demo Web uygulamasını](https://insights.timeseries.azure.com/preview/demo)görüntülemek için kullanın.

### <a name="theme-selection"></a>Tema seçimi

Yeni bir tema seçmek için sağ üst köşede bulunan profil simgenizi seçin. Ardından, **Temayı Değiştir**' i seçin.

  [![Tema seçimi](media/v2-update-explorer/tsi-preview-theme-selection.png)](media/v2-update-explorer/tsi-preview-theme-selection.png#lightbox)

> [!TIP]
> Dil seçimi, profil simgenizin seçilerek de kullanılabilir.

Azure Time Series Insights önizlemesi iki tema destekler:

* **Açık tema**: Bu belge boyunca gösterilen varsayılan tema.
* **Koyu tema**: gezgin 'i burada gösterildiği gibi işler:

  [![Seçili Koyu tema](media/v2-update-explorer/tsi-preview-dark-theme-selected.png)](media/v2-update-explorer/tsi-preview-dark-theme-selected.png#lightbox)

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="preview-terms-panel"></a>Önizleme terimleri bölmesi

Bu bölüm yalnızca, güncelleştirilmiş Kullanıcı arabiriminde gezgin kullanmayı deneyen mevcut S1/S2 ortamları için geçerlidir. Genel kullanıma açık ürün ve önizleme birleşimini kullanmak isteyebilirsiniz. Mevcut kullanıcı arabiriminden güncelleştirilmiş gezgin 'e bazı işlevler ekledik, ancak var olan Time Series Insights Gezgininde S1/S2 ortamı için tam kullanıcı arabirimi deneyimini edinebilirsiniz. 

Hiyerarşi yerine, Time Series Insights terimleri bölmesi görüntülenir. Terimler paneli ortamınızda sorgular tanımlamanızı sağlar. Ayrıca, bir koşula göre verileri filtrelemek için kullanın.

  [![Sorgu paneli nerede](media/v2-update-explorer/s1-s2-preview-query.png)](media/v2-update-explorer/s1-s2-preview-query.png#lightbox)

Time Series Insights önizleme Koşulları Düzenleyicisi paneli aşağıdaki parametreleri alır:

**Burada**: aşağıdaki tabloda listelenen işlenenleri kümesini kullanarak olaylarınızı hızlıca filtrelemek için WHERE yan tümcesini kullanın. Bir işlenen seçerek bir arama yaparsanız, bu arama temelinde koşul otomatik olarak güncelleştirilir. Desteklenen işlenen türleri şunları içerir:

| Çalışma | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | Dize, bool, Double, DateTime, TimeSpan, NULL |
| `IN` | Dize, bool, Double, DateTime, TimeSpan, NULL | Tüm işlenenler aynı türde veya NULL sabit olmalıdır. |
| `HAS` | Dize | Sağ tarafta yalnızca sabit dize sabit değerlerine izin verilir. Boş dize ve NULL değerlerine izin verilmez. |

Desteklenen sorgu işlemleri ve veri türleri hakkında daha fazla bilgi edinmek için, okuma [zaman serisi ifadesi (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax).

### <a name="examples-of-where-clauses"></a>WHERE yan tümcelerinin örnekleri

  [![WHERE yan tümcesi örnekleri](media/v2-update-explorer/tsi-preview-example-queries.png)](media/v2-update-explorer/tsi-preview-example-queries.png#lightbox)

**Ölçü**: geçerli grafiğiniz için öğe olarak kullanabileceğiniz tüm sayısal sütunları (**Double**) görüntüleyen bir açılan liste.

**Bölme ölçütü**: Bu aşağı açılan liste, modelinizde, verilerinizi gruplabilmeniz için kullanabileceğiniz tüm kullanılabilir kategorik sütunları (dizeler) görüntüler. Aynı x ekseninde görüntülenecek en fazla beş terim ekleyebilirsiniz. İstediğiniz parametreleri girin ve sonra yeni bir terim eklemek için **Ekle** ' yi seçin.

  [![Sorgulanan ve filtrelenmiş görünüm bir](media/v2-update-explorer/s1-s2-preview-filtered-view.png)](media/v2-update-explorer/s1-s2-preview-filtered-view.png#lightbox)

Aşağıdaki görüntüde gösterildiği gibi görünür simgesini seçerek grafik panelinde öğeleri görüntüleyebilir ve gizleyebilirsiniz. Sorguları tamamen kaldırmak için kırmızı **X**seçeneğini belirleyin.

  [![Sorgulanan ve filtrelenmiş bir seçeneği iptal et](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png)](media/v2-update-explorer/s1-s2-preview-filtered-view-cancel.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Verileri](./concepts-ingestion-overview.md) ortamınıza alma hakkında bilgi edinin.

- [Depolama](concepts-storage.md) hakkındaki makaleye göz atın

- [Veri modellemesi](./concepts-model-overview.md)Time Series Insights önizleme belgesini okuyun.

- Time Series Insights örneğinizi [tanılamayı ve sorun gidermeyi](./time-series-insights-update-how-to-troubleshoot.md) öğrenin.
