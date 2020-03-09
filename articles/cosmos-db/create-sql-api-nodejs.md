---
title: Hızlı başlangıç-Azure Cosmos DB SQL API hesabından sorgulamak için Node. js kullanma
description: Azure Cosmos DB SQL API hesabına ve sorgu verilerine bağlanan bir uygulama oluşturmak için Node. js kullanma.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: dech
ms.openlocfilehash: 729fd776321a90257289dcf92f13079a8206d9d9
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2020
ms.locfileid: "78927409"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-from-azure-cosmos-db-sql-api-account"></a>Hızlı başlangıç: Azure Cosmos DB SQL API hesabına bağlanmak ve veri sorgulamak için Node. js kullanma

> [!div class="op_single_selector"]
> - [.NET V3](create-sql-api-dotnet.md)
> - [.NET V4](create-sql-api-dotnet-V4.md)
> - [Java](create-sql-api-java.md)
> - [Node.js](create-sql-api-nodejs.md)
> - [Python](create-sql-api-python.md)
> - [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portal Azure Cosmos DB bir SQL API hesabı oluşturup, GitHub 'dan kopyalanmış bir Node. js uygulaması kullanarak oluşturursunuz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . [Azure Cosmos DB öykünücüsü](https://aka.ms/cosmosdb-emulator) ' nü BIR `https://localhost:8081` URI 'siyle ve anahtar `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`de kullanabilirsiniz.
- [Node. js 6.0.0 +](https://nodejs.org/).
- [Git](https://www.git-scm.com/downloads).

## <a name="create-an-azure-cosmos-account"></a>Azure Cosmos hesabı oluşturma

Bu hızlı başlangıç amacıyla Azure Cosmos hesabı oluşturmak için [Azure Cosmos DB dene seçeneğini ücretsiz](https://azure.microsoft.com/try/cosmosdb/) olarak kullanabilirsiniz.

1. [Ücretsiz deneme Azure Cosmos DB](https://azure.microsoft.com/try/cosmosdb/) sayfasına gidin.

1. **SQL** API hesabı ' nı seçin ve **Oluştur**' u seçin. Microsoft hesabı kullanarak oturum açın.

1. Oturum açma başarılı olduktan sonra Azure Cosmos hesabınız hazırlanmalıdır. Yeni oluşturulan hesabı açmak için **Azure Portal aç '** ı seçin.

"Ücretsiz Azure Cosmos DB dene" seçeneği bir Azure aboneliği gerektirmez ve 30 günlük sınırlı bir süre boyunca size bir Azure Cosmos hesabı sağlar. Azure Cosmos hesabını daha uzun bir süre için kullanmak istiyorsanız, hesabı Azure aboneliğinizde [oluşturmanız](create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account) gerekir.

## <a name="add-a-container"></a>Kapsayıcı ekleme

Artık bir veritabanı ve kapsayıcı oluşturmak için Azure portal Veri Gezgini aracı 'nı kullanabilirsiniz.

1. **Yeni kapsayıcı** > **Veri Gezgini** seçin.

   **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

   ![Azure portal Veri Gezgini, kapsayıcı bölmesi Ekle](./media/create-sql-api-nodejs/azure-cosmosdb-data-explorer.png)

2. **Kapsayıcı Ekle** sayfasında, yeni kapsayıcının ayarlarını girin.

   | Ayar           | Önerilen değer | Açıklama                                                                                                                                                                                                                                                                                                                                                                           |
   | ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | **Veritabanı Kimliği**   | Görevler           | Yeni veritabanınızın adı olarak _Görevler_ girin. Veritabanı adları 1 ila 255 karakterden oluşmalıdır ve `/, \\, #, ?`veya sonunda boşluk içermemelidir. Veritabanı **Işleme sağlama** seçeneğini kontrol edin, veritabanı içindeki tüm kapsayıcılar üzerinde veritabanı için sağlanan aktarım hızını paylaşmanıza olanak sağlar. Bu seçenek maliyet tasarruflarıyla de yardımcı olur. |
   | **Aktarım hızı**    | 400             | Aktarım hızını saniyede 400 istek birimi (RU/s) olarak bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.                                                                                                                                                                                                                                                    |
   | **Kapsayıcı KIMLIĞI**  | Öğeler           | _Öğeleri_ yeni kapsayıcının adı olarak girin. Kapsayıcı kimlikleri, veritabanı adlarıyla aynı karakter gereksinimlerine sahiptir.                                                                                                                                                                                                                                                               |
   | **Bölüm anahtarı** | /kategori       | Bu makalede açıklanan örnek, bölüm anahtarı olarak _/category_ kullanır.                                                                                                                                                                                                                                                                                                           |

   Önceki ayarlara ek olarak, kapsayıcı için isteğe bağlı olarak **benzersiz anahtarlar** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Bir kapsayıcı oluştururken benzersiz bir anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerin benzersizliği olduğundan emin olursunuz. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md) makalesine bakın.

   **Tamam**’ı seçin. Veri Gezgini yeni veritabanını ve kapsayıcıyı görüntüler.

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub 'dan bir Node. js uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım.

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

   ```bash
   git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started.git
   ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Azure Cosmos veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmeye ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz.

SQL JavaScript SDK 'sının önceki sürümüne alışkın değilseniz _, hüküm ve_ _belge_koşullarını görmek için kullanabilirsiniz. Azure Cosmos DB [birden çok API modelini](introduction.md)desteklediğinden, [JavaScript SDK 'sının 2.0 + sürümü](https://www.npmjs.com/package/@azure/cosmos) bir koleksiyon, grafik veya tablo olabilecek genel terimler _kapsayıcısını_ve kapsayıcının içeriğini açıklayan _öğeyi_ kullanır.

Cosmos DB JavaScript SDK 'Sı "@azure/cosmos" olarak adlandırılır ve NPM 'den yüklenebilir...

```bash
npm install @azure/cosmos
```

Aşağıdaki kod parçacıklarının tamamı, _app.js_ dosyasından alınmıştır.

- `CosmosClient`, `@azure/cosmos` NPM paketinden içeri aktarılır.

  ```javascript
  const CosmosClient = require("@azure/cosmos").CosmosClient;
  ```

- Yeni bir `CosmosClient` nesnesi başlatılır.

  ```javascript
  const client = new CosmosClient({ endpoint, key });
  ```

- "Görevler" veritabanını seçin.

  ```javascript
  const database = await client.databases(databaseId);
  ```

- "Öğeler" kapsayıcısını/toplamayı seçin.

  ```javascript
  const container = await client.databases(containerId);
  ```

- "Öğeler" kapsayıcısındaki tüm öğeleri seçin.

  ```javascript
  // query to return all items
  const querySpec = {
    query: "SELECT * from c"
  };

  const { resources: results } = await container.items
    .query(querySpec)
    .fetchAll();
  ```

- Yeni öğe oluştur

  ```javascript
  const { resource: createdItem } = await container.items.create(newItem);
  ```

- Öğe güncelleştirme

  ```javascript
  const { id, category } = createdItem;

  createdItem.isComplete = true;
  const { resource: itemToUpdate } = await container
    .item(id, category)
    .replace(itemToUpdate);
  ```

- Öğe silme

  ```javascript
  const { resource: result } = await this.container.item(id, category).delete();
  ```

> [!NOTE]
> Hem "güncelleştirme" hem de "silme" yöntemlerinde, `container.item()`çağırarak, öğe veritabanından seçilmelidir. Geçirilen iki parametre öğenin kimliği ve öğenin bölüm anahtarıdır. Bu durumda, Parma anahtarı "Category" alanının değeridir.

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Şimdi Azure Cosmos hesabınızın bağlantı dizesi ayrıntılarını almak için Azure portal geri dönün. Veritabanınıza bağlanabilmeleri için bağlantı dizesini uygulamaya kopyalayın.

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda, sol gezinti bölmesinde **anahtarlar** ' ı seçin ve ardından **okuma-yazma anahtarları**' nı seçin. Bir sonraki adımda _app. js_ dosyasına URI ve birincil anahtar kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanın.

   ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-sql-api-dotnet/keys.png)

2. İçinde, _config. js_ dosyasını açın.

3. Portaldan URI değerini kopyalayın (kopyalama düğmesini kullanarak) ve _config. js_' de bitiş noktası anahtarının değeri yapın.

   `endpoint: "<Your Azure Cosmos account URI>"`

4. Ardından portaldan BIRINCIL anahtar değerini kopyalayın ve _config. js_' de `config.key` değeri yapın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz.

   `key: "<Your Azure Cosmos account key>"`

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. "@azure/cosmos" NPM paketini yüklemek için bir terminalde `npm install` çalıştırma

2. Node.js uygulamanızı başlatmak için bir terminalde `node app.js` komutunu çalıştırın.

3. Bu hızlı başlangıçta daha önce oluşturduğunuz iki öğe listelenmiştir. Yeni bir öğe oluşturulur. Bu öğenin "ıstamamlanmıştır" bayrağı "true" olarak güncelleştirildiğinden öğe silinir.

Bu örnek uygulamayla denemeler yapmaya devam edebilir veya Veri Gezgini, değiştirebilir ve verilerle çalışabilirsiniz.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini kullanarak bir kapsayıcı oluşturmayı ve bir Node. js uygulamasını çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB 'ye veri aktarma](import-data.md)
