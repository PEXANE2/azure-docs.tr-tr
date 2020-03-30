---
title: Azure portalında explorer sorgusu aracını arama
titleSuffix: Azure Cognitive Search
description: Bu Azure portalında hızlı başlatın, sorgu sözdizimini öğrenmek, sorgu ifadelerini test etmek veya bir arama belgesini incelemek için Arama Gezgini'ni kullanın. Arama gezgini Azure Bilişsel Arama'da dizinleri sorgular.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 03/27/2020
ms.openlocfilehash: 9fb34141d19866a2f49ac164e0d89802cf7818c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369692"
---
# <a name="quickstart-use-search-explorer-to-run-queries-in-the-portal"></a>Hızlı başlangıç: Portaldaki sorguları çalıştırmak için Arama gezginini kullanma

**Arama gezgini,** Azure Bilişsel Arama'da sorguları arama dizinine karşı çalıştırmak için kullanılan yerleşik bir sorgu aracıdır. Bu araç sorgu sözdizimini öğrenmeyi, sorguyu test etmeyi veya ifadeyi filtrelemeyi veya yeni içeriğin var olduğunu doğrulayarak dizin yenileme sonuçlarını onaylamayı kolaylaştırır.

Bu hızlı başlangıç, Arama gezginini göstermek için **emlak-us-örnek dizini** kullanır. İstekler [Arama REST API](https://docs.microsoft.com/rest/api/searchservice/)kullanılarak formüle edilir ve yanıtlar JSON belgeleri olarak döndürülür.

## <a name="prerequisites"></a>Ön koşullar

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

+ **realestate-us-örnek-indeksi** bu hızlı başlangıç için kullanılır. Yerleşik örnekler veri kaynağından dizin oluşturmak için [**İçe Aktarma veri**](search-import-data-portal.md) sibunu ile adım atın.

## <a name="start-search-explorer"></a>Arama gezginini başlat

1. Azure [portalında,](https://portal.azure.com)panodan arama hizmeti sayfasını açın veya [hizmetinizi bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)

1. Komut çubuğundan Arama gezginini aç:

   ![Portalda explorer arama komutu](./media/search-explorer/search-explorer-cmd2.png "Portalda explorer arama komutu")

    Veya açık bir dizin üzerinde katışnalı **Arama gezgini** sekmesini kullanın:

   ![Explorer sekmesini ara](./media/search-explorer/search-explorer-tab.png "Explorer sekmesini ara")

## <a name="unspecified-query"></a>Belirtilmeyen sorgu

İçerime ilk kez bakmak için, hiçbir terim sunulmadan **Ara'yı** tıklatarak boş bir arama gerçekleştirin. Belge kompozisyonunu gözden geçirebilmeniz için tüm belgeleri döndürdeğinden boş bir arama ilk sorgu olarak yararlıdır. Boş bir aramada, arama sırası yoktur ve belgeler (tüm belgeler`"@search.score": 1` için) rasgele sırayla döndürülür. Varsayılan olarak, bir arama isteğinde 50 belge döndürülür.

Boş bir arama için `*` eşdeğer `search=*`sözdizimi veya .
   
   ```http
   search=*
   ```

   **Sonuç -ları**
   
   ![Boş sorgu örneği](./media/search-explorer/search-explorer-example-empty.png "Niteliksiz veya boş sorgu örneği")

## <a name="free-text-search"></a>Ücretsiz metin arama

Operatörlü veya operatörsüz serbest biçimli sorgular, özel bir uygulamadan Azure Bilişsel Arama'ya gönderilen kullanıcı tanımlı sorguları simüle etmek için yararlıdır. Yalnızca dizin tanımında **Aranabilir** olarak atfedilen alanlar eşleşmeler için taranır. 

Sorgu terimleri veya ifadeler gibi arama ölçütleri sağladığınızda, arama sıralamasının devreye girdiğini unutmayın. Aşağıdaki örnekte ücretsiz bir metin araması gösteriş verilmiştir.

   ```http
   Seattle apartment "Lake Washington" miele OR thermador appliance
   ```

   **Sonuç -ları**

   Belirli ilgi çekici terimler için sonuçlarında arama yapmak için Ctrl- F' yi kullanabilirsiniz.

   ![Ücretsiz metin sorgusu örneği](./media/search-explorer/search-explorer-example-freetext.png "Ücretsiz metin sorgusu örneği")

## <a name="count-of-matching-documents"></a>Eşleşen belgelerin sayısı 

Bir dizinte bulunan eşleşme sayısını almak için **$count=True** ekleyin. Boş bir aramada, sayım dizindeki toplam belge sayısıdır. Nitelikli bir aramada, sorgu girişiyle eşleşen belge sayısıdır.

   ```http
   $count=true
   ```

   **Sonuç -ları**

   ![Belge sayısı örneği](./media/search-explorer/search-explorer-example-count.png "Dizinteki eşleşen belgelerin sayısı")

## <a name="limit-fields-in-search-results"></a>Arama sonuçlarında alanları sınırlandır

**Arama gezgininde**daha okunabilir çıktı için sonuçları açık adlandırılmış alanlara sınırlamak için [**$select**](search-query-odata-select.md) ekleyin. Arama dizesini ve **$count=true,** önek bağımsız değişkenlerini **&**. 

   ```http
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true
   ```

   **Sonuç -ları**

   ![Alanları sınırla örneği](./media/search-explorer/search-explorer-example-selectfield.png "Arama sonuçlarında alanları kısıtlama")

## <a name="return-next-batch-of-results"></a>Sonraki sonuç toplu larını döndürme

Azure Bilişsel Arama, arama sıralamasına bağlı olarak en iyi 50 eşleşmeyi döndürür. Bir sonraki eşleşen belge kümesini almak için, sonucu 100 belgeye (varsayılan 50, en fazla 1000'dir) artırmak için **$top=100.&$skip=50** ekler, ilk 50 belgeyi atlayın. Sıralanmış sonuçlar almak için sorgu terimi veya ifade gibi arama ölçütleri sağlamanız gerektiğini hatırlayın. Arama puanlarının arama sonuçlarına ne kadar derin ulaşırsanız o kadar azaldığına dikkat edin.

   ```http
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true&$top=100&$skip=50
   ```

   **Sonuç -ları**

   ![Toplu arama sonuçları](./media/search-explorer/search-explorer-example-topskip.png "Sonraki arama sonuçları toplu işlerini döndürme")

## <a name="filter-expressions-greater-than-less-than-equal-to"></a>Filtre ifadeleri (büyük, daha az, eşit)

Ücretsiz metin araması yerine kesin ölçütler belirtmek istediğinizde [**$filter**](search-query-odata-filter.md) parametresini kullanın. Alan dizinde **Filtrelenebilir** olarak atfedilmelidir. Bu örnekte 3'ten büyük yatak odaları aranrıyor:

   ```http
   search=seattle condo&$filter=beds gt 3&$count=true
   ```
   
   **Sonuç -ları**

   ![Filtre ifadesi](./media/search-explorer/search-explorer-example-filter.png "Ölçütlere göre filtreleme")

## <a name="order-by-expressions"></a>Sıralı ifadeler

Sonuçları arama puanının yanı sıra başka bir alana göre sıralamak için [**$orderby**](search-query-odata-orderby.md) ekleyin. Alan dizinde **Sıralanabilir** olarak atfedilmelidir. Bunu test etmek için kullanabileceğiniz bir örnek ifade:

   ```http
   search=seattle condo&$select=listingId,beds,price&$filter=beds gt 3&$count=true&$orderby=price asc
   ```
   
   **Sonuç -ları**

   ![Sıralı ifade](./media/search-explorer/search-explorer-example-ordery.png "Sıralama düzenini değiştirme")

Hem **$filter** hem de **$orderby** ifadeler OData yapılardır. Daha fazla bilgi edinmek için bkz. [OData söz dizimini filtreleme](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

<a name="start-search-explorer"></a>

## <a name="takeaways"></a>Paketler

Bu hızlı başlatmada, REST API'sini kullanarak bir dizini sorgulamak için **Arama gezginini** kullandınız.

+ Sonuçlar, belge yapımını ve içeriğini tam olarak görüntüleyebilmeniz için ayrıntılı JSON belgeleri olarak döndürülür. Örneklerde gösterilen sorgu ifadelerini, döndürülen alanları sınırlamak için kullanabilirsiniz.

+ Belgeler, dizinde **Alınabilecek** olarak işaretlenmiş tüm alanlardan oluşur. Portaldaki dizin özniteliklerini görüntülemek için, arama genel bakış sayfasındaki **Dizinler** listesindeki *emlak-us örneğini* tıklatın.

+ Ticari bir web tarayıcısına girebileceklerinize benzer şekilde serbest biçimli sorgular, son kullanıcı deneyimini sınamak için yararlıdır. Örneğin, yerleşik gayrimenkul örnek endeksini varsayarsak, "Seattle apartments lake washington" adresini girebilir ve ardından arama sonuçlarındaki terimleri bulmak için Ctrl-F'yi kullanabilirsiniz. 

+ Sorgu ve filtre ifadeleri Azure Bilişsel Arama tarafından desteklenen bir sözdiziminde ifade edilir. Varsayılan basit bir [sözdizimidir,](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)ancak isteğe bağlı olarak daha güçlü sorgular için [tam Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) kullanabilirsiniz. [Filtre ifadeleri](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) bir OData sözdizimidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Sorgu yapıları ve sözdizimi hakkında daha fazla bilgi edinmek için, API'nin daha fazla parçasından yararlanan sorgu ifadeleri oluşturmak için Postacı'yı veya eşdeğer bir aracı kullanın. [Arama REST API](https://docs.microsoft.com/rest/api/searchservice/) özellikle öğrenme ve keşif için yararlıdır.

> [!div class="nextstepaction"]
> [Postacı'da temel bir sorgu oluşturma](search-query-simple-examples.md)