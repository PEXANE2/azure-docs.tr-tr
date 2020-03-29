---
title: Gremlin SDK ile Azure Cosmos DB kaynak belirteçlerini kullanma
description: Kaynak belirteçleri oluşturmayı ve Grafik veritabanına erişmek için bunları nasıl kullanacağınızı öğrenin.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 42f3c7f3351bddab429489dccf28587549d76e18
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78897840"
---
# <a name="use-azure-cosmos-db-resource-tokens-with-the-gremlin-sdk"></a>Gremlin SDK ile Azure Cosmos DB kaynak belirteçlerini kullanma

Bu makalede, Gremlin SDK üzerinden Grafik veritabanına erişmek için [Azure Cosmos DB kaynak belirteçlerinin](secure-access-to-data.md) nasıl kullanılacağı açıklanmaktadır.

## <a name="create-a-resource-token"></a>Kaynak belirteci oluşturma

Apache TinkerPop Gremlin SDK kaynak belirteçleri oluşturmak için kullanmak için bir API yok. Kaynak *belirteci* terimi bir Azure Cosmos DB kavramıdır. Kaynak belirteçleri oluşturmak için [Azure Cosmos DB SDK'yı](sql-api-sdk-dotnet.md)indirin. Uygulamanızın kaynak belirteçleri oluşturması ve Grafik veritabanına erişmek için bunları kullanması gerekiyorsa, iki ayrı SDK gerekir.

Kaynak belirteçleri üzerindeki nesne modeli hiyerarşisi aşağıdaki anasatırda gösterilmiştir:

- **Azure Cosmos DB hesabı** - Onunla ilişkili bir DNS'si `contoso.gremlin.cosmos.azure.com`olan üst düzey varlık (örneğin, ).
  - **Azure Cosmos DB veritabanı**
    - **Kullanıcı**
      - **İzin**
        - **Belirteç** - Hangi eylemlerin izin verildiğini veya reddedildiğini gösteren Bir İzin nesnesi özelliği.

Kaynak belirteci aşağıdaki biçimi `"type=resource&ver=1&sig=<base64 string>;<base64 string>;"`kullanır: . Bu dize istemciler için opaktır ve değişiklik veya yorum olmadan olduğu gibi kullanılmalıdır.

```csharp
// Notice that document client is created against .NET SDK endpoint, rather than Gremlin.
DocumentClient client = new DocumentClient(
  new Uri("https://contoso.documents.azure.com:443/"), 
  "<master key>", 
  new ConnectionPolicy 
  {
    EnableEndpointDiscovery = false, 
    ConnectionMode = ConnectionMode.Direct 
  });

  // Read specific permission to obtain a token.
  // The token isn't returned during the ReadPermissionReedAsync() call.
  // The call succeeds only if database id, user id, and permission id already exist. 
  // Note that <database id> is not a database name. It is a base64 string that represents the database identifier, for example "KalVAA==".
  // Similar comment applies to <user id> and <permission id>.
  Permission permission = await client.ReadPermissionAsync(UriFactory.CreatePermissionUri("<database id>", "<user id>", "<permission id>"));

  Console.WriteLine("Obtained token {0}", permission.Token);
}
```

## <a name="use-a-resource-token"></a>Kaynak belirteci kullanma
GremlinServer sınıfını inşa ederken kaynak belirteçlerini doğrudan "parola" özelliği olarak kullanabilirsiniz.

```csharp
// The Gremlin application needs to be given a resource token. It can't discover the token on its own.
// You can obtain the token for a given permission by using the Azure Cosmos DB SDK, or you can pass it into the application as a command line argument or configuration value.
string resourceToken = GetResourceToken();

// Configure the Gremlin server to use a resource token rather than a master key.
GremlinServer server = new GremlinServer(
  "contoso.gremlin.cosmosdb.azure.com",
  port: 443,
  enableSsl: true,
  username: "/dbs/<database name>/colls/<collection name>",

  // The format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;".
  password: resourceToken);

  using (GremlinClient gremlinClient = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
  {
      await gremlinClient.SubmitAsync("g.V().limit(1)");
  }
```

Aynı yaklaşım tüm TinkerPop Gremlin SDKs çalışır.

```java
Cluster.Builder builder = Cluster.build();

AuthProperties authenticationProperties = new AuthProperties();
authenticationProperties.with(AuthProperties.Property.USERNAME,
    String.format("/dbs/%s/colls/%s", "<database name>", "<collection name>"));

// The format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;".
authenticationProperties.with(AuthProperties.Property.PASSWORD, resourceToken);

builder.authProperties(authenticationProperties);
```

## <a name="limit"></a>Sınır

Tek bir Gremlin hesabı yla sınırsız sayıda jeton verebilirsiniz. Ancak, 1 saat içinde aynı anda yalnızca en fazla 100 jeton kullanabilirsiniz. Bir uygulama saat başına belirteç sınırını aşarsa, kimlik doğrulama isteği reddedilir ve aşağıdaki hata iletisini alırsınız: "Aynı anda kullanılabilecek 100 kaynak belirteç sınırı aşılır." Yeni belirteçler için yuvaları boşaltmak için belirli belirteçleri kullanan etkin bağlantıları kapatmak işe yaramaz. Azure Cosmos DB Gremlin veritabanı altyapısı, kimlik doğrulama isteğinden hemen önceki saat boyunca benzersiz jetonları izler.

## <a name="permission"></a>İzin

Uygulamaların kaynak belirteçlerini kullanırken karşılaştıkları yaygın bir hata şudur: "İlgili istek için yetkilendirme üstbilgisinde sağlanan yetersiz izinler. Lütfen başka bir yetkilendirme üstbilgisiyle yeniden deneyin." Gremlin geçiş bir kenar veya tepe noktası yazmaya çalıştığında ancak kaynak belirteci yalnızca *Okuma* izinleri verdiğinde bu hata döndürülür. Geçişinizi aşağıdaki adımlardan herhangi birini içerip içermediğini incelemek için: *.addV()*, *.addE()*, *.drop()* veya *.property()*.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB'de [rol tabanlı erişim denetimi](role-based-access-control.md)
* Azure Cosmos DB'deki [verilere erişimi nasıl güvenli hale erdirebilirsiniz öğrenin](secure-access-to-data.md)
