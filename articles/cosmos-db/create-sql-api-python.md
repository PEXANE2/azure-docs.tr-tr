---
title: 'Hızlı başlangıç: Azure Cosmos DB SQL API hesabı kullanarak bir Python uygulaması oluşturma'
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78942629"
---
# <a name="quickstart-build-a-python-application-using-an-azure-cosmos-db-sql-api-account"></a>Hızlı başlangıç: Azure Cosmos DB SQL API hesabı kullanarak bir Python uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta, Azure portal ve Visual Studio Code içinden GitHub 'dan kopyalanmış bir Python uygulamasıyla Azure Cosmos DB bir SQL API hesabı oluşturup yönetirsiniz. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . [Azure Cosmos DB öykünücüsünü](https://aka.ms/cosmosdb-emulator) bir URI `https://localhost:8081` ve anahtar `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`ile de kullanabilirsiniz.
- İçinde `python` çalıştırılabilir ile `PATH` [Python 3.6 +](https://www.python.org/downloads/).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Visual Studio Code Için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python#overview).
- [Git](https://www.git-scm.com/downloads). 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Kapsayıcı ekleme

Artık bir veritabanı ve kapsayıcı oluşturmak için Azure portal Veri Gezgini aracı 'nı kullanabilirsiniz. 

1. Yeni **Veri Gezgini** > **kapsayıcı**seçin. 
    
    **Kapsayıcı Ekle** alanı en sağda görüntülenir, görmek için sağa kaydırmanız gerekebilir.

    ![Azure portalındaki Veri Gezgini, Kapsayıcı Ekle bölmesi](./media/create-sql-api-python/azure-cosmosdb-data-explorer.png)

2. **Kapsayıcı Ekle** sayfasında, yeni kapsayıcının ayarlarını girin.

    |Ayar|Önerilen değer|Açıklama
    |---|---|---|
    |**Veritabanı Kimliği**|Görevler|Yeni veritabanınızın adı olarak *Görevler* girin. Veritabanı adları 1 ila 255 karakterden oluşmalıdır ve boşluk içeremez veya sonunda boşluk olamaz `/, \\, #, ?`. Veritabanı **Işleme sağlama** seçeneğini kontrol edin, veritabanı içindeki tüm kapsayıcılar üzerinde veritabanı için sağlanan aktarım hızını paylaşmanıza olanak sağlar. Bu seçenek maliyet tasarruflarıyla de yardımcı olur. |
    |**Aktarım hızı**|400|Aktarım hızını saniyede 400 istek birimi (RU/s) olarak bırakın. Daha sonra gecikme süresini azaltmak isterseniz aktarım hızının ölçeğini artırabilirsiniz.| 
    |**Kapsayıcı KIMLIĞI**|Öğeler|*Öğeleri* yeni kapsayıcının adı olarak girin. Kapsayıcı kimliklerinin karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır.|
    |**Bölüm anahtarı**| /kategori| Bu makalede açıklanan örnek, bölüm anahtarı olarak */category* kullanır.|
    
    Önceki ayarlara ek olarak, kapsayıcı için isteğe bağlı olarak **benzersiz anahtarlar** ekleyebilirsiniz. Bu örnekte bu alanı boş bırakalım. Benzersiz anahtarlar sayesinde geliştiriciler veritabanına bir veri bütünlüğü katmanı ekleyebilir. Bir kapsayıcı oluştururken benzersiz bir anahtar ilkesi oluşturarak, bölüm anahtarı başına bir veya daha fazla değerin benzersizliği olduğundan emin olursunuz. Daha fazla bilgi edinmek için [Azure Cosmos DB'de benzersiz anahtarlar](unique-keys.md) makalesine bakın.
    
    **Tamam**’ı seçin. Veri Gezgini, yeni veritabanını ve kapsayıcıyı görüntüler.

## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir SQL API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Bu hızlı başlangıçta [Python SDK 'sının](https://pypi.org/project/azure-cosmos/#history)4 sürümü kullanılmaktadır.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```cmd
    md "git-samples"
    ```
   Bash istemi kullanıyorsanız, bunun yerine aşağıdaki komutu kullanmanız gerekir:

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

1. [Azure portal](https://portal.azure.com/)Azure Cosmos DB hesabınızda, sol gezinti bölmesinde **anahtarlar** ' ı seçin. Bir sonraki adımda *cosmos_get_started.* Kopyala dosyasına **URI** ve **birincil anahtar** kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanın.

    ![Azure portal anahtarlar ayarlarından bir erişim anahtarı ve URI alın](./media/create-sql-api-dotnet/access-key-and-uri-in-keys-settings-in-the-azure-portal.png)

2. Visual Studio Code içinde *cosmos_get_started. Kopyala* dosyasını *\git-samples\azure-Cosmos-DB-Python-Getting-Started*içinde açın.

3. Portaldan **URI** değerini kopyalayın (kopyalama düğmesini kullanarak) ve *cosmos_get_started.* Kopyala içindeki **uç nokta** değişkeninin değerini yapın. 

    `endpoint = 'https://FILLME.documents.azure.com',`

4. Ardından, portaldan **BIRINCIL anahtar** değerini kopyalayın ve *cosmos_get_started. Kopyala*içindeki **anahtar** değerini yapın. Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

    `key = 'FILLME'`

5. *Cosmos_get_started. Kopyala* dosyasını kaydedin.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodda oluşturulan veritabanı kaynakları hakkında bilgi edinin veya [Bağlantı dizenizi güncelleştirmek](#update-your-connection-string)için bir adım atlayın.

Aşağıdaki kod parçacıklarının hepsi *cosmos_get_started. Kopyala* dosyasından alınmıştır.

* CosmosClient başlatılır. [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümünde açıklandığı gibi "Endpoint" ve "Key" değerlerini güncelleştirdiğinizden emin olun. 

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_cosmos_client)]

* Yeni bir veritabanı oluşturulur.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_database_if_not_exists)]

* [Sağlanan aktarım hızı](request-units.md)400 ru/sn ile yeni bir kapsayıcı oluşturulur. Bu özelliğe `lastName` filtre uygulayan etkili sorgular yapabilmemiz için [bölüm anahtarı](partitioning-overview.md#choose-partitionkey)olarak tercih ederiz. 

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_container_if_not_exists)]

* Kapsayıcıya bazı öğeler eklenir. Kapsayıcılar, değişen şemaya sahip olabilecek öğelerin (JSON belgeleri) bir koleksiyonudur. Yardımcı yöntemler ```get_[name]_family_item``` , Azure Cosmos DB JSON belgeleri olarak depolanan bir aileyi döndürür.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=create_item)]

* Nokta okuma (anahtar değeri aramaları) `read_item` yöntemi kullanılarak gerçekleştirilir. Her bir işlemin [ru ücreti](request-units.md) yazdırılsın.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=read_item)]

* SQL sorgu söz dizimi kullanılarak bir sorgu gerçekleştirilir. WHERE yan tümcesinde bölüm anahtarı değerlerini kullandığımızda Azure Cosmos DB, bu sorguyu ilgili bölümlere etkin bir ```lastName``` şekilde yönlendirerek performansı geliştirir.

    [!code-python[](~/azure-cosmos-db-python-getting-started/cosmos_get_started.py?name=query_items)]
   
## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Visual Studio Code, **görüntüleme** > **komut paleti**' ni seçin. 

2. Komut isteminde, **Python: Yorumlayıcı Seçin** girip kullanılacak Python sürümün seçin.

    Visual Studio Code’daki alt bilgi, seçilen yorumlayıcıyı belirtmek için güncelleştirilir. 

3. Tümleşik **View** > **terminalyi** görüntüle ' yi seçerek Visual Studio Code tümleşik Terminal ' i açın.

4. Tümleşik Terminal penceresinde, *Azure-Cosmos-DB-Python-alma-başlatma* klasöründe olduğunuzdan emin olun. Değilseniz, örnek klasörü değiştirmek için aşağıdaki komutu çalıştırın. 

    ```cmd
    cd "\git-samples\azure-cosmos-db-python-getting-started"`
    ```

5. azure-cosmos package'i yüklemek için aşağıdaki komutu çalıştırın. 

    ```python
    pip install --pre azure-cosmos
    ```

    azure-cosmos'u yüklemeye çalışırken erişim engellendi hatası alırsanız, [VS Code’u yönetici olarak çalıştırmanız gerekir](https://stackoverflow.com/questions/37700536/visual-studio-code-terminal-how-to-run-a-command-with-administrator-rights).

6. Örneği çalıştırmak ve Azure Cosmos DB yeni belgeler oluşturmak ve depolamak için aşağıdaki komutu çalıştırın.

    ```python
    python cosmos_get_started.py
    ```

7. Oluşturulan ve kaydedilen yeni öğeleri onaylamak için, Azure Portal **Veri Gezgini** > **AzureSampleFamilyDatabase** > **öğelerini**seçin. Oluşturulan öğeleri görüntüleyin. Örneğin, Andersen ailesi için örnek bir JSON belgesi aşağıda verilmiştir:
   
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

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini kullanarak bir kapsayıcı oluşturmayı ve Visual Studio Code bir Python uygulamasını çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [SQL API'si için Azure Cosmos DB'ye veri aktarma](import-data.md)


