---
title: 'Öğretici: Azure Veri Tuğlaları Delta sı tablolarını güncelleştirmek için veri gölü yakalama deseni uygulayın | Microsoft Dokümanlar'
description: Bu öğretici, Azure DataLake Storage Gen2'de depolanan bir tabloya veri satırları eklemek için Olay Ağı aboneliği, Azure İşi ve Azure Databricks işini nasıl kullanacağınızı gösterir.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 08/20/2019
ms.author: normesta
ms.reviewer: sumameh
ms.openlocfilehash: 85fad873b6c176d2278ea48709d2892ab515a025
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78303316"
---
# <a name="tutorial-implement-the-data-lake-capture-pattern-to-update-a-databricks-delta-table"></a>Öğretici: Databricks Delta tablosunu güncelleştirmek için veri gölü yakalama deseni uygulayın

Bu öğretici, hiyerarşik ad alanına sahip bir depolama hesabındaki olayları nasıl işleyeceğinizi gösterir.

Bir satış siparişini açıklayan virgülle ayrılmış değerler (csv) dosyasını yükleyerek, kullanıcının Bir Databricks Delta tablosunu doldurmasını sağlayan küçük bir çözüm oluşturursunuz. Bu çözümü, Bir Olay Ağı aboneliği, Bir Azure İşi ve Azure Veri Tuğlaları'nda [Bir İş'i](https://docs.azuredatabricks.net/user-guide/jobs.html) birbirine bağlayarak oluşturursunuz.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure İşi çağıran bir Olay Izgara aboneliği oluşturun.
> * Bir olaydan bildirim alan ve ardından işi Azure Databricks'te çalıştıran bir Azure İşlevi oluşturun.
> * Müşteri siparişini depolama hesabında bulunan Databricks Delta tablosuna ekleyen bir Databricks işi oluşturun.

Bu çözümü Azure Databricks çalışma alanından başlayarak ters sırada oluşturacağız.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

* Hiyerarşik ad alanı olan bir depolama hesabı oluşturun (Azure Veri Gölü Depolama Gen2). Bu öğretici, adlı `contosoorders`bir depolama hesabı kullanır. Kullanıcı hesabınızın kendisine atanan [Depolama Blob Veri Katılımcısı rolüne](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) sahip olduğundan emin olun.

  Bkz. [Bir Azure Veri Gölü Depolama Gen2 hesabı oluşturun.](data-lake-storage-quickstart-create-account.md)

* Bir hizmet ilkesi oluşturun. [Bkz. Nasıl Olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

  Bu makaledeki adımları gerçekleştirirken yapmanız gereken birkaç özel şey vardır.

  :heavy_check_mark: [Uygulamayı makalenin bir rol bölümüne atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) adımları gerçekleştirirken, Depolama **Blob Veri Katılımcısı** rolünü hizmet ilkesine atadiğinizden emin olun.

  > [!IMPORTANT]
  > Rolü Veri Gölü Depolama Gen2 depolama hesabı kapsamındaki olduğunuzdan emin olun. Bir rolü üst kaynak grubuna veya aboneye atayabilirsiniz, ancak bu rol atamaları depolama hesabına yayılana kadar izinlerle ilgili hatalar alırsınız.

  :heavy_check_mark: Makalenin oturum [açma değerleri için Al](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) adımları gerçekleştirirken, kiracı kimliğini, uygulama kimliğini ve parola değerlerini bir metin dosyasına yapıştırın. Bu değerlere daha sonra ihtiyacın olacak.

## <a name="create-a-sales-order"></a>Satış siparişi oluşturma

İlk olarak, satış siparişini açıklayan bir csv dosyası oluşturun ve ardından bu dosyayı depolama hesabına yükleyin. Daha sonra, Veri Tuğlaları Deltası tablomuzdaki ilk satırı doldurmak için bu dosyadaki verileri kullanırsınız.

1. Azure Depolama Gezgini'ni açın. Ardından, depolama hesabınıza gidin ve **Blob Kapsayıcıları** bölümünde **veri**adında yeni bir kapsayıcı oluşturun.

   ![veri klasörü](./media/data-lake-storage-events/data-container.png "veri klasörü")

   Depolama Gezgini'ni nasıl kullanacağı hakkında daha fazla bilgi için, [bir Azure Veri Gölü Depolama Gen2 hesabındaki verileri yönetmek için Azure Depolama Gezgini'ni kullanın'a](data-lake-storage-explorer.md)bakın.

2. **Veri** kapsayıcısında, **giriş**adlı bir klasör oluşturun.

3. Aşağıdaki metni bir metin düzenleyicisine yapıştırın.

   ```
   InvoiceNo,StockCode,Description,Quantity,InvoiceDate,UnitPrice,CustomerID,Country
   536365,85123A,WHITE HANGING HEART T-LIGHT HOLDER,6,12/1/2010 8:26,2.55,17850,United Kingdom
   ```

4. Bu dosyayı yerel bilgisayarınıza kaydedin ve **data.csv**adını verin.

5. Depolama Gezgini'nde, bu dosyayı **giriş** klasörüne yükleyin.  

## <a name="create-a-job-in-azure-databricks"></a>Azure Databricks'te iş oluşturma

Bu bölümde, şu görevleri gerçekleştireceksiniz:

* Azure Databricks çalışma alanı oluşturun.
* Bir not defteri oluşturun.
* Bir Databricks Delta tablosu oluşturun ve doldurun.
* Databricks Delta tablosuna satır ekleyen kod ekleyin.
* Bir İş Oluşturun.

### <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde Azure portalını kullanarak bir Azure Databricks çalışma alanı oluşturursunuz.

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](./media/data-lake-storage-quickstart-create-databricks-account/azure-databricks-on-portal.png "Azure portalında veri tuğlaları")

2. **Azure Databricks Hizmeti** bölümünde, Databricks çalışma alanı oluşturmak için değerler sağlayın.

    ![Azure Databricks çalışma alanı oluşturma](./media/data-lake-storage-events/new-databricks-service.png "Azure Databricks çalışma alanı oluşturma")

    Çalışma alanının oluşturulması birkaç dakika sürer. İşlem durumunu izlemek için en üstteki ilerleme çubuğunu görüntüleyin.

### <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure [portalında,](https://portal.azure.com)oluşturduğunuz Azure Veri Tuğlaları çalışma alanına gidin ve ardından **Çalışma Alanını Başlat'ı**seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan **Yeni** > **Küme'yi**seçin.

    ![Azure'da Veri Tuğlaları](./media/data-lake-storage-events/databricks-on-azure.png "Azure'da Veri Tuğlaları")

3. **Yeni küme** sayfasında, bir küme oluşturmak için değerleri girin.

    ![Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma](./media/data-lake-storage-events/create-databricks-spark-cluster.png "Azure'da Veri Tuğlaları Kıvılcım kümesi Oluşturma")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

    * Küme için bir ad girin.
    * **120 dakika işlem yapılmadığında sonlandır** onay kutusunu seçtiğinizden emin olun. Küme kullanılmazsa kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

4. **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

Küme oluşturma hakkında daha fazla bilgi için bkz. [Azure Databricks üzerinde Spark kümesi oluşturma](https://docs.azuredatabricks.net/user-guide/clusters/create.html).

### <a name="create-a-notebook"></a>Not defteri oluşturma

1. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur** > **Not Defteri**’ni seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-quickstart-create-databricks-account/databricks-create-notebook.png "Databricks'te not defteri oluşturma")

2. **Not Defteri Oluştur** iletişim kutusunda, not defterinizin adını girin. Dil olarak **Python'u** seçin ve ardından daha önce oluşturduğunuz Kıvılcım kümesini seçin.

    ![Databricks'te not defteri oluşturma](./media/data-lake-storage-events/new-databricks-notebook.png "Databricks'te not defteri oluşturma")

    **Oluştur'u**seçin.

### <a name="create-and-populate-a-databricks-delta-table"></a>Databricks Delta tablosu oluşturma ve doldurma

1. Oluşturduğunuz not defterinde, aşağıdaki kod bloğunu ilk hücreye kopyalayın ve yapıştırın, ancak bu kodu henüz çalıştırmayın.  

   Bu `appId`kod `password` `tenant` bloğundaki yer tutucu değerlerini, bu öğreticinin ön koşullarını tamamlarken topladığınız değerlerle değiştirin.

    ```Python
    dbutils.widgets.text('source_file', "", "Source File")

    spark.conf.set("fs.azure.account.auth.type", "OAuth")
    spark.conf.set("fs.azure.account.oauth.provider.type", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
    spark.conf.set("fs.azure.account.oauth2.client.id", "<appId>")
    spark.conf.set("fs.azure.account.oauth2.client.secret", "<password>")
    spark.conf.set("fs.azure.account.oauth2.client.endpoint", "https://login.microsoftonline.com/<tenant>/oauth2/token")

    adlsPath = 'abfss://data@contosoorders.dfs.core.windows.net/'
    inputPath = adlsPath + dbutils.widgets.get('source_file')
    customerTablePath = adlsPath + 'delta-tables/customers'
    ```

    Bu kod **source_file**adlı bir widget oluşturur. Daha sonra, bu kodu çağıran ve dosya yolunu bu widget'a geçen bir Azure İşlevi oluşturursunuz.  Bu kod ayrıca depolama hesabıyla servis anaparanızı doğrular ve diğer hücrelerde kullanacağınız bazı değişkenler oluşturur.

    > [!NOTE]
    > Üretim ortamında, kimlik doğrulama anahtarınızı Azure Veri Tuğlaları'nda depolamayı düşünün. Ardından, kimlik doğrulama anahtarı yerine kod bloğunuza bir arama anahtarı ekleyin. <br><br>Örneğin, bu kod satırını kullanmak `spark.conf.set("fs.azure.account.oauth2.client.secret", "<password>")`yerine: , aşağıdaki kod `spark.conf.set("fs.azure.account.oauth2.client.secret", dbutils.secrets.get(scope = "<scope-name>", key = "<key-name-for-service-credential>"))`satırını kullanırsınız: . <br><br>Bu öğreticiyi tamamladıktan sonra, bu yaklaşımın örneklerini görmek için Azure Veri Tuğlaları Web Sitesindeki [Azure Veri Gölü Depolama Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) makalesine bakın.

2. Bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

3. Aşağıdaki kod bloğunu farklı bir hücreye kopyalayıp yapıştırın ve ardından bu bloktaki kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın.

   ```Python
   from pyspark.sql.types import StructType, StructField, DoubleType, IntegerType, StringType


   inputSchema = StructType([
   StructField("InvoiceNo", IntegerType(), True),
   StructField("StockCode", StringType(), True),
   StructField("Description", StringType(), True),
   StructField("Quantity", IntegerType(), True),
   StructField("InvoiceDate", StringType(), True),
   StructField("UnitPrice", DoubleType(), True),
   StructField("CustomerID", IntegerType(), True),
   StructField("Country", StringType(), True)
   ])

   rawDataDF = (spark.read
    .option("header", "true")
    .schema(inputSchema)
    .csv(adlsPath + 'input')
   )

   (rawDataDF.write
     .mode("overwrite")
     .format("delta")
     .saveAsTable("customer_data", path=customerTablePath))
   ```

   Bu kod, depolama hesabınızda Databricks Delta tablosunu oluşturur ve daha önce yüklediğiniz csv dosyasından bazı ilk verileri yükler.

4. Bu kod bloğu başarıyla çalıştırdıktan sonra, bu kod bloğunu not defterinizden kaldırın.

### <a name="add-code-that-inserts-rows-into-the-databricks-delta-table"></a>Databricks Delta tablosuna satır ekleyen kod ekleme

1. Aşağıdaki kod bloğunu farklı bir hücreye kopyalayıp yapıştırın, ancak bu hücreyi çalıştırmayın.

   ```Python
   upsertDataDF = (spark
     .read
     .option("header", "true")
     .csv(inputPath)
   )
   upsertDataDF.createOrReplaceTempView("customer_data_to_upsert")
   ```

   Bu kod, csv dosyasındaki verileri kullanarak verileri geçici bir tablo görünümüne ekler. Bu csv dosyasına giden yol, önceki bir adımda oluşturduğunuz giriş widget'ından gelir.

2. Geçici tablo görünümünün içeriğini Databricks Delta tablosuyla birleştirmek için aşağıdaki kodu ekleyin.

   ```
   %sql
   MERGE INTO customer_data cd
   USING customer_data_to_upsert cu
   ON cd.CustomerID = cu.CustomerID
   WHEN MATCHED THEN
     UPDATE SET
       cd.StockCode = cu.StockCode,
       cd.Description = cu.Description,
       cd.InvoiceNo = cu.InvoiceNo,
       cd.Quantity = cu.Quantity,
       cd.InvoiceDate = cu.InvoiceDate,
       cd.UnitPrice = cu.UnitPrice,
       cd.Country = cu.Country
   WHEN NOT MATCHED
     THEN INSERT (InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country)
     VALUES (
       cu.InvoiceNo,
       cu.StockCode,
       cu.Description,
       cu.Quantity,
       cu.InvoiceDate,
       cu.UnitPrice,
       cu.CustomerID,
       cu.Country)
   ```

### <a name="create-a-job"></a>İş Oluşturma

Daha önce oluşturduğunuz not defterini çalıştıran bir İş oluşturun. Daha sonra, bir olay yükseltildiğinde bu işi çalıştıran bir Azure İşi oluşturursunuz.

1. **İşler'i**tıklatın.

2. **İşler** sayfasında İş **Oluştur'u**tıklatın.

3. İşe bir ad verin ve `upsert-order-data` ardından çalışma kitabını seçin.

   ![Bir iş oluşturma](./media/data-lake-storage-events/create-spark-job.png "Bir iş oluşturma")

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

İş'i çalıştıran bir Azure İşi oluşturun.

1. Databricks çalışma alanının üst köşesinde, kişiler simgesini seçin ve ardından **Kullanıcı ayarlarını**seçin.

   ![Hesabı yönetme](./media/data-lake-storage-events/generate-token.png "Kullanıcı ayarları")

2. Yeni **belirteç oluştur** düğmesini tıklatın ve sonra **Oluştur** düğmesini tıklatın.

   Belirteci güvenli bir yere kopyaladığından emin olun. Azure İşcünüzün, İş'i çalıştırabilmesi için Databricks ile kimlik doğrulaması yapmak için bu belirteci gerekir.
  
3. Azure portalının sol üst köşesinde bulunan **kaynak oluştur** düğmesini seçin ve ardından **> İşlev Uygulaması'nı seçin.**

   ![Azure işlevi oluşturma](./media/data-lake-storage-events/function-app-create-flow.png "Azure işlevi oluşturma")

4. İşlev Uygulaması'nın **Oluştur** sayfasında, çalışma zamanı yığını için **.NET Core'u** seçtiğinizden ve Uygulama Öngörüleri örneğini yapılandırdığından emin olun.

   ![İşlev uygulamasını yapılandırma](./media/data-lake-storage-events/new-function-app.png "İşlev uygulamasını yapılandırma")

5. İşlev Uygulamasının **Genel Bakış** sayfasında **Yapılandırma'yı**tıklatın.

   ![İşlev uygulamasını yapılandırma](./media/data-lake-storage-events/configure-function-app.png "İşlev uygulamasını yapılandırma")

6. Uygulama **Ayarları** sayfasında, her ayarı eklemek için **Yeni uygulama ayarı** düğmesini seçin.

   ![Yapılandırma ayarı ekleme](./media/data-lake-storage-events/add-application-setting.png "Yapılandırma ayarı ekleme")

   Aşağıdaki ayarları ekleyin:

   |Ayar adı | Değer |
   |----|----|
   |**DBX_INSTANCE**| Veri tuğlalarınızın çalışma alanının bölgesi. Örneğin, `westus2.azuredatabricks.net`|
   |**DBX_PAT**| Daha önce oluşturduğunuz kişisel erişim belirteci. |
   |**DBX_JOB_ID**|Çalışan işin tanımlayıcısı. Bizim durumumuzda, bu `1`değer .|
7. İşlev uygulamasının genel bakış sayfasında **Yeni işlev** düğmesini tıklatın.

   ![Yeni işlev](./media/data-lake-storage-events/new-function.png "Yeni işlev")

8. **Azure Olay Izgara Tetikleyicisini**seçin.

   Sizden istenirse **Microsoft.Azure.WebJobs.Extensions.EventGrid** uzantısını yükleyin. Yüklemeniz gerekiyorsa, işlevi oluşturmak için Azure **Olay Izgara Tetikleyicisini** yeniden seçmeniz gerekir.

   **Yeni İşlev** bölmesi görüntülenir.

9. Yeni **İşlev** bölmesinde, **İşlev UpsertOrder'ı**adlandırın ve ardından **Oluştur** düğmesini tıklatın.

10. Kod dosyasının içeriğini bu kodla değiştirin ve sonra **Kaydet** düğmesini tıklatın:

    ```cs
    using "Microsoft.Azure.EventGrid"
    using "Newtonsoft.Json"
    using Microsoft.Azure.EventGrid.Models;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;

    private static HttpClient httpClient = new HttpClient();

    public static async Task Run(EventGridEvent eventGridEvent, ILogger log)
    {
        log.LogInformation("Event Subject: " + eventGridEvent.Subject);
        log.LogInformation("Event Topic: " + eventGridEvent.Topic);
        log.LogInformation("Event Type: " + eventGridEvent.EventType);
        log.LogInformation(eventGridEvent.Data.ToString());

        if (eventGridEvent.EventType == "Microsoft.Storage.BlobCreated" | | eventGridEvent.EventType == "Microsoft.Storage.FileRenamed") {
            var fileData = ((JObject)(eventGridEvent.Data)).ToObject<StorageBlobCreatedEventData>();
            if (fileData.Api == "FlushWithClose") {
                log.LogInformation("Triggering Databricks Job for file: " + fileData.Url);
                var fileUrl = new Uri(fileData.Url);
                var httpRequestMessage = new HttpRequestMessage {
                    Method = HttpMethod.Post,
                    RequestUri = new Uri(String.Format("https://{0}/api/2.0/jobs/run-now", System.Environment.GetEnvironmentVariable("DBX_INSTANCE", EnvironmentVariableTarget.Process))),
                    Headers = {
                        { System.Net.HttpRequestHeader.Authorization.ToString(), "Bearer " +  System.Environment.GetEnvironmentVariable ("DBX_PAT", EnvironmentVariableTarget.Process)},
                        { System.Net.HttpRequestHeader.ContentType.ToString (), "application/json" }
                    },
                    Content = new StringContent(JsonConvert.SerializeObject(new {
                        job_id = System.Environment.GetEnvironmentVariable ("DBX_JOB_ID", EnvironmentVariableTarget.Process) ,
                        notebook_params = new {
                            source_file = String.Join("", fileUrl.Segments.Skip(2))
                        }
                    }))
                 };
                var response = await httpClient.SendAsync(httpRequestMessage);
                response.EnsureSuccessStatusCode();
            }
        }
    }
    ```

   Bu kod, yükseltilen depolama olayı yla ilgili bilgileri ayrıştırır ve ardından olayı tetikleyen dosyanın url'sini içeren bir istek iletisi oluşturur. İletinin bir parçası olarak, işlev daha önce oluşturduğunuz **source_file** widget'ına bir değer geçirir. işlev kodu iletiyi Databricks Job'a gönderir ve daha önce kimlik doğrulaması olarak elde ettiğiniz belirteci kullanır.

## <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

Bu bölümde, dosyalar depolama hesabına yüklendiğinde Azure İşlevi çağıran bir Olay Ağısı aboneliği oluşturursunuz.

1. İşlev kodu sayfasında, **Olay Izgaraekle abonelik** düğmesini tıklatın.

   ![Yeni etkinlik aboneliği](./media/data-lake-storage-events/new-event-subscription.png "Yeni etkinlik aboneliği")

2. Etkinlik **Aboneliği Oluştur** sayfasında, aboneliği adlandırın ve depolama hesabınızı seçmek için sayfadaki alanları kullanın.

   ![Yeni etkinlik aboneliği](./media/data-lake-storage-events/new-event-subscription-2.png "Yeni etkinlik aboneliği")

3. Olay **Türleri** açılır listesinde, **Oluşturulan Blob**ve **Blob Deleted** olayları seçin ve ardından **Oluştur** düğmesini tıklatın.

## <a name="test-the-event-grid-subscription"></a>Olay Izgara aboneliğini test edin

1. Adlı `customer-order.csv`bir dosya oluşturun, aşağıdaki bilgileri bu dosyaya yapıştırın ve yerel bilgisayarınıza kaydedin.

   ```
   InvoiceNo,StockCode,Description,Quantity,InvoiceDate,UnitPrice,CustomerID,Country
   536371,99999,EverGlow Single,228,1/1/2018 9:01,33.85,20993,Sierra Leone
   ```

2. Depolama Gezgini'nde, bu dosyayı depolama hesabınızın **giriş** klasörüne yükleyin.

   Dosya yüklemek **Microsoft.Storage.BlobCreated** olayını yükseltir. Olay Grid bu olay için tüm aboneleri not. Bizim durumumuzda, Azure İşlevi tek abonedir. Azure İşlevi, hangi olayın gerçekleştiğini belirlemek için olay parametrelerini ayrıştirır. Daha sonra dosyanın URL'sini Databricks Job'a geçirir. Databricks Job dosyayı okur ve depolama hesabınızın bulunduğu Databricks Delta tablosuna bir satır ekler.

3. İşin başarılı olup olmadığını kontrol etmek için veri tuğlalarınızın çalışma alanını açın, **İşler** düğmesini tıklatın ve ardından işinizi açın.

4. İş sayfasını açmak için işi seçin.

   ![Kıvılcım işi](./media/data-lake-storage-events/spark-job.png "Kıvılcım işi")

   İş tamamlandığında, bir tamamlama durumu görürsünüz.

   ![Başarıyla tamamlanan iş](./media/data-lake-storage-events/spark-job-completed.png "Başarıyla tamamlanan iş")

5. Yeni bir çalışma kitabı hücresinde, güncelleştirilmiş delta tablosunu görmek için bu sorguyu bir hücrede çalıştırın.

   ```
   %sql select * from customer_data
   ```

   Döndürülen tablo en son kaydı gösterir.

   ![En son kayıt tabloda görünür](./media/data-lake-storage-events/final_query.png "En son kayıt tabloda görünür")

6. Bu kaydı güncelleştirmek için, `customer-order-update.csv`adlı bir dosya oluşturun, aşağıdaki bilgileri bu dosyaya yapıştırın ve yerel bilgisayarınıza kaydedin.

   ```
   InvoiceNo,StockCode,Description,Quantity,InvoiceDate,UnitPrice,CustomerID,Country
   536371,99999,EverGlow Single,22,1/1/2018 9:01,33.85,20993,Sierra Leone
   ```

   Bu csv dosyası, siparişin `228` miktarı nın . `22`

7. Depolama Gezgini'nde, bu dosyayı depolama hesabınızın **giriş** klasörüne yükleyin.

8. Güncelleştirilmiş `select` delta tablosunu görmek için sorguyu yeniden çalıştırın.

   ```
   %sql select * from customer_data
   ```

   Döndürülen tablo güncelleştirilen kaydı gösterir.

   ![Güncelleştirilmiş kayıt tabloda görünür](./media/data-lake-storage-events/final_query-2.png "Güncelleştirilmiş kayıt tabloda görünür")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve ilgili tüm kaynakları silin. Bunu yapmak için, depolama hesabı için kaynak grubunu seçin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Blob depolama olaylarına yanıt verme](storage-blob-event-overview.md)
