---
title: "Quickstart: REST API'lerini kullanarak PowerShell'de arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu REST API hızlı başlangıcında, PowerShell'in Invoke-RestMethod ve Azure Bilişsel Arama REST API'sini kullanarak bir dizin oluşturmayı, veri yüklemeyi ve sorguları çalıştırmayı öğrenin.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 02/10/2020
ms.openlocfilehash: 612751c2405cd55ad0b3760aa8e093e434a22f57
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121595"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-powershell-using-rest-apis"></a>Quickstart: REST API'lerini kullanarak PowerShell'de Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C #](search-create-index-dotnet.md)
> * [Postacı (REST)](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

Bu makale, PowerShell ve [Azure Bilişsel Arama REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/)kullanarak bir Azure Bilişsel Arama dizini oluşturma, yükleme ve sorgulama sürecinde size yol gösterir. Bu makalede, PowerShell komutlarının etkileşimli olarak nasıl çalıştırılacakolduğu açıklanmaktadır. Alternatif olarak, aynı işlemleri gerçekleştiren [bir Powershell komut dosyası indirebilir ve çalıştırabilirsiniz.](https://github.com/Azure-Samples/azure-search-powershell-samples/tree/master/Quickstart)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için aşağıdaki hizmetler ve araçlar gereklidir. 

+ [PowerShell 5.1 veya sonraki,](https://github.com/PowerShell/PowerShell)sıralı ve etkileşimli adımlar için [Invoke-RestMethod'u](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) kullanarak.

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

## <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

Tüm istekler, hizmetinize gönderilen her istekiçin bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-cognitive-search"></a>Azure Bilişsel Arama'ya bağlanın

1. PowerShell'de, içerik türü ve API anahtarını depolamak için **$headers** bir nesne oluşturun. Yönetici API anahtarını (YOUR-ADMIN-API-KEY) arama hizmetiniz için geçerli bir anahtarla değiştirin. Bu üstbilginin oturum süresince yalnızca bir kez ayarlanız gerekir, ancak her isteğe eklersiniz. 

    ```powershell
    $headers = @{
    'api-key' = '<YOUR-ADMIN-API-KEY>'
    'Content-Type' = 'application/json' 
    'Accept' = 'application/json' }
    ```

2. Hizmetin dizinler koleksiyonunu belirten **$url** bir nesne oluşturun. Hizmet adını (SİzARAMA-HİzMET-İsİm) geçerli bir arama hizmetiyle değiştirin.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06&$select=name"
    ```

3. Hizmete GET isteği göndermek ve bağlantıyı doğrulamak için **Invoke-RestMethod'u** çalıştırın. Hizmetten geri gönderilen yanıtları görüntüleyebilmeniz için **ConvertTo-Json'ı** ekleyin.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

   Hizmet boşsa ve dizinleri yoksa, sonuçlar aşağıdaki örneğe benzer. Aksi takdirde, dizin tanımlarının JSON temsilini görürsünüz.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes",
        "value":  [

                ]
    }
    ```

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Portalı kullanmıyorsanız, veri yükleyemeden önce hizmette bir dizin bulunması gerekir. Bu adım dizini tanımlar ve hizmete iter. Bu adım için [Create Index REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) kullanılır.

Bir dizin gerekli öğeleri bir ad ve alanlar koleksiyonu içerir. Alanlar koleksiyonu *belgenin*yapısını tanımlar. Her alanın nasıl kullanıldığını belirleyen bir adı, türü ve öznitelikleri vardır (örneğin, arama sonuçlarında tam metin aranabilir mi, filtrelenebilir mi veya alınabiliyor mu). Bir dizin içinde, tür `Edm.String` alanlarından biri belge kimliği için *anahtar* olarak atanmalıdır.

Bu dizin "oteller-quickstart" olarak adlandırılır ve aşağıda gördüğünüz alan tanımları vardır. Diğer walkthroughs kullanılan daha büyük bir [Oteller endeksinin](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) bir alt kümesidir. Kısalık için bu hızlı başlangıçta kestik.

1. Dizin şemasını içeren **$body** bir nesne oluşturmak için bu örneği PowerShell'e yapıştırın.

    ```powershell
    $body = @"
    {
        "name": "hotels-quickstart",  
        "fields": [
            {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
            {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
            {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
            {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
            {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
            {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Address", "type": "Edm.ComplexType", 
            "fields": [
            {"name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true},
            {"name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true}
            ]
         }
      ]
    }
    "@
    ```

2. URI'yi hizmetinizdeki dizinler koleksiyonuna ve *otellerin hızlı başlangıç* dizinine ayarlayın.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06"
    ```

3. Hizmetteki dizin oluşturmak için komutu **$url**, **$headers**ve **$body** ile çalıştırın. 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    Sonuçlar buna benzer olmalıdır (kısalık için ilk iki alana kesilir):

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6EDE28CFEABDA\"",
        "name":  "hotels-quickstart",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "HotelId",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "HotelName",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  false,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  false,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> Doğrulama için, portaldaki Dizinler listesini de kontrol edebilirsiniz.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - Belgeleri yükleyin

Belgeleri zorlamak için, dizininizin URL bitiş noktasına bir HTTP POST isteği kullanın. Bu göreviçin REST API [Belgeleri Ekle, Güncelleştir veya Sil'](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)dir.

1. Yüklemek istediğiniz belgeleri içeren **$body** bir nesne oluşturmak için bu örneği PowerShell'e yapıştırın. 

    Bu istek iki tam ve bir kısmi kayıt içerir. Kısmi kayıt, eksik belgeleri yükleyebileceğinizi gösterir. Parametre, `@search.action` dizin oluşturmanın nasıl yapıldığını belirtir. Geçerli değerler yükleme, birleştirme, birleştirmeOrUpload ve silme yi içerir. BirleştirmeOrUpload davranışı, hotelId = 3 için yeni bir belge oluşturur veya zaten varsa içeriği güncelleştirir.

    ```powershell
    $body = @"
    {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
            "StreetAddress": "677 5th Ave",
            "City": "New York",
            "StateProvince": "NY",
            "PostalCode": "10022",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
            "StreetAddress": "140 University Town Center Dr",
            "City": "Sarasota",
            "StateProvince": "FL",
            "PostalCode": "34243",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "3",
        "HotelName": "Triple Landscape Hotel",
        "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": 
            {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "4",
        "HotelName": "Sublime Cliff Hotel",
        "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": 
            {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
            }
        }
    ]
    }
    "@
    ```

1. Bitiş noktasını *otellerin hızlı başlatma* dokümanları koleksiyonuna ayarlayın ve dizin işlemini (dizinler/oteller-quickstart/docs/index) ekleyin.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06"
    ```

1. Belgeleri otellerin hızlı başlatma dizinine yüklemek için **komutu $url**, **$headers**ve **$body** çalıştırın.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    Sonuçlar aşağıdaki örneğe benzer olmalıdır. [201'in durum kodunu](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes)görmelisiniz.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
        "value":  [
                    {
                        "key":  "1",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "2",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "3",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "4",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - Dizin arama

Bu adım, [Arama Belgeleri API'sini](https://docs.microsoft.com/rest/api/searchservice/search-documents)kullanarak bir dizini nasıl sorgulayabileceğinizi gösterir.

Arama $urls tek tırnak kullandığınızdan emin olun. Sorgu dizeleri **$** karakterleri içerir ve tüm dize tek tırnak içinde kapalı ise onları kaçmak zorunda atlayabilirsiniz...

1. Bitiş noktasını *otellerin hızlı başlatma* dokümanları koleksiyonuna ayarlayın ve sorgu dizesinde geçmek için bir **arama** parametresi ekleyin. 
  
   Bu dize, rasgele belgelerin sıralanmamış listesini (arama puanı = 1,0) döndürerek boş bir aramayı (search=*) yürütür. Varsayılan olarak, Azure Bilişsel Arama bir seferde 50 eşleşme döndürür. Yapılandırılmış olarak, bu sorgu tüm belge yapısını ve değerlerini döndürür. Sonuçlardaki tüm belgelerin sayısını almak için **$count=True** ekleyin.

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$count=true'
    ```

1. **$url** hizmete göndermek için komutu çalıştırın.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    Sonuçlar aşağıdaki çıktıya benzer olmalıdır.

    ```
    {
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#docs(*)",
    "@odata.count":  4,
    "value":  [
                  {
                      "@search.score":  0.1547872,
                      "HotelId":  "2",
                      "HotelName":  "Twin Dome Motel",
                      "Description":  "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                      "Category":  "Boutique",
                      "Tags":  "pool free wifi concierge",
                      "ParkingIncluded":  false,
                      "LastRenovationDate":  "1979-02-18T00:00:00Z",
                      "Rating":  3.6,
                      "Address":  "@{StreetAddress=140 University Town Center Dr; City=Sarasota; StateProvince=FL; PostalCode=34243; Country=USA}"
                  },
                  {
                      "@search.score":  0.009068266,
                      "HotelId":  "3",
                      "HotelName":  "Triple Landscape Hotel",
                      "Description":  "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel\u0027s restaurant services.",
                      "Category":  "Resort and Spa",
                      "Tags":  "air conditioning bar continental breakfast",
                      "ParkingIncluded":  true,
                      "LastRenovationDate":  "2015-09-20T00:00:00Z",
                      "Rating":  4.8,
                      "Address":  "@{StreetAddress=3393 Peachtree Rd; City=Atlanta; StateProvince=GA; PostalCode=30326; Country=USA}"
                  },
                . . . 
    ```

Sözdizimini görmek için birkaç sorgu örneğini deneyin. Dize araması yapabilir, sorguları tam olarak $filter, sonuçları ayarlayabilir, aramayı belirli alanlara göre kapsamda ve daha fazlasını yapabilirsiniz.

```powershell
# Query example 1
# Search the entire index for the terms 'restaurant' and 'wifi'
# Return only the HotelName, Description, and Tags fields
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=restaurant wifi&$count=true&$select=HotelName,Description,Tags'

# Query example 2 
# Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$filter=Rating gt 4&$select=HotelName,Rating'

# Query example 3
# Take the top two results, and show only HotelName and Category in the results
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=boutique&$top=2&$select=HotelName,Category'

# Query example 4
# Sort by a specific field (Address/City) in ascending order

$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating'
```
## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Bilişsel Arama'da içerik oluşturmak ve bunlara erişmek için temel iş akışını aşmak için PowerShell'i kullandınız. Kavramları göz önünde bulundurarak, Azure veri kaynaklarından dizin oluşturma gibi daha gelişmiş senaryolara geçmenizi öneririz;

> [!div class="nextstepaction"]
> [REST Tutorial: Azure Bilişsel Arama'da yarı yapılandırılmış verileri (JSON blobs) dizinle ve arama](search-semi-structured-data.md)