---
title: Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturun
description: C# Konsol uygulaması kullanarak Azure Cosmos db SQL API kaynakları oluşturmayı öğrenin.
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: kirankk
ms.openlocfilehash: ea3779ce7d4d77e6b0a55b8a00374b701a073b66
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876433"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Azure Cosmos DB SQL API hesabındaki verileri yönetmek için bir .NET konsol uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

SQL API Başlarken öğreticisine Azure Cosmos DB hoş geldiniz. Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız. Bu öğretici, [.NET Framework](https://dotnet.microsoft.com/download) veya [.NET Core](https://dotnet.microsoft.com/download)'a hedeflenmiş Azure Cosmos DB .NET SDK 'sının [3.0 + sürümünü](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) kullanır.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Oluşturma ve bir Azure Cosmos hesabına bağlanma
> * Visual Studio 'da projenizi yapılandırma
> * Veritabanı ve kapsayıcı oluşturma
> * Kapsayıcıya öğeleri ekleme
> * Kapsayıcıyı sorgulama
> * Öğe üzerindeki CRUD işlemleri
> * Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz. Hızlı yönergeler için [Tüm öğretici çözümünü al bölümüne](#GetSolution) atlayın.

Şimdi başlayalım!

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1\. adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB öykünücüsü kullanıyorsanız, öykünücüyü kurmak ve [Visual Studio projenizi kuruluma](#SetupVS)devam etmek Için [Azure Cosmos DB öykünücü](local-emulator.md) ' daki adımları izleyin.

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>2. Adım: Visual Studio projenizi kurma
1. Bilgisayarınızda **Visual Studio 2017**'yi açın.
1. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
1. **Yeni proje** iletişim kutusunda  / , **görsel C#**  **konsol uygulaması (.NET Framework)** öğesini seçin, projenizi adlandırın ve ardından **Tamam**' a tıklayın.
    ![Yeni proje penceresinin ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)

    > [!NOTE]
    > .NET Core hedefi için **Yeni proje** iletişim kutusunda,  /  **görsel C#**  **konsol uygulaması (.NET Core)** seçeneğini belirleyin, projenizi adlandırın ve ardından **Tamam** ' a tıklayın.

1. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.

    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. İçinde **NuGet** sekmesinde **Gözat**ve türü **Microsoft.Azure.Cosmos** arama kutusuna.
1. Bul sonuçları içinde **Microsoft.Azure.Cosmos** tıklatıp **yükleme**.
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.
   ![Azure Cosmos DB Istemci SDK 'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz.

## <a id="Connect"></a>Adım 3: Azure Cosmos DB hesabına bağlanma
1. İlk olarak, C# **program.cs** dosyasında, uygulamanızın başındaki başvuruları şu başvurularla değiştirin:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Configuration;
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
   ```

1. Şimdi, bu sabitleri ve değişkenleri ortak sınıfınıza ``Program``ekleyin.

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

    Unutmayın, .NET SDK'sının önceki sürümüyle biliyorsanız, koşulları 'collection' ve 'document' görmeye kullanılabilir Azure Cosmos DB, birden çok API modelini desteklediğinden, sürüm 3.0 + .NET SDK'sının genel Koşulları 'kapsayıcısı' ve 'öğesi' kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. [Daha fazla ilgili veritabanları, kapsayıcıları ve öğeleri öğrenin.](databases-containers-items.md)

1. Uç nokta URL 'nizi ve birincil anahtarınızı [Azure Portal](https://portal.azure.com)alın.

    Azure portalında Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın.

    Portaldan URI 'yi kopyalayın ve `<your endpoint URL>` ```Program.cs``` dosyaya yapıştırın. Portaldan BIRINCIL anahtarı kopyalayın ve içine `<your primary key>`yapıştırın.

   ![Azure portal Azure Cosmos DB anahtarları almak için ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. Sonra, yeni bir örneği ```CosmosClient``` oluşturacak ve programımız için bazı yapı iskelesi ayarlamaya başlayacağız.

    **Main** yönteminin altında, yeni bir zaman uyumsuz görev ekleyin,bu, yeni ```CosmosClient```bir örnek oluşturacak. Azure Cosmos DB kaynaklarında çalışan yöntemleri çağıran giriş noktası olarak **Getstarteddemoasync** kullanacağız.

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

1. **Ana** yönteminizin **Getstarteddemoasync** görevi çalıştırmak için aşağıdaki kodu ekleyin. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Uygulamanızı çalıştırmak için **F5** ' i seçin. Konsol penceresi çıktısı, Azure Cosmos DB bağlantısının yapıldığını `End of demo, press any key to exit.` onaylayan iletiyi görüntüler. Ardından konsol penceresini kapatabilirsiniz.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız. 

## <a name="step-4-create-a-database"></a>4\. Adım: Veritabanı oluşturma
Bir veritabanı, ``CosmosClient`` sınıfının [**createdatabaseifnotexistsasync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) veya [**createdatabaseasync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) işlevi kullanılarak oluşturulabilir. Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır.

1. **Createdatabaseasync** yöntemini kopyalayın ve **Getstarteddemoasync** yönteminizin altına yapıştırın. **Createdatabaseasync** , ``databaseId`` alandan belirtilen kimlikle, zaten mevcut ``FamilyDatabase`` değilse kimliği olan yeni bir veritabanı oluşturur. 

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

1. Daha önce eklediğiniz **Createdatabaseasync** yöntemini çağırmak için CosmosClient örneklendirmeniz gereken kodu kopyalayın ve yapıştırın.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    Bu noktada, kodunuz artık uç nokta ve birincil anahtarınızla doldurulmuş şekilde şöyle görünmelidir.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using System.Collections.Generic;
    using System.Net;
    using Microsoft.Azure.Cosmos;

    namespace CosmosGettingStarted
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

Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Azure Cosmos veritabanını başarıyla oluşturdunuz.  

## <a id="CreateColl"></a>5. Adım: Bir kapsayıcı oluşturma
> [!WARNING]
> **Createcontainerifnotexistsasync** yöntemini çağırmak, fiyatlandırma etkilerine sahip yeni bir kapsayıcı oluşturur. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
>
>

Bir kapsayıcı, **Cosmosdatabase** sınıfında [**Createcontainerifnotexistsasync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerIfNotExistsAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) veya [**createcontainsilinebilir Sync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) işlevi kullanılarak oluşturulabilir. Bir kapsayıcı, JavaScript 'te, örneğin saklı yordamlar, Kullanıcı tanımlı işlevler ve tetikleyiciler gibi öğeler (SQL API 'SI ise JSON belgeleri) ve ilişkili sunucu tarafı uygulama mantığını içerir.

1. Createcontainksync yöntemini kopyalayın ve **createdatabaseasync** yönteminizin altına yapıştırın. **Createcontainsilinebilir Sync** , zaten mevcut değilse, özelliği tarafından ``FamilyContainer`` ``LastName`` bölümlenmiş ``containerId`` alandan belirtilen kimliğe sahip yeni bir kapsayıcı oluşturur.

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

   Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Azure Cosmos kapsayıcısını başarıyla oluşturdunuz.  

## <a id="CreateDoc"></a>Adım 6: Kapsayıcıya öğe ekleme
Bir öğe **Cosmoscontainer** sınıfının [**Createıtemmasync**](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Container_CreateItemAsync__1___0_System_Nullable_Microsoft_Azure_Cosmos_PartitionKey__Microsoft_Azure_Cosmos_ItemRequestOptions_System_Threading_CancellationToken_) işlevi kullanılarak oluşturulabilir. SQL API'sini kullanarak öğeler, kullanıcı tanımlı (rastgele) JSON içeriği olan belgeler olarak görüntülenir. Artık Azure Cosmos kapsayıcınıza bir öğe ekleyebilirsiniz.

İlk olarak, bu örnekte Azure Cosmos DB içinde depolanan nesneleri temsil edecek bir **Aile** sınıfı oluşturalım. **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacağız. Note öğesinde JSON içinde **ID** olarak seri hale getirilmiş bir **ID** özelliği olmalıdır.

1. **Yeni öğe Ekle** iletişim kutusunu açmak için **CTRL + SHIFT + A** ' yı seçin. Projenize yeni bir **Family.cs** sınıfı ekleyin.

    ![Projeye yeni bir Family.cs sınıfı ekleme ekranının ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. **Aile**, **üst**, **alt**, **Evcil hayvan**ve **Adres** sınıfını kopyalayıp **Family.cs**içine yapıştırın.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]

1. **Program.cs** adresine gidin ve **createcontaintemsync** yönteminizin altına **additemstocontainsilinebilir eşitleme** yöntemini ekleyin.
Kod, aynı KIMLIĞE sahip bir öğenin oluşturmadan önce mevcut olmadığından emin olmak için kontrol eder. Her biri bir ve Wakefield ailesi için olmak üzere iki öğe ekleyecek.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]

1. ``GetStartedDemoAsync`` Yöntemine bir çağrı ``AddItemsToContainerAsync`` ekleyin.

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

Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! İki Azure Cosmos öğesini başarıyla oluşturdunuz.  

## <a id="Query"></a>Adım 7: Azure Cosmos DB kaynaklarını sorgula
Azure Cosmos DB, her kapsayıcıda depolanan JSON belgelerine karşı zengin [sorguları](sql-api-sql-query.md) destekler. Aşağıdaki örnek kod, önceki adımda eklediğimiz öğelerde bir sorgunun nasıl çalıştırılacağını gösterir.

1. **Queryıtemsasync** yöntemini kopyalayın ve **Additemstocontainkıtemyıtemyıtemyıtemyıtemyıtem**

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. ``GetStartedDemoAsync`` Yöntemine bir çağrı ``QueryItemsAsync`` ekleyin.

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

Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Azure Cosmos kapsayıcısına başarıyla sorguladınız.

## <a id="ReplaceItem"></a>8. Adım: JSON öğesini değiştirme
Artık Azure Cosmos DB bir öğeyi güncelleştireceğiz.

1. **ReplaceFamilyItemAsync** yöntemini kopyalayın ve **Queryıtemsasync** yönteminizin altına yapıştırın. Notta, ailenin ``Grade`` ve ``IsRegistered`` alt öğelerinden birinin özelliğini değiştiriyorsunuz.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. ``GetStartedDemoAsync`` Yöntemine bir çağrı ``ReplaceFamilyItemAsync`` ekleyin.

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

   Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Azure Cosmos öğesini başarıyla değiştirdik.

## <a id="DeleteDocument"></a>9. Adım: Öğeyi sil
Artık Azure Cosmos DB bir öğeyi silecağız.

1. **DeleteFamilyItemAsync** yöntemini kopyalayın ve **ReplaceFamilyItemAsync** yönteminizin altına yapıştırın.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. ``GetStartedDemoAsync`` Yöntemine bir çağrı ``DeleteFamilyItemAsync`` ekleyin.

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

Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Azure Cosmos öğesini başarıyla sildiniz.

## <a id="DeleteDatabase"></a>10. Adım: Veritabanını silme
Şimdi veritabanımızı silecağız. Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (kapsayıcılar, öğeler ve saklı yordamlar, Kullanıcı tanımlı işlevler ve Tetikleyiciler) kaldırılır. Ayrıca **CosmosClient** örneğini de atıyoruz.

1. **DeleteDatabaseAndCleanupAsync** yöntemini kopyalayın ve **DeleteFamilyItemAsync** yönteminizin altına yapıştırın.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. ``GetStartedDemoAsync`` Yöntemine bir çağrı ``DeleteDatabaseAndCleanupAsync`` ekleyin.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

Uygulamanızı çalıştırmak için **F5** ' i seçin.

Tebrikler! Bir Azure Cosmos veritabanını başarıyla sildiniz.

## <a id="Run"></a>Adım 11: C# Konsol uygulamanızı birlikte çalıştırın!
Visual Studio 'da F5 ' i seçerek uygulamayı derleyip hata ayıklama modunda çalıştırın.

Tüm uygulamanızın çıkışını bir konsol penceresinde görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

```
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

## <a id="GetSolution"></a> Tam öğretici çözümünü edinin
Bu öğreticideki adımları tamamlama fırsatınız olmadıysa veya yalnızca kod örneklerini indirmek isterseniz [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'dan ulaşabilirsiniz. 

GetStarted çözümünü oluşturmak için aşağıdakilere ihtiyacınız olacak:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
* [Azure Cosmos DB hesabı][cosmos-db-create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) çözümü.

Başvuruları Visual Studio 'daki Azure Cosmos DB .NET SDK 'sına geri yüklemek için, Çözüm Gezgini içindeki **getstarted** çözümüne sağ tıklayın ve ardından **NuGet paketlerini geri yükle**' ye tıklayın. Ardından, App. config dosyasında, dosyasında değiştirilecek endpointUri ve PrimaryKey değerlerini [Azure Cosmos DB hesabına bağlanma](#Connect)bölümünde açıklandığı gibi güncelleştirin.

Bu, derdir ve sizin de sizin.

## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz [. asp.NET MVC öğreticisi: Azure Cosmos DB](sql-api-dotnet-application-preview.md)ile Web uygulaması geliştirme.
* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure Cosmos DB ile performans ve ölçek testi](performance-testing.md)
* [Azure Cosmos DB isteklerini, kullanımını ve depolamasını izlemeyi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
