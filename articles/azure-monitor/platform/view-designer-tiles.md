---
title: Azure Monitör'de Görünüm Tasarımcısı kutucukları için bir başvuru kılavuzu | Microsoft Dokümanlar
description: Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında görüntülenen özel görünümler oluşturabilir ve Günlük Analizi çalışma alanındaki verilerle ilgili çeşitli görselleştirmeler içerebilir. Bu makale, özel görünümlerinizde kullanılabilen kutucukların ayarlarına yönelik bir başvuru kılavuzudur.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/17/2018
ms.openlocfilehash: 0320be3a2cfbb96367799577a6e56bcf5da87dcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658515"
---
# <a name="reference-guide-to-view-designer-tiles-in-azure-monitor"></a>Azure Monitör'de Tasarımcı kutularını görüntüleme başvuru kılavuzu
Azure Monitörü'nde Görünüm Tasarımcısı'nı kullanarak, Azure portalında Günlük Analizi çalışma alanınızdaki verileri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makale, özel görünümlerinizde kullanılabilen kutucukların ayarlarına yönelik bir başvuru kılavuzudur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Tasarımcıyı Görüntüle](view-designer.md): Görünüm Tasarımcısı'na ve özel görünümler oluşturma ve düzenleme yordamlarına genel bir bakış sağlar.
* [Visualization parçası başvurusu](view-designer-parts.md): Özel görünümlerinizde bulunan görselleştirme parçalarının ayarları için bir başvuru kılavuzu sağlar.


Kullanılabilir Görünüm Tasarımcısı döşemeleri aşağıdaki tabloda açıklanmıştır:  

| Kutucuk | Açıklama |
|:--- |:--- |
| [Numarası](#number-tile) |Bir sorgudaki kayıtların sayısı. |
| [İki sayı](#two-numbers-tile) |İki farklı sorgudaki kayıt sayıları. |
| [Çörek](#donut-tile) | Ortada özet değeri olan bir sorguyu temel alan bir grafik. |
| Satır grafiği ve dış arama | Sorguyu temel alan bir satır grafiği ve özet değeri olan bir dış bilgi. |
| [Çizgi grafik](#line-chart-tile) |Bir sorguya dayalı bir çizgi grafiği. |
| [İki zaman çizelgesi](#two-timelines-tile) | Her biri ayrı bir sorguya dayanan iki serili bir sütun grafiği. |

Sonraki bölümlerde döşeme türleri ve özellikleri ayrıntılı olarak açıklayınız.

> [!NOTE]
> Görünümdeki kutucuklar, Log Analytics çalışma alanınızdaki [günlük sorgularını](../log-query/log-query-overview.md) temel alar. Şu anda Application Insights'tan veri almak için [çapraz kaynak sorgularını](../log-query/cross-workspace-query.md) desteklemez.

## <a name="number-tile"></a>Sayı döşemesi
**Sayı** döşemesi, hem günlük sorgusundaki kayıt sayısını hem de etiketi görüntüler.

![Sayı döşemesi](media/view-designer-tiles/tile-number.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Karo** | |
| Açıklama |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılabilen sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="two-numbers-tile"></a>İki Sayı döşemesi
Bu döşeme, iki farklı günlük sorgusundaki kayıt sayısını ve her biri için bir etiket görüntüler.

![İki Sayı döşemesi](media/view-designer-tiles/tile-two-numbers.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **İlk Kiremit** | |
| Açıklama |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılabilen sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| **İkinci Kiremit** | |
| Açıklama |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılabilen sorgu. Sorgu tarafından döndürülen kayıtların sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="donut-tile"></a>Çörek çini
**Donut** döşemesi, günlük sorgusunda değer sütunu özetleyen tek bir sayı görüntüler. Donut, ilk üç kaydın sonuçlarını grafiksel olarak görüntüler.

![Çörek çini](media/view-designer-tiles/tile-donut.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Çörek** | |
| Sorgu |Donut için çalıştırılabilen sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. |
| **Çörek** |**> Merkezi** |
| Metin |Donutun içindeki değerin altında görüntülenen metin. |
| İşlem |Değer özelliği üzerinde gerçekleştirilen işlem, tek bir değer olarak özetlemek için.<ul><li>Toplam: Özellik değeri ile tüm kayıtların değerlerini ekleyin.</li><li>Yüzde: Tüm kayıtların özetlenen değerleriyle karşılaştırıldığında özellik değeriolan kayıtlardan alınan toplam değerlerin yüzdesi.</li></ul> |
| Merkez işleminde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işaretini (+) seçin. Sorgunun sonuçları, belirttiğiniz özellik değerleriyle olan kayıtlarla sınırlıdır. Değer eklenmezse, tüm kayıtlar sorguya dahil edilir. |
| **Çörek** |**> Ek seçenekler** |
| Renkler |En iyi üç özelliğin her biri için görüntülenen renk. Belirli özellik değerleri için alternatif renkler belirtmek için *Gelişmiş Renk Eşleme'yi*kullanın. |
| Gelişmiş Renk Eşleme |Belirli özellik değerlerini temsil eden bir renk görüntüler. Belirttiğiniz değer ilk üçteyse, standart renk yerine alternatif renk görüntülenir. Özellik ilk üçte değilse, renk görüntülenmez. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="line-chart-tile"></a>Çizgi grafiği döşemesi
Bu döşeme, zaman içinde bir günlük sorgusundan birden çok seri görüntüleyen bir çizgi grafiğidir. 

![Satır grafiği ve çağrı tonusu](media/view-designer-tiles/tile-line-chart.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Satır grafiği için çalışan sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. Sorgu *aralık* anahtar sözcüğü kullanıyorsa, x ekseni bu zaman aralığını kullanır. Sorgu *aralık* anahtar sözcüğü kullanmıyorsa, x ekseni saatlik aralıkları kullanır. |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçeği kullan |Y ekseni için logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimleri belirtin. Bu bilgiler, değer türlerini gösteren etiketleri grafikte görüntülemek ve isteğe bağlı olarak değerleri dönüştürmek için kullanılır. **Birim Türü,** birimin kategorisini belirtir ve kullanılabilir **Geçerli Birim Türü** değerlerini tanımlar. **Dönüştür'te** bir değer seçerseniz, sayısal değerler **Geçerli Birim** türünden **Türe Dönüştür'e** dönüştürülür. |
| Özel etiket |*Birim* türü için etiketin yanındaki y ekseni için görüntülenen metin. Etiket belirtilmemişse, yalnızca *Birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="line-chart-and-callout-tile"></a>Satır grafiği ve çağrı tonusu
Bu döşeme, hem zaman içinde bir günlük sorgusundan birden çok seri görüntüleyen bir satır grafiğine hem de özet değeri olan bir ek bilgiye sahiptir. 

![Satır grafiği ve çağrı tonusu](media/view-designer-tiles/tile-line-chart-callout.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Satır grafiği için çalışan sorgu. İlk özellik bir metin değeri, ikinci özellik sayısal bir değerdir. Bu sorgu, normalde sonuçları özetlemek için *ölçü* anahtar sözcük kullanır. Sorgu *aralık* anahtar sözcüğü kullanıyorsa, x ekseni bu zaman aralığını kullanır. Sorgu *aralık* anahtar sözcüğü kullanmıyorsa, x ekseni saatlik aralıkları kullanır. |
| **Çizgi grafik** |**> Çağrı** |
| Çağrı başlığı | İlan değerinin üzerinde görüntülenen metin. |
| Seri adı |Ara bilgi değeri olarak kullanılacak seri özellik değeri. Seri sağlanmadıysa, sorgudaki tüm kayıtlar kullanılır. |
| İşlem |Ek bilgi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardaki değerlerin ortalaması.</li><li>Sayı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: Grafikte yer alan son aralığın değeri.</li><li>Max: Grafikte yer alan aralıkların maksimum değeri.</li><li>Min: Grafikte yer alan aralıkların minimum değeri.</li><li>Toplam: Tüm kayıtlardaki değerlerin toplamı.</li></ul> |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçeği kullan |Y ekseni için logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülecek değerler için birimleri belirtin. Bu bilgiler, değer türlerini gösteren grafik etiketlerini görüntülemek ve isteğe bağlı olarak değerleri dönüştürmek için kullanılır. *Birim* türü, birimin kategorisini belirtir ve kullanılabilir *Geçerli Birim* türü değerlerini tanımlar. *Dönüştür'te*bir değer seçerseniz, sayısal değerler *Geçerli Birim* türünden Convert *to type'a* dönüştürülür. |
| Özel etiket |*Birim* türü için etiketin yanındaki y ekseni için görüntülenen metin. Etiket belirtilmemişse, yalnızca *Birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="two-timelines-tile"></a>İki zaman çizelgesi döşemesi
**İki zaman çizelgesi** döşemesi, zaman içinde iki günlük sorgusunun sonuçlarını sütun grafikleri olarak görüntüler. Her seri için bir çağrı görüntülenir. 

![İki zaman çizelgesi döşemesi](media/view-designer-tiles/tile-two-timelines.png)

| Ayar | Açıklama |
|:--- |:--- |
| Adı |Döşemenin üst kısmında görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| İlk Grafik | |
| Açıklama |İlk serinin çağrı dışında görüntülenen metin. |
| Renk |İlk serideki sütunlar için kullanılan renk. |
| Grafik sorgusu |İlk seri için çalıştırılabilen sorgu. Her zaman aralığındaki kayıtların sayısı grafik sütunları tarafından temsil edilir. |
| İşlem |Ek bilgi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardaki değerlerin ortalaması.</li><li>Sayı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: Grafikte yer alan son aralığın değeri.</li><li>Max: Grafikte yer alan aralıkların maksimum değeri.</li></ul> |
| **İkinci grafik** | |
| Açıklama |İkinci serinin çağrı nın altında görüntülenen metin. |
| Renk |İkinci serideki sütunlar için kullanılan renk. |
| Grafik Sorgusu |İkinci seri için çalıştırılabilen sorgu. Her zaman aralığındaki kayıtların sayısı grafik sütunları tarafından temsil edilir. |
| İşlem |Ek bilgi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardaki değerlerin ortalaması.</li><li>Sayı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: Grafikte yer alan son aralığın değeri.</li><li>Max: Grafikte yer alan aralıkların maksimum değeri. |
| **Gelişmiş** |**> Veri akışı doğrulaması** |
| Etkin |Döşeme için veri akışı doğrulaması etkinleştirilmişse bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşım alternatif bir ileti sağlar. Görünümün yüklenmesi ve verilerin kullanılabilir hale gelmesi geçici dönemde bir ileti sağlamak için normalde bu yaklaşımı kullanırsınız. |
| Sorgu |Görünüm için kullanılabilir veri olup olmadığını belirlemek için çalıştırılabilen sorgu. Sorgu sonuç döndürmezse, ana sorgunun değerinin yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürmezse görüntülenen ileti. İleti sağlamazsanız, *Performans Değerlendirmesi* durum iletisi görüntülenir. |


## <a name="next-steps"></a>Sonraki adımlar
* Kutucuktaki sorguları desteklemek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
* Özel görünümünüze [görselleştirme parçaları](view-designer-parts.md) ekleyin.
