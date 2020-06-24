---
title: 'Öğretici: Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturma'
description: 'Öğretici: C# konsol uygulaması kullanarak Azure Cosmos DB SQL API kaynakları oluşturmayı öğrenin.'
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: kirankk
ms.openlocfilehash: 10a630aa04f51dc96b948b01e5fc01cfad4356fd
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85118823"
---
# <a name="tutorial-build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Öğretici: Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

SQL API Başlarken öğreticisine Azure Cosmos DB hoş geldiniz. Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Bu öğretici, [Azure Cosmos DB .NET SDK 'sının](https://www.nuget.org/packages/Microsoft.Azure.Cosmos)3,0 veya sonraki bir sürümünü kullanır. [.NET Framework veya .NET Core](https://dotnet.microsoft.com/download)ile çalışabilirsiniz.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
>
> * Azure Cosmos hesabı oluşturma ve bu hesaba bağlanma
> * Visual Studio 'da projenizi yapılandırma
> * Veritabanı ve kapsayıcı oluşturma
> * Kapsayıcıya öğeleri ekleme
> * Kapsayıcıyı sorgulama
> * Öğe üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştiriliyor
> * Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [Tüm öğretici çözümünü al bölümüne](#GetSolution) atlayın.

Şimdi başlayalım!

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesabınız zaten varsa, bu bölümü atlayın. Azure Cosmos DB öykünücüsünü kullanmak için, öykünücüyü ayarlamak üzere [Azure Cosmos DB öykünücüsü](local-emulator.md) ' nde bulunan adımları izleyin. [2. Adım: Visual Studio projenizi ayarlama](#SetupVS)bölümüne atlayın.

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="step-2-set-up-your-visual-studio-project"></a><a id="SetupVS"></a>2. Adım: Visual Studio projenizi ayarlama

1. Visual Studio 'Yu açın ve **Yeni proje oluştur**' u seçin.
1. **Yeni proje oluştur**bölümünde C# için **konsol uygulaması (.NET Framework)** öğesini seçin ve ardından **İleri**' yi seçin.
1. Projenizin *Cosmosgettingstartedöğreticisini*adlandırın ve **Oluştur**' u seçin.

    :::image type="content" source="./media/sql-api-get-started/configure-cosmos-getting-started-2019.png" alt-text="Projenizi yapılandırma":::

1. **Çözüm Gezgini**, Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.
1. **NuGet Paket Yöneticisi**'Nde, **Araştır** ' ı seçin ve *Microsoft. Azure. Cosmos*için arama yapın. **Microsoft. Azure. Cosmos** öğesini seçin ve **yüklemeyi**seçin.

   :::image type="content" source="./media/sql-api-get-started/cosmos-getting-started-manage-nuget-2019.png" alt-text="Azure Cosmos DB Istemci SDK 'Sı için NuGet 'i yükler":::

   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanan projesi için bkz. [Azure Cosmos DB kullanarak .NET konsol uygulaması geliştirme](https://github.com/Azure-Samples/cosmos-dotnet-getting-started).

## <a name="step-3-connect-to-an-azure-cosmos-db-account"></a><a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma

1. *Program.cs* dosyasındaki C# uygulamanızın başındaki başvuruları şu başvurularla değiştirin:

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
   > Önceki .NET SDK sürümü hakkında bilginiz varsa *, hüküm ve* *belge*hakkında bilgi sahibi olabilirsiniz. Azure Cosmos DB birden çok API modelini desteklediğinden, .NET SDK 'nın 3,0 sürümü genel terimler *kapsayıcısını* ve *öğesini*kullanır. *Kapsayıcı* bir koleksiyon, grafik veya tablo olabilir. Bir *öğe* bir belge, Kenar/köşe veya satır olabilir ve bir kapsayıcı içindeki içeriktir. Daha fazla bilgi için bkz. [Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md).

1. [Azure Portal](https://portal.azure.com)açın. Azure Cosmos DB hesabınızı bulun ve ardından **anahtarlar**' ı seçin.

   :::image type="content" source="./media/sql-api-get-started/cosmos-getting-started-portal-keys.png" alt-text="Azure portal Azure Cosmos DB anahtarları al":::

1. *Program.cs*' de, `<your endpoint URL>` **URI**değeri ile değiştirin. `<your primary key>` **Birincil anahtar**değeriyle değiştirin.

1. **Main** yönteminin altında, yeni bir zaman uyumsuz görev **ekleyin, bu, yeni**bir örneği oluşturur `CosmosClient` .

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

    Azure Cosmos DB kaynaklarında çalışan yöntemleri çağıran giriş noktası olarak **Getstarteddemoasync** kullanıyoruz.

1. **Ana** yönteminizin **Getstarteddemoasync** görevi çalıştırmak için aşağıdaki kodu ekleyin. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

    Konsol şu iletiyi görüntüler: **demo sonu, çıkmak için herhangi bir tuşa basın.** Bu ileti, uygulamanızın Azure Cosmos DB bağlantı yaptığını onaylar. Ardından konsol penceresini kapatabilirsiniz.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız.

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma

Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. `CreateDatabaseIfNotExistsAsync` `CreateDatabaseAsync` [CosmosClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) sınıfının ya da yöntemi bir veritabanı oluşturabilir.

1. Yöntemi kopyalayın ve `CreateDatabaseAsync` yönteminin altına yapıştırın `GetStartedDemoAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

    `CreateDatabaseAsync``FamilyDatabase`zaten mevcut değilse, bu kimliği alandan BELIRTILEN kimliğe sahip yeni bir veritabanı oluşturur `databaseId` .

1. Yeni eklediğiniz **Createdatabaseasync** yöntemini çağırmak için CosmosClient örneklediğinizde aşağıdaki kodu kopyalayın ve yapıştırın.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    *Program.cs* , bitiş noktanız ve birincil anahtarınızla doldurulmuş olarak şöyle görünmelidir.

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

   > [!NOTE]
   > Bir "503 hizmeti kullanılamayan özel durum" hatası alırsanız, doğrudan bağlantı modu için gerekli [bağlantı noktaları](performance-tips.md#networking) güvenlik duvarı tarafından engelleniyor olabilir. Bu sorunu onarmak için gerekli bağlantı noktalarını açın ya da aşağıdaki kodda gösterildiği gibi ağ geçidi modu bağlantısını kullanın:
   ```csharp
     // Create a new instance of the Cosmos Client in Gateway mode
     this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey, new CosmosClientOptions()
            {
                ConnectionMode = ConnectionMode.Gateway
            });
   ```

Tebrikler! Azure Cosmos veritabanını başarıyla oluşturdunuz.  

## <a name="step-5-create-a-container"></a><a id="CreateColl"></a>5. Adım: kapsayıcı oluşturma

> [!WARNING]
> Yöntemi, `CreateContainerIfNotExistsAsync` fiyatlandırma etkilerine sahip yeni bir kapsayıcı oluşturur. Daha fazla ayrıntı için lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/)sayfamızı ziyaret edin.
>
>

Bir kapsayıcı, sınıfında [**Createcontainerifnotexistsasync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerIfNotExistsAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) veya [**Createcontainsilinebilir Sync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) yöntemi kullanılarak oluşturulabilir `CosmosDatabase` . Bir kapsayıcı, JavaScript 'te, örneğin saklı yordamlar, Kullanıcı tanımlı işlevler ve tetikleyiciler gibi öğeler (SQL API 'SI ise JSON belgeleri) ve ilişkili sunucu tarafı uygulama mantığını içerir.

1. Yöntemi kopyalayın ve `CreateContainerAsync` yönteminin altına yapıştırın `CreateDatabaseAsync` . `CreateContainerAsync``FamilyContainer` `containerId` özellik tarafından bölümlenmiş alandan belirtilen kimliği kullanarak, zaten mevcut DEĞILSE, kimlik ile yeni bir kapsayıcı oluşturur `LastName` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Daha önce eklediğiniz **CreateContainer** yöntemini çağırmak için CosmosClient örneklendirmeniz gereken kodu kopyalayın ve yapıştırın.

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! Azure Cosmos kapsayıcısını başarıyla oluşturdunuz.  

## <a name="step-6-add-items-to-the-container"></a><a id="CreateDoc"></a>6. Adım: kapsayıcıya öğe ekleme

Sınıfın [**Createitemasync**](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Container_CreateItemAsync__1___0_System_Nullable_Microsoft_Azure_Cosmos_PartitionKey__Microsoft_Azure_Cosmos_ItemRequestOptions_System_Threading_CancellationToken_) yöntemi `CosmosContainer` bir öğe oluşturabilir. SQL API 'sini kullanırken öğeler, Kullanıcı tanımlı rastgele JSON içeriği olan belgeler olarak yansıtılacaktır. Artık Azure Cosmos kapsayıcınıza bir öğe ekleyebilirsiniz.

İlk olarak, `Family` Bu örnekteki Azure Cosmos db içinde depolanan nesneleri temsil eden bir sınıf oluşturalım. Ayrıca `Parent` , `Child` `Pet` `Address` içinde kullanılan alt sınıflar `Family` da oluşturacağız. Öğe `Id` JSON içinde olarak seri hale getirilmiş bir özelliğe sahip olmalıdır `id` .

1. **Yeni öğe Ekle**' yi açmak için CTRL + SHIFT + A ' yı seçin. Projenize yeni bir sınıf ekleyin `Family.cs` .

    :::image type="content" source="./media/sql-api-get-started/cosmos-getting-started-add-family-class-2019.png" alt-text="Projeye yeni bir Family.cs sınıfı ekleme ekranının ekran görüntüsü":::

1. ,,, `Family` `Parent` `Child` `Pet` Ve `Address` sınıfını kopyalayıp içine yapıştırın `Family.cs` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]


1. *Program.cs*'e geri döndüğünüzde, yönteminizi `AddItemsToContainerAsync` uyguladıktan sonra yöntemi ekleyin `CreateContainerAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]


    Kod, aynı KIMLIĞE sahip bir öğenin zaten mevcut olmadığından emin olmak için kontrol eder. Her biri *bir ve* *Wakefield ailesi*için olmak üzere iki öğe ekleyeceğiz.

1. Yöntemine bir çağrı ekleyin `AddItemsToContainerAsync` `GetStartedDemoAsync` .

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! İki Azure Cosmos öğesini başarıyla oluşturdunuz.  

## <a name="step-7-query-azure-cosmos-db-resources"></a><a id="Query"></a>7. Adım: Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir kapsayıcıda depolanan JSON belgeleri için zengin sorguların gerçekleştirilmesini destekler. Daha fazla bilgi için bkz. [SQL sorgularıyla çalışmaya](sql-api-sql-query.md)başlama. Aşağıdaki örnek kod, önceki adımda eklediğimiz öğelerde bir sorgunun nasıl çalıştırılacağını gösterir.

1. Yönteminden `QueryItemsAsync` sonra yöntemi kopyalayıp yapıştırın `AddItemsToContainerAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. Yöntemine bir çağrı ekleyin ``QueryItemsAsync`` ``GetStartedDemoAsync`` .

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! Azure Cosmos kapsayıcısını başarıyla sorguladınız.

## <a name="step-8-replace-a-json-item"></a><a id="ReplaceItem"></a>8. Adım: JSON öğesini değiştirme

Artık Azure Cosmos DB bir öğeyi güncelleştireceğiz. `IsRegistered` `Family` Alt öğelerinden birinin ve öğelerinin özelliğini değiştireceksiniz `Grade` .

1. Yönteminden `ReplaceFamilyItemAsync` sonra yöntemi kopyalayıp yapıştırın `QueryItemsAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. Yöntemine bir çağrı ekleyin `ReplaceFamilyItemAsync` `GetStartedDemoAsync` .

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! Azure Cosmos öğesini başarıyla değiştirdiniz.

## <a name="step-9-delete-item"></a><a id="DeleteDocument"></a>9. Adım: öğeyi silme

Artık Azure Cosmos DB bir öğeyi sileceğiz.

1. Yönteminden `DeleteFamilyItemAsync` sonra yöntemi kopyalayıp yapıştırın `ReplaceFamilyItemAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. Yöntemine bir çağrı ekleyin `DeleteFamilyItemAsync` `GetStartedDemoAsync` .

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

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! Azure Cosmos öğesini başarıyla sildiniz.

## <a name="step-10-delete-the-database"></a><a id="DeleteDatabase"></a>10. Adım: Veritabanını silme

Şimdi veritabanımızı sileceğiz. Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar kaldırılır. Kaynaklar kapsayıcılar, öğeler ve saklı yordamlar, Kullanıcı tanımlı işlevler ve Tetikleyiciler içerir. Örneği de atıyoruz `CosmosClient` .

1. Yönteminden `DeleteDatabaseAndCleanupAsync` sonra yöntemi kopyalayıp yapıştırın `DeleteFamilyItemAsync` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. Yöntemine bir çağrı ekleyin ``DeleteDatabaseAndCleanupAsync`` ``GetStartedDemoAsync`` .

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

1. Uygulamanızı çalıştırmak için F5 ' i seçin.

Tebrikler! Azure Cosmos veritabanını başarıyla sildiniz.

## <a name="step-11-run-your-c-console-application-all-together"></a><a id="Run"></a>11. Adım: C# konsol uygulamanızın tümünü çalıştırın!

Visual Studio 'da F5 ' i seçerek uygulamayı derleyip hata ayıklama modunda çalıştırın.

Tüm uygulamanızın çıkışını bir konsol penceresinde görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir. Aşağıdaki örnek metinle eşleşmelidir.

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

Bu öğreticideki adımları tamamlamaya yönelik bir zaman yoksa veya yalnızca kod örneklerini indirmek istiyorsanız, indirebilirsiniz.

Çözümü oluşturmak için `GetStarted` aşağıdaki önkoşullara sahip olmanız gerekir:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* Bir [Azure Cosmos DB hesabı][cosmos-db-create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) çözümü.

Başvuruları Visual Studio 'daki Azure Cosmos DB .NET SDK 'sına geri yüklemek için, **Çözüm Gezgini**çözüme sağ tıklayın ve ardından **NuGet paketlerini geri yükle**' yi seçin. Sonra, *App.config* dosyasında, `EndPointUri` ve `PrimaryKey` değerlerini [Adım 3: bir Azure Cosmos DB hesabına bağlanma](#Connect)bölümünde açıklandığı gibi güncelleştirin.

Bu, derdir ve sizin de sizin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz. [öğretici: .NET SDK kullanarak Azure Cosmos DB ile ASP.NET Core MVC web uygulaması geliştirme](sql-api-dotnet-application.md).
* Azure Cosmos DB ölçek ve performans testi yapmak istiyor musunuz? Bkz. [Azure Cosmos DB performans ve ölçek testi](performance-testing.md).
* Azure Cosmos DB isteklerinin, kullanımın ve depolamanın nasıl izleneceğini öğrenmek için bkz. [Azure Cosmos DB 'de performans ve depolama ölçümlerini izleme](monitor-accounts.md).
* Sorguları örnek veri kümesiyle çalıştırmak için [sorgu deneme alanı](https://www.documentdb.com/sql/demo)bakın.
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
