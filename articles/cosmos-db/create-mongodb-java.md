---
title: "Hızlı başlangıç: Mongo DB ve Java SDK için Azure Cosmos DB API'sini kullanarak bir web uygulaması oluşturun"
description: Azure Cosmos DB'nin MongoDB için API'sini kullanarak bağlanmak ve sorgulamak için kullanabileceğiniz bir Java kodu örneği oluşturmayı öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: java
ms.topic: quickstart
ms.date: 12/26/2018
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 35c6944ddcfac1553ffb2c1cc28472f2a56d4515
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77061735"
---
# <a name="quickstart-create-a-console-app-with-java-and-the-mongodb-api-in-azure-cosmos-db"></a>Quickstart: Azure Cosmos DB'de Java ve MongoDB API ile bir konsol uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Bu hızlı başlangıçta, Azure portalından MongoDB API hesabı için bir Azure Cosmos DB oluşturup yönetebilir ve GitHub'dan klonlanmış bir Java SDK uygulamasını kullanarak veri ekleyebilirsiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Bağlantı dizesini `.mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true`içeren [Azure Cosmos DB Emülatörü](https://aka.ms/cosmosdb-emulator) de kullanabilirsiniz.
- [Java Geliştirme Kiti (JDK) sürüm 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk). 
- [Maven,](https://maven.apache.org/download.cgi)ne kadar. Ya `apt-get install maven` da Maven yüklemek için çalıştırın.
- [Git.](https://git-scm.com/downloads) 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

Yeni veritabanı **db**ve yeni koleksiyon **coll**adı .

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)] 

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir uygulamayı klonlayalım, bağlantı dizesini ayarlayalım ve çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Daha sonra kodu sık kullandığınız düzenleyicinizde açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

Aşağıdaki parçacıkların tümü *Program.java* dosyasından alınmıştır.

Bu konsol uygulaması [MongoDB Java sürücüsünü](https://docs.mongodb.com/ecosystem/drivers/java/)kullanır. 

* DocumentClient başlatılır.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* Yeni bir veritabanı ve koleksiyonu oluşturulur.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* `MongoCollection.insertOne` kullanılarak birkaç belge eklenir

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* `MongoCollection.find` kullanılarak birkaç sorgu gerçekleştirilir

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. Azure Cosmos DB hesabınızdan **Hızlı Başlangıç,** **Java'yı**seçin ve ardından bağlantı dizesini panonuza kopyalayın.

2. *Program.java* dosyasını açın, bağımsız değişkeni bağlantı dizesiyle MongoClientURI oluşturucusu olarak değiştirin. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 
    
## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Gerekli npm modüllerini yüklemek için bir terminalde `mvn package` komutunu çalıştırın

2. Java uygulamanızı başlatmak için bir terminalde `mvn exec:java -D exec.mainClass=GetStarted.Program` komutunu çalıştırın.

Artık [robomongo](mongodb-robomongo.md) / [Studio 3T'yi](mongodb-mongochef.md) bu yeni verileri sorgulamak, değiştirmek ve çalışmak için kullanabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Mongo DB hesabı için bir Azure Cosmos DB API oluşturmayı, Data Explorer'ı kullanarak bir veritabanı ve kapsayıcı eklemeyi ve Java konsolu uygulamasını kullanarak veri eklemeyi öğrendiniz. Artık Cosmos veritabanınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
