---
title: MongoDB ve .NET SDK için Azure Cosmos DB'nin API'yi kullanarak bir web uygulaması oluşturun
description: Azure Cosmos DB'nin MongoDB için API'sini kullanarak bağlanmak ve sorgulamak için kullanabileceğiniz bir .NET kod örneği sunar.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.openlocfilehash: 55f40e8f22b39caa3527a378f33982276bed597d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240187"
---
# <a name="quickstart-build-a-net-web-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Hızlı başlangıç: MongoDB için Azure Cosmos DB'nin API'sini kullanarak bir .NET web uygulaması oluşturun 

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Cosmos DB'nin çekirdeğindeki küresel dağıtım ve yatay ölçek özelliklerinden yararlanan belge, anahtar/değer ve grafik veritabanlarını hızlı bir şekilde oluşturabilir ve sorgulayabilirsiniz. 

Bu hızlı başlangıç, [Azure Cosmos DB'nin MongoDB için API'si](mongodb-introduction.md)ile nasıl bir Cosmos hesabı oluşturulacak larını gösterir. Daha sonra [MongoDB .NET sürücüsü](https://docs.mongodb.com/ecosystem/drivers/csharp/)kullanılarak oluşturulmuş bir görev listesi web uygulaması oluşturur ve dağıtırsınız.

## <a name="prerequisites-to-run-the-sample-app"></a>Örnek uygulamayı çalıştırmak için önkoşullar

Örneği çalıştırmak için [Visual Studio'ya](https://www.visualstudio.com/downloads/) ve geçerli bir Azure Cosmos DB hesabına ihtiyacınız vardır.

Visual Studio'nuz yoksa, kurulumla yüklü ASP.NET ve web geliştirme iş yükünüiçeren [Visual Studio 2019 Community Edition'ı](https://www.visualstudio.com/downloads/) **indirin.**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

Bu makalede açıklanan örnek, MongoDB.Driver 2.6.1 sürümü ile uyumludur.

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

İlk olarak, örnek uygulamayı GitHub'dan indirin. 

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

Git kullanmak istemiyorsanız, [projeyi ZIP dosyası olarak](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started/archive/master.zip)da indirebilirsiniz.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tümü, DAL dizinindeki Dal.cs dosyasından alınmıştır.

* İstemciyi initialize edin.

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credential = new MongoCredential("SCRAM-SHA-1", identity, evidence);

        MongoClient client = new MongoClient(settings);
    ```

* Veritabanı ve koleksiyonu alın.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Tüm belgeleri alın.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

Görev oluşturma ve koleksiyona ekleme

   ```csharp
    public void CreateTask(MyTask task)
    {
        var collection = GetTasksCollectionForEdit();
        try
        {
            collection.InsertOne(task);
        }
        catch (MongoCommandException ex)
        {
            string msg = ex.Message;
        }
    }
   ```
   Benzer şekilde, [collection.UpdateOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.updateOne/index.html) ve [collection.DeleteOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.deleteOne/index.html) metotlarını kullanarak belgeleri güncelleştirebilir ve silebilirsiniz. 

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. Azure [portalında](https://portal.azure.com/), Cosmos hesabınızda, sol navigasyonda **Bağlantı Dizesini**tıklatın ve ardından **Oku-Yaz Tuşlarını**tıklatın. Sonraki adımda ekranın sağ tarafındaki kopyalama düğmelerini kullanarak Kullanıcı Adı, Parola ve Ana Bilgisayar değerlerini Dal.cs dosyasına kopyalayacaksınız.

2. **DAL** dizinindeki **Dal.cs** dosyasını açın. 

3. **userename** değerini portaldan kopyalayın (kopyalama düğmesini kullanarak) ve **Dal.cs** dosyasına **username** değeri olarak yapıştırın. 

4. Ardından **host** değerini portaldan kopyalayın ve **Dal.cs** dosyanıza **host** değeri olarak yapıştırın. 

5. Son olarak, **password** değerini portaldan kopyalayın ve **Dal.cs** dosyanıza **password** değeri olarak yapıştırın. 

Artık uygulamanızı Cosmos DB ile iletişim kurmak için gereken tüm bilgilerle güncellediniz. 
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Visual Studio'nun **Çözüm Gezgini** bölümünde projeye sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet **Gözat** kutusuna *MongoDB* yazın.

3. Sonuçlardan **MongoDB.Driver** kitaplığını yükleyin. Bunu yaptığınızda MongoDB.Driver paketi ve tüm bağımlılıklar yüklenir.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın. Uygulamanız tarayıcınızda görüntülenir. 

5. Tarayıcıda **Yeni** düğmesine tıklayın ve görev listesi uygulamanızda birkaç yeni görev oluşturun.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Cosmos hesabı oluşturmayı, bir koleksiyon oluşturmayı ve bir konsol uygulamasını çalıştırmayı öğrendiniz. Artık Cosmos veritabanınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
