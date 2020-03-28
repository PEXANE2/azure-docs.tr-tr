---
title: Öğretici - Cosmos DB bitiş noktasıyla Azure Veri Tuğlaları uygulayın
description: Bu öğretici, Cosmos DB için etkinleştirilmiş bir Hizmet Bitiş Noktası ile sanal ağda Azure Veri Tuğlalarının nasıl uygulanacağını açıklar.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: 4ac8c01e986cf1f3158c615a0791ba476e5bf1bb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74706157"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Öğretici: Cosmos DB bitiş noktasıyla Azure Veri Tuğlaları uygulayın

Bu öğretici, Cosmos DB için etkin leştirilmiş bir Hizmet Bitiş Noktası ile VNet enjekte edilen Databricks ortamının nasıl uygulanacağını açıklar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal ağda Azure Databricks çalışma alanı oluşturma
> * Cosmos DB hizmet bitiş noktası oluşturma
> * Cosmos DB hesabı oluşturma ve veri alma
> * Azure Veri Tuğlaları kümesi oluşturma
> * Azure Databricks dizüstü bilgisayarından Cosmos DB sorgula

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri yapın:

* Sanal [ağda Azure Databricks çalışma alanı](quickstart-create-databricks-workspace-vnet-injection.md)oluşturun.

* Kıvılcım [konektörünü](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar)indirin.

* [NOAA Ulusal Çevre Bilgi Merkezleri'nden](https://www.ncdc.noaa.gov/stormevents/)örnek verileri indirin. Bir durum veya alan seçin ve **Ara'yı**seçin. Bir sonraki sayfada varsayılanları kabul edin ve **Ara'yı**seçin. Ardından sonuçları indirmek için sayfanın sol tarafında **KiC İndir'i** seçin.

* Azure Cosmos DB Veri Geçiş Aracı'nın [önceden derlenmiş ikilisini](https://aka.ms/csdmtool) indirin.

## <a name="create-a-cosmos-db-service-endpoint"></a>Cosmos DB hizmet bitiş noktası oluşturma

1. Bir Azure Databricks çalışma alanını sanal ağa dağıttıktan sonra, Azure [portalındaki](https://portal.azure.com)sanal ağa gidin. Databricks dağıtımı aracılığıyla oluşturulan genel ve özel alt ağlara dikkat edin.

   ![Sanal ağ alt ağları](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. Ortak *alt ağı* seçin ve bir Cosmos DB hizmet bitiş noktası oluşturun. Sonra **Kaydet**.
   
   ![Cosmos DB hizmet bitiş noktası ekleme](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma

1. Azure portalı açın. Ekranın sol üst tarafında, Azure **Cosmos DB'> Veritabanı > kaynak oluştur'u**seçin.

2. **Temel Bilgiler** sekmesindeki **Örnek Ayrıntılarını** aşağıdaki ayarlarla doldurun:

   |Ayar|Değer|
   |-------|-----|
   |Abonelik|*aboneliğiniz*|
   |Kaynak Grubu|*kaynak grubunuz*|
   |Hesap Adı|db-vnet-hizmet-bitiş noktası|
   |API|Çekirdek (SQL)|
   |Konum|Batı ABD|
   |Coğrafi Yedeklilik|Devre Dışı Bırak|
   |Birden Çok Bölgeli Yazmalar|Etkinleştirme|

   ![Cosmos DB hizmet bitiş noktası ekleme](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. **Ağ** sekmesini seçin ve sanal ağınızı yapılandırın. 

   a. Ön koşul olarak oluşturduğunuz sanal ağı seçin ve ardından *genel alt ağı*seçin. Özel *alt ağda* *'Microsoft AzureCosmosDB' bitiş noktası eksik'* notuna sahip olduğuna dikkat edin. Bunun nedeni, *yalnızca ortak alt ağdaki*Cosmos DB hizmet bitiş noktasını etkinleştirmenizdir.

   b. **Azure portalından Erişime İzin Verme** etkin olduğundan emin olun. Bu ayar, Azure portalından Cosmos DB hesabınıza erişmenizi sağlar. Bu seçenek **Reddet**olarak ayarlanmışsa, hesabınıza erişmeye çalışırken hatalar alırsınız. 

   > [!NOTE]
   > Bu öğretici için gerekli değildir, ancak yerel makinenizden Cosmos DB hesabınıza erişebilmenizi istiyorsanız *IP'me erişime izin* ver'i de etkinleştirebilirsiniz. Örneğin, Cosmos DB SDK'yı kullanarak hesabınıza bağlanıyorsanız, bu ayarı etkinleştirmeniz gerekir. Devre dışı bırakılırsa, "Erişim Reddedildi" hataları alırsınız.

   ![Cosmos DB Hesap ağ ayarları](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. **Gözden Geçir + Oluştur'u**seçin ve ardından sanal ağ içinde Cosmos DB hesabınızı oluşturmak için **oluşturun.**

5. Cosmos DB hesabınız oluşturulduktan sonra **Ayarlar**altında **Keys'e** gidin. Birincil bağlantı dizesini kopyalayın ve daha sonra kullanmak üzere bir metin düzenleyicisinde kaydedin.

    ![Cosmos DB hesap anahtarları sayfası](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Cosmos DB hesabınıza yeni bir veritabanı ve koleksiyon eklemek için **Veri Gezgini** ve **Yeni Koleksiyon'u** seçin.

    ![Cosmos DB yeni koleksiyonu](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Cosmos DB'ye veri yükleme

1. [Cosmos DB](https://aka.ms/csdmtool), **Dtui.exe**için veri geçiş aracının grafik arabirim sürümünü açın.

    ![Cosmos DB Veri Geçişi Aracı](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. Kaynak **Bilgiler** sekmesinde, açılan **dosyadan Alma'da** **CSV File(lar)'ı** seçin. Ardından **Dosya Ekle'yi** seçin ve ön koşul olarak indirdiğiniz fırtına verileri CSV'yi ekleyin.

    ![Cosmos DB Veri Geçiş Aracı kaynak bilgileri](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. Hedef **Bilgiler** sekmesinde bağlantı dizenizi giriş kurun. Bağlantı dize `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>`biçimi. AccountEndpoint ve AccountKey, önceki bölümde kaydettiğiniz birincil bağlantı dizesinde yer almaktadır. Bağlantı `Database=<your database name>` dizesinin sonuna ekleyip **Doğrula'yı**seçin. Ardından, Koleksiyon adını ve bölüm anahtarını ekleyin.

    ![Cosmos DB Veri Geçiş Aracı hedef bilgileri](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Özet sayfasına gelene kadar **İleri'yi** seçin. Ardından, **İçe Aktar'ı**seçin.

## <a name="create-a-cluster-and-add-library"></a>Küme oluşturma ve kitaplık ekleme

1. [Azure portalındaki](https://portal.azure.com) Azure Databricks hizmetinize gidin ve **Çalışma Alanını Başlat'ı**seçin.

   ![Databricks çalışma alanını başlat](./media/service-endpoint-cosmosdb/launch-workspace.png)

2. Yeni bir küme oluşturun. Bir Küme Adı seçin ve kalan varsayılan ayarları kabul edin.

   ![Yeni küme ayarları](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Kümeniz oluşturulduktan sonra küme sayfasına gidin ve **Kitaplıklar** sekmesini seçin. **Yeni Yükle'yi** seçin ve kitaplığı yüklemek için Kıvılcım bağlayıcı sıcağı dosyasını yükleyin.

    ![Kıvılcım bağlayıcı kitaplığını yükleme](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    Kitaplığın **Kitaplıklar** sekmesinde yüklü olduğunu doğrulayabilirsiniz.

    ![Databricks küme Kitaplıkları sekmesi](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Databricks not defterinden Cosmos DB sorgula

1. Azure Databricks çalışma alanınıza gidin ve yeni bir python dizüstü bilgisayar oluşturun.

    ![Yeni Databricks dizüstü bilgisayar oluşturma](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Cosmos DB bağlantı yapılandırmasını ayarlamak için aşağıdaki python kodunu çalıştırın. Bitiş **Noktası,** **Masterkey,** **Veritabanı**ve **Koleksiyon'u** buna göre değiştirin.

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

4. Verileri döndüren bir SQL deyimini yürütmek için aşağıdaki sihirli komutu kullanın.

    ```python
    %sql
    select * from storm
    ```

    VNet enjekte edilen Databricks çalışma alanınızı hizmet bitiş noktası etkin cosmos DB kaynağına başarıyla bağladınız. Cosmos DB'ye nasıl bağlanacağınız hakkında daha fazla bilgi için [Apache Spark için Azure Cosmos DB Bağlayıcısı'na](https://github.com/Azure/azure-cosmosdb-spark)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde kaynak grubunu, Azure Veri Tuğlaları çalışma alanını ve ilgili tüm kaynakları silin. İşi silerken gereksiz faturalandırmaönler. Gelecekte Azure Veri Tuğlaları çalışma alanını kullanmayı planlıyorsanız, kümeyi durdurabilir ve daha sonra yeniden başlatabilirsiniz. Bu Azure Veri Tuğlaları çalışma alanını kullanmaya devam etmeyecekseniz, aşağıdaki adımları kullanarak bu eğitimde oluşturduğunuz tüm kaynakları silin:

1. Azure portalındaki sol menüden Kaynak **gruplarını** tıklatın ve ardından oluşturduğunuz kaynak grubunun adını tıklatın.

2. Kaynak grubu sayfanızda **Sil,** metin kutusuna silmek için kaynağın adını yazın ve ardından yeniden **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, sanal bir ağa bir Azure Databricks çalışma alanı dağıttınız ve Cosmos DB Spark bağlayıcısını Databricks'ten Cosmos DB verilerini sorgulamak için kullandınız. Sanal ağda Azure Databricks ile çalışma hakkında daha fazla bilgi edinmek için, Azure Databricks ile SQL Server'ı kullanma eğitimine devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Databricks dizüstü bilgisayarından sanal ağda sql server Linux Docker kapsayıcısı sorgulayın](vnet-injection-sql-server.md)
