---
title: Azure Monitor çalışma kitaplarıyla etkileşimli raporlar oluşturun | Microsoft dokümanlar
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 15543f7f761c707e8eff8e0cc0a0e4532475ddf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671010"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Azure Monitor çalışma kitaplarıyla etkileşimli raporlar oluşturun

Çalışma kitapları, metin, [Analitik sorguları,](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)Azure Ölçümleri ve parametreleri zengin etkileşimli raporlarda birleştirir. Çalışma kitapları, aynı Azure kaynaklarına erişimi olan diğer ekip üyeleri tarafından değiştirilebilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

* İlgi metriklerini önceden bilmediğiniz durumlarda uygulamanızın kullanımını keşfetmek: kullanıcı sayısı, bekletme oranları, dönüşüm oranları, vb. Diğer kullanım analizi araçlarının aksine, çalışma kitapları birden fazla görselleştirme ve analiz türünü birleştirerek bu tür serbest biçimli keşifler için harika hale getirmenize izin verirken, bu tür bir araştırma için harika hale getirir.
* Anahtar etkileşimler ve diğer ölçümler için kullanıcı sayılarını göstererek, yeni yayımlanan bir özelliğin nasıl performans gösterdiğini ekibinize açıklar.
* Uygulamanızdaki Bir A/B denemesinin sonuçlarını ekibinizin diğer üyeleriyle paylaşmak. Denemenin hedeflerini metinle açıklayabilir, ardından denemeyi değerlendirmek için kullanılan her kullanım ölçümü ve Analytics sorgusunu ve her bir ölçümün hedefin üstünde mi yoksa altında mı olduğuna ilişkin net çağrı-çıkışları gösterebilirsiniz.
* Bir kesintinin uygulamanızın kullanımı üzerindeki etkisini bildirmek, verileri, metin açıklamayı ve gelecekte kesintileri önlemek için sonraki adımların tartışılmasını birleştirir.

## <a name="starting-with-a-template-or-saved-workbook"></a>Şablon veya kaydedilmiş çalışma kitabıyla çalışmaya başlama

Çalışma kitabı, bağımsız olarak kullanılabilir grafikler, tablolar, metin ve giriş denetimlerinden oluşan bölümlerden oluşur. Çalışma kitaplarını daha iyi anlamak için, en iyisi birini açmaktır. 

Uygulamanız için Application Insights deneyiminin içinden sol menüden **Çalışma Kitapları'nı** seçin.

![Çalışma kitaplarında gezinme ekran görüntüsü](./media/usage-workbooks/001-workbooks.png)

Bu, başlamanıza yardımcı olmak için önceden oluşturulmuş çalışma kitapları içeren bir çalışma kitabı galerisi başlayır.

![Çalışma kitabı galerisinin ekran görüntüsü](./media/usage-workbooks/002-workbook-gallery.png)

**Hızlı başlangıç**başlığı altında bulunan **Varsayılan Şablon**ile başlayacağız.

![Çalışma kitabı galerisinin ekran görüntüsü](./media/usage-workbooks/003-default-template.png)

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Çalışma kitabı bölümlerini düzenleme, yeniden düzenleme, klonlama ve silme

Çalışma kitaplarının iki modu vardır: **düzenleme modu**ve **okuma modu.** Varsayılan çalışma kitabı ilk başlatıldığında, **düzenleme modunda**açılır. Bu, aksi takdirde gizli olan adımlar ve parametreler de dahil olmak üzere çalışma kitabının tüm içeriğini gösterir. **Okuma modu** basitleştirilmiş bir rapor stili görünümü sunar. Bu, bir rapor oluşturmaya giden karmaşıklığı soyutlamanızı sağlarken, altta yatan mekaniğin değiştirilmesi gerektiğinde yalnızca birkaç tıklama uzakta olmasını sağlar.

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/editing-controls-new.png)

1. Bir bölümü düzenlemeyi bitirdiğinizde, bölümün sol alt **köşesindeKi Düzenleme Bitti'yi** tıklatın.

2. Bir bölümün kopyasını oluşturmak için **bu bölüm simgesini Klon'ı** tıklatın. Yinelenen bölümler oluşturmak, önceki yinelemeleri kaybetmeden bir sorguda yinelemenin harika bir yoludur.

3. Çalışma kitabındaki bir bölümü yukarı taşımak için **Yukarı Taşıy** veya Aşağı **Taşı** simgesini tıklatın.

4. Bir bölümü kalıcı olarak kaldırmak için **Kaldır** simgesini tıklatın.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Çalışma kitaplarınıza başlıklar, açıklamalar ve yorumlar eklemek, bir dizi tablo ve grafiğin anlatıya dönüştürülmesine yardımcı olur. Çalışma kitaplarındaki metin bölümleri, başlıklar, kalın, italik ler ve madde işaretli listeler gibi metin biçimlendirmesi için [İşaretleme sözdizimini](https://daringfireball.net/projects/markdown/) destekler.

Çalışma kitabınıza metin bölümü eklemek için çalışma kitabının altındaki veya herhangi bir bölümün altındaki **metin ekle** düğmesini kullanın.

## <a name="adding-query-sections"></a>Sorgu bölümleri ekleme

![Çalışma Kitaplarında sorgu bölümü](./media/usage-workbooks/analytics-section-new.png)

Çalışma kitabınıza sorgu bölümü eklemek için çalışma kitabının altındaki veya herhangi bir bölümün altındaki **sorgu ekle** düğmesini kullanın.

Sorgu bölümleri son derece esnektir ve aşağıdaki gibi soruları yanıtlamak için kullanılabilir:

* Siteniz kullanımda düşüş olarak aynı zaman diliminde kaç özel durum attı?
* Bazı sayfayı görüntüleyen kullanıcılar için sayfa yükleme sürelerinin dağılımı neydi?
* Sitenizdeki bazı sayfa kümesini kaç kullanıcı görüntüledi, ancak başka bir sayfa kümesini görüntülemedi? Bu, sitenizin işlevselliğinin farklı alt kümelerini kullanan kullanıcı kümeleriniz varsa bunu `join` anlamak için `kind=leftanti` yararlı olabilir [(Kusto sorgu dilinde](/azure/kusto/query/)değiştirici ile işleci kullanın).

Ayrıca, çalışma kitabını başlattığınız uygulama bağlamından sorgulama yla da sınırlı değilsiniz. Bu kaynaklara erişim izniniz olduğu sürece birden çok Application Insights tarafından izlenen uygulamalarda ve Log Analytics çalışma alanlarında sorgu yapabilirsiniz.

Ek harici Application Insights kaynaklarından sorgulamak için **uygulama** tanımlayıcısını kullanın.

```
union app('app01').requests, app('app02').requests, requests
| summarize count() by bin(timestamp, 1h)
```  

Bu sorgu, üç farklı uygulamadan gelen istekleri birleştiriyor. app01 adlı bir uygulama, app02 adlı bir uygulama ve yerel Application Insights kaynağından gelen istekler.

Harici bir Log Analytics çalışma alanından veri çekmek için **çalışma alanı** tanımlayıcısını kullanın.

Kaynaklar arası sorgular hakkında daha fazla bilgi edinmek için [resmi kılavuza](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search)bakın.

### <a name="advanced-analytic-query-settings"></a>Gelişmiş analitik sorgu ayarları

Her bölümün, ![ **ayarlar** simgesi Application Insights Workbooks bölüm düzenleme denetimleri](./media/usage-workbooks/005-settings.png) üzerinden erişilebilen kendi gelişmiş ayarları vardır.

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/0006-settings-expanded.png)

   |         |          |
   | ---------------- |:-----|
   | **Özel genişlik**    | Bunu, bir öğeyi rasgele boyut yapacak şekilde ayarlayın, böylece birçok öğeyi tek bir satıra sığdırarak grafiklerinizi ve tablolarınızı daha iyi zengin etkileşimli raporlara dönüştürebilirsiniz.  |
   | **Koşullu görünür** | Okuma modundayken parametreye dayalı adımları gizlemek için bunu kullanın. |
   | **Parametre dışa aktarma**| Bu, ızgara veya grafikte seçili bir satırın değerleri değiştirmek veya görünür hale getirmek için daha sonraki adımları neden sağlar.  |
   | **Düzenleme yaparken sorgugösterme** | Bu, okuma modundayken bile grafiğin veya tablonun üstündeki sorguyu görüntüler.
   | **Düzenleme yapmazken analitik düğmesinde açık göster** | Bu, tek tıklamayla erişime izin vermek için grafiğin sağ köşesine mavi Analytics simgesini ekler.|

Bu ayarların çoğu oldukça sezgiseldir, ancak **bir parametre dışa aktarmak** için bu işlevselliği kullanan bir çalışma kitabı incelemek daha iyidir.

Önceden oluşturulmuş çalışma kitaplarından biri Etkin Kullanıcılar hakkında bilgi sağlar.

Çalışma kitabının ilk bölümü Analitik sorgu verilerini temel almaktadır:

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/003-active-users.png)

İkinci bölüm aynı zamanda analitik sorgu verilerine de dayanır, ancak ilk tabloda bir satır seçmek grafiğin içeriğini etkileşimli olarak günceller:

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/004-active-users-trend.png)

 Bu, bir öğe **seçildiğinde,** tablonun Analytics sorgusunda etkinleştirilmiş bir parametre gelişmiş ayarları dışa aktarın yoluyla mümkündür.

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/007-settings-export.png)

İkinci analitik sorgusu, bir satır seçildiğinde dışa aktarılan değerleri kullanır. Satır seçili değilse, genel değerleri temsil eden satırvarsayılan. 

```
let start = startofday(ago({TimeRange} + {Metric}));
union customEvents, pageViews
| where timestamp >= start
| where name in ({Activities}) or '*' in ({Activities}) or ('%' in ({Activities}) and itemType == 'pageView') or ('#' in ({Activities}) and itemType == 'customEvent')
{OtherFilters}
| where '{Filter}' == '' or '{Filter}' == '🔸 Overall' or {AnalyzeBy} == replace('🔹 ', '', '{Filter}')
| evaluate activity_engagement(user_Id, timestamp, start, now(), 1d, {Metric})
| where timestamp >= startofday(ago({TimeRange}))
| project timestamp, ["Active User"] = dcount_activities_outer
| render timechart 
```

## <a name="adding-metrics-sections"></a>Ölçüm bölümleri ekleme

Ölçümler bölümleri, Etkileşimli raporlarınıza Azure Monitor metrik verilerini dahil etmek için tam erişim sunar. Önceden oluşturulmuş çalışma kitaplarının çoğu, hem analitik sorgu verileri hem de metrik veriler içererek her iki özelliğin en iyilerinden tek bir yerde tam olarak yararlanmanızı sağlar. Ayrıca, erişebildiğiniz aboneliklerin herhangi birinde kaynaklardan metrik veri çekme olanağınız da vardır.

Burada Sanal makine verilerinin bir çalışma kitabına CPU performansının ızgara görselleştirmesi sağlamak için çekilen bir örneği:

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/008-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parametre bölümleri ekleme

Çalışma kitabı parametreleri, sorguyu veya metin bölümlerini el ile değiştirmek zorunda kalmadan çalışma kitabındaki değerleri değiştirmenize olanak sağlar.  Bu, temel analitik sorgu dilini anlama gereksinimini ortadan kaldırır ve çalışma kitabı tabanlı raporlamanın potansiyel hedef kitlesini büyük ölçüde genişletir.

Parametrelerin değerleri sorgu, metin veya diğer parametre bölümlerinde parametrenin adını parantez içinde ``{parameterName}``koyarak değiştirilir.  Parametre adları JavaScript tanımlayıcıları, temelde alfabetik karakterler veya alt çizerler gibi benzer kurallarla sınırlıdır ve bunu alfasayısal karakterler veya alt çizerler takip eder. Örneğin, **a1'e** izin verilir, ancak **1a'ya** izin verilmez.

Parametreler, çalışma kitabının üstünden başlayıp sonraki adımlara doğrulanır.  Çalışma kitabında daha sonra bildirilen parametreler, daha yukarıya doğru bildirilenparametreleri geçersiz kılabilir.  Bu, daha yukarıda tanımlanan parametrelerden değerlere erişmek için sorguları kullanan parametrelere de izin verir.  Bir parametrenin adımın kendisi içinde, parametreler aynı adımda daha önce bildirilen bir parametreye bağlı olabileceği doğrusal, soldan sağa.
 
Şu anda desteklenen dört farklı parametre türü vardır:

  |         |          |
   | ---------------- |:-----|
   | **Metin**    | kullanıcı bir metin kutusunu yeniden düzenlenecektir ve varsayılan değeri doldurmak için isteğe bağlı olarak bir sorgu sağlayabilirsiniz. |
   | **Açılan** | Kullanıcı bir değer kümesi arasından seçim yapacaktır. |
   | **Zaman aralığı seçici**| Kullanıcı, önceden tanımlanmış bir zaman aralığı değerleri kümesi arasından seçim yapacak veya özel bir zaman aralığından seçim yapacaktır.|
   | **Kaynak seçici** | Kullanıcı, çalışma kitabı için seçilen kaynaklar arasından seçim yapacaktır.|

### <a name="using-a-text-parameter"></a>Metin parametresi kullanma

Bir kullanıcının textbox'ta yazdığı değer, hiçbir kaçış veya alıntı olmadan doğrudan sorguda değiştirilir. İhtiyacınız olan değer bir dize ise, sorgunun parametre **({parametre}'** gibi) etrafında tırnak işaretleri olmalıdır.

Bu, metin kutusundaki değerin herhangi bir yerde kullanılmasını sağlar. Bir tablo adı, sütun adı, işlev adı, işleç, vb olabilir.

Metin parametre türü, çalışma kitabı yazarının söz lezizin varsayılan değerini doldurmak için bir sorgu kullanmasına olanak tanıyan **analitik sorgusundan varsayılan değeri al**bir ayardır.

Bir analitik sorgusundaki varsayılan değeri kullanırken, varsayılan değer olarak yalnızca ilk satırın (satır 0, sütun 0) ilk değeri kullanılır. Bu nedenle, yalnızca bir satır ve bir sütun dönmek için sorgunuzu sınırlamanız önerilir. Sorgu tarafından döndürülen diğer veriler yoksayılır. 

Sorgu nun döndürdeceği değer ne olursa olsun, doğrudan hiçbir kaçış veya alıntı olmadan değiştirilir. Sorgu satır döndürmezse, parametrenin sonucu boş bir dize (parametre gerekli değilse) veya tanımlanmamış (parametre gerekiyorsa).

### <a name="using-a-dropdown"></a>Açılır bırakma kullanma

Açılır parametre türü, bir veya birden çok değerin seçilmesine izin veren bir açılır bırakma denetimi oluşturmanıza olanak tanır.

Açılır bırakma, bir analiz sorgusu tarafından doldurulur. Sorgu bir sütun döndürürse, bu sütundaki değerler açılır bırakma denetimindeki **hem değer** hem de **etikettir.** Sorgu iki sütun döndürürse, ilk sütun **değerdir**ve ikinci sütun açılır açılır listede gösterilen **etikettir.**  Sorgu üç sütun döndürürse, üçüncü sütun bu açılır açılır daki varsayılan seçimi belirtmek için kullanılır.  Bu sütun herhangi bir tür olabilir, ancak en basit bool veya sayısal türleri kullanmaktır, burada 0 yanlış ve 1 doğrudur.

 Sütun bir dize türüyse, null/boş dize yanlış olarak kabul edilir ve başka bir değer doğru kabul edilir. Tek seçim açılır bırakmaları için, gerçek değeri olan ilk değer varsayılan seçim olarak kullanılır.  Birden çok seçim açılır geçişleri için, gerçek değeri olan tüm değerler varsayılan seçili küme olarak kullanılır. Açılan açılır daki öğeler, sorgunun satırlarını iade etmesi durumunda gösterilir. 

Etkin Kullanıcılar raporunda bulunan parametrelere bakalım. **TimeRange'in**yanındaki edit simgesini tıklatın.

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/009-time-range.png)

Bu, Parametreyi Düzenley ekiöğeyi başlatacaktır:

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/010-time-range-edit.png)

Sorgu, analiz sorgu dilinin bir özelliğini kullanır ve bu tablo, içerikle dolu, yoktan var olan keyfi **bir** tablo oluşturmanıza olanak tanır! Örneğin, aşağıdaki analitik sorgu:

```
datatable( column1:string, column2:string )
[
 "row 1 column 1", "row 1 column 2",
"row 2 column 1", "row 2 column 2"
]
```

Sonucu oluşturur:

![Uygulama Öngörüleri Çalışma Kitapları bölüm düzenleme denetimleri](./media/usage-workbooks/011-data-table.png)

Daha uygulanabilir bir örnek, bir dizi ülkeden/bölgeden ada göre seçmek için açılır bir düşüş kullanmaktır:

```
customEvents
| where timestamp >= ago(14d)
| summarize count() by client_CountryOrRegion
| top 100 by count_
| project  client_CountryOrRegion 
| order by client_CountryOrRegion asc
```

Sorgu sonuçları aşağıdaki gibi görüntüler:

![Ülke açılır düşüşü](./media/usage-workbooks/012-country-dropdown.png)

Açılır bırakmalar, etkileşimli raporları özelleştirmek ve oluşturmak için inanılmaz derecede güçlü araçlardır.

### <a name="time-range-parameters"></a>Zaman aralığı parametreleri

Açılan parametre türü üzerinden kendi özel zaman aralığı parametrenizi yapabilirsiniz, ancak aynı esneklik derecesine ihtiyacınız yoksa, kutu dan sıcağı zaman aralığı parametre sini de kullanabilirsiniz. 

Zaman aralığı parametre türleri, beş dakikadan son 90 güne kadar değişen 15 varsayılan aralıka sahiptir. Ayrıca, raporun işlecinin zaman aralığı için açık başlangıç ve durdurma değerlerini seçmesine olanak tanıyan özel zaman aralığı seçimine izin verme seçeneği de vardır.

### <a name="resource-picker"></a>Kaynak seçici

Kaynak seçici parametre türü, raporunuzu belirli kaynak türlerine kapsama olanağı sağlar. Kaynak seçici türünden yararlanan önceden oluşturulmuş çalışma kitabı **örneği, Failure Insights** çalışma kitabıdır.

![Ülke açılır düşüşü](./media/usage-workbooks/013-resource-picker.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Çalışma kitaplarını kaydetme ve ekibinizle paylaşma

Çalışma kitapları, size özel olan **Raporlarım** bölümünde veya Application Insights kaynağına erişimi olan herkesin erişebileceği **Paylaşılan Raporlar** bölümünde bir Application Insights kaynağına kaydedilir. Kaynaktaki tüm çalışma kitaplarını görüntülemek için eylem çubuğundaki **Aç** düğmesini tıklatın.

Şu anda **Raporlarım'da**bulunan bir çalışma kitabını paylaşmak için:

1. Eylem çubuğunda **Aç'ı** tıklatın
2. "..." seçeneğini tıklayın. paylaşmak istediğiniz çalışma kitabının yanındaki düğme
3. **Paylaşılan Raporlara Taşı'yı**tıklatın.

Bir çalışma kitabını bir bağlantıyla veya e-posta yoluyla paylaşmak için eylem çubuğunda **Paylaş'ı** tıklatın. Bağlantının alıcılarının çalışma kitabını görüntülemek için Azure portalındaki bu kaynağa erişmeleri gerektiğini unutmayın. Değişiklik yapmak için alıcıların kaynak için en az Katkıda Bulunan izinlerine ihtiyacı vardır.

Çalışma kitabının bağlantısını Azure Panosuna sabitlemek için:

1. Eylem çubuğunda **Aç'ı** tıklatın
2. "..." seçeneğini tıklayın. sabitlemek istediğiniz çalışma kitabının yanındaki düğme
3. **Panoya Sabitle'yi**tıklatın.

## <a name="contributing-workbook-templates"></a>Katkıda bulunan çalışma kitabı şablonları

Harika bir çalışma kitabı şablonu oluşturdunuz ve bunu toplulukla paylaşmak mı istiyorsunuz? Daha fazla bilgi için [GitHub repomuzu](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım deneyimlerini etkinleştirmek için [özel etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views)göndermeye başlayın.
- Zaten özel etkinlikler veya sayfa görünümleri gönderiyorsanız, kullanıcıların hizmetinizi nasıl kullandığını öğrenmek için Kullanım araçlarını keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](../../azure-monitor/app/usage-segmentation.md)
    - [Huniler](../../azure-monitor/app/usage-funnels.md)
    - [Bekletme](../../azure-monitor/app/usage-retention.md)
    - [Kullanıcı Akışları](../../azure-monitor/app/usage-flows.md)
    - [Kullanıcı bağlamı ekleme](../../azure-monitor/app/usage-send-user-context.md)
