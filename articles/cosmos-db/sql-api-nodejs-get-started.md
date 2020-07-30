---
title: Azure Cosmos DB için SQL API Node.js öğreticisi
description: SQL API ile Azure Cosmos DB bağlantısı kurma ve sorgulama yapma adımlarını gösteren bir Node.js öğreticisi
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: dech
ms.custom: devx-track-javascript
ms.openlocfilehash: e86f79a286c10c45b2586dd1aa4d3aed035fc450
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421867"
---
# <a name="tutorial-build-a-nodejs-console-app-with-the-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Öğretici: Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK’sını kullanarak bir Node.js konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Geliştirici olarak, NoSQL belge verileri kullanan uygulamalarınız olabilir. Bu belge verilerini depolamak ve erişmek için Azure Cosmos DB bir SQL API hesabı kullanabilirsiniz. Bu öğreticide, Azure Cosmos DB kaynakları oluşturmak ve sorgulamak için bir Node.js konsol uygulamasının nasıl oluşturulacağı gösterilmektedir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure Cosmos DB bir hesap oluşturun ve bir hesabı bağlayın.
> * Uygulamanızı ayarlayın.
> * Bir veritabanı oluşturun.
> * Bir kapsayıcı oluşturun.
> * Kapsayıcıya öğe ekleyin.
> * Öğeler, kapsayıcı ve veritabanı üzerinde temel işlemleri gerçekleştirin.

## <a name="prerequisites"></a>Önkoşullar 

Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) v 6.0.0 veya üzeri.

## <a name="create-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluştur

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın. 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="set-up-your-nodejs-application"></a><a id="SetupNode"></a>Node.js uygulamanızı ayarlama

Uygulamayı derlemek için kod yazmaya başlamadan önce, uygulamanızın çerçevesini oluşturabilirsiniz. Framework koduna sahip Node.js uygulamanızı ayarlamak için aşağıdaki adımları çalıştırın:

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

4. Bir dosya oluşturun ve başlatın `package.json` . Aşağıdaki komutu kullanın:
   * ```npm init -y```

5. npm aracılığıyla @azure/cosmos modülünü yükleyin. Aşağıdaki komutu kullanın:
   * ```npm install @azure/cosmos --save```

## <a name="set-your-apps-configurations"></a><a id="Config"></a>Uygulamanızın yapılandırmasını ayarlama

Artık uygulamanız mevcut olduğuna göre, Azure Cosmos DB konuştuğunu unutmayın. Aşağıdaki adımlarda gösterildiği gibi, birkaç yapılandırma ayarını güncelleştirerek uygulamanızı Azure Cosmos DB konuşacak şekilde ayarlayabilirsiniz:

1. *config.js* dosyasını en sevdiğiniz metin düzenleyicisinde açın.

1. Aşağıdaki kod parçacığını kopyalayıp *config.js* dosyasına yapıştırın ve özellikleri `endpoint` ve `key` Azure Cosmos DB uç nokta URI 'si ve birincil anahtarınızı ayarlayın. Veritabanı, kapsayıcı adları **Görevler** ve **öğeler**olarak ayarlanır. Bu uygulama için kullanacağınız bölüm anahtarı **/category**' dir.

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/config.js":::

   Uç nokta ve anahtar ayrıntılarını [Azure Portal](https://portal.azure.com) **anahtarlar** bölmesinde bulabilirsiniz.

   :::image type="content" source="media/sql-api-nodejs-get-started/node-js-tutorial-keys.png" alt-text="Azure portaldan anahtarları alma ekran görüntüsü":::

JavaScript SDK 'Sı genel terimler *kapsayıcısını* ve *öğesini*kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. Önceki kod parçacığında, `module.exports = config;` kod, *app.js* dosyası içinde başvurabilmeniz için yapılandırma nesnesini dışarı aktarmak için kullanılır.

## <a name="create-a-database-and-a-container"></a>Veritabanı ve kapsayıcı oluşturma

1. *databaseContext.js* dosyasını en sevdiğiniz metin düzenleyicisinde açın.

1. Aşağıdaki kodu kopyalayıp *databaseContext.js* dosyasına yapıştırın. Bu kod, Azure Cosmos hesabınızda zaten mevcut değilse "görevler", "öğeler" veritabanını ve kapsayıcıyı oluşturan bir işlevi tanımlar:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/data/databaseContext.js" id="createDatabaseAndContainer":::

   Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. `createIfNotExists` **Veritabanları** sınıfının ya da Create işlevini kullanarak bir veritabanı oluşturursunuz. Bir kapsayıcı, SQL API 'si JSON belgeleri olduğu durumda olan öğelerden oluşur. `createIfNotExists` **Kapsayıcılar** sınıfından ya da Create işlevini kullanarak bir kapsayıcı oluşturursunuz. Bir kapsayıcı oluşturduktan sonra verileri depolayıp sorgulayabilirsiniz.

   > [!WARNING]
   > Bir kapsayıcının oluşturulması fiyatlandırma etkilerine sahiptir. Beklendiklerinizi bilmeniz için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/) sayfamızı ziyaret edin.

## <a name="import-the-configuration"></a>Yapılandırmayı içeri aktarma

1. *app.js* dosyasını en sevdiğiniz metin düzenleyicisinde açın.

1. `@azure/cosmos`Modül, yapılandırma ve önceki adımlarda tanımladığınız databaseContext içeri aktarmak için aşağıdaki kodu kopyalayın ve yapıştırın. 

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="ImportConfiguration":::

## <a name="connect-to-the-azure-cosmos-account"></a>Azure Cosmos hesabına bağlanma

*app.js* dosyasında, yeni bir CosmosClient nesnesi oluşturmak için önceden kaydedilmiş uç noktasını ve anahtarı kullanmak üzere aşağıdaki kodu kopyalayın ve yapıştırın.

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="CreateClientObjectDatabaseContainer":::

> [!Note]
> **Cosmos DB öykünücüye**bağlanıyorsanız, düğüm IŞLEMINIZ için TLS doğrulamasını devre dışı bırakın:
>   ```javascript
>   process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
>   const client = new CosmosClient({ endpoint, key });
>   ```

Artık Azure Cosmos DB istemcisini başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.

## <a name="query-items"></a><a id="QueryItem"></a>Sorgu öğeleri

Azure Cosmos DB, her kapsayıcıda depolanan JSON öğelerine yönelik zengin sorguları destekler. Aşağıdaki örnek kod, kapsayıcıınızda öğelerde çalıştırabileceğiniz bir sorgu gösterir. Sınıfının sorgu işlevini kullanarak öğeleri sorgulayabilirsiniz `Items` . Azure Cosmos hesabınızdan öğeleri sorgulamak için aşağıdaki kodu *app.js* dosyasına ekleyin:

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="QueryItems":::

## <a name="create-an-item"></a><a id="CreateItem"></a>Öğe oluşturma

Bir öğe, sınıfının oluşturma işlevi kullanılarak oluşturulabilir `Items` . SQL API 'sini kullanırken öğeler, Kullanıcı tanımlı (rastgele) JSON içeriği olan belgeler olarak yansıtılacaktır. Bu öğreticide, görevler veritabanı içinde yeni bir öğe oluşturacaksınız.

1. app.js dosyasında, öğe tanımını tanımlayın:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="DefineNewItem":::

1. Daha önce tanımlanan öğeyi oluşturmak için aşağıdaki kodu ekleyin:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="CreateItem":::

## <a name="update-an-item"></a><a id="ReplaceItem"></a>Öğeyi güncelleştirme

Azure Cosmos DB öğelerin içeriğini değiştirmeyi destekler. Aşağıdaki kodu kopyalayıp *app.js* dosyasına yapıştırın. Bu kod, kapsayıcıdan bir öğe alır ve *ıstamamlanma* alanını doğru olarak güncelleştirir.

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js" id="UpdateItem":::

## <a name="delete-an-item"></a><a id="DeleteItem"></a>Öğeyi silme

Azure Cosmos DB, JSON öğelerini silmeyi destekler. Aşağıdaki kod, bir öğenin KIMLIĞINE göre nasıl alınacağını ve silineceğini gösterir. Aşağıdaki kodu kopyalayıp *app.js* dosyasına yapıştırın:

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

Bu öğreticideki adımları tamamlamaya yönelik bir zaman yoksa veya yalnızca kodu indirmek istiyorsanız, [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )' dan edinebilirsiniz.

Bu makaledeki tüm kodu içeren başlangıç çözümünü çalıştırmak için şunlar gerekir:

* Bir [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [Başlangıç](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) çözümü.

Projenin bağımlılıklarını NPM aracılığıyla yükler. Aşağıdaki komutu kullanın:

* ```npm install``` 

Ardından, ```config.js``` dosyada, config. Endpoint ve config. Key değerlerini [3. Adım: uygulamanızın yapılandırmalarını ayarlama](#Config)bölümünde açıklandığı gibi güncelleştirin.  

Ardından terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın:  

```bash  
node app.js 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara artık ihtiyaç duyulmadığında, kaynak grubunu, Azure Cosmos DB hesabı ve tüm ilgili kaynakları silebilirsiniz. Bunu yapmak için Azure Cosmos DB hesabı için kullandığınız kaynak grubunu seçin, **Sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Cosmos DB bir hesabı izleme](monitor-accounts.md)

[create-account]: create-sql-api-dotnet.md#create-account
