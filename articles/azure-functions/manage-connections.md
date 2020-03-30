---
title: Azure İşlevleri'ndeki bağlantıları yönetme
description: Statik bağlantı istemcilerini kullanarak Azure İşlevleri'nde performans sorunlarını nasıl önleyebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 02/25/2018
ms.openlocfilehash: 872ad9a1b8f0a7da6fe410e68f08469ac11045a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276458"
---
# <a name="manage-connections-in-azure-functions"></a>Azure İşlevleri'ndeki bağlantıları yönetme

İşlev uygulamasındaki işlevler kaynakları paylaşır. Paylaşılan kaynaklar arasında bağlantılar şunlardır: HTTP bağlantıları, veritabanı bağlantıları ve Azure Depolama gibi hizmetlere bağlantılar. Birçok işlev aynı anda çalışırken, kullanılabilir bağlantıları niçin bitebilir. Bu makalede, gereksinim duyduklarından daha fazla bağlantı kullanmaktan kaçınmak için işlevlerinizi nasıl kodlarınız açıklanmaktadır.

## <a name="connection-limit"></a>Bağlantı sınırı

Bir işlev uygulaması [bir kum havuzu ortamında](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)çalıştığından, kullanılabilir bağlantı sayısı kısmen sınırlıdır. Sandbox'ın kodunuza uyguladığı kısıtlamalardan biri, şu anda her örnek te 600 etkin (toplam 1.200) bağlantı olan giden bağlantı sayısının sınırıdır. Bu sınıra ulaştığınızda, çalışma zamanı işlevleri günlüklere aşağıdaki `Host thresholds exceeded: Connections`iletiyi yazar: . Daha fazla bilgi [için, Fonksiyonlar hizmet sınırlarına](functions-scale.md#service-limits)bakın.

Bu sınır her örnek başınadır. Ölçek [denetleyicisi](functions-scale.md#how-the-consumption-and-premium-plans-work) daha fazla isteği işlemek için işlev uygulaması örnekleri eklediğinde, her örnekte bağımsız bir bağlantı sınırı vardır. Bu, genel bağlantı sınırı olmadığı anlamına gelir ve tüm etkin durumlarda 600'den fazla etkin bağlantınız olabilir.

Sorun giderme de işlev uygulamanız için Uygulama Öngörüleri'ni etkinleştirdiğinizden emin olun. Uygulama Öngörüleri, yürütmeler gibi işlev uygulamalarınız için ölçümleri görüntülemenizi sağlar. Daha fazla bilgi için [bkz.](functions-monitoring.md#view-telemetry-in-application-insights)  

## <a name="static-clients"></a>Statik istemciler

Gerekenden daha fazla bağlantı tutmamak için, her işlev çağırmasında yeni bağlantılar oluşturmak yerine istemci örneklerini yeniden kullanın. İşlevinizi yazabileceğiniz herhangi bir dil için istemci bağlantılarını yeniden kullanmanızı öneririz. Örneğin, [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx), [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
)ve Azure Depolama istemcileri gibi .NET istemcileri, tek bir statik istemci kullanıyorsanız bağlantıları yönetebilir.

Azure İşlevleri uygulamasında hizmete özel bir istemci kullanırken uymanız gereken bazı yönergeler şunlardır:

- Her işlev çağırmaile yeni bir istemci *oluşturmayın.*
- Her işlev çağırmanın kullanabileceği tek, statik bir istemci *oluşturun.*
- Farklı işlevler aynı hizmeti kullanıyorsa, paylaşılan bir yardımcı sınıfında tek, statik istemci oluşturmayı *düşünün.*

## <a name="client-code-examples"></a>İstemci kodu örnekleri

Bu bölümde, işlev kodunuzdan istemciler oluşturmak ve kullanmak için en iyi uygulamalar gösterin.

### <a name="httpclient-example-c"></a>Httpİste örneği (C#)

Aşağıda statik bir [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) örneği oluşturan C# işlev kodunun bir örneği verilmiştir:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

.NET'te [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) ile ilgili yaygın bir soru "Müvekkilimi elden çıkarmalı mıyım?" Genel olarak, bunları kullanmayı bitirdiğinizde uygulanan `IDisposable` nesneleri imha ekarsınız. Ancak statik bir istemciyi elden çıkarmazsınız, çünkü işlev sona erdiğinde onu kullanmayı bitirmezsiniz. Statik istemcinin başvurunuz süresince yaşamasını istiyorsunuz.

### <a name="http-agent-examples-javascript"></a>HTTP aracı örnekleri (JavaScript)

Daha iyi bağlantı yönetimi seçenekleri sağladığından, [`http.agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) `node-fetch` modül gibi yerel olmayan yöntemler yerine yerel sınıfı kullanmanız gerekir. Bağlantı parametreleri sınıftaki `http.agent` seçenekler aracılığıyla yapılandırılır. HTTP aracısı ile birlikte sunulan ayrıntılı seçenekler için [yeni Temsilci\[(seçenekler)\]](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options)bakın.

Kullanılan `http.request()` `http.globalAgent` genel sınıf, tüm bu değerleri kendi varsayılanlarına göre ayarlamıştır. İşlevlerde bağlantı sınırlarını yapılandırmanın önerilen yolu, genel olarak en büyük sayıyı ayarlamaktır. Aşağıdaki örnek, işlev uygulaması için en fazla soket sayısını ayarlar:

```js
http.globalAgent.maxSockets = 200;
```

 Aşağıdaki örnek, yalnızca bu istek için özel bir HTTP aracısı içeren yeni bir HTTP isteği oluşturur:

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>DocumentClient kod örneği (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) bir Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belgeleri, [uygulamanızın ömrü boyunca tek tonluk bir Azure Cosmos DB istemcisi kullanmanızı](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage)önerir. Aşağıdaki örnek, bir işlevde bunu yapmak için bir desen gösterir:

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

### <a name="cosmosclient-code-example-javascript"></a>CosmosClient kod örneği (JavaScript)
[CosmosClient](/javascript/api/@azure/cosmos/cosmosclient) bir Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belgeleri, [uygulamanızın ömrü boyunca tek tonluk bir Azure Cosmos DB istemcisi kullanmanızı](../cosmos-db/performance-tips.md#sdk-usage)önerir. Aşağıdaki örnek, bir işlevde bunu yapmak için bir desen gösterir:

```javascript
const cosmos = require('@azure/cosmos');
const endpoint = process.env.COSMOS_API_URL;
const key = process.env.COSMOS_API_KEY;
const { CosmosClient } = cosmos;

const client = new CosmosClient({ endpoint, key });
// All function invocations also reference the same database and container.
const container = client.database("MyDatabaseName").container("MyContainerName");

module.exports = async function (context) {
    const { resources: itemArray } = await container.items.readAll().fetchAll();
    context.log(itemArray);
}
```

## <a name="sqlclient-connections"></a>SqlClient bağlantıları

İşlev kodunuz, SQL ilişkisel veritabanına bağlantı yapmak için SQL Server için .NET Framework Data Provider[(SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)kullanabilir. Bu aynı zamanda [Varlık Çerçevesi](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx)gibi ADO.NET dayanan veri çerçeveleri için temel sağlayıcıdır. [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) ve [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) bağlantılarının aksine, ADO.NET varsayılan olarak bağlantı havuzu uygular. Ancak hala bağlantılarınız bitebildiği için veritabanına bağlantıları en iyi duruma getirmelisiniz. Daha fazla bilgi için [SQL Server Connection Pooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)adresine bakın.

> [!TIP]
> Varlık Çerçevesi gibi bazı veri çerçeveleri genellikle yapılandırma dosyasının **ConnectionStrings** bölümünden bağlantı dizeleri alır. Bu durumda, işlev uygulama ayarlarınızın **Bağlantı dizeleri** koleksiyonuna ve yerel projenizdeki [local.settings.json dosyasına](functions-run-local.md#local-settings-file) açıkça SQL veritabanı bağlantı dizeleri eklemeniz gerekir. İşlev kodunuzda [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) örneği oluşturuyorsanız, bağlantı dize değerini diğer bağlantılarınızla birlikte **Uygulama ayarlarında** depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Statik istemcileri neden önerdiğimiz hakkında daha fazla bilgi [için, Uygunsuz anlık antidesen](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)ebak.

Daha fazla Azure İşlevleri performans ipucu için azure [işlevlerinin performansını ve güvenilirliğini optimize edin.](functions-best-practices.md)
