---
title: Azure Bilişsel Arama’ya giriş
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, Microsoft tarafından tamamen yönetilen bir bulut arama hizmetidir. Özellik açıklamalarını, geliştirme iş akışını, diğer Microsoft arama ürünleriyle karşılaştırmaları ve nasıl başlatılanın gerektiğini okuyun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 01/06/2020
ms.openlocfilehash: fee7c8eb73fe0bb7c9fd0bd9de9aa57bd8c40215
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77580660"
---
# <a name="what-is-azure-cognitive-search"></a>Azure Bilişsel Arama nedir?

Azure Bilişsel Arama[(eski adıyla "Azure Arama"),](whats-new.md#new-service-name)geliştiricilere web, mobil ve kurumsal uygulamalardaki özel, heterojen içerik üzerinde zengin bir arama deneyimi eklemek için API'ler ve araçlar sağlayan bir hizmet olarak arama bulutçözümüdür. Kodunuz veya bir araç, bir dizin oluşturmak ve yüklemek için veri alımını (dizin oluşturmayı) çağırır. İsteğe bağlı olarak, indeksleme sırasında AI süreçlerini uygulamak için bilişsel beceriler ekleyebilirsiniz. Bunu yapmak, arama ve diğer senaryolar için yararlı yeni bilgiler ve yapılar ekleyebilirsiniz.

Hizmetinizin diğer tarafında, uygulama kodunuz sorgu isteklerini verir ve yanıtları işler. Arama deneyimi, hizmetinizde oluşturduğunuz, sahip olduğunuz ve depoladığınız kalıcı bir dizini üzerinden sorgu yürütmesi ile Azure Bilişsel Arama'daki işlevselliği kullanarak istemcinizde tanımlanır.

![Azure Bilişsel Arama mimarisi](media/search-what-is-azure-search/azure-search-diagram.svg "Azure Bilişsel Arama mimarisi")

Bilgi alma sürecinin karmaşıklığını maskeleyen basit bir [REST API’si](/rest/api/searchservice/) veya [.NET SDK’sı](search-howto-dotnet-sdk.md) aracılığıyla bu işlev sunulur. Azure portalı, API’lere ek olarak dizinlerinizin prototipini oluşturma ve dizinlerinizi sorgulama araçlarıyla birlikte yönetim ve içerik yönetimi desteği sağlar. Hizmet bulutta çalıştığından, altyapı ve kullanılabilirlik Microsoft tarafından yönetilir.

## <a name="when-to-use-azure-cognitive-search"></a>Azure Bilişsel Arama ne zaman kullanılır?

Azure Bilişsel Arama aşağıdaki uygulama senaryoları için uygundur:

+ Heterojen içerik türlerinin özel, tek, aranabilir bir dizinde konsolidasyonu. Sorgular her zaman oluşturduğunuz ve belgelerle yüklediğiniz bir dizinin üzerindedir ve dizin her zaman Azure Bilişsel Arama hizmetinizde bulutta bulunur. Herhangi bir kaynaktan veya platformdan JSON belge akışları içeren bir dizini doldurabilirsiniz. Alternatif olarak, Azure kaynaklı içerik için, verileri bir dizin içine çekmek için bir *dizinleyici* kullanabilirsiniz. Dizin tanımı ve yönetimi/sahipliği, Azure Bilişsel Arama'yı kullanmanın önemli bir nedenidir.

+ Ham içerik, Azure Blob depolama veya Cosmos DB gibi bir Azure veri kaynağındaki Office içerik türleri gibi büyük farklanmamış metin, resim dosyaları veya uygulama dosyalarıdır. Yapı eklemek veya görüntü ve uygulama dosyalarından anlam çıkarmak için dizin oluşturma sırasında bilişsel becerileri uygulayabilirsiniz.

+ Aramayla ilgili özelliklerin kolay uygulanması. Azure Bilişsel Arama API'leri sorgu yapımını, yönlü gezinmeyi, filtreleri (coğrafi uzamsal arama dahil), eşanlamlı eşlemi, önceden yazma sorgularını ve alaka düzeyini basitleştirir. Yerleşik özellikleri kullanarak, ticari web arama motorlarına benzer bir arama deneyimi için son kullanıcı beklentilerini karşılayabilirsiniz.

+ Yapılandırılmamış metni dizine ekleme veya görüntü dosyalarından metin ve bilgi ayıklama. Azure Bilişsel Arama'nın [AI zenginleştirme](cognitive-search-concept-intro.md) özelliği, dizine AI işleme ekler. Bazı yaygın kullanım durumları arasında taranan belge üzerinden OCR, büyük belgeler üzerinde varlık tanıma ve anahtar ifade çıkarma, dil algılama ve metin çevirisi ve duygu analizi yer alır.

+ Azure Bilişsel Arama'nın özel ve dil çözümleyicilerini kullanarak dil gereksinimleri karşılandı. Azure Bilişsel Arama, İngilizce olmayan içeriğe sahipseniz, hem Lucene çözümleyicilerini hem de Microsoft'un doğal dil işlemcilerini destekler. Ayrıca, aksanfiltreleme gibi ham içeriğin özel olarak işlenmesini sağlamak için çözümleyicileri yapılandırabilirsiniz.

<a name="feature-drilldown"></a>

## <a name="feature-descriptions"></a>Özellik açıklamaları

| Çekirdek&nbsp;arama&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  | Özellikler |
|-------------------|----------|
|Serbest biçimli metin arama | [**Tam metin arama,**](search-lucene-query-architecture.md) çoğu arama tabanlı uygulama için birincil kullanım örneğidir. Desteklenen bir söz dizimi kullanılarak sorgular formüle edilebilir. <br/><br/>[**Basit sorgu söz dizimi**](query-simple-syntax.md), mantıksal işleçler, tümcecik arama işleçleri, sonek işleçleri, öncelik işleçleri sağlar.<br/><br/>[**Lucene sorgu söz dizimi**](query-lucene-syntax.md), belirsiz arama, yakınlık araması, terimle yükseltme ve düzenli ifadeler için uzantılarla birlikte basit söz diziminde tüm işlemleri içerir.|
| İlgi düzeyi | [**Basit puanlama,**](index-add-scoring-profiles.md) Azure Bilişsel Arama'nın önemli bir avantajıdır. Belgelerdeki değer işlevi olarak ilgi düzeyini modellemek için puanlama profilleri kullanılır. Örneğin, yeni ürünlerin veya indirimli ürünlerin arama sonuçlarında daha yukarıda görüntülenmesini isteyebilirsiniz. Ayrı olarak izleyip depoladığınız müşteri arama tercihlerine göre kişiselleştirilmiş puanlama için etiketleri kullanarak da puanlama profilleri derleyebilirsiniz. |
| Coğrafi arama | Azure Bilişsel Arama işlemleri, filtreler ve coğrafi konumlar görüntüler. Kullanıcıların, bir arama sonucunun fiziksel bir konuma göre yakınlığına göre verileri bulmasını sağlar. Daha fazla bilgi edinmek için [bu videoyu izleyin](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) veya [bu örneği gözden geçirin](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). |
| Filtreler ve fasonlar | [**Modellenmiş gezinti**](search-faceted-navigation.md), tek bir sorgu parametresi aracılığıyla etkinleştirilir. Azure Bilişsel Arama, kendi kendini yönlendirmiş filtreleme (örneğin, katalog öğelerini fiyat aralığına veya markaya göre filtrelemek için) için kategoriler listesinin arkasındaki kod olarak kullanabileceğiniz yönlü bir gezinme yapısını döndürür. <br/><br/> [**Filtreler**](query-odata-filter-orderby-syntax.md), uygulamanın kullanıcı arabiriminde çok yönlü gezintiye yer vermek, sorgu oluşumunu geliştirmek ve kullanıcı veya geliştirici tarafından belirtilen ölçütlere göre filtreleme yapmak için kullanılabilir. OData söz dizimini kullanarak filtreler oluşturun. |
| Kullanıcı deneyimi özellikleri | [**Bir**](search-autocomplete-tutorial.md) arama çubuğunda ön yazma sorguları için otomatik tamamlama etkinleştirilebilir. <br/><br/>[**Arama önerileri**](https://docs.microsoft.com/rest/api/searchservice/suggesters) de arama çubuğuna girilen kısmi metinler için kullanılabilir ancak sonuçlar sorgu terimi yerine dizininizdeki gerçek belgeler olur. <br/><br/>[**Eş anlamlılar**](search-synonyms.md), kullanıcının alternatif terim belirtmesine gerek kalmadan bir sorguyu kapsamını genişleten eşdeğer terimlerle ilişkilendirir. <br/><br/>[**İsabet vurgulama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents), arama sonuçlarında eşleşen bir anahtar sözcüğe metin biçimlendirmesi uygular. Hangi alanların vurgulanan kod parçacıklarını döndürdüğünü seçebilirsiniz.<br/><br/>Dizin şeması aracılığıyla birden fazla alan için [**Sıralama**](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) sunulur ve sonra tek bir arama parametresi ile sorgu zamanında açılıp kapatılır.<br/><br/> Azure Bilişsel Arama'nın arama sonuçlarınız üzerinde sunduğu ince ayarlı denetimle arama sonuçlarınızı [**daraltma**](search-pagination-page-layout.md) ve daraltma işlemleri basittir.  <br/><br/>|

| AI&nbsp;zenginleştirme&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;       | Özellikler |
|-------------------|----------|
|Dizin oluşturma sırasında AI işleme | Görüntü ve metin analizi için [**AI zenginleştirme**](cognitive-search-concept-intro.md) ham içerikten metin bilgilerini ayıklamak için bir dizinleme ardışık için uygulanabilir. [Yerleşik yeteneklere](cognitive-search-predefined-skills.md) örnek olarak optik karakter tanıma (taranmış JPEG'leri aranabilir duruma getirme), varlık tanıma (bir kuruluşu, adı veya konumu tanıma) ve anahtar ifade tanıma verilebilir. İşlem hattına ekleme yapmak için [özel yetenek kodu da yazabilirsiniz](cognitive-search-create-custom-skill-example.md). |
| Arama dışı senaryolarda analiz ve tüketim için zenginleştirilmiş içeriğidepolama | [**Bilgi deposu (önizleme)**](knowledge-store-concept-intro.md) AI tabanlı dizin oluşturmanın bir uzantısıdır. Azure depolama nın arka uç olduğu için, dizin oluşturma sırasında oluşturulan zenginleştirmeleri kaydedebilirsiniz. Bu yapılar, daha iyi beceri kümeleri tasarlamanıza veya biçimsiz veya belirsiz verilerden şekil ve yapı oluşturmanıza yardımcı olmak için kullanılabilir. Belirli iş yüklerini veya kullanıcıları hedefleyen bu yapıların projeksiyonlarını oluşturabilirsiniz. Ayrıca, çıkarılan verileri doğrudan analiz edebilir veya diğer uygulamalara yükleyebilirsiniz.<br/><br/> |
| Önbelleğe alınmış içerik | [**Artımlı zenginleştirme (önizleme),**](cognitive-search-incremental-indexing-conceptual.md) ardışık nedenolan bölümler için önbelleğe alınmış içeriği kullanarak, yalnızca ardışık yöndeki belirli düzenlemeyle değiştirilen belgelerle işlemeyi sınırlar. |

| Veri&nbsp;alma/dizine aktarma | Özellikler |
|----------------------------------|----------|
| Veri kaynakları | Azure Bilişsel Arama dizinleri, JSON veri yapısı olarak gönderilmesi koşuluyla, verileri herhangi bir kaynaktan kabul eder. <br/><br/> [**Dizin leyiciler**](search-indexer-overview.md) desteklenen Azure veri kaynakları için veri alımını otomatikleştirin ve JSON serileştirmeişlemini işler. Birincil veri depolarında aranabilir içerik ayıklamak için [Azure SQL Veritabanına](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [Azure Cosmos DB'ye](search-howto-index-cosmosdb.md)veya [Azure Blob depolamaalanına](search-howto-indexing-azure-blob-storage.md) bağlanın. Azure Blob dizin oluşturucuları, Microsoft Office, PDF ve HTML belgeleri de dahil, [başlıca dosya biçimlerinden metin ayıklamak](search-howto-indexing-azure-blob-storage.md) için *belge çözme* işlemini gerçekleştirebilir. |
| Hiyerarşik ve iç içe veri yapıları | [**Karmaşık türler**](search-howto-complex-data-types.md) ve koleksiyonlar, hemen hemen her tür JSON yapısını Azure Bilişsel Arama dizini olarak modellemenize olanak tanır. Bir-çok ve çok-çok için çok önemlilik koleksiyonları, karmaşık türleri ve karmaşık türleri koleksiyonları ile doğal olarak ifade edilebilir.|
| Dil çözümleme | Çözümleyiciler, dizin oluşturma ve arama işlemleri sırasında metin işleme için kullanılan bileşenlerdir. İki tür vardır. <br/><br/>[**Özel sözcük temelli çözümleyiciler**](index-add-custom-analyzers.md), fonetik eşleştirme ve düzenli ifadeler kullanılarak yapılan karmaşık arama sorguları için kullanılır. <br/><br/>Lucene veya Microsoft’un [**dil çözümleyicileri**](index-add-language-analyzers.md), zaman kipleri, cinsiyet belirteçleri, düzensiz çoğul adlar (İngilizce’deki 'mouse' ve 'mice' gibi), sözcüğü bileşenlerine ayırma, sözcüklere bölme (boşluk içermeyen diller için) vb. gibi dile özgü linguistik durumları akıllıca işlemek için kullanılır. <br/><br/>|


| Platform&nbsp;düzeyi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Özellikler |
|-------------------|----------|
| Prototip oluşturma ve inceleme araçları | Portalda, dizin oluşturucuları yapılandırmak için [**Veri içeri aktarma sihirbazını**](search-import-data-portal.md), dizini öne çıkarmak için dizin tasarımcısını ve sorguları test edip puanlama profillerini daraltmak için [**Arama gezgini**](search-explorer.md)’ni kullanabilirsiniz. Şemasını görüntülemek için herhangi bir dizini de açabilirsiniz. |
| İzleme ve tanılama | [**İzleme özelliklerinin,**](search-monitor-usage.md) portalda her zaman görülebilen ölçümlerin ötesine geçmesine olanak sağlar. Ek bir yapılandırma gerekmeden saniye başına sorgu sayısı, gecikme süresi ve azaltma ölçümleri toplanıp portal sayfalarında raporlanır.|
| Sunucu tarafı şifrelemesi | [**Microsoft tarafından yönetilen şifreleme-at-rest**](search-security-overview.md#encrypted-transmission-and-storage) dahili depolama katmanıyerleşik ve geri alınamaz. İsteğe bağlı olarak, varsayılan şifrelemeyi [**müşteri tarafından yönetilen şifreleme anahtarlarıyla**](search-security-manage-encryption-keys.md)tamamlayabilirsiniz. Azure Anahtar Kasası'nda oluşturduğunuz ve yönettiğiniz anahtarlar, Azure Bilişsel Arama'daki dizinleri ve eşanlamlı haritaları şifrelemek için kullanılır. |
| Altyapı | **Yüksek oranda kullanılabilir platform**, son derece güvenilir arama hizmeti deneyimi sağlar. Azure Bilişsel Arama düzgün ölçeklendirildiğinde [%99,9 SLA sunar.](https://azure.microsoft.com/support/legal/sla/search/v1_0/)<br/><br/> **Tamamen yönetilen ve** uçtan uca bir çözüm olarak ölçeklenebilir Olan Azure Bilişsel Arama, kesinlikle altyapı yönetimi gerektirmez. Hizmetiniz daha fazla belge depolamayı, daha yüksek sorgu yüklerini veya her ikisini birden işlemek için iki boyutta ölçeklendirilerek ihtiyaçlarınıza göre uyarlanabilir.<br/><br/>|

## <a name="how-to-use-azure-cognitive-search"></a>Azure Bilişsel Arama nasıl kullanılır?
### <a name="step-1-provision-service"></a>1. Adım: Hizmeti sağlama
[Azure portalında](https://portal.azure.com/) veya [Azure Kaynak Yönetimi API'si](/rest/api/searchmanagement/)aracılığıyla bir Azure Bilişsel Arama hizmeti sağlayabilirsiniz. Diğer abonelerle paylaşılan ücretsiz hizmeti veya yalnızca hizmetiniz tarafından kullanılan kaynakları ayıran [ücretli katmanı](https://azure.microsoft.com/pricing/details/search/) seçebilirsiniz. Ücretli katmanlar için bir hizmeti iki boyutta ölçeklendirebilirsiniz: 

- Çoğaltmalar ekleyerek yoğun sorgu yüklerini işlemek için kapasitenizi büyütün.   
- Bölümler ekleyerek daha fazla belge için depolamayı büyütün. 

Belge depolamayı ve sorgu aktarım hızını ayrı olarak işleyerek üretim gereksinimlerine göre kaynak sağlamayı kalibre edebilirsiniz.

### <a name="step-2-create-index"></a>2. Adım: Dizin oluşturma
Aranabilir içerik yükleyebilmek için önce bir Azure Bilişsel Arama dizini tanımlamanız gerekir. Dizin, verilerinizi bulunduran ve arama sorgularını kabul edebilen bir veritabanı tablosuna benzer. Bir veritabanındaki alanlara benzer şekilde, aramak istediğiniz belgelerin yapısını yansıtmak için eşlenecek dizin şemasını tanımlarsınız.

Azure portalında veya [.NET SDK](search-howto-dotnet-sdk.md) ya da [REST API](/rest/api/searchservice/) kullanılarak programlama yoluyla bir şema oluşturulabilir.

### <a name="step-3-load-data"></a>3. Adım: Verileri yükleme
Bir dizin tanımladıktan sonra içeriği karşıya yüklemeye hazır olursunuz. Bir itme veya çekme modeli kullanabilirsiniz.

Çekme modeli, dış veri kaynaklarından verileri alır. Verilere bağlanma, verileri okuma ve seri hale getirme gibi veri alımı işlemlerini kolaylaştıran ve otomatikleştiren *dizin oluşturucular* aracılığıyla desteklenir. [Dizin oluşturucular](/rest/api/searchservice/Indexer-operations), bir Azure sanal makinesinde barındırılan Azure Cosmos DB, Azure SQL Veritabanı, Azure Blob Depolama ve SQL Server için kullanılabilir. İsteğe bağlı veya zamanlanan veri yenileme için dizin oluşturucuyu yapılandırabilirsiniz.

Güncelleştirilmiş belgeleri dizine göndermek için kullanılan SDK veya REST API’leri aracılığıyla itme modeli sağlanır. JSON biçimini kullanarak hemen hemen her veri kümesinden verileri itebilirsiniz. Verileri yüklemeye ilişkin kılavuz için bkz. [Belgeler ekleme, güncelleştirme veya silme](/rest/api/searchservice/addupdate-or-delete-documents) veya [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md).

### <a name="step-4-search"></a>4. Adım: Arama
Bir dizin doldurma sonra, [REST API](/rest/api/searchservice/Search-Documents) veya [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations)ile basit HTTP isteklerini kullanarak hizmet bitiş noktasına [arama sorguları düzenleyebilirsiniz.](search-query-overview.md)

Oluşturmak için [ilk arama uygulamanızı oluştur'a](tutorial-csharp-create-first-app.md) adım atın ve ardından kullanıcı girdisini toplayan ve sonuçları işleyen bir web sayfasını genişletin. [İnteraktif REST](search-get-started-postman.md) aramaları için Postacı'yı veya varolan bir dizini sorgulamak için Azure portalındaki yerleşik [Arama Gezgini'ni](search-explorer.md) de kullanabilirsiniz.

## <a name="how-it-compares"></a>Karşılaştırma

Müşteriler genellikle Azure Bilişsel Arama'nın aramayla ilgili diğer çözümlerle nasıl karşılaştırışta olduğunu sorar. Aşağıdaki tabloda temel farklılıklar özetlenmiştir.

| Karşılaştırılan | Temel farklılıklar |
|-------------|-----------------|
|Bing | [Bing Web Araması API'si](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/), Bing.com adresindeki dizinlerde gönderdiğiniz eşleşen terimleri arar. Dizinler, HTML, XML ve genel sitelerdeki diğer web içeriklerinden derlenir. Aynı temel üzerine oluşturulan [Bing Özel Arama](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/), tek tek web sitelerine kapsamı belirlenmiş şekilde web içeriği türleri için aynı gezgin teknolojisini sunar.<br/><br/>Azure Bilişsel Arama, tanımladığınız ve genellikle farklı kaynaklardan sahip olduğunuz veri ve belgelerle dolu bir dizini arar. Azure Bilişsel Arama, [dizinleyiciler](search-indexer-overview.md)aracılığıyla bazı veri kaynakları için tarayıcı özelliklerine sahiptir, ancak dizin şemasınıza uygun herhangi bir JSON belgesini tek bir bir leştirilmiş, birleştirilmiş aranabilir kaynağa itebilirsiniz. |
|Veritabanı araması | Birçok veritabanı platformu yerleşik bir arama deneyimi içerir. SQL Server'ın [tam metin araması](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) vardır. Cosmos DB ve benzeri teknolojilerin sorgulanabilir dizinleri vardır. Arama ile depolamayı birleştiren ürünler değerlendirilirken, hangisinin tercih edileceğini saptamak zor olabilir. Birçok çözüm hem depolama için DBMS'yi hem de özel arama özellikleri için Azure Bilişsel Arama'yı kullanır.<br/><br/>Azure Bilişsel Arama, DBMS araması ile karşılaştırıldığında içeriği heterojen kaynaklardan depolar ve [56 dilde](https://docs.microsoft.com/rest/api/searchservice/language-support)dil-bilinçli metin işleme (stemming, lemmatization, word forms) gibi özel metin işleme özellikleri sunar. Ayrıca yanlış yazılmış sözcükleri otomatik düzeltme, [eş anlamlılar](https://docs.microsoft.com/rest/api/searchservice/synonym-map-operations), [öneriler](https://docs.microsoft.com/rest/api/searchservice/suggestions), [puanlama denetimleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [modeller](https://docs.microsoft.com/azure/search/search-filters-facets) ve [özel belirteçlere ayırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) özelliklerini de destekler. Azure Bilişsel Arama'daki [tam metin arama motoru,](search-lucene-query-architecture.md) bilgi alma da bir endüstri standardı olan Apache Lucene üzerine kurulmuştur. Azure Bilişsel Arama verileri ters bir dizin biçiminde devam ederken, nadiren gerçek veri depolamanın yerini alabilir. Daha fazla bilgi için bu [forum gönderisine](https://stackoverflow.com/questions/40101159/can-azure-search-be-used-as-a-primary-database-for-some-data) bakın. <br/><br/>Kaynak kullanımı, bu kategorideki başka bir çekim noktasıdır. Dizin oluşturma ve bazı sorgu işlemleri çoğunlukla yoğun bilgi işlem içerir. Arama DBMS'den buluttaki ayrılmış bir çözüme aktarıldığında işlem yürütme için sistem kaynakları korunur. Üstelik, aramayı harici hale getirerek, sorgu hacmiyle eşleşecek şekilde kolayca ölçeği ayarlayabilirsiniz.|
|Ayrılmış arama çözümü | Tüm işlevleriyle birlikte ayrılmış arama kullanmaya karar verdiğinizi varsayarak, son kategorik karşılaştırma şirket içi çözümlerle bulut hizmetleri arasındadır. Birçok arama teknolojisi dizin oluşturma ve sorgu işlem hatları üzerinde kontrol, daha zengin sorgulama ve filtreleme söz dizimine erişim, derece ve ilgi düzeyi üzerinde kontrol ve kendinden yönlendirmeli ve akıllı arama özellikleri sunar. <br/><br/>Minimum ek yük ve bakım ile anahtar teslim bir çözüm istiyorsanız bulut hizmeti doğru seçimdir. <br/><br/>Bulut paradigması içinde birçok sağlayıcı, tam metin araması, coğrafi arama ve arama girişlerindeki belirli bir belirsizlik düzeyini işleme yeteneğiyle birlikte karşılaştırılabilir temel özellikler sunar. Genellikle bu [özel bir özellik olup](#feature-drilldown) en iyi uygunluğu belirleyen yönetimin, araçların ve API’lerin kolaylaştırılmasını sağlar. |

Bulut sağlayıcıları arasında Azure Bilişsel Arama, azure'daki içerik mağazaları ve veritabanları üzerindeki tam metin arama iş yükleri ve öncelikle hem bilgi alma hem de içerik gezintisi araması için gereken bilgilere dayanan uygulamalar için en güçlüdür. 

Temel güçlü yönleri şunlardır:

+ Dizin oluşturma katmanında Azure veri tümleştirmesi (gezginler)
+ Merkezi yönetim için Azure portalı
+ Azure ölçekleme, güvenilirlik ve birinci sınıf kullanılabilirlik
+ AI, görüntülerden gelen metin veya yapılandırılmamış içerikteki desenleri bulma dahil olmak üzere ham verilerin daha aranabilir hale getirmek için işlenmesi.
+ 56 dilde güçlü metin araması için çözümleyicilerle birlikte linguistik ve özel analiz
+ [Arama odaklı uygulamalarda ortak olan temel özellikler](#feature-drilldown): puanlama, modelleme, öneriler, eş anlamlılar, coğrafi arama ve daha fazlası.

> [!Note]
> Azure olmayan veri kaynakları tam olarak desteklenir, ancak dizin oluşturucular yerine daha kod kullanımı yoğun bir itme yöntemini kullanır. API'leri kullanarak, herhangi bir JSON belge koleksiyonunu Azure Bilişsel Arama dizinine taşıyabilirsiniz.

Müşterilerimiz arasında, Azure Bilişsel Arama'daki en geniş özellik yelpazesinden yararlanabilenler arasında çevrimiçi kataloglar, iş yeri programları ve belge bulma uygulamaları yer almaktadır.

## <a name="rest-api--net-sdk"></a>REST API | .NET SDK

Portalda birçok görev gerçekleştirilebilse de, Azure Bilişsel Arama, arama işlevselliğini varolan uygulamalara entegre etmek isteyen geliştiriciler için tasarlanmıştır. Aşağıdaki programlama arabirimleri kullanılabilir.

|Platform |Açıklama |
|-----|------------|
|[Geri kalanı](/rest/api/searchservice/) | Xamarin, Java ve JavaScript gibi, programlama platformu ve dili tarafından desteklenen HTTP komutları|
|[.NET SDK](search-howto-dotnet-sdk.md) | REST API’si için .NET sarmalayıcı, C# dilinde ve .NET Framework’ü hedefleyen diğer yönetilen kod dillerinde verimli kodlama sunar |

## <a name="free-trial"></a>Ücretsiz deneme sürümü
Azure aboneleri, [Ücretsiz katmanda bir hizmet sağlayabilir](search-create-service-portal.md).

Abone değilseniz, [ücretsiz olarak bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Ücretli Azure hizmetlerini denemek için krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez.

Alternatif olarak [MSDN abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

1. [Ücretsiz hizmet](search-create-service-portal.md) oluşturun. Tüm hızlı başlangıçlar ve öğreticiler ücretsiz hizmetle tamamlanabilir.

2. [Dizinleme ve sorgulama için yerleşik araçları kullanma öğreticisindeki](search-get-started-portal.md) adımları izleyin. Önemli kavramları öğrenin ve portalın sağladığı bilgileri inceleyin.

3. .NET veya REST API'sini kullanarak kod yazmaya yönelin:

   + [.NET SDK’sını kullanma](search-howto-dotnet-sdk.md), yönetilen kodda ana iş akışını gösterir.  
   + [REST API'si ile çalışmaya başlama](https://github.com/Azure-Samples/search-rest-api-getting-started), REST API’sinin kullanımıyla aynı adımları gösterir. Ayrıca Postacı veya Fiddler REST API'ler aramak için bu hızlı başlatış kullanabilirsiniz: [Azure Bilişsel Arama REST API'leri keşfedin.](search-get-started-postman.md)

## <a name="watch-this-video"></a>Bu videoyu izleyin

Arama motorları, mobil uygulamalarda, web’de ve kurumsal veri depolarında bilgi alımını sağlayan genel tetikleyicilerdir. Azure Bilişsel Arama, büyük ticari web sitelerindekine benzer bir arama deneyimi oluşturmak için araçlar sağlar.

Program yöneticisi Liam Cavanagh’ın bu 9 dakikalık videosunda, arama motorunu tümleştirmenin uygulamanıza nasıl fayda sağlayabileceğini öğrenebilirsiniz. Kısa demolar Azure Bilişsel Arama'daki temel özellikleri ve tipik bir iş akışının nasıl göründüğünü kapsar. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0.-3. dakikada, temel özellikler ve kullanım durumları ele alınmaktadır.
+ 3.-4. dakikada, hizmet sağlama ele alınmaktadır. 
+ 4.-6. dakikada, yerleşik emlak veri kümesini kullanarak bir dizin oluşturmak için kullanılan Veri İçeri Aktarma sihirbazı ele alınmaktadır.
+ 6.-9. dakikada, Arama gezgini ve çeşitli sorgular ele alınmaktadır.
