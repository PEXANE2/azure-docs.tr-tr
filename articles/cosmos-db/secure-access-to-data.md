---
title: Azure Cosmos DB veri erişimini güvenli hale getirme hakkında bilgi edinin
description: Ana anahtarlar, salt okuma anahtarları, kullanıcılar ve izinler dahil Azure Cosmos DB erişim denetimi kavramları hakkında bilgi edinin.
author: thomasweiss
ms.author: thweiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 1349ec3fc0fe9b5db60af883558a9d0aed5ee612
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85392191"
---
# <a name="secure-access-to-data-in-azure-cosmos-db"></a>Azure Cosmos DB'de verilere erişimin güvenliğini sağlama

Bu makalede, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)depolanan verilere erişimin güvenliğini sağlamaya yönelik bir genel bakış sunulmaktadır.

Azure Cosmos DB, kullanıcıların kimliğini doğrulamak ve veri ve kaynaklarına erişim sağlamak için iki tür anahtar kullanır. 

|Anahtar türü|Kaynaklar|
|---|---|
|[Ana anahtarlar](#master-keys) |Yönetim kaynakları için kullanılır: veritabanı hesapları, veritabanları, kullanıcılar ve izinler|
|[Kaynak belirteçleri](#resource-tokens)|Uygulama kaynakları için kullanılır: kapsayıcılar, belgeler, ekler, saklı yordamlar, Tetikleyiciler ve UDF 'ler|

<a id="master-keys"></a>

## <a name="master-keys"></a>Ana anahtarlar

Ana anahtarlar veritabanı hesabının tüm yönetim kaynaklarına erişim sağlar. Ana anahtarlar:

- Hesaplara, veritabanlarına, kullanıcılara ve izinlere erişim sağlayın. 
- Kapsayıcılara ve belgelere parçalı erişim sağlamak için kullanılamaz.
- , Bir hesabın oluşturulması sırasında oluşturulur.
- Herhangi bir zamanda yeniden oluşturulabilir.

Her hesap iki ana anahtardan oluşur: birincil anahtar ve ikincil anahtar. Çift anahtarların amacı, hesap ve verilerinize sürekli erişim sağlamak için, anahtarları yeniden oluşturmak veya almak için kullanılır.

Cosmos DB hesap için iki ana anahtara ek olarak, salt okunurdur iki anahtar vardır. Salt okunurdur bu anahtarlar yalnızca hesapta okuma işlemlerine izin verir. Salt okuma anahtarları okuma izni kaynaklarına erişim sağlamaz.

Birincil, ikincil, salt okuma ve okuma/yazma ana anahtarları Azure portal kullanılarak alınabilir ve yeniden oluşturulabilir. Yönergeler için bkz. [erişim anahtarlarını görüntüleme, kopyalama ve yeniden](manage-with-cli.md#regenerate-account-key)oluşturma.

![Azure portal-gösteren NoSQL veritabanı güvenliğine erişim denetimi (ıAM)](./media/secure-access-to-data/nosql-database-security-master-key-portal.png)

### <a name="key-rotation"></a>Anahtar döndürme<a id="key-rotation"></a>

Ana anahtarınızı döndürme işlemi basittir. 

1. İkincil anahtarınızı almak için Azure portal gidin.
2. Birincil anahtarınızı uygulamanızdaki ikincil anahtarınızla değiştirin. Tüm dağıtımlardaki tüm Cosmos DB istemcilerinin hemen yeniden başlatıldığından ve güncelleştirilmiş anahtarın kullanılmasına başlayadığınızdan emin olun.
3. Azure portal birincil anahtarı döndürün.
4. Yeni birincil anahtarın tüm kaynaklara karşı çalışıp çalışmadığını doğrulayın. Anahtar döndürme işlemi, Cosmos DB hesabının boyutuna bağlı olarak bir dakikadan kısa bir süreden saate kadar olan her yerde olabilir.
5. İkincil anahtarı yeni birincil anahtarla değiştirin.

:::image type="content" source="./media/secure-access-to-data/nosql-database-security-master-key-rotate-workflow.png" alt-text="Azure portal-gösteren NoSQL veritabanı güvenliğine ana anahtar döndürme" border="false":::

### <a name="code-sample-to-use-a-master-key"></a>Ana anahtar kullanmak için kod örneği

Aşağıdaki kod örneği, bir Cosmos DB hesabı uç noktasının ve ana anahtarın bir DocumentClient örneği oluşturmak ve bir veritabanı oluşturmak için nasıl kullanılacağını göstermektedir.

```csharp
//Read the Azure Cosmos DB endpointUrl and authorization keys from config.
//These values are available from the Azure portal on the Azure Cosmos DB account blade under "Keys".
//Keep these values in a safe and secure location. Together they provide Administrative access to your Azure Cosmos DB account.

private static readonly string endpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
private static readonly string authorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];

CosmosClient client = new CosmosClient(endpointUrl, authorizationKey);
```

## <a name="resource-tokens"></a>Kaynak belirteçleri<a id="resource-tokens"></a>

Kaynak belirteçleri, bir veritabanı içindeki uygulama kaynaklarına erişim sağlar. Kaynak belirteçleri:

- Belirli kapsayıcılara, Bölüm anahtarlarına, belgelere, eklere, saklı yordamlara, tetikleyicilere ve UDF erişimine erişim sağlar.
- , Bir [kullanıcıya](#users) belirli bir kaynağa [izin](#permissions) verildiğinde oluşturulur.
- , Bir izin kaynağı GÖNDERI, al veya koy çağrısıyla üzerinde işlem yapıldığında yeniden oluşturulur.
- Kullanıcı, kaynak ve izin için özel olarak oluşturulan bir karma kaynak belirteci kullanın.
- , Özelleştirilebilir bir geçerlilik süresi ile zaman bağlalardır. Varsayılan geçerli zaman aralığı bir saattir. Ancak, belirteç ömrü, en fazla beş saate kadar açıkça belirtilebilir.
- Ana anahtarı vermek için güvenli bir alternatif sağlayın.
- İstemcilerin, izin verilen izinlere göre Cosmos DB hesabındaki kaynakları okumasını, yazmasını ve silmesini sağlar.

Ana anahtarla güvenilmeyen bir istemciye Cosmos DB hesabınızdaki kaynaklara erişim sağlamak istediğinizde bir kaynak belirteci (Cosmos DB kullanıcılar ve izinler oluşturarak) kullanabilirsiniz.  

Cosmos DB kaynak belirteçleri, istemcilerin Cosmos DB hesabınızdaki kaynakları, verdiğiniz izinlere göre okumasını, yazmasına ve silmesine olanak tanıyan güvenli bir alternatif sağlar.

Kaynak belirteçlerinin istendiği, üretibildiği ve istemcilere teslim edileceği tipik bir tasarım deseninin olması aşağıda verilmiştir:

1. Orta katman hizmeti, Kullanıcı fotoğraflarını paylaşmak üzere bir mobil uygulama sunacak şekilde ayarlanır.
2. Orta katman hizmeti Cosmos DB hesabının ana anahtarına sahiptir.
3. Fotoğraf uygulaması, Son Kullanıcı mobil cihazlarına yüklenir.
4. Oturum açıldığında, fotoğraf uygulaması, orta katman hizmeti ile kullanıcının kimliğini belirler. Bu kimlik kurulumu mekanizması, uygulamaya tamamen sahiptir.
5. Kimlik kurulduktan sonra, orta katman hizmeti kimlik temelinde izinleri ister.
6. Orta katman hizmeti, bir kaynak belirtecini telefon uygulamasına geri gönderir.
7. Telefon uygulaması, kaynak belirteci tarafından tanımlanan izinlerle ve kaynak belirtecinin izin verdiği aralığa göre Cosmos DB kaynaklarına doğrudan erişmek için kaynak belirtecini kullanmaya devam edebilir.
8. Kaynak belirtecinin süresi dolmuşsa, sonraki istekler 401 Yetkisiz bir özel durum alır.  Bu noktada, telefon uygulaması kimliği yeniden oluşturur ve yeni bir kaynak belirteci ister.

    :::image type="content" source="./media/secure-access-to-data/resourcekeyworkflow.png" alt-text="Kaynak belirteçleri Azure Cosmos DB iş akışı" border="false":::

Kaynak belirteci oluşturma ve yönetimi, yerel Cosmos DB istemci kitaplıkları tarafından gerçekleştirilir; Ancak REST kullanırsanız istek/kimlik doğrulama üst bilgilerini oluşturmanız gerekir. REST için kimlik doğrulama üstbilgileri oluşturma hakkında daha fazla bilgi için, bkz. [Access Control Cosmos DB kaynakları](/rest/api/cosmos-db/access-control-on-cosmosdb-resources) veya [.net SDK](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos/src/AuthorizationHelper.cs) veya [Node.js SDK](https://github.com/Azure/azure-cosmos-js/blob/master/src/auth.ts)'mız için kaynak kodu.

Kaynak belirteçleri oluşturmak için kullanılan bir orta katman hizmeti örneği için, bkz. [Resourcetokenbroker uygulaması](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker/Controllers).

## <a name="users"></a>Kullanıcılar<a id="users"></a>

Azure Cosmos DB kullanıcılar bir Cosmos veritabanıyla ilişkilendirilir.  Her veritabanı sıfır veya daha fazla Kullanıcı Cosmos DB içerebilir. Aşağıdaki kod örneği, [.NET SDK v3 Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/UserManagement)kullanarak Cosmos DB bir kullanıcının nasıl oluşturulacağını göstermektedir.

```csharp
//Create a user.
Database database = benchmark.client.GetDatabase("SalesDatabase");

User user = await database.CreateUserAsync("User 1");
```

> [!NOTE]
> Her Cosmos DB kullanıcının, kullanıcıyla ilişkili [izin](#permissions) listesini almak için kullanılabilecek bir ReadAsync () yöntemi vardır.

## <a name="permissions"></a>İzinler<a id="permissions"></a>

Bir izin kaynağı, bir kullanıcıyla ilişkilendirilir ve bölüm anahtarı düzeyi olarak kapsayıcıda atanır. Her Kullanıcı sıfır veya daha fazla izin içerebilir. Bir izin kaynağı, belirli bir bölüm anahtarındaki belirli bir kapsayıcıya veya verilere erişmeyi denerken kullanıcının ihtiyacı olan bir güvenlik belirtecine erişim sağlar. Bir izin kaynağı tarafından sağlanarak kullanılabilecek iki erişim düzeyi vardır:

- Tümü: kullanıcının kaynakta tam izni vardır.
- Okuma: Kullanıcı yalnızca kaynağın içeriğini okuyabilir, ancak kaynakta yazma, güncelleştirme veya silme işlemlerini gerçekleştiremez.

> [!NOTE]
> Saklı yordamları çalıştırmak için kullanıcının saklı yordamın çalıştırılacağı kapsayıcıda tüm izne sahip olması gerekir.

### <a name="code-sample-to-create-permission"></a>İzin oluşturmak için kod örneği

Aşağıdaki kod örneği, bir izin kaynağı oluşturmayı, izin kaynağının kaynak belirtecini okumayı ve izinleri yukarıda oluşturulan [kullanıcıyla](#users) ilişkilendirmeyi gösterir.

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

### <a name="code-sample-to-read-permission-for-user"></a>Kullanıcı için okuma izni için kod örneği

Aşağıdaki kod parçacığı, yukarıda oluşturulan kullanıcıyla ilişkili iznin nasıl alınacağını gösterir ve tek bir bölüm anahtarı kapsamındaki Kullanıcı adına yeni bir CosmosClient örneği oluşturur.

```csharp
//Read a permission, create user client session.
PermissionProperties permissionProperties = await user.GetPermission("permissionUser1Orders")

CosmosClient client = new CosmosClient(accountEndpoint: "MyEndpoint", authKeyOrResourceToken: permissionProperties.Token);
```

## <a name="add-users-and-assign-roles"></a>Kullanıcı ekleme ve rol atama

Kullanıcı hesabınıza Azure Cosmos DB hesap okuyucusu erişimi eklemek için bir abonelik sahibine Azure portal aşağıdaki adımları uygulayın.

1. Azure portal açın ve Azure Cosmos DB hesabınızı seçin.
2. **Erişim denetimi (IAM)** sekmesine tıklayın ve ardından **+ rol ataması Ekle**' ye tıklayın.
3. **Rol ataması Ekle** bölmesinde, **rol** kutusunda **Cosmos DB hesap okuyucu rolü**' nü seçin.
4. **Erişim ata kutusunda** **Azure AD Kullanıcı, Grup veya uygulama**' yı seçin.
5. Dizininizde erişim vermek istediğiniz kullanıcı, Grup veya uygulamayı seçin.  Dizinde görünen ad, e-posta adresi veya nesne tanımlayıcıları ile arama yapabilirsiniz.
    Seçilen Kullanıcı, Grup veya uygulama seçilen Üyeler listesinde görünür.
6. **Kaydet**’e tıklayın.

Varlık artık Azure Cosmos DB kaynaklarını okuyabilir.

## <a name="delete-or-export-user-data"></a>Kullanıcı verilerini silme veya dışarı aktarma

Azure Cosmos DB, veritabanı veya koleksiyonlardaki tüm kişisel verileri aramanıza, seçmenize, değiştirmenize ve silmenizi sağlar. Azure Cosmos DB, kişisel verileri bulmak ve silmek için API 'Ler sağlar. Bu, API 'Leri kullanmak ve kişisel verileri silmek için gerekli mantığı tanımlamak sizin sorumluluğunuzdadır. Her bir çok modelli API (SQL, MongoDB, Gremlin, Cassandra, tablo), kişisel verileri aramak ve silmek için yöntemler içeren farklı dil SDK 'leri sağlar. Ayrıca, belirli bir süreden sonra verileri otomatik olarak silmek için [yaşam süresi (TTL)](time-to-live.md) özelliğini de etkinleştirebilirsiniz.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Cosmos veritabanı güvenliği hakkında daha fazla bilgi edinmek için bkz. [Cosmos DB veritabanı güvenliği](database-security.md).
- Azure Cosmos DB yetkilendirme belirteçleri oluşturmayı öğrenmek için [Azure Cosmos DB kaynaklarında Access Control](/rest/api/cosmos-db/access-control-on-cosmosdb-resources)bakın.
- Kullanıcılar ve izinlerle Kullanıcı yönetimi örnekleri, [.NET SDK V3 Kullanıcı yönetimi örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/UserManagement/UserManagementProgram.cs)
