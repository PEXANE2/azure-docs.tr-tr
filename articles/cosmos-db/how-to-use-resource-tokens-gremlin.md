---
title: Gremlin ile kaynak belirteçleri Azure Cosmos DB
description: Kaynak belirteçleri oluşturmayı ve Graf veritabanına erişmek için bunları kullanmayı öğrenin
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 09/06/2019
ms.author: olignat
ms.openlocfilehash: fcb18fb14cf787713735da07ca2048d0853fa46c
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806914"
---
# <a name="azure-cosmos-db-resource-tokens-with-gremlin"></a>Gremlin ile kaynak belirteçleri Azure Cosmos DB
Bu makalede, Gremlin SDK aracılığıyla Graph veritabanına erişmek için [Cosmos DB kaynak belirteçlerinin](secure-access-to-data.md) nasıl kullanılacağı açıklanmaktadır.

## <a name="create-a-resource-token"></a>Kaynak belirteci oluşturma

Inkerpop Gremlin SDK 'sının kaynak belirteçleri oluşturmak için bir API 'si yok. Kaynak belirteci bir Cosmos DB kavramıdır. Kaynak belirteçleri oluşturmak için [Azure Cosmos DB SDK 'sını](sql-api-sdk-dotnet.md)indirin. Uygulamanızın, grafik veritabanına erişmek için kaynak belirteçleri oluşturması ve bunları kullanması gerekiyorsa 2 ayrı SDK 'lar gerekir.

Kaynak belirteçlerinin üzerindeki nesne modeli hiyerarşisi:
- **Cosmos DB hesap** -kendısıyle ilişkilendirilmiş DNS olan üst düzey varlık (örneğin,`contoso.gremlin.cosmos.azure.com`
  - **Cosmos DB veritabanı**
    - **Kullanıcısını**
      - **Yetkisi**
        - *Token* - **izin nesnesinin hangi eylemlere izin verileceğini** veya reddedildiğini belirten bir özelliği.

Kaynak belirtecinin biçimi `"type=resource&ver=1&sig=<base64 string>;<base64 string>;"`vardır. Bu dize, istemciler için opaktır ve değişiklik veya yorum olmadan olduğu gibi kullanılmalıdır.

```csharp
// Notice that document client is created against .NET SDK end-point rather than Gremlin.
DocumentClient client = new DocumentClient(
  new Uri("https://contoso.documents.azure.com:443/"), 
  "<master key>", 
  new ConnectionPolicy 
  {
    EnableEndpointDiscovery = false, 
    ConnectionMode = ConnectionMode.Direct 
  });

  // Read specific permission to obtain a token.
  // Token will not be returned during ReadPermissionReedAsync() call.
  // This call will succeed only if database id, user id and permission id already exist. 
  // Note that <database id> is not a database name, it is a base64 string that represents database identifier, for example "KalVAA==".
  // Similar comment applies to <user id> and <permission id>
  Permission permission = await client.ReadPermissionAsync(UriFactory.CreatePermissionUri("<database id>", "<user id>", "<permission id>"));

  Console.WriteLine("Obtained token {0}", permission.Token);
}
```

## <a name="use-a-resource-token"></a>Kaynak belirteci kullanma
Kaynak belirteçleri, sınıf oluşturulurken `GremlinServer` doğrudan "Password" özelliği olarak kullanılabilir.

```csharp
// Gremlin application needs to be given a resource token. It can't discover the token on its own.
// Token can be obtained for a given permission using Cosmos DB SDK or passed into the application as command line argument or configuration value.
string resourceToken = GetResourceToken();

// Configure gremlin servier to use resource token rather than master key
GremlinServer server = new GremlinServer(
  "contoso.gremlin.cosmosdb.azure.com",
  port: 443,
  enableSsl: true,
  username: "/dbs/<database name>/colls/<collection name>",

  // Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
  password: resourceToken);

  using (GremlinClient gremlinClient = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
  {
      await gremlinClient.SubmitAsync("g.V().limit(1)");
  }
```

Aynı yaklaşım, tüm TinkerPop Gremlin SDK 'lerinde de geçerlidir.

```java
Cluster.Builder builder = Cluster.build();

AuthProperties authenticationProperties = new AuthProperties();
authenticationProperties.with(AuthProperties.Property.USERNAME,
    String.format("/dbs/%s/colls/%s", "<database name>", "<collection name>"));

// Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
authenticationProperties.with(AuthProperties.Property.PASSWORD, resourceToken);

builder.authProperties(authenticationProperties);
```

## <a name="limit"></a>Sınır

Tek bir Gremlin hesabı sınırsız sayıda belirteç verebilir, ancak en fazla **100** belirteç, **1 saat**içinde aynı anda kullanılabilir. Uygulama, saat başına belirteç sınırını aşarsa, kimlik doğrulama isteği hata iletisiyle `"Exceeded allowed resource token limit of 100 that can be used concurrently"`reddedilir. Yeni belirteçler için yuvaları serbest bırakmak üzere belirli belirteçlerle etkin bağlantıları kapatmak, hiçbir şekilde kullanılamaz. Cosmos DB Gremlin veritabanı altyapısı, kimlik doğrulama isteğinden önce geçen saatteki farklı belirteçleri izler.

## <a name="permission"></a>İzin

Ortak hata uygulamaları, `"Insufficient permissions provided in the authorization header for the corresponding request. Please retry with another authorization header."`kaynak belirteçleri kullanılırken ile birlikte gelir. Bu hata, Gremlin geçişi bir kenar veya köşe yazmaya çalıştığında, ancak kaynak belirteci yalnızca izin verdiğinde `Read` döndürülür. Aşağıdaki adımlardan herhangi birini içerip içermediğini denetleyin `.addV()`:, `.addE()`, `.drop()`veya `.property()`.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB [rol tabanlı erişim denetimi](role-based-access-control.md)
* Azure Cosmos DB [veri erişimini güvenli hale getirme hakkında bilgi edinin](secure-access-to-data.md)
