---
title: Öğretici-Azure Databricks bir Cosmos DB uç noktası ile uygulama
description: Bu öğreticide, Cosmos DB için etkin bir hizmet uç noktası ile sanal bir ağda Azure Databricks nasıl uygulanacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: e18c2b0f03f9ac2155c441580d62d6085581de12
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83779592"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Öğretici: Cosmos DB uç noktası ile Azure Databricks uygulama

Bu öğreticide, Cosmos DB için etkinleştirilmiş bir hizmet uç noktası ile VNet 'e eklenmiş Databricks ortamının nasıl uygulanacağı açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal ağda Azure Databricks çalışma alanı oluşturma
> * Cosmos DB hizmeti uç noktası oluşturma
> * Cosmos DB hesabı oluşturma ve verileri içeri aktarma
> * Azure Databricks kümesi oluşturma
> * Azure Databricks bir not defterinden sorgu Cosmos DB

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri yapın:

* [Bir sanal ağda Azure Databricks çalışma alanı](quickstart-create-databricks-workspace-vnet-injection.md)oluşturun.

* [Spark bağlayıcısını](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar)indirin.

* [Çevresel bilgiler Için NOAA National Centers](https://www.ncdc.noaa.gov/stormevents/)örnek verileri indirin. Bir durum veya alan seçin ve **Ara**' yı seçin. Sonraki sayfada, Varsayılanları kabul edin ve **Ara**' yı seçin. Ardından, sonuçları indirmek için sayfanın sol tarafındaki **CSV indir** ' i seçin.

* Azure Cosmos DB Data Migration aracının [önceden derlenmiş ikilisini](https://aka.ms/csdmtool) indirin.

## <a name="create-a-cosmos-db-service-endpoint"></a>Cosmos DB hizmeti uç noktası oluşturma

1. Bir sanal ağa Azure Databricks çalışma alanı dağıttıktan sonra, [Azure Portal](https://portal.azure.com)sanal ağa gidin. Databricks dağıtımı aracılığıyla oluşturulmuş ortak ve özel alt ağlara dikkat edin.

   ![Sanal ağ alt ağları](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. *Ortak alt ağı* seçin ve bir Cosmos DB hizmeti uç noktası oluşturun. Sonra **kaydedin**.
   
   ![Cosmos DB hizmeti uç noktası ekleme](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma

1. Azure portalı açın. Ekranın sol üst tarafında **Azure Cosmos DB > veritabanları > oluştur**' u seçin.

2. **Temel bilgiler** sekmesinde aşağıdaki ayarlarla **örnek ayrıntılarını** doldurun:

   |Ayar|Değer|
   |-------|-----|
   |Abonelik|*aboneliğiniz*|
   |Kaynak Grubu|*Kaynak grubunuz*|
   |Hesap Adı|DB-VNET-hizmet uç noktası|
   |API|Çekirdek (SQL)|
   |Konum|Batı ABD|
   |Coğrafi Yedeklilik|Devre Dışı Bırak|
   |Birden Çok Bölgeli Yazmalar|Etkinleştir|

   ![Cosmos DB hizmeti uç noktası ekleme](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. **Ağ** sekmesini seçin ve Sanal ağınızı yapılandırın. 

   a. Bir önkoşul olarak oluşturduğunuz sanal ağı seçin ve ardından *genel alt ağ*' ı seçin. *Özel-alt ağın* *' Microsoft Azuversmosdb ' uç noktası eksik '* notuna sahip olduğuna dikkat edin. Bunun nedeni, *genel alt ağda*yalnızca Cosmos DB hizmet uç noktasını etkinleştirmiş olmanız.

   b. **Azure Portal etkin erişime Izin verin** . Bu ayar, Azure portal Cosmos DB hesabınıza erişmenizi sağlar. Bu seçenek **Reddet**olarak ayarlandıysa, hesabınıza erişmeye çalışırken hatalarla karşılaşırsınız. 

   > [!NOTE]
   > Bu öğretici için gerekli değildir, ancak Cosmos DB hesabınıza yerel makinenizden erişme olanağına sahip olmak istiyorsanız *IP 'den erişime Izin ver* ' i de etkinleştirebilirsiniz. Örneğin, Cosmos DB SDK kullanarak hesabınıza bağlanıyorsanız, bu ayarı etkinleştirmeniz gerekir. Devre dışıysa, "erişim engellendi" hataları alırsınız.

   ![Cosmos DB hesabı ağ ayarları](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. Bir sanal ağ içinde Cosmos DB hesabınızı oluşturmak için **gözden geçir + oluştur**' u ve ardından **Oluştur** ' u seçin.

5. Cosmos DB Hesabınız oluşturulduktan sonra **Ayarlar**' ın altındaki **anahtarlar** ' a gidin. Birincil bağlantı dizesini kopyalayın ve daha sonra kullanmak üzere bir metin düzenleyicisinde kaydedin.

    ![Cosmos DB hesap anahtarları sayfası](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Cosmos DB hesabınıza yeni bir veritabanı ve koleksiyon eklemek için **Veri Gezgini** ve **Yeni koleksiyon** ' ı seçin.

    ![Yeni koleksiyon Cosmos DB](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Cosmos DB verileri karşıya yükleme

1. Cosmos DB, **dtuı. exe** [için veri taşıma aracının](https://aka.ms/csdmtool)grafik arabirim sürümünü açın.

    ![Cosmos DB Veri Geçişi Aracı](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. **Kaynak bilgileri** sekmesinde, açılan **listeden Içeri aktarma** bölümünde **CSV dosyaları** ' nı seçin. Ardından, **Dosya Ekle** ' yi seçin ve bir önkoşul olarak indirmiş olduğunuz FıRTıNASı verisi CSV 'yi ekleyin.

    ![Cosmos DB veri geçiş aracı kaynak bilgileri](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. **Hedef bilgileri** sekmesinde, Bağlantı dizenizi girin. Bağlantı dizesi biçimi `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>` . AccountEndpoint ve AccountKey, önceki bölümde kaydettiğiniz birincil bağlantı dizesine dahildir. `Database=<your database name>`Bağlantı dizesinin sonuna ekleyin ve **Doğrula**' yı seçin. Ardından, koleksiyon adını ve bölüm anahtarını ekleyin.

    ![Cosmos DB veri geçiş aracı hedef bilgileri](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Özet sayfasına ulaşana kadar **İleri ' yi** seçin. Ardından **Içeri aktar**' ı seçin.

## <a name="create-a-cluster-and-add-library"></a>Küme oluşturma ve kitaplık ekleme

1. [Azure portal](https://portal.azure.com) Azure Databricks hizmetinize gidin ve **çalışma alanını Başlat**' ı seçin.

2. Yeni bir küme oluşturun. Bir küme adı seçin ve kalan varsayılan ayarları kabul edin.

   ![Yeni küme ayarları](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Kümeniz oluşturulduktan sonra, küme sayfasına gidin ve **Kitaplıklar** sekmesini seçin. kitaplığı yüklemek Için **Yeni Yükle ' yi** seçin ve Spark Bağlayıcısı jar dosyasını karşıya yükleyin.

    ![Spark bağlayıcı kitaplığı 'nı yükler](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    Kitaplığın **Kitaplıklar** sekmesinde yüklü olduğunu doğrulayabilirsiniz.

    ![Databricks küme kitaplıkları sekmesi](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Databricks Not defterinden sorgu Cosmos DB

1. Azure Databricks çalışma alanınıza gidin ve yeni bir Python Not defteri oluşturun.

    ![Yeni Databricks Not Defteri Oluştur](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Cosmos DB bağlantı yapılandırmasını ayarlamak için aşağıdaki python kodunu çalıştırın. **Endpoint**, **MasterKey**, **Database**ve **Collection** 'ı uygun şekilde değiştirin.

    ```python
    connectionConfig = {
      "Endpoint" : "https://<your Cosmos DB account name.documents.azure.com:443/",
      "Masterkey" : "<your Cosmos DB primary key>",
      "Database" : "<your database name>",
      "preferredRegions" : "West US 2",
      "Collection": "<your collection name>",
      "schema_samplesize" : "1000",
      "query_pagesize" : "200000",
      "query_custom" : "SELECT * FROM c"
    }
    ```

3. Verileri yüklemek ve geçici bir görünüm oluşturmak için aşağıdaki python kodunu kullanın.

    ```python
    users = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**connectionConfig).load()
    users.createOrReplaceTempView("storm")
    ```

4. Veri döndüren bir SQL ifadesini yürütmek için aşağıdaki Magic komutunu kullanın.

    ```python
    %sql
    select * from storm
    ```

    VNet 'e eklenmiş Databricks çalışma alanınızı bir hizmet uç noktası etkinleştirilmiş Cosmos DB kaynağına başarıyla bağladınız. Cosmos DB bağlanma hakkında daha fazla bilgi edinmek için [Apache Spark için Azure Cosmos DB Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark)' na bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, Azure Databricks çalışma alanını ve tüm ilgili kaynakları silin. İşin silinmesi gereksiz faturalandırmayı önler. Azure Databricks çalışma alanını gelecekte kullanmayı planlıyorsanız kümeyi durdurup daha sonra yeniden başlatabilirsiniz. Bu Azure Databricks çalışma alanını kullanmaya devam edemeyecekecekseniz, aşağıdaki adımları kullanarak bu öğreticide oluşturduğunuz tüm kaynakları silin:

1. Azure portal sol taraftaki menüden **kaynak grupları** ' na ve ardından oluşturduğunuz kaynak grubunun adına tıklayın.

2. Kaynak grubu sayfanızda **Sil**' i seçin, metin kutusuna silinecek kaynağın adını yazın ve ardından yeniden **Sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağa Azure Databricks çalışma alanı dağıttık ve Databricks 'ten Cosmos DB verileri sorgulamak için Cosmos DB Spark bağlayıcısını kullandınız. Bir sanal ağda Azure Databricks ile çalışma hakkında daha fazla bilgi edinmek için Azure Databricks ile SQL Server kullanmaya yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: bir Azure Databricks not defteriyle sanal ağda SQL Server Linux Docker kapsayıcısını sorgulama](vnet-injection-sql-server.md)
