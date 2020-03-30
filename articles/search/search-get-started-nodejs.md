---
title: "Quickstart: REST API'lerini kullanarak Düğüm.js'de bir arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu Düğüm.js hızlı başlangıcında, JavaScript ve REST API'lerini kullanarak Azure Bilişsel Arama'da nasıl dizin oluşturup veri yükleyip sorguları çalıştıracağımız hakkında bilgi edinin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.devlang: nodejs
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 02/25/2020
ms.openlocfilehash: cbef6029b93f134f95ee54aa87ce0dd65bcdf50d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77624002"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-nodejs-using-rest-apis"></a>Quickstart: REST API'lerini kullanarak Düğüm.js'de Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [Javascript](search-get-started-nodejs.md)
> * [C #](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [Powershell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)

Azure Bilişsel Arama dizini oluşturan, yükleyen ve sorgulayan bir Düğüm.js uygulaması oluşturun. Bu makalede, uygulama adım adım nasıl oluşturulacak gösteriş. Alternatif olarak, [kaynak kodu ve verileri karşıdan yükleyebilir ve](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/) uygulamayı komut satırından çalıştırabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı oluşturmak ve test etmek için aşağıdaki yazılım ve hizmetleri kullandık:

+ [Node.js](https://nodejs.org)

+ [NPM](https://www.npmjs.com) Node.js tarafından yüklenmelidir

+ Örnek dizin yapısı ve eşleşen belgeler bu makalede veya [repo'nun **hızlı başlangıç** dizininden](https://github.com/Azure-Samples/azure-search-javascript-samples/)

+ [Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

Önerilen:

* [Visual Studio Code](https://code.visualstudio.com)

* VSCode için [daha güzel](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) ve [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) uzantıları.

<a name="get-service-info"></a>

## <a name="get-keys-and-urls"></a>Anahtarları ve URL'leri alın

Hizmete yapılan aramalar için her istekte bir URL bitiş noktası ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda arama hizmetinizin adını alın. Bitiş noktası URL'sini inceleyerek hizmet adınızı onaylayabilirsiniz. Bitiş noktası URL'niz olsaydı, `https://mydemo.search.windows.net`hizmet `mydemo`adınız .

2. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

    Sorgu anahtarını da alın. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

![Hizmet adını ve yöneticive sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

Tüm istekler, hizmetinize gönderilen her isteğin üstbilgisinde bir api anahtarı gerektirir. Geçerli bir anahtar, istek bazında, isteği gönderen uygulama ile onu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Bir Powershell konsolu veya Node.js yüklediğiniz başka bir ortamı açarak başlayın.

1. Bir geliştirme dizini oluşturun, `quickstart` ona adını vererek:

    ```powershell
    mkdir quickstart
    cd quickstart
    ```

2. Çalıştırarak NPM ile boş `npm init`bir proje başlatma. "MIT" olarak ayarlamanız gereken Lisans dışında varsayılan değerleri kabul edin. 

1. Koda bağlı olacak paketleri ekleyin ve geliştirmeye yardımcı olun:

    ```powershell
    npm install nconf node-fetch
    npm install --save-dev eslint eslint-config-prettier eslint-config-airbnb-base eslint-plugin-import prettier
    ```

4. **Package.json** dosyanızın aşağıdakilere benzer görünüp görünmediğini kontrol ederek projeleri ve bağımlılıklarını yapılandırdığınızı doğrulayın:

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
        "Azure_Search"
      ],
      "author": "Your Name",
      "license": "MIT",
      "dependencies": {
        "nconf": "^0.10.0",
        "node-fetch": "^2.6.0"
      },
      "devDependencies": {
        "eslint": "^6.1.0",
        "eslint-config-airbnb-base": "^13.2.0",
        "eslint-config-prettier": "^6.0.0",
        "eslint-plugin-import": "^2.18.2",
        "prettier": "^1.18.2"
      }
    }
    ```

5. Arama hizmeti verilerinizi tutmak için **azure_search_config** bir dosya oluşturun:

    ```json
    {
        "serviceName" : "[SEARCH_SERVICE_NAME]",
        "adminKey" : "[ADMIN_KEY]",
        "queryKey" : "[QUERY_KEY]",
        "indexName" : "hotels-quickstart"
    }
    ```

`[SERVICE_NAME]` Değeri arama hizmetinizin adı ile değiştirin. Daha `[ADMIN_KEY]` `[QUERY_KEY]` önce kaydettiğiniz anahtar değerleri değiştirin. 

## <a name="1---create-index"></a>1 - Dizin oluşturma 

Bir dosya **hotels_quickstart_index.json**oluşturun.  Bu dosya, Azure Bilişsel Arama'nın bir sonraki adımda yüklediğiniz belgelerle nasıl çalıştığını tanımlar. Her alan a `name` ile tanımlanır ve `type`belirtilmiş bir . Her alanın ayrıca Azure Bilişsel Arama'nın alan üzerinde arama yapıp yapamayacağını, filtreleyip sıralayamadığını ve yönünü belirleyip belirleyemeyeceğini belirten bir dizi dizin özniteliği de vardır. Alanların çoğu basit veri türleridir, ancak `AddressType` bazıları, dizininizde zengin veri yapıları oluşturmanıza olanak tanıyan karmaşık türlerdir.  [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) ve [dizin öznitelikleri](https://docs.microsoft.com/azure/search/search-what-is-an-index#index-attributes)hakkında daha fazla bilgi edinebilirsiniz. 

**hotels_quickstart_index.json'a** aşağıdakileri ekleyin veya [dosyayı indirin.](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/hotels_quickstart_index.json) 

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
    

Belirli bir senaryonun özelliklerini genel olarak uygulanabilir olan koddan ayırmak iyi bir uygulamadır. `AzureSearchClient` **AzureSearchClient.js** dosyasında tanımlanan sınıf, istek URL'lerinin nasıl oluşturacağını, API'yi getir'i kullanarak istekte bulunacağını ve yanıtın durum koduna nasıl tepki vereceğini bilir.

Düğüm **getir** paketini içe aktararak ve basit bir sınıf oluşturarak **AzureSearchClient.js** üzerinde çalışmaya başlayın. `AzureSearchClient` Sınıfın değiştirilebilir bölümlerini, çeşitli yapılandırma değerlerini oluşturucuya geçirerek yalıtın:

```javascript
const fetch = require('node-fetch');

class AzureSearchClient {
    constructor(searchServiceName, adminKey, queryKey, indexName) {
        this.searchServiceName = searchServiceName;
        this.adminKey = adminKey;
        // The query key is used for read-only requests and so can be distributed with less risk of abuse.
        this.queryKey = queryKey;
        this.indexName = indexName;
        this.apiVersion = '2019-05-06';
    }

    // All methods go inside class body here!
}

module.exports = AzureSearchClient;
```

Sınıfın ilk sorumluluğu, çeşitli istekleri göndermek için URL'lerin nasıl yapılacağını bilmektir. Bu URL'leri, sınıf oluşturucuya geçen yapılandırma verilerini kullanan örnek yöntemleriyle oluşturun. Oluşturdukları URL'nin bir API sürümüne özgü olduğuna ve bu sürümü belirten `2019-05-06`bir bağımsız değişkene sahip olması gerektiğine dikkat edin (bu uygulamada). 

Bu yöntemlerden ilki, dizinin URL'sini döndürür. Sınıf gövdesi içinde aşağıdaki yöntemi ekleyin:

```javascript
getIndexUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}?api-version=${this.apiVersion}`; }

```

Bir sonraki `AzureSearchClient` sorumluluk, Getir API'si ile eşzamanlı bir istekte bulunmaktır. Eşiz statik yöntem, `request` bir URL, HTTP yöntemini belirten bir dize ("GET", "PUT", "POST", "DELETE"), istekte kullanılacak anahtar ve isteğe bağlı JSON nesnesi alır. Değişken `headers` eşler `queryKey` (ister yönetici anahtarı veya salt okunur sorgu anahtarı) "api-key" HTTP istek üstbilgisine. İstek seçenekleri her `method` zaman kullanılacak ve `headers`. Değilse, `bodyJson` HTTP isteğinin gövdesi dize gösterimi `bodyJson`için ayarlanır. `null` Yöntem, `request` HTTP isteğini yürütmek için API'nin Söz'ü döndürür.

```javascript
static async request(url, method, apiKey, bodyJson = null) {
    // Uncomment the following for request details:
    /*
    console.log(`\n${method} ${url}`);
    console.log(`\nKey ${apiKey}`);
    if (bodyJson !== null) {
        console.log(`\ncontent: ${JSON.stringify(bodyJson, null, 4)}`);
    }
    */

    const headers = {
        'content-type' : 'application/json',
        'api-key' : apiKey
    };
    const init = bodyJson === null ?
        { 
            method, 
            headers
        }
        : 
        {
            method, 
            headers,
            body : JSON.stringify(bodyJson)
        };
    return fetch(url, init);
}
```

HTTP isteği başarılı değilse demo amaçlı bir istisna atmasanız bile bir istisna hazırlanın. Gerçek bir uygulamada, büyük olasılıkla arama hizmeti isteği bazı günlük `response` ve HTTP durum kodu tanı yapmak istiyorsunuz. 
    
```javascript
static throwOnHttpError(response) {
    const statusCode = response.status;
    if (statusCode >= 300){
        console.log(`Request failed: ${JSON.stringify(response, null, 4)}`);
        throw new Error(`Failure in request. HTTP Status was ${statusCode}`);
    }
}
```

Son olarak, Azure Bilişsel Arama dizinini algılama, silme ve oluşturma yöntemlerini ekleyin. Bu yöntemlerin tümü aynı yapıya sahiptir:

* İsteğin yapılacağı bitiş noktasını alın.
* İsteği uygun bitiş noktası, HTTP fiili, API tuşu ve uygunsa Bir JSON gövdesi ile oluşturun. `indexExistsAsync()`ve `deleteIndexAsync()` bir JSON vücut yok, ama `createIndexAsync(definition)` yok.
* `await`isteğe yanıt.  
* Yanıtın durum koduna göre hareket edin.
* Uygun bir değere (Boolean `this`veya sorgu sonuçları) bir Söz döndürün. 

```javascript
async indexExistsAsync() { 
    console.log("\n Checking if index exists...");
    const endpoint = this.getIndexUrl();
    const response = await AzureSearchClient.request(endpoint, "GET", this.adminKey);
    // Success has a few likely status codes: 200 or 204 (No Content), but accept all in 200 range...
    const exists = response.status >= 200 && response.status < 300;
    return exists;
}

async deleteIndexAsync() {
    console.log("\n Deleting existing index...");
    const endpoint = this.getIndexUrl();
    const response = await AzureSearchClient.request(endpoint, "DELETE", this.adminKey);
    AzureSearchClient.throwOnHttpError(response);
    return this;
}

async createIndexAsync(definition) {
    console.log("\n Creating index...");
    const endpoint = this.getIndexUrl();
    const response = await AzureSearchClient.request(endpoint, "PUT", this.adminKey, definition);
    AzureSearchClient.throwOnHttpError(response);
    return this;
}
```

Yöntemlerinizin sınıfın içinde olduğunu ve sınıfı dışa aktardığınızı doğrulayın. **AzureSearchClient.js'nin** en dışkapsamı aşağıdaki olmalıdır:

```javascript
const fetch = require('node-fetch');

class AzureSearchClient {
    // ... code here ...
}

module.exports = AzureSearchClient;
```

Nesne yönelimli bir sınıf, yeniden kullanılabilir **AzureSearchClient.js** modülü için iyi bir seçimdi, ancak **index.js**adlı bir dosyaya koymanız gereken ana program için gerekli değildir. 

**Index.js** oluşturun ve getirerek başlayın:

* JSON, ortam değişkenleri veya komut satırı bağımsız değişkenleriyle yapılandırmayı belirtmeniz için esneklik sağlayan **nconf** paketi.
* **hotels_quickstart_index.json** dosyasındaki veriler.
* `AzureSearchClient` modülü.

```javascript
const nconf = require('nconf');

const indexDefinition = require('./hotels_quickstart_index.json');
const AzureSearchClient = require('./AzureSearchClient.js');
```

[ **nconf** paketi,](https://github.com/indexzero/nconf) yapılandırma verilerini ortam değişkenleri veya komut satırı gibi çeşitli biçimlerde belirtmenize olanak tanır. Bu örnek, **dosyayı azure_search_config.json** okumak ve dosyanın içeriğini sözlük olarak döndürmek için temel bir şekilde **nconf** kullanır. **nconf'ın** `get(key)` işlevini kullanarak, yapılandırma bilgilerinin düzgün şekilde özelleştirildiğini hızlı bir şekilde kontrol edebilirsiniz. Son olarak, işlev yapılandırmayı döndürür:

```javascript
function getAzureConfiguration() {
    const config = nconf.file({ file: 'azure_search_config.json' });
    if (config.get('serviceName') === '[SEARCH_SERVICE_NAME]' ) {
        throw new Error("You have not set the values in your azure_search_config.json file. Change them to match your search service's values.");
    }
    return config;
}
```

İşlev, `sleep` belirli `Promise` bir süre sonra çözen bir işlev oluşturur. Bu işlevi kullanmak, eşzamanlı dizin işlemlerinin tamamlanmasını ve kullanılabilir hale gelmesini beklerken uygulamanın duraklatmasını sağlar. Böyle bir gecikme eklemek genellikle yalnızca demolarda, testlerde ve örnek uygulamalarda gereklidir.

```javascript
function sleep(ms) {
    return(
        new Promise(function(resolve, reject) {
            setTimeout(function() { resolve(); }, ms);
        })
    );
}
```

Son olarak, ana asynchronous `run` işlevini belirtin ve çağırın. Bu işlev, s çözmek `Promise`için gerekli olarak bekleyen, sırayla diğer işlevleri çağırır.

* Yapılandırmayı daha `getAzureConfiguration()` önce yazdığınız yapılandırmayla alma
* Yapılandırmanızdan `AzureSearchClient` değerleri aktaran yeni bir örnek oluşturma
* Dizinin var olup olmadığını kontrol edin ve varsa silmek
* `indexDefinition` **hotels_quickstart_index.json'dan** yükleneni kullanarak bir dizin oluşturma

```javascript
const run = async () => {
    try {
        const cfg = getAzureConfiguration();
        const client = new AzureSearchClient(cfg.get("serviceName"), cfg.get("adminKey"), cfg.get("queryKey"), cfg.get("indexName));
        
        const exists = await client.indexExistsAsync();
        await exists ? client.deleteIndexAsync() : Promise.resolve();
        // Deleting index can take a few seconds
        await sleep(2000);
        await client.createIndexAsync(indexDefinition);
    } catch (x) {
        console.log(x);
    }
}

run();
```

Son çağrıyı `run()`unutma! Bir sonraki adımda çalıştırdığınızda `node index.js` programınızın giriş noktasıdır.

Buna `AzureSearchClient.indexExistsAsync()` dikkat `AzureSearchClient.deleteIndexAsync()` edin ve parametreleri almayın. Bu işlevler `AzureSearchClient.request()` `bodyJson` hiçbir bağımsız değişken ile çağırır. Içinde `AzureSearchClient.request()`, `bodyJson === null` `true`çünkü `init` , yapı sadece HTTP fiil ("GET" `indexExistsAsync()` için ve `deleteIndexAsync()`"DELETE" için) ve üstbilgileri, istek anahtarı nı belirtmek olarak ayarlanır.  

Buna karşılık, `AzureSearchClient.createIndexAsync(indexDefinition)` _yöntem_ bir parametre alır. İşlev, `run` dosya nın içeriğini **hotels_quickstart_index.json'ı** yönteme `AzureSearchClient.createIndexAsync(indexDefinition)` geçirir. `index.js` Yöntem `createIndexAsync()` bu tanımı `AzureSearchClient.request()`. Şimdi `AzureSearchClient.request()`olduğu `bodyJson === null` `false`için `init` yapı yalnızca HTTP fiilini ("PUT") ve üstbilgileri içermez, aynı zamanda dizin tanımı verilerini ayarlar. `body`

### <a name="prepare-and-run-the-sample"></a>Örneği hazırlama ve çalıştırma

Aşağıdaki komutlar için bir terminal penceresi kullanın.

1. **package.json** dosyasını ve kodunuzun geri kalanını içeren klasöre gidin.
1. Örnek için paketleri `npm install`' ile yükleyin.  Bu komut, kodun bağlı olduğu paketleri karşıdan yükler.
1. Programınızı ' `node index.js`ile çalıştırın.

Program tarafından gerçekleştirilen eylemleri açıklayan bir dizi ileti görmeniz gerekir. İsteklerin daha fazla ayrıntısını görmek istiyorsanız, `AzureSearchClient.request()` https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/AzureSearchClient.js#L21-L27) **AzureSearchClient.js'de**[yöntemin başındaki satırlar] yorumunu geri leyebilirsiniz. 

Azure portalında arama hizmetinizin **Genel Görünümünü** açın. **Dizinler** sekmesini seçin. Aşağıdaki gibi bir şey görmeniz gerekir:

![Azure portalının ekran görüntüsü, arama hizmetine Genel Bakış, Dizinler sekmesi](media/search-get-started-nodejs/create-index-no-data.png)

Bir sonraki adımda, dizin için veri eklersiniz. 

## <a name="2---load-documents"></a>2 - Yükleme Belgeleri 

Azure Bilişsel Arama'da belgeler, hem dizin oluşturma hem de sorgulardan çıktı girişi olan veri yapılarıdır. Bu tür verileri dizin için GÖNDERMENiz gerekir. Bu, önceki adımda yapılan işlemlerden farklı bir bitiş noktası kullanır. **AzureSearchClient.js'yi** açın ve `getIndexUrl()`aşağıdaki yöntemi ekleyin:

```javascript
 getPostDataUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs/index?api-version=${this.apiVersion}`;  }
```

Gibi, `AzureSearchClient.createIndexAsync(definition)`onun vücut olması `AzureSearchClient.request()` için otel verilerini arar ve geçer bir fonksiyon gerekir. **AzureSearchClient.js** sonra `postDataAsync(hotelsData)` `createIndexAsync(definition)`eklemek:

```javascript
async postDataAsync(hotelsData) {
    console.log("\n Adding hotel data...");
    const endpoint = this.getPostDataUrl();
    const response = await AzureSearchClient.request(endpoint,"POST", this.adminKey, hotelsData);
    AzureSearchClient.throwOnHttpError(response);
    return this;
}
```

 Belge girişleri veritabanındaki satırlar, Blob depolamasındaki lekeler veya bu örnekte olduğu gibi diskteki JSON belgeleri olabilir. [Hotels.json'ı](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/hotels.json) indirebilir veya aşağıdaki içerikle kendi **hotels.json** dosyanızı oluşturabilirsiniz:

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

Bu verileri programınıza yüklemek için, `hotelData` en üste atıfta bulunan satırı ekleyerek **index.js'yi** değiştirin:

```javascript
const nconf = require('nconf');

const hotelData = require('./hotels.json');
const indexDefinition = require('./hotels_quickstart_index.json');
```

Şimdi `run()` **index.js işlevini değiştirin.** Dizinin kullanılabilir olması birkaç saniye sürebilir, bu nedenle aramadan `AzureSearchClient.postDataAsync(hotelData)`önce 2 saniyelik bir duraklama ekleyin:

```javascript
const run = async () => {
    try {
        const cfg = getAzureConfiguration();
        const client = new AzureSearchClient(cfg.get("serviceName"), cfg.get("adminKey"), cfg.get("queryKey"), cfg.get("indexName"));
        
        const exists = await client.indexExistsAsync();
        await exists ? client.deleteIndexAsync() : Promise.resolve();
        // Deleting index can take a few seconds
        await sleep(2000);
        await client.createIndexAsync(indexDefinition);
        // Index availability can take a few seconds
        await sleep(2000);
        await client.postDataAsync(hotelData);
    } catch (x) {
        console.log(x);
    }
}
```

Programı yeniden çalıştırın. `node index.js` Adım 1'de gördükleriniz den biraz farklı bir ileti kümesi görmeniz gerekir. Bu kez, _dizin_ var ve uygulama yeni dizin oluşturmadan ve ona veri göndermeden önce dizini silme yle ilgili iletiyi görmeniz gerekir. 

## <a name="3---search-an-index"></a>3 - Dizin arama

Azure portalındaki arama hizmetinizin **Genel Görünümü'ndeki** **Dizinler** sekmesine geri dönün. Dizinşimdi dört belge içerir ve bir miktar depolama alanı tüketir (Kullanıcı Arabirimi'nin dizinin temel durumunu düzgün bir şekilde yansıtması birkaç dakika sürebilir). **Arama Gezgini'ne**götürülmek üzere dizin adını tıklatın. Bu sayfa, veri sorguları ile deneme yapmanızı sağlar. Bir sorgu dize `*&$count=true` arama deneyin ve tüm belgeleri ve sonuç sayısını geri almak gerekir. Sorgu dizesini `historic&highlight=Description&$filter=Rating gt 4` deneyin ve etiketlere `<em></em>` "tarihi" sözcüğü sarılı tek bir belgeyi geri almalısınız. [Azure Bilişsel Arama'da sorgu oluşturma](https://docs.microsoft.com/azure/search/search-query-overview)hakkında daha fazla bilgi edinin. 

**Index.js'yi** açarak ve bu kodu en üste ekleyerek bu sorguları kod halinde çoğaltın:

```javascript
const queries = [
    "*&$count=true",
    "historic&highlight=Description&$filter=Rating gt 4&"
];
```

Aynı **index.js** dosyasında, `doQueriesAsync()` aşağıda gösterilen işlevi yazın. Bu işlev `AzureSearchClient` bir nesne alır `AzureSearchClient.queryAsync` ve `queries` dizideki değerlerin her birine yöntemi uygular. Yalnızca tüm `Promise.all()` sorgular çözüldüğünde çözüme kavuşturan bir single `Promise` döndürmek için işlevi kullanır. Sorgu sonucunu `JSON.stringify(body, null, 4)` daha okunabilir olacak şekilde biçimlendirme çağrısı.

```javascript
async function doQueriesAsync(client) {
    return Promise.all(
        queries.map( async query => {
            const result = await client.queryAsync(query);
            const body = await result.json();
            const str = JSON.stringify( body, null, 4);
            console.log(`Query: ${query} \n ${str}`);
        })
    );
}
```

Dizinleyicinin `run()` çalışması için yeterince uzun süre duraklatmak `doQueriesAsync(client)` ve sonra işlevi çağırmak için işlevi değiştirin:

```javascript
const run = async () => {
    try {
        const cfg = getAzureConfiguration();
        const client = new AzureSearchClient(cfg.get("serviceName"), cfg.get("adminKey"), cfg.get("queryKey"), cfg.get("indexName"));
        
        const exists = await client.indexExistsAsync();
        await exists ? client.deleteIndexAsync() : Promise.resolve();
        // Deleting index can take a few seconds
        await sleep(2000);
        await client.createIndexAsync(indexDefinition);
        // Index availability can take a few seconds
        await sleep(2000);
        await client.postDataAsync(hotelData);
        // Data availability can take a few seconds
        await sleep(5000);
        await doQueriesAsync(client);
    } catch (x) {
        console.log(x);
    }
}
```

Uygulamak `AzureSearchClient.queryAsync(query)`için **AzureSearchClient.js**dosyasını edin. Arama yapmak farklı bir bitiş noktası gerektirir ve arama terimleri URL bağımsız değişkenleri haline gelir, bu nedenle işlevi `getSearchUrl(searchTerm)` daha önce yazdığınız `getIndexUrl()` yöntemlerin `getPostDataUrl()` yanına ekleyin.

```javascript
getSearchUrl(searchTerm) { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs?api-version=${this.apiVersion}&search=${searchTerm}&searchMode=all`; }
 ```

İşlev `queryAsync(searchTerm)` ayrıca **AzureSearchClient.js'de** de geçer `postDataAsync(data)` ve diğer sorgu işlevleriyle aynı yapıyı izler: 

```javascript
async queryAsync(searchTerm) {
    console.log("\n Querying...")
    const endpoint = this.getSearchUrl(searchTerm);
    const response = await AzureSearchClient.request(endpoint, "GET", this.queryKey);
    AzureSearchClient.throwOnHttpError(response);
    return response;
}
```

Arama terimi URL'nin bir parçası olduğundan, arama "GET" fiili ile yapılır ve gövde yoktur. Yönetici `queryAsync(searchTerm)` anahtarını kullanan diğer işlevlerin aksine, kullandığına `this.queryKey`dikkat edin. Sorgu anahtarları, adından da anlaşılacağı gibi, yalnızca dizin sorgulamak için kullanılabilir ve dizin herhangi bir şekilde değiştirmek için kullanılamaz. Bu nedenle sorgu anahtarları istemci uygulamalarına dağıtmak için daha güvenlidir.

Programı `node index.js`' yla çalıştırın. Şimdi, önceki adımlara ek olarak, sorgular gönderilir ve sonuçlar konsola yazılır.

### <a name="about-the-sample"></a>Örnek hakkında

Örnek, Azure Bilişsel Arama dizini oluşturma ve sorgulama temellerini göstermek için yeterli olan az miktarda otel verisi kullanır.

**AzureSearchClient** sınıfı, arama hizmeti için yapılandırmayı, URL'leri ve temel HTTP isteklerini kapsüller. **Index.js** dosyası, Azure Bilişsel Arama hizmetinin yapılandırma verilerini, dizin oluşturma için yüklenecek otel `run` verilerini ve işlevinde çeşitli işlemleri siparişlerve yürütür.

İşlevin `run` genel davranışı, varsa Azure Bilişsel Arama dizinini silmek, dizin oluşturmak, bazı veriler eklemek ve bazı sorgular gerçekleştirmektir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu Düğüm.js hızlı başlatmada, bir dizin oluşturmak, belgelerle yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinde çalıştınız. Yapılandırmayı okuma ve sorguları tanımlama gibi belirli adımları mümkün olan en basit şekilde yaptık. Gerçek bir uygulamada, bu endişeleri esneklik ve kapsülleme sağlayacak ayrı modüllere koymak isteyebilirsiniz. 
 
Azure Bilişsel Arama'da zaten bir geçmişiniz varsa, bu örneği öneriyi önerenler (ileri de yazı veya otomatik tamamlama sorguları), filtreler ve yönlü gezinmeyi denemek için sıçrama tahtası olarak kullanabilirsiniz. Azure Bilişsel Arama'da yeniyseniz, neler oluşturabileceğinizi anlamak için diğer öğreticileri denemenizi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. 

> [!div class="nextstepaction"]
> [Javascript kullanarak Bir Web Sayfasından Azure Bilişsel Arama'yı Arayın](https://github.com/liamca/azure-search-javascript-samples)
