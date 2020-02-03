---
title: 'Hızlı başlangıç: Azure Cosmos DB SQL API hesabından sorgulamak için Node. js kullanma'
description: Azure Cosmos DB SQL API hesabına ve sorgu verilerine bağlanan bir uygulama oluşturmak için Node. js kullanma.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 11/19/2019
ms.author: dech
ms.openlocfilehash: 8df78df27ffb7e8bb8fc88567bd0b3d37be20488
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719509"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Hızlı başlangıç: Azure Cosmos DB SQL API hesabına bağlanmak ve veri sorgulamak için Node. js kullanma

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure Cosmos DB 'de [SQL API](sql-api-introduction.md) hesabına bağlanmak için Node. js uygulamasının nasıl kullanılacağı gösterilmektedir. Daha sonra verileri sorgulamak ve yönetmek için SQL sorguları Azure Cosmos DB kullanabilirsiniz. Bu makalede oluşturduğunuz Node. js uygulaması [SQL JavaScript SDK 'sını](sql-api-sdk-node.md)kullanır. Bu hızlı başlangıçta [JavaScript SDK](https://www.npmjs.com/package/@azure/cosmos) 2.0 sürümü kullanılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Buna ek olarak:
    * [Node.js](https://nodejs.org/en/) sürüm v6.0.0 veya üzeri
    * [Git](https://git-scm.com/)

## <a name="create-a-database"></a>Veritabanı oluşturma 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Kapsayıcı ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub 'dan bir Node. js uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım.

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Azure Cosmos veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmeye ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

JavaScript SDK'sının eski sürümlerini kullandıysanız 'koleksiyon' ve 'belge' terimlerine aşina olabilirsiniz. Azure Cosmos DB [birden fazla API modelini](https://docs.microsoft.com/azure/cosmos-db/introduction) desteklediğinden JavaScript SDK'sının 2.0 ve üzeri sürümlerinde kapsayıcının içeriğini tanımlamak için koleksiyon, grafik veya tablo olabilen 'kapsayıcı' ile 'öğe' terimleri kullanılmaktadır.

Aşağıdaki kod parçacıklarının tamamı, **app.js** dosyasından alınmıştır.

* `CosmosClient` nesnesi başlatıldı.

    ```javascript
    const client = new CosmosClient({ endpoint, key });
    ```

* Yeni bir Azure Cosmos veritabanı oluşturun.

    ```javascript
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    ```

* Veritabanı içinde yeni bir kapsayıcı (koleksiyon) oluşturulur.

    ```javascript
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    ```

* Bir öğe (belge) oluşturulur.

    ```javascript
    const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
    ```

* Aile veritabanında JSON üzerinden bir SQL sorgusu gerçekleştirilir. Sorgu "Anderson" ailesinin tüm alt öğelerini döndürür. 

    ```javascript
      const querySpec = {
        query: 'SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName',
        parameters: [
          {
            name: '@lastName',
            value: 'Andersen'
          }
        ]
      }

      const { resources: results } = await client
        .database(databaseId)
        .container(containerId)
        .items.query(querySpec)
        .fetchAll()
      for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult)
        console.log(`\tQuery returned ${resultString}\n`)
      }
    ```    

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Şimdi Azure Cosmos hesabınızın bağlantı dizesi ayrıntılarını almak için Azure portal geri dönün. Veritabanınıza bağlanabilmeleri için bağlantı dizesini uygulamaya kopyalayın.

1. [Azure Portal](https://portal.azure.com/)Azure Cosmos hesabınızda, sol gezinti bölmesinde **anahtarlar**' a ve ardından **okuma-yazma anahtarları**' na tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda `config.js` dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. `config.js` dosyasını açın. 

3. Portaldaki URI değerinizi kopyalayın (kopyalama düğmesini kullanarak) ve `config.js` dosyasına uç nokta değeri olarak yapıştırın. 

    `config.endpoint = "<Your Azure Cosmos account URI>"`

4. Ardından portaldaki BİRİNCİL ANAHTAR değerinizi kopyalayıp `config.key` dosyasına `config.js` değeri olarak yapıştırın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `config.key = "<Your Azure Cosmos account key>"`
    
## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Gerekli npm modüllerini yüklemek için bir terminalde `npm install` komutunu çalıştırın

2. Node.js uygulamanızı başlatmak için bir terminalde `node app.js` komutunu çalıştırın.

Artık Veri Gezgini, değiştirebilir ve bu yeni verilerle çalışabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos hesabı oluşturmayı, Veri Gezgini 'ni kullanarak bir kapsayıcı oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Artık Azure Cosmos veritabanınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


