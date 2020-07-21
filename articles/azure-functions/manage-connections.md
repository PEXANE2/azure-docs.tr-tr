---
title: Azure Işlevlerinde bağlantıları yönetme
description: Statik bağlantı istemcileri kullanarak Azure Işlevlerinde performans sorunlarından kaçınmaya nasıl engel olabileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 02/25/2018
ms.openlocfilehash: 5ab59d82ad4b11e4ac5179ef727392a83bb263e3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86506001"
---
# <a name="manage-connections-in-azure-functions"></a>Azure Işlevlerinde bağlantıları yönetme

Bir işlev uygulamasındaki işlevler kaynakları paylaşır. Bu paylaşılan kaynaklar arasında bağlantılar: HTTP bağlantıları, veritabanı bağlantıları ve Azure depolama gibi hizmetlere bağlantılar. Aynı anda çok sayıda işlev çalıştığında, kullanılabilir bağlantıların tükenme olasılığı vardır. Bu makalede, işlevlerinizin, gerekenden daha fazla bağlantı kullanmaktan kaçınmak için nasıl kodleneceği açıklanır.

## <a name="connection-limit"></a>Bağlantı sınırı

Bir işlev uygulaması bir [korumalı alan ortamında](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)çalıştığından, kullanılabilir bağlantı sayısı kısmen sınırlıdır. Korumalı alanın kodunuzda hangi kısıtlamalardan biri, örnek başına 600 etkin (1.200 toplam) bağlantı olan giden bağlantı sayısı için bir sınırlamadır. Bu sınıra ulaştığınızda, işlevler çalışma zamanı şu iletiyi günlüklere Yazar: `Host thresholds exceeded: Connections` . Daha fazla bilgi için bkz. [işlevler hizmet limitleri](functions-scale.md#service-limits).

Bu sınır örnek başına. Ölçek denetleyicisi daha fazla isteği işlemek için [işlev uygulama örnekleri eklerse](functions-scale.md#how-the-consumption-and-premium-plans-work) , her örneğin bağımsız bir bağlantı sınırı vardır. Bu, genel bağlantı sınırı olmadığı anlamına gelir ve tüm etkin örneklerde 600 ' den çok etkin bağlantınız olabilir.

Sorun giderirken, işlev uygulamanız için Application Insights etkinleştirdiğinizden emin olun. Application Insights, yürütme gibi işlev uygulamalarınız için ölçümleri görüntülemenize olanak sağlar. Daha fazla bilgi için bkz. [Application Insights Telemetriyi görüntüleme](functions-monitoring.md#view-telemetry-in-application-insights).  

## <a name="static-clients"></a>Statik istemciler

Gerekenden daha fazla bağlantı tutmaya kaçınmak için, her bir işlev çağrısında yeni bir tane oluşturmak yerine istemci örneklerini yeniden kullanın. İşlevinizi yazmak isteyebileceğiniz herhangi bir dil için istemci bağlantılarını yeniden kullanmanızı öneririz. Örneğin, [HttpClient](/dotnet/api/system.net.http.httpclient?view=netcore-3.1), [Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient)ve Azure Storage istemcileri gibi .NET istemcileri, tek bir statik istemci kullanırsanız bağlantıları yönetebilir.

Azure Işlevleri uygulamasında hizmete özel bir istemci kullanırken izlenecek bazı yönergeler aşağıda verilmiştir:

- Her işlev çağrısında yeni bir *istemci oluşturmayın.*
- Her işlev çağrısının kullanabileceği tek bir statik *istemci oluşturun.*
- Farklı işlevler aynı hizmeti kullanıyorsa, paylaşılan bir yardımcı sınıfta tek bir statik istemci oluşturmayı *düşünün* .

## <a name="client-code-examples"></a>İstemci kodu örnekleri

Bu bölümde, işlev kodunuzda istemcileri oluşturmak ve kullanmak için en iyi yöntemler gösterilmektedir.

### <a name="httpclient-example-c"></a>HttpClient örneği (C#)

Statik bir [HttpClient](/dotnet/api/system.net.http.httpclient?view=netcore-3.1) örneği oluşturan C# işlev kodu örneği aşağıda verilmiştir:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

.NET 'teki [HttpClient](/dotnet/api/system.net.http.httpclient?view=netcore-3.1) hakkında sık sorulan sorular "İstemcimin atılmalıdır mi?" Genel olarak, `IDisposable` bunları kullanarak işiniz bittiğinde uygulayan nesneleri atıyorsunuz. Ancak, işlev sona erdiğinde bu işlemi yapmadığınızda, bir statik istemciyi atmayın. Statik istemcinin uygulamanızın süresini gerçek zamanlı olarak istiyor.

### <a name="http-agent-examples-javascript"></a>HTTP Aracısı örnekleri (JavaScript)

Daha iyi bağlantı yönetimi seçenekleri sağladığından, [`http.agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent) modül gibi yerel olmayan yöntemler yerine yerel sınıfı kullanmanız gerekir `node-fetch` . Bağlantı parametreleri, sınıfındaki seçenekler aracılığıyla yapılandırılır `http.agent` . HTTP aracısında kullanılabilen ayrıntılı seçenekler için, bkz. [Yeni Aracı ( \[ Seçenekler \] )](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options).

`http.globalAgent`Tarafından kullanılan genel sınıf, `http.request()` Bu değerlerin tümünü kendi varsayılanlarına ayarlanmış olarak içerir. Işlevlerde bağlantı sınırlarını yapılandırmak için önerilen yöntem, genel olarak en yüksek sayıyı ayarlayamaktır. Aşağıdaki örnek, işlev uygulaması için maksimum yuva sayısını ayarlar:

```js
http.globalAgent.maxSockets = 200;
```

 Aşağıdaki örnek yalnızca bu istek için özel bir HTTP aracısına sahip yeni bir HTTP isteği oluşturur:

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>DocumentClient kod örneği (C#)

[Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient) bir Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belge, [Uygulamanızın ömrü boyunca tek bir Azure Cosmos db istemci kullanmanızı](../cosmos-db/performance-tips.md#sdk-usage)önerir. Aşağıdaki örnek, bir işlevinde bunu yapmak için bir model gösterir:

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

### <a name="cosmosclient-code-example-javascript"></a>CosmosClient kodu örneği (JavaScript)
[CosmosClient](/javascript/api/@azure/cosmos/cosmosclient) bir Azure Cosmos DB örneğine bağlanır. Azure Cosmos DB belge, [Uygulamanızın ömrü boyunca tek bir Azure Cosmos db istemci kullanmanızı](../cosmos-db/performance-tips.md#sdk-usage)önerir. Aşağıdaki örnek, bir işlevinde bunu yapmak için bir model gösterir:

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

İşlev kodunuz, bir SQL ilişkisel veritabanına bağlantı kurmak için SQL Server ([SqlClient](/dotnet/api/system.data.sqlclient?view=dotnet-plat-ext-3.1)) için .NET Framework veri sağlayıcısı kullanabilir. Bu aynı zamanda, [Entity Framework](/ef/ef6/)gibi ADO.NET kullanan veri çerçeveleri için temel sağlayıcıdır. [HttpClient](/dotnet/api/system.net.http.httpclient?view=netcore-3.1) ve [documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient) bağlantılarından farklı olarak, ADO.NET varsayılan olarak bağlantı havuzu uygular. Ancak hala bağlantı dışında çalışmaya devam edebilirsiniz. Daha fazla bilgi için bkz. [SQL Server bağlantı havuzu (ADO.net)](/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Entity Framework gibi bazı veri çerçeveleri, genellikle bir yapılandırma dosyasının **connectionStrings** bölümünden bağlantı dizelerini alır. Bu durumda, işlev uygulaması ayarlarınızın **bağlantı dizeleri** koleksiyonuna ve yerel projenizdeki [local.settings.jsdosyasına](functions-run-local.md#local-settings-file) açıkça SQL veritabanı bağlantı dizelerini eklemeniz gerekir. İşlev kodunuzda [SqlConnection](/dotnet/api/system.data.sqlclient.sqlconnection?view=dotnet-plat-ext-3.1) 'ın bir örneğini oluşturuyorsanız, bağlantı dizesi değerini diğer Bağlantılarınızdaki **uygulama ayarlarında** depolamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Statik istemcileri neden önerdiğimiz hakkında daha fazla bilgi için bkz. [Hatalı örnek oluşturma kötü modeli](/azure/architecture/antipatterns/improper-instantiation/).

Daha fazla Azure Işlevleri performans ipucu için bkz. [Azure işlevlerinin performansını ve güvenilirliğini iyileştirme](functions-best-practices.md).
