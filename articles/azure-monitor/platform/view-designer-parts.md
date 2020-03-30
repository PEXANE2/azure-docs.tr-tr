---
title: Azure Monitör'de Görünüm Tasarımcısı bölümleri için bir başvuru kılavuzu | Microsoft Dokümanlar
description: Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında görüntülenen özel görünümler oluşturabilir ve Günlük Analizi çalışma alanındaki verilerle ilgili çeşitli görselleştirmeler içerebilir. Bu makale, özel görünümlerinizde kullanılabilen görselleştirme parçalarının ayarlarına yönelik bir başvuru kılavuzudur.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2018
ms.openlocfilehash: 7dc4164cac1998a02ae62d7682f9630aa4faa619
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658566"
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-azure-monitor"></a>Azure Monitor'da Tasarımcı görselleştirme parçalarını görüntüleme başvuru kılavuzu
Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında Günlük Analizi çalışma alanınızdaki verileri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makale, özel görünümlerinizde kullanılabilen görselleştirme parçalarının ayarlarına yönelik bir başvuru kılavuzudur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Tasarımcıyı Görüntüle](view-designer.md): Görünüm Tasarımcısı'na ve özel görünümler oluşturma ve düzenleme yordamlarına genel bir bakış sağlar.
* [Döşeme başvurusu](view-designer-tiles.md): Özel görünümlerinizde kullanılabilir her döşeme için ayarlara başvuru sağlar.


Kullanılabilir Görünüm Tasarımcısı döşeme türleri aşağıdaki tabloda açıklanmıştır:

| Türü görüntüleme | Açıklama |
|:--- |:--- |
| [Sorgu listesi](#list-of-queries-part) |Günlük sorgularının listesini görüntüler. Sonuçlarını görüntülemek için her sorguseçebilirsiniz. |
| [Sayı ve liste](#number-and-list-part) |Üstbilgi, günlük sorgusundaki kayıt sayısını gösteren tek bir sayı görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler. |
| [İki sayı ve liste](#two-numbers-and-list-part) |Üstbilgi, ayrı günlük sorgularından kayıt sayısını gösteren iki sayı görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler. |
| [Donut ve listesi](#donut-and-list-part) |Üstbilgi, günlük sorgusunda değer sütunu özetleyen tek bir sayı görüntüler. Donut, ilk üç kaydın sonuçlarını grafiksel olarak görüntüler. |
| [İki zaman çizelgesi ve liste](#two-timelines-and-list-part) |Üstbilgi, bir günlük sorgusunda değer sütunu özetleyen tek bir sayı görüntüleyen bir ek bilgiyle, zaman içinde iki günlük sorgusunun sonuçlarını sütun grafikleri olarak görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler. |
| [Bilgi](#information-part) |Üstbilgi statik metin ve isteğe bağlı bir bağlantı görüntüler. Liste, statik bir başlık ve metin içeren bir veya daha fazla öğe görüntüler. |
| [Satır grafiği, diğer arama ve liste](#line-chart-callout-and-list-part) |Üstbilgi, zaman içinde bir günlük sorgusundan birden çok seri ve özet değere sahip bir ek bilgi ile bir satır grafiği görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler. |
| [Çizgi grafiği ve listesi](#line-chart-and-list-part) |Üstbilgi, zaman içinde bir günlük sorgusundan birden çok seri içeren bir satır grafiği görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler. |
| [Satır grafikleri bölümü yığını](#stack-of-line-charts-part) |Zaman içinde bir günlük sorgusundan birden çok seri içeren üç ayrı çizgi grafiği görüntüler. |

Sonraki bölümlerde döşeme türleri ve özellikleri ayrıntılı olarak açıklayınız.

> [!NOTE]
> Görünümdeki parçalar, Log Analytics çalışma alanınızdaki [günlük sorgularına](../log-query/log-query-overview.md) dayanır. Şu anda Application Insights'tan veri almak için [çapraz kaynak sorgularını](../log-query/cross-workspace-query.md) desteklemez.

## <a name="list-of-queries-part"></a>Sorgular bölümü listesi
Sorgular bölümü listesi günlük sorgularının listesini görüntüler. Sonuçlarını görüntülemek için her sorguseçebilirsiniz. Görünüm varsayılan olarak tek bir sorgu içerir ve ek sorgular eklemek için **+ Sorgu'yu** seçebilirsiniz.

![Sorgular görünümü listesi](media/view-designer-parts/view-list-queries.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Başlık |Görünümün üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Önceden seçilmiş filtreler |Sorgu seçtiğinizde sol filtre bölmesine dahil ediletilecek virgüllü özelliklerin listesi. |
| Render modu |Sorgu seçildiğinde görüntülenen ilk görünüm. Sorguyu açtıktan sonra kullanılabilir görünümleri seçebilirsiniz. |
| **Sorgular** | |
| Arama sorgusu |Çalıştırılabilmek için sorgu. |
| Kolay ad | Görüntülenen açıklayıcı ad. |

## <a name="number-and-list-part"></a>Sayı ve liste kısmı
Üstbilgi, günlük sorgusundaki kayıt sayısını gösteren tek bir sayı görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler.

![Sorgular görünümü listesi](media/view-designer-parts/view-number-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünümün üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Başlık** | |
| Açıklama |Üstbilginin üst kısmında görüntülenen metin. |
| Sorgu |Üstbilgi için çalışacak sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sonuçlardaki ilk on kaydın ilk iki özelliği görüntülenir. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Çubuklar, sayısal sütunun göreli değerini temel alan otomatik olarak oluşturulur.<br><br>Listedeki `Sort` kayıtları sıralamak için sorgudaki komutu kullanın. Sorguyu çalıştırmak ve tüm kayıtları döndürmek için **Tümünü Gör'i**seçebilirsiniz. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| Ad ve değer ayırıcısı |Metin özelliğini birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="two-numbers-and-list-part"></a>İki sayı ve liste parçası
Üstbilgi, ayrı günlük sorgularından bir kayıt sayısı görüntüleyen iki sayıya sahiptir. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler.

![İki Sayı & liste görünümü](media/view-designer-parts/view-two-numbers-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünümün üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Başlık Gezintisi** | |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Başlık** | |
| Açıklama |Üstbilginin üst kısmında görüntülenen metin. |
| Sorgu |Üstbilgi için çalışacak sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sonuçlardaki ilk on kaydın ilk iki özelliği görüntülenir. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Çubuklar, sayısal sütunun göreli değerine göre otomatik olarak oluşturulur.<br><br>Listedeki `Sort` kayıtları sıralamak için sorgudaki komutu kullanın. Sorguyu çalıştırmak ve tüm kayıtları döndürmek için **Tümünü Gör'i**seçebilirsiniz. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| İşlem |Sparkline için gerçekleştirecek işlem. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Ad ve değer ayırıcısı |Metin özelliğini birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="donut-and-list-part"></a>Donut ve liste parçası
Üstbilgi, günlük sorgusunda değer sütunu özetleyen tek bir sayı görüntüler. Donut, ilk üç kaydın sonuçlarını grafiksel olarak görüntüler.

![Donut ve liste görünümü](media/view-designer-parts/view-donut-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Başlık |Üstbilginin üst kısmında görüntülenen metin. |
| Alt başlık |Üstbilginin üst kısmında başlığın altında görüntülenen metin. |
| **Çörek** | |
| Sorgu |Donut için çalıştırmak için sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Çörek** |**> Merkezi** |
| Metin |Donutun içindeki değerin altında görüntülenen metin. |
| İşlem |Değer özelliği üzerinde gerçekleştirmek için işlem tek bir değer olarak özetlemek için.<ul><li>Toplam: Tüm kayıtların değerlerini ekler.</li><li>Yüzde: **Merkez işleminde kullanılan Sonuç değerleri** yle döndürülen kayıtların sorgudaki toplam kayıtlara oranı.</li></ul> |
| Merkez işleminde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işaretini (+) seçin. Sorgunun sonuçları, belirttiğiniz özellik değerleriyle olan kayıtlarla sınırlıdır. Değer eklenmezse, tüm kayıtlar sorguya dahil edilir. |
| **Ek seçenekler** |**> Renkler** |
| Renk 1<br>Renk 2<br>Renk 3 |Donut'ta görüntülenen değerlerin her biri için rengi seçin. |
| **Ek seçenekler** |**> Gelişmiş renk eşleme** |
| Alan değeri |Donuta dahil edilmişse farklı bir renk olarak görüntülemek için bir alanın adını yazın. |
| Renk |Benzersiz alanın rengini seçin. |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| İşlem |Sparkline için gerçekleştirecek işlem. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Ad ve değer ayırıcısı |Metin özelliğini birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="two-timelines-and-list-part"></a>İki zaman çizelgesi ve liste parçası
Üstbilgi, bir günlük sorgusunda değer sütunu özetleyen tek bir sayı görüntüleyen bir ek bilgiyle, zaman içinde iki günlük sorgusunun sonuçlarını sütun grafikleri olarak görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler.

![İki zaman çizelgesi ve liste görünümü](media/view-designer-parts/view-two-timelines-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Başlık Gezintisi** | |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Birinci<br>grafik İkinci grafik** | |
| Açıklama |İlk serinin çağrı dışında görüntülenen metin. |
| Renk |Serideki sütunlar için kullanılacak renk. |
| Sorgu |İlk seri için çalıştırılabilmek için sorgu. Her zaman aralığındaki kayıtların sayısı grafik sütunları tarafından temsil edilir. |
| İşlem |Ek bilgi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirmek için işlem.<ul><li>Toplam: Tüm kayıtlardaki değerlerin toplamı.</li><li>Ortalama: Tüm kayıtlardaki değerlerin ortalaması.</li><li>Son örnek: Grafikte yer alan son aralıktaki değer.</li><li>İlk örnek: Grafikte yer alan ilk aralıktaki değer.</li><li>Sayı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li></ul> |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| İşlem |Sparkline için gerçekleştirecek işlem. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="information-part"></a>Bilgi bölümü
Üstbilgi statik metin ve isteğe bağlı bir bağlantı görüntüler. Liste, statik bir başlık ve metin içeren bir veya daha fazla öğe görüntüler.

![Bilgi görünümü](media/view-designer-parts/view-information.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Renk |Üstbilgi için arka plan rengi. |
| **Üst bilgi** | |
| Görüntü |Üstbilgide görüntülenen resim dosyası. |
| Etiketle |Üstbilgide görüntülenen metin. |
| **Üst bilgi** |**> Link** |
| Etiketle |Bağlantı metni. |
| Url |Bağlantının Url'si. |
| **Bilgi öğeleri** | |
| Başlık |Her öğenin başlığı için görüntülenen metin. |
| İçerik |Her öğe için görüntülenen metin. |

## <a name="line-chart-callout-and-list-part"></a>Satır grafiği, diğer belirtme ve liste parçası
Üstbilgi, zaman içinde bir günlük sorgusundan birden çok seri içeren bir satır grafiği ve özet değere sahip bir ek bilgi görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler.

![Satır grafiği, diğer arama ve liste görünümü](media/view-designer-parts/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Başlık |Üstbilginin üst kısmında görüntülenen metin. |
| Alt başlık |Üstbilginin üst kısmında başlığın altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Satır grafiği için çalıştırılabilmek için sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. Sorgu *aralık* anahtar sözcüğü kullanıyorsa, grafiğin x ekseni bu zaman aralığını kullanır. Sorgu *aralık* anahtar sözcüğü içermiyorsa, x ekseni saatlik aralıkları kullanır. |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Çizgi grafik** |**> Çağrı** |
| Çağrı başlığı |İlan değerinin üzerinde görüntülenen metin. |
| Dizi Adı |Ara bilgi değeri için kullanılacak serinin özellik değeri. Seri sağlanmadıysa, sorgudaki tüm kayıtlar kullanılır. |
| İşlem |Ek bilgi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirmek için işlem.<ul><li>Ortalama: Tüm kayıtlardaki değerlerin ortalaması.</li><li>Sayı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: Grafikte yer alan son aralıktaki değer.</li><li>Max: Grafikte yer alan aralıklardan gelen maksimum değer.</li><li>Min: Grafikte yer alan aralıklardan gelen minimum değer.</li><li>Toplam: Tüm kayıtlardaki değerlerin toplamı.</li></ul> |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçeği kullan |Y ekseni için logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülecek değerler için birimleri belirtin. Bu bilgiler, değer türlerini gösteren grafik etiketlerini görüntülemek ve isteğe bağlı olarak değerleri dönüştürmek için kullanılır. *Birim* türü, birimin kategorisini belirtir ve kullanılabilir *Geçerli Birim* türü değerlerini tanımlar. *Dönüştür'te*bir değer seçerseniz, sayısal değerler *Geçerli Birim* türünden Convert *to type'a* dönüştürülür. |
| Özel etiket |*Birim* türü için etiketin yanındaki y ekseni için görüntülenen metin. Etiket belirtilmemişse, yalnızca *Birim* türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| İşlem |Sparkline için gerçekleştirecek işlem. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Ad ve değer ayırıcısı |Metin özelliğini birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="line-chart-and-list-part"></a>Çizgi grafiği ve liste parçası
Üstbilgi, zaman içinde bir günlük sorgusundan birden çok seriiçeren bir çizgi grafiği görüntüler. Liste, sayısal bir sütunun göreli değerini veya zaman içinde değişimini gösteren bir grafikle bir sorgudan elde edilen ilk on sonucu görüntüler.

![Çizgi grafiği ve liste görünümü](media/view-designer-parts/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| Simgeyi Kullan |Simgeyi görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Başlık |Üstbilginin üst kısmında görüntülenen metin. |
| Alt başlık |Üstbilginin üst kısmında başlığın altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Satır grafiği için çalıştırılabilmek için sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. Sorgu *aralık* anahtar sözcüğü kullanıyorsa, grafiğin x ekseni bu zaman aralığını kullanır. Sorgu *aralık* anahtar sözcüğü içermiyorsa, x ekseni saatlik aralıkları kullanır. |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçeği kullan |Y ekseni için logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülecek değerler için birimleri belirtin. Bu bilgiler, değer türlerini gösteren grafik etiketlerini görüntülemek ve isteğe bağlı olarak değerleri dönüştürmek için kullanılır. *Birim* türü, birimin kategorisini belirtir ve kullanılabilir *Geçerli Birim* türü değerlerini tanımlar. *Dönüştür'te*bir değer seçerseniz, sayısal değerler *Geçerli Birim* türünden Convert *to type'a* dönüştürülür. |
| Özel etiket |*Birim* türü için etiketin yanındaki y ekseni için görüntülenen metin. Etiket belirtilmemişse, yalnızca *Birim* türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılabilmek için sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| Grafiği gizle |Sayısal sütunun sağındaki grafiği devre dışı kalmak için bu bağlantıyı seçin. |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Renk |Çubukların veya ışıltıların rengi. |
| İşlem |Sparkline için gerçekleştirecek işlem. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Ad ve değer ayırıcısı |Metin özelliğini birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [Ortak Ayarlar'a](#sparklines)bakın. |
| Tıklama navigasyonu | Listedeki bir öğeyi tıklattığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Liste** |**> Sütun başlıkları** |
| Adı |İlk sütunun üst kısmında görüntülenen metin. |
| Değer |İkinci sütunun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [Ortak Ayarlar'a](#thresholds)bakın. |

## <a name="stack-of-line-charts-part"></a>Satır grafikleri bölümü yığını
Çizgi grafiği yığını, burada gösterildiği gibi, zaman içinde bir günlük sorgusundan birden fazla seri içeren üç ayrı çizgi grafiği görüntüler:

![Çizgi grafikleri yığını](media/view-designer-parts/view-stack-line-charts.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşemenin üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünümden başlayarak görünümde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üstbilgide sonucun yanında görüntülenen resim dosyası. |
| **Grafik<br>1<br>Grafik 2 Grafik 3** |**> Üstbilgi** |
| Başlık |Grafiğin üst kısmında görüntülenen metin. |
| Alt başlık |Grafiğin üst kısmında ki başlık altında görüntülenen metin. |
| **Grafik<br>1<br>Grafik 2 Grafik 3** |**Çizgi grafik** |
| Sorgu |Satır grafiği için çalıştırılabilmek için sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. Sorgu *aralık* anahtar sözcüğü kullanıyorsa, grafiğin x ekseni bu zaman aralığını kullanır. Sorgu *aralık* anahtar sözcüğü içermiyorsa, x ekseni saatlik aralıkları kullanır. |
| Tıklama navigasyonu | Üstbilgiyi tıklattığınızda alınan eylem.  Daha fazla bilgi için [Ortak Ayarlar'a](#click-through-navigation)bakın. |
| **Grafik** |**> Y ekseni** |
| Logaritmik Ölçeği kullan |Y ekseni için logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülecek değerler için birimleri belirtin. Bu bilgiler, değer türlerini gösteren grafik etiketlerini görüntülemek ve isteğe bağlı olarak değerleri dönüştürmek için kullanılır. *Birim* türü, birimin kategorisini belirtir ve kullanılabilir *Geçerli Birim* türü değerlerini tanımlar. *Dönüştür'te*bir değer seçerseniz, sayısal değerler *Geçerli Birim* türünden Convert *to type'a* dönüştürülür. |
| Özel etiket |*Birim* türü için etiketin yanındaki y ekseni için görüntülenen metin. Etiket belirtilmemişse, yalnızca *Birim* türü görüntülenir. |

## <a name="common-settings"></a>Ortak ayarlar
Aşağıdaki bölümlerde, birkaç görselleştirme parçasında ortak olan ayarlar açıklanmaktadır.

### <a name="name-and-value-separator"></a><a name="name-value-separator"></a>Ad ve değer ayırıcısı
Ad ve değer ayırıcısı, metin özelliğini liste sorgusundan birden çok değere ayırmak için kullanılacak tek karakterli sınırlayıcıdır. Bir sınır layıcı belirtirseniz, **Ad** kutusunda aynı sınırlayıcıyla ayrılmış her alan için adlar sağlayabilirsiniz.

Örneğin, *Redmond-Building 41* ve *Bellevue-Building 12*gibi değerleri içeren *Konum* adlı bir özelliği düşünün. Ad ve değer ayırıcısı için tire (-) ve ad için *Şehir Binası* belirtebilirsiniz. Bu yaklaşım, her değeri *Şehir* ve *Bina*olarak adlandırılan iki özelliğe ayrıştirır.

### <a name="click-through-navigation"></a><a name="click-through-navigation"></a>Tıkla Navigasyon
Tıklatma gezintisi, bir üstbilgiyi veya liste öğeyi görünümde tıklattığınızda hangi eylemin alınacağını tanımlar.  Bu, [Log Analytics'te](../../azure-monitor/log-query/portals.md) bir sorgu açar veya başka bir görünüm başlatacaktır.

Aşağıdaki tabloda tıkla navigasyon için ayarlar açıklanmaktadır.

| Ayar           | Açıklama |
|:--|:--|
| Günlük Arama (Otomatik) | Üstbilgi öğesi seçtiğinizde çalıştırmak için sorgu günlük.  Bu, öğenin temel aldığı aynı günlük sorgusudur.
| Günlük araması        | Listedeki bir öğeyi seçtiğinizde çalıştırmak için sorgu günlükleyin.  Sorguyu Gezinti **sorgu** kutusuna yazın.   Kullanıcının seçtiği öğenin sözdizimini eklemek için *{seçili öğe}* kullanın.  Örneğin, sorguda *Bilgisayar* adında bir sütun varsa ve gezinti sorgusu *{selected item}* ise, bilgisayar seçtiğinizde *Computer="MyComputer"* gibi bir sorgu çalıştırılır. Gezinti sorgusu *Type=Event {selected item}* ise, sorgu *Türü=Olay Bilgisayarı="MyComputer"* çalıştırılır. |
| Görünüm              | Üstbilgi öğesi veya listedeki bir öğeyi seçtiğinizde açmak için görüntüleyin.  **Görünüm Adı** kutusunda çalışma alanınızdaki görünümün adını seçin. |



### <a name="sparklines"></a><a name="sparklines"></a>Sparklines
Sparkline, liste girişinin zaman daki değerini gösteren küçük bir çizgi grafiğidir. Listeli görselleştirme parçaları için, sayısal bir sütunun göreli değerini veya zaman içinde değerini gösteren bir ışıltıyı gösteren yatay bir çubuk gösterip göstermeyeceğini seçebilirsiniz.

Aşağıdaki tabloda sparklines ayarları açıklanır:

| Ayar | Açıklama |
|:--- |:--- |
| Sparklines etkinleştirme |Yatay bir çubuk yerine ışıltı lı bir çizgi görüntülemek için bu bağlantıyı seçin. |
| İşlem |Sparklines etkinse, bu, sparkline değerlerini hesaplamak için listedeki her özellik üzerinde gerçekleştirmek için işlemdir.<ul><li>Son örnek: Zaman aralığındaki serinin son değeri.</li><li>Max: Zaman aralığında seri için maksimum değer.</li><li>Min: Zaman aralığında ki serinin minimum değeri.</li><li>Toplam: Zaman aralığındaki seri değerlerinin toplamı.</li><li>Özet: Üstbilgideki sorguyla aynı `measure` komutu kullanır.</li></ul> |

### <a name="thresholds"></a><a name="thresholds"></a>Eşik
Eşikleri kullanarak, bir listedeki her öğenin yanında renkli bir simge görüntüleyebilirsiniz. Eşikler, belirli bir değeri aşan veya belirli bir aralıkta bulunan öğelerin hızlı bir görsel göstergesini sağlar. Örneğin, kabul edilebilir değeri olan öğeler için yeşil bir simge, değer uyarıyı gösteren bir aralıkiçindeyse sarı ve hata değerini aşıyorsa kırmızı görüntüleyebilirsiniz.

Bir parça için eşikleri etkinleştirdiğinizde, bir veya daha fazla eşik belirtmeniz gerekir. Bir maddenin değeri bir eşik değerinden büyükse ve bir sonraki eşik değerinden daha düşükse, bu değerin rengi kullanılır. Öğe en yüksek eşik değerinden büyükse, başka bir renk kullanılır. 

Her eşik kümesinin **Varsayılan**değeri olan bir eşiği vardır. Bu, başka değerler aşılmazsa ayarlanan renktir. **Ekle** (+) veya **Sil** (x) düğmesini seçerek eşikleri ekleyebilir veya kaldırabilirsiniz.

Aşağıdaki tabloda eşiklerin ayarları açıklanmaktadır:

| Ayar | Açıklama |
|:--- |:--- |
| Eşikleri Etkinleştir |Her değerin solunda bir renk simgesi görüntülemek için bu bağlantıyı seçin. Simge, belirtilen eşiklere göre değerin sistem durumunu gösterir. |
| Adı |Eşik değerinin adı. |
| Eşik |Eşiğin değeri. Her liste öğesinin sistem durumu rengi, öğenin değeri yle aşılmış en yüksek eşik değerinin rengine ayarlanır. Eşik değerleri aşıldığında, varsayılan renk kullanılır. |
| Renk |Eşik değerini gösteren renk. |

## <a name="next-steps"></a>Sonraki adımlar
* Görselleştirme bölümlerindeki sorguları desteklemek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
