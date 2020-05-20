---
title: Yönetilen kimlik (Önizleme) kullanarak Cosmos DB hesabıyla bağlantı kurma
titleSuffix: Azure Cognitive Search
description: Yönetilen kimlik (Önizleme) kullanarak Cosmos DB hesaba Dizin Oluşturucu bağlantısı ayarlamayı öğrenin
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 3524f55f70ff42bd5ff800fb2bd7ab7b0e732596
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83664686"
---
# <a name="set-up-an-indexer-connection-to-a-cosmos-db-database-using-a-managed-identity-preview"></a>Yönetilen kimlik (Önizleme) kullanarak Cosmos DB veritabanına Dizin Oluşturucu bağlantısı kurma

> [!IMPORTANT] 
> Yönetilen kimlik kullanarak bir veri kaynağına bağlantı ayarlama desteği şu anda geçitli genel önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> [Bu formu](https://aka.ms/azure-cognitive-search/mi-preview-request)doldurarak önizlemeye erişim isteğinde bulabilirsiniz.

Bu sayfada, veri kaynağı nesne bağlantı dizesinde kimlik bilgileri sağlamak yerine yönetilen kimlik kullanarak bir Azure Cosmos DB veritabanına Dizin Oluşturucu bağlantısının nasıl ayarlanacağı açıklanır.

Bu özellik hakkında daha fazla bilgi edinmek için, bir dizin oluşturucunun ne olduğu ve veri kaynağınız için dizin oluşturucunun nasıl ayarlanacağı hakkında bilgi sahibi olmanız önerilir. Aşağıdaki bağlantılarda daha fazla bilgi bulabilirsiniz:
* [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
* [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)

## <a name="set-up-a-connection-using-a-managed-identity"></a>Yönetilen kimlik kullanarak bağlantı kurma

### <a name="1---turn-on-system-assigned-managed-identity"></a>1-sistem tarafından atanan yönetilen kimliği aç

Sistem tarafından atanan bir yönetilen kimlik etkinleştirildiğinde Azure, arama hizmetiniz için aynı kiracı ve abonelik içindeki diğer Azure hizmetlerinde kimlik doğrulaması yapmak üzere kullanılabilecek bir kimlik oluşturur. Daha sonra bu kimliği, dizin oluşturma sırasında verilere erişime izin veren rol tabanlı erişim denetimi (RBAC) atamaları içinde kullanabilirsiniz.

![Sistem tarafından atanan yönetilen kimliği aç](./media/search-managed-identities/turn-on-system-assigned-identity.png "Sistem tarafından atanan yönetilen kimliği aç")

**Kaydet** ' i seçtikten sonra, arama hizmetinize atanmış BIR nesne kimliği görürsünüz.

![Nesne Kimliği](./media/search-managed-identities/system-assigned-identity-object-id.png "Nesne Kimliği")
 
### <a name="2---add-a-role-assignment"></a>2-rol ataması ekleme

Bu adımda, Azure Bilişsel Arama hizmetine, Cosmos DB veritabanınızdaki verileri okuma izni verirsiniz.

1. Azure portal, dizine eklemek istediğiniz verileri içeren Cosmos DB hesabına gidin.
2. **Erişim denetimi (IAM)** seçeneğini belirleyin
3. **Ekle** ' yi seçin ve **rol ataması Ekle**

    ![Rol ataması ekle](./media/search-managed-identities/add-role-assignment-cosmos-db.png "Rol ataması ekle")

4. **Cosmos DB hesap okuyucu rolünü** seçin
5. **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak **erişim atamasını** bırak
6. Arama hizmetinizi arayın, seçin ve ardından **Kaydet** ' i seçin.

    ![Okuyucu ve veri erişimi rol ataması ekleme](./media/search-managed-identities/add-role-assignment-cosmos-db-account-reader-role.png "Okuyucu ve veri erişimi rol ataması ekleme")

### <a name="3---create-the-data-source"></a>3-veri kaynağını oluşturma

Veri **kaynağı** , verilerdeki değişiklikleri tanımlamaya yönelik verileri, kimlik bilgilerini ve ilkeleri belirtir (koleksiyonunuzdaki değiştirilen veya silinen belgeler gibi). Veri kaynağı, birden çok Dizin Oluşturucu tarafından kullanılabilmesi için bağımsız bir kaynak olarak tanımlanır.

Veri kaynağında kimlik doğrulaması yapmak için Yönetilen kimlikler kullanıldığında, **kimlik bilgileri** hesap anahtarı içermez.

[REST API](https://docs.microsoft.com/rest/api/searchservice/create-data-source)kullanarak Cosmos DB veri kaynağı nesnesi oluşturma örneği:

```
POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
Content-Type: application/json
api-key: [Search service admin key]

{
    "name": "cosmos-db-datasource",
    "type": "cosmosdb",
    "credentials": {
        "connectionString": "Database=sql-test-db;ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/cosmos-db-resource-group/providers/Microsoft.DocumentDB/databaseAccounts/my-cosmos-db-account/;"
    },
    "container": { "name": "myCollection", "query": null },
    "dataChangeDetectionPolicy": {
        "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName": "_ts"
    }
}
```    

İsteğin gövdesi, aşağıdaki alanları içermesi gereken veri kaynağı tanımını içerir:

| Alan   | Açıklama |
|---------|-------------|
| **ada** | Gereklidir. Veri kaynağı nesnenizin temsil edilebilmesi için herhangi bir ad seçin. |
|**türüyle**| Gereklidir. Olmalıdır `cosmosdb` . |
|**Credentials** | Gereklidir. <br/><br/>Yönetilen bir kimlik kullanılarak bağlanırken, **kimlik bilgileri** biçimi şu şekilde olmalıdır: *veritabanı = [veritabanı-adı]; RESOURCEID = [kaynak-kimliği-dize];(Apıkind = [api-Kind];)*<br/> <br/>RESOURCEID biçimi: *RESOURCEID =/Subscriptions/**abonelik kimliği**/ResourceGroups/**kaynak grubu adı**/Providers/Microsoft.DocumentDB/databaseAccounts/**Cosmos DB hesabınızın adı**/;*<br/><br/>SQL koleksiyonları için bağlantı dizesi bir ApiKind gerektirmez.<br/><br/>MongoDB koleksiyonları için bağlantı dizesine **Apikind = MongoDb** ekleyin. <br/><br/>Gremlin grafikleri ve Cassandra tablolarında, önizlemeye erişim sağlamak için [geçitli Dizin Oluşturucu önizlemesine](https://aka.ms/azure-cognitive-search/indexer-preview) kaydolun ve kimlik bilgilerini biçimlendirme hakkında bilgi alın.<br/>|
| **kapsayıcı** | Aşağıdaki öğeleri içerir: <br/>**ad**: gerekli. Endekslenecek veritabanı koleksiyonunun KIMLIĞINI belirtin.<br/>**sorgu**: isteğe bağlı. Rastgele bir JSON belgesini, Azure Bilişsel Arama 'in dizinetarafından kullanılabilecek düz bir şemaya düzleştirmek için bir sorgu belirtebilirsiniz.<br/>MongoDB API 'SI, Gremlin API ve Cassandra API için sorgular desteklenmez. |
| **dataChangeDetectionPolicy** | Önerilen |
|**dataDeletionDetectionPolicy** | İsteğe Bağlı |

Azure portal ve [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) , Yönetilen kimlikler bağlantı dizesini de destekler. Azure portal, bu sayfanın en üstündeki bağlantıyı kullanarak önizlemeye kaydolurken size sağlanacak bir özellik bayrağı gerektirir. 

### <a name="4---create-the-index"></a>4-dizin oluşturma

Dizin, bir belge, öznitelik ve arama deneyimini şekillendirip diğer yapıların alanlarını belirtir.

Aranabilir bir alanla dizin oluşturma `booktitle` :

```
POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
    "name" : "my-target-index",
    "fields": [
    { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    { "name": "booktitle", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
    ]
}
```

Dizinler oluşturma hakkında daha fazla bilgi için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="5---create-the-indexer"></a>5-dizin oluşturucuyu oluşturma

Dizin Oluşturucu bir veri kaynağını hedef arama diziniyle bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Dizin ve veri kaynağı oluşturulduktan sonra Dizin oluşturucuyu oluşturmaya hazırsınız demektir.

Örnek Dizin Oluşturucu tanımı:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "cosmos-db-indexer",
      "dataSourceName" : "cosmos-db-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu Dizin Oluşturucu her iki saatte bir çalışır (zamanlama aralığı "PT2H" olarak ayarlanır). Her 30 dakikada bir dizin oluşturucu çalıştırmak için, aralığı "PT30M" olarak ayarlayın. Desteklenen en kısa Aralık 5 dakikadır. Zamanlama isteğe bağlıdır-atlanırsa, Dizin Oluşturucu yalnızca bir kez oluşturulduğunda çalışır. Ancak, bir dizin oluşturucuyu dilediğiniz zaman isteğe bağlı olarak çalıştırabilirsiniz.   

Dizin Oluşturucu oluşturma API 'SI hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluştur](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Dizin Oluşturucu zamanlamalarını tanımlama hakkında daha fazla bilgi için bkz. [Azure bilişsel arama için Dizin Oluşturucu zamanlama](search-howto-schedule-indexers.md).

## <a name="see-also"></a>Ayrıca bkz.

Cosmos DB Dizin oluşturucular hakkında daha fazla bilgi edinin:
* [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
