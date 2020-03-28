---
title: Öğretici - Azure Databricks'i kullanarak ETL işlemlerini gerçekleştirin
description: Bu eğitimde, Veri Gölü Depolama Gen2'den Azure Veri Tuğlaları'na nasıl veri ayıklanın, verileri dönüştürecek ve verileri Azure SQL Veri Ambarı'na yükleyin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.date: 01/29/2020
ms.openlocfilehash: 8819b79a105b7a654a34e47c5ba9b3d351a1d926
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239407"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Öğretici: Azure Databricks'i kullanarak verileri ayıklayın, dönüştürün ve yükleyin

Bu eğitimde, Azure Databricks'i kullanarak bir ETL (verileri ayıklama, dönüştürme ve yükleme) işlemi gerçekleştirebilirsiniz. Azure Veri Gölü Depolama Gen2'den Azure Veri Tuğlaları'na veri ayıklar, Azure Veri Tuğlaları'ndaki verilerde dönüşümler çalıştırır ve dönüştürülmüş verileri Azure SQL Veri Ambarı'na yüklersiniz.

Bu öğreticideki adımlarda, verileri Azure Databricks'e aktarmak üzere Azure Databricks için SQL Veri Ambarı bağlayıcısı kullanılır. Bu bağlayıcı da, Azure Databricks kümesiyle Azure SQL Veri Ambarı arasında aktarılan veriler için geçici depolama alanı olarak Azure Blob Depolama'yı kullanır.

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Veri Gölü Deposu ve SQL Veri Ambarı ile Azure Veri Tuğlaları](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Veri Gölü Deposu ve SQL Veri Ambarı ile Azure Veri Tuğlaları")

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir Azure Databricks hizmeti oluşturun.
> * Azure Databricks'te bir Kıvılcım kümesi oluşturun.
> * Veri Gölü Depolama Gen2 hesabında bir dosya sistemi oluşturun.
> * Azure Veri Gölü Depolama Gen2 hesabına örnek verileri yükleyin.
> * Bir hizmet ilkesi oluşturun.
> * Azure Veri Gölü Depolama Gen2 hesabından veri ayıklayın.
> * Azure Databricks'te verileri dönüştürün.
> * Verileri Azure SQL Veri Ambarı'na yükleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!Note]
> Bu öğretici **Azure Ücretsiz Deneme Aboneliği**kullanılarak gerçekleştirilemez.
> Ücretsiz bir hesabınız varsa, profilinize gidin ve aboneliğinizi istediğiniz **kadar öde**olarak değiştirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Ardından, [harcama sınırını kaldırın](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)ve bölgenizdeki VCPU'lar için kota artışı [isteyin.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) Azure Databricks çalışma alanınızı oluşturduğunuzda, çalışma alanına ücretsiz Premium Azure Databricks DBUs'a 14 gün süreyle erişim sağlamak için **Deneme (Premium - 14 Gün Ücretsiz DBUs)** fiyatlandırma katmanını seçebilirsiniz.
     
## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce bu görevleri tamamlayın:

* Bir Azure SQL veri ambarı oluşturun, sunucu düzeyinde bir güvenlik duvarı kuralı oluşturun ve sunucu yöneticisi olarak sunucuya bağlanın. Bkz. [Hızlı Başlangıç: Azure portalında bir Azure SQL veri ambarı oluşturun ve sorgula.](../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md)

* Azure SQL veri ambarı için bir ana anahtar oluşturun. Bkz. [Veritabanı ana anahtarı oluştur.](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key)

* Azure Blob depolama hesabı ve bu hesabın içinde bir kapsayıcı oluşturun. Ayrıca, depolama hesabına erişmek için erişim anahtarını alın. [Bkz. Hızlı Başlangıç: Azure portalı ile yükleme, indirme ve liste lekeleri.](../storage/blobs/storage-quickstart-blobs-portal.md)

* Bir Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturun. Bkz. [Hızlı Başlangıç: Bir Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturun.](../storage/blobs/data-lake-storage-quickstart-create-account.md)

* Bir hizmet ilkesi oluşturun. [Bkz. Nasıl Olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

   Bu makaledeki adımları gerçekleştirirken yapmanız gereken birkaç özel şey vardır.

   * Uygulamanın bir [rol bölümüne atamasını](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) sağlarken, Veri Gölü Depolama Gen2 hesabı kapsamında **Depolama Blob Veri Katılımcısı** rolünü hizmet ilkesine atadığından emin olun. Rolü üst kaynak grubuna veya aboneye atarsanız, bu rol atamaları depolama hesabına yayılana kadar izinlerle ilgili hatalar alırsınız.

      Hizmet ilkesini belirli bir dosya veya dizinle ilişkilendirmek için bir erişim denetim listesi (ACL) kullanmayı tercih ederseniz, [Azure Veri Gölü Depolama Gen2'de Access denetimine](../storage/blobs/data-lake-storage-access-control.md)başvurun.

   * Makalenin bölümünde [oturum açmak için Al değerleri](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) adımlarını gerçekleştirirken, kiracı kimliğini, uygulama kimliğini ve gizli değerleri bir metin dosyasına yapıştırın. Yakında bunlara ihtiyacın olacak.

* [Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="gather-the-information-that-you-need"></a>İhtiyacınız olan bilgileri toplama

Bu öğreticinin ön koşulları nın tamamladığından emin olun.

   Başlamadan önce, şu bilgi öğelerine sahip olmalısınız:

   :heavy_check_mark: Azure SQL Veri ambarınızın veritabanı adı, veritabanı sunucu adı, kullanıcı adı ve parolası.

   :heavy_check_mark: Blob depolama hesabınızın erişim anahtarı.

   :heavy_check_mark: Data Lake Storage Gen2 depolama hesabınızın adı.

   :heavy_check_mark: Aboneliğinizin kiracı kimliği.

   :heavy_check_mark: Azure Active Directory (Azure AD) ile kaydolduğunuz uygulamanın uygulama kimliği.

   :heavy_check_mark: Azure AD'ye kaydettiğiniz uygulamanın kimlik doğrulama anahtarı.

## <a name="create-an-azure-databricks-service"></a>Azure Databricks hizmeti oluşturma

Bu bölümde, Azure portalını kullanarak bir Azure Databricks hizmeti oluşturursunuz.

1. Azure portalı menüsünden **kaynak oluştur'u**seçin.

    ![Azure portalında kaynak oluşturma](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png)

    Ardından, **Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında Azure Veri Tuğlaları oluşturma](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-resource-create.png)



2. **Azure Databricks Hizmeti**altında, bir Databricks hizmeti oluşturmak için aşağıdaki değerleri sağlar:

    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın.        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../azure-resource-manager/management/overview.md). |
    |**Konum**     | **Batı ABD 2**'yi seçin.  Kullanılabilir diğer bölgeler için bkz. [Bölgeye göre kullanılabilir Azure hizmetleri](https://azure.microsoft.com/regions/services/).      |
    |**Fiyatlandırma Katmanı**     |  **Standart'ı**seçin.     |

3. Hesabın oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için en üstteki ilerleme çubuğunu görüntüleyin.

4. **Panoya sabitle**’yi ve sonra **Oluştur**’u seçin.

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Azure Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks hizmetine gidin ve **Çalışma Alanını Başlat'ı**seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Küme**’yi seçin.

    ![Azure'da Veri Tuğlaları](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

4. Aşağıdaki alanlara değerleri girin ve diğer alanlar için varsayılan değerleri kabul edin:

    * Küme için bir ad girin.

    * Etkinlik dakikaları onay kutusunu **sonlandır'dan sonra \_ \_ Sonlandırma'yı seçtiğinizden** emin olun. Küme kullanılmıyorsa, kümeyi sonlandırmak için bir süre (dakika olarak) sağlayın.

    * **Küme oluştur**’u seçin. Küme çalıştıktan sonra, kümeye not defterleri ekleyebilir ve Spark işlerini çalıştırabilirsiniz.

## <a name="create-a-file-system-in-the-azure-data-lake-storage-gen2-account"></a>Azure Veri Gölü Depolama Gen2 hesabında bir dosya sistemi oluşturma

Bu bölümde, Azure Databricks çalışma alanında bir not defteri oluşturur sunuz ve ardından depolama hesabını yapılandırmak için kod parçacıkları çalıştırırsınız

1. Azure [portalında,](https://portal.azure.com)oluşturduğunuz Azure Databricks hizmetine gidin ve **Çalışma Alanını Başlat'ı**seçin.

2. Solda, Çalışma **Alanı'nı**seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

3. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Scala**’yı seçin ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks'te bir not defteri için ayrıntılar sağlama](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Databricks'te bir not defteri için ayrıntılar sağlama")

4. **Oluştur'u**seçin.

5. Aşağıdaki kod bloğu, Kıvılcım oturumunda erişilen herhangi bir ADLS Gen 2 hesabı için varsayılan hizmet temel kimlik bilgilerini ayarlar. İkinci kod bloğu, belirli bir ADLS Gen 2 hesabının kimlik bilgilerini belirtmek için hesap adını ayarına ekler.  Kod bloğundan birini Azure Databricks not defterinizin ilk hücresine kopyalayıp yapıştırın.

   **Oturum yapılandırması**

   ```scala
   val appID = "<appID>"
   val secret = "<secret>"
   val tenantID = "<tenant-id>"

   spark.conf.set("fs.azure.account.auth.type", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id", "<appID>")
   spark.conf.set("fs.azure.account.oauth2.client.secret", "<secret>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   ```

   **Hesap yapılandırması**

   ```scala
   val storageAccountName = "<storage-account-name>"
   val appID = "<app-id>"
   val secret = "<secret>"
   val fileSystemName = "<file-system-name>"
   val tenantID = "<tenant-id>"

   spark.conf.set("fs.azure.account.auth.type." + storageAccountName + ".dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type." + storageAccountName + ".dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id." + storageAccountName + ".dfs.core.windows.net", "" + appID + "")
   spark.conf.set("fs.azure.account.oauth2.client.secret." + storageAccountName + ".dfs.core.windows.net", "" + secret + "")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint." + storageAccountName + ".dfs.core.windows.net", "https://login.microsoftonline.com/" + tenantID + "/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://" + fileSystemName  + "@" + storageAccountName + ".dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
   ```

6. Bu kod bloğunda, `<app-id>` `<secret>`bu `<tenant-id>`kod `<storage-account-name>` bloğundaki , , ve yer tutucu değerlerini, bu öğreticinin ön koşullarını tamamlarken topladığınız değerlerle değiştirin. `<file-system-name>` Yer tutucu değerini dosya sistemine vermek istediğiniz adla değiştirin.

   * Bir `<app-id>` `<secret>` hizmet ilkesi oluşturmanın bir parçası olarak etkin dizine kaydettiğiniz uygulamadan dır.

   * Abonelikten. `<tenant-id>`

   * Azure `<storage-account-name>` Veri Gölü Depolama Gen2 depolama hesabınızın adıdır.

7. Bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

## <a name="ingest-sample-data-into-the-azure-data-lake-storage-gen2-account"></a>Azure Veri Gölü Depolama Gen2 hesabına örnek verileri alma

Bu bölüme başlamadan önce aşağıdaki önkoşulları tamamlamanız gerekir:

Aşağıdaki kodu bir not defteri hücresine girin:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

Hücrede, kodu çalıştırmak için **SHIFT + ENTER** tuşuna basın.

Şimdi bunun altındaki yeni bir hücrede, aşağıdaki kodu girin ve parantez içinde görünen değerleri daha önce kullandığınız değerlerle değiştirin:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://" + fileSystemName + "@" + storageAccountName + ".dfs.core.windows.net/")

Hücrede, kodu çalıştırmak için **SHIFT + ENTER** tuşuna basın.

## <a name="extract-data-from-the-azure-data-lake-storage-gen2-account"></a>Azure Veri Gölü Depolama Gen2 hesabından veri ayıklama

1. Artık örnek json dosyasını Azure Databricks'te veri çerçevesi olarak yükleyebilirsiniz. Aşağıdaki kodu yeni bir hücreye yapıştırın. Parantez içinde gösterilen yer tutucuları değerlerinizle değiştirin.

   ```scala
   val df = spark.read.json("abfss://" + fileSystemName + "@" + storageAccountName + ".dfs.core.windows.net/small_radio_json.json")
   ```
2. Bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

3. Veri çerçevesinin içeriğini görmek için aşağıdaki kodu çalıştırın:

    ```scala
    df.show()
    ```
   Aşağıdaki kod parçacığına benzer bir çıkış görürsünüz:

   ```output
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
   | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
   | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
   ...
   ...
   ```

   Artık verileri Azure Data Lake Storage Gen2'den Azure Databricks'e ayıkladınız.

## <a name="transform-data-in-azure-databricks"></a>Azure Databricks'te verileri dönüştürme

Ham örnek veri **small_radio_json.json** dosyası bir radyo istasyonu için dinleyici yakalar ve sütunlar çeşitli vardır. Bu bölümde, verileri yalnızca veri kümesinden belirli sütunları almak için dönüştürürsunuz.

1. İlk olarak, oluşturduğunuz veri çerçevesinden **yalnızca ilk Ad,** **soyad,** **cinsiyet,** **konum**ve **düzey** sütunlarını alın.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
   ```

   Aşağıdaki parçacıkta gösterildiği gibi çıktı alırsınız:

   ```output
   +---------+----------+------+--------------------+-----+
   |firstname|  lastname|gender|            location|level|
   +---------+----------+------+--------------------+-----+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   +---------+----------+------+--------------------+-----+
   ```

2. Bu verilerde başka dönüştürmeler de yaparak **level** sütununun adını **subscription_type** olarak değiştirebilirsiniz.

   ```scala
   val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
   renamedColumnsDF.show()
   ```

   Aşağıdaki snippet'te gösterildiği gibi çıktı alırsınız.

   ```output
   +---------+----------+------+--------------------+-----------------+
   |firstname|  lastname|gender|            location|subscription_type|
   +---------+----------+------+--------------------+-----------------+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   +---------+----------+------+--------------------+-----------------+
   ```

## <a name="load-data-into-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı’na veri yükleme

Bu bölümde, dönüştürülen verileri Azure SQL Veri Ambarı'na yüklersiniz. Bir veri çerçevesini doğrudan SQL veri ambarında tablo olarak yüklemek için Azure Veri Ambarı bağlayıcısını kullanırsınız.

Daha önce de belirtildiği gibi, SQL Veri Ambarı bağlayıcısı, Azure Veri Tuğlaları ve Azure SQL Veri Ambarı arasında veri yüklemek için geçici depolama alanı olarak Azure Blob depolamasını kullanır. Bu nedenle, depolama hesabına bağlanmak için kullanılacak yapılandırmayı sağlayarak başlarsınız. Bu makalenin ön koşullarının bir parçası olarak hesabı zaten oluşturmuş olmalısınız.

1. Azure Databricks'ten Azure Depolama hesabına erişmek için yapılandırmayı sağlayın.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Azure Veri Tuğlaları ve Azure SQL Veri Ambarı arasında veri taşınırken kullanılacak geçici bir klasör belirtin.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Aşağıdaki kod parçacığını çalıştırarak Azure Blob depolama erişim anahtarlarını yapılandırmada depolayın. Bu eylem, erişim anahtarını not defterinde düz metinolarak tutmamanızı sağlar.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Azure SQL Veri Ambarı örneğine bağlanmak için değerleri sağlayın. Ön koşul olarak bir SQL veri ambarı oluşturmuş olmalısınız. **dwServer**için tam nitelikli sunucu adını kullanın. Örneğin, `<servername>.database.windows.net`.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Dönüştürülmüş veri çerçevesini yüklemek için aşağıdaki snippet'i çalıştırın, **sütunlar df olarak yeniden adlandırıldı,** bir SQL veri ambarında tablo olarak. Bu kod parçacığı SQL veritabanında **SampleTable** adlı bir tablo oluşturur.

   ```scala
   spark.conf.set(
       "spark.sql.parquet.writeLegacyFormat",
       "true")

   renamedColumnsDF.write.format("com.databricks.spark.sqldw").option("url", sqlDwUrlSmall).option("dbtable", "SampleTable")       .option( "forward_spark_azure_storage_credentials","True").option("tempdir", tempDir).mode("overwrite").save()
   ```

   > [!NOTE]
   > Bu örnek, `forward_spark_azure_storage_credentials` SQL Veri Ambarı'nın bir Erişim Anahtarı kullanarak blob depolamadan verilere erişmesine neden olan bayrağı kullanır. Bu, desteklenen tek kimlik doğrulama yöntemidir.
   >
   > Azure Blob Depolama'nız belirli sanal ağlarla sınırlıysa, SQL Veri Ambarı [Erişim Anahtarları yerine Yönetilen Hizmet Kimliği](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)gerektirir. Bu hata "Bu istek bu işlemi gerçekleştirmek için yetkili değildir neden olur."

6. SQL veritabanına bağlanın ve **SampleTable**adında bir veritabanı gördüğünüzden doğrulayın.

   ![Örnek tabloyu doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Örnek tabloyu doğrulama")

7. Tablonun içindekileri doğrulamak için bir seçme sorgusu çalıştırın. Tablo, **yeniden adlandırılanColumnsDF** veri çerçevesi ile aynı verilere sahip olmalıdır.

    ![Örnek tablo içeriğini doğrulama](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Örnek tablo içeriğini doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi bitirdikten sonra kümeyi sonlandırabilirsiniz. Azure Databricks çalışma alanından soldaki **Kümeler'i** seçin. Kümenin sonlandırması için **Eylemler**altında elipsleri (...) işaret edin ve **Sonlandırma** simgesini seçin.

![Databricks kümesini durdurma](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Databricks kümesini durdurma")

Kümeyi el ile sonlandırmazsanız, kümeyi oluşturduğunuzda **etkinlik \_ \_ dakikaları** onay kutusunu seçtikten sonra Sonlandırma kutusunu seçmeniz koşuluyla otomatik olarak durur. Böyle bir durumda, küme belirtilen süre boyunca etkin değilse otomatik olarak durur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Databricks hizmeti oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Azure Databricks’te not defteri oluşturma
> * Veri Gölü Depolama Gen2 hesabından veri ayıklama
> * Azure Databricks'te verileri dönüştürme
> * Azure SQL Veri Ambarı’na veri yükleme

Azure Event Hubs kullanarak Azure Databricks'e gerçek zamanlı veri akışı yapmayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Event Hubs kullanarak Azure Databricks’e veri akışı sağlama](databricks-stream-from-eventhubs.md)
