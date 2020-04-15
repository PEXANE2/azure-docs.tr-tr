---
title: 'Öğretici: Azure Veri Gölü Depolama Gen2, Azure Databricks & Kıvılcım | Microsoft Dokümanlar'
description: Bu öğretici, Bir Azure Veri Gölü Depolama Gen2 depolama hesabındaki verilere erişmek için Bir Azure Veri Tuğlaları kümesinde Spark sorgularının nasıl çalıştırılabildiğini gösterir.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 5889afa033b30606f8981ddb826aa192f24efa10
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312906"
---
# <a name="tutorial-azure-data-lake-storage-gen2-azure-databricks--spark"></a>Öğretici: Azure Veri Gölü Depolama Gen2, Azure Databricks & Kıvılcım

Bu öğretici, Azure Veri Tuğlaları kümenizi Azure Veri Gölü Depolama Gen2 etkinleştirilmiş bir Azure depolama hesabında depolanan verilere nasıl bağlayabileceğinizi gösterir. Bu bağlantı, verilerinizde kümenizdeki sorguları ve analizleri yerel olarak çalıştırmanızı sağlar.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Databricks kümesi oluşturma
> * Yapılandırılmamış verileri bir depolama hesabına alma
> * Blob depolama alanında verileriniz üzerinde analiz çalıştırın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure Veri Gölü Depolama Gen2 hesabı oluşturun.

  Bkz. [Bir Azure Veri Gölü Depolama Gen2 hesabı oluşturun.](data-lake-storage-quickstart-create-account.md)

* Kullanıcı hesabınızın kendisine atanan [Depolama Blob Veri Katılımcısı rolüne](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) sahip olduğundan emin olun.

* AzCopy v10'u yükleyin. [AzCopy v10 ile veri aktarımı](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

* Bir hizmet ilkesi oluşturun. [Bkz. Nasıl Olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

  Bu makaledeki adımları gerçekleştirirken yapmanız gereken birkaç özel şey vardır.

  :heavy_check_mark: [Uygulamayı makalenin bir rol bölümüne atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) adımları gerçekleştirirken, Depolama **Blob Veri Katılımcısı** rolünü hizmet ilkesine atadiğinizden emin olun.

  > [!IMPORTANT]
  > Rolü Veri Gölü Depolama Gen2 depolama hesabı kapsamındaki olduğunuzdan emin olun. Bir rolü üst kaynak grubuna veya aboneye atayabilirsiniz, ancak bu rol atamaları depolama hesabına yayılana kadar izinlerle ilgili hatalar alırsınız.

  :heavy_check_mark: Makalenin oturum [açma değerleri için Al adımları](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) gerçekleştirirken, kiracı kimliğini, uygulama kimliğini ve istemci gizli değerlerini bir metin dosyasına yapıştırın. Yakında bunlara ihtiyacın olacak.

### <a name="download-the-flight-data"></a>Uçuş verilerini indirme

Bu öğretici, Bir ETL işleminin nasıl yapılacağını göstermek için Ulaştırma İstatistikleri Bürosu'ndan gelen uçuş verilerini kullanır. Öğreticiyi tamamlamak için bu verileri indirmeniz gerekir.

1. Araştırma [ve Yenilikçi Teknoloji İdaresi, Ulaşım İstatistikleri Bürosu](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time)gidin.

2. Tüm veri alanlarını seçmek için **Hazır Dosya** onay kutusunu seçin.

3. **İndir** düğmesini seçin ve sonuçları bilgisayarınıza kaydedin. 

4. Sıkıştırılanan dosyanın içeriğini açın ve dosya adını ve dosyanın yolunu not edin. Bu bilgilere daha sonraki bir adımda ihtiyacınız var.

## <a name="create-an-azure-databricks-service"></a>Azure Databricks hizmeti oluşturma

Bu bölümde, Azure portalını kullanarak bir Azure Databricks hizmeti oluşturursunuz.

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](./media/data-lake-storage-use-databricks-spark/azure-databricks-on-portal.png "Azure portalında veri tuğlaları")

2. **Azure Databricks Hizmeti**altında, bir Databricks hizmeti oluşturmak için aşağıdaki değerleri sağlar:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.  |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin. Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).       |
    |**Fiyatlandırma Katmanı**     |  **Standart'ı**seçin.     |

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-use-databricks-spark/create-databricks-workspace.png "Azure Databricks hizmeti oluşturma")

3. Hesabın oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için en üstteki ilerleme çubuğunu görüntüleyin.

4. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Azure Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks hizmetine gidin ve **Çalışma Alanını Başlat'ı**seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure'da Veri Tuğlaları](./media/data-lake-storage-use-databricks-spark/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](./media/data-lake-storage-use-databricks-spark/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

    Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    - Küme için bir ad girin.
     
    - **120 dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

4. **Küme oluştur**’u seçin. Küme çalıştıktan sonra, kümeye not defterleri ekleyebilir ve Spark işlerini çalıştırabilirsiniz.

## <a name="ingest-data"></a>Veriyi çekme

### <a name="copy-source-data-into-the-storage-account"></a>Kaynak verilerini depolama hesabına kopyalama

*.csv* dosyanızdaki verileri Veri Gölü Depolama Gen2 hesabınıza kopyalamak için AzCopy'yi kullanın.

1. Bir komut istemi penceresi açın ve depolama hesabınıza giriş yapmak için aşağıdaki komutu girin.

   ```bash
   azcopy login
   ```

   Kullanıcı hesabınızın kimliğini doğrulamak için komut istemi penceresinde görünen yönergeleri izleyin.

2. *.csv* hesabındaki verileri kopyalamak için aşağıdaki komutu girin.

   ```bash
   azcopy cp "<csv-folder-path>" https://<storage-account-name>.dfs.core.windows.net/<container-name>/folder1/On_Time.csv
   ```

   * `<csv-folder-path>` Yer tutucu değerini *.csv* dosyasına giden yol ile değiştirin.

   * `<storage-account-name>` Yer tutucu değerini depolama hesabınızın adı ile değiştirin.

   * Yer `<container-name>` tutucuyu depolama hesabınızdaki bir kapsayıcının adı ile değiştirin.

## <a name="create-a-container-and-mount-it"></a>Bir kapsayıcı oluşturun ve monte edin

Bu bölümde, depolama hesabınızda bir kapsayıcı ve klasör oluşturursunuz.

1. Azure [portalında,](https://portal.azure.com)oluşturduğunuz Azure Databricks hizmetine gidin ve **Çalışma Alanını Başlat'ı**seçin.

2. Solda, Çalışma **Alanı'nı**seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-use-databricks-spark/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Python'u** seçin ve ardından daha önce oluşturduğunuz Kıvılcım kümesini seçin.

4. **Oluştur'u**seçin.

5. Aşağıdaki kod bloğunu ilk hücreye kopyalayıp yapıştırın, ancak bu kodu henüz çalıştırmayın.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<appId>",
           "fs.azure.account.oauth2.client.secret": "<clientSecret>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```

18. Bu kod bloğunda, `appId` `clientSecret`bu `tenant`kod `storage-account-name` bloğundaki , , ve yer tutucu değerlerini, bu öğreticinin ön koşullarını tamamlarken topladığınız değerlerle değiştirin. `container-name` Yer tutucu değerini kapsayıcının adı ile değiştirin.

19. Bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

   Daha sonra komutlar ekleyeceğiniz için bu not defterini açık tutun.

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Databricks Not Defteri'ni kullanarak CSV'yi Parquet biçimine dönüştürme

Daha önce oluşturduğunuz not defterine yeni bir hücre ekleyin ve bu hücreye aşağıdaki kodu yapıştırın. 

```python
# Use the previously established DBFS mount point to read the data.
# create a data frame to read data.

flightDF = spark.read.format('csv').options(
    header='true', inferschema='true').load("/mnt/flightdata/*.csv")

# read the airline csv file and write the output to parquet format for easy query.
flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
print("Done")
```

## <a name="explore-data"></a>Verileri inceleme

Yeni bir hücrede, AzCopy üzerinden yüklenen CSV dosyalarının listesini almak için aşağıdaki kodu yapıştırın.

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata"))
```

Yeni bir dosya oluşturmak ve *parquet/flights* klasöründeki dosyaları listelemek için şu betiği çalıştırın:

```python
dbutils.fs.put("/mnt/flightdata/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/parquet/flights")
```

Bu kod örnekleriyle Data Lake Storage 2. Nesil etkin bir depolama hesabında depolanan verileri kullanarak HDFS’nin hiyerarşik özelliklerini keşfettiniz.

## <a name="query-the-data"></a>Verileri sorgulama

Bir sonraki adımda depolama hesabınıza yüklediğiniz verileri sorgulamaya başlayabilirsiniz. Aşağıdaki kod bloklarını **Cmd 1** bölümüne girin ve **Cmd + Enter** tuşlarına basarak Python betiğini çalıştırın.

Veri kaynaklarınız için veri çerçeveleri oluşturmak için aşağıdaki komut dosyasını çalıştırın:

* `<csv-folder-path>` Yer tutucu değerini *.csv* dosyasına giden yol ile değiştirin.

```python
# Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(
    header='true', inferschema='true').load("/mnt/flightdata/On_Time.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

# read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(
    header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

# print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

# print the flight database size
print("Number of flights in the database: ", flightDF.count())

# show the first 20 rows (20 is the default)
# to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

# Display to run visualizations
# preferably run this in a separate cmd cell
display(flightDF)
```

Verilere karşı bazı temel çözümleme sorguları çalıştırmak için bu komut dosyasını girin.

```python
# Run each of these queries, preferably in a separate cmd cell for separate analysis
# create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

# create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

# using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights = out2.count()
print("Jan 2016: ", NumJan2016Flights, " Feb 2016: ", NumFeb2016Flights)
Total = NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql(
    "SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'")
print('Airports in Texas: ', out.show(100))

# find all airlines that fly from Texas
out1 = spark.sql(
    "SELECT distinct(Reporting_Airline) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve ilgili tüm kaynakları silin. Bunu yapmak için, depolama hesabı için kaynak grubunu seçin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme](data-lake-storage-tutorial-extract-transform-load-hive.md)
