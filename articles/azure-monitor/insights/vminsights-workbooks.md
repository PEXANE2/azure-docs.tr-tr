---
title: VM’ler için Azure İzleyici’de çalışma kitaplarıyla etkileşimli raporlar oluşturma
description: VM'ler için Azure Monitor için önceden tanımlanmış ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: a6ab126c3a5b0d2a82b17fac42dcc9e20f6aba3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480462"
---
# <a name="create-interactive-reports-azure-monitor-for-vms-with-workbooks"></a>VM’ler için Azure İzleyici’de çalışma kitaplarıyla etkileşimli raporlar oluşturma

Çalışma kitapları, metin, [günlük sorguları,](../log-query/query-language.md)ölçümler ve parametreleri zengin etkileşimli raporlarda birleştirir. Çalışma kitapları, aynı Azure kaynaklarına erişimi olan diğer ekip üyeleri tarafından değiştirilebilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

* İlgi metriklerini önceden bilmediğiniz zaman sanal makinenizin kullanımını keşfetmek: CPU kullanımı, disk alanı, bellek, ağ bağımlılıkları, vb. Diğer kullanım analizi araçlarının aksine, çalışma kitapları birden fazla görselleştirme ve analiz türünü birleştirerek bu tür serbest biçimli keşifler için harika hale getirmenize izin verirken, bu tür bir araştırma için harika hale getirir.
* Anahtar sayaçları ve diğer günlük olayları için ölçümler göstererek, yakın zamanda sağlanan bir VM'nin nasıl performans gösterdiğini ekibinize açıklar.
* VM'nizin yeniden boyutlandırma deneyinin sonuçlarını ekibinizin diğer üyeleriyle paylaşmak. Denemenin hedeflerini metinle açıklayabilir, ardından denemeyi değerlendirmek için kullanılan her kullanım metrik ve analiz sorgularını ve her bir ölçümün hedefin üstünde mi yoksa altında mı olduğuna ilişkin net çağrı-çıkışları gösterebilirsiniz.
* Bir kesintinin VM'nizin kullanımı üzerindeki etkisini bildirmek, verileri, metin açıklamalarını ve gelecekte kesintileri önlemek için sonraki adımların tartışılmasını birleştirir.

Aşağıdaki tablo, VM'ler için Azure Monitor'un sizi işe başlamak için içerdiği çalışma kitaplarını özetleyin.

| Çalışma Kitabı | Açıklama | Kapsam |
|----------|-------------|-------|
| Performans | Etkinleştirdiğiniz Tüm Log Analytics performans sayaçlarından yararlanan tek bir çalışma kitabında En İyi N Listesi ve Grafikler görünümümüzün özelleştirilebilir bir sürümünü sağlar.| Ölçekte |
| Performans sayaçları | Çok sayıda performans sayacı arasında Top N grafik görünümü. | Ölçekte |
| Bağlantılar | Bağlantılar, izlenen VM'lerinizden gelen ve giden bağlantıların derinlemesine görünümünü sağlar. | Ölçekte |
| Etkin Bağlantı Noktaları | İzlenen VM'lerde bağlantı noktalarına bağlanan işlemlerin ve seçilen zaman dilimindeki etkinliklerinin bir listesini sağlar. | Ölçekte |
| Açık Bağlantı Noktaları | İzlenen VM'lerinizde açık olan bağlantı noktası sayısını ve bu açık bağlantı noktalarındaki ayrıntıları sağlar. | Ölçekte |
| Başarısız Bağlantılar | İzlenen VM'lerinizde başarısız bağlantıların sayısını, hata eğilimini ve zaman içinde hata yüzdesi artıyorsa görüntüleyin. | Ölçekte |
| Güvenlik ve Denetim | Genel bağlantılar, kötü amaçlı bağlantılar ve IP uç noktalarının genel olarak bulunduğu raporlar içeren TCP/IP trafiğinizin analizi.  Tüm özellikleri etkinleştirmek için Güvenlik Algılama'yı etkinleştirmeniz gerekir. | Ölçekte |
| TCP Trafiği | İzlenen VM'leriniz ve bunların bir ızgaradaki gönderilen, alınan ve toplam ağ trafiği için sıralanmış bir rapor ve bir eğilim çizgisi olarak görüntülenir. | Ölçekte |
| Trafik Karşılaştırması | Bu çalışma kitapları, tek bir makine veya bir makine grubu için ağ trafiği eğilimlerini karşılaştırmanızı sağlar. | Ölçekte |
| Performans | Etkinleştirdiğiniz tüm Log Analytics performans sayaçlarından yararlanan Performans görünümümüzün özelleştirilebilir bir sürümünü sağlar. | Tek VM | 
| Bağlantılar | Bağlantılar, VM'nizden gelen ve giden bağlantıların derinlemesine görünümünü sağlar. | Tek VM |
 
## <a name="creating-a-new-workbook"></a>Yeni bir çalışma kitabı oluşturma

Çalışma kitabı, bağımsız olarak kullanılabilir grafikler, tablolar, metin ve giriş denetimlerinden oluşan bölümlerden oluşur. Çalışma kitaplarını daha iyi anlamak için, bir şablon açarak başlayalım ve özel bir çalışma kitabı oluşturmada yürüyelim. 

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. **Sanal Makineleri**seçin.

3. Listeden bir VM seçin.

4. VM sayfasında, **İzleme** **bölümünde, Insights'ı**seçin.

5. VM öngörüleri sayfasında **Performans** veya **Haritalar** sekmesini seçin ve ardından sayfadaki bağlantıdan **Çalışma Kitaplarını** Görüntüle'yi seçin. Açılan listeden **Galeriye Git'i**seçin.

    ![Çalışma kitabı açılır listesinin ekran görüntüsü](media/vminsights-workbooks/workbook-dropdown-gallery-01.png)

    Bu, başlangıç yardımcı olmak için önceden oluşturulmuş çalışma kitapları bir dizi çalışma kitabı galerisi başlattı.

7. **Yeni'yi**seçerek yeni bir çalışma kitabı oluşturun.

    ![Çalışma kitabı galerisinin ekran görüntüsü](media/vminsights-workbooks/workbook-gallery-01.png)

## <a name="editing-workbook-sections"></a>Çalışma kitabı bölümlerini düzenleme

Çalışma kitaplarının iki modu vardır: **düzenleme modu**ve **okuma modu.** Yeni bir çalışma kitabı ilk başlatıldığında, **düzenleme modunda**açılır. Aksi takdirde gizli herhangi bir adım ve parametreler de dahil olmak üzere çalışma kitabının tüm içeriğini gösterir. **Okuma modu** basitleştirilmiş bir rapor stili görünümü sunar. Okuma modu, bir rapor oluşturmaya giden karmaşıklığı soyutlamanızı sağlarken, altta yatan mekaniğin değiştirilmesi gerektiğinde yalnızca birkaç tıklama uzakta olmasını sağlar.

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/workbook-new-workbook-editor-01.png)

1. Bir bölümü düzenlemeyi bitirdiğinizde, bölümün sol alt köşesinde **Ki Düzenleme'yi** tıklatın.

2. Bir bölümün kopyasını oluşturmak için **bu bölüm simgesini Klon'ı** tıklatın. Yinelenen bölümler oluşturmak, önceki yinelemeleri kaybetmeden bir sorguda yinelemenin harika bir yoludur.

3. Çalışma kitabındaki bir bölümü yukarı taşımak için **Yukarı Taşıy** veya Aşağı **Taşı** simgesini tıklatın.

4. Bir bölümü kalıcı olarak kaldırmak için **Kaldır** simgesini tıklatın.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Çalışma kitaplarınıza başlıklar, açıklamalar ve yorumlar eklemek, bir dizi tablo ve grafiğin anlatıya dönüştürülmesine yardımcı olur. Çalışma kitaplarındaki metin bölümleri, başlıklar, kalın, italik ler ve madde işaretli listeler gibi metin biçimlendirmesi için [İşaretleme sözdizimini](https://daringfireball.net/projects/markdown/) destekler.

Çalışma kitabınıza metin bölümü eklemek için çalışma kitabının altındaki veya herhangi bir bölümün altındaki **metin ekle** düğmesini kullanın.

## <a name="adding-query-sections"></a>Sorgu bölümleri ekleme

![Çalışma Kitaplarında sorgu bölümü](media/vminsights-workbooks/005-workbook-query-section.png)

Çalışma kitabınıza sorgu bölümü eklemek için çalışma kitabının altındaki veya herhangi bir bölümün altındaki **sorgu ekle** düğmesini kullanın.

Sorgu bölümleri son derece esnektir ve aşağıdaki gibi soruları yanıtlamak için kullanılabilir:

* Ağ trafiğinde bir artış olarak aynı zaman diliminde CPU kullanımım nasıldı?
* Son bir ay içinde kullanılabilir disk alanında eğilim ne oldu?
* Son iki hafta içinde VM deneyimim kaç ağ bağlantısı hatası oldu? 

Ayrıca, çalışma kitabını başlattırdığınız sanal makinenin bağlamından sorgulama yapmakla da sınırlı değilsiniz. Bu kaynaklara erişim izniniz olduğu sürece, birden çok sanal makinenin yanı sıra Log Analytics çalışma alanlarında sorgu yapabilirsiniz.

Diğer Log Analytics çalışma alanlarından veya **çalışma alanı** tanımlayıcısını kullanarak belirli bir Application Insights uygulamasından gelen verileri eklemek için. Kaynaklar arası sorgular hakkında daha fazla bilgi edinmek için [resmi kılavuza](../log-query/cross-workspace-query.md)bakın.

### <a name="advanced-analytic-query-settings"></a>Gelişmiş analitik sorgu ayarları

Her bölümün ayarlar ![Çalışma Kitapları bölümü düzenleme denetimleri](media/vminsights-workbooks/006-settings.png) simgesi üzerinden erişilebilir kendi gelişmiş ayarları vardır **parametre ekle** düğmesinin sağında bulunan.

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/007-settings-expanded.png)

|         |          |
| ---------------- |:-----|
| **Özel genişlik**    | Bir öğeyi rasgele boyut haline getirir, böylece birçok öğeyi tek bir satıra sığdırarak grafiklerinizi ve tablolarınızı zengin etkileşimli raporlara daha iyi düzenlemenizi sağlar.  |
| **Koşullu görünür** | Okuma modundayken parametreye dayalı adımları gizlemek için belirtin. |
| **Parametre dışa aktarma**| Kılavuzda veya grafikte seçili bir satırın değerleri değiştirmesine veya görünür hale gelmesine sonraki adımların girmesine izin verin.  |
| **Düzenleme yaparken sorgugösterme** | Okuma modundayken bile sorguyu grafiğin veya tablonun üzerinde görüntüler.
| **Düzenleme yapmazken analitik düğmesinde açık göster** | Tek tıklamayla erişime izin vermek için grafiğin sağ köşesine mavi Analytics simgesini ekler.|

Bu ayarların çoğu oldukça sezgiseldir, ancak **bir parametre dışa aktarmak** için bu işlevselliği kullanan bir çalışma kitabı incelemek daha iyidir.

Önceden oluşturulmuş çalışma kitaplarından biri olan **TCP Traffic,** VM'den bağlantı ölçümleri hakkında bilgi sağlar.

Çalışma kitabının ilk bölümü günlük sorgusu verilerine dayanır. İkinci bölüm de günlük sorgusu verilerine dayanır, ancak ilk tabloda bir satır seçmek grafiklerin içeriğini etkileşimli olarak günceller:

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/008-workbook-tcp-traffic.png)

Davranış, **bir öğe seçildiğinde,** tablonun günlük sorgusunda etkinleştirilen bir parametre gelişmiş ayarları dışa aktarın kullanımı yla mümkündür.

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/009-settings-export.png)

İkinci günlük sorgusu, daha sonra bölüm başlığı ve grafikler tarafından kullanılan bir değer kümesi oluşturmak için bir satır seçildiğinde dışa aktarılan değerleri kullanır. Satır seçili değilse, bölüm başlığını ve grafikleri gizler. 

Örneğin, ikinci bölümdeki gizli parametre ızgarada seçilen satırdan aşağıdaki başvuruyu kullanır:

```
VMConnection
| where TimeGenerated {TimeRange}
| where Computer in ("{ComputerName}") or '*' in ("{ComputerName}") 
| summarize Sent = sum(BytesSent), Received = sum(BytesReceived) by bin(TimeGenerated, {TimeRange:grain})
```

## <a name="adding-metrics-sections"></a>Ölçüm bölümleri ekleme

Ölçümler bölümleri, Etkileşimli raporlarınıza Azure Monitor metrik verilerini dahil etmek için tam erişim sunar. Sanal Veriler için Azure Monitor'da, önceden oluşturulmuş çalışma kitapları genellikle metrik veriler yerine analitik sorgu verileri içerir.  Metrik verilere sahip çalışma kitapları oluşturmayı seçerek her iki özelliğin en iyilerinden tek bir yerde tam olarak yararlanmanızı sağlayabilirsiniz. Ayrıca, erişebildiğiniz aboneliklerin herhangi birinde kaynaklardan metrik veri çekme olanağınız da vardır.

Burada sanal makine verilerinin bir çalışma kitabına CPU performansının ızgara görselleştirmesi sağlamak için çekilen bir örneği verilmiştir:

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/010-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parametre bölümleri ekleme

Çalışma kitabı parametreleri, sorguyu veya metin bölümlerini el ile değiştirmek zorunda kalmadan çalışma kitabındaki değerleri değiştirmenize olanak sağlar. Bu, temel analitik sorgu dilini anlama gereksinimini ortadan kaldırır ve çalışma kitabı tabanlı raporlamanın potansiyel hedef kitlesini büyük ölçüde genişletir.

Parametrelerin değerleri sorgu, metin veya diğer parametre bölümlerinde parametrenin adını parantez içinde ``{parameterName}``koyarak değiştirilir. Parametre adları, JavaScript tanımlayıcıları, alfabetik karakterler veya alt çizerler gibi benzer kurallarla sınırlıdır ve bunu alfasayısal karakterler veya alt çizerler takip eder. Örneğin, **a1'e** izin verilir, ancak **1a'ya** izin verilmez.

Parametreler, çalışma kitabının üstünden başlayıp sonraki adımlara doğrulanır.  Çalışma kitabında daha sonra bildirilen parametreler, daha önce bildirilen parametreleri geçersiz kılabilir. Bu, daha önce tanımlanan parametrelerden değerlere erişmek için sorguları kullanan parametrelere de izin verir. Bir parametrenin adımın kendisi içinde, parametreler aynı adımda daha önce bildirilen bir parametreye bağlı olabileceği doğrusal, soldan sağa.
 
Şu anda desteklenen dört farklı parametre türü vardır:

|                  |      |
| ---------------- |:-----|
| **Metin**    | Kullanıcının bir metin kutusunu yeniden atmasına izin verir ve varsayılan değeri doldurmak için isteğe bağlı olarak bir sorgu sağlayabilirsiniz. |
| **Açılan** | Kullanıcının bir değer kümesi arasından seçim yapmasına olanak tanır. |
| **Zaman aralığı seçici**| Kullanıcının önceden tanımlanmış bir zaman aralığı değerleri kümesi arasından seçim yapmasına veya özel bir zaman aralığından seçim yapmasına olanak tanır.|
| **Kaynak seçici** | Kullanıcının çalışma kitabı için seçilen kaynaklararasından seçim yapmasına olanak tanır.|

### <a name="using-a-text-parameter"></a>Metin parametresi kullanma

Bir kullanıcının metin kutusunda yazdığı değer, hiçbir kaçış veya alıntı olmadan doğrudan sorguda değiştirilir. İhtiyacınız olan değer bir dize ise, sorgunun parametre **({parametre}'** gibi) etrafında tırnak işaretleri olmalıdır.

Metin parametresi, metin kutusundaki değerin herhangi bir yerde kullanılmasını sağlar. Bir tablo adı, sütun adı, işlev adı, işleç, vb olabilir.  Metin parametre türü, çalışma kitabı yazarının söz lezizinin varsayılan değerini doldurmak için bir sorgu kullanmasına olanak tanıyan **analitik sorgusundan varsayılan değeri al**bir ayardır.

Günlük sorgusundaki varsayılan değeri kullanırken, varsayılan değer olarak yalnızca ilk satırın (satır 0, sütun 0) ilk değeri kullanılır. Bu nedenle, yalnızca bir satır ve bir sütun dönmek için sorgunuzu sınırlamanız önerilir. Sorgu tarafından döndürülen diğer veriler yoksayılır. 

Sorgu nun döndürdeceği değer ne olursa olsun, doğrudan hiçbir kaçış veya alıntı olmadan değiştirilir. Sorgu satır döndürmezse, parametrenin sonucu boş bir dize (parametre gerekli değilse) veya tanımlanmamış (parametre gerekiyorsa).

### <a name="using-a-drop-down"></a>Açılan bırakma

Açılır parametre türü, bir veya birden çok değerin seçilmesine izin veren bir açılır-aşağı denetimi oluşturmanıza olanak tanır.

Açılır bir günlük sorgusu veya JSON tarafından doldurulur. Sorgu bir sütun döndürürse, bu sütundaki değerler açılır denetimdeki hem değer hem de etikettir. Sorgu iki sütun döndürürse, ilk sütun değerdir ve ikinci sütun açılır açılır da gösterilen etikettir. Sorgu üç sütun döndürürse, üçüncü sütun bu açılır açılır durumda varsayılan seçimi belirtmek için kullanılır. Bu sütun herhangi bir tür olabilir, ancak en basit bool veya sayısal türleri kullanmaktır, burada 0 yanlış ve 1 doğrudur.

Sütun bir dize türüyse, null/boş dize yanlış olarak kabul edilir ve başka bir değer doğru kabul edilir. Tek seçim açılır bırakmaları için, gerçek değeri olan ilk değer varsayılan seçim olarak kullanılır.  Birden çok seçim açılır düşüşleri için, gerçek değeri olan tüm değerler varsayılan seçili küme olarak kullanılır. Açılan daki öğeler, sorgu nun satırlarını iade etmesi durumunda gösterilir. 

Bağlantılara Genel Bakış raporunda bulunan parametrelere bakalım. **Yön'ün**yanındaki yap simgesini tıklatın.

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/011-workbook-using-dropdown.png)

Bu, **Parametreyi Düzenley** menüsü öğesini başlatacaktır.

![VMs Çalışma Kitapları bölüm düzenleme denetimleri için Azure Monitörü](media/vminsights-workbooks/012-workbook-edit-parameter.png)

JSON, içerikle dolu rasgele bir tablo oluşturmanıza olanak tanır. Örneğin, aşağıdaki JSON açılır bırakma iki değer oluşturur:

```
[
    { "value": "inbound", "label": "Inbound"},
    { "value": "outbound", "label": "Outbound"}
]
```

Daha uygulanabilir bir örnek, ada göre bir performans sayaçları kümesinden seçmek için açılır bir düşüş kullanmaktır:

```
Perf
| summarize by CounterName, ObjectName
| order by ObjectName asc, CounterName asc
| project Counter = pack('counter', CounterName, 'object', ObjectName), CounterName, group = ObjectName
```

Sorgu sonuçları aşağıdaki gibi görüntüler:

![Perf sayaç açılır](media/vminsights-workbooks/013-workbook-edit-parameter-perf-counters.png)

Açılır bırakmalar, etkileşimli raporları özelleştirmek ve oluşturmak için inanılmaz derecede güçlü araçlardır.

### <a name="time-range-parameters"></a>Zaman aralığı parametreleri

Açılan parametre türü üzerinden kendi özel zaman aralığı parametrenizi yapabilirsiniz, ancak aynı esneklik derecesine ihtiyacınız yoksa, kutu dan sıcağı zaman aralığı parametre sini de kullanabilirsiniz. 

Zaman aralığı parametre türleri, beş dakikadan son 90 güne kadar değişen 15 varsayılan aralıka sahiptir. Ayrıca, raporun işlecinin zaman aralığı için açık başlangıç ve durdurma değerlerini seçmesine olanak tanıyan özel zaman aralığı seçimine izin verme seçeneği de vardır.

### <a name="resource-picker"></a>Kaynak seçici

Kaynak seçici parametre türü, raporunuzu belirli kaynak türlerine kapsama olanağı sağlar. Kaynak seçici türünden yararlanan önceden oluşturulmuş bir çalışma kitabı örneği **Performans** çalışma kitabıdır.

![Çalışma alanları açılır](media/vminsights-workbooks/014-workbook-edit-parameter-workspaces.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Çalışma kitaplarını kaydetme ve ekibinizle paylaşma

Çalışma kitapları, çalışma kitapları galerisine nasıl eriştiğinize bağlı olarak bir Log Analytics Çalışma Alanı veya sanal makine kaynağına kaydedilir. Çalışma kitabı, size özel olan **Raporlarım** bölümüne veya kaynağa erişimi olan herkesin erişebileceği **Paylaşılan Raporlar** bölümüne kaydedilebilir. Kaynaktaki tüm çalışma kitaplarını görüntülemek için eylem çubuğundaki **Aç** düğmesini tıklatın.

Şu anda **Raporlarım'da**bulunan bir çalışma kitabını paylaşmak için:

1. Eylem çubuğunda **Aç'ı** tıklatın
2. "..." seçeneğini tıklayın. paylaşmak istediğiniz çalışma kitabının yanındaki düğme
3. **Paylaşılan Raporlara Taşı'yı**tıklatın.

Bir çalışma kitabını bir bağlantıyla veya e-posta yoluyla paylaşmak için eylem çubuğunda **Paylaş'ı** tıklatın. Bağlantının alıcılarının çalışma kitabını görüntülemek için Azure portalındaki bu kaynağa erişmeleri gerektiğini unutmayın. Değişiklik yapmak için alıcıların kaynak için en az Katkıda Bulunan izinlerine ihtiyacı vardır.

Çalışma kitabının bağlantısını Azure Panosuna sabitlemek için:

1. Eylem çubuğunda **Aç'ı** tıklatın
2. "..." seçeneğini tıklayın. sabitlemek istediğiniz çalışma kitabının yanındaki düğme
3. **Panoya Sabitle'yi**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

- Sınırlamaları ve genel VM performansını belirlemek için [bkz.](vminsights-performance.md)

- Keşfedilen uygulama bağımlılıkları hakkında bilgi edinmek [için VM Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın.
