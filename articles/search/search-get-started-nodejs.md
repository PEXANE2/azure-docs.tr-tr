---
title: "Hızlı başlangıç: REST API 'Leri kullanarak Node.js arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu Node.js hızlı başlangıçta, JavaScript ve REST API 'Leri kullanarak bir dizin oluşturma, veri yükleme ve Azure Bilişsel Arama sorguları çalıştırma hakkında bilgi edinin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.devlang: nodejs
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 17c64fd1d0e04130867b484ff7eb3fcdb3f0977d
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85263288"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-in-nodejs-using-rest-apis"></a>Hızlı başlangıç: REST API 'Leri kullanarak Node.js Azure Bilişsel Arama dizini oluşturma
> [!div class="op_single_selector"]
> * [JavaScript](search-get-started-nodejs.md)
> * [C#](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)

Bir Azure Bilişsel Arama dizini oluşturan, yükleyen ve sorgulayan bir Node.js uygulaması oluşturun. Bu makalede, uygulamanın adım adım nasıl oluşturulacağı gösterilmektedir. Alternatif olarak, [kaynak kodu ve verileri indirebilir](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/) ve uygulamayı komut satırından çalıştırabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı derlemek ve test etmek için aşağıdaki yazılım ve Hizmetleri kullandık:

+ [Node.js](https://nodejs.org)

+ [NPM](https://www.npmjs.com) Node.js tarafından yüklenmelidir

+ Bu makalede veya [deponun **hızlı başlangıç** dizininden örnek bir dizin yapısı ve eşleşen belgeler verilmiştir](https://github.com/Azure-Samples/azure-search-javascript-samples/)

+ Geçerli aboneliğinizde [bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

Önerilen:

* [Visual Studio Code](https://code.visualstudio.com)

* VSCode için [prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) ve [eslint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) uzantıları.

<a name="get-service-info"></a>

## <a name="get-keys-and-urls"></a>Anahtar ve URL 'Leri al

Hizmete yapılan çağrılar, her istekte bir URL uç noktası ve erişim anahtarı gerektirir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetinize **genel bakış** sayfasında, arama hizmetinizin adını alın. Uç nokta URL 'sini inceleyerek hizmet adınızı doğrulayabilirsiniz. Uç nokta URL 'niz olsaydı `https://mydemo.search.windows.net` , hizmet adınız olur `mydemo` .

2. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

    Sorgu anahtarını da alın. Salt okuma erişimiyle sorgu istekleri vermek en iyi uygulamadır.

![Hizmet adı ve yönetici ve sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

Tüm istekler, hizmetinize gönderilen her isteğin üstbilgisinde bir API anahtarı gerektirir. Geçerli bir anahtar, istek başına, isteği gönderen uygulama ve onu işleyen hizmet arasında güven oluşturur.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Node.js yüklediğiniz bir PowerShell konsolunu veya başka bir ortamı açarak başlayın.

1. Şu adı vererek bir geliştirme dizini oluşturun `quickstart` :

    ```powershell
    mkdir quickstart
    cd quickstart
    ```

2. Çalıştırarak, NPM ile boş bir proje başlatın `npm init` . Lisans dışında, "MıT" olarak ayarlamanız gereken varsayılan değerleri kabul edin. 

1. Kod ve geliştirmeye yardımcı olacak paketler ekleyin:

    ```powershell
    npm install nconf node-fetch
    npm install --save-dev eslint eslint-config-prettier eslint-config-airbnb-base eslint-plugin-import prettier
    ```

4. **package.js** dosyanın şuna benzer göründüğünü denetleyerek projeleri ve bağımlılıklarını yapılandırdığınızdan emin olun:

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

5. Arama hizmeti verilerinizi tutmak için **azure_search_config.js** bir dosya oluşturun:

    ```json
    {
        "serviceName" : "[SEARCH_SERVICE_NAME]",
        "adminKey" : "[ADMIN_KEY]",
        "queryKey" : "[QUERY_KEY]",
        "indexName" : "hotels-quickstart"
    }
    ```

Değeri, `[SERVICE_NAME]` arama hizmetinizin adıyla değiştirin. `[ADMIN_KEY]`Ve `[QUERY_KEY]` değerlerini daha önce kaydettiğiniz anahtar değerleriyle değiştirin. 

## <a name="1---create-index"></a>1-Dizin oluşturma 

Üzerinde bir dosya **hotels_quickstart_index.js**oluşturun.  Bu dosya, Azure Bilişsel Arama 'nin bir sonraki adımda yüklediğiniz belgelerle nasıl çalıştığını tanımlar. Her bir alan ile tanımlanır `name` ve belirtilmiş olur `type` . Her alan Ayrıca, Azure Bilişsel Arama alan üzerinde arama, filtreleme, sıralama ve model kullanıp kullanamayacağını belirten bir dizi dizin özniteliklerine sahiptir. Alanların çoğu basit veri türleridir, ancak bazıları `AddressType` dizininiz içinde zengin veri yapıları oluşturmanıza imkan tanıyan karmaşık türlerdir.  [Desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) ve [dizin öznitelikleri](https://docs.microsoft.com/azure/search/search-what-is-an-index#index-attributes)hakkında daha fazla bilgi edinebilirsiniz. 

**hotels_quickstart_index.js** için aşağıdakini ekleyin veya [dosyayı indirin](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/hotels_quickstart_index.json). 

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
    

Genel olarak geçerli olacak koddan belirli bir senaryonun özelliklerini ayırmak iyi bir uygulamadır. `AzureSearchClient`Dosya **AzureSearchClient.js** tanımlanan sınıf, istek URL 'lerinin nasıl OLUŞTURULDUĞUNU, Fetch API kullanarak istek yapmayı ve yanıtın durum koduna tepki vermesini bilecektir.

**Düğüm getirme** paketini içeri aktararak ve basit bir sınıf oluşturarak **AzureSearchClient.js** üzerinde çalışmaya başlayabilirsiniz. `AzureSearchClient`Oluşturucusunun çeşitli yapılandırma değerlerini geçirerek, sınıfın değiştirilebilir parçalarını yalıtın:

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

Sınıfının ilk sorumluluğu, çeşitli isteklerin gönderileceği URL 'Leri oluşturmayı öğrenmektir. Sınıf oluşturucusuna geçirilen yapılandırma verilerini kullanan örnek yöntemleriyle bu URL 'Leri derleyin. Oluşturdukları URL 'nin bir API sürümüne özgü olduğunu ve bu sürümü belirten bir bağımsız değişken (Bu uygulamada) olması gerektiğini unutmayın `2019-05-06` . 

Bu yöntemlerin ilki dizinin URL 'sini döndürür. Aşağıdaki yöntemi sınıf gövdesinin içine ekleyin:

```javascript
getIndexUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}?api-version=${this.apiVersion}`; }

```

Bir sonraki sorumluluğu, `AzureSearchClient` Fetch API 'si ile zaman uyumsuz bir istek yapıyor. Zaman uyumsuz statik yöntem `request` BIR URL 'yi, http yöntemini ("Get", "put", "Post", "Delete"), istekte kullanılacak anahtarı ve isteğe bağlı BIR JSON nesnesini belirten bir dize alır. `headers`Değişkeni, `queryKey` "api-Key" http istek üst bilgisi ile (yönetici anahtarı veya salt okuma sorgu anahtarı) ile eşleşir. İstek seçenekleri her zaman kullanılacak öğesini `method` ve öğesini içerir `headers` . `bodyJson`Değilse `null` , http isteğinin gövdesi öğesinin dize gösterimine ayarlanır `bodyJson` . `request`Yöntemi, http isteğini yürütmek Için GETIRME API 'Sinin taahhüdünü döndürür.

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

Tanıtım amacıyla, HTTP isteği başarılı değilse yalnızca bir özel durum oluşturur. Gerçek bir uygulamada büyük olasılıkla, arama hizmeti isteğinden içindeki HTTP durum kodunu bir günlüğe kaydetme ve tanılama yapabilirsiniz `response` . 
    
```javascript
static throwOnHttpError(response) {
    const statusCode = response.status;
    if (statusCode >= 300){
        console.log(`Request failed: ${JSON.stringify(response, null, 4)}`);
        throw new Error(`Failure in request. HTTP Status was ${statusCode}`);
    }
}
```

Son olarak, Azure Bilişsel Arama dizinini algılamaya, silmeye ve oluşturmaya yönelik yöntemleri ekleyin. Bu yöntemlerin hepsi aynı yapıya sahiptir:

* İsteğin oluşturulacağı uç noktayı alın.
* İsteği uygun uç nokta, HTTP fiili, API anahtarı ve uygunsa bir JSON gövdesi ile oluşturun. `indexExistsAsync()`ve `deleteIndexAsync()` BIR JSON gövdesi yoktur, ancak bunu `createIndexAsync(definition)` yapar.
* `await`isteğin yanıtı.  
* Yanıtın durum kodu üzerinde işlem yapın.
* Uygun bir değer (Boolean, `this` veya sorgu sonuçları) için bir Promise döndürün. 

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

Yöntemlerinizin sınıfın içinde olduğunu ve sınıfı dışarı aktarıp çıkardığınızı doğrulayın. **AzureSearchClient.js** en dıştaki kapsam şu olmalıdır:

```javascript
const fetch = require('node-fetch');

class AzureSearchClient {
    // ... code here ...
}

module.exports = AzureSearchClient;
```

Nesne odaklı bir sınıf, olası yeniden kullanılabilir **AzureSearchClient.js** modülü için iyi bir seçimdir, ancak **index.js**adlı bir dosyaya yerleştirmeniz gereken ana program için gerekli değildir. 

**index.js** oluşturun ve şu şekilde başlatın:

* JSON, ortam değişkenleri veya komut satırı bağımsız değişkenleriyle yapılandırmayı belirtme esnekliği sağlayan **NConf** paketi.
* Dosyadaki **hotels_quickstart_index.js** verileri.
* `AzureSearchClient` modülü.

```javascript
const nconf = require('nconf');

const indexDefinition = require('./hotels_quickstart_index.json');
const AzureSearchClient = require('./AzureSearchClient.js');
```

[ **NConf** paketi](https://github.com/indexzero/nconf) , yapılandırma verilerini ortam değişkenleri veya komut satırı gibi çeşitli biçimlerde belirtmenize olanak tanır. Bu örnek, **azure_search_config.js** dosyayı okumak ve dosyanın içeriğini bir sözlük olarak döndürmek için temel bir şekilde **NConf** kullanır. **NConf**'nin `get(key)` işlevini kullanarak, yapılandırma bilgilerinin düzgün şekilde özelleştirildiğini hızlı bir şekilde denetleyebilirsiniz. Son olarak, işlev yapılandırmayı döndürür:

```javascript
function getAzureConfiguration() {
    const config = nconf.file({ file: 'azure_search_config.json' });
    if (config.get('serviceName') === '[SEARCH_SERVICE_NAME]' ) {
        throw new Error("You have not set the values in your azure_search_config.json file. Change them to match your search service's values.");
    }
    return config;
}
```

`sleep`İşlevi, `Promise` belirli bir süre sonra çözümlenen bir oluşturur. Bu işlevin kullanılması, zaman uyumsuz dizin işlemlerinin tamamlanmasını beklerken ve kullanılabilir hale geleken uygulamanın duraklatılmasını sağlar. Böyle bir gecikme eklemek, genellikle yalnızca tanıtımlar, testler ve örnek uygulamalar için gereklidir.

```javascript
function sleep(ms) {
    return(
        new Promise(function(resolve, reject) {
            setTimeout(function() { resolve(); }, ms);
        })
    );
}
```

Son olarak, Main zaman uyumsuz işlevini belirtin ve çağırın `run` . Bu işlev, diğer işlevleri sırayla çağırır ve bunları çözmek için gereken şekilde bekliyor `Promise` .

* `getAzureConfiguration()`Daha önce yazdığınız yapılandırmayla yapılandırmayı alın
* `AzureSearchClient`Yapılandırmanızda değerler geçirerek yeni bir örnek oluşturma
* Dizinin mevcut olup olmadığını denetleyin ve varsa silin
* `indexDefinition` **Üzerinde yüklenenhotels_quickstart_index.js** kullanarak dizin oluşturma

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
    } catch (x) {
        console.log(x);
    }
}

run();
```

Son ' a yapılan çağrıyı unutmayın `run()` ! Bu, bir sonraki adımda çalıştırdığınızda programınızın giriş noktasıdır `node index.js` .

Bu `AzureSearchClient.indexExistsAsync()` ve `AzureSearchClient.deleteIndexAsync()` parametre almaz. Bu işlevler `AzureSearchClient.request()` `bodyJson` bağımsız değişken olmadan çağırır. İçinde `AzureSearchClient.request()` `bodyJson === null` olduğu için, `true` `init` yapı yalnızca http fiili ("Get" for `indexExistsAsync()` ve "Delete" for) olarak ayarlanır `deleteIndexAsync()` ve istek anahtarını belirten üst bilgileri ve üst bilgileri.  

Buna karşılık, `AzureSearchClient.createIndexAsync(indexDefinition)` yöntemi bir _does_ parametre alır. `run`İçindeki işlevi `index.js` , **hotels_quickstart_index.js** dosya içeriğini `AzureSearchClient.createIndexAsync(indexDefinition)` yöntemine geçirir. `createIndexAsync()`Yöntemi bu tanımı öğesine geçirir `AzureSearchClient.request()` . ' De, `AzureSearchClient.request()` `bodyJson === null` artık olduğu `false` için, `init` yapı yalnızca http fiili ("put") ve üst bilgileri içermez, ancak öğesini `body` Dizin tanımı verileri olarak ayarlar.

### <a name="prepare-and-run-the-sample"></a>Örneği hazırlama ve çalıştırma

Aşağıdaki komutlar için bir Terminal penceresi kullanın.

1. Dosyadaki **package.js** ve kodunuzun geri kalanında bulunan klasöre gidin.
1. Örneği için paketleri ile birlikte yükler `npm install` .  Bu komut, kodun bağımlı olduğu paketleri indirir.
1. Programınızı ile çalıştırın `node index.js` .

Program tarafından gerçekleştirilen eylemleri açıklayan bir ileti serisi görmeniz gerekir. İsteklerle daha fazla ayrıntı görmek isterseniz, `AzureSearchClient.request()` https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/AzureSearchClient.js#L21-L27) **AzureSearchClient.js**' de [yöntemin başındaki satırların] açıklamasını kaldırabilirsiniz. 

Azure portal arama hizmetinize **Genel Bakış ' ı** açın. **Dizinler** sekmesini seçin. Aşağıdakine benzer bir şey görmeniz gerekir:

![Azure portal, arama hizmetine genel bakış, dizinler sekmesinin ekran görüntüsü](media/search-get-started-nodejs/create-index-no-data.png)

Sonraki adımda, dizine veri ekleyeceksiniz. 

## <a name="2---load-documents"></a>2-belge yükleme 

Azure Bilişsel Arama 'de belgeler, sorguların dizin oluşturma ve çıkışlara yönelik giriş olan veri yapılarıdır. Bu tür verileri dizine NAKLETMENIZ gerekir. Bu, önceki adımda gerçekleştirilen işlemlerden farklı bir uç nokta kullanır. **AzureSearchClient.js** açın ve sonra aşağıdaki yöntemi ekleyin `getIndexUrl()` :

```javascript
 getPostDataUrl() { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs/index?api-version=${this.apiVersion}`;  }
```

Benzer `AzureSearchClient.createIndexAsync(definition)` `AzureSearchClient.request()` şekilde, otel verilerini çağıran ve gövdesinde geçen bir işleve ihtiyacınız vardır. **AzureSearchClient.js** sonra Ekle `postDataAsync(hotelsData)` `createIndexAsync(definition)` :

```javascript
async postDataAsync(hotelsData) {
    console.log("\n Adding hotel data...");
    const endpoint = this.getPostDataUrl();
    const response = await AzureSearchClient.request(endpoint,"POST", this.adminKey, hotelsData);
    AzureSearchClient.throwOnHttpError(response);
    return this;
}
```

 Belge girişleri bir veritabanındaki satırlar, blob depolamada Bloblar veya bu örnekte olduğu gibi, diskteki JSON belgeleri olabilir. [Üzerindehotels.js](https://github.com/Azure-Samples/azure-search-javascript-samples/blob/master/quickstart/hotels.json) indirebilir veya kendi **hotels.js** dosyanızı aşağıdaki içerikle oluşturabilirsiniz:

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

Bu verileri programınıza yüklemek için, en üst tarafına başvuran satırı ekleyerek **index.js** değiştirin `hotelData` :

```javascript
const nconf = require('nconf');

const hotelData = require('./hotels.json');
const indexDefinition = require('./hotels_quickstart_index.json');
```

Şimdi `run()` **index.js**işlevini değiştirin. Dizinin kullanılabilir hale gelmesi birkaç saniye sürebilir, bu nedenle çağrılmadan önce 2 saniyelik bir duraklama ekleyin `AzureSearchClient.postDataAsync(hotelData)` :

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

Programını ile yeniden çalıştırın `node index.js` . Adım 1 ' de gördüğenlerden biraz farklı bir ileti kümesi görmeniz gerekir. Bu kez _, Dizin mevcut_ olur ve uygulama yeni dizin oluşturmadan ve verileri kendisine göndermeksizin silme hakkında bir ileti görmeniz gerekir. 

## <a name="3---search-an-index"></a>3 - Dizin arama

Azure portal arama hizmetinize **genel bakış** bölümünde **dizinler** sekmesine dönün. Dizininiz artık dört belge içerir ve bir miktar depolama alanı tüketir (UI 'nin dizinin temel durumunu düzgün bir şekilde yansıtması birkaç dakika sürebilir). **Arama Gezgini**'ne alınacak dizin adına tıklayın. Bu sayfa, veri sorgularıyla denemeler yapmanıza olanak sağlar. Bir sorgu dizesinde arama yapmayı deneyin `*&$count=true` ve tüm belgelerinizi ve sonuç sayısını geri almanız gerekir. Sorgu dizesiyle deneyin `historic&highlight=Description&$filter=Rating gt 4` ve etiketlerin "geçmiş" kelimesiyle birlikte tek bir belgeyi geri almanız gerekir `<em></em>` . [Azure bilişsel arama 'de bir sorgu oluşturma](https://docs.microsoft.com/azure/search/search-query-overview)hakkında daha fazla bilgi edinin. 

**index.js** açıp üst kısımdaki Bu kodu ekleyerek bu sorguları kodda yeniden oluşturun:

```javascript
const queries = [
    "*&$count=true",
    "historic&highlight=Description&$filter=Rating gt 4&"
];
```

Aynı **index.js** dosyasında, `doQueriesAsync()` aşağıda gösterilen işlevi yazın. Bu işlev bir `AzureSearchClient` nesnesi alır ve `AzureSearchClient.queryAsync` dizideki her değere yöntemi uygular `queries` . `Promise.all()` `Promise` Yalnızca tüm sorgular çözümlendikten sonra çözümlenen bir Single döndürmek için işlevini kullanır. `JSON.stringify(body, null, 4)`Sorgu sonucunu daha okunaklı olacak şekilde biçimlendirir.

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

`run()`Dizin oluşturucunun çalışması için yeteri kadar uzun süre duraklatmak ve sonra işlevi çağırmak için işlevi değiştirin `doQueriesAsync(client)` :

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

Uygulamak için `AzureSearchClient.queryAsync(query)` **AzureSearchClient.js**dosyayı düzenleyin. Arama farklı bir uç nokta gerektiriyor ve arama terimleri URL bağımsız değişkenleri haline geldi, bu nedenle işlevi `getSearchUrl(searchTerm)` , `getIndexUrl()` daha `getPostDataUrl()` önce yazdığınız ve metotların yanına ekleyin.

```javascript
getSearchUrl(searchTerm) { return `https://${this.searchServiceName}.search.windows.net/indexes/${this.indexName}/docs?api-version=${this.apiVersion}&search=${searchTerm}&searchMode=all`; }
 ```

`queryAsync(searchTerm)`İşlev ayrıca **AzureSearchClient.js** gider ve `postDataAsync(data)` diğer sorgulama işlevleriyle aynı yapıyı izler: 

```javascript
async queryAsync(searchTerm) {
    console.log("\n Querying...")
    const endpoint = this.getSearchUrl(searchTerm);
    const response = await AzureSearchClient.request(endpoint, "GET", this.queryKey);
    AzureSearchClient.throwOnHttpError(response);
    return response;
}
```

Arama terimi URL 'nin bir parçası olduğundan, arama "GET" fiili ve gövde olmadan gerçekleştirilir. `queryAsync(searchTerm)` `this.queryKey` ' Nin, yönetici anahtarını kullanan diğer işlevlerin aksine, kullandığını fark. Adın gösterdiği gibi sorgu anahtarları yalnızca dizini sorgulamak için kullanılabilir ve dizini herhangi bir şekilde değiştirmek için kullanılamaz. Bu nedenle, sorgu anahtarlarının istemci uygulamalarına dağıtılması daha güvenlidir.

Programı ile çalıştırın `node index.js` . Şimdi, önceki adımlara ek olarak sorgular gönderilir ve sonuçlar konsola yazılır.

### <a name="about-the-sample"></a>Örnek hakkında

Örnek, Azure Bilişsel Arama Dizin oluşturma ve sorgulama hakkında temel bilgileri göstermek için yeterli miktarda otel verisi kullanır.

**AzureSearchClient** sınıfı, arama hizmeti için yapılandırma, URL 'ler ve temel http isteklerini kapsar. **index.js** dosyası, Azure bilişsel arama hizmeti için yapılandırma verilerini, dizin oluşturma için karşıya yüklenecek otel verilerini ve kendi `run` işlevinde, siparişlerde ve çeşitli işlemleri yürüten şekilde yükler.

İşlevin genel davranışı, `run` varsa Azure bilişsel arama dizinini silmek, dizini oluşturmak, bazı verileri eklemek ve bazı sorgular gerçekleştirmek için kullanılır.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu Node.js hızlı başlangıçta, dizin oluşturmak, belgeler ile yüklemek ve sorguları çalıştırmak için bir dizi görev üzerinden çalıştık. Yapılandırmayı okuma ve sorguları tanımlama gibi mümkün olan en basit yol için bazı adımlar yaptık. Gerçek bir uygulamada, bu kaygıları esneklik ve kapsülleme sağlayacak ayrı modüller halinde yerleştirmek isteyeceksiniz. 
 
Azure Bilişsel Arama 'de zaten bir arka plana sahipseniz, bu örneği, öneri araçları (tür kullanımı veya otomatik tamamlama sorguları), filtreler ve çok yönlü gezinme için bir Springboard olarak kullanabilirsiniz. Azure Bilişsel Arama 'de yeni başladıysanız, oluşturabileceğiniz özellikler hakkında daha fazla öğreticiyi anlamak için başka öğreticiler denemeyi öneririz. Daha fazla kaynak bulmak için [belge sayfamızı](https://azure.microsoft.com/documentation/services/search/) ziyaret edin. 

> [!div class="nextstepaction"]
> [JavaScript kullanarak bir Web sayfasından Azure Bilişsel Arama çağırma](https://github.com/liamca/azure-search-javascript-samples)
