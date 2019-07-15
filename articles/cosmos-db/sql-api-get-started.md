---
title: Azure Cosmos DB SQL API hesabı verileri yönetmek için bir .NET konsol uygulaması oluşturma
description: Kullanarak Azure Cosmos DB SQL API kaynakları oluşturmayı öğrenmenin bir C# konsol uygulaması.
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: kirankk
ms.openlocfilehash: 6fd7efe38aeb1f1094d240cf1675d432f3766229
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985717"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Azure Cosmos DB SQL API hesabı verileri yönetmek için bir .NET konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Azure Cosmos DB SQL API almak için Hoş Geldiniz Öğreticisi. Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız. Bu öğreticide [sürüm 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) Azure Cosmos DB .NET SDK'sının için hedeflenen, [.NET Framework](https://dotnet.microsoft.com/download) veya [.NET Core](https://dotnet.microsoft.com/download).

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Oluşturma ve bir Azure Cosmos hesabına bağlanma
> * Projenizi Visual Studio'da yapılandırma
> * Bir veritabanı ve kapsayıcı oluşturma
> * Kapsayıcıya öğeleri ekleme
> * Kapsayıcıyı sorgulama
> * Öğesi CRUD işlemleri
> * Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz. Atla [edinme Öğreticisi tam çözümünü bölümüne](#GetSolution) hızlı yönergeler için.

Şimdi başlayalım!

## <a name="prerequisites"></a>Önkoşullar

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1\. adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Visual Studio Çözümünüzü Kurma](#SetupVS)'ya atlayabilirsiniz. Azure Cosmos DB öykünücüsü'nü kullanıyorsanız, bu adımları izleyin [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) öykünücünün kurulumunu ve atlayın [Kurulum, Visual Studio projesi](#SetupVS).

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>2. adım: Kurulum, Visual Studio projesi
1. Bilgisayarınızda **Visual Studio 2017**'yi açın.
1. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
1. İçinde **yeni proje** iletişim kutusunda **Visual C#**   /  **konsol uygulaması (.NET Framework)** , projenizi adlandırın ve ardından **Tamam** .
    ![Yeni Proje penceresinin ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)

    > [!NOTE]
    > .NET için hedef, çekirdek **yeni proje** iletişim kutusunda **Visual C#**   /  **konsol uygulaması (.NET Core)** , projenizi adlandırın ve ardından tıklayın **Tamam**

1. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.

    ![Proje için Sağ Tıklama Menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. İçinde **NuGet** sekmesinde **Gözat**ve türü **Microsoft.Azure.Cosmos** arama kutusuna.
1. Bul sonuçları içinde **Microsoft.Azure.Cosmos** tıklatıp **yükleme**.
   Azure Cosmos DB SQL API'si İstemci Kitaplığının paket kimliği [Microsoft Azure Cosmos DB İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)’dır.
   ![Azure Cosmos DB istemci SDK'sını bulmak için NuGet menüsünün ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

Harika! Kurulumu tamamladığımıza göre, biraz kod yazmaya başlayalım. Bu öğreticinin tamamlanmış kod projesini [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)'da bulabilirsiniz.

## <a id="Connect"></a>3. adım: Bir Azure Cosmos DB hesabına bağlanma
1. İlk olarak, başında başvuruları değiştirin, C# içinde uygulama **Program.cs** bu başvuruları dosyasıyla:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Configuration;
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
   ```

1. Şimdi bu sabitleri ve değişkenleri ortak sınıfının ekleyin ``Program``.

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

1. Uç nokta URL'nizi ve birincil anahtarı almak [Azure portalında](https://portal.azure.com).

    Azure portalında Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın.

    Portaldaki URI'yi kopyalayın ve yapıştırın `<your endpoint URL>` içinde ```Program.cs``` dosya. Portaldan birincil anahtarı kopyalayın ve yapıştırın `<your primary key>`.

   ![Azure portalında Azure Cosmos DB anahtarlarını almak için ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. Ardından, yeni bir örneğini oluşturacağız ```CosmosClient``` ve programımız için bazı iskele kurun.

    Aşağıda **ana** yöntemi olarak adlandırılan yeni bir zaman uyumsuz görev eklemek **GetStartedDemoAsync**, örneğini oluşturacak yeni ```CosmosClient```. Kullanacağız **GetStartedDemoAsync** yöntemler, çağıran giriş noktası olarak Azure Cosmos DB kaynakları üzerinde çalışır.

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

1. Çalıştırmak için aşağıdaki kodu ekleyin **GetStartedDemoAsync** zaman uyumsuz görevi, **ana** yöntemi. **Main** yöntemi özel durumları yakalar ve bunları konsola yazar.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Seçin **F5** uygulamanızı çalıştırmak için. Konsol penceresi çıktısı görüntülenir `End of demo, press any key to exit.` Azure Cosmos DB bağlantının kurulduğunu onaylayan. Ardından konsol penceresini kapatabilirsiniz.

Tebrikler! Bir Azure Cosmos DB hesabına başarıyla bağlandınız. 

## <a name="step-4-create-a-database"></a>4\. Adım: Veritabanı oluşturma
Bir veritabanını kullanarak oluşturulabilir [ **Createdatabaseasync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) veya [ **Documentclient** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) işlevi ``CosmosClient`` sınıfı. Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır.

1. Kopyalama ve yapıştırma **Documentclient** yöntemi aşağıdaki, **GetStartedDemoAsync** yöntemi. **Documentclient** Kimliğine sahip yeni bir veritabanı oluşturur ``FamilyDatabase`` , zaten, öğesinden belirtilen Kimliğe sahip yoksa ``databaseId`` alan. 

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

1. Çağrılacak CosmosClient örneği burada aşağıdaki kodu yapıştırın **Documentclient** eklediğiniz yöntemi.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    Bu noktada, kodunuzu gibi uç noktanızı ve birincil anahtar doldurulmuş ile görünmesi gerekir.

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

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.  

## <a id="CreateColl"></a>5. adım: Bir kapsayıcı oluşturma
> [!WARNING]
> Yöntemini çağırarak **CreateContainerIfNotExistsAsync** ödenmesini yeni bir kapsayıcı oluşturur. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
>
>

Bir kapsayıcı kullanarak oluşturulabilir [ **CreateContainerIfNotExistsAsync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabase) veya [ **CreateContainerAsync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabase) işlevi **CosmosDatabase** sınıfı. Bir kapsayıcı öğelerden oluşur (JSON belgeleri, SQL API'si) ve sunucu tarafı uygulama mantığını saklı yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler JavaScript ilişkili.

1. Kopyalama ve yapıştırma **CreateContainerAsync** yöntemi aşağıdaki, **Documentclient** yöntemi. **CreateContainerAsync** Kimliğine sahip yeni bir kapsayıcı oluşturacak ``FamilyContainer`` , zaten, öğesinden belirtilen Kimliğe sahip olmaması halinde ``containerId`` alan bölümlenmiş tarafından ``LastName`` özelliği.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Çağrılacak CosmosClient örneği burada aşağıdaki kodu yapıştırın **CreateContainer** eklediğiniz yöntemi.

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

   Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB kapsayıcısı oluşturdunuz.  

## <a id="CreateDoc"></a>6. adım: Öğeleri kapsayıcıya Ekle
Bir öğeyi kullanarak oluşturulabilir [ **Createıtemasync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmoscontainer) işlevi **CosmosContainer** sınıfı. SQL API'sini kullanarak öğeler, kullanıcı tanımlı (rastgele) JSON içeriği olan belgeler olarak görüntülenir. Bu gibi durumlarda, bir öğe artık Azure Cosmos DB kapsayıcınız ekleyebilirsiniz.

İlk olarak, oluşturalım bir **ailesi** Bu örnekte Azure Cosmos DB içinde depolanan nesneleri temsil edecek sınıfı. **Family**'nin içinde kullanılan **Parent**, **Child**, **Pet**, **Address** alt sınıflarını da oluşturacağız. Not öğesi içermelidir bir **kimliği** özelliği seri olarak **kimliği** JSON.

1. Seçin **Ctrl + Shift + A** açmak için **Yeni Öğe Ekle** iletişim. Yeni bir sınıf ekleyin **Family.cs** projenize.

    ![Projeye yeni Family.cs sınıf ekleme işleminin ekran görüntüsü](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Kopyalama ve yapıştırma **ailesi**, **üst**, **alt**, **evcil hayvan**, ve **adresi** sınıfına**Family.cs**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]

1. Geri gidin **Program.cs** ve ekleme **AddItemsToContainerAsync** altındaki yöntemin, **CreateContainerAsync** yöntemi.
Kod oluşturmadan önce aynı Kimliğe sahip bir öğe zaten yok emin olmak için kontrol eder. Biz, her biri Andersen ailesi ve Wakefield ailesi için iki öğeyi ekler.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]

1. Bir çağrı ekleyin ``AddItemsToContainerAsync`` içinde ``GetStartedDemoAsync`` yöntemi.

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

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! İki Azure Cosmos DB öğeleri başarıyla oluşturdunuz.  

## <a id="Query"></a>7. adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, önceki adımda eklememizden öğeleri karşı sorgu çalıştırmak gösterilmektedir.

1. Kopyalama ve yapıştırma **QueryItemsAsync** yöntemi aşağıdaki, **AddItemsToContainerAsync** yöntemi.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. Bir çağrı ekleyin ``QueryItemsAsync`` içinde ``GetStartedDemoAsync`` yöntemi.

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

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Bir Azure Cosmos DB kapsayıcısı karşı başarıyla sorguladınız.

## <a id="ReplaceItem"></a>8. adım: Bir JSON öğesini değiştirin
Şimdi, Azure Cosmos DB içindeki bir öğeyi güncelleştireceğiz.

1. Kopyalama ve yapıştırma **ReplaceFamilyItemAsync** yöntemi aşağıdaki, **QueryItemsAsync** yöntemi. Değiştirmeyi unutmayın ``IsRegistered`` özellik ailesinin ve ``Grade`` alt birinin.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. Bir çağrı ekleyin ``ReplaceFamilyItemAsync`` içinde ``GetStartedDemoAsync`` yöntemi.

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

   Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini değiştirdiniz.

## <a id="DeleteDocument"></a>9. adım: Öğeyi Sil
Şimdi, size Azure Cosmos DB içindeki bir öğeyi siler.

1. Kopyalama ve yapıştırma **DeleteFamilyItemAsync** yöntemi aşağıdaki, **ReplaceFamilyItemAsync** yöntemi.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. Bir çağrı ekleyin ``DeleteFamilyItemAsync`` içinde ``GetStartedDemoAsync`` yöntemi.

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

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini sildiniz.

## <a id="DeleteDatabase"></a>10. adım: Veritabanını silme
Şimdi biz veritabanımızdaki silecek. Oluşturulan veritabanı silindiğinde kaldırılır veritabanını ve tüm alt kaynaklarını (kapsayıcılar, öğeleri ve depolanan yordamlar, kullanıcı tanımlı işlevler ve tetikleyiciler). Biz de elden **CosmosClient** örneği.

1. Kopyalama ve yapıştırma **DeleteDatabaseAndCleanupAsync** yöntemi aşağıdaki, **DeleteFamilyItemAsync** yöntemi.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. Bir çağrı ekleyin ``DeleteDatabaseAndCleanupAsync`` içinde ``GetStartedDemoAsync`` yöntemi.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

Seçin **F5** uygulamanızı çalıştırmak için.

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanını sildiniz.

## <a id="Run"></a>11. adım: Çalıştırın, C# konsol uygulaması tümünü bir araya!
Derleme ve uygulamayı hata ayıklama modunda çalıştırmak için Visual Studio'da F5'i seçin.

Konsol penceresinde tüm uygulamanızın çıktısını görmeniz gerekir. Çıktı, eklediğimiz sorguların sonuçlarını gösterir ve aşağıdaki örnek metinle eşleşmelidir.

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
* Bir [Azure Cosmos DB hesabı][cosmos-db-create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) çözümü.

Visual Studio'da Azure Cosmos DB .NET SDK başvuruları geri yüklemek için sağ **GetStarted** çözümde, Çözüm Gezgini ve ardından **NuGet paketlerini geri yükle**. Ardından, App.config dosyasında açıklanan şekilde EndPointUri ve PrimaryKey değerlerini güncelleştirin [bir Azure Cosmos DB hesabına bağlanma](#Connect).

İşte, yapı, hepsi ve bu kadar!

## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir ASP.NET MVC öğreticisi mi istiyorsunuz? Bkz: [ASP.NET MVC Öğreticisi: Azure Cosmos DB ile uygulama geliştirme Web](sql-api-dotnet-application-preview.md).
* Azure Cosmos DB ile ölçek ve performans testi mi yapmak istiyorsunuz? Bkz. [Azure Cosmos DB ile performans ve ölçek testi](performance-testing.md)
* [Azure Cosmos DB isteklerini, kullanımını ve depolamasını izlemeyi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
