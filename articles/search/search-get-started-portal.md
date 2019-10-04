---
title: 'Hızlı başlangıç: Azure portal Azure Search kullanarak arama dizini oluşturma'
description: Azure Search ilk dizininizi oluşturmak, yüklemek ve sorgulamak için Azure portal veri alma Sihirbazı 'nı kullanın.
author: lobrien
manager: nitinme
tags: azure-portal
services: search
ms.service: search
ms.topic: quickstart
ms.date: 09/10/2019
ms.author: laobri
ms.openlocfilehash: a4a25b8504d873b624e1f6822807c9c08ebd2e4f
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936975"
---
# <a name="quickstart-create-an-azure-search-index-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure Search dizin oluşturma
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-get-started-powershell.md)
> * [Postman](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [C#](search-get-started-dotnet.md)

Azure Search kavramlarını hızlı bir şekilde artırma için Azure portal yerleşik araçları deneyin. Sihirbazlar ve düzenleyiciler .NET ve REST API 'Leri ile tam eşlik sunmaz, ancak bir kod ücretsiz giriş ile hızlı bir şekilde çalışmaya başlayıp dakikalar içinde bir dizine yönelik ilginç sorgular yazabilirsiniz.

> [!div class="checklist"]
> * Azure 'da barındırılan ücretsiz bir genel örnek veri kümesiyle başlayın
> * Veri yüklemek ve bir dizin oluşturmak için Azure Search **veri alma** Sihirbazı 'nı çalıştırın
> * Portalda dizin oluşturma ilerlemesini izleme
> * Var olan bir dizini ve değiştirme seçeneklerini görüntüleme
> * **Arama Gezgini** ile tam metin arama, filtreler, modeller, belirsiz arama ve coğrafi arama 'yı keşfet

Araçlar çok sınırlandırıyorsa, [.net 'teki Azure Search programlama](search-howto-dotnet-sdk.md) veya [REST API çağrısı yapmak için Postman kullanma konusunda](search-get-started-postman.md)kod tabanlı bir giriş yapmayı düşünebilirsiniz. Ayrıca, bu öğreticideki adımların 6 dakikalık bir gösterimini izleyerek bu [Azure Search genel bakış videosunu](https://channel9.msdn.com/Events/Connect/2016/138)üç dakika içinde izleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Prerequisites

Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

### <a name="check-for-space"></a>Boşluk denetle

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç Dizin, üç veri kaynağı ve üç Dizin Oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğretici her bir nesneden birini oluşturur.

Hizmet panosundaki bölümler, kaç Dizin, Dizin Oluşturucu ve veri kaynağı olduğunu gösterir. 

![Dizin, Dizin oluşturucular ve veri kaynakları listesi](media/search-get-started-portal/tiles-indexers-datasources.png)

## <a name="create-index"></a>Dizin oluşturma ve veri yükleme

Arama sorguları, aranabilir verileri, meta verileri ve belirli arama davranışlarını en iyileştiren ek yapıları içeren bir [*Dizin*](search-what-is-an-index.md) üzerinde yinelenir.

Bu öğreticide, [ **veri alma** Sihirbazı](search-import-data-portal.md)aracılığıyla bir [*Dizin Oluşturucu*](search-indexer-overview.md) kullanılarak gezinilebilen yerleşik bir örnek veri kümesi kullanırız. Dizin Oluşturucu, desteklenen Azure veri kaynaklarından meta verileri ve içeriği okuyabilen kaynağa özgü bir gezgin. Normalde, Dizin oluşturucular programlı olarak kullanılır, ancak portalda **verileri Içeri aktarma** Sihirbazı aracılığıyla erişebilirsiniz. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>1\. adım-veri alma Sihirbazı 'nı başlatın ve bir veri kaynağı oluşturun

1. Azure Search hizmeti panosunda, bir arama dizini oluşturmak ve doldurmak için komut çubuğunda **verileri Içeri aktar** ' a tıklayın.

   ![Verileri içeri aktar komutu](media/search-get-started-portal/import-data-cmd.png)

2. Sihirbazda, **verilerinize bağlan** > **örnek** > **oteller-örnek**' e tıklayın. Bu veri kaynağı yerleşik olarak bulunur. Kendi veri kaynağınızı oluşturuyorsanız ad, tür ve bağlantı bilgilerini belirtmeniz gerekir. Oluşturulduktan sonra, diğer içeri aktarma işlemlerinde yeniden kullanılabilen bir "var olan veri kaynağı" haline gelir.

   ![Örnek veri kümesi seçin](media/search-get-started-portal/import-datasource-sample.png)

3. Sonraki sayfaya devam edin.

   ![Bilişsel arama için sonraki sayfa düğmesi](media/search-get-started-portal/next-button-add-cog-search.png)

### <a name="step-2---skip-cognitive-skills"></a>2\. adım-bilişsel becerileri atlama

Sihirbaz bilişsel hizmetler AI algoritmalarını dizine eklemek için bilişsel [yetenekler işlem hattının](cognitive-search-concept-intro.md) oluşturulmasını destekler. 

Bu adımı şimdilik atlayacağız ve **hedef dizini özelleştirmek**için doğrudan üzerine ilerliyoruz.

   ![Bilişsel Beceri adımını atla](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> Bir [hızlı başlangıç](cognitive-search-quickstart-blob.md) veya [öğreticide](cognitive-search-tutorial-blob.md)bir AI dizin oluşturma örneği aracılığıyla ilerlemesini sağlayabilirsiniz.

### <a name="step-3---configure-index"></a>3\. adım-dizini yapılandırma

Genellikle, dizin oluşturma, veri yüklenmeden önce tamamlanan kod tabanlı bir uygulamadır. Ancak, Bu öğreticinin gösterdiği gibi, sihirbaz gezinebileceği herhangi bir veri kaynağı için temel bir dizin oluşturabilir. En düşük düzeyde, bir dizin ad ve alanlar koleksiyonu gerektirir; Her bir belgeyi benzersiz bir şekilde tanımlamak için alanlardan biri belge anahtarı olarak işaretlenmelidir. Ayrıca, otomatik tamamlama veya önerilen sorguları isterseniz dil Çözümleyicileri veya öneri araçları belirtebilirsiniz.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanıldığını denetleyen *Dizin öznitelikleridir* .

* **Alınabilir** , arama sonuçları listesinde gösterildiği anlamına gelir. Bu onay kutusunu temizleyerek, örneğin yalnızca filtre ifadelerinde kullanılan alanlar için, bu onay kutusunu temizleyerek, tek tek alanları, arama sonuçları için sınırlı olarak işaretleyebilirsiniz.
* **Anahtar** , benzersiz belge tanımlayıcısıdır. Her zaman bir dizedir ve gereklidir.
* **Filtrelenebilir**, **sıralanabilir**ve çok **yönlü tablo** , alanların bir filtre, sıralama veya çok yönlü gezinti yapısında kullanılıp kullanılmadığını belirtir.
* **Aranabilir** , bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizeler aranabilir. Sayısal alanlar ve Boole alanları genellikle aranamaz olarak işaretlenir.

Depolama gereksinimleri, seçiminizin sonucu olarak farklılık gösterir. Örneğin, birden çok alanda **alınabilir** özniteliğini ayarlarsanız, depolama gereksinimleri güncel değildir.

Varsayılan olarak, sihirbaz, anahtar alanının temeli olarak benzersiz tanımlayıcılar için veri kaynağını tarar. *Dizeler* **alınabilir** ve **aranabilir**olarak atanır. *Tamsayılar* **alınabilir**, **filtrelenebilir**, **sıralanabilir**ve çok **yönlü tablo**olarak atanır.

1. Varsayılanları kabul edin. 

   Sihirbazı ikinci kez mevcut bir oteller veri kaynağını kullanarak yeniden çalıştırırsanız, dizin varsayılan özniteliklerle yapılandırılmaz. Gelecekteki içeri aktarmalardan öznitelikleri el ile seçmeniz gerekir. 

   ![Oluşturulan oteller dizini](media/search-get-started-portal/hotelsindex.png)

2. Sonraki sayfaya devam edin.

   ![Sonraki sayfa Dizin Oluşturucu oluştur](media/search-get-started-portal/next-button-create-indexer.png)

### <a name="step-4---configure-indexer"></a>4\. adım-Dizin oluşturucuyu yapılandırma

**Veri Içeri aktarma** sihirbazında, **Dizin Oluşturucu** > **adı**' na tıklayın ve Dizin Oluşturucu için bir ad yazın.

Bu nesne yürütülebilir bir işlem tanımlıyor. Yinelenen bir zamanlamaya göre koyabilirsiniz, ancak şimdilik Dizin oluşturucuyu hemen çalıştırmak için varsayılan seçeneği kullanın.

Dizin oluşturucuyu oluşturmak ve aynı anda çalıştırmak için **Gönder** ' e tıklayın.

  ![oteller Dizin Oluşturucusu](media/search-get-started-portal/hotels-indexer.png)

## <a name="monitor-progress"></a>İlerlemeyi izleme

Sihirbaz sizi ilerlemeyi izleyebileceğiniz Dizin oluşturucular listesine götürebilmelidir. Kendi kendine gezinme için genel bakış sayfasına gidin ve **Dizin oluşturucular**' ye tıklayın.

Portalın sayfayı güncelleştirmesi birkaç dakika sürebilir, ancak "devam ediyor" veya Success, "sürüyor" veya Success (Dizin oluşturulmuş belge sayısıyla birlikte) durumuyla yeni oluşturulan dizin oluşturucuyu görmeniz gerekir.

   ![Dizin Oluşturucu ilerleme iletisi](media/search-get-started-portal/indexers-inprogress.png)

## <a name="view-the-index"></a>Dizini görüntüle

Ana hizmet sayfası, Azure Search hizmetinizde oluşturulan kaynaklara bağlantılar sağlar.  Yeni oluşturduğunuz dizini görüntülemek için bağlantılar listesinden **dizinler** ' e tıklayın. 

   ![Hizmet panosundaki dizinler listesi](media/search-get-started-portal/indexes-list.png)

Bu listeden, yeni oluşturduğunuz *oteller-örnek* dizinine tıklayabilirsiniz, Dizin şemasını görüntüleyebilirsiniz. ve isteğe bağlı olarak yeni alanlar ekleyin. 

**Alanlar** sekmesi Dizin şemasını gösterir. Yeni bir alan girmek için listenin en altına gidin. Çoğu durumda, mevcut alanları değiştiremezsiniz. Mevcut alanlar Azure Search bir fiziksel gösterimine sahiptir ve bu nedenle kodda bile değil, değiştirilemeyen değildir. Var olan bir alanı temel olarak değiştirmek için yeni bir dizin oluşturun ve özgün olanı bırakarak.

   ![örnek dizin tanımı](media/search-get-started-portal/sample-index-def.png)

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir zamanda eklenebilir.

Dizin tasarımı sırasında neleri düzenleyebileceğinizi ve düzenleyebileceğinizi açıkça anlamak için Dizin tanımı seçeneklerini görüntülemek üzere bir dakikanızı alın. Gri olmayan seçenekler, bir değerin değiştirilemeyen veya silinemediği bir göstergedir. 

## <a name="query-index"></a>Arama Gezgini 'ni kullanarak sorgulama

Bundan sonra, yerleşik [**Arama Gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgu için hazır olan bir arama dizinine sahip olmanız gerekir. Rastgele Sorgu dizelerini test edebilmeniz için bir arama kutusu sağlar.

**Arama Gezgini** yalnızca [REST API isteklerini](https://docs.microsoft.com/rest/api/searchservice/search-documents)işleyecek şekilde donatılmış, ancak hem [basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) hem de [tam Lucene sorgu ayrıştırıcısı](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)için sözdizimi kabul eder, ayrıca [arama belgesinde](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) bulunan tüm arama parametreleri de kullanılabilir REST API operasyonları.

> [!TIP]
> Aşağıdaki adımlar, 6m08s ' de [Azure Search genel bakış videosu](https://channel9.msdn.com/Events/Connect/2016/138)' ne gösterilmiştir.
>

1. Komut çubuğunda **Arama Gezgini** ' ne tıklayın.

   ![Arama Gezgini komutu](media/search-get-started-portal/search-explorer-cmd.png)

2. **Dizin** açılan listesinden *oteller-örnek*' i seçin. Hangi REST API 'Lerinin kullanılabilir olduğunu görmek için **API sürümü** açılan listesine tıklayın. Aşağıdaki sorgular için genel kullanıma sunulan sürümü (2019-05-06) kullanın.

   ![Dizin ve API komutları](media/search-get-started-portal/search-explorer-changeindex.png)

3. Arama çubuğunda, aşağıdaki sorgu dizelerini yapıştırın ve **Ara**' ya tıklayın.

   ![Sorgu dizesi ve arama düğmesi](media/search-get-started-portal/search-explorer-query-string-example.png)

## <a name="example-queries"></a>Örnek sorgular

Bing veya Google aramasında yapabileceklerinize benzer şekilde hüküm ve ifadeler girebilir veya tam olarak belirtilen sorgu ifadeleriyle karşılaşabilirsiniz. Sonuçlar ayrıntılı JSON belgeleri olarak döndürülür.

### <a name="simple-query-with-top-n-results"></a>İlk N sonuçdan basit sorgu

#### <a name="example-string-query-searchspa"></a>Örnek (dize sorgusu): `search=spa`

* **Arama** parametresi, tam metin araması için bir anahtar sözcük arama girişi yapmak için kullanılır, bu durumda, belgedeki aranabilir herhangi bir alanda *Spa* 'yı içeren kişiler için otel verileri döndürür.

* **Arama Gezgini** , belgeler yoğun bir yapıya sahip olduğunda ayrıntılı ve okunması zor olan JSON 'daki sonuçları döndürür. Bu bilerek yapılır; belgenin tamamına ilişkin görünürlük, özellikle test sırasında geliştirme amaçlarıyla önemlidir. Daha iyi bir kullanıcı deneyimi için, önemli öğeleri getirmek üzere [arama sonuçlarını işleyen](search-pagination-page-layout.md) bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. Portalda Dizin özniteliklerini görüntülemek için, **dizinler** listesinde *oteller-Sample* ' a tıklayın.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Örnek (parametreli sorgu): `search=spa&$count=true&$top=10`

* **@No__t-1** simgesi, herhangi bir sırada belirtilebilen arama parametrelerini eklemek için kullanılır.

* **$Count = true** parametresi, döndürülen tüm belgelerin toplam sayısını döndürür. Bu değer, arama sonuçlarının en üstünde görünür. **$Count = true**tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtreniz 'nin çalıştığını gösterir.

* **$Top = 10** , toplamda en yüksek dereceli 10 belgeyi döndürür. Varsayılan olarak, Azure Search en iyi 50 eşleşme döndürür. **$Top**aracılığıyla miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-query"></a>Sorguyu filtrele

Filtre, **$Filter** parametresini eklediğinizde arama isteklerine dahil edilir. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Örnek (filtrelenmiş): `search=beach&$filter=Rating gt 4`

* **$Filter** parametresi, belirttiğiniz ölçütlerle eşleşen sonuçları döndürür. Bu durumda, 4 ' ten büyük derecelendirmeler.

* Filtre sözdizimi bir OData yapıdır. Daha fazla bilgi için bkz. [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-query"></a>Sorgu modeli

Model filtreleri arama isteklerine dahildir. Sağladığınız bir model değeriyle eşleşen belgelerin toplanmış sayısını döndürmek için model parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Örnek (kapsam azaltmasıyla çok yönlü): `search=*&facet=Category&$top=2`

* **Search =** * boş bir aramadır. Boş aramalar her şey üzerinde arama yapar. Boş bir sorgu göndermenin bir nedeni, tüm belge kümesi üzerinde filtrelemeye veya bir şekilde bir model göndermeye yöneliktir. Örneğin, dizin içindeki tüm otellerden oluşan bir gezinti yapısının olmasını istersiniz.
* **model** , bir UI denetimine geçirebilmeniz için bir gezinti yapısı döndürür. Kategoriler ve bir sayı döndürür. Bu durumda, kategoriler *Kategori*olarak adlandırılan bir alanı temel alır. Azure Search bir toplama yoktur, ancak her kategoride belge sayısını sağlayan `facet` aracılığıyla toplamayı yaklaşık olarak yapabilirsiniz.

* **$Top = 2** iki belgeyi geri getirir, böylece sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Örnek (sayısal değerler esası): `search=spa&facet=Rating`

* Bu sorgu, *Spa*'nın metin aramasında derecelendirme için bir model. Alan, dizinde alınabilir, *filtrelenebilir ve* çok yönlü tablo olarak işaretlendiğinden ve içerdiği değerler (sayısal, 1 ' den 5 ' e kadar), listelerin gruplar halinde kategorilere ayrılması için uygundur.

* Yalnızca filtrelenebilir alanlar çok yönlü olabilir. Sonuçlarda yalnızca alınabilir alanlar döndürülebilir.

* *Derecelendirme* alanı çift duyarlıklı kayan nokta ve gruplama kesin değere göre yapılır. Aralığa göre gruplandırma hakkında daha fazla bilgi için (örneğin, "3 yıldız derecelendirmesi," "4 yıldız derecelendirmesi" vb.), bkz. [Azure Search çok yönlü gezintiyi uygulama](https://docs.microsoft.com/azure/search/search-faceted-navigation#filter-based-on-a-range).


### <a name="highlight-query"></a>Arama sonuçlarını Vurgula

İsabet vurgulama, belirli bir alanda bulunan eşleşmelerin, anahtar sözcükle eşleşen metin üzerinde biçimlendirme olduğunu ifade eder. Arama teriminizi bir açıklamada daha fazla yere alıyorsa, daha kolay olması için isabet vurgulama özelliği ekleyebilirsiniz.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Örnek (vurgulayıcı): `search=beach&highlight=Description`

* Bu örnekte, biçimlendirilen sözcük *plaj* , açıklama alanında daha kolay olacak.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Örnek (dil analizi): `search=beaches&highlight=Description`

* Tam metin araması, Word formlarında temel çeşitlemeleri tanır. Bu durumda, arama sonuçları "plaj" için vurgulanmış metni, bu sözcüğü "Güzeller" üzerinde bir anahtar sözcük aramasına yanıt olarak, bu sözcüğe sahip olan oteller için "plaj" için vurgulanmış metni içerir. Aynı sözcüğe ait farklı formlar, dil analizi nedeniyle sonuçlarda görünebilir. 

* Azure Search, Lucene ve Microsoft 'tan 56 Çözümleyicileri destekler. Azure Search tarafından kullanılan varsayılan değer standart Lucene çözümleyicisidir.

### <a name="fuzzy-search"></a>Belirsiz aramayı deneyin

Varsayılan olarak, "Seattle" için *Seatle* gibi yanlış yazılmış sorgu terimleri, tipik aramada eşleşmeleri döndürmeyebilir. Aşağıdaki örnek hiçbir sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Örnek (yanlış yazılmış terim, işlenmemiş): `search=seatle`

Yanlış yazımları işlemek için, benzer aramayı kullanabilirsiniz. İki şey yaptığınızda oluşan tam Lucene sorgu söz dizimini kullandığınızda belirsiz arama etkinleştirilir: sorgu üzerinde **queryType = Full** ayarla ve arama dizesine **~** ekler.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Örnek (yanlış yazılmış terim, işlenmiş): `search=seatle~&queryType=full`

Bu örnek şimdi "Seattle" ile eşleşmeleri içeren belgeleri geri döndürüyor.

**QueryType** belirtilmediğinde, varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcısı daha hızlıdır, ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerine ihtiyacınız varsa, tam söz dizimi gerekir.

Benzer arama ve joker karakter araması, arama çıktısındaki etkileri vardır. Dil analizi bu sorgu biçimlerinde gerçekleştirilmez. Benzer ve joker karakter aramasını kullanmadan önce, [tam metin aramasının Azure Search nasıl çalıştığını](search-lucene-query-architecture.md#stage-2-lexical-analysis) gözden geçirin ve sözcük temelli analize yönelik özel durumlar hakkındaki bölümü bulun.

Tam sorgu ayrıştırıcısı tarafından etkinleştirilen sorgu senaryoları hakkında daha fazla bilgi için, bkz. [Azure Search Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

### <a name="geo-search"></a>Jeo-uzamsal aramayı deneyin

Jeo-uzamsal arama EDM aracılığıyla desteklenir [. Koordinatları içeren bir alanda Geographyıpoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) . Geosearch, [filtre OData sözdiziminde](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search)belirtilen bir filtre türüdür.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Örnek sorgu, sonuçların belirli bir noktadan 5 kilometreden (Enlem ve boylam koordinatları olarak belirtilir) daha az olduğu konum verileri için tüm sonuçları filtreler. **$Count**ekleyerek, mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Jeo-uzamsal arama, arama uygulamanız "yakın beni bul" özelliğine sahipse veya harita gezintisi kullanıyorsa yararlıdır. Ancak tam metin araması değildir. Ada göre şehir veya ülke/bölge aramak için Kullanıcı gereksinimleriniz varsa, koordinatlara ek olarak şehir veya ülke/bölge adlarını içeren alanları ekleyin.

## <a name="takeaways"></a>Dersler

Bu öğreticide, Azure portal kullanılarak Azure Search hızlı bir giriş sağlanmış.

**Veri alma** Sihirbazı 'nı kullanarak bir arama dizini oluşturmayı öğrendiniz. [Bir yayımlanan dizinde desteklenen değişiklikler](https://docs.microsoft.com/rest/api/searchservice/update-index)dahil olmak üzere dizin oluşturma için temel iş akışını [ve dizin oluşturma hakkında bilgi](search-indexer-overview.md)edindiniz.

Azure portal **Arama Gezgini** 'ni kullanarak, filtreler, isabet vurgulama, benzer arama ve coğrafi arama gibi önemli özellikleri gösteren uygulamalı örneklerle basit sorgu söz dizimi öğrenmiş olursunuz.

Ayrıca, portalda Dizin, Dizin oluşturucular ve veri kaynaklarını bulmayı de öğrendiniz. Gelecekte herhangi bir yeni veri kaynağı verildiğinde, en az çabayla tanımlarını veya alan koleksiyonlarını hızlıca denetlemek için portalı kullanabilirsiniz.

## <a name="clean-up"></a>Temizle

Kendi aboneliğinizde çalışırken, sizin oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için bir projenin sonunda iyi bir fikir olur. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Programlama araçlarını kullanarak Azure Search daha fazlasını inceleyebilirsiniz:

* [.NET SDK kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [REST API 'Leri kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* [Postman veya Fiddler ile Azure Search REST API 'Lerini kullanarak bir dizin oluşturma](search-get-started-postman.md)
