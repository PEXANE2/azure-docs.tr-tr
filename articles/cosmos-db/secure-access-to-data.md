---
title: Azure Cosmos DB'deki verilere erişimi nasıl güvenli hale erdirebilirsiniz öğrenin
description: Azure Cosmos DB'deki erişim denetimi kavramları hakkında (ana anahtarlar, salt okunur tuşlar, kullanıcılar ve izinler dahil) hakkında bilgi edinin.
author: thomasweiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 448b14168e85e75b7ed19e189600186ce11c2902
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251823"
---
# <a name="secure-access-to-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de verilere erişimin güvenliğini sağlama

Bu makale, [Microsoft Azure Cosmos DB'de](https://azure.microsoft.com/services/cosmos-db/)depolanan verilere erişimi güvence altına almaya genel bir bakış sağlar.

Azure Cosmos DB, kullanıcıların kimliğini doğrulamak ve verilerine ve kaynaklarına erişim sağlamak için iki tür anahtar kullanır. 

|Anahtar türü|Kaynaklar|
|---|---|
|[Ana anahtarlar](#master-keys) |Yönetim kaynakları için kullanılır: veritabanı hesapları, veritabanları, kullanıcılar ve izinler|
|[Kaynak belirteçleri](#resource-tokens)|Uygulama kaynakları için kullanılır: kapsayıcılar, belgeler, ekler, depolanan yordamlar, tetikleyiciler ve UDF'ler|

<a id="master-keys"></a>

## <a name="master-keys"></a>Ana anahtarlar

Ana anahtarlar veritabanı hesabının tüm yönetim kaynaklarına erişim sağlar. Ana anahtarlar:

- Hesaplara, veritabanlarına, kullanıcılara ve izinlere erişim sağlayın. 
- Kapsayıcılara ve belgelere parçalı erişim sağlamak için kullanılamaz.
- Bir hesap oluşturulur sırasında oluşturulur.
- Herhangi bir zamanda yenilenebilir.

Her hesap iki Ana anahtardan oluşur: birincil anahtar ve ikincil anahtar. Çift anahtarların amacı, hesabınıza ve verilerinize sürekli erişim sağlayarak anahtarları yeniden oluşturabilmeniz veya yuvarlamanızdır.

Cosmos DB hesabının iki ana anahtarına ek olarak, salt okunur iki anahtar vardır. Salt okunur bu anahtarlar yalnızca hesaptaki okuma işlemlerine izin verir. Salt okunur anahtarlar okuma izinlerine erişim sağlamaz.

Birincil, ikincil, salt okunur ve okundu-yazma ana anahtarları Azure portalı kullanılarak alınabilir ve yeniden oluşturulabilir. Yönergeler için [bkz.](manage-with-cli.md#regenerate-account-key)

![Azure portalında erişim denetimi (IAM) - NoSQL veritabanı güvenliğini gösterme](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

Ana anahtarınızı döndürme işlemi basittir. İkincil anahtarınızı almak için Azure portalına gidin, ardından uygulamanızdaki birincil anahtarınızı ikincil anahtarınızla değiştirin ve ardından Azure portalındaki birincil anahtarı döndürün.

![Azure portalında ana anahtar döndürme - NoSQL veritabanı güvenliğini gösterme](./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png)

### <a name="code-sample-to-use-a-master-key"></a>Ana anahtar kullanmak için kod örneği

Aşağıdaki kod örneği, DocumentClient'ı anında kullanmak ve bir veritabanı oluşturmak için Cosmos DB hesap bitiş noktası nın ve ana anahtarın nasıl kullanılacağını göstermektedir.

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//Keep these values in a safe and secure location. Together they provide Administrative access to your Azure Cosmos DB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly string authorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];

CosmosClient client = new CosmosClient(endpointUrl, authorizationKey);
```

## <a name="resource-tokens"></a>Kaynak belirteçleri<a id="resource-tokens"></a>

Kaynak belirteçleri, veritabanıiçindeki uygulama kaynaklarına erişim sağlar. Kaynak belirteçleri:

- Belirli kapsayıcılara, bölme anahtarlarına, belgelere, eklere, depolanan yordamlara, tetikleyicilere ve UDF'lere erişim sağlayın.
- Bir [kullanıcıya](#users) belirli bir kaynağa [izin](#permissions) verildiğinde oluşturulur.
- Bir izin kaynağı POST, GET veya PUT çağrısı tarafından harekete geçirildiğinde yeniden oluşturulur.
- Kullanıcı, kaynak ve izin için özel olarak oluşturulmuş karma kaynak belirteci kullanın.
- Zaman özelleştirilebilir bir geçerlilik süresi ile bağlıdır. Varsayılan geçerli zaman aralığı bir saattir. Ancak, belirteç ömrü en fazla beş saate kadar açıkça belirtilebilir.
- Ana anahtarı vermek için güvenli bir alternatif sağlayın.
- İstemcilerin Cosmos DB hesabındaki kaynakları, verilen izinlere göre okumalarını, yazmalarını ve silmelerini sağlar.

Cosmos DB hesabınızdaki kaynaklara ana anahtarla güvenilemeyen bir istemciye erişim sağlamak istediğinizde (Cosmos DB kullanıcıları ve izinleri oluşturarak) bir kaynak belirteci kullanabilirsiniz.  

Cosmos DB kaynak belirteçleri, müşterilerin Cosmos DB hesabınızdaki kaynakları verdiğiniz izinlere göre ve bir ana veya yalnızca anahtarı okumaya gerek kalmadan okumasına, yazmasına ve silebilmesini sağlayan güvenli bir alternatif sağlar.

Kaynak belirteçlerinin istenebileceği, oluşturulabileceği ve istemcilere teslim edilebildiği tipik bir tasarım deseni aşağıda veda eder:

1. Kullanıcı fotoğraflarını paylaşmak için bir mobil uygulama sunmak üzere orta katman bir hizmet ayarlanır.
2. Orta katman hizmeti, Cosmos DB hesabının ana anahtarına sahip.
3. Fotoğraf uygulaması son kullanıcı mobil cihazlara yüklenir.
4. Oturum açmada, fotoğraf uygulaması orta katman hizmeti ile kullanıcının kimliğini belirler. Kimlik kurma mekanizması tamamen uygulamaya bağlıdır.
5. Kimlik oluşturulduktan sonra, orta katman hizmet kimliğe dayalı izinler ister.
6. Orta katman hizmeti, telefon uygulamasına bir kaynak belirteci gönderir.
7. Telefon uygulaması, kaynak belirteci tarafından tanımlanan izinlerle ve kaynak belirteci tarafından izin verilen aralıkla Cosmos DB kaynaklarına doğrudan erişmek için kaynak belirteci kullanmaya devam edebilir.
8. Kaynak belirteci süresi dolduğunda, sonraki istekler 401 yetkisiz özel durum alır.  Bu noktada, telefon uygulaması kimliği yeniden belirler ve yeni bir kaynak belirteci ister.

    ![Azure Cosmos DB kaynak belirteçleri iş akışı](./media/secure-access-to-data/resourcekeyworkflow.png)

Kaynak belirteç oluşturma ve yönetimi yerel Cosmos DB istemci kitaplıkları tarafından işlenir; ancak, REST kullanıyorsanız istek/kimlik doğrulama üstbilgisini oluşturmanız gerekir. REST için kimlik doğrulama üstbilgileri oluşturma hakkında daha fazla bilgi için [Cosmos DB Kaynaklarında Erişim Denetimi'ne](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources) veya [.NET SDK](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos/src/AuthorizationHelper.cs) veya [Node.js SDK'mızın](https://github.com/Azure/azure-cosmos-js/blob/master/src/auth.ts)kaynak koduna bakın.

Kaynak belirteçleri oluşturmak veya aracılık etmek için kullanılan bir orta katman hizmeti örneği için [ResourceTokenBroker uygulamasına](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers)bakın.

## <a name="users"></a>Kullanıcı<a id="users"></a>

Azure Cosmos DB kullanıcıları bir Cosmos veritabanıyla ilişkilidir.  Her veritabanı sıfır veya daha fazla Cosmos DB kullanıcısı içerebilir. Aşağıdaki kod örneği, [Azure Cosmos DB .NET SDK v3'ü](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/UserManagement)kullanarak bir Cosmos DB kullanıcısının nasıl oluşturulacak olduğunu gösterir.

```csharp
//Create a user.
Database database = benchmark.client.GetDatabase("SalesDatabase");

User user = await database.CreateUserAsync("User 1");
```

> [!NOTE]
> Her Cosmos DB kullanıcısı, kullanıcıyla ilişkili [izinlerin](#permissions) listesini almak için kullanılabilecek bir ReadAsync() yöntemine sahiptir.

## <a name="permissions"></a>Izin<a id="permissions"></a>

İzin kaynağı bir kullanıcıyla ilişkilidir ve kapsayıcının yanı sıra bölüm anahtar düzeyinde atanır. Her kullanıcı sıfır veya daha fazla izin içerebilir. İzin kaynağı, belirli bir kapsayıcıya veya belirli bir bölüm anahtarındaki verilere erişmeye çalışırken kullanıcının ihtiyaç duyduğu bir güvenlik belirtecine erişim sağlar. İzin kaynağı tarafından sağlanabilecek iki kullanılabilir erişim düzeyi vardır:

- Tüm: Kullanıcı kaynak üzerinde tam izine sahiptir.
- Oku: Kullanıcı yalnızca kaynağın içeriğini okuyabilir, ancak kaynak üzerindeki yazma, güncelleme veya silme işlemlerini gerçekleştiremez.

> [!NOTE]
> Saklanan yordamları çalıştırmak için kullanıcının, depolanan yordamın çalıştırılacak kapsayıcıda Tüm iznine sahip olması gerekir.

### <a name="code-sample-to-create-permission"></a>İzin oluşturmak için kod örneği

Aşağıdaki kod örneği, izin kaynağının nasıl oluşturulup oluşturulacak, izin kaynağının kaynak belirteci nin nasıl okunduğunu ve izinlerin yukarıda oluşturulan [kullanıcıyla](#users) nasıl ilişkilendirileni gösterir.

```csharp
// Create a permission on a container and specific partition key value
Container container = client.GetContainer("SalesDatabase", "OrdersContainer");
user.CreatePermissionAsync(
    new PermissionProperties(
        id: "permissionUser1Orders",
        permissionMode: PermissionMode.All,
        container: benchmark.container,
        resourcePartitionKey: new PartitionKey("012345")));
```

### <a name="code-sample-to-read-permission-for-user"></a>Kullanıcı için izin okumak için kod örneği

Aşağıdaki kod snippet yukarıda oluşturulan kullanıcı ile ilişkili izin almak ve kullanıcı adına yeni bir CosmosClient anlık, tek bir bölüm anahtarı na mı kapsanan gösterir.

```csharp
//Read a permission, create user client session.
PermissionProperties permissionProperties = await user.GetPermission("permissionUser1Orders")

CosmosClient client = new CosmosClient(accountEndpoint: "MyEndpoint", authKeyOrResourceToken: permissionProperties.Token);
```

## <a name="add-users-and-assign-roles"></a>Kullanıcı ekleme ve roller atama

Azure Cosmos DB hesap okuyucu erişimini kullanıcı hesabınıza eklemek için, bir abonelik sahibinin Azure portalında aşağıdaki adımları gerçekleştirmesini sağlar.

1. Azure portalını açın ve Azure Cosmos DB hesabınızı seçin.
2. Access **denetimi (IAM)** sekmesini tıklatın ve sonra **+ Rol Atamaekle'yi**tıklatın.
3. Rol **atama ekle** bölmesinde, **Rol** kutusunda **Cosmos DB Hesap Okuyucu Rolü'nü**seçin.
4. **Kutuya Erişim Atla'da** **Azure AD kullanıcısını, grubunu veya uygulamasını**seçin.
5. Dizininizde erişim izni vermek istediğiniz kullanıcıyı, grubu veya uygulamayı seçin.  Dizinde görüntü adına, e-posta adresine veya nesne tanımlayıcılarıile arama yapabilirsiniz.
    Seçili kullanıcı, grup veya uygulama seçili üyeler listesinde görünür.
6. **Kaydet**'e tıklayın.

Varlık artık Azure Cosmos DB kaynaklarını okuyabilir.

## <a name="delete-or-export-user-data"></a>Kullanıcı verilerini silme veya dışa aktarma

Azure Cosmos DB, veritabanında veya koleksiyonlarda bulunan kişisel verileri aramanızı, seçmenize, değiştirmenize ve silmenize olanak tanır. Azure Cosmos DB, kişisel verileri bulmak ve silmek için API'ler sağlar, ancak KIŞISEL verileri silmek için gerekli API'leri kullanmak ve mantığı tanımlamak sizin sorumluluğunuzdadır. Her çok modelli API (SQL, MongoDB, Gremlin, Cassandra, Tablo) kişisel verileri arama ve silme yöntemleri içeren farklı dil SDK'ları sağlar. Ayrıca, belirli bir süre sonra, herhangi bir ek ücret ödemeden verileri otomatik olarak silmek [için canlı (TTL) özelliğinin süresini](time-to-live.md) de etkinleştirebilirsiniz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Cosmos veritabanı güvenliği hakkında daha fazla bilgi edinmek için [Cosmos DB Veritabanı güvenliğine](database-security.md)bakın.
- Azure Cosmos DB yetkilendirme belirteçlerini nasıl oluşturacağız öğrenmek için Azure [Cosmos DB Kaynaklarında Erişim Denetimi'ne](https://docs.microsoft.com/rest/api/cosmos-db/access-control-on-cosmosdb-resources)bakın.
- Kullanıcı ve izinleri olan kullanıcı yönetimi [örnekleri, .NET SDK v3 kullanıcı yönetimi örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/UserManagement/UserManagementProgram.cs)
