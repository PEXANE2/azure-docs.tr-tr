---
title: "Hızlı başlangıç: JavaScript 'te arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu JavaScript hızlı başlangıçta, JavaScript kullanarak dizin oluşturmayı, verileri yüklemeyi ve Azure Bilişsel Arama sorguları çalıştırmayı öğrenin
author: dereklegenzoff
manager: luisca
ms.author: delegenz
ms.devlang: javascript
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 02/02/2021
ms.custom: devx-track-js
ms.openlocfilehash: 7d31201f9d1a4519538aba3ac57ddfd340d936d1
ms.sourcegitcommit: b85ce02785edc13d7fb8eba29ea8027e614c52a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99509410"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-using-the-javascript-sdk"></a>Hızlı başlangıç: JavaScript SDK 'sını kullanarak Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [JavaScript](search-get-started-javascript.md)
> * [C#](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](./search-get-started-powershell.md)
> * [Python](search-get-started-python.md)
> * [REST](search-get-started-rest.md)


JavaScript 'te bir arama dizini oluşturan, yükleyen ve sorgulayan bir Node.js uygulaması oluşturmak için [Azure bilişsel arama JavaScript/Typscript SDK 'sını](/javascript/api/overview/azure/search-documents-readme) kullanın.

Bu makalede, uygulamanın adım adım nasıl oluşturulacağı gösterilir. Alternatif olarak, [kaynak kodu ve verileri indirebilir](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) ve uygulamayı komut satırından çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, aşağıdaki araçlara ve hizmetlere sahip olursunuz:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

+ Bir Azure Bilişsel Arama hizmeti. [Bir hizmet oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

+ [Node.js](https://nodejs.org) ve [NPM](https://www.npmjs.com)

+ [Visual Studio Code](https://code.visualstudio.com) veya başka bir IDE


## <a name="set-up-your-project"></a>Projenizi ayarlama

' İ, arama hizmetinizin uç noktasını ve anahtarını alarak başlatın. Ardından aşağıda özetlenen NPM ile yeni bir proje oluşturun.

<a name="get-service-info"></a>

### <a name="copy-a-key-and-endpoint"></a>Anahtar ve uç nokta kopyalama

Hizmete yapılan çağrılar, her istekte bir URL uç noktası ve erişim anahtarı gerektirir. İlk adım olarak, projenize eklenecek API anahtarını ve URL 'YI bulun. Daha sonraki bir adımda istemciyi oluştururken her iki değeri de belirtirsiniz.

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

2. **Ayarlar**  >  **anahtarlarında**, hizmet üzerinde tam haklar için bir yönetici anahtarı alın, nesne oluşturuyorsanız veya silerseniz gereklidir. İki adet değiştirilebilir birincil ve ikincil anahtar vardır. Bunlardan birini kullanabilirsiniz.

   ![HTTP uç noktası ve erişim anahtarı al](media/search-get-started-rest/get-url-key.png "HTTP uç noktası ve erişim anahtarı al")

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

### <a name="create-a-new-npm-project"></a>Yeni NPM projesi oluşturma

VS Code ve [Tümleşik terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal) veya Node.js komut istemi gibi başka bir terminali açarak başlayın.

1. Şu adı vererek bir geliştirme dizini oluşturun `quickstart` :

    ```cmd
    mkdir quickstart
    cd quickstart
    ```

2. Şunu çalıştırarak NPM ile boş bir proje başlatın 

    ```cmd
    npm init
    ```
     Lisans dışında, "MıT" olarak ayarlamanız gereken varsayılan değerleri kabul edin. 

3. `@azure/search-documents` [Azure bilişsel arama için JavaScript/TYPSCRIPT SDK](/javascript/api/overview/azure/search-documents-readme)'yı yükler.

    ```cmd
    npm install @azure/search-documents
    ```

4. `dotenv`Hizmeti adı ve API anahtarımız gibi ortam değişkenlerini içeri aktarmak için kullanılan uygulamasını yükler.
    ```cmd
    npm install dotenv
    ```

5. **package.js** dosyanın aşağıdaki JSON 'a benzer göründüğünü denetleyerek projeleri ve bağımlılıklarını yapılandırdığınızdan emin olun:

    ```json
    {
      "name": "quickstart",
      "version": "1.0.0",
      "description": "Azure Cognitive Search Quickstart",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [
        "Azure",
        "Search"
      ],
      "author": "Your Name",
      "license": "MIT",
      "dependencies": {
        "@azure/search-documents": "^11.0.3",
        "dotenv": "^8.2.0"
      }
    }
    ```

6. Arama hizmeti parametrelerinizi tutmak için bir dosya **. env** oluşturun:

    ```
    SEARCH_API_KEY=<search-admin-key>
    SEARCH_API_ENDPOINT=https://<search-service-name>.search.windows.net
    ```

Değeri, `<search-service-name>` arama hizmetinizin adıyla değiştirin. `<search-admin-key>`Daha önce kaydettiğiniz anahtar değeriyle değiştirin. 

### <a name="create-indexjs-file"></a>index.js dosyası oluştur

Ardından, kodumuzu barındıracak ana dosya olan bir **index.js** dosyası oluşturacağız.

Bu dosyanın en üstünde, kitaplığı içeri aktardık `@azure/search-documents` :

```javascript
const { SearchIndexClient, SearchClient, AzureKeyCredential, odata } = require("@azure/search-documents");
```

Sonra, `dotenv` paketin **. env** dosyasındaki parametrelerde şu şekilde okumasını gerektirmemiz gerekir:

```javascript
// Load the .env file if it exists
require("dotenv").config();

// Getting endpoint and apiKey from .env file
const endpoint = process.env.SEARCH_API_ENDPOINT || "";
const apiKey = process.env.SEARCH_API_KEY || "";
```

İçeri aktarmalarımızın ve ortam değişkenlerinin yerine ana işlevi tanımlamaya hazırız.

SDK 'daki işlevlerin çoğu zaman uyumsuzdur, bu nedenle ana işlevimizi sunuyoruz `async` . Ayrıca `main().catch()` , karşılaşılan hataları yakalamak ve günlüğe kaydetmek için Main işlevinin altına da dahil ediyoruz:

```javascript
async function main() {
    console.log(`Running Azure Cognitive Search Javascript quickstart...`);
    if (!endpoint || !apiKey) {
        console.log("Make sure to set valid values for endpoint and apiKey with proper authorization.");
        return;
    }

    // remaining quickstart code will go here
}

main().catch((err) => {
    console.error("The sample encountered an error:", err);
});
```

Bu şekilde, bir dizin oluşturmaya hazırız.

## <a name="1---create-index"></a>1-Dizin oluşturma 

Üzerinde bir dosya **hotels_quickstart_index.js** oluşturun.  Bu dosya, Azure Bilişsel Arama 'nin bir sonraki adımda yüklediğiniz belgelerle nasıl çalıştığını tanımlar. Her bir alan ile tanımlanır `name` ve belirtilmiş olur `type` . Her alan Ayrıca, Azure Bilişsel Arama alan üzerinde arama, filtreleme, sıralama ve model kullanıp kullanamayacağını belirten bir dizi dizin özniteliklerine sahiptir. Alanların çoğu basit veri türleridir, ancak bazıları `AddressType` dizininiz içinde zengin veri yapıları oluşturmanıza imkan tanıyan karmaşık türlerdir.  [Desteklenen veri türleri](/rest/api/searchservice/supported-data-types) ve Dizin [oluşturma (REST)](/rest/api/searchservice/create-index)bölümünde açıklanan dizin öznitelikleri hakkında daha fazla bilgi edinebilirsiniz. 

**hotels_quickstart_index.js** için aşağıdakini ekleyin veya [dosyayı indirin](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/v11/hotels_quickstart_index.json). 

```json
{
    "name": "hotels-quickstart",
    "fields": [
        {
            "name": "HotelId",
            "type": "Edm.String",
            "key": true,
            "filterable": true
        },
        {
            "name": "HotelName",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "sortable": true,
            "facetable": false
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "sortable": false,
            "facetable": false,
            "analyzer": "en.lucene"
        },
        {
            "name": "Description_fr",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "sortable": false,
            "facetable": false,
            "analyzer": "fr.lucene"
        },
        {
            "name": "Category",
            "type": "Edm.String",
            "searchable": true,
            "filterable": true,
            "sortable": true,
            "facetable": true
        },
        {
            "name": "Tags",
            "type": "Collection(Edm.String)",
            "searchable": true,
            "filterable": true,
            "sortable": false,
            "facetable": true
        },
        {
            "name": "ParkingIncluded",
            "type": "Edm.Boolean",
            "filterable": true,
            "sortable": true,
            "facetable": true
        },
        {
            "name": "LastRenovationDate",
            "type": "Edm.DateTimeOffset",
            "filterable": true,
            "sortable": true,
            "facetable": true
        },
        {
            "name": "Rating",
            "type": "Edm.Double",
            "filterable": true,
            "sortable": true,
            "facetable": true
        },
        {
            "name": "Address",
            "type": "Edm.ComplexType",
            "fields": [
                {
                    "name": "StreetAddress",
                    "type": "Edm.String",
                    "filterable": false,
                    "sortable": false,
                    "facetable": false,
                    "searchable": true
                },
                {
                    "name": "City",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
                {
                    "name": "StateProvince",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
                {
                    "name": "PostalCode",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
                {
                    "name": "Country",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                }
            ]
        }
    ],
    "suggesters": [
        {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields": [
                "HotelName"
            ]
        }
    ]
}
```

Dizin tanımımız ile, ana işlevin Dizin tanımına erişebilmeleri için **index.js** en üstünde **hotels_quickstart_index.js** içeri aktarmak istiyoruz.

```javascript
const indexDefinition = require('./hotels_quickstart_index.json');
```

Ana işlevin içinde, `SearchIndexClient` Azure bilişsel arama için dizin oluşturmak ve yönetmek üzere kullanılan bir oluşturur. 

```javascript
const indexClient = new SearchIndexClient(endpoint, new AzureKeyCredential(apiKey));
```

Daha sonra, zaten varsa dizini silmek istiyoruz. Bu, test/tanıtım kodu için yaygın bir uygulamadır.

Bunu, dizini silmeye çalışan basit bir işlev tanımlayarak yapacağız.

```javascript
async function deleteIndexIfExists(indexClient, indexName) {
    try {
        await indexClient.deleteIndex(indexName);
        console.log('Deleting index...');
    } catch {
        console.log('Index does not exist yet.');
    }
}
```

İşlevi çalıştırmak için Dizin tanımından dizin adını ayıklayıp `indexName` ile birlikte `indexClient` işlevine geçirin `deleteIndexIfExists()` .

```javascript
const indexName = indexDefinition["name"];

console.log('Checking if index exists...');
await deleteIndexIfExists(indexClient, indexName);
```

Bundan sonra, yöntemi ile dizini oluşturmaya hazırız `createIndex()` .

```javascript
console.log('Creating index...');
let index = await indexClient.createIndex(indexDefinition);

console.log(`Index named ${index.name} has been created.`);
```

### <a name="run-the-sample"></a>Örneği çalıştırma

Bu noktada, örneği çalıştırmaya hazırsınız demektir. Aşağıdaki komutu çalıştırmak için bir Terminal penceresi kullanın:

```cmd
node index.js
```

[Kaynak kodu indirdiyseniz](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) ve gerekli paketleri henüz yüklemediyseniz, `npm install` önce çalıştırın.

Program tarafından gerçekleştirilen eylemleri açıklayan bir ileti serisi görmeniz gerekir. 

Azure portal arama hizmetinize **Genel Bakış ' ı** açın. **Dizinler** sekmesini seçin. Aşağıdakine benzer bir şey görmeniz gerekir:

:::image type="content" source="media/search-get-started-javascript/create-index-no-data.png" alt-text="Azure portal, arama hizmetine genel bakış, dizinler sekmesinin ekran görüntüsü" border="false":::

Sonraki adımda, dizine veri ekleyeceksiniz. 

## <a name="2---load-documents"></a>2-belge yükleme 


Azure Bilişsel Arama 'de belgeler, sorguların dizin oluşturma ve çıkışlara yönelik giriş olan veri yapılarıdır. Bu tür verileri dizine gönderebilir veya bir [Dizin Oluşturucu](search-indexer-overview.md)kullanabilirsiniz. Bu durumda, belgeleri dizine iteceğiz program aracılığıyla.

Belge girişleri bir veritabanındaki satırlar, blob depolamada Bloblar veya bu örnekte olduğu gibi, diskteki JSON belgeleri olabilir. [Üzerindehotels.js](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/v11/hotels.json) indirebilir veya kendi **hotels.js** dosyanızı aşağıdaki içerikle oluşturabilirsiniz:

```json
{
    "value": [
        {
            "HotelId": "1",
            "HotelName": "Secret Point Motel",
            "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
            "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
            "Category": "Boutique",
            "Tags": ["pool", "air conditioning", "concierge"],
            "ParkingIncluded": false,
            "LastRenovationDate": "1970-01-18T00:00:00Z",
            "Rating": 3.6,
            "Address": {
                "StreetAddress": "677 5th Ave",
                "City": "New York",
                "StateProvince": "NY",
                "PostalCode": "10022"
            }
        },
        {
            "HotelId": "2",
            "HotelName": "Twin Dome Motel",
            "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
            "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
            "Category": "Boutique",
            "Tags": ["pool", "free wifi", "concierge"],
            "ParkingIncluded": "false",
            "LastRenovationDate": "1979-02-18T00:00:00Z",
            "Rating": 3.6,
            "Address": {
                "StreetAddress": "140 University Town Center Dr",
                "City": "Sarasota",
                "StateProvince": "FL",
                "PostalCode": "34243"
            }
        },
        {
            "HotelId": "3",
            "HotelName": "Triple Landscape Hotel",
            "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
            "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
            "Category": "Resort and Spa",
            "Tags": ["air conditioning", "bar", "continental breakfast"],
            "ParkingIncluded": "true",
            "LastRenovationDate": "2015-09-20T00:00:00Z",
            "Rating": 4.8,
            "Address": {
                "StreetAddress": "3393 Peachtree Rd",
                "City": "Atlanta",
                "StateProvince": "GA",
                "PostalCode": "30326"
            }
        },
        {
            "HotelId": "4",
            "HotelName": "Sublime Cliff Hotel",
            "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
            "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
            "Category": "Boutique",
            "Tags": ["concierge", "view", "24-hour front desk service"],
            "ParkingIncluded": true,
            "LastRenovationDate": "1960-02-06T00:00:00Z",
            "Rating": 4.6,
            "Address": {
                "StreetAddress": "7400 San Pedro Ave",
                "City": "San Antonio",
                "StateProvince": "TX",
                "PostalCode": "78216"
            }
        }
    ]
}
```

Indexdefinition ile yaptığımız gibi, `hotels.json` verilere ana işlevimizde erişilebilmesi için **index.js** en üstünde içeri aktarmanız gerekir.

```javascript
const hotelData = require('./hotels.json');
```


Arama dizinine veri dizini oluşturmak için şimdi bir oluşturmanız gerekiyor `SearchClient` . `SearchIndexClient`Bir dizin oluşturmak ve yönetmek için kullanıldığında, `SearchClient` belgeleri karşıya yüklemek ve dizini sorgulamak için kullanılır.

Oluşturmak için iki yol vardır `SearchClient` . İlk seçenek sıfırdan bir oluşturmak için `SearchClient` :

```javascript
 const searchClient = new SearchClient(endpoint, indexName, new AzureKeyCredential(apiKey));
```

Alternatif olarak, öğesini `getSearchClient()` `SearchIndexClient` oluşturmak için yöntemini kullanabilirsiniz `SearchClient` :

```javascript
const searchClient = indexClient.getSearchClient(indexName);
```

Artık istemci tanımlandığından, belgeleri arama dizinine yükleyin. Bu durumda, `mergeOrUploadDocuments()` aynı anahtara sahip bir belge zaten varsa, belgeleri karşıya yükleyecek veya mevcut bir belgeyle birleştirilecek olan yöntemini kullanırız.

```javascript
console.log('Uploading documents...');
let indexDocumentsResult = await searchClient.mergeOrUploadDocuments(hotelData['value']);

console.log(`Index operations succeeded: ${JSON.stringify(indexDocumentsResult.results[0].succeeded)}`);
```

Programını ile yeniden çalıştırın `node index.js` . Adım 1 ' de gördüğenlerden biraz farklı bir ileti kümesi görmeniz gerekir. Bu kez *, Dizin mevcut* olur ve uygulama yeni dizin oluşturmadan ve verileri kendisine göndermeksizin silme hakkında bir ileti görmeniz gerekir. 

Sonraki adımda sorguları çalıştırmadan önce, programın bir saniye beklemesi için bir işlev tanımlayın. Bu işlem, dizin oluşturmanın tamamlandığından emin olmak için yalnızca test/demo amaçları için yapılır ve sorgularımızda, belgelerin dizinde kullanılabilmesini sağlar.

```javascript
function sleep(ms) {
    var d = new Date();
    var d2 = null;
    do {
        d2 = new Date();
    } while (d2 - d < ms);
}
```

Programın bir saniye beklemesi için `sleep` aşağıdaki gibi işlevi çağırın:

```javascript
sleep(1000);
```

## <a name="3---search-an-index"></a>3 - Dizin arama

Oluşturulan bir dizin ve belge karşıya yüklendiğinde, dizine sorgu göndermeye hazırsınız demektir. Bu bölümde, kullanabileceğiniz farklı sorgu işlevleri parçalarını göstermek için arama dizinine beş farklı sorgu göndereceğiz.

Sorgular, `sendQueries()` ana işlevde aşağıdaki gibi çağıracağımız bir işlevde yazılır:

```javascript
await sendQueries(searchClient);
```

Sorguları, yöntemi kullanılarak gönderilir `search()` `searchClient` . İlk parametre arama metni, ikinci parametre ise ek arama seçeneklerdir.

İlk sorgu `*` , her şeyi aramaya eşdeğer olan ve dizindeki alanları üç kez seçen arama yapar. Yalnızca ihtiyacınız olan alanlara en iyi uygulamadır `select` . Bu, gereksiz verileri geri çekmek, sorgularınıza gecikme süresi ekleyebildiğinden.

`searchOptions`Bu sorgu için de, `includeTotalCount` `true` bulunan eşleşen sonuçların sayısını döndüren olarak ayarlanır.

```javascript
async function sendQueries(searchClient) {
    console.log('Query #1 - search everything:');
    let searchOptions = {
        includeTotalCount: true,
        select: ["HotelId", "HotelName", "Rating"]
    };

    let searchResults = await searchClient.search("*", searchOptions);
    for await (const result of searchResults.results) {
        console.log(`${JSON.stringify(result.document)}`);
    }
    console.log(`Result count: ${searchResults.count}`);

    // remaining queries go here
}
```

Aşağıda özetlenen kalan sorgular da `sendQueries()` işlevine eklenmelidir. Bunlar, okunabilir olmaları için burada ayrılır.

Sonraki sorguda arama terimini belirttik `"wifi"` ve yalnızca durumun eşit olduğu sonuçları döndürmek için bir filtre de içerir `'FL'` . Sonuçlar Ayrıca otel tarafından sıralanır `Rating` .

```javascript
console.log('Query #2 - Search with filter, orderBy, and select:');
let state = 'FL';
searchOptions = {
    filter: odata`Address/StateProvince eq ${state}`,
    orderBy: ["Rating desc"],
    select: ["HotelId", "HotelName", "Rating"]
};

searchResults = await searchClient.search("wifi", searchOptions);
for await (const result of searchResults.results) {
    console.log(`${JSON.stringify(result.document)}`);
}
```

Sonra, arama parametresi kullanılarak tek bir aranabilir alanla sınırlandırılmıştır `searchFields` . Bu, yalnızca belirli alanlardaki eşleştirmelerle ilgilendiğinizi bildiğiniz durumlarda sorgunuzu daha verimli hale getirmek için harika bir seçenektir. 

```javascript
console.log('Query #3 - Limit searchFields:');
searchOptions = {
    select: ["HotelId", "HotelName", "Rating"],
    searchFields: ["HotelName"]
};

searchResults = await searchClient.search("sublime cliff", searchOptions);
for await (const result of searchResults.results) {
    console.log(`${JSON.stringify(result.document)}`);
}
console.log();
```

Bir sorguya dahil etmek için başka bir ortak seçenek de vardır `facets` . Modeller, kullanıcıların filtreleyebilecekleri değerleri bilmesini kolaylaştırmak için Kullanıcı arabiriminizdeki filtreleri oluşturmanızı sağlar.

```javascript
console.log('Query #4 - Use facets:');
searchOptions = {
    facets: ["Category"],
    select: ["HotelId", "HotelName", "Rating"],
    searchFields: ["HotelName"]
};

searchResults = await searchClient.search("*", searchOptions);
for await (const result of searchResults.results) {
    console.log(`${JSON.stringify(result.document)}`);
}
```

Son sorgu `getDocument()` , yöntemini kullanır `searchClient` . Bu, bir belgeyi anahtarına göre etkin bir şekilde almanızı sağlar. 

```javascript
console.log('Query #5 - Lookup document:');
let documentResult = await searchClient.getDocument(key='3')
console.log(`HotelId: ${documentResult.HotelId}; HotelName: ${documentResult.HotelName}`)
```

### <a name="run-the-sample"></a>Örneği çalıştırma

Programı ile çalıştırın `node index.js` . Şimdi, önceki adımlara ek olarak sorgular gönderilir ve sonuçlar konsola yazılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu JavaScript hızlı başlangıçta, dizin oluşturmak, belgeler ile yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinden çalıştık. 

Azure Bilişsel Arama 'de zaten bir arka plana sahipseniz, bu örneği, öneri araçları (tür kullanımı veya otomatik tamamlama sorguları), filtreler ve çok yönlü gezinme için bir Springboard olarak kullanabilirsiniz. Azure Bilişsel Arama 'de yeni başladıysanız, oluşturabileceğiniz özellikler hakkında daha fazla öğreticiyi anlamak için başka öğreticiler denemeyi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. 

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için tepki temelli bir ön uç oluşturun](https://github.com/dereklegenzoff/azure-search-react-template)