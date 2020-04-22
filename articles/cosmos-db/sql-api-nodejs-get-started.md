---
title: Azure Cosmos DB için SQL API için Node.js öğretici
description: SQL API ile Azure Cosmos DB bağlantısı kurma ve sorgulama yapma adımlarını gösteren bir Node.js öğreticisi
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: dech
Customer intent: As a developer, I want to build a Node.js console application to access and manage SQL API account resources in Azure Cosmos DB, so that customers can better use the service.
ms.openlocfilehash: 212dd243842a8bdacc8a77241f456795ef508d9e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731684"
---
# <a name="tutorial-build-a-nodejs-console-app-with-the-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Öğretici: Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK ile bir Düğüm.js konsol uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Geliştirici olarak, NoSQL belge verilerini kullanan uygulamalarınız olabilir. Bu belge verilerini depolamak ve bunlara erişmek için Azure Cosmos DB'deki bir SQL API hesabı kullanabilirsiniz. Bu öğretici, Azure Cosmos DB kaynaklarını oluşturmak ve bunları sorgulamak için bir Düğüm konsolu uygulamasını nasıl oluşturabileceğinizi gösterir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Bir Azure Cosmos DB hesabı oluşturun ve bağlanın.
> * Başvurunuzu ayarlayın.
> * Bir veritabanı oluşturun.
> * Bir kapsayıcı oluşturun.
> * Kapsayıcıya öğeler ekleyin.
> * Öğeler, kapsayıcı ve veritabanı üzerinde temel işlemleri gerçekleştirin.

## <a name="prerequisites"></a>Ön koşullar 

Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Düğüm.js](https://nodejs.org/) v6.0.0 veya daha yüksek.

## <a name="create-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın. 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="set-up-your-nodejs-application"></a><a id="SetupNode"></a>Node.js uygulamanızı ayarlama

Uygulamayı oluşturmak için kod yazmaya başlamadan önce, uygulamanız için çerçeve oluşturabilirsiniz. Çerçeve koduna sahip Düğüm.js uygulamanızı ayarlamak için aşağıdaki adımları çalıştırın:

1. Sık kullandığınız terminali açın.
2. Node.js uygulamanızı kaydetmek istediğiniz klasör veya dizini bulun.
3. Aşağıdaki komutlarla boş JavaScript dosyaları oluşturun:

   * Windows:
     * `fsutil file createnew app.js 0`
     * `fsutil file createnew config.js 0`
     * `md data`
     * `fsutil file createnew data\databaseContext.js 0`

   * Linux/OS X:
     * `touch app.js`
     * `touch config.js`
     * `mkdir data`
     * `touch data/databaseContext.js`

4. Bir `package.json` dosya oluşturun ve başlatma. Aşağıdaki komutu kullanın:
   * ```npm init -y```

5. npm aracılığıyla @azure/cosmos modülünü yükleyin. Aşağıdaki komutu kullanın:
   * ```npm install @azure/cosmos --save```

## <a name="set-your-apps-configurations"></a><a id="Config"></a>Uygulamanızın yapılandırmalarını ayarlama

Uygulamanız var olduğuna göre, uygulamanın Azure Cosmos DB ile konuşabildiğinden emin olmanız gerekir. Aşağıdaki adımlarda gösterildiği gibi birkaç yapılandırma ayarını güncelleyerek uygulamanızı Azure Cosmos DB ile konuşacak şekilde ayarlayabilirsiniz:

1. En sevdiğiniz metin düzenleyicisinde *config.js* dosyasını açın.

1. Aşağıdaki kod parçacıklarını *config.js* dosyasına kopyalayıp yapıştırın ve `endpoint` `key` özellikleri ve Azure Cosmos DB bitiş noktası URI ve birincil anahtarınızı ayarlayın. Veritabanı, kapsayıcı adları **Görevler** ve **Öğeler**olarak ayarlanır. Bu uygulama için kullanacağınız bölüm anahtarı **/kategoridir.**

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/config.js":::

   Azure [portalının](https://portal.azure.com) **Anahtarlar** bölmesinde bitiş noktası ve anahtar ayrıntılarını bulabilirsiniz.

   ![Azure portaldan anahtarları alma ekran görüntüsü][keys]

JavaScript SDK genel terimler *kapsayıcı* ve *öğe*kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. Önceki kod snippet'inde, `module.exports = config;` kod config nesnesini dışa aktarmak için kullanılır, böylece *app.js* dosyasında başvuruda bulunabilirsiniz.

## <a name="create-a-database-and-a-container"></a>Veritabanı ve kapsayıcı oluşturma

1. En sevdiğiniz metin düzenleyicisinde *databaseContext.js* dosyasını açın.

1. Aşağıdaki kodu *veritabanıContext.js* dosyasına kopyalayıp yapıştırın. Bu kod, Azure Cosmos hesabınızda zaten yoksa "Görevler", "Öğeler" veritabanı ve kapsayıcıyı oluşturan bir işlev tanımlar:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/data/databaseContext.js" id="createDatabaseAndContainer":::

   Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. Veritabanları sınıfının `createIfNotExists` işlevini kullanarak bir veritabanı **oluşturursunuz.** Kapsayıcı, SQL API durumunda JSON belgeleri olan öğelerden oluşur. `createIfNotExists` **Kapsayıcılar** sınıfından işlevi kullanarak veya işlev oluşturarak bir kapsayıcı oluşturursunuz. Kapsayıcı oluşturduktan sonra verileri depolayabilir ve sorgulayabilirsiniz.

   > [!WARNING]
   > Kapsayıcı oluşturmanın fiyatlandırma etkileri vardır. Ne yle karşılaşacağınızı bilmeniz için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

## <a name="import-the-configuration"></a>Yapılandırmayı içeri aktarma

1. En sevdiğiniz metin düzenleyicisinde *app.js* dosyasını açın.

1. Önceki adımlarda tanımladığınız `@azure/cosmos` modülü, yapılandırmayı ve veritabanıBağlamını almak için aşağıdaki kodu kopyalayıp yapıştırın. 

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="ImportConfiguration":::

## <a name="connect-to-the-azure-cosmos-account"></a>Azure Cosmos hesabına bağlanın

*app.js* dosyasında, daha önce kaydedilmiş bitiş noktasını ve yeni bir CosmosClient nesnesi oluşturmak için aşağıdaki kodu kopyalayıp yapıştırın.

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="CreateClientObjectDatabaseContainer":::

> [!Note]
> **Cosmos DB Emülatör'e**bağlanıyorsanız, düğüm işleminiz için TLS doğrulamasını devre dışı kılabilir:
>   ```javascript
>   process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
>   const client = new CosmosClient({ endpoint, key });
>   ```

Artık Azure Cosmos DB istemcisini başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.

## <a name="query-items"></a><a id="QueryItem"></a>Öğeleri sorgula

Azure Cosmos DB, her kapsayıcıda depolanan JSON öğelerine karşı zengin sorguları destekler. Aşağıdaki örnek kod, kapsayıcınızdaki öğelere karşı çalıştırabileceğiniz bir sorguyu gösterir. Sınıfın sorgu işlevini kullanarak öğeleri sorgulayabilirsiniz. `Items` Azure Cosmos hesabınızdaki öğeleri sorgulamak için *app.js* dosyasına aşağıdaki kodu ekleyin:

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="QueryItems":::

## <a name="create-an-item"></a><a id="CreateItem"></a>Öğe oluşturma

Bir öğe `Items` sınıfın oluşturma işlevi kullanılarak oluşturulabilir. SQL API'yi kullanırken, öğeler kullanıcı tanımlı (rasgele) JSON içeriği olan belgeler olarak yansıtılır. Bu öğreticide, görevler veritabanı içinde yeni bir öğe oluşturursunuz.

1. app.js dosyasında madde tanımını tanımlayın:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="DefineNewItem":::

1. Daha önce tanımlanmış öğeyi oluşturmak için aşağıdaki kodu ekleyin:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="CreateItem":::

## <a name="update-an-item"></a><a id="ReplaceItem"></a>Öğeyi güncelleştirme

Azure Cosmos DB öğelerin içeriğini değiştirmeyi destekler. Aşağıdaki kodu *app.js* dosyasına kopyalayıp yapıştırın. Bu kod kapsayıcıdan bir öğe alır ve *isComplete* alanını doğru olarak güncelleştirir.

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="UpdateItem":::

## <a name="delete-an-item"></a><a id="DeleteItem"></a>Öğeyi silme

Azure Cosmos DB, JSON öğelerini silmeyi destekler. Aşağıdaki kod, bir öğeyi kimliğine göre nasıl alacağını ve nasıl silebildiğini gösterir. Aşağıdaki kodu *app.js* dosyasına kopyalayıp yapıştırın:

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="DeleteItem":::

## <a name="run-your-nodejs-application"></a><a id="Run"></a>Node.js uygulamanızı çalıştırın

Kodunuzun son hali şu şekilde olmalıdır:

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js":::

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın:

```bash 
node app.js
```

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı aşağıdaki örnek metinle eşleşmelidir.

```
Created database:
Tasks

Created container:
Items

Querying container: Items
1 - Pick up apples and strawberries.

Created new item: 3 - Complete Cosmos DB Node.js Quickstart ⚡

Updated item: 3 - Complete Cosmos DB Node.js Quickstart ⚡
Updated isComplete to true

Deleted item with id: 3
```

## <a name="get-the-complete-nodejs-tutorial-solution"></a><a id="GetSolution"></a>Eksiksiz Node.js öğreticisi çözümünü edinme

Bu öğreticideki adımları tamamlamak için zamanın yoksa veya sadece kodu indirmek istiyorsanız, [bunu GitHub'dan](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )alabilirsiniz.

Bu makaledeki tüm kodu içeren başlangıç çözümlerini çalıştırmak için şunları yapmanız gerekir:

* Bir [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [Başlangıç](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) çözümü.

Projenin bağımlılıklarını npm üzerinden yükleyin. Aşağıdaki komutu kullanın:

* ```npm install``` 

Ardından, ```config.js``` dosyada, Adım 3'te açıklandığı gibi config.endpoint ve config.key değerlerini güncelleştirin: [Uygulamanızın yapılandırmalarını ayarlayın.](#Config)  

Ardından terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın:  

```bash  
node app.js 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara artık gerek kalmadığında, kaynak grubunu, Azure Cosmos DB hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için Azure Cosmos DB hesabı için kullandığınız kaynak grubunu seçin, **Sil'i**seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabını izleme](monitor-accounts.md)

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
