---
title: 'Öğretici: Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturun'
description: 'Öğretici: C# konsolu uygulamasını kullanarak Azure Cosmos DB SQL API kaynaklarını nasıl oluşturabilirsiniz öğrenin.'
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: kirankk
ms.openlocfilehash: 2681b2199f321f695bc621ed5580319a5e907b34
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78274022"
---
# <a name="tutorial-build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Öğretici: Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Azure Cosmos DB SQL API'ye hoş geldiniz. Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu öğretici, [Azure Cosmos DB .NET SDK'nın](https://www.nuget.org/packages/Microsoft.Azure.Cosmos)3.0 veya sonraki sürümlerini kullanır. [.NET Framework veya .NET Core](https://dotnet.microsoft.com/download)ile çalışabilirsiniz.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
>
> * Azure Cosmos hesabı oluşturma ve bağlanma
> * Visual Studio'da projenizi yapılandırma
> * Veritabanı ve kapsayıcı oluşturma
> * Kapsayıcıya öğeleri ekleme
> * Kapsayıcıyı sorgulama
> * Öğede oluşturma, okuma, güncelleme ve silme (CRUD) işlemlerini gerçekleştirme
> * Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz. Hızlı talimatlar için [tam öğretici çözüm bölümüne](#GetSolution) atlayın.

Şimdi başlayalım!

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Zaten kullanmak istediğiniz bir hesabınız varsa, bu bölümü atlayın. Azure Cosmos DB Emülatörü kullanmak için, emülatörkurmak için [Azure Cosmos DB Emulator'daki](local-emulator.md) adımları izleyin. Ardından Adım [2'ye geçin: Visual Studio projenizi ayarlayın.](#SetupVS)

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="step-2-set-up-your-visual-studio-project"></a><a id="SetupVS"></a>Adım 2: Visual Studio projenizi ayarlayın

1. Visual Studio'u açın ve **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da,** C#için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **ardından İleri'yi**seçin.
1. Projenizi *CosmosGettingStartedTutorial*adını alın ve sonra **Oluştur'u**seçin.

    ![Projenizi yapılandırın](./media/sql-api-get-started/configure-cosmos-getting-started-2019.png)

1. Solution **Explorer'da**Visual Studio çözümünüzün altında bulunan yeni konsol uygulamanızı sağ tıklatın ve **NuGet Paketlerini Yönet'i**seçin.
1. **NuGet Paket**Yöneticisi'nde, *Microsoft.Azure.Cosmos'u* **tarayın** ve arayın'ı seçin. **Microsoft.Azure.Cosmos'u** seçin ve **Yükle'yi**seçin.

   ![Azure Cosmos DB İstemci SDK için NuGet'i yükleyin](./media/sql-api-get-started/cosmos-getting-started-manage-nuget-2019.png)

   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanan projesi için [Azure Cosmos DB kullanarak bir .NET konsol uygulaması geliştirme bölümüne](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)bakın.

## <a name="step-3-connect-to-an-azure-cosmos-db-account"></a><a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma

1. *Program.cs* dosyasındaki C# uygulamanızın başındaki başvuruları şu referanslarla değiştirin:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Configuration;
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
   ```

1. Bu sabitleri ve değişkenleri `Program` sınıfınıza ekleyin.

    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE

        // The Azure Cosmos DB endpoint for running this sample.
        private static readonly string EndpointUri = "<your endpoint here>";
        // The primary key for the Azure Cosmos account.
        private static readonly string PrimaryKey = "<your primary key>";

        // The Cosmos client instance
        private CosmosClient cosmosClient;

        // The database we will create
        private Database database;

        // The container we will create.
        private Container container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```

   > [!NOTE]
   > .NET SDK'nın önceki sürümüne aşinaysanız, *terimlerin toplanması* ve *belgesini*biliyor olabilirsiniz. Azure Cosmos DB birden çok API modelini desteklediğinden, .NET SDK'nın 3.0 sürümü genel terimler *kapsayıcısını* ve *öğesini*kullanır. *Kapsayıcı* bir koleksiyon, grafik veya tablo olabilir. Öğe *item* belge, kenar/tepe noktası veya satır olabilir ve bir kapsayıcının içindeki içeriktir. Daha fazla bilgi için azure [cosmos DB'deki veritabanları, kapsayıcılar ve öğelerle çalışma 'ya](databases-containers-items.md)bakın.

1. Azure [portalını](https://portal.azure.com)açın. Azure Cosmos DB hesabınızı bulun ve ardından **Keys'i**seçin.

   ![Azure portalından Azure Cosmos DB tuşlarını alın](./media/sql-api-get-started/cosmos-getting-started-portal-keys.png)

1. Program.cs *Program.cs*yılında, `<your endpoint URL>` **URI**değeri ile değiştirin. PRIMARY `<your primary key>` **KEY**değeri ile değiştirin.

1. **Ana** yöntemin altında, **getStartedDemoAsync**adlı yeni bir asynchronous görev `CosmosClient`ekleyin , hangi yeni instantiates .

    ```csharp
    public static async Task Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    /*
        Entry point to call methods that operate on Azure Cosmos DB resources in this sample
    */
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
    }
    ```

    Azure Cosmos DB kaynaklarında çalışan yöntemleri çağıran giriş noktası olarak **GetStartedDemoAsync'i** kullanıyoruz.

1. **Ana** yönteminizden **GetStartedDemoAsync** asynchronous görevini çalıştırmak için aşağıdaki kodu ekleyin. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Uygulamanızı çalıştırmak için F5'i seçin.

    Konsol mesajı görüntüler: **Demo sonu, çıkmak için herhangi bir tuşa basın.** Bu ileti, uygulamanızın Azure Cosmos DB ile bağlantı yaptığını doğrular. Ardından konsol penceresini kapatabilirsiniz.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız.

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma

Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. [CosmosClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) sınıfının `CreateDatabaseIfNotExistsAsync` veya `CreateDatabaseAsync` yöntemi bir veritabanı oluşturabilir.

1. Yönteminizin `CreateDatabaseAsync` `GetStartedDemoAsync` altındaki yöntemi kopyalayıp yapıştırın.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

    `CreateDatabaseAsync`zaten yoksa, `FamilyDatabase` `databaseId` kimlik alanından belirtilen yeni bir veritabanı oluşturur.

1. Az önce eklediğiniz **CreateDatabaseAsync** yöntemini aramak için CosmosClient'ı anlık olarak oluşturduğunuz aşağıdaki kodu kopyalayıp yapıştırın.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    *Program.cs* şimdi, bitiş noktası ve birincil anahtar dolu, bu gibi görünmelidir.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using System.Collections.Generic;
    using System.Net;
    using Microsoft.Azure.Cosmos;

    namespace CosmosGettingStartedTutorial
    {
        class Program
        {
            // The Azure Cosmos DB endpoint for running this sample.
            private static readonly string EndpointUri = "<your endpoint here>";
            // The primary key for the Azure Cosmos account.
            private static readonly string PrimaryKey = "<your primary key>";

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
                    Console.WriteLine("Beginning operations...");
                    Program p = new Program();
                    await p.GetStartedDemoAsync();
                }
                catch (CosmosException de)
                {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: {0}\n", e);
                }
                finally
                {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
                }
            }

            /// <summary>
            /// Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            /// </summary>
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabaseAsync();
            }

            /// <summary>
            /// Create the database if it does not exist
            /// </summary>
            private async Task CreateDatabaseAsync()
            {
                // Create a new database
                this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

   > [!NOTE]
   > "503 hizmet kullanılamayan özel durum" hatası alırsanız, doğrudan bağlantı modu için gerekli [bağlantı noktalarının](performance-tips.md#networking) bir güvenlik duvarı tarafından engellenmesi mümkündür. Bu sorunu gidermek için, gerekli bağlantı noktalarını açın veya aşağıdaki kodda gösterildiği gibi ağ geçidi modu bağlantısını kullanın:
   ```csharp
     // Create a new instance of the Cosmos Client in Gateway mode
     this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey, new CosmosClientOptions()
            {
                ConnectionMode = ConnectionMode.Gateway
            });
   ```

Tebrikler! Azure Cosmos veritabanını başarıyla oluşturdunuz.  

## <a name="step-5-create-a-container"></a><a id="CreateColl"></a>Adım 5: Kapsayıcı oluşturma

> [!WARNING]
> Yöntem, `CreateContainerIfNotExistsAsync` fiyatlandırma etkileri olan yeni bir kapsayıcı oluşturur. Daha fazla bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/)ziyaret edin.
>
>

`CosmosDatabase` Kapsayıcı, [**sınıftacreateContainerIfNotExistsAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerIfNotExistsAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) veya [**CreateContainerAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) yöntemi kullanılarak oluşturulabilir. Kapsayıcı öğelerden (SQL API'se JSON belgeleri) ve JavaScript'te ilişkili sunucu tarafı uygulama mantığından (örneğin, depolanan yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler) oluşur.

1. Yönteminizin `CreateContainerAsync` `CreateDatabaseAsync` altındaki yöntemi kopyalayıp yapıştırın. `CreateContainerAsync`özellik tarafından `FamilyContainer` `containerId` `LastName` bölümlenen alandan belirtilen kimliği kullanarak, zaten yoksa, kimlikle birlikte yeni bir kapsayıcı oluşturur.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Az önce eklediğiniz **CreateContainer** yöntemini aramak için CosmosClient'ı anlık olarak oluşturduğunuz kodu kopyalayıp yapıştırın.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainerAsync();
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! Bir Azure Cosmos kapsayıcısı başarıyla oluşturdunuz.  

## <a name="step-6-add-items-to-the-container"></a><a id="CreateDoc"></a>Adım 6: Kapsayıcıya öğe ekleme

`CosmosContainer` Sınıfın [**CreateItemAsync**](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Container_CreateItemAsync__1___0_System_Nullable_Microsoft_Azure_Cosmos_PartitionKey__Microsoft_Azure_Cosmos_ItemRequestOptions_System_Threading_CancellationToken_) yöntemi bir öğe oluşturabilirsiniz. SQL API'si kullanırken, öğeler kullanıcı tarafından tanımlanan rasgele JSON içeriği olan belgeler olarak yansıtılır. Artık Azure Cosmos kapsayıcınıza bir öğe ekleyebilirsiniz.

İlk olarak, bu `Family` örnekte Azure Cosmos DB içinde depolanan nesneleri temsil eden bir sınıf oluşturalım. Ayrıca `Parent`içinde `Child` `Pet` `Family`kullanılan , `Address` , , alt sınıflar oluşturacağız. Öğenin JSON'daki `Id` gibi `id` serileştirilmiş bir özelliği olmalıdır.

1. **Yeni Öğe Ekle'yi**açmak için Ctrl+Shift+A'yı seçin. Projenize yeni `Family.cs` bir sınıf ekleyin.

    ![Projeye yeni bir Family.cs sınıfı ekleme ekran görüntüsü](./media/sql-api-get-started/cosmos-getting-started-add-family-class-2019.png)

1. Kopyalayın ve `Family`yapıştırın `Child` `Pet`, `Parent` `Address` , `Family.cs`, ve sınıf içine .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]


1. geri *Program.cs,* yönteminizden `AddItemsToContainerAsync` `CreateContainerAsync` sonra yöntemi ekleyin.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]


    Kod, aynı kimlikle bir öğenin zaten var olmadığından emin olmak için denetler. *Andersen Ailesi* ve *Wakefield Ailesi*için birer tane olmak üzere iki er parça ekleyeceğiz.

1. `AddItemsToContainerAsync` Yönteme bir çağrı `GetStartedDemoAsync` ekleyin.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainerAsync();
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! İki Azure Cosmos öğeyi başarıyla oluşturdunuz.  

## <a name="step-7-query-azure-cosmos-db-resources"></a><a id="Query"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir kapsayıcıda depolanan JSON belgeleri için zengin sorguların gerçekleştirilmesini destekler. Daha fazla bilgi için bkz: [SQL sorguları ile başlarken.](sql-api-sql-query.md) Aşağıdaki örnek kod, önceki adımda eklediğimiz öğelere karşı bir sorgunun nasıl çalıştırılabildiğini gösterir.

1. `QueryItemsAsync` Yöntemden sonra yöntemi kopyalayıp yapıştırın. `AddItemsToContainerAsync`

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. ``QueryItemsAsync`` Yönteme bir çağrı ``GetStartedDemoAsync`` ekleyin.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.QueryItemsAsync();
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! Bir Azure Cosmos kapsayıcıyı başarıyla sorguladınız.

## <a name="step-8-replace-a-json-item"></a><a id="ReplaceItem"></a>Adım 8: JSON öğeyi değiştirme

Şimdi, Azure Cosmos DB'deki bir öğeyi güncelleştireceğiz. Çocukların ve çocukların `IsRegistered` malını `Family` `Grade` değiştireceğiz.

1. `ReplaceFamilyItemAsync` Yöntemden sonra yöntemi kopyalayıp yapıştırın. `QueryItemsAsync`

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. `ReplaceFamilyItemAsync` Yönteme bir çağrı `GetStartedDemoAsync` ekleyin.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItemAsync();
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! Bir Azure Cosmos öğeyi başarıyla değiştirdiniz.

## <a name="step-9-delete-item"></a><a id="DeleteDocument"></a>Adım 9: Öğeyi silme

Şimdi, Azure Cosmos DB'deki bir öğeyi sileceğiz.

1. `DeleteFamilyItemAsync` Yöntemden sonra yöntemi kopyalayıp yapıştırın. `ReplaceFamilyItemAsync`

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. `DeleteFamilyItemAsync` Yönteme bir çağrı `GetStartedDemoAsync` ekleyin.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();
        await this.ReplaceFamilyItemAsync();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItemAsync();
    }
    ```

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! Bir Azure Cosmos öğeni başarıyla sildiniz.

## <a name="step-10-delete-the-database"></a><a id="DeleteDatabase"></a>10. Adım: Veritabanını silme

Şimdi veritabanımızı sileceğiz. Oluşturulan veritabanını siler, veritabanını ve tüm küçük kaynakları kaldırır. Kaynaklar kapsayıcıları, maddeleri ve depolanan yordamları, kullanıcı tanımlı işlevleri ve tetikleyicileri içerir. Biz de `CosmosClient` örnek bertaraf.

1. `DeleteDatabaseAndCleanupAsync` Yöntemden sonra yöntemi kopyalayıp yapıştırın. `DeleteFamilyItemAsync`

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. ``DeleteDatabaseAndCleanupAsync`` Yönteme bir çağrı ``GetStartedDemoAsync`` ekleyin.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

1. Uygulamanızı çalıştırmak için F5'i seçin.

Tebrikler! Azure Cosmos veritabanını başarıyla sildiniz.

## <a name="step-11-run-your-c-console-application-all-together"></a><a id="Run"></a>11. Adım: C# konsol uygulamanızın tümünü çalıştırın!

Uygulamayı hata ayıklama modunda oluşturmak ve çalıştırmak için Visual Studio'da F5'i seçin.

Uygulamanızın tüm çıktısını bir konsol penceresinde görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir. Aşağıdaki örnek metinle eşleşmelidir.

```cmd
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

Tebrikler! Bu öğreticiyi tamamladınız ve çalışan bir C# konsol uygulamasına sahipsiniz!

## <a name="get-the-complete-tutorial-solution"></a><a id="GetSolution"></a> Tam öğretici çözümünü edinin

Bu öğreticideki adımları tamamlamak için zamanın yoksa veya sadece kod örneklerini indirmek istiyorsanız, indirebilirsiniz.

`GetStarted` Çözümü oluşturmak için aşağıdaki ön koşullara ihtiyacınız vardır:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* Bir [Azure Cosmos DB hesabı][cosmos-db-create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) çözümü.

Visual Studio'da Azure Cosmos DB .NET SDK'ya yapılan başvuruları geri yüklemek için **Solution Explorer'daki**çözüme sağ tıklayın ve ardından **NuGet Paketlerini Geri Yükle'yi**seçin. Ardından, *App.config* dosyasında, `EndPointUri` Adım `PrimaryKey` 3'te açıklandığı gibi değerleri ve değerleri güncelleştirin: [Bir Azure Cosmos DB hesabına bağlanın.](#Connect)

İşte bu, inşa et ve sen yoldasın!

## <a name="next-steps"></a>Sonraki adımlar

* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz. [Öğretici: .NET SDK'yı kullanarak Azure Cosmos DB ile ASP.NET Core MVC web uygulaması geliştirin.](sql-api-dotnet-application.md)
* Azure Cosmos DB ile ölçek lendirme ve performans testi yapmak ister misiniz? [Azure Cosmos DB ile Performans ve ölçek testine](performance-testing.md)bakın.
* Azure Cosmos DB isteklerini, kullanımını ve depolamasını nasıl izleyeceğinizi öğrenmek için [Azure Cosmos DB'deki performansı ve depolama ölçümlerini izleyin.](monitor-accounts.md)
* Sorguları örnek veri kümemize karşı çalıştırmak için [Sorgu Alanı'na](https://www.documentdb.com/sql/demo)bakın.
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
