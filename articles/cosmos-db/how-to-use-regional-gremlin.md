---
title: Azure Cosmos DB Graph veritabanı için bölgesel uç noktalar
description: Uygulamanız için en yakın Grafik veritabanı bitiş noktasına nasıl bağlanışsüreceğinizi öğrenin
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 09/09/2019
ms.openlocfilehash: 7aa1e0aa6bbbee9d40eb0d48318a8e2908a75f9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78897857"
---
# <a name="regional-endpoints-for-azure-cosmos-db-graph-account"></a>Azure Cosmos DB Grafiği hesabı için bölgesel uç noktalar
Azure Cosmos DB Graph veritabanı, uygulamaların birden çok okuma uç noktasını kullanabilmesi için [genel olarak dağıtılır.](distribute-data-globally.md) Birden çok konumda yazma erişimi gerektiren uygulamalar [çok büyük](how-to-multi-master.md) kapasite özelliğine sahip olmalıdır.

Birden fazla bölge seçmek için nedenler:
1. **Yatay okuma ölçeklenebilirliği** - uygulama yükü arttıkça okuma trafiğini farklı Azure bölgelerine yönlendirmek akıllıca olabilir.
2. **Daha düşük gecikme gecikmesi** - en yakın Azure bölgesine okuma ve yazma trafiğini yönlendirme yaparak her geçişin ağ gecikme sini azaltabilirsiniz.

**Veri ihtisas** gereksinimi Cosmos DB hesabında Azure Kaynak Yöneticisi ilkesi ayarlayarak sağlanır. Müşteri, Cosmos DB'nin verileri çoğalttıldığı bölgeleri sınırlayabilir.

## <a name="traffic-routing"></a>Trafik yönlendirme

Cosmos DB Graph veritabanı altyapısı, her biri birden çok küme içeren birden çok bölgede çalışıyor. Her kümede yüzlerce makine vardır. Cosmos DB Graph hesabı DNS CNAME *accountname.gremlin.cosmos.azure.com* DNS bir kümenin bir kaydını çözer. Bir yük dengeleyicisinin tek bir IP adresi iç küme topolojisini gizler.

Cosmos DB Graph hesabının her bölgesi için bölgesel bir DNS CNAME kaydı oluşturulur. Bölgesel bitiş noktasının biçimi *accountname-region.gremlin.cosmos.azure.com.* Bölgesel bitiş noktasının bölge kesimi, Azure [bölge](https://azure.microsoft.com/global-infrastructure/regions) adından tüm boşlukları kaldırarak elde edilir. Örneğin, `"East US 2"` genel `"contoso"` veritabanı hesabı için bölge bir DNS CNAME *contoso-eastus2.gremlin.cosmos.azure.com*

TinkerPop Gremlin istemcisi tek bir sunucu ile çalışmak üzere tasarlanmıştır. Uygulama okuma ve yazma trafiği için genel yazılabilir DNS CNAME kullanabilirsiniz. Bölge duyarlı uygulamalar okuma trafiği için bölgesel bitiş noktası kullanmalıdır. Yalnızca belirli bir bölge yazıları kabul etmek üzere yapılandırılırsa yazma trafiği için bölgesel bitiş noktasını kullanın. 

> [!NOTE]
> Cosmos DB Graph motoru, yazma bölgesine trafik vekalet vererek okuma bölgesinde yazma işlemini kabul edebilir. Geçiş gecikmesini artırdığı ve gelecekte kısıtlamalara tabi olduğu için yalnızca okunan bölgeye yazma gönderilmesi önerilmez.

Global veritabanı hesabı CNAME her zaman geçerli bir yazma bölgesini işaret ediyor. Yazma bölgesinin sunucu tarafı tarafından başarısız olması sırasında Cosmos DB, küresel veritabanı hesabı CNAME'yi yeni bölgeye işaret etmek üzere güncelleştirir. Uygulama, başarısız olduktan sonra trafik yönlendirmeyi işleyemediyse, genel veritabanı hesabı DNS CNAME'yi kullanmalıdır.

> [!NOTE]
> Cosmos DB, arayanın coğrafi yakınlığına bağlı olarak trafiği yönlendirmez. Benzersiz uygulama ihtiyaçlarına göre doğru bölgeyi seçmek her uygulamaya kalmış.

## <a name="portal-endpoint-discovery"></a>Portal uç nokta bulma

Azure Cosmos DB Graph hesabının bölgelerin listesini almanın en kolay yolu Azure portalına genel bakış bıçaktır. Sık sık bölgeleri değiştirmeyen veya listeyi uygulama yapılandırması yoluyla güncelleştirmenin bir yolu olan uygulamalar için çalışır.

![Cosmos DB Graph hesabının bölgelerini portaldan alma](./media/how-to-use-regional-gremlin/get-end-point-portal.png )

Aşağıdaki örnek, bölgesel Gremlin bitiş noktasına erişimin genel ilkelerini göstermektedir. Uygulama, trafiği göndermek için bölge sayısını ve ilgili Gremlin istemcilerinin anlık sayısını göz önünde bulundurmalıdır.

```csharp
// Example value: Central US, West US and UK West. This can be found in the overview blade of you Azure Cosmos DB Gremlin Account. 
// Look for Write Locations in the overview blade. You can click to copy and paste.
string[] gremlinAccountRegions = new string[] {"Central US", "West US" ,"UK West"};
string gremlinAccountName = "PUT-COSMOSDB-ACCOUNT-NAME-HERE";
string gremlinAccountKey = "PUT-ACCOUNT-KEY-HERE";
string databaseName = "PUT-DATABASE-NAME-HERE";
string graphName = "PUT-GRAPH-NAME-HERE";

foreach (string gremlinAccountRegion in gremlinAccountRegions)
{
  // Convert preferred read location to the form "[acountname]-[region].gremlin.cosmos.azure.com".
  string regionalGremlinEndPoint = $"{gremlinAccountName}-{gremlinAccountRegion.ToLowerInvariant().Replace(" ", string.Empty)}.gremlin.cosmos.azure.com";

  GremlinServer regionalGremlinServer = new GremlinServer(
    hostname: regionalGremlinEndPoint, 
    port: 443,
    enableSsl: true,
    username: "/dbs/" + databaseName + "/colls/" + graphName,
    password: gremlinAccountKey);

  GremlinClient regionalGremlinClient = new GremlinClient(
    gremlinServer: regionalGremlinServer,
    graphSONReader: new GraphSON2Reader(),
    graphSONWriter: new GraphSON2Writer(),
    mimeType: GremlinClient.GraphSON2MimeType);
}
```

## <a name="sdk-endpoint-discovery"></a>SDK uç nokta bulma

Uygulama, Graph hesabı için okuma ve yazma konumlarını bulmak için [Azure Cosmos DB SDK'yı](sql-api-sdk-dotnet.md) kullanabilir. Bu konumlar, sunucu tarafında el ile yeniden yapılandırma veya otomatik arıza yoluyla herhangi bir zamanda değişebilir.

TinkerPop Gremlin SDK Cosmos DB Graph veritabanı hesap bölgelerini keşfetmek için bir API yok. Çalışma zamanı uç nokta bulma gereksinimi olan uygulamaların işlem alanında 2 ayrı SDK barındırması gerekir.

```csharp
// Depending on the version and the language of the SDK (.NET vs Java vs Python)
// the API to get readLocations and writeLocations may vary.
IDocumentClient documentClient = new DocumentClient(
    new Uri(cosmosUrl),
    cosmosPrimaryKey,
    connectionPolicy,
    consistencyLevel);

DatabaseAccount databaseAccount = await cosmosClient.GetDatabaseAccountAsync();

IEnumerable<DatabaseAccountLocation> writeLocations = databaseAccount.WritableLocations;
IEnumerable<DatabaseAccountLocation> readLocations = databaseAccount.ReadableLocations;

// Pick write or read locations to construct regional endpoints for.
foreach (string location in readLocations)
{
  // Convert preferred read location to the form "[acountname]-[region].gremlin.cosmos.azure.com".
  string regionalGremlinEndPoint = location
    .Replace("http:\/\/", string.Empty)
    .Replace("documents.azure.com:443/", "gremlin.cosmos.azure.com");
  
  // Use code from the previous sample to instantiate Gremlin client.
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB'de [veritabanı hesapları denetimi nasıl yönetilir?](how-to-manage-database-account.md)
* Azure Cosmos DB'de [yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB ile genel dağıtım - başlık altında](global-dist-under-the-hood.md)
* Azure Cosmos DB için [Azure CLI Örnekleri](cli-samples.md)
