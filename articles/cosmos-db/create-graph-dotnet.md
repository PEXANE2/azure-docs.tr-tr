---
title: Gremlin API'sini kullanarak Azure Cosmos DB .NET Framework, Core uygulaması oluşturun
description: Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET Framework/Core kod örneği sunar
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 02/21/2020
ms.author: lbosq
ms.openlocfilehash: bf453587b354b5db3f3ef1a80f974bcb8f8f4e14
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730029"
---
# <a name="quickstart-build-a-net-framework-or-core-application-using-the-azure-cosmos-db-gremlin-api-account"></a>Hızlı başlatma: Azure Cosmos DB Gremlin API hesabını kullanarak bir .NET Framework veya Core uygulaması oluşturun

> [!div class="op_single_selector"]
> * [Gremlin konsolu](create-graph-gremlin-console.md)
> * [.NET](create-graph-dotnet.md)
> * [Java](create-graph-java.md)
> * [Node.js](create-graph-nodejs.md)
> * [Python](create-graph-python.md)
> * [PHP](create-graph-php.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç, Azure portalını kullanarak bir Azure Cosmos DB [Gremlin API](graph-introduction.md) hesabı, veritabanı ve grafik (kapsayıcı) nasıl oluşturulacak larını gösterir. Daha sonra açık kaynaklı [Gremlin.Net](https://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet) sürücüsünü kullanarak bir konsol uygulaması oluşturabilir ve çalıştırabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

Visual Studio 2019 yüklü değilseniz, **ücretsiz** [Visual Studio 2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Graf ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Gremlin API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-gremlindotnet-getting-started.git
    ```

4. Ardından Visual Studio’yu ve çözüm dosyasını açın.

5. Projedeki NuGet paketlerini geri yükleyin. Bu Gremlin.Net sürücüsünü ve Newtonsoft.Json paketini içermelidir.


6. Ayrıca NuGet paket yöneticisi veya [NuGet komut satırı yardımcı programını](https://docs.microsoft.com/nuget/install-nuget-client-tools) kullanarak Gremlin.Net sürücüsünü kendiniz de yükleyebilirsiniz: 

    ```bash
    nuget install Gremlin.Net
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tamamı, Program.cs dosyasından alınır.

* Bağlantı parametrelerinizi yukarıda oluşturulan hesaba göre ayarlayın: 

   :::code language="csharp" source="~/azure-cosmosdb-graph-dotnet/GremlinNetSample/Program.cs" id="configureConnectivity":::

* Yürütülecek Gremlin komutları sözlükte listelenir:

   :::code language="csharp" source="~/azure-cosmosdb-graph-dotnet/GremlinNetSample/Program.cs" id="defineQueries":::

* Yukarıda sağlanan `GremlinServer` `GremlinClient` parametreleri kullanarak yeni ve bağlantı nesneleri oluşturun:

   :::code language="csharp" source="~/azure-cosmosdb-graph-dotnet/GremlinNetSample/Program.cs" id="defineClientandServerObjects":::

* Async görevi ile `GremlinClient` nesnekullanarak her Gremlin sorgu yürütmek. Gremlin sorgularını önceki adımda tanımlanan sözlükten okuyabilir ve çalıştırabilirsiniz. Daha sonra sonucu almak ve Newtonsoft.Json paketinden `JsonSerializer` sınıf kullanarak, bir sözlük olarak biçimlendirilmiş değerleri okuyun:

   :::code language="csharp" source="~/azure-cosmosdb-graph-dotnet/GremlinNetSample/Program.cs" id="executeQueries":::

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalda](https://portal.azure.com/) graf veritabanı hesabınıza gidin. **Genel Bakış** sekmesinde iki uç nokta görebilirsiniz: 
 
   **.NET SDK URI** - Bu değer, Microsoft.Azure.Graphs kitaplığını kullanarak grafik hesabına bağlandığınızda kullanılır. 

   **Gremlin Uç Noktası** - Bu değer, Gremlin.Net kitaplığını kullanarak graf hesabına bağlandığınızda kullanılır.

    ![Uç noktayı kopyalama](./media/create-graph-dotnet/endpoint.png)

   Bu örneği çalıştırmak için **Gremlin Endpoint** değerini kopyalayın, sonundaki bağlantı noktası `https://<your cosmos db account name>.gremlin.cosmosdb.azure.com`numarasını silin, yani URI olur. Uç nokta değeri aşağıdaki gibi görünmelidir`testgraphacct.gremlin.cosmosdb.azure.com`

1. Ardından, **Keys** sekmesine gidin ve Azure portalından **PRIMARY KEY** değerini kopyalayın. 

1. Hesabınızın URI ve BIRINCIL ANAHTARINI kopyaladıktan sonra, bunları uygulamayı çalıştıran yerel makinede yeni bir ortam değişkenine kaydedin. Ortam değişkenini ayarlamak için bir komut istemi penceresi açın ve aşağıdaki komutu çalıştırın. > Your_Azure_Cosmos_account_URI <ve> değerleri Your_Azure_Cosmos_account_PRIMARY_KEY <emin olun.

   ```console
   setx EndpointUrl "<your Azure Cosmos account name>.gremlin.cosmosdb.azure.com"
   setx PrimaryKey "<Your_Azure_Cosmos_account_PRIMARY_KEY>"
   ```

1. *Program.cs* dosyasını açın ve yukarıda oluşturulan veritabanı ve kapsayıcı (grafik adı da) adlarıyla "veritabanı ve "kapsayıcı" değişkenlerini güncelleştirin.

    `private static string database = "your-database-name";` `private static string container = "your-container-or-graph-name";`

1. Program.cs dosyasını kaydedin. 

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın. Uygulama hem Gremlin sorgu komutlarını hem de konsol sonuçlarını yazdırır.

   Konsol penceresinde grafiğe eklenmekte olan kenarlar ve köşeler gösterilir. Betik tamamlandığında, ENTER tuşuna basarak konsol penceresini kapatın.

## <a name="browse-using-the-data-explorer"></a>Veri Gezgini'ni kullanarak göz atma

Şimdi Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinize göz atıp sorgu gönderebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Graflar bölmesinde görüntülenir. Veritabanını ve kapsayıcı düğümlerini genişletip **Graf**’a tıklayın.

2. Graftaki tüm köşeleri görüntülemek üzere varsayılan sorguyu kullanmak için **Filtre Uygula** düğmesine tıklayın. Örnek uygulama tarafından oluşturulan veriler Grafikler bölmesinde görüntülenir.

    Grafiği yakınlaştırıp uzaklaştırabilir, grafik görüntüleme alanını genişletebilir, başka köşeler ekleyebilir ve görüntüleme alanında köşeleri taşıyabilirsiniz.

    ![Azure portalındaki Veri Gezgini'nde grafiği görüntüleme](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak grafik oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Artık daha karmaşık sorgular derleyebilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)

