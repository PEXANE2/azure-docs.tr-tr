---
title: Azure Cosmos DB SQL API hesabından sorgulamak için Node.js kullanın
description: Azure Cosmos DB SQL API hesabına bağlanan ve verileri sorgulayan bir uygulama oluşturmak için Node.js nasıl kullanılır?
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: dech
ms.openlocfilehash: 0b29f9c1f395e079c97d5877d08bd7bd73c7ea53
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80240308"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Quickstart: Azure Cosmos DB SQL API hesabından veri bağlamak ve sorgulamak için Node.js kullanın

> [!div class="op_single_selector"]
> - [.NET V3](create-sql-api-dotnet.md)
> - [.NET V4](create-sql-api-dotnet-V4.md)
> - [Java](create-sql-api-java.md)
> - [Node.js](create-sql-api-nodejs.md)
> - [Python](create-sql-api-python.md)
> - [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portalından ve GitHub'dan klonlanmış bir Node.js uygulamasını kullanarak bir Azure Cosmos DB SQL API hesabı oluşturabilir ve yönetebilirsiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="walkthrough-video"></a>Walkthrough video

Bu makaledeki içeriğin tam bir gözden geçirin.

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Quickstart-Use-Nodejs-to-connect-and-query-data-from-Azure-Cosmos-DB-SQL-API-account/player]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Ayrıca Bir URI `https://localhost:8081` ve anahtarı `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile Azure [Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator) kullanabilirsiniz.
- [Düğüm.js 6.0.0+](https://nodejs.org/).
- [Git.](https://www.git-scm.com/downloads)

## <a name="create-an-azure-cosmos-account"></a>Azure Cosmos hesabı oluşturma

Bu hızlı başlangıç amacıyla, azure cosmos hesabı oluşturmak için ücretsiz seçenek [için Azure Cosmos DB'yi deneyin'i](https://azure.microsoft.com/try/cosmosdb/) kullanabilirsiniz.

1. Ücretsiz sayfa [için Azure Cosmos DB'yi deneyin'e](https://azure.microsoft.com/try/cosmosdb/) gidin.

1. **SQL** API hesabını seçin ve **Oluştur'u**seçin. Microsoft hesabınızı kullanarak oturum açın.

1. Oturum açma başarılı olduktan sonra Azure Cosmos hesabınız hazır olmalıdır. Yeni oluşturulan hesabı açmak için **Azure portalında Aç'ı** seçin.

"Azure Cosmos DB'yi ücretsiz olarak deneyin" seçeneği Azure aboneliği gerektirmez ve size sınırlı bir süre için 30 günlük bir Azure Cosmos hesabı sunar. Azure Cosmos hesabını daha uzun bir süre kullanmak istiyorsanız, bunun yerine hesabı Azure aboneliğinizde [oluşturmanız](create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account) gerekir.

## <a name="add-a-container"></a>Kapsayıcı ekleme

Artık bir veritabanı ve kapsayıcı oluşturmak için Azure portalındaki Veri Gezgini aracını kullanabilirsiniz.

1. Veri Gezgini > **Yeni** **Kapsayıcı'yı**seçin.

   **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

   ![Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi](./media/create-sql-api-nodejs/azure-cosmosdb-data-explorer.png)

2. Kapsayıcı **Ekle** sayfasına yeni kapsayıcının ayarlarını girin.

   | Ayar           | Önerilen değer | Açıklama                                                                                                                                                                                                                                                                                                                                                                           |
   | ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | **Veritabanı Kimliği**   | Görevler           | Yeni veritabanınızın adı olarak _Görevler_ girin. Veritabanı adları 1 ile 255 arasında karakter içermelidir ve bunlar veya bir iz alanı içeremez. `/, \\, #, ?` Tedarik **veritabanı iş verme** seçeneğini denetleyin, veritabanına sağlanan iş veri tabanına sağlanan iş veri tabanı içindeki tüm kapsayıcılar arasında paylaşmanızı sağlar. Bu seçenek aynı zamanda maliyet tasarrufu ile yardımcı olur. |
   | **Aktarım hızı**    | 400             | Çıktıyı saniyede 400 istek biriminde (RU/s) bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.                                                                                                                                                                                                                                                    |
   | **Konteyner Kimliği**  | Öğeler           | Yeni kapsayıcınızın adı olarak _Öğeleri_ girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.                                                                                                                                                                                                                                                               |
   | **Bölüm anahtarı** | /kategori       | Bu makalede açıklanan örnek, _/kategoriyi_ bölüm anahtarı olarak kullanır.                                                                                                                                                                                                                                                                                                           |

   Önceki ayarlara ek olarak, isteğe bağlı olarak kapsayıcı için **Benzersiz tuşları** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Kapsayıcı oluştururken benzersiz bir anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerin benzersizliğini sağlarsınız. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md) makalesine bakın.

   **Tamam'ı**seçin. Veri Gezgini, yeni veritabanını ve kapsayıcıyı görüntüler.

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Düğüm.js uygulamasını klonlayalım, bağlantı dizesini ayarlayalım ve çalıştıralım.

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

   ```bash
   git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
   ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Azure Cosmos veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmek istiyorsanız, aşağıdaki parçacıkları inceleyebilirsiniz. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz.

SQL JavaScript SDK'nın önceki sürümünü biliyorsanız, _terimlerin toplanmasını_ ve _belgeyi_görmek için kullanılabilirsiniz. Azure Cosmos DB [birden çok API modelini](introduction.md) [desteklediğiiçin, JavaScript SDK'nın sürüm 2.0+](https://www.npmjs.com/package/@azure/cosmos) sürümü, kapsayıcının içeriğini açıklamak için koleksiyon, grafik veya tablo ve _öğe_ olabilecek genel terimler _kapsayıcısını_kullanır.

Cosmos DB JavaScript SDK@azure/cosmos" olarak adlandırılır ve npm......

```bash
npm install @azure/cosmos
```

Aşağıdaki kod parçacıklarının tamamı, _app.js_ dosyasından alınmıştır.

- NPM `CosmosClient` paketinden `@azure/cosmos` alınır.

  ```javascript
  const CosmosClient = require("@azure/cosmos").CosmosClient;
  ```

- Yeni `CosmosClient` bir nesne baş harfe getirilir.

  ```javascript
  const client = new CosmosClient({ endpoint, key });
  ```

- "Görevler" veritabanını seçin.

  ```javascript
  const database = client.database(databaseId);
  ```

- "Öğeler" kapsayıcısını/koleksiyonunu seçin.

  ```javascript
  const container = database.container(containerId);
  ```

- "Öğeler" kapsayıcısındaki tüm öğeleri seçin.

  ```javascript
  // query to return all items
  const querySpec = {
    query: "SELECT * from c"
  };

  const { resources: items } = await container.items
    .query(querySpec)
    .fetchAll();
  ```

- Yeni bir öğe oluşturun

  ```javascript
  const { resource: createdItem } = await container.items.create(newItem);
  ```

- Öğeyi güncelleştirme

  ```javascript
  const { id, category } = createdItem;

  createdItem.isComplete = true;
  const { resource: updatedItem } = await container
    .item(id, category)
    .replace(createdItem);
  ```

- Öğeyi silme

  ```javascript
  const { resource: result } = await container.item(id, category).delete();
  ```

> [!NOTE]
> Hem "güncelleştirme" hem de "sil" yöntemlerinde, öğenin veritabanından `container.item()`'' dilerek seçilmesi gerekir. Geçirilen iki parametre öğenin kimliği ve öğenin bölüm anahtarıdır. Bu durumda, parition anahtarı "kategori" alanının değeridir.

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Şimdi Azure Cosmos hesabınızın bağlantı dize ayrıntılarını almak için Azure portalına geri dön. Veritabanınıza bağlanabilmesi için bağlantı dizesini uygulamaya kopyalayın.

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda, sol daki **gezintiden Anahtarlar'ı** seçin ve ardından **Oku-Yaz Tuşlarını**seçin. Bir sonraki adımda URI ve Birincil Anahtarı _app.js_ dosyasına kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanın.

   ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. _Config.js_ dosyasını aç'ta.

3. URI değerinizi portaldan kopyalayın (kopyalama düğmesini kullanarak) ve _config.js'deki_uç nokta anahtarının değeri haline getirin.

   `endpoint: "<Your Azure Cosmos account URI>"`

4. Daha sonra PORTALDAN BIRINCIL KEY değeri kopyalayın `config.key` ve _config.js_değeri olun. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz.

   `key: "<Your Azure Cosmos account key>"`

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. " `npm install` npm@azure/cosmospaketini yüklemek için bir terminalde çalıştırın

2. Node.js uygulamanızı başlatmak için bir terminalde `node app.js` komutunu çalıştırın.

3. Bu hızlı başlatmada daha önce oluşturduğunuz iki öğe listelenir. Yeni bir öğe oluşturulur. Bu öğedeki "isComplete" bayrağı "true" olarak güncelleştirilir ve son olarak öğe silinir.

Bu örnek uygulamayla denemeler yapmaya devam edebilir veya Data Explorer'a geri dönebilir, verilerinizi değiştirebilir ve bunlarla çalışabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir kapsayıcı oluşturmayı ve bir Düğüm.js uygulamasını çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz.

> [!div class="nextstepaction"]
> [azure cosmos db içine veri alma](import-data.md)
