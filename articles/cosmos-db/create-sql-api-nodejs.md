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
ms.openlocfilehash: d8b17472bb531ec799be227706261962d7914d68
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77134475"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Hızlı başlangıç: Azure Cosmos DB SQL API hesabına bağlanmak ve veri sorgulamak için Node. js kullanma

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portal Azure Cosmos DB bir SQL API hesabı oluşturup, GitHub 'dan kopyalanmış bir Node. js uygulaması kullanarak oluşturursunuz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . [Azure Cosmos DB öykünücüsü](https://aka.ms/cosmosdb-emulator) ' nü BIR `https://localhost:8081` URI 'siyle ve anahtar `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`de kullanabilirsiniz.
- [Node. js 6.0.0 +](https://nodejs.org/).
- [Git](https://www.git-scm.com/downloads).

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

SQL JavaScript SDK 'sının önceki sürümüne alışkın değilseniz *, hüküm ve* *belge*koşullarını görmek için kullanabilirsiniz. Azure Cosmos DB [birden çok API modelini](introduction.md)desteklediğinden, [JavaScript SDK 'sının 2.0 + sürümü](https://www.npmjs.com/package/@azure/cosmos) bir koleksiyon, grafik veya tablo olabilecek genel terimler *kapsayıcısını*ve kapsayıcının içeriğini açıklayan *öğeyi* kullanır.

Aşağıdaki kod parçacıklarının tamamı, *app.js* dosyasından alınmıştır.

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

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda, sol gezinti bölmesinde **anahtarlar** ' ı seçin ve ardından **okuma-yazma anahtarları**' nı seçin. Bir sonraki adımda bulunan *config. js* dosyasına URI ve birincil anahtar kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. İçinde, *config. js* dosyasını açın. 

3. Portaldan URI değerini kopyalayın (kopyalama düğmesini kullanarak) ve *config. js*' de bitiş noktası anahtarının değeri yapın. 

    `config.endpoint = "<Your Azure Cosmos account URI>"`

4. Ardından portaldan BIRINCIL anahtar değerini kopyalayın ve *config. js*' de `config.key` değeri yapın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

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

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini kullanarak bir kapsayıcı oluşturmayı ve bir Node. js uygulamasını çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


