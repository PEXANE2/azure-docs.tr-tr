---
title: Quickstart - Azure Cosmos DB SQL API kaynaklarını yönetmek için bir .NET konsol uygulaması oluşturun
description: Bu hızlı başlangıçta Azure Cosmos DB SQL API hesap kaynaklarını yönetmek için bir .NET konsol uygulaması oluşturmayı öğrenin.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/12/2019
ms.openlocfilehash: 0981ed30c6bcd9d4246ce1eb047aa66168e3884a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240418"
---
# <a name="quickstart-build-a-net-console-app-to-manage-azure-cosmos-db-sql-api-resources"></a>Hızlı başlangıç: Azure Cosmos DB SQL API kaynaklarını yönetmek için bir .NET konsol uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

.NET için Azure Cosmos DB SQL API istemci kitaplığı ile başlayın. .NET paketini yüklemek, bir uygulama oluşturmak ve Azure Cosmos DB'de depolanan verilerdeki temel CRUD işlemleri için örnek kodu denemek için bu dokümandaki adımları izleyin. 

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Anahtar/değer, belge ve grafik veritabanlarını hızla oluşturmak ve sorgulamak için Azure Cosmos DB'yi kullanabilirsiniz. Şu şekilde .NET için Azure Cosmos DB SQL API istemci kitaplığını kullanın:

* Azure Cosmos veritabanı ve kapsayıcı oluşturma
* Kapsayıcıya örnek veri ekleme
* Verileri sorgulama 
* Veritabanını silme

[API başvuru belgeleri](/dotnet/api/microsoft.azure.cosmos?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-cosmos-dotnet-v3) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Cosmos)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/) veya Azure aboneliği olmadan ücretsiz, ücretsiz ve taahhütler için Azure [Cosmos DB'yi ücretsiz olarak deneyebilirsiniz.](https://azure.microsoft.com/try/cosmosdb/) 
* [.NET Core 2.1 SDK veya sonrası](https://dotnet.microsoft.com/download/dotnet-core/2.1).

## <a name="setting-up"></a>Ayarlama

Bu bölüm, bir Azure Cosmos hesabı oluşturmanız ve kaynakları yönetmek için .NET için Azure Cosmos DB SQL API istemci kitaplığını kullanan bir proje oluşturmanız için size yol kat maktadır. Bu makalede açıklanan örnek kod, `FamilyDatabase` bu veritabanıiçinde bir veritabanı ve aile üyeleri (her aile üyesi bir öğedir) oluşturur. Her aile üyesinin `Id, FamilyName, FirstName, LastName, Parents, Children, Address,`. Özellik `LastName` kapsayıcı için bölme anahtarı olarak kullanılır. 

### <a name="create-an-azure-cosmos-account"></a><a id="create-account"></a>Azure Cosmos hesabı oluşturma

Azure Cosmos hesabı oluşturmak için ücretsiz seçenek için [Azure Cosmos DB'yi deneyin'i](https://azure.microsoft.com/try/cosmosdb/) kullanıyorsanız, **SQL API**türünden bir Azure Cosmos DB hesabı oluşturmanız gerekir. Sizin için zaten bir Azure Cosmos DB test hesabı oluşturuldu. Hesabı açıkça oluşturmanız gerekmesin, böylece bu bölümü atlayıp bir sonraki bölüme geçebilirsiniz.

Kendi Azure aboneliğiniz varsa veya ücretsiz bir abonelik oluşturduysanız, açıkça bir Azure Cosmos hesabı oluşturmanız gerekir. Aşağıdaki kod, oturum tutarlılığı olan bir Azure Cosmos hesabı oluşturur. Hesap çoğaltılır `South Central US` ve `North Central US`.  

Azure Cosmos hesabını oluşturmak için Azure Bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell, Azure kaynaklarını yönetmek için kullanabileceğiniz etkileşimli, kimliği doğrulanmış ve tarayıcı ile erişilebilen bir kabuktur. Bu hizmet, Bash veya PowerShell ile çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği getirir. Bu hızlı başlangıç için **Bash** modunu seçin. Azure Cloud Shell de bir depolama hesabı gerektirir, istendiğinde bir tane oluşturabilirsiniz.

Aşağıdaki kodun yanındaki **Try It** düğmesini seçin, **Bash** modunu seçin bir depolama **hesabı oluşturun** ve Cloud Shell'e giriş yapın. Sonraki kopyalayıp aşağıdaki kodu Azure bulut kabuğuna yapıştırın ve çalıştırın. Azure Cosmos hesap adı genel olarak benzersiz olmalıdır, `mysqlapicosmosdb` komutu çalıştırmadan önce değeri güncelleştirdiğinizden emin olun.

```azurecli-interactive

# Set variables for the new SQL API account, database, and container
resourceGroupName='myResourceGroup'
location='southcentralus'

# The Azure Cosmos account name must be globally unique, make sure to update the `mysqlapicosmosdb` value before you run the command
accountName='mysqlapicosmosdb'

# Create a resource group
az group create \
    --name $resourceGroupName \
    --location $location

# Create a SQL API Cosmos DB account with session consistency and multi-master enabled
az cosmosdb create \
    --resource-group $resourceGroupName \
    --name $accountName \
    --kind GlobalDocumentDB \
    --locations regionName="South Central US" failoverPriority=0 --locations regionName="North Central US" failoverPriority=1 \
    --default-consistency-level "Session" \
    --enable-multiple-write-locations true

```

Azure Cosmos hesabının oluşturulması biraz zaman alır, işlem başarılı olduğunda onay çıktısını görebilirsiniz. Komut başarıyla tamamlandıktan sonra [Azure portalında](https://portal.azure.com/) oturum açın ve belirtilen ada sahip Azure Cosmos hesabının var olduğunu doğrulayın. Kaynak oluşturulduktan sonra Azure Bulut Bulutu penceresini kapatabilirsiniz. 

### <a name="create-a-new-net-app"></a><a id="create-dotnet-core-app"></a>Yeni bir .NET uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET uygulaması oluşturun. Yerel bilgisayarınızdan Windows komut istemini veya Terminal penceresini açın. Komut istemi veya terminalsonraki bölümlerdeki tüm komutları çalıştırın.  Adı `todo`ile yeni bir uygulama oluşturmak için aşağıdaki dotnet yeni komutu çalıştırın. --langVersion parametresi, oluşturulan proje dosyasındaki LangVersion özelliğini ayarlar.

```console
dotnet new console --langVersion 7.1 -n todo
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
cd todo
dotnet build
```

Yapıdan beklenen çıktı şuna benzer:

```console
  Restore completed in 100.37 ms for C:\Users\user1\Downloads\CosmosDB_Samples\todo\todo.csproj.
  todo -> C:\Users\user1\Downloads\CosmosDB_Samples\todo\bin\Debug\netcoreapp2.2\todo.dll
  todo -> C:\Users\user1\Downloads\CosmosDB_Samples\todo\bin\Debug\netcoreapp2.2\todo.Views.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:34.17
```

### <a name="install-the-azure-cosmos-db-package"></a><a id="install-package"></a>Azure Cosmos DB paketini yükleme

Uygulama dizinindeyken, dotnet ekle paket komutunu kullanarak .NET Core için Azure Cosmos DB istemci kitaplığını yükleyin.

```console
dotnet add package Microsoft.Azure.Cosmos
```

### <a name="copy-your-azure-cosmos-account-credentials-from-the-azure-portal"></a>Azure Cosmos hesap kimlik bilgilerinizi Azure portalından kopyalama

Örnek uygulamanın Azure Cosmos hesabınızda kimliğinin doğrulanması gerekir. Kimlik doğrulaması yapmak için Azure Cosmos hesap kimlik bilgilerini uygulamaya geçirmeniz gerekir. Aşağıdaki adımları izleyerek Azure Cosmos hesap kimlik bilgilerinizi alın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure Cosmos hesabınıza gidin.

1. **Keys** bölmesini açın ve hesabınızın **URI** ve **BIRINCIL ANAHTARINI** kopyalayın. Bir sonraki adımda URI ve tuş değerlerini bir ortam değişkenine eklersiniz.

### <a name="set-the-environment-variables"></a>Ortam değişkenlerini ayarlama

Hesabınızın **URI** ve **BIRINCIL ANAHTARINI** kopyaladıktan sonra, bunları uygulamayı çalıştıran yerel makinede yeni bir ortam değişkenine kaydedin. Ortam değişkenini ayarlamak için bir konsol penceresi açın ve aşağıdaki komutu çalıştırın. Değiştirdiğinizden `<Your_Azure_Cosmos_account_URI>` ve `<Your_Azure_Cosmos_account_PRIMARY_KEY>` değer bmutlaka.

**Windows**

```console
setx EndpointUrl "<Your_Azure_Cosmos_account_URI>"
setx PrimaryKey "<Your_Azure_Cosmos_account_PRIMARY_KEY>"
```

**Linux**

```bash
export EndpointUrl = "<Your_Azure_Cosmos_account_URI>"
export PrimaryKey = "<Your_Azure_Cosmos_account_PRIMARY_KEY>"
```

**Macos**

```bash
export EndpointUrl = "<Your_Azure_Cosmos_account_URI>"
export PrimaryKey = "<Your_Azure_Cosmos_account_PRIMARY_KEY>"
```

 ## <a name="object-model"></a><a id="object-model"></a>Nesne modeli

Uygulamayı oluşturmaya başlamadan önce, Azure Cosmos DB'deki kaynaklar hiyerarşisini ve bu kaynakları oluşturmak ve bunlara erişmek için kullanılan nesne modelini inceleyelim. Azure Cosmos DB aşağıdaki sırada kaynak oluşturur:

* Azure Cosmos hesabı 
* Veritabanları 
* Kapsayıcılar 
* Öğeler

Farklı varlıkların hiyerarşisi hakkında daha fazla bilgi edinmek için [Azure Cosmos DB makalesinde veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) bölümüne bakın. Bu kaynaklarla etkileşim kurmak için aşağıdaki .NET sınıflarını kullanırsınız:

* [CosmosClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient?view=azure-dotnet) - Bu sınıf, Azure Cosmos DB hizmeti için istemci tarafı mantıksal gösterimi sağlar. İstemci nesnesi, istekleri hizmete karşı yapılandırmak ve yürütmek için kullanılır.

* [CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.cosmosclient.createdatabaseifnotexistsasync?view=azure-dotnet) - Bu yöntem (yoksa) oluşturur veya (zaten varsa) bir veritabanı kaynağı asynchronous işlem olarak alır. 

* [CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet)- - Bu yöntem (yoksa) oluşturur veya (zaten varsa) bir kapsayıcı bir eşzamanlı işlem olarak alır. Kapsayıcının yeni oluşturulduğunu (201) veya varolan bir kapsayıcının döndürülüp döndürülmediğini (200) belirlemek için yanıttan durum kodunu denetleyebilirsiniz. 
* [CreateItemAsync](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet) - Bu yöntem kapsayıcı içinde bir öğe oluşturur. 

* [UpsertItemAsync](/dotnet/api/microsoft.azure.cosmos.container.upsertitemasync?view=azure-dotnet) - Bu yöntem, zaten yoksa kapsayıcı içinde bir öğe oluşturur veya zaten varsa öğenin yerini alır. 

* [GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.GetItemQueryIterator?view=azure-dotnet
) - Bu yöntem, parametreli değerlere sahip bir SQL deyimi kullanarak Azure Cosmos veritabanında bir kapsayıcı altında öğeler için bir sorgu oluşturur. 

* [DeleteAsync](/dotnet/api/microsoft.azure.cosmos.database.deleteasync?view=azure-dotnet) - Belirtilen veritabanını Azure Cosmos hesabınızdan siler. `DeleteAsync`yöntem yalnızca veritabanını siler. `Cosmosclient` Örneğin atılması ayrı ayrı gerçekleşmelidir (deletedatabaseAndCleanupAsync yönteminde yapar. 

 ## <a name="code-examples"></a><a id="code-examples"></a>Kod örnekleri

Bu makalede açıklanan örnek kod, Azure Cosmos DB'de bir aile veritabanı oluşturur. Aile veritabanı, ad, adres, konum, ilişkili ebeveynler, çocuklar ve evcil hayvanlar gibi aile ayrıntılarını içerir. Verileri Azure Cosmos hesabınıza doldurmadan önce bir aile öğesinin özelliklerini tanımlayın. Örnek uygulamanızın `Family.cs` kök düzeyinde adlandırılmış yeni bir sınıf oluşturun ve bu sınıfa aşağıdaki kodu ekleyin:

```csharp
using Newtonsoft.Json;

namespace todo
{
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        // The ToString() method is used to format the output, it's used for demo purpose only. It's not required by Azure Cosmos DB
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }
}
```

### <a name="add-the-using-directives--define-the-client-object"></a>İstemci nesnesini tanımlamak & kullanarak yönergeleri ekleme

Proje dizininden, düzenleyicinizdeki `Program.cs` dosyayı açın ve uygulamanızın üst kısmındaki yönergeleri kullanarak aşağıdaki yönergeleri ekleyin:

```csharp

using System;
using System.Threading.Tasks;
using System.Configuration;
using System.Collections.Generic;
using System.Net;
using Microsoft.Azure.Cosmos;
```

**Program.cs** dosyaya, önceki adımda ayarladığınız ortam değişkenlerini okumak için kod ekleyin. " `CosmosClient`ve `Database`nesneleri `Container` tanımlayın. Sonraki Azure Cosmos hesap kaynaklarını `GetStartedDemoAsync` yönettiğiniz yöntemi çağıran ana yönteme kod ekleyin. 

```csharp
namespace todo
{
public class Program
{

    /// The Azure Cosmos DB endpoint for running this GetStarted sample.
    private string EndpointUrl = Environment.GetEnvironmentVariable("EndpointUrl");

    /// The primary key for the Azure DocumentDB account.
    private string PrimaryKey = Environment.GetEnvironmentVariable("PrimaryKey");

    // The Cosmos client instance
    private CosmosClient cosmosClient;

    // The database we will create
    private Database database;

    // The container we will create.
    private Container container;

    // The name of the database and container we will create
    private string databaseId = "FamilyDatabase";
    private string containerId = "FamilyContainer";

    public static async Task Main(string[] args)
    {
       try
       {
           Console.WriteLine("Beginning operations...\n");
           Program p = new Program();
           await p.GetStartedDemoAsync();

        }
        catch (CosmosException de)
        {
           Exception baseException = de.GetBaseException();
           Console.WriteLine("{0} error occurred: {1}", de.StatusCode, de);
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: {0}", e);
        }
        finally
        {
            Console.WriteLine("End of demo, press any key to exit.");
            Console.ReadKey();
        }
    }
}
}
```

### <a name="create-a-database"></a>Veritabanı oluşturma 

`CreateDatabaseAsync` Yöntemsınıf içinde `program.cs` tanımlayın. Bu yöntem, `FamilyDatabase` if'in zaten var olmadığını oluşturur.

```csharp
private async Task CreateDatabaseAsync()
{
    // Create a new database
    this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
    Console.WriteLine("Created Database: {0}\n", this.database.Id);
}
```

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

`CreateContainerAsync` Yöntemsınıf içinde `program.cs` tanımlayın. Bu yöntem, `FamilyContainer` if'in zaten var olmadığını oluşturur. 

```csharp
/// Create the container if it does not exist. 
/// Specifiy "/LastName" as the partition key since we're storing family information, to ensure good distribution of requests and storage.
private async Task CreateContainerAsync()
{
    // Create a new container
    this.container = await this.database.CreateContainerIfNotExistsAsync(containerId, "/LastName");
    Console.WriteLine("Created Container: {0}\n", this.container.Id);
}
```

### <a name="create-an-item"></a>Öğe oluşturma

`AddItemsToContainerAsync` Yöntemi aşağıdaki kodla ekleyerek bir aile öğesi oluşturun. Bir öğe `CreateItemAsync` oluşturmak `UpsertItemAsync` için veya yöntemleri kullanabilirsiniz:

```csharp
private async Task AddItemsToContainerAsync()
{
    // Create a family object for the Andersen family
    Family andersenFamily = new Family
    {
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
           new Parent { FirstName = "Thomas" },
           new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
           new Child
            {
                FirstName = "Henriette Thaulow",
                Gender = "female",
                Grade = 5,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Fluffy" }
                }
            }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = false
    };

    try
    {
        // Create an item in the container representing the Andersen family. Note we provide the value of the partition key for this item, which is "Andersen".
        ItemResponse<Family> andersenFamilyResponse = await this.container.CreateItemAsync<Family>(andersenFamily, new PartitionKey(andersenFamily.LastName));
        // Note that after creating the item, we can access the body of the item with the Resource property of the ItemResponse. We can also access the RequestCharge property to see the amount of RUs consumed on this request.
        Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", andersenFamilyResponse.Resource.Id, andersenFamilyResponse.RequestCharge);
    }
    catch (CosmosException ex) when (ex.StatusCode == HttpStatusCode.Conflict)
    {
        Console.WriteLine("Item in database with id: {0} already exists\n", andersenFamily.Id);                
    }
}

```

### <a name="query-the-items"></a>Öğeleri sorgula

Bir öğe ekledikten sonra, "Andersen" ailesinin ayrıntılarını almak için bir sorgu çalıştırabilirsiniz. Aşağıdaki kod, doğrudan SQL sorgusunu kullanarak sorgunun nasıl yürütüleceklerini gösterir. "Anderson" aile bilgilerini almak için SQL `SELECT * FROM c WHERE c.LastName = 'Andersen'`sorgusu: . Sınıf `QueryItemsAsync` içindeki yöntemi `program.cs` tanımlayın ve buna aşağıdaki kodu ekleyin:


```csharp
private async Task QueryItemsAsync()
{
    var sqlQueryText = "SELECT * FROM c WHERE c.LastName = 'Andersen'";

    Console.WriteLine("Running query: {0}\n", sqlQueryText);

    QueryDefinition queryDefinition = new QueryDefinition(sqlQueryText);
    FeedIterator<Family> queryResultSetIterator = this.container.GetItemQueryIterator<Family>(queryDefinition);

    List<Family> families = new List<Family>();

    while (queryResultSetIterator.HasMoreResults)
    {
        FeedResponse<Family> currentResultSet = await queryResultSetIterator.ReadNextAsync();
        foreach (Family family in currentResultSet)
        {
            families.Add(family);
            Console.WriteLine("\tRead {0}\n", family);
        }
    }
}

```

### <a name="delete-the-database"></a>Veritabanını silme 

Son olarak aşağıdaki kodu `DeleteDatabaseAndCleanupAsync` ile yöntem ekleyerek veritabanı silebilir:

```csharp
private async Task DeleteDatabaseAndCleanupAsync()
{
   DatabaseResponse databaseResourceResponse = await this.database.DeleteAsync();
   // Also valid: await this.cosmosClient.Databases["FamilyDatabase"].DeleteAsync();

   Console.WriteLine("Deleted Database: {0}\n", this.databaseId);

   //Dispose of CosmosClient
    this.cosmosClient.Dispose();
}
```

### <a name="execute-the-crud-operations"></a>CRUD işlemlerini yürüt

Gerekli tüm yöntemleri tanımladıktan sonra, yöntemle `GetStartedDemoAsync` çalıştırın. Yöntem, `DeleteDatabaseAndCleanupAsync` bu yöntem yürütülürse herhangi bir kaynak görmezsiniz, çünkü yöntem bu kodda yorumladı. Azure Cosmos DB kaynaklarınızın Azure portalında oluşturulduğunu doğruladıktan sonra yorumunuzu çıkarabilirsiniz. 

```csharp
public async Task GetStartedDemoAsync()
{
    // Create a new instance of the Cosmos Client
    this.cosmosClient = new CosmosClient(EndpointUrl, PrimaryKey);
    await this.CreateDatabaseAsync();
    await this.CreateContainerAsync();
    await this.AddItemsToContainerAsync();
    await this.QueryItemsAsync();
}
```

Gerekli tüm yöntemleri ekledikten sonra `Program.cs` dosyayı kaydedin. 

## <a name="run-the-code"></a>Kodu çalıştırma

Sonraki yapı ve Azure Cosmos DB kaynakları oluşturmak için uygulama çalıştırın. Yeni bir komut istemi penceresi açtığından emin olun, ortam değişkenlerini ayarlamak için kullandığınız örneği kullanmayın. Çünkü ortam değişkenleri geçerli açık pencerede ayarlanmamıştır. Güncelleştirmeleri görmek için yeni bir komut istemi açmanız gerekir. 

```console
dotnet build
```

```console
dotnet run
```

Uygulamayı çalıştırdığınızda aşağıdaki çıktı oluşturulur. Azure portalında oturum açabilir ve kaynakların oluşturulduğunu doğrulayabilirsiniz:

```console
Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.62 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

End of demo, press any key to exit.
```

Verilerin Azure portalında oturum açarak oluşturulduğunu doğrulayabilir ve Azure Cosmos hesabınızda gerekli öğeleri görebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, Azure Cosmos hesabını ve ilgili kaynak grubunu kaldırmak için Azure CLI veya Azure PowerShell'i kullanabilirsiniz. Aşağıdaki komut, Azure CLI'yi kullanarak kaynak grubunun nasıl silinir olduğunu gösterir:

```azurecli
az group delete -g "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos hesabı oluşturmayı, bir veritabanı ve bir kapsayıcı oluşturmayı bir .NET Core uygulamasını kullanarak öğrendiniz. Artık aşağıdaki makalede yer alan yönergelerle Azure Cosmos hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
