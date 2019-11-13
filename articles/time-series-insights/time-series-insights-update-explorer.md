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
ms.date: 10/07/2019
ms.custom: seodec18
ms.openlocfilehash: f09de6fee4c9de39d98ec66714686ab1e805c4cd
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74014871"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Gezginde verileri görselleştirme (Önizleme)

Bu belgede, Azure Time Series Insights Preview [demo Web uygulamasının](https://insights.timeseries.azure.com/preview/demo)Kullanıcı arabirimi ve Kullanıcı deneyimi özellikleri ve arabirimi açıklanmaktadır. Özellikle, barındırılan örnek, arabirim özelleştirme seçeneklerinin yerleşimini ve sunulan demo aracılığıyla gezinmeyi ele alır.

## <a name="prerequisites"></a>Önkoşullar

Azure Time Series Insights önizleme Gezginini kullanmaya başlamak için şunları yapmanız gerekir:

* Ayarlanmış bir zaman serisi görüşleri ortamına sahip. Örnek sağlama hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme](./time-series-insights-update-create-environment.md) öğreticisini deneyin.
* Hesap için oluşturduğunuz Time Series Insights ortamına [veri erişimi sağlar](./time-series-insights-data-access.md) . Diğerleri de kendiniz için farklı erişim sağlayabilir.
* Ortama veri göndermek için Time Series Insights ortamına bir olay kaynağı ekleyin:
  * [Bir olay hub 'ına nasıl bağlanacağınızı](./time-series-insights-how-to-add-an-event-source-eventhub.md)öğrenin.
  * [IoT Hub 'ına nasıl bağlanacağınızı](./time-series-insights-how-to-add-an-event-source-iothub.md)öğrenin.

## <a name="learn-about-the-preview-explorer"></a>Önizleme Gezgini hakkında bilgi edinin

Azure zaman serisi öngörüleri önizlemesi Gezgini aşağıdaki öğelerden oluşur:

[Gezgin görünümünü ![](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Ortam paneli</a>: Azure Time Series Insights ortamlarınızı görüntüler.
- <a href="#navigation-menu">Gezinti menüsü</a>: **Çözümle** ve **model** sayfaları arasında geçiş yapmak için kullanın.
- <a href="#hierarchy-tree">Hiyerarşi ağacı</a>: grafiği oluşturulacak belirli model ve veri öğelerini seçmek için kullanın.
- <a href="#preview-well">Zaman serisi iyi</a>: seçili olan veri öğelerinizi renk kodlamasına sahip tablo biçiminde görüntüler.
- <a href="#preview-chart">Grafik paneli</a>: geçerli çalışma haritanız görüntüler.
- <a href="#time-editor-panel">Zaman çizelgesi</a>: çalışma süresi aralığını değiştirmek için kullanın.
- <a href="#navigation-panel">Uygulama çubuğu</a>: geçerli kiracı gibi Kullanıcı yönetimi seçeneklerinizi içerir. Temayı ve dil ayarlarını değiştirmek için bunu kullanabilirsiniz.

## <a name="environment-drop-down-list"></a>Ortam açılan listesi

Ortam açılan listesi, erişiminiz olan tüm Time Series Insights ortamlarını görüntüler. Liste, Time Series Insights önizlemesi gibi Kullandıkça Öde ortamlarını içerir. Bu liste, genel olarak kullanılabilen S1/S2 ortamlarını da içerir.

1. Görüntülenmiş ortamınızın yanındaki aşağı açılan oku seçin.

   [Denetim Masası ![](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Ardından istediğiniz ortamı seçin.

## <a name="navigation-menu"></a>Gezinti menüsü

  [Gezinti menüsünü ![](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

İki görünüm arasından seçim yapmak için gezinti menüsünü kullanın:

* **Çözümle**: modellenen veya Modellenmemiş zaman serisi verilerinizde zengin analizler gerçekleştirmek için bunu kullanın.
* **Model**: yeni Time Series Insights önizleme türlerini, hiyerarşileri ve örnekleri Time Series Insights modelinize göndermek için bunu kullanın.

## <a name="hierarchy-tree"></a>Hiyerarşi ağacı

Hiyerarşi ağacı, cihazlarınızda modeller, belirli cihazlar ve algılayıcılar içeren seçili veri öğelerini görüntüler.

### <a name="model-search-panel"></a>Model arama paneli

Grafiğinizde görüntülenmesini istediğiniz belirli zaman serisi örneklerini bulmak için zaman serisi modeli hiyerarşinizde kolayca arama yapmak için model arama panelini kullanabilirsiniz. Örneklerinizi seçtikten sonra, bunlar hem geçerli grafiğe hem de veri kutusuna eklenir.

  [Model arama panelini ![](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Model yazma

Azure Time Series Insights önizlemesi, zaman serisi modelinizdeki tam oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini destekler.

* **Zaman serisi model türü**: hesaplamalar yapmak için değişkenler veya formüller tanımlamak üzere Time Series Insights türlerini kullanabilirsiniz. Bunlar, belirli bir Time Series Insights örneğiyle ilişkilendirilir. Bir türü veya daha fazla değişken olabilir.
* **Zaman serisi modeli hiyerarşisi**: hiyerarşileri verilerinizin sistematik kuruluşlar şunlardır. Time Series Insights veri farklı varlıklar arasındaki ilişkileri hiyerarşileri kullanılırlar.
* **Zaman serisi modeli örneği**: zaman serisi örneklerdir. Çoğu durumda, bu, ortamdaki varlığın benzersiz tanımlayıcısı olan **DeviceID** veya **AssetID**.

Zaman serisi modeli hakkında daha fazla bilgi için bkz: [kez serisi modelleri](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Önizleme kutusu

İyi, örnek alanlarını ve seçili Time Series Insights örneklerle ilişkili diğer meta verileri görüntüler. Sağ taraftaki onay kutularını seçerek geçerli grafikten belirli örnekleri gizleyebilir veya görüntüleyebilirsiniz. Ayrıca, öğenin sol tarafındaki kırmızı **silme** (çöp kutusu) denetimini seçerek geçerli verilerdeki belirli veri öğelerini de kaldırabilirsiniz.

  [Önizleme iyi ![](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Grafik **Analizi** sayfanızın yerleşimini yeniden yapılandırmak için sağ üst köşedeki üç nokta simgesini seçin:

  [![telemetri düzeni seçenekleri](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Aşağıdaki iletiyi görürseniz, örnek seçilen zaman aralığı boyunca hiçbir veri içermez. Sorunu çözmek için, zaman aralığını artırın veya örneğin veri iletme süresini onaylayın.
>
> ![Veri bildirimi yok](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Önizleme grafiği

Grafik ile Time Series Insights örnekleri satır olarak görüntüleyebilirsiniz. Grafiği daha büyük hale getirmek için Web denetimlerini seçerek ortam paneli, veri modeli ve zaman aralığı denetim masasını daraltabilirsiniz.

  [![önizleme grafiğine genel bakış](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Seçilen tarih aralığı**: görselleştirmede hangi veri öğelerinin kullanılabilir olduğunu denetler.

- **İç tarih aralığı kaydırıcı aracı**: iki uç nokta denetimini istediğiniz zaman aralığında sürükleyerek kullanın.

- **Zaman aralığı Daralt denetim**: daraltır ve zaman aralığı paneli Düzenleyicisi genişletir.

- **Y ekseni biçim denetimi**: kullanılabilir Y ekseni görünümü seçeneklerinde geçiş yapar:

    * `Default`: her satırda tek bir y ekseni vardır.
    * `Stacked`: aynı y ekseninde birden çok satırı, seçilen satıra göre değişen y ekseni verileriyle birlikte yığmak için kullanın.
    * `Shared`: tüm y ekseni verileri birlikte görüntülendi.

- **Geçerli bir veri öğesi**: şu anda seçili veri öğesi ve onun ilişkili ayrıntılar.

Belirli bir veri dilimi hakkında daha fazla ayrıntıya geçmek için, geçerli grafikteki bir veri noktasına sol tıklayın ve sonra seçili alanı seçtiğiniz uç noktaya sürükleyin. Seçilen gri alanına sağ tıklayın ve aşağıdaki görüntüde gösterildiği gibi **Yakınlaştır**' ı seçin:

  [![önizlemesi grafiği önizlemesi](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

**Yakınlaştırma** eylemini gerçekleştirdikten sonra, seçtiğiniz veri kümesini görürsünüz. Time Series Insights verilerinizin üç y ekseni temsilinden dolaşmak için y ekseni biçim denetimini seçin.

  [![Preview grafik y ekseni](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Burada, paylaşılan Y eksenlerinin bir örneğini görebilirsiniz:

  [![paylaşılan Y eksenlerinin önizlemesi](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Zaman Düzenleyicisi bölmesi

Zaman serisi öngörüleri Önizleme ile çalışırken, ilk olarak bir zaman aralığı seçin. Seçilen zaman aralığı, Time Series Insights önizleme pencere öğeleri ile düzenleme için kullanılabilen veri kümesini denetler. Aşağıdaki Web denetimleri, çalışma zamanı aralığını seçmek için Time Series Insights önizlemede sunulmaktadır:

  [![zaman seçimi paneli](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **İç tarih aralığı kaydırıcı aracı**: iki uç nokta denetimini istediğiniz zaman aralığında sürükleyerek kullanın. Bu iç tarih aralığı, dış tarih aralığı kaydırıcı denetimi tarafından sınırlandırılır.

1. **Artırın ve azaltın tarih aralığı düğmeleri**: artışa veya sürenizi span istediğiniz aralığı için herhangi bir düğmeyi seçerek.

1. **Zaman aralığı daraltma denetimi**: Bu Web denetimi, iç tarih aralığı kaydırıcı aracı dışında tüm denetimleri gizlemenizi sağlar.

1. **Dış tarih aralığı kaydırıcı denetimi**: bitiş tarihi aralığını seçmek için uç nokta denetimlerini kullanın. Bu, iç tarih aralığı denetiminizdeki kullanılabilir olacak.

1. **Hızlı saat tarih aralığı açılan listesi**: son **30 dakika**, **son 12 saat**veya **özel bir Aralık**gibi önceden ayarlanmış zaman aralığı seçimleri arasında hızlıca geçiş yapmak için kullanın. Bu değeri değiştirmeniz aralık boyutu kaydırıcı aracını tartışılan kullanılabilir aralığı aralıkları değiştirir.

1. **Aralık boyutu kaydırıcı aracı**: aynı zaman dilimi boyunca aralıkların ölçeğini ve ölçeğini yakınlaştırmak için kullanın. Bu eylem büyük saat dilimleri arasında hareket daha kesin bir denetim sağlar. Bir milisaniyeye kadar küçük olan dilimleri aşağı doğru eğilimler halinde görüntüler. Verilerinizin parçalı, yüksek çözünürlüklü bir şekilde nasıl olduğunu görmek için kullanabilirsiniz. Kaydırıcının varsayılan başlangıç noktası çözümleme, sorgu hızı ve ayrıntı düzeyi dengeleyen seçimden, verilerin en iyi görünüm olarak ayarlanır.

1. **Tarih aralığı-ve-from web Control**: Bu Web denetimiyle, istediğiniz tarih ve saat aralıklarını kolayca seçebilirsiniz. Ayrıca, farklı saat dilimleri arasında geçiş yapmak için denetimi de kullanabilirsiniz. Geçerli çalışma alanınıza uygulanacak değişiklikleri yaptıktan sonra **Kaydet**' i seçin.

   [seçim paneline ve ![](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Gezinti paneli

Time Series Insights önizlemesi gezinti paneli Time Series Insights uygulamanızın en üstünde görünür. Aşağıdaki işlevleri sağlar:

### <a name="current-session-share-link-control"></a>Geçerli oturumdaki paylaşımını bağlantı denetimi

  [![paylaşma simgesi](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Ekibinizle bir URL bağlantısı paylaşmak için yeni **paylaşma** simgesini seçin.

  [Örnek URL 'nizi ![paylaşma](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Kiracı bölümü

  [![kiracı seçimi](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Geçerli Time Series Insights oturum açma hesabı bilgilerini görüntüler.
* Kullanılabilir Time Series Insights temaları arasında geçiş yapmak için bunu kullanın.
* Önizleme [demo Web uygulamasını](https://insights.timeseries.azure.com/preview/demo)görüntülemek için kullanın.

### <a name="theme-selection"></a>Tema seçimi

Yeni bir tema seçmek için sağ üst köşede bulunan profil simgenizi seçin. Ardından, **Temayı Değiştir**' i seçin.

  [![teması seçimi](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Dil seçimi, profil simgenizin seçilerek de kullanılabilir.

Azure zaman serisi öngörüleri Önizleme iki tema destekler:

* **Açık tema**: Bu belge boyunca gösterilen varsayılan tema.
* **Koyu tema**: gezgin 'i burada gösterildiği gibi işler:

  [Seçili Koyu tema ![](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="preview-terms-panel"></a>Önizleme terimleri bölmesi

Bu bölüm yalnızca güncelleştirilmiş kullanıcı Arabiriminde Gezgin kullanma girişimi mevcut S1/S2 ortamlar için geçerlidir. Genel kullanıma açık ürün ve önizleme birleşimini kullanmak isteyebilirsiniz. Bazı işlevler mevcut kullanıcı Arabiriminden güncelleştirilmiş Gezgini'ne ekledik ancak mevcut zaman serisi görüşleri Gezgin ortamında S1/S2 için tam kullanıcı Arabirimi deneyimi elde edebilirsiniz. 

Hiyerarşi yerine, ortamınızda sorgular tanımladığınız Time Series Insights terimler panelini görürsünüz. Bir koşula göre verilerinizi filtrelemek için kullanın.

  [sorgu panelinin ![](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

Zaman serisi öngörüleri Önizleme Koşulları Düzenleyicisi paneli aşağıdaki parametreleri alır:

**Burada**: aşağıdaki tabloda listelenen işlenenleri kümesini kullanarak olaylarınızı hızlıca filtrelemek için WHERE yan tümcesini kullanın. Bir işlenen seçerek bir arama yapın, o aramaya bağlı koşul otomatik olarak güncelleştirilir. Desteklenen işlenen türleri şunları içerir:

| İşlem | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Çift, DateTime, zaman aralığı | |
| `=`, `!=`, `<>` | Dize, Bool, Double, DateTime, zaman aralığı, NULL |
| `IN` | Dize, Bool, Double, DateTime, zaman aralığı, NULL | Tüm işlenenler aynı türde veya NULL sabiti olması. |
| `HAS` | Dize | Sağ tarafta yalnızca sabit dize sabit değerlerine izin verilir. Boş dize ve NULL değerlerine izin verilmez. |

Desteklenen sorgu işlemleri ve veri türleri hakkında daha fazla bilgi edinmek için bkz. [zaman serisi ifadesi (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>WHERE yan tümcelerinin örnekleri

  [![WHERE yan tümcesi örnekleri](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Ölçü**: geçerli grafiğiniz için öğe olarak kullanabileceğiniz tüm sayısal sütunları (**Double**) görüntüleyen bir açılan liste.

**Bölme ölçütü**: Bu aşağı açılan liste, modelinizde, verilerinizi gruplabilmeniz için kullanabileceğiniz tüm kullanılabilir kategorik sütunları (dizeler) görüntüler. Aynı x ekseninde görüntülenecek en fazla beş terim ekleyebilirsiniz. İstediğiniz parametreleri girin ve sonra yeni bir terim eklemek için **Ekle** ' yi seçin.

  [![sorgulanan ve filtrelenmiş bir görünüm](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Aşağıdaki görüntüde gösterildiği gibi görünür simgesini seçerek grafik panelinde öğeleri görüntüleyebilir ve gizleyebilirsiniz. Sorguları tamamen kaldırmak için kırmızı **X**seçeneğini belirleyin.

  [![sorgulanan ve filtrelenmiş görünüm iki](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Time Series Insights önizlemede [depolama ve](./time-series-insights-update-storage-ingress.md) giriş hakkında bilgi edinin.
- [Veri modellemesi](./time-series-insights-update-tsm.md)Time Series Insights önizleme belgesini okuyun.
- Time Series Insights örneğinizi [tanılamayı ve sorun gidermeyi](./time-series-insights-update-how-to-troubleshoot.md) öğrenin.
