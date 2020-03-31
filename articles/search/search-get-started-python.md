---
title: "Quickstart: REST API'lerini kullanarak Python'da arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Python, Jupyter Notebook'lar ve Azure Bilişsel Arama REST API'sini kullanarak dizin oluşturmayı, verileri yüklemeyi ve sorguları çalıştırmayı açıklar.
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 02/10/2020
ms.openlocfilehash: 93fb9ec735de1abf89eb217d0f4096fcfc0afe94
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78227107"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-python-using-jupyter-notebooks"></a>Quickstart: Jupyter dizüstü bilgisayarları kullanarak Python'da Azure Bilişsel Arama dizini oluşturma

> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C #](search-create-index-dotnet.md)
> * [Postacı (REST)](search-get-started-postman.md)
> * [Portal](search-create-index-portal.md)
> 

Python ve [Azure Bilişsel Arama REST API'lerini](https://docs.microsoft.com/rest/api/searchservice/)kullanarak bir Azure Bilişsel Arama dizini oluşturan, yükleyen ve sorgulayan bir Jupyter dizüstü bilgisayar oluşturun. Bu makalede, not defteri adım adım nasıl inşa edilebisiz açıklanmaktadır. Alternatif olarak, [bitmiş bir Jupyter Python not defterini indirip çalıştırabilirsiniz.](https://github.com/Azure-Samples/azure-search-python-samples)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç için aşağıdaki hizmetler ve araçlar gereklidir. 

+ [Anaconda 3.x](https://www.anaconda.com/distribution/#download-section), Python 3.x ve Jupyter Notebooksağlayan.

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için Ücretsiz katmanı kullanabilirsiniz. 

## <a name="get-a-key-and-url"></a>Bir anahtar ve URL alın

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

![Http bitiş noktası ve erişim anahtarı alın](media/search-get-started-postman/get-url-key.png "Http bitiş noktası ve erişim anahtarı alın")

Tüm istekler, hizmetinize gönderilen her istekiçin bir api anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="connect-to-azure-cognitive-search"></a>Azure Bilişsel Arama'ya bağlanın

Bu görevde, bir Jupyter dizüstü bilgisayar başlatın ve Azure Bilişsel Arama'ya bağlanabileceğinizi doğrulayın. Bunu, hizmetinizden bir dizin listesi isteyerek yaparsınız. Anaconda3 ile Windows'da, bir dizüstü bilgisayar başlatmak için Anaconda Navigator'ı kullanabilirsiniz.

1. Yeni bir Python3 dizüstü bilgisayarı oluşturun.

1. İlk hücrede, JSON ile çalışmak ve HTTP isteklerini formüle etmek için kullanılan kitaplıkları yükleyin.

   ```python
   import json
   import requests
   from pprint import pprint
   ```

1. İkinci hücrede, her istekte sabit olacak istek öğelerini girdi. Arama hizmeti adını (Sİz-ARAMA-HİzMET-NAME) ve yönetici API anahtarını (YOUR-ADMIN-API-KEY) geçerli değerlerle değiştirin. 

   ```python
   endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
   api_version = '?api-version=2019-05-06'
   headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>' }
   ```

   ConnectionError `"Failed to establish a new connection"`alırsanız, api anahtarının birincil veya ikincil yönetici anahtarı olduğunu ve tüm satır`?` ve `/`sondaki karakterlerin (ve) yerinde olduğunu doğrulayın.

1. Üçüncü hücrede, isteği formüle edin. Bu GET isteği, arama hizmetinizin dizin koleksiyonunu hedefler ve varolan dizinlerin ad özelliğini seçer.

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. Her adımı çalıştırın. Dizinler varsa, yanıt dizin adlarının listesini içerir. Aşağıdaki ekran görüntüsünde, hizmetzaten bir azureblob-index ve bir realestate-us-örnek indeksi vardır.

   ![Azure Bilişsel Arama'ya HTTP istekleriyle Jupyter not defterinde Python komut dosyası](media/search-get-started-python/connect-azure-search.png "Azure Bilişsel Arama'ya HTTP istekleriyle Jupyter not defterinde Python komut dosyası")

   Buna karşılık, boş bir dizin koleksiyonu bu yanıtı döndürür:`{'@odata.context': 'https://mydemo.search.windows.net/$metadata#indexes(name)', 'value': []}`

## <a name="1---create-an-index"></a>1 - Dizin oluşturma

Portalı kullanmıyorsanız, veri yükleyemeden önce hizmette bir dizin bulunması gerekir. Bu adım, bir dizin şemasını hizmete itmek için [Dizin REST API'sını kullanır.](https://docs.microsoft.com/rest/api/searchservice/create-index)

Dizinin gerekli öğeleri bir ad, alanlar koleksiyonu ve bir anahtar içerir. Alanlar koleksiyonu *belgenin*yapısını tanımlar. Her alanın, alanın nasıl kullanılacağını belirleyen bir adı, türü ve öznitelikleri vardır (örneğin, arama sonuçlarında tam metin aranabilir mi, filtrelenebilir mi veya alınabilen). Bir dizin içinde, tür `Edm.String` alanlarından biri belge kimliği için *anahtar* olarak atanmalıdır.

Bu dizin "oteller-quickstart" olarak adlandırılır ve aşağıda gördüğünüz alan tanımları vardır. Diğer walkthroughs kullanılan daha büyük bir [Oteller endeksinin](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) bir alt kümesidir. Kısalık için bu hızlı başlangıçta kestik.

1. Sonraki hücrede, şemayı sağlamak için aşağıdaki örneği bir hücreye yapıştırın. 

    ```python
    index_schema = {
       "name": "hotels-quickstart",  
       "fields": [
         {"name": "HotelId", "type": "Edm.String", "key": "true", "filterable": "true"},
         {"name": "HotelName", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "true", "facetable": "false"},
         {"name": "Description", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "en.lucene"},
         {"name": "Description_fr", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "fr.lucene"},
         {"name": "Category", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Tags", "type": "Collection(Edm.String)", "searchable": "true", "filterable": "true", "sortable": "false", "facetable": "true"},
         {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Rating", "type": "Edm.Double", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Address", "type": "Edm.ComplexType", 
         "fields": [
         {"name": "StreetAddress", "type": "Edm.String", "filterable": "false", "sortable": "false", "facetable": "false", "searchable": "true"},
         {"name": "City", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "StateProvince", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "PostalCode", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Country", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"}
        ]
       }
      ]
    }
    ```

2. Başka bir hücrede, isteği formüle edin. Bu POST isteği, arama hizmetinizin dizin koleksiyonunu hedefler ve önceki hücrede sağladığınız dizin şeasını temel alan bir dizin oluşturur.

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. Her adımı çalıştırın.

   Yanıt şema JSON temsil içerir. Aşağıdaki ekran görüntüsü yanıtın yalnızca bir bölümünü gösteriyor.

    ![Dizin oluşturma isteği](media/search-get-started-python/create-index.png "Dizin oluşturma isteği")

> [!Tip]
> Dizin oluşturmayı doğrulamanın başka bir yolu da portaldaki Dizinler listesini denetlemektir.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - Belgeleri yükleyin

Belgeleri zorlamak için, dizininizin URL bitiş noktasına bir HTTP POST isteği kullanın. REST API [Belgeleri Ekle, Güncelleştir veya Sil'](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)dir. Belgeler GitHub'daki [HotelsData'dan](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON) kaynaklanır.

1. Yeni bir hücrede, dizin şemasına uygun dört belge sağlayın. Her belge için bir yükleme eylemi belirtin.

    ```python
    documents = {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": {
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
        "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": {
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

2. Başka bir hücrede, isteği formüle edin. Bu POST isteği, otellerin hızlı başlangıç dizininin doküman koleksiyonunu hedefler ve önceki adımda sağlanan belgeleri iter.

   ```python
   url = endpoint + "indexes/hotels-quickstart/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. Belgeleri arama hizmetinizdeki bir dizin eitetmek için her adımı çalıştırın. Sonuçlar aşağıdaki örneğe benzer olmalıdır. 

    ![Belgeleri dizine gönderme](media/search-get-started-python/load-index.png "Belgeleri dizine gönderme")

## <a name="3---search-an-index"></a>3 - Dizin arama

Bu adım, Arama Belgeleri REST [API'yi](https://docs.microsoft.com/rest/api/searchservice/search-documents)kullanarak bir dizini nasıl sorgulayabileceğinizi gösterir.

1. Hücrede, rasgele belgelerin sıralanmamış listesini (arama puanı = 1,0) döndürerek boş bir aramayı (search=*) çalıştıran bir sorgu ifadesi sağlayın. Varsayılan olarak, Azure Bilişsel Arama bir seferde 50 eşleşme döndürür. Yapılandırılmış olarak, bu sorgu tüm belge yapısını ve değerlerini döndürür. Sonuçlardaki tüm belgelerin sayısını almak için $count=True ekleyin.

   ```python
   searchstring = '&search=*&$count=true'
   ```

1. Yeni bir hücrede, "oteller" ve "wifi" terimlerini aramak için aşağıdaki örneği sağlayın. Arama sonuçlarına hangi alanların dahil edileleyeceğini belirtmek için $select ekleyin.

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

1. Başka bir hücrede, bir istek formüle. Bu GET isteği, otellerin hızlı başlatma dizininin doküman koleksiyonunu hedefler ve önceki adımda belirttiğiniz sorguyu bağlar.

   ```python
   url = endpoint + "indexes/hotels-quickstart/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

1. Her adımı çalıştırın. Sonuçlar aşağıdaki çıktıya benzer olmalıdır. 

    ![Dizin arama](media/search-get-started-python/search-index.png "Dizin arama")

1. Sözdizimini görmek için birkaç sorgu örneğini deneyin. Aşağıdaki `searchstring` örneklerle değiştirebilir ve ardından arama isteğini yeniden çalıştırabilirsiniz. 

   Filtre uygulayın: 

   ```python
   searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description,Rating'
   ```

   İlk iki sonucu alın:

   ```python
   searchstring = '&search=boutique&$top=2&$select=HotelId,HotelName,Description,Category'
   ```

    Belirli bir alana göre sipariş:

   ```python
   searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince, Tags'
   ```

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Basitleştirme olarak, bu hızlı başlangıç Oteller dizininin kısaltılmış bir sürümünü kullanır. Daha ilginç sorguları denemek için tam sürümünü oluşturabilirsiniz. Tam sürümü ve 50 belgenin tümünü almak **için,** yerleşik örnek veri kaynaklarından *otel örneğini* seçerek Alma veri sihirbazını çalıştırın.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure portalında dizin oluşturma](search-get-started-portal.md)
