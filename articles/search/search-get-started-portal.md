---
title: Azure portalında arama dizini oluşturma
titleSuffix: Azure Cognitive Search
description: Bu Azure portalında hızlı bir şekilde başlatın, Azure Bilişsel Arama'da ilk arama dizininizi oluşturmak, yüklemek ve sorgulamak için Veri Alma sihirbazını kullanın.
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 02/10/2020
ms.openlocfilehash: 8324ca0184c508591fa4568175bad0f606f952a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369465"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-the-azure-portal"></a>Hızlı başlangıç: Azure portalında Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [C #](search-get-started-dotnet.md)
> * [Java](search-get-started-java.md)
> * [Node.js](search-get-started-nodejs.md)
> * [Powershell](search-get-started-powershell.md)
> * [Postman](search-get-started-postman.md)
> * [Python](search-get-started-python.md)

Kavramları hızla hızlandırmak ve birkaç dakika içinde bir dizin karşı ilginç sorgular yazmak için portalın **İçe Veri** sihirbazı ve **Arama gezgini** araçlarını kullanın.

Araçlar çok sınırlayıcıysa, [.NET'te Azure Bilişsel Arama'yı programlamak için kod tabanlı](search-howto-dotnet-sdk.md) bir giriş yapmayı düşünebilir veya [REST API aramaları yapmak için Postacı'yı](search-get-started-postman.md)kullanabilirsiniz. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

[Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

### <a name="check-for-space"></a>Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur.

Hizmet panosundaki bölümler, kaç dizin, dizin ve veri kaynağınız olduğunu gösterir. 

![Dizinler, dizinler ve veri kaynakları listeleri](media/search-get-started-portal/tiles-indexers-datasources.png)

## <a name="create-an-index-and-load-data"></a><a name="create-index"></a>Dizin oluşturma ve veri yükleme

Arama sorguları aranabilir veriler, meta veriler ve arama davranışlarını iyileştiren ek yapılar içeren bir [*dizin*](search-what-is-an-index.md) kullanır.

Bu öğretici için, [ **İçe Aktar veri** sihirbazı](search-import-data-portal.md)aracılığıyla bir [*dizin oluşturucu*](search-indexer-overview.md) kullanılarak taranabilir yerleşik bir örnek veri kümesi kullanırız. Dizin oluşturucu, kaynağa özgü bir gezgindir ve desteklenen Azure veri kaynaklarındaki meta verileri ve içeriği okuyabilir. Normalde dizinleyiciler programlı olarak kullanılır, ancak portalda bunları **Alma veri** sihirbazı aracılığıyla erişebilirsiniz. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>Adım 1 - Veri Alma sihirbazını başlatın ve bir veri kaynağı oluşturun

1. Azure Bilişsel Arama hizmeti panosunda, arama dizini oluşturmak ve doldurmak için komut çubuğundaki **Verileri İçe Aktar'ı** tıklatın.

   ![Verileri içeri aktar komutu](media/search-get-started-portal/import-data-cmd.png)

2. Sihirbazda, **verilerinize** > Bağlan'ı tıklatın**Örnekler** > **oteller-örnek**. Bu veri kaynağı yerleşiktir. Kendi veri kaynağınızı oluşturuyorsanız, bir ad, tür ve bağlantı bilgileri belirtmeniz gerekir. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

   ![Örnek veri kümesi seçme](media/search-get-started-portal/import-datasource-sample.png)

3. Sonraki sayfaya devam edin.

### <a name="step-2---skip-the-enrich-content-page"></a>Adım 2 - "İçeriği zenginleştir" sayfasını atla

Sihirbaz, Bilişsel Hizmetler AI algoritmalarını dizine dahil etmek için bir [AI zenginleştirme ardışık ardışık lığı](cognitive-search-concept-intro.md) oluşturulmasını destekler. 

Şimdilik bu adımı atlayacağız ve doğrudan **hedef dizini özelleştir'e**geçeceğiz.

   ![Bilişsel beceri adımını atlama](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> [Hızlı bir başlangıç](cognitive-search-quickstart-blob.md) veya [öğretici](cognitive-search-tutorial-blob.md)olarak Bir AI dizine örnek adım atabilirsiniz.

### <a name="step-3---configure-index"></a>Adım 3 - Yapıldizim

Genellikle, dizin oluşturma veri yüklemeden önce tamamlanmış kod tabanlı bir alıştırmadır. Ancak, bu öğreticinin gösterdiği gibi, sihirbaz tarayabileceği herhangi bir veri kaynağı için temel bir dizin oluşturabilir. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir. Ayrıca, otomatik tamamlama veya önerilen sorgular istiyorsanız dil çözümleyicileri veya öneriyiler belirtebilirsiniz.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*.

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. Bu onay kutusunu temizleyerek tek tek alanları arama sonuçları için sınır dışı olarak işaretleyebilirsiniz, örneğin yalnızca filtre ifadelerinde kullanılan alanlar için.
* **Anahtar** benzersiz belge tanımlayıcısıdır. Her zaman bir dize, ve gereklidir.
* **Filtrelenebilir**, **Sıralanabilir**ve **Facetable** alanları bir filtre, sıralama veya yönlü navigasyon yapısında kullanılıp kullanılmadığını belirler.
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir.

Depolama gereksinimleri seçiminizin bir sonucu olarak değişmez. Örneğin, **Alınabilen** özniteliği birden çok alanda ayarlarsanız, depolama gereksinimleri artmaz.

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. *Dizeleri* **Alınabilen** ve **Aranabilir**olarak atfedilir. *İntegerler* **Alınabilen**, **Filtrelenebilir**, **Sıralanabilir**ve **Facetable**olarak atfedilir.

1. Varsayılanları kabul edin. 

   Sihirbazı varolan bir otel veri kaynağını kullanarak ikinci kez yeniden çalıştırarsanız, dizin varsayılan özniteliklerle yapılandırılamayacaktır. Gelecekteki içe almalarda öznitelikleri el ile seçmeniz gerekir. 

   ![Oluşturulan oteller endeksi](media/search-get-started-portal/hotelsindex.png)

2. Sonraki sayfaya devam edin.


### <a name="step-4---configure-indexer"></a>Adım 4 - Yapıddin dizini

**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın.

Bu nesne, yürütülebilir bir işlemi tanımlar. Yinelenen zamanlamaya koyabilirsiniz, ancak şimdilik dizinleyiciyi hemen çalıştırmak için varsayılan seçeneği kullanın.

Dizin oluşturup aynı anda çalıştırmak için **Gönder'i** tıklatın.

  ![oteller dizinleyici](media/search-get-started-portal/hotels-indexer.png)

## <a name="monitor-progress"></a>İlerlemeyi izleme

Sihirbaz, ilerlemeyi izleyebileceğiniz Dizinleyiciler listesine götürmelidir. Kendi kendine gezinme için Genel Bakış sayfasına gidin ve **Dizin Leyiciler'i**tıklatın.

Portalın sayfayı güncelleştirmesi birkaç dakika sürebilir, ancak listede yeni oluşturulan dizin leyiciyi, dizine eklenen belge sayısıyla birlikte "devam eden" veya başarıyı gösteren durumla birlikte görmeniz gerekir.

   ![Dizin oluşturucu ilerleme durumu iletisi](media/search-get-started-portal/indexers-inprogress.png)

## <a name="view-the-index"></a>Dizini görüntüleme

Ana hizmet sayfası, Azure Bilişsel Arama hizmetinizde oluşturulan kaynaklara bağlantılar sağlar.  Oluşturduğunuz dizini görüntülemek için bağlantılar listesinden **Dizinler'i** tıklatın. 

Portal sayfasının yenilenmesini bekleyin. Birkaç dakika sonra, belge sayısı ve depolama boyutu ile dizin görmeniz gerekir.

   ![Hizmet panosundaki dizinler listesi](media/search-get-started-portal/indexes-list.png)

Bu listeden, az önce oluşturduğunuz *otellere* örnek endeksini tıklayabilir, dizin şeasını görüntüleyebilirsiniz. ve isteğe bağlı olarak yeni alanlar ekleyin. 

**Alanlar** sekmesi dizin şeasını gösterir. Yeni bir alan girmek için listenin en altına kaydırın. Çoğu durumda, varolan alanları değiştiremezsiniz. Varolan alanlar Azure Bilişsel Arama'da fiziksel bir gösterime sahiptir ve bu nedenle kodda bile değiştirilemez. Varolan bir alanı temelden değiştirmek için, orijinali bırakarak yeni bir dizin oluşturun.

   ![Örnek dizin tanımı](media/search-get-started-portal/sample-index-def.png)

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir noktada eklenebilir.

Dizin tasarımı sırasında düzenleme yapabileceğiniz ve yapamayacağınız alanları kavramak için birkaç dakikanızı ayırarak dizin tanımı seçeneklerini görüntüleyin. Gri renkli seçenekler, bir değerin değiştirme veya silme işlemleri için uygun olmadığını gösterir. 

## <a name="query-using-search-explorer"></a><a name="query-index"></a>Arama gezginini kullanarak sorgula

Artık yerleşik [**Arama gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgulayabileceğiniz bir arama dizinine sahipsiniz. Bu sayfada rastgele sorgu dizelerini test etmek için kullanabileceğiniz bir arama kutusu bulunur.

**Arama gezgini** yalnızca [REST API isteklerini](https://docs.microsoft.com/rest/api/searchservice/search-documents)işlemek için donatılmıştır, ancak hem [basit sorgu sözdizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) hem de tam [Lucene sorgu aracı,](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)artı [Arama Belgesi REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) işlemlerinde bulunan tüm arama parametreleri için sözdizimini kabul eder.

> [!TIP]
> Aşağıdaki adımlar Azure Bilişsel Arama Genel Bakış [videosuna](https://channel9.msdn.com/Events/Connect/2016/138)6m08s'de gösterilmiştir.
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

   ![Search gezgini komutu](media/search-get-started-portal/search-explorer-cmd.png)

2. **Index** açılır düşüşünden *otellerin örnek endeksini*seçin. Hangi REST **API'lerinin** kullanılabilip kullanılabilip bulunmadığını görmek için API Sürüm açılır dosyasını tıklatın. Aşağıdaki sorgular için genel olarak kullanılabilir sürümü kullanın (2019-05-06).

   ![Dizin ve API komutları](media/search-get-started-portal/search-explorer-changeindex.png)

3. Arama çubuğuna, aşağıdaki sorgu dizelerine yapıştırın ve **Ara'yı**tıklatın.

   ![Sorgu dizesi ve arama düğmesi](media/search-get-started-portal/search-explorer-query-string-example.png)

## <a name="example-queries"></a>Örnek sorgular

Bing veya Google aramasında veya tam olarak belirtilen sorgu ifadelerinde yapabileceklerine benzer terimler ve ifadeler girebilirsiniz. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür.

### <a name="simple-query-with-top-n-results"></a>İlk N sonucu içeren basit sorgu

#### <a name="example-string-query-searchspa"></a>Örnek (string sorgusu):`search=spa`

* **Arama** parametresi, tam metin arama için bir anahtar kelime araması yapmak için kullanılır, bu durumda, belgedeki herhangi bir aranabilir alanda *spa* içerenlerin otel verilerini döndürmektedir.

* **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. Bu kasıtlı olduğunu; belgenin tamamının görünürlüğü, özellikle test sırasında geliştirme amacıyla önemlidir. Daha iyi bir kullanıcı deneyimi sunmak için [arama sonuçlarını işleyerek](search-pagination-page-layout.md) önemli öğeleri öne çıkaran bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. Portaldaki dizin özniteliklerini görüntülemek **için, Dizinler** *listesindeki oteller örneğini* tıklatın.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Örnek (parametreli sorgu):`search=spa&$count=true&$top=10`

* Sembol, **&** herhangi bir sırada belirtilebilen arama parametrelerini bulmak için kullanılır.

* **$count=true** parametresi döndürülen tüm belgelerin toplam sayısını döndürür. Bu değer arama sonuçlarının en üstüne yakın bir konumda görünür. **$count=true** tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtrenizin çalıştığını gösterir.

* **$top=10** toplamın en yüksek sırada yer alan 10 belgesini döndürür. Varsayılan olarak, Azure Bilişsel Arama ilk 50 en iyi eşleşmeyi döndürür. **$top** ile bu miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-the-query"></a><a name="filter-query"></a> Sorguyu filtreleme

**$filter** parametresini eklediğinizde, arama isteklerine filtreler de eklenir. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Örnek (filtrelenmiş): `search=beach&$filter=Rating gt 4`

* **$filter** parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. Bu durumda, derecelendirmeler 4'ten büyüktür.

* Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-the-query"></a><a name="facet-query"></a> Sorguyu modelleme

Model filtreleri arama isteklerine dahil edilir. Sağladığınız model değeriyle eşleşen belgelerin toplam sayısını döndürmek için facet parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Örnek (kapsamı azaltılarak modellenmiş): `search=*&facet=Category&$top=2`

* **search=*** boş bir aramadır. Boş aramalar her şeyi arar. Boş sorgu göndermenin nedenlerinden biri, belge kümesinin tamamını filtrelemek veya görüntülemektir. Örneğin, bir yönlü navigasyon yapısının dizindeki tüm otellerden oluşmasını istersiniz.
* **facet**, bir kullanıcı arabirimi denetimine geçirebileceğiniz bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. Bu durumda, kategoriler uygun *Kategori*olarak adlandırılan bir alan ayar. Azure Bilişsel Arama'da toplama yoktur, ancak her kategorideki `facet`belgelerin sayısını veren aracılığıyla yaklaşık toplama yapabilirsiniz.

* **$top=2** iki belge getirir ve sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Örnek (sayısal değerlerle modelleme): `search=spa&facet=Rating`

* Bu sorgu, *spa*için bir metin arama, derecelendirme için fason . Alan dizinde alınabilen, filtrelenebilir ve facetable olarak işaretlendive içerdiği değerler (sayısal, 1 ile 5) listeleri gruplara ayırmak için uygun *olduğundan, Derecelendirme* terimi bir faset olarak belirtilebilir.

* Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir. Yalnızca getirilebilir alanlar sonuçlarda döndürülebilir.

* *Derecelendirme* alanı çift duyarlıklı kayan noktadır ve gruplandırma kesin değere göre olacaktır. Arayla gruplandırma hakkında daha fazla bilgi için (örneğin, "3 yıldızlı derecelendirmeler", "4 yıldız derecelendirmeleri", vb.), [Azure Bilişsel Arama'da yüzlü gezinmenin nasıl uygulanacağını](https://docs.microsoft.com/azure/search/search-faceted-navigation#filter-based-on-a-range)görün.


### <a name="highlight-search-results"></a><a name="highlight-query"></a> Arama sonuçlarını vurgulama

İsabet vurgulama, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme eklenmesini ifade eder. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Örnek (vurgulama): `search=beach&highlight=Description`

* Bu örnekte, biçimlendirilmiş sözcük *plajıaçıklama* alanında daha kolay olarak incelenir.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Örnek (dilbilimsel analiz): `search=beaches&highlight=Description`

* Tam metin arama, sözcük formlarında temel varyasyonları tanır. Bu durumda, arama sonuçları, "plajlar" üzerinde bir anahtar kelime arama yanıt olarak, aranabilir alanlarında bu kelime var oteller için "plaj" için vurgulanmış metin içerir. Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir. 

* Azure Bilişsel Arama, hem Lucene hem de Microsoft'tan 56 çözümleyiciyi destekler. Azure Bilişsel Arama tarafından kullanılan varsayılan standart Lucene çözümleyicisidir.

### <a name="try-fuzzy-search"></a><a name="fuzzy-search"></a> Belirsiz aramayı deneme

Varsayılan olarak, "Seattle" için *koltuk gibi* yanlış yazılmış sorgu terimleri, tipik aramada eşleşmeleri döndürmez. Aşağıdaki örnek sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Örnek (yanlış yazılmış terim, işlenmiyor): `search=seatle`

Yazım hatalarını işlemek için belirsiz aramayı kullanabilirsiniz. Belirsiz arama, tam Lucene sorgu söz dizimini kullandığınızda etkinleştirilir ve bunun için yapmanız gereken iki işlem vardır: sorguda **queryType=full** belirtin ve arama sorgusunun sonuna **~** ekleyin.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Örnek (yanlış yazılmış terim, işleniyor): `search=seatle~&queryType=full`

Bu örnek şimdi "Seattle" üzerindeki eşleşmeleri içeren belgeleri döndürür.

**queryType** belirtildiğinde varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcı daha hızlıdır ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam söz dizimi gereklidir.

Belirsiz arama ve joker karakter araması, arama sonucunu etkiler. Bu sorgu biçimlerinde dilbilimsel analiz gerçekleştirilmez. Bulanık ve joker karakter aramasını kullanmadan önce, [Azure Bilişsel Arama'da tam metin aramanın nasıl çalıştığını](search-lucene-query-architecture.md#stage-2-lexical-analysis) gözden geçirin ve sözlü analize ilişkin özel durumlar hakkındaki bölümü arayın.

Tam sorgu arayıcı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi için [Azure Bilişsel Arama'da Lucene sorgu sözdizimine](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)bakın.

### <a name="try-geospatial-search"></a><a name="geo-search"></a> Jeo-uzamsal aramayı deneme

Koordinat içeren bir alanda [edm.GeographyPoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Coğrafi arama, [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Örnek sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 5 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. **$count** ekleyerek mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Arama uygulamanız "yakınımda bul" özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. Bir şehri veya ülkeyi/bölgeyi ada göre aramak için kullanıcı gereksinimleriniz varsa, koordinatlara ek olarak şehir veya ülke/bölge adlarını içeren alanlar ekleyin.

## <a name="takeaways"></a>Paketler

Bu öğretici, Azure portalını kullanarak Azure Bilişsel Arama'ya hızlı bir giriş sağladı.

**Verileri içeri aktar** sihirbazını kullanarak arama dizini oluşturmayı öğrendiniz. [Dizin oluşturucuların](search-indexer-overview.md) yanı sıra [yayımlanmış dizinler için desteklenen değişiklikler](https://docs.microsoft.com/rest/api/searchservice/update-index) gibi dizin tasarımıyla ilgili temel iş akışı hakkında bilgi edindiniz.

Azure portalda **Arama gezginini** kullanarak filtreler, sonuç vurgulama, belirsiz arama ve coğrafi arama gibi önemli özelliklerin gösterildiği örneklerle temel sorgu söz dizimini öğrendiniz.

Ayrıca portalda dizinleri, dizinleyicileri ve veri kaynaklarını nasıl bulacağınızı da öğrendiniz. Gelecekte yeni verilerle karşılaştığınızda portalı kullanarak tanımlarını veya alan koleksiyonlarını hızlı bir şekilde denetleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bir tarayıcıda çalışan kullanıma hazır bir web uygulaması oluşturmak için bir portal sihirbazı kullanın. Bu sihirbazı az önce oluşturduğunuz küçük dizinde deneyebilir veya daha zengin bir arama deneyimi için yerleşik örnek veri kümelerinden birini kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Portalda arama uygulaması oluşturma](search-create-app-portal.md)