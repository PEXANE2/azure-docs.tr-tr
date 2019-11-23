---
title: Create a search index in the Azure portal
titleSuffix: Azure Cognitive Search
description: In this portal quickstart, learn how to use the Import Data wizard to create, load, and query your first search index in Azure Cognitive Search.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 11/04/2019
ms.openlocfilehash: be2425d97573e7990ea7f0dfd4c2d999e85fe922
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74407006"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-the-azure-portal"></a>Quickstart: Create an Azure Cognitive Search index in the Azure portal
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [C#](search-get-started-dotnet.md)
> * [Java](search-get-started-java.md)
> * [Node.js](search-get-started-nodejs.md)
> * [PowerShell](search-get-started-powershell.md)
> * [Postman](search-get-started-postman.md)
> * [Python](search-get-started-python.md)

Use the portal's **Import data** wizard and **Search explorer** tools to quickly ramp up on concepts, and write interesting queries against an index within minutes.

If the tools are too limiting, you can consider a [code-based introduction to programming Azure Cognitive Search in .NET](search-howto-dotnet-sdk.md) or use [Postman for making REST API calls](search-get-started-postman.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

[Create an Azure Cognitive Search service](search-create-service-portal.md) or [find an existing service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) under your current subscription. You can use a free service for this quickstart. 

### <a name="check-for-space"></a>Alan denetleme

Birçok müşteri ücretsiz hizmetle başlar. Bu sürüm üç dizin, üç veri kaynağı ve üç dizin oluşturucu ile sınırlıdır. Başlamadan önce ek öğeler için yeriniz olduğundan emin olun. Bu öğreticide her nesneden birer tane oluşturulur.

Sections on the service dashboard show how many indexes, indexers, and data sources you already have. 

![Lists of indexes, indexers, and datasources](media/search-get-started-portal/tiles-indexers-datasources.png)

## <a name="create-index"></a> Dizin oluşturma ve verileri yükleme

Arama sorguları aranabilir veriler, meta veriler ve arama davranışlarını iyileştiren ek yapılar içeren bir [*dizin*](search-what-is-an-index.md) kullanır.

For this tutorial, we use a built-in sample dataset that can be crawled using an [*indexer*](search-indexer-overview.md) via the [**Import data** wizard](search-import-data-portal.md). Dizin oluşturucu, kaynağa özgü bir gezgindir ve desteklenen Azure veri kaynaklarındaki meta verileri ve içeriği okuyabilir. Normally, indexers are used programmatically, but in the portal, you can access them through the **Import data** wizard. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>Step 1 - Start the Import data wizard and create a data source

1. On the Azure Cognitive Search service dashboard, click **Import data** on the command bar to create and populate a search index.

   ![Verileri içeri aktar komutu](media/search-get-started-portal/import-data-cmd.png)

2. In the wizard, click **Connect to your data** > **Samples** > **hotels-sample**. This data source is built-in. If you were creating your own data source, you would need to specify a name, type, and connection information. Oluşturulan kaynak, diğer içeri aktarma işlemlerinde yeniden kullanılabilecek bir “mevcut veri kaynağı” olur.

   ![Örnek veri kümesi seçme](media/search-get-started-portal/import-datasource-sample.png)

3. Continue to the next page.

### <a name="step-2---skip-the-enrich-content-page"></a>Step 2 - Skip the "Enrich content" page

The wizard supports the creation of an [AI enrichment pipeline](cognitive-search-concept-intro.md) for incorporating the Cognitive Services AI algorithms into indexing. 

We'll skip this step for now, and move directly on to **Customize target index**.

   ![Bilişsel beceri adımını atlama](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> You can step through an AI-indexing example in a [quickstart](cognitive-search-quickstart-blob.md) or [tutorial](cognitive-search-tutorial-blob.md).

### <a name="step-3---configure-index"></a>Step 3 - Configure index

Typically, index creation is a code-based exercise, completed prior to loading data. However, as this tutorial indicates, the wizard can generate a basic index for any data source it can crawl. Dizin için en azından bir ad ve alan koleksiyonu gerekir ve her belgenin benzersiz olarak tanımlanabilmesi için bir alanın belge anahtarı olarak işaretlenmiş olması gerekir. Additionally, you can specify language analyzers or suggesters if you want autocomplete or suggested queries.

Alanların veri türleri ve öznitelikleri vardır. Üstteki onay kutuları, alanın nasıl kullanılacağını denetleyen *dizin öznitelikleridir*.

* **Alınabilir**, arama sonuçları listesinde çıktığı anlamına gelir. You can mark individual fields as off limits for search results by clearing this checkbox, for example for fields used only in filter expressions.
* **Key** is the unique document identifier. It's always a string, and it is required.
* **Filterable**, **Sortable**, and **Facetable** determine whether fields are used in a filter, sort, or faceted navigation structure.
* **Aranabilir**, bir alanın tam metin aramasına dahil olduğu anlamına gelir. Dizelerde arama yapılabilir. Sayısal alanlar ve Boolean alanları genellikle aranamaz olarak işaretlenir.

Storage requirements do not vary as a result of your selection. For example, if you set the **Retrievable** attribute on multiple fields, storage requirements do not go up.

Varsayılan olarak sihirbaz tarafından anahtar alanının temeli olarak benzersiz tanımlayıcıların bulunması için veri kaynağı taranır. *Strings* are attributed as **Retrievable** and **Searchable**. *Integers* are attributed as **Retrievable**, **Filterable**, **Sortable**, and **Facetable**.

1. Varsayılanları kabul edin. 

   If you rerun the wizard a second time using an existing hotels data source, the index won't be configured with default attributes. You'll have to manually select attributes on future imports. 

   ![Generated hotels index](media/search-get-started-portal/hotelsindex.png)

2. Continue to the next page.


### <a name="step-4---configure-indexer"></a>Step 4 - Configure indexer

**Verileri içeri aktarma** sihirbazından çıkmadan **Dizin Oluşturucu** > **Ad**’a tıklayın ve dizin oluşturucu için bir ad yazın.

Bu nesne, yürütülebilir bir işlemi tanımlar. You could put it on recurring schedule, but for now use the default option to run the indexer once, immediately.

Click **Submit** to create and simultaneously run the indexer.

  ![hotels indexer](media/search-get-started-portal/hotels-indexer.png)

## <a name="monitor-progress"></a>Monitor progress

The wizard should take you to the Indexers list where you can monitor progress. For self-navigation, go to the Overview page and click **Indexers**.

It can take a few minutes for the portal to update the page, but you should see the newly created indexer in the list, with status indicating "in progress" or success, along with the number of documents indexed.

   ![Dizin oluşturucu ilerleme durumu iletisi](media/search-get-started-portal/indexers-inprogress.png)

## <a name="view-the-index"></a>Dizini görüntüleme

The main service page provides links to the resources created in your Azure Cognitive Search service.  To view the index you just created, click **Indexes** from the list of links. 

Wait for the portal page to refresh. After a few minutes, you should see the index with a document count and storage size.

   ![Indexes list on the service dashboard](media/search-get-started-portal/indexes-list.png)

From this list, you can click on the *hotels-sample* index that you just created, view the index schema. and optionally add new fields. 

The **Fields** tab shows the index schema. Scroll to the bottom of the list to enter a new field. In most cases, you cannot change existing fields. Existing fields have a physical representation in Azure Cognitive Search and are thus non-modifiable, not even in code. To fundamentally change an existing field, create a new index, dropping the original.

   ![Örnek dizin tanımı](media/search-get-started-portal/sample-index-def.png)

Puanlama profilleri ve CORS seçenekleri gibi diğer yapılar herhangi bir noktada eklenebilir.

Dizin tasarımı sırasında düzenleme yapabileceğiniz ve yapamayacağınız alanları kavramak için birkaç dakikanızı ayırarak dizin tanımı seçeneklerini görüntüleyin. Gri renkli seçenekler, bir değerin değiştirme veya silme işlemleri için uygun olmadığını gösterir. 

## <a name="query-index"></a> Query using Search explorer

Artık yerleşik [**Arama gezgini**](search-explorer.md) sorgu sayfasını kullanarak sorgulayabileceğiniz bir arama dizinine sahipsiniz. Bu sayfada rastgele sorgu dizelerini test etmek için kullanabileceğiniz bir arama kutusu bulunur.

**Search explorer** is only equipped to handle [REST API requests](https://docs.microsoft.com/rest/api/searchservice/search-documents), but it accepts syntax for both [simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) and [full Lucene query parser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus all the search parameters available in [Search Document REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) operations.

> [!TIP]
> The following steps are demonstrated at 6m08s into the [Azure Cognitive Search Overview video](https://channel9.msdn.com/Events/Connect/2016/138).
>

1. Komut çubuğunda **Arama gezgini**'ne tıklayın.

   ![Search gezgini komutu](media/search-get-started-portal/search-explorer-cmd.png)

2. From the **Index** dropdown, choose  *hotels-sample-index*. Click the **API Version** dropdown, to see which REST APIs are available. For the queries below, use the generally available version (2019-05-06).

   ![Dizin ve API komutları](media/search-get-started-portal/search-explorer-changeindex.png)

3. In the search bar, paste in the query strings below and click **Search**.

   ![Query string and search button](media/search-get-started-portal/search-explorer-query-string-example.png)

## <a name="example-queries"></a>Example queries

You can enter terms and phrases, similar to what you might do in a Bing or Google search, or fully-specified query expressions. Results are returned as verbose JSON documents.

### <a name="simple-query-with-top-n-results"></a>İlk N sonucu içeren basit sorgu

#### <a name="example-string-query-searchspa"></a>Example (string query): `search=spa`

* The **search** parameter is used to input a keyword search for full text search, in this case, returning hotel data for those containing *spa* in any searchable field in the document.

* **Search gezgini** sonuçları JSON biçiminde döndürülür. Bu biçim ayrıntılı olmakla birlikte, belgelerin yoğun bir yapısı varsa okunması zordur. This is intentional; visibility into the entire document is important for development purposes, especially during testing. Daha iyi bir kullanıcı deneyimi sunmak için [arama sonuçlarını işleyerek](search-pagination-page-layout.md) önemli öğeleri öne çıkaran bir kod yazmanız gerekir.

* Belgeler, dizinde "alınabilir" olarak işaretlenmiş tüm alanlardan oluşur. To view index attributes in the portal, click *hotels-sample* in the **Indexes** list.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Example (parameterized query): `search=spa&$count=true&$top=10`

* **&** simgesi, herhangi bir sırada belirtilebilen arama parametreleri eklemek için kullanılır.

* The **$count=true** parameter returns the total count of all documents returned. Bu değer arama sonuçlarının en üstüne yakın bir konumda görünür. **$count=true** tarafından bildirilen değişiklikleri izleyerek filtre sorgularını doğrulayabilirsiniz. Daha küçük sayılar filtrenizin çalıştığını gösterir.

* The **$top=10** returns the highest ranked 10 documents out of the total. By default, Azure Cognitive Search returns the first 50 best matches. **$top** ile bu miktarı artırabilir veya azaltabilirsiniz.

### <a name="filter-query"></a> Sorguyu filtreleme

**$filter** parametresini eklediğinizde, arama isteklerine filtreler de eklenir. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Örnek (filtrelenmiş): `search=beach&$filter=Rating gt 4`

* **$filter** parametresi, sağladığınız ölçütlerle eşleşen sonuçları döndürür. In this case, ratings greater than 4.

* Filtre söz dizimi bir OData yapısıdır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

### <a name="facet-query"></a> Sorguyu modelleme

Model filtreleri arama isteklerine dahil edilir. Sağladığınız model değeriyle eşleşen belgelerin toplam sayısını döndürmek için facet parametresini kullanabilirsiniz.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Örnek (kapsamı azaltılarak modellenmiş): `search=*&facet=Category&$top=2`

* **search=** * boş bir aramadır. Boş aramalar her şeyi arar. Boş sorgu göndermenin nedenlerinden biri, belge kümesinin tamamını filtrelemek veya görüntülemektir. For example, you want a faceting navigation structure to consist of all hotels in the index.
* **facet**, bir kullanıcı arabirimi denetimine geçirebileceğiniz bir gezinti yapısı döndürür. Kategorileri ve bir sayımı döndürür. In this case, categories are based on a field conveniently called *Category*. There is no aggregation in Azure Cognitive Search, but you can approximate aggregation via `facet`, which gives a count of documents in each category.

* **$top=2** iki belge getirir ve sonuçları azaltmak veya artırmak için `top` kullanabileceğinizi gösterir.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Example (facet on numeric values): `search=spa&facet=Rating`

* This query is facet for rating, on a text search for *spa*. The term *Rating* can be specified as a facet because the field is marked as retrievable, filterable, and facetable in the index, and the values it contains (numeric, 1 through 5), are suitable for categorizing listings into groups.

* Yalnızca filtrelenebilir alanlardan görünüm oluşturulabilir. Yalnızca getirilebilir alanlar sonuçlarda döndürülebilir.

* The *Rating* field is double-precision floating point and the grouping will be by precise value. For more information on grouping by interval (for instance, "3 star ratings," "4 star ratings," etc.), see [How to implement faceted navigation in Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-faceted-navigation#filter-based-on-a-range).


### <a name="highlight-query"></a> Arama sonuçlarını vurgulama

İsabet vurgulama, belirli bir alanda eşleşme bulunduğunda anahtar sözcükle eşleşen metinlere biçimlendirme eklenmesini ifade eder. Arama teriminiz uzun bir açıklamanın belirsiz bir yerindeyse, terimi bulmayı kolaylaştırmak için isabet vurgulama ekleyebilirsiniz.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Örnek (vurgulama): `search=beach&highlight=Description`

* In this example, the formatted word *beach* is easier to spot in the description field.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Örnek (dilbilimsel analiz): `search=beaches&highlight=Description`

* Full text search recognizes basic variations in word forms. In this case, search results contain highlighted text for "beach", for hotels that have that word in their searchable fields, in response to a keyword search on "beaches". Dilbilimsel analiz nedeniyle sonuçlarda aynı kelimenin farklı biçimleri görüntülenebilir. 

* Azure Cognitive Search supports 56 analyzers from both Lucene and Microsoft. The default used by Azure Cognitive Search is the standard Lucene analyzer.

### <a name="fuzzy-search"></a> Belirsiz aramayı deneme

By default, misspelled query terms, like *seatle* for "Seattle", fail to return matches in typical search. Aşağıdaki örnek sonuç döndürmez.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Örnek (yanlış yazılmış terim, işlenmiyor): `search=seatle`

Yazım hatalarını işlemek için belirsiz aramayı kullanabilirsiniz. Belirsiz arama, tam Lucene sorgu söz dizimini kullandığınızda etkinleştirilir ve bunun için yapmanız gereken iki işlem vardır: sorguda **queryType=full** belirtin ve arama sorgusunun sonuna **~** ekleyin.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Örnek (yanlış yazılmış terim, işleniyor): `search=seatle~&queryType=full`

This example now returns documents that include matches on "Seattle".

**queryType** belirtildiğinde varsayılan basit sorgu ayrıştırıcı kullanılır. Basit sorgu ayrıştırıcı daha hızlıdır ancak belirsiz arama, normal ifadeler, yakınlık araması ya da diğer gelişmiş sorgu türlerini kullanmanız gerekiyorsa tam söz dizimi gereklidir.

Belirsiz arama ve joker karakter araması, arama sonucunu etkiler. Bu sorgu biçimlerinde dilbilimsel analiz gerçekleştirilmez. Before using fuzzy and wildcard search, review [How full text search works in Azure Cognitive Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) and look for the section about exceptions to lexical analysis.

For more information about query scenarios enabled by the full query parser, see [Lucene query syntax in Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

### <a name="geo-search"></a> Jeo-uzamsal aramayı deneme

Koordinat içeren bir alanda [edm.GeographyPoint veri türü](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) aracılığıyla jeo-uzamsal arama desteklenir. Coğrafi arama, [OData söz dizimini filtrele](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) seçeneğinde belirtilen bir tür filtredir.

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Örnek (coğrafi koordinat filtreleri): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Örnek sorgu tüm sonuçları konumsal verilere göre filtreler ve belirli bir noktaya 5 kilometreden daha yakın olan sonuçlar (enlem ve boylam koordinatları olarak belirtilir) döndürülür. **$count** ekleyerek mesafeyi veya koordinatları değiştirdiğinizde döndürülen sonuç sayısını görebilirsiniz.

Arama uygulamanız "yakınımda bul" özelliği içeriyorsa ya da harita navigasyonu kullanıyorsa jeo-uzamsal arama kullanışlıdır. Ancak tam metin arama değildir. If you have user requirements for searching on a city or country/region by name, add fields containing city or country/region names, in addition to coordinates.

## <a name="takeaways"></a>Paketler

This tutorial provided a quick introduction to Azure Cognitive Search using the Azure portal.

**Verileri içeri aktar** sihirbazını kullanarak arama dizini oluşturmayı öğrendiniz. [Dizin oluşturucuların](search-indexer-overview.md) yanı sıra [yayımlanmış dizinler için desteklenen değişiklikler](https://docs.microsoft.com/rest/api/searchservice/update-index) gibi dizin tasarımıyla ilgili temel iş akışı hakkında bilgi edindiniz.

Azure portalda **Arama gezginini** kullanarak filtreler, sonuç vurgulama, belirsiz arama ve coğrafi arama gibi önemli özelliklerin gösterildiği örneklerle temel sorgu söz dizimini öğrendiniz.

You also learned how to find indexes, indexers, and data sources in the portal. Gelecekte yeni verilerle karşılaştığınızda portalı kullanarak tanımlarını veya alan koleksiyonlarını hızlı bir şekilde denetleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

When you're working in your own subscription, it's a good idea at the end of a project to identify whether you still need the resources you created. Resources left running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.

You can find and manage resources in the portal, using the **All resources** or **Resource groups** link in the left-navigation pane.

If you are using a free service, remember that you are limited to three indexes, indexers, and data sources. You can delete individual items in the portal to stay under the limit. 

## <a name="next-steps"></a>Sonraki adımlar

While the portal is useful for initial exploration and small tasks, reviewing the APIs early on will help you understand the concepts and workflow on a deeper level:

> [!div class="nextstepaction"]
> [Create an index using .NET SDK](https://docs.microsoft.com/azure/search/search-create-index-dotnet)