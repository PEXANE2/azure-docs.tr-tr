---
title: "Quickstart: REST API'lerini kullanarak Postacı'da arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu REST API hızlı başlangıcında, Postacı'yı kullanarak Azure Bilişsel Arama REST API'lerini nasıl arayacağınızı ve örnek verileri ve tanımları öğrenin.
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 02/10/2020
ms.openlocfilehash: c502886aac9d13f7a470a9b83f1fc12334913beb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121640"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-postman-using-rest-apis"></a>Quickstart: REST API'lerini kullanarak Postacı'da Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [Postman](search-get-started-postman.md)
> * [C #](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [Powershell](search-howto-dotnet-sdk.md)
>*

[Azure Bilişsel Arama REST API'lerini](https://docs.microsoft.com/rest/api/searchservice) keşfetmenin en kolay yollarından biri, HTTP isteklerini formüle etmek ve yanıtları incelemek için Postacı veya başka bir web test aracı kullanmaktır. Doğru araçlar ve bu yönergelerden yararlanarak herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

Bu makalede, isteklerin etkileşimli olarak nasıl formüle edilecek olduğu açıklanmaktadır. Alternatif olarak, önceden tanımlanmış istekleri kullanmak için [bir Postacı koleksiyonu indirebilir ve içe aktarabilirsiniz.](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Quickstart)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için aşağıdaki hizmetler ve araçlar gereklidir. 

+ [Postacı masaüstü uygulaması,](https://www.getpostman.com/) Azure Bilişsel Arama'ya istek göndermek için kullanılır.

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

## <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

Tüm istekler, hizmetinize gönderilen her istekiçin bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-cognitive-search"></a>Azure Bilişsel Arama'ya bağlanın

Bu bölümde, Azure Bilişsel Arama bağlantıları kurmak için seçtiğiniz web aracını kullanın. Her araç, oturum için üstbilgi isteği nde kalır, bu da api tuşu ve İçerik Türü'ne yalnızca bir kez girmeniz anlamına gelir.

Her iki araç için de bir komut (GET, POST, PUT ve benzeri) seçmeniz, bir URL bitiş noktası sağlamanız ve bazı görevler için isteğin gövdesinde JSON sağlamanız gerekir. Arama hizmeti adını (SİzİARAMA-HİzMET-İsİm) geçerli bir değerle değiştirin. Her `$select=name` dizinin adını döndürmek için ekleyin. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06&$select=name

HTTPS önekine, hizmetin adına, bir nesnenin adına (bu durumda, dizinler koleksiyonuna) ve [api sürümüne](search-api-versions.md)dikkat edin. Api sürümü, geçerli sürüm için `?api-version=2019-05-06` belirtilen gerekli, küçük bir dizedir. API sürümleri düzenli olarak güncelleştirilir. api-version parametresini her isteğe dahil etmeniz hangisinin kullanıldığıyla ilgili tam denetim sahibi olmanızı sağlar.  

İstek üstbilgi kompozisyonu, içerik türü ve Azure Bilişsel Arama'ya kimlik doğrulamak için kullanılan api anahtarı olmak üzere iki öğe içerir. Yönetici API anahtarını (YOUR-AZURE-SEARCH-ADMIN-API-KEY) geçerli bir değerle değiştirin. 

    api-key: <YOUR-AZURE-SEARCH-ADMIN-API-KEY>
    Content-Type: application/json

Postman'da, aşağıdaki ekran görüntüsüne benzeyen bir istek formüle edin. Fiil olarak **AL'yi** seçin, URL'yi sağlayın ve **Gönder'i**tıklatın. Bu komut Azure Bilişsel Arama'ya bağlanır, dizinler koleksiyonunu okur ve başarılı bir bağlantıda HTTP durum kodu 200'ü döndürür. Hizmetinizin zaten dizinleri varsa, yanıt dizin tanımlarını da içerir.

![Postacı istek URL'si ve üstbilgi](media/search-get-started-postman/postman-url.png "Postacı istek URL'si ve üstbilgi")

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Azure Bilişsel Arama'da, genellikle dizin oluşturmadan önce verilerle birlikte yüklersiniz. Bu görev için [Create Index REST API](https://docs.microsoft.com/rest/api/searchservice/create-index) kullanılır. 

URL, `hotels` dizin adını içerecek şekilde genişletilir.

Postacı bunu yapmak için:

1. Fiili **PUT**olarak değiştirin.

2. Bu URL'yi `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06`kopyalayın.

3. İstek gövdesinde dizin tanımını (kopyaya hazır kod aşağıda verilmiştir) sağlayın.

4. **Gönder**’e tıklayın.

![İstek gövdesinde Dizin JSON belge](media/search-get-started-postman/postman-request.png "İstek gövdesinde Dizin JSON belge")

### <a name="index-definition"></a>Dizin tanımı

Alanlar koleksiyonu belge yapısını tanımlar. Her belgede bu alanlar alabilmeli ve her alanın bir veri türü olmalıdır. Dize alanları tam metin araması için kullanılır. Bu nedenle içerikte arama yapılabilmesini istiyorsanız sayısal verileri dize olarak ayarlamak isteyebilirsiniz.

Alan öznitelikleri izin verilen eylemi belirler. REST API'leri varsayılan olarak birçok eyleme izin verir. Örneğin tüm dizelerde arama, getirme, filtreleme ve modelleme özellikleri varsayılan olarak etkindir. Genellikle, yalnızca bir davranışı kapatmanız gerektiğinde öznitelikleri ayarlamanız gerekir.

```json
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
```

Bu isteği gönderdiğinizde dizinin başarıyla oluşturulduğunu belirten HTTP 201 yanıtı almanız gerekir. Bu eylemi portaldan doğrulayabilirsiniz ancak portal sayfasındaki yenileme aralıkları nedeniyle bilgilerin güncellenmesi bir-iki dakika sürebilir.

> [!TIP]
> HTTP 504 yanıtı alırsanız HTTPS'yi belirten URL'yi doğrulayın. HTTP 400 veya 404 yanıtı görürseniz kopyala-yapıştır hatası olmadığını doğrulamak için istek gövdesini kontrol edin. HTTP 403 genelde api anahtarı ile ilgili bir sorunu gösterir (geçersiz anahtar veya api anahtarının nasıl belirtildiğine ilişkin söz dizimi sorunu).

## <a name="2---load-documents"></a>2 - Belgeleri yükleyin

Dizini oluşturma ve dizini doldurma ayrı adımlardır. Azure Bilişsel Arama'da dizin, JSON belgeleri olarak sağlayabileceğiniz tüm aranabilir verileri içerir. Bu görev için [Belgeleri Ekle, Güncelleştir veya Sil REST API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) kullanılır. 

URL `docs` koleksiyonları ve `index` işlemi içerecek şekilde genişletilir.

Postacı bunu yapmak için:

1. Fiili **POST** olarak değiştirin.

2. Bu URL'yi `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06`kopyalayın.

3. JSON belgelerini (kopyaya hazır kod aşağıdadır) isteğin gövdesinde sağlayın.

4. **Gönder**’e tıklayın.

![JSON belgeleri istek gövdesinde](media/search-get-started-postman/postman-docs.png "JSON belgeleri istek gövdesinde")

### <a name="json-documents-to-load-into-the-index"></a>JSON belgeleri dizin içine yüklemek için

İstek Gövdesi, oteller dizinine eklenecek dört belge içerir.

```json
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
```

Birkaç saniye içinde, oturum listesinde bir HTTP 201 yanıtı görmeniz gerekir. Bu, belgelerin başarıyla oluşturulduğunu belirtir. 

207 yanıtı alırsanız en az bir belge karşıya yüklenemedi. 404 yanıtı alırsanız üst bilgi veya istek gövdesinde söz dizimi hatanız vardır. Uç noktayı `/docs/index` içerecek şekilde değiştirdiğinizden emin olun.

> [!Tip]
> Seçili veri kaynaklarında dizin oluşturma için gerekli kodları sadeleştiren ve miktarını azaltan alternatif *dizin oluşturucu* yaklaşımını kullanabilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucu işlemleri](https://docs.microsoft.com/rest/api/searchservice/indexer-operations).


## <a name="3---search-an-index"></a>3 - Dizin arama

Artık bir dizin ve belgeler yüklendiğine göre, [Arama Belgeleri REST API'yi](https://docs.microsoft.com/rest/api/searchservice/search-documents)kullanarak onlara karşı sorgular düzenleyebilirsiniz.

URL, arama işleci kullanılarak belirtilen bir sorgu ifadesini içerecek şekilde genişletilir.

Postacı bunu yapmak için:

1. Fiili **GET**olarak değiştirin.

2. Bu URL'yi `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$count=true&api-version=2019-05-06`kopyalayın.

3. **Gönder**’e tıklayın.

Bu sorgu boştur ve arama sonuçlarındaki belgelerin sayısını döndürür. İstek ve yanıt, **Gönder'i**tıklattıktan sonra Postacı için aşağıdaki ekran görüntüsüne benzer olmalıdır. Durum kodu 200 olmalıdır.

 ![URL'deki arama dizesi ile GET](media/search-get-started-postman/postman-query.png "URL'deki arama dizesi ile GET")

Sözdizimini görmek için birkaç sorgu örneğini deneyin. Dize araması yapabilir, sorguları tam olarak $filter, sonuçları ayarlayabilir, aramayı belirli alanlara göre kapsamda ve daha fazlasını yapabilirsiniz.

Sonuçları görüntülemek için her seferinde **Gönder'i** tıklatarak geçerli URL'yi aşağıdakiurl'lerle değiştirin.

```
# Query example 1 - Search on restaurant and wifi
# Return only the HotelName, Description, and Tags fields
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=restaurant wifi&$count=true&$select=HotelName,Description,Tags&api-version=2019-05-06

# Query example 2 - Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelName and Rating. Two documents match
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=*&$filter=Rating gt 4&$select=HotelName,Rating&api-version=2019-05-06

# Query example 3 - Take the top two results, and show only HotelName and Category in the results
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=boutique&$top=2&$select=HotelName,Category&api-version=2019-05-06

# Query example 4 - Sort by a specific field (Address/City) in ascending order
https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?search=pool&$orderby=Address/City asc&$select=HotelName, Address/City, Tags, Rating&api-version=2019-05-06
```

## <a name="get-index-properties"></a>Dizin özelliklerini alma
Belge sayımları ve dizin boyutu için sorgulamak için [İstatistik al'ı](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) da kullanabilirsiniz: 

```
https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels-quickstart/stats?api-version=2019-05-06
```

URL `/stats` döndürme dizin bilgilerinize ekleme. Postman uygulamasında isteğinizin aşağıdakine benzer olması ve yanıtta belge sayısı ile kullanılan alanın bayt cinsinden değerinin belirtilmesi gerekir.

 ![Dizin bilgilerini alma](media/search-get-started-postman/postman-system-query.png "Dizin bilgilerini alma")

api-version söz diziminin farklı olduğuna dikkat edin. Bu istek için api-version parametresine `?` ekleyin. URL `?` yolunu sorgu dizesinden ayırırken, & sorgu dizesindeki her 'ad=değer' çiftini ayırır. Bu sorgu için api-version, sorgu dizesindeki ilk ve tek öğedir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Artık temel görevleri nasıl gerçekleştirebileceğinizi bildiğinize göre, dizin leyiciler veya bilişsel arama [ardışık bir denetim hattı kurma](cognitive-search-tutorial-blob.md)gibi daha gelişmiş özellikler için ek REST API çağrılarıyla ilerleyebilirsiniz. Bir sonraki adımiçin aşağıdaki bağlantıyı öneririz:

> [!div class="nextstepaction"]
> [REST Tutorial: Azure Bilişsel Arama'da yarı yapılandırılmış verileri (JSON blobs) dizinle ve arama](search-semi-structured-data.md)
