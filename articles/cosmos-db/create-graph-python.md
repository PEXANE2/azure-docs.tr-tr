---
title: 'Hızlı başlangıç: Python ile Gremlin API - Azure Cosmos DB'
description: Bu hızlı başlangıçta Azure portalı ve Python ile konsol uygulaması oluşturmak için Azure Cosmos DB Gremlin API’sinin nasıl kullanılacağı gösterilmektedir
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.devlang: python
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: lbosq
ms.openlocfilehash: b1286daaa76c71f88d44ea387a92876a8676783c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77062258"
---
# <a name="quickstart-create-a-graph-database-in-azure-cosmos-db-using-python-and-the-azure-portal"></a>Hızlı başlatma: Python ve Azure portalını kullanarak Azure Cosmos DB'de grafik veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Gremlin konsolu](create-graph-gremlin-console.md)
> * [.NET](create-graph-dotnet.md)
> * [Java](create-graph-java.md)
> * [Node.js](create-graph-nodejs.md)
> * [Python](create-graph-python.md)
> * [PHP](create-graph-php.md)
>  

Bu hızlı başlangıçta, Azure portalından bir Azure Cosmos DB Gremlin (grafik) API hesabı oluşturur ve yönetir siniz ve GitHub'dan klonlanmış bir Python uygulamasını kullanarak veri ekleyebilirsiniz. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/)
- [Pip](https://pip.pypa.io/en/stable/installing/) paket yükleyici dahil [Python 3.5+](https://www.python.org/downloads/)
- [Gremlin için Python Sürücüsü](https://github.com/apache/tinkerpop/tree/master/gremlin-python).
- [Git.](https://git-scm.com/downloads)

> [!NOTE]
> Bu hızlı başlangıç için 20 Aralık 2017’den sonra oluşturulmuş bir grafik veritabanı hesabı gerekir. Mevcut hesaplar genel kullanılabilirliğe geçirildikten sonra Python’u destekleyecektir.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir grafik veritabanı oluşturmadan önce Azure Cosmos DB ile bir Gremlin (Graf) veritabanı hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Graf ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub'dan bir Gremlin API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz.  

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örek uygulamayı yüklemek üzere bir klasör olarak değiştirmek için `cd` komutunu kullanın.  

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-python-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Parçacıklar *c:\git-samples\azure-cosmos-db-graph-python-getting-started\\ * klasöründeki *connect.py* dosyasından alınır. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-information) bölümüne atlayabilirsiniz. 

* Gremlin `client` *connect.py*satır 104 olarak başharflenir:

    ```python
    ...
    client = client.Client('wss://<YOUR_ENDPOINT>.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ...
    ```

* *connect.py* dosyasının başında bir dizi Gremlin adımı bildirilir. Bu adımlar daha sonra `client.submitAsync()` yöntemi kullanılarak yürütülür:

    ```python
    client.submitAsync(_gremlin_cleanup_graph)
    ```

## <a name="update-your-connection-information"></a>Bağlantı bilgilerinizi güncelleştirme

Şimdi, Azure portalına dönerek bağlantı bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu ayarlar, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda **Keys'i**seçin. 

    URI değerinin ilk parçasını kopyalayın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar sayfası](./media/create-graph-python/keys.png)

2. *connect.py* dosyasını açın ve satır 104 uri `<YOUR_ENDPOINT>` değerini buraya yapıştırın:

    ```python
    client = client.Client('wss://<YOUR_ENDPOINT>.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ```

    İstemci nesnesinin URI kısmı artık şu koda benzer görünmelidir:

    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/<YOUR_DATABASE>/colls/<YOUR_COLLECTION_OR_GRAPH>", 
        password="<YOUR_PASSWORD>")
    ```

3. `client` nesnesinin ikinci parametresini `<YOUR_DATABASE>` ve `<YOUR_COLLECTION_OR_GRAPH>` dizelerinin yerine geçecek şekilde değiştirin. Önerilen değerleri kullandıysanız, parametre şu kod gibi görünmelidir:

    `username="/dbs/sample-database/colls/sample-graph"`

    Tüm `client` nesnesi artık şu kod gibi görünmelidir:

    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/sample-database/colls/sample-graph", 
        password="<YOUR_PASSWORD>")
    ```

4. **Keys** sayfasında, PRIMARY KEY'i kopyalamak ve `<YOUR_PASSWORD>` `password=<YOUR_PASSWORD>` parametreye yapıştırmak için kopyalama düğmesini kullanın.

    Tüm `client` nesne tanımı artık şu kod gibi görünmelidir:
    ```python
    client = client.Client('wss://test.gremlin.cosmosdb.azure.com:443/','g', 
        username="/dbs/sample-database/colls/sample-graph", 
        password="asdb13Fadsf14FASc22Ggkr662ifxz2Mg==")
    ```

6. *connect.py* dosyasını kaydedin.

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Git terminal penceresinde `cd` komutuyla azure-cosmos-db-graph-python-getting-started klasörüne ulaşın.

    ```git
    cd "C:\git-samples\azure-cosmos-db-graph-python-getting-started"
    ```

2. Git terminal penceresinde aşağıdaki komutu kullanarak gerekli Python paketlerini yükleyin.

   ```
   pip install -r requirements.txt
   ```

3. Git terminal penceresinde, Python uygulamasını başlatmak için aşağıdaki komutları kullanın.
    
    ```
    python connect.py
    ```

    Terminal penceresinde grafiğe eklenmekte olan köşe ve kenarlar gösterilir. 
    
    Zaman aşımı hatası alırsanız, bağlantı bilgilerini, [Bağlantı bilgilerinizi güncelleştirme](#update-your-connection-information), konusunda belirtildiği şekilde güncelleştirdiğinizden emin olun ve son komutu çalıştırmayı yeniden deneyin. 
    
    Program durduktan sonra Enter tuşuna basın ve ardından İnternet tarayıcınızdaki Azure portalına geçin.

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Örnek verileri inceleme ve ekleme

Tepe noktaları ve kenarlar eklendikten sonra, artık Veri Gezgini'ne geri dönebilir ve grafiğe eklenen vertices'i görebilir ve ek veri noktaları ekleyebilirsiniz.

1. Azure portalındaki Azure Cosmos DB hesabınızda **Veri Gezgini'ni**seçin, **örnek grafiği**genişletin, **Grafiği**seçin ve ardından **Filtre Uygula'yı**seçin. 

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-graph-python/azure-cosmosdb-data-explorer-expanded.png)

2. **Sonuçlar** listesinde, grafiğe üç yeni kullanıcının eklenmiştir. Köşeleri sürükleyip bırakarak hareket ettirebilir, farenizin tekerleğini kaydırarak öğeleri yakınlaştırabilir ve uzaklaştırabilir, ayrıca çift okla grafiğin boyutunu genişletebilirsiniz. 

   ![Azure portalında Veri Gezgini'ndeki grafikte yeni köşeler](./media/create-graph-python/azure-cosmosdb-graph-explorer-new.png)

3. Şimdi birkaç yeni kullanıcı ekleyelim. Grafiğinize veri eklemek için **Yeni Vertex** düğmesini seçin.

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-graph-python/azure-cosmosdb-data-explorer-new-vertex.png)

4. *Kişi* etiketi girin.

5. Aşağıdaki özelliklerin her birini eklemek için **özellik ekle'yi** seçin. Graftaki her kişi için benzersiz özellikler oluşturabileceğinizi görürsünüz. Yalnızca kimliği anahtarı gereklidir.

    anahtar|value|Notlar
    ----|----|----
    Pk|/pk| 
    id|ashley|Köşe için benzersiz tanımlayıcı. Kimlik belirtmezseniz, bir kimlik otomatik olarak oluşturulur.
    cinsiyet|kadın| 
    teknoloji | java | 

    > [!NOTE]
    > Bu hızlı başlangıçta bölümlenmemiş bir koleksiyon oluşturun. Ancak koleksiyon oluşturma sırasında bir bölüm anahtarı belirterek bölümlendirilmiş bir koleksiyon oluşturursanız, daha sonra bölüm anahtarını her yeni köşede anahtar olarak eklemeniz gerekir. 

6. **Tamam'ı**seçin. Ekranın en altındaki **Tamam** seçeneğini görmek için ekranınızı genişletmeniz gerekebilir.

7. **Yeni Vertex'i** yeniden seçin ve ek bir yeni kullanıcı ekleyin. 

8. *Kişi* etiketi girin.

9. Aşağıdaki özelliklerin her birini eklemek için **Özellik Ekle'yi** seçin:

    anahtar|value|Notlar
    ----|----|----
    Pk|/pk| 
    id|rakesh|Köşe için benzersiz tanımlayıcı. Kimlik belirtmezseniz, bir kimlik otomatik olarak oluşturulur.
    cinsiyet|erkek| 
    okul|MIT| 

10. **Tamam'ı**seçin. 

11. Grafikteki **Apply Filter** tüm değerleri görüntülemek `g.V()` için varsayılan filtreyle Filtre Uygula düğmesini seçin. Tüm kullanıcılar **Sonuç listesinde** gösterilir. 

    Daha fazla veri ekledikçe sonuçlarınızı sınırlamak için filtreleri kullanabilirsiniz. Veri Gezgini, varsayılan olarak bir grafikteki tüm köşeleri almak için `g.V()` kullanır. JSON biçimindeki bir grafikteki tüm köşelerin sayımını döndürmek için, bu değeri `g.V().count()` gibi farklı bir [grafik sorgusu](tutorial-query-graph.md) olarak değiştirebilirsiniz. Filtreyi değiştirdiyseniz, filtreyi değiştirin ve tüm sonuçları yeniden görüntülemek için `g.V()` **Filtreyi Uygula'yı** seçin.

12. Artık rakesh ve ashley arasında bağlantı kurabiliriz. **Sonuçlar** listesinde **ashley'nin** seçildiğinden emin olun, ardından sağ alt taki **Hedefler'in** yanındaki edit düğmesini seçin. **Özellikler** alanını görmek için pencerenizi genişletmeniz gerekebilir.

    ![Hedef grafikteki bir köşeyi değiştirme](./media/create-graph-python/azure-cosmosdb-data-explorer-edit-target.png)

13. **Hedef** kutusu *türünde tırmık,* ve **Edge etiket** kutusu türü *bilir*ve sonra onay seçin.

    ![Veri Gezgininde ashley ve rakesh arasında bir bağlantı ekleyin](./media/create-graph-python/azure-cosmosdb-data-explorer-set-target.png)

14. Sonuç listesinden **rakesh**’i seçin, ashley ve rakesh’in bağlantılı olduğunu görürsünüz. 

    ![Veri Gezgini'nde bağlı iki köşe](./media/create-graph-python/azure-cosmosdb-graph-explorer.png)

Bu işlemle birlikte, bu öğreticideki kaynak oluşturma bölümünü tamamladınız. Grafiğinize köşe eklemeye, var olan köşeleri veya sorguları değiştirmeye devam edebilirsiniz. Şimdi, Azure Cosmos DB’nin sağladığı ölçümleri gözden geçirip kaynakları temizleyelim. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak bir grafik oluşturmayı ve grafiğe veri eklemek için bir Python uygulamasını çalıştırmayı öğrendiniz. Artık daha karmaşık sorgular derleyebilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)

