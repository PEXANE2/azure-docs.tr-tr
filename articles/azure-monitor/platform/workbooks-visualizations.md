---
title: Azure Monitör çalışma kitabı görselleştirmeleri
description: Metin, grafikler, ızgaralar, ağaçlar ve grafikler dahil olmak üzere tüm Azure Monitor çalışma kitabı görselleştirmeleri bileşenleri hakkında bilgi edinin.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d05902f47dff3dd2f8a63ae240c0b8825a5c441f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658039"
---
# <a name="azure-monitor-workbook-visualizations"></a>Azure Monitör çalışma kitabı görselleştirmeleri

Azure Monitor çalışma kitapları, raporlama ihtiyaçlarınızı karşılamak için bir dizi farklı görselleştirme stilini destekler. Bu makalede, her görüntüleme türünden örnekler verilmektedir.

## <a name="text"></a>Metin

Çalışma kitapları, yazarların çalışma kitaplarına metin blokları eklemelerine olanak sağlar. Metin, telemetrinin insan analizi, kullanıcıların verilerinizi yorumlamasına yardımcı olacak bilgiler, bölüm başlıkları vb. olabilir.

![Apdex metin tablosunun ekran görüntüsü](./media/workbooks-visualizations/apdex.png)

Metin, tam biçimlendirme denetimi sağlayan bir Markdown denetimi ile eklenir.

![İşlenen tabloyu oluşturan ham işaretlemenin ekran görüntüsü](./media/workbooks-visualizations/markdown.png)

### <a name="add-a-text-control"></a>Metin denetimi ekleme

1. İş kitabını **düzenleme** araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına metin denetimi eklemek için **metin ekle** bağlantısını kullanın.
3. Kontrole Markdown'u ekleyin.
4. Biçimlendirilmiş metni görmek için **Bitti Düzenleme** düğmesini tıklatın.

> [!TIP]
> Farklı biçimlendirme seçenekleri hakkında bilgi edinmek için bu [Markdown hile sayfasını](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) kullanın.

## <a name="charts"></a>Grafikler

Çalışma kitapları izleme verilerinin grafik olarak sunulmasını sağlar. Desteklenen grafik türleri satır, çubuk, çubuk kategorik, alan, dağılım çizimleri, pasta ve zaman içerir. Yazarlar grafiğin yüksekliğini, genişliğini, renk paletini, göstergeyi, başlıkları, veri olmayan iletiyi vb. özelleştirmeyi seçebilirler.

Çalışma kitapları hem günlükler hem de metrik veri kaynakları için grafikleri destekler. 

### <a name="adding-a-log-chart"></a>Günlük grafiği ekleme

1. İş kitabını **düzenleme** araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için **sorgu ekle** bağlantısını kullanın.
3. Sorgu türünü **Günlük,** kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemeniz için [KQL'yi](https://docs.microsoft.com/azure/kusto/query/) girmek için Sorgu düzenleyicisini kullanın (örneğin, istek eğilimi).
5. Görselleştirmeyi aşağıdakilerden birine ayarlayın: **Alan,** **Çubuk,** **Çubuk (kategorik),** **Çizgi,** **Pasta,** **Dağılım,** veya **Zaman**.
6. Zaman aralığı, görselleştirme, boyut, renk paleti ve gösterge gibi gerekirse diğer parametreleri ayarlayın.

![Günlük grafiğin edit modunda ekran görüntüsü](./media/workbooks-visualizations/log-chart.png)

#### <a name="log-chart-parameters"></a>Günlük grafik parametreleri

| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `Query Type` | Kullanılacak sorgu türü | Günlük, Azure Kaynak Grafiği vb. |
| `Resource Type` | Hedeflenene kadar kaynak türü | Uygulama Öngörüleri, Günlük Analitiği veya Azure ilk |
| `Resources` | Metrik değerini almak için bir kaynak kümesi | MyApp1 |
| `Time Range` | Günlük grafiğini görüntülemek için zaman penceresi | Son saat, Son 24 saat, vb. |
| `Visualization` | Kullanılacak görselleştirme | Alan, Çubuk, Çizgi, Pasta, Dağılım, Zaman, çubuk kategorik |
| `Size` | Denetimin dikey boyutu | Küçük, orta, büyük veya tam |
| `Color palette` | Grafikte kullanılacak renk paleti. Çok metrikveya parçalı modda yoksayılır. | Mavi, yeşil, kırmızı, vb. |
| `Legend` | Gösterge için kullanılacak toplama işlevi | Değerlerin Toplamı veya Ortalaması veya Max, Min, İlk, Son değer |
| `Query` | Grafik görselleştirme tarafından beklenen biçimde veri döndüren herhangi bir KQL sorgusu | _istekleri \| make-series Requests = count() default = 0 zaman damgası üzerinde önce(1d) şimdi() adım 1h_ |

### <a name="adding-a-metric-chart"></a>Metrik grafik ekleme

1. İş kitabını **düzenleme** araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına metrik denetim eklemek için **metrik** ekle bağlantısını kullanın.
3. Bir kaynak türü (örneğin, Depolama Hesabı), hedeflenene kadar kaynakları, metrik ad alanını ve adını ve kullanılacak toplamayı seçin.
4. Gerekirse diğer parametreleri ayarlayın - zaman aralığı, bölme, görselleştirme, boyut ve renk paleti gibi.

![Edit modunda metrik grafiğin ekran görüntüsü](./media/workbooks-visualizations/metric-chart.png)

#### <a name="metric-chart-parameters"></a>Metrik grafik parametreleri

| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `Resource Type` | Hedeflenene kadar kaynak türü | Depolama veya Sanal Makine. |
| `Resources` | Metrik değerini almak için bir kaynak kümesi | MyStorage1 |
| `Namespace` | Metrik ile ad alanı | Depolama > Blob |
| `Metric` | Görselleştirmek için metrik | Depolama > Blob > İşlemleri |
| `Aggregation` | Metrik uygulamak için toplama işlevi | Toplam, Sayı, Ortalama, vb. |
| `Time Range` | Metriğin görüntülenebilmek için zaman penceresi | Son saat, Son 24 saat, vb. |
| `Visualization` | Kullanılacak görselleştirme | Alan, Çubuk, Çizgi, Dağılım, Izgara |
| `Split By` | İsteğe bağlı olarak metrik bir boyut üzerinde bölme | Geo türüne göre hareketler |
| `Size` | Denetimin dikey boyutu | Küçük, orta veya büyük |
| `Color palette` | Grafikte kullanılacak renk paleti. Parametre `Split by` kullanılırsa yoksayıldı | Mavi, yeşil, kırmızı, vb. |

## <a name="grids"></a>Kılavuzlar

Izgaralar veya tablolar, kullanıcılara veri sunmanın yaygın bir yoludur. Çalışma kitapları, kullanıcıların raporları için zengin bir kullanıcı arabirimi sağlamak için ızgarasütunlarını tek tek stile biçmesine olanak sağlar.

Aşağıdaki örnekte, karmaşık bilgileri sunmak için simgeleri, ısı eşlemlerini ve kıvılcım çubuklarını birleştiren bir ızgara gösterilmektedir. Çalışma kitabı ayrıca sıralama, arama kutusu ve analitik gider düğmesi de sağlar.

![Günlük tabanlı ızgaranın ekran görüntüsü](./media/workbooks-visualizations/grid.png)

### <a name="adding-a-log-based-grid"></a>Günlük tabanlı ızgara ekleme

1. İş kitabını **düzenleme** araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için **sorgu ekle** bağlantısını kullanın.
3. Sorgu türünü **Günlük,** kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemeniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın (örneğin, bir eşiğin altında bellek olan VM'ler)
5. Görselleştirmeyi **Grid** olarak ayarlama
6. Zaman aralığı, boyut, renk paleti ve gösterge gibi gerekirse diğer parametreleri ayarlayın.

![Günlük tabanlı ızgara sorgusunun ekran görüntüsü](./media/workbooks-visualizations/grid-query.png)

## <a name="tiles"></a>Kutucuklar

Kutucuklar, özet verileri çalışma kitaplarında sunmanın çok yararlı bir yoludur. Aşağıdaki resimde, ayrıntılı bir ızgaranın üstündeki uygulama düzeyi özeti gibi, ortak bir kutucuk kullanım örneği gösterilmektedir.

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/tiles-summary.png)

Çalışma kitabı kutucukları bir başlık, altyazı, büyük metin, simgeler, metrik tabanlı degradeler, kıvılcım çizgisi/çubukları, altbilgi vb. gösteren desteği destekler.

### <a name="adding-a-tile"></a>Döşeme Ekleme

1. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için **sorgu ekle** bağlantısını kullanın. 
3. Sorgu türünü **Günlük,** kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize Requests = count() by appName, name
    | top 7 by Requests desc
    ```
5. Boyutu **Tam** olarak ayarla
6. Görselleştirmeyi **Kutucuklar** olarak ayarlama
7. Ayarlar bölmesini açmak için **Döşeme Ayarları** düğmesini tıklatın
8. **Kiremit Alanlarında**, set:
    * Başlık:`name`
    * Sol: `Requests`, Renderer: `Big Number`, `Green to Red`Renk Paleti: , Min Değer:`0`
    * Alt:`appName`
9. Bölmenin altındaki **Kaydet ve Kapat** düğmesini tıklatın.

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/tile-settings.png)

Bu, kutucukların okuma modunda nasıl görüneceğidir:

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/tiles-read-mode.png)

## <a name="trees"></a>Ağaç

Çalışma kitapları, ağaç ızgaraları üzerinden hiyerarşik görünümleri destekler. Ağaçlar, bazı satırların bir sonraki seviyeye genişletilebilir olmasını sağlar.

Aşağıdaki örnek, ağaç ızgarası olarak görselleştirilmiş kapsayıcı sistem durumu ölçümlerini (çalışma kümesi boyutu) gösterir. Buradaki üst düzey düğümler Azure Kubernetes Service (AKS) düğümleri, bir sonraki düzey bölmeler ve son düzey kapsayıcılar. Sütunlarınızı ızgaradaki gibi biçimlendirebildiğinize dikkat edin (ısı haritası, simgeler, bağlantı). Bu durumda altta yatan veri kaynağı AKS günlükleri ile bir Log Analytics çalışma alanıdır.

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/trees.png)

### <a name="adding-a-tree-grid"></a>Ağaç Izgara Ekleme
1. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için **sorgu ekle** bağlantısını kullanın. 
3. Sorgu türünü **Günlük,** kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    requests
    | summarize Requests = count() by ParentId = appName, Id = name
    | extend Kind = 'Request', Name = strcat('🌐 ', Id)
    | union (requests
    | summarize Requests = count() by Id = appName
    | extend Kind = 'Request', ParentId = '', Name = strcat('📱 ', Id))
    | project Name, Kind, Requests, Id, ParentId
    | order by Requests desc
    ```
5. Görselleştirmeyi **Grid** olarak ayarlama
6. Ayarlar bölmesini açmak için **Sütun Ayarları** düğmesini tıklatın
7. Alttaki **Ağaçlara/Ayarlara Göre Gruplandırma** bölümünde, ayarlayın:
    * Ağaç Türü:`Parent/Child`
    * Id Alanı:`Id`
    * Üst Kimlik Alanı:`ParentId`
    * Genişleticiüzerinde göster:`Name`
    * Ağacın üst düzeyini genişletin:`checked`
8. Üstteki _Sütunlar_ bölümünde, ayarlayın:
    * _Id_ - Sütun İşleyicisi:`Hidden`
    * _Üst Id_ - Sütun İşleyicisi:`Hidden`
    * _İstekler_ - Sütun `Bar`İşleyicisi: , Renk: `Blue`, Minimum Değer:`0`
9. Bölmenin altındaki _Kaydet ve Kapat_ düğmesini tıklatın.    

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/tree-settings.png)

### <a name="tree-settings"></a>Ağaç Ayarları

| Ayar | Açıklama |
|:------------- |:-------------|
| `Id Field` | Kılavuzdaki her satırın benzersiz Kimliği |
| `Parent Id Field` | Geçerli satırın üst numarası |
| `Show the expander on` | Ağaç genişleticinin gösterilen sütun. Ağaç ızgaralarının çok okunabildikleri nden kimlik lerini ve üst kimlik alanını gizlemeleri yaygındır. Bunun yerine, genişletici daha okunabilir değeri olan bir alanda görünür - varlığın adı gibi |
| `Expand the top level of the tree` | İşaretlenirse, ağaç ızgarası en üst düzeyde genişletilir. Varsayılan olarak daha fazla bilgi göstermek istiyorsanız yararlı |

## <a name="graphs"></a>Grafik

Çalışma kitapları, izleme varlıkları arasındaki ilişkileri göstermek için günlüklerden gelen verilere dayalı rasgele grafikleri görselleştirmeyi destekler.

Aşağıdaki grafikte, harici bilgisayarlara/harici bilgisayarlara çeşitli bağlantı noktası üzerinden bilgisayara giriş/çıkış akan veriler gösterin. Türüne göre renklenir (bilgisayar vs port u harici IP' ye karşı) ve kenar boyutları arada akan veri miktarına karşılık gelir. Altta yatan veriler VM bağlantılarını hedefleyen KQL sorgusundan gelir.

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/graph.png)

### <a name="adding-a-graph"></a>Grafik Ekleme
1. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
2. Çalışma kitabına günlük sorgusu denetimi eklemek için **sorgu ekle** bağlantısını kullanın. 
3. Sorgu türünü **Günlük,** kaynak türü (örneğin, Uygulama Öngörüleri) ve hedefalacak kaynakları olarak seçin.
4. Çözümlemesiniz için KQL'yi girmek için Sorgu düzenleyicisini kullanın
    ```kusto
    let data = dependencies
    | summarize Calls = count() by App = appName, Request = operation_Name, Dependency = name
    | extend RequestId = strcat(App, '::', Request);
    let links = data
    | summarize Calls = sum(Calls) by App, RequestId
    | project SourceId = App, TargetId = RequestId, Calls, Kind = 'App -> Request'
    | union (data
        | project SourceId = RequestId, TargetId = Dependency, Calls, Kind = 'Request -> Dependency');
    let nodes = data
    | summarize Calls = sum(Calls) by App
    | project Id = App, Name = App, Calls, Kind = 'App'
    | union (data
        | summarize Calls = sum(Calls) by RequestId, Request
        | project Id = RequestId, Name = Request, Calls, Kind = 'Request')
    | union (data
        | summarize Calls = sum(Calls) by Dependency
        | project Id = Dependency, Name = Dependency, Calls, Kind = 'Dependency');
    nodes
    | union (links)
    ```
7. Görselleştirmeyi **Grafik** olarak ayarlama
8. Ayarlar bölmesini açmak için **Grafik Ayarları** düğmesini tıklatın
9. Alttaki _Düzen Alanları'nda,_ ayarlayın:
    * `Node Id`: `Id`
    * `Source Id`: `SourceId`
    * `Target Id`: `TargetId`
    * `Edge Label`: `None`
    * `Edge Size`: `Calls`
    * `Node Size`: `None`
    * `Coloring Type`: `Categorical`
    * `Node Color Field`: `Kind`
    * `Color palette`: `Pastel`
10. En üstteki _Düğüm Biçimi Ayarları'nda:_
    * _En İyi İçerik_ `Name`- Kullanım Sütunu: , Sütun İşleyicisi:`Text`
    * _Merkezi İçerik_- `Calls`Kullanım Sütunu: `Big Number`, Sütun İşleyici: , Renk Paleti:`None`
    * _Alt İçerik_- `Kind`Kullanım Sütunu: , Sütun İşleyicisi:`Text`
10. Bölmenin altındaki _Kaydet ve Kapat_ düğmesini tıklatın.

![Döşeme özeti görünümü ekran görüntüsü](./media/workbooks-visualizations/graph-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitaplarını Azure Kaynak Yöneticisi ile [dağıtın.](workbooks-automate.md)
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
