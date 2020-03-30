---
title: 'Hızlı başlangıç: Azure Cosmos DB SQL API hesabını kullanarak bir Python uygulaması oluşturma'
description: Azure Cosmos DB SQL API'sine bağlanmak ve sorgu göndermek için kullanabileceğiniz bir Python kodu örneği sunar
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: quickstart
ms.date: 03/09/2020
ms.author: sngun
ms.custom:
- seodec18
- seo-javascript-september2019
- seo-python-october2019
ms.openlocfilehash: 10247e22b3fbe1250a15b06a0cce974905ca6b7f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78942629"
---
# <a name="quickstart-build-a-python-application-using-an-azure-cosmos-db-sql-api-account"></a>Hızlı başlatma: Azure Cosmos DB SQL API hesabını kullanarak Python uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portalından ve Görsel Studio Kodu'ndan GitHub'dan klonlanmış bir Python uygulamasıyla bir Azure Cosmos DB SQL API hesabı oluşturur ve yönetirsiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/) Ayrıca Bir URI `https://localhost:8081` ve anahtarı `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile Azure [Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator) kullanabilirsiniz.
- [Python 3.6+](https://www.python.org/downloads/), `python` sizin `PATH`.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
- [Visual Studio Code için Python uzantısı.](https://marketplace.visualstudio.com/items?itemName=ms-python.python#overview)
- [Git.](https://www.git-scm.com/downloads) 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Kapsayıcı ekleme

Artık bir veritabanı ve kapsayıcı oluşturmak için Azure portalındaki Veri Gezgini aracını kullanabilirsiniz. 

1. Veri Gezgini > **Yeni** **Kapsayıcı'yı**seçin. 
    
    **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi](./media/create-sql-api-python/azure-cosmosdb-data-explorer.png)

2. Kapsayıcı **Ekle** sayfasına yeni kapsayıcının ayarlarını girin.

    |Ayar|Önerilen değer|Açıklama
    |---|---|---|
    |**Veritabanı Kimliği**|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adları 1 ile 255 arasında karakter içermelidir ve bunlar veya bir iz alanı içeremez. `/, \\, #, ?` Tedarik **veritabanı iş verme** seçeneğini denetleyin, veritabanına sağlanan iş veri tabanına sağlanan iş veri tabanı içindeki tüm kapsayıcılar arasında paylaşmanızı sağlar. Bu seçenek aynı zamanda maliyet tasarrufu ile yardımcı olur. |
    |**Aktarım hızı**|400|Çıktıyı saniyede 400 istek biriminde (RU/s) bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.| 
    |**Konteyner Kimliği**|Öğeler|Yeni kapsayıcınızın adı olarak *Öğeleri* girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.|
    |**Bölüm anahtarı**| /kategori| Bu makalede açıklanan örnek, */kategoriyi* bölüm anahtarı olarak kullanır.|
    
    Önceki ayarlara ek olarak, isteğe bağlı olarak kapsayıcı için **Benzersiz tuşları** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Kapsayıcı oluştururken benzersiz bir anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerin benzersizliğini sağlarsınız. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md) makalesine bakın.
    
    **Tamam'ı**seçin. Veri Gezgini, yeni veritabanını ve kapsayıcıyı görüntüler.

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir SQL API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Bu quickstart [Python SDK](https://pypi.org/project/azure-cosmos/#history)sürümü 4 kullanır.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```cmd
    md "git-samples"
    ```
   Bir bash istemi kullanıyorsanız, bunun yerine aşağıdaki komutu kullanmalısınız:

   ```bash
   mkdir "git-samples"
   ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-python-getting-started.git
    ```  

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda sol daki gezintiden **Anahtarlar'ı** seçin. **Uri** ve **Birincil Anahtarı** bir sonraki adımda *cosmos_get_started.py* dosyasına kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanın.

    ![Azure portalındaki Anahtar ayarlarında bir erişim anahtarı ve URI alın](./media/create-sql-api-dotnet/access-key-and-uri-in-keys-settings-in-the-azure-portal.png)

2. Visual Studio Code'da *cosmos_get_started.py* dosyasını *\git-samples\azure-cosmos-db-python-getting-started'da*açın.

3. **URI** değerinizi portaldan kopyalayın (kopyalama düğmesini kullanarak) ve *cosmos_get_started.py'deki* **uç nokta** değişkeninin değeri haline getirin. 

    `endpoint = 'https://FILLME.documents.azure.com',`

4. Daha sonra **BIRINCIL KEY** değerinizi portaldan kopyalayın ve *cosmos_get_started.py'deki* **anahtarın** değeri haline getirin. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `key = 'FILLME'`

5. *cosmos_get_started.py* dosyasını kaydedin.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodda oluşturulan veritabanı kaynakları hakkında bilgi edinin veya [bağlantı dizenizi güncelleştirmek](#update-your-connection-string)için ileri ye atlayın.

Aşağıdaki parçacıkların tümü *cosmos_get_started.py* dosyasından alınır.

* CosmosClient başlatılır. [Bağlantı dizenizi güncelleştir](#update-your-connection-string) bölümünde açıklandığı gibi "uç nokta" ve "anahtar" değerlerini güncelleştirdiğinizden emin olun. 

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_cosmos_client)]

* Yeni bir veritabanı oluşturulur.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_database_if_not_exists)]

* Yeni bir konteyner, 400 RU / s [sağlanan iş kadar lığı](request-units.md)ile oluşturulur. Bu `lastName` özellik üzerinde filtre verimli sorgular yapmanızı sağlayan [bölüm anahtarı](partitioning-overview.md#choose-partitionkey)olarak seçin. 

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_container_if_not_exists)]

* Kapsayıcıya bazı öğeler eklenir. Kapsayıcılar çeşitli şema olabilir öğeleri (JSON belgeleri) bir koleksiyon vardır. Yardımcı yöntemler, ```get_[name]_family_item``` Azure Cosmos DB'de json belgeleri olarak depolanan bir ailenin temsillerini döndürer.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_item)]

* Nokta okumaları (anahtar değer aramaları) `read_item` yöntemi kullanılarak gerçekleştirilir. Her operasyonun [RU şarjını](request-units.md) yazdırıyoruz.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=read_item)]

* Bir sorgu SQL sorgusu sözdizimi kullanılarak gerçekleştirilir. WHERE yan tümcesinin ```lastName``` bölüm anahtar değerlerini kullandığımız için, Azure Cosmos DB bu sorguyu verimli bir şekilde ilgili bölümlere yönlendirerek performansı artırır.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=query_items)]
   
## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio Code'da**Komut Paletini** **Görüntüle'yi** > seçin. 

2. Komut isteminde, **Python: Yorumlayıcı Seçin** girip kullanılacak Python sürümün seçin.

    Visual Studio Code’daki alt bilgi, seçilen yorumlayıcıyı belirtmek için güncelleştirilir. 

3. Visual Studio Code entegre terminalini açmak için**Entegre Terminali** **Görüntüle'yi** > seçin.

4. Tümleşik terminal penceresinde, *azure-cosmos-db-python-getting-started klasöründe* olduğunuzdan emin olun. Değilseniz, örnek klasörü değiştirmek için aşağıdaki komutu çalıştırın. 

    ```cmd
    cd "\git-samples\azure-cosmos-db-python-getting-started"`
    ```

5. azure-cosmos package'i yüklemek için aşağıdaki komutu çalıştırın. 

    ```python
    pip install --pre azure-cosmos
    ```

    azure-cosmos'u yüklemeye çalışırken erişim engellendi hatası alırsanız, [VS Code’u yönetici olarak çalıştırmanız gerekir](https://stackoverflow.com/questions/37700536/visual-studio-code-terminal-how-to-run-a-command-with-administrator-rights).

6. Örneği çalıştırmak ve Azure Cosmos DB'de yeni belgeler oluşturmak ve depolamak için aşağıdaki komutu çalıştırın.

    ```python
    python cosmos_get_started.py
    ```

7. Yeni öğelerin oluşturulduğunu ve kaydedildiğini doğrulamak için Azure portalında **Veri Gezgini** > **AzureSampleFamilyDatabase** > **Öğeleri'ni**seçin. Oluşturulan öğeleri görüntüleyin. Örneğin, andersen ailesi için örnek bir JSON belgesi aşağıda verilmiştir:
   
   ```json
   {
       "id": "Andersen-1569479288379",
       "lastName": "Andersen",
       "district": "WA5",
       "parents": [
           {
               "familyName": null,
               "firstName": "Thomas"
           },
           {
               "familyName": null,
               "firstName": "Mary Kay"
           }
       ],
       "children": null,
       "address": {
           "state": "WA",
           "county": "King",
           "city": "Seattle"
       },
       "registered": true,
       "_rid": "8K5qAIYtZXeBhB4AAAAAAA==",
       "_self": "dbs/8K5qAA==/colls/8K5qAIYtZXc=/docs/8K5qAIYtZXeBhB4AAAAAAA==/",
       "_etag": "\"a3004d78-0000-0800-0000-5d8c5a780000\"",
       "_attachments": "attachments/",
       "_ts": 1569479288
   }
   ```

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir kapsayıcı oluşturmayı ve Visual Studio Code'da bir Python uygulamasını çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [SQL API'si için Azure Cosmos DB'ye veri aktarma](import-data.md)


