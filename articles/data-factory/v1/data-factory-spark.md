---
title: Azure Veri Fabrikası'ndan Spark programlarını çağırma
description: MapReduce etkinliğini kullanarak bir Azure veri fabrikasından Spark programlarını nasıl çağıracağınız hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: ce5fb014c7d954b3e8430a86430c6a666adff204
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75969241"
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Azure Veri Fabrikası ardışık hatlarından Spark programlarını çağırma

> [!div class="op_single_selector" title1="Dönüşüm Faaliyetleri"]
> * [Kovan aktivitesi](data-factory-hive-activity.md)
> * [Domuz aktivitesi](data-factory-pig-activity.md)
> * [MapAz etkinliği](data-factory-map-reduce.md)
> * [Hadoop Akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Kıvılcım etkinliği](data-factory-spark.md)
> * [Makine Öğrenimi Toplu Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Makine Öğrenimi Güncelleme Kaynak etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Depolanan yordam etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Apache Spark etkinliğini kullanarak verileri dönüştür'e](../transform-data-using-spark.md)bakın.

## <a name="introduction"></a>Giriş
Kıvılcım etkinliği, Veri Fabrikası tarafından desteklenen [veri dönüştürme etkinliklerinden](data-factory-data-transformation-activities.md) biridir. Bu etkinlik, Azure HDInsight'ta Spark kümenizde belirtilen Spark programını çalıştırAr.

> [!IMPORTANT]
> - Spark etkinliği, Birincil depolama alanı olarak Azure Veri Gölü Deposu'nu kullanan HDInsight Spark kümelerini desteklemez.
> - Spark etkinliği yalnızca varolan (kendi) HDInsight Spark kümelerini destekler. İsteğe bağlı HDInsight bağlantılı bir hizmeti desteklemez.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>Walkthrough: Bir Kıvılcım etkinliği ile bir boru hattı oluşturma
Spark etkinliği olan bir veri fabrikası ardışık hattı oluşturmak için tipik adımlar şunlardır:

* Veri fabrikası oluşturma.
* HDInsight Spark kümenizle ilişkili depolama alanınızı veri fabrikasına bağlamak için bir Azure Depolama bağlantılı hizmet oluşturun.
* Spark kümenizi HDInsight'taki veri fabrikasına bağlamak için BIR HDInsight bağlantılı hizmet oluşturun.
* Depolama bağlantılı hizmete başvuran bir veri kümesi oluşturun. Şu anda, üretilen çıktı olmasa bile bir etkinlik için bir çıktı veri kümesi belirtmeniz gerekir.
* Oluşturduğunuz HDInsight bağlantılı hizmete atıfta bulunan Spark etkinliği içeren bir ardışık hatlar oluştur. Etkinlik, önceki adımda oluşturduğunuz veri kümesiyle çıktı veri kümesi olarak yapılandırılır. Çıktı veri kümesi zamanlamayı (saatlik, günlük) yönlendiren şeydir. Bu nedenle, etkinlik gerçekten bir çıktı üretmese bile çıktı veri kümesini belirtmeniz gerekir.

### <a name="prerequisites"></a>Ön koşullar
1. Depolama hesabı oluştur'daki yönergeleri izleyerek genel amaçlı [bir depolama hesabı oluşturun.](../../storage/common/storage-account-create.md)

1. HDInsight'ta bir Kıvılcım kümesi oluştur öğreticideki yönergeleri izleyerek [HDInsight'ta bir Kıvılcım kümesi oluşturun.](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md) Adım 1'de oluşturduğunuz depolama hesabını bu kümeyle ilişkilendirin.

1. Python komut dosyası **dosyasını** test.py'da [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py)bulunan dosyayı indirin ve inceleyin.

1. **test.py** blob depolamanızdaki **adfspark** kapsayıcısındaki **pyFiles** klasörüne yükleyin. Yoksa kapsayıcıyı ve klasörü oluşturun.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için bu adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Yeni** > **Veri + Analitik** > **Veri Fabrikası'nı**seçin.

1. Yeni **veri fabrikası** bıçak, **Adı**altında, **SparkDF**girin.

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı SparkDF kullanılamıyor" hatasını görürseniz, veri fabrikasının adını değiştirin. Örneğin, adınızı SparkDFdate kullanın ve veri fabrikasını yeniden oluşturun. Adlandırma kuralları hakkında daha fazla bilgi için bkz. [Data Factory: Adlandırma kuralları](data-factory-naming-rules.md).

1. Abonelik bölümünde, veri fabrikasını oluşturmak istediğiniz **Azure aboneliğini** seçin.

1. Varolan bir kaynak grubu seçin veya bir Azure kaynak grubu oluşturun.

1. **Panoya sabitle** onay kutusunu seçin.

1. **Oluştur'u**seçin.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.

1. Azure portalının panosunda oluşturulan veri fabrikasını görürsünüz.

1. Veri fabrikası oluşturulduktan sonra veri fabrikasının içeriğinin gösterildiği **Veri fabrikası** sayfasını görürsünüz. **Veri fabrikası** sayfasını görmüyorsanız, panodaki veri fabrikanızın döşemesini seçin.

    ![Data Factory dikey penceresi](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, iki bağlantılı hizmet oluşturursunuz. Bir hizmet, Kıvılcım kümenizi veri fabrikanıza, diğer hizmet ise depolama alanınızı veri fabrikanıza bağlar.

#### <a name="create-a-storage-linked-service"></a>Depolama bağlı hizmeti oluşturma
Bu adımda, depolama hesabınızı veri fabrikanıza bağlarsınız. Bu iznin ilerleyen adımlarında oluşturduğunuz bir veri kümesi, bu bağlantılı hizmete başvurur. Bir sonraki adımda tanımladığınız HDInsight bağlantılı hizmet, bu bağlantılı hizmete de başvurur.

1. Veri **fabrika** bıçağında **Yazar'ı seçin ve dağıtın.** Veri Fabrikası Düzenleyicisi görüntülenir.

1. **Yeni veri deposu**’na tıklayın ve **Azure Depolama**’yı seçin.

   ![Yeni veri deposu](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

1. Depolama bağlantılı bir hizmet oluşturmak için kullandığınız JSON komut dosyası düzenleyicide görünür.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

1. **Hesap adı** ve **hesap anahtarını** depolama hesabınızın adı ve erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınızı öğrenmek için [bkz.](../../storage/common/storage-account-keys-manage.md)

1. Bağlantılı hizmeti dağıtmak için komut çubuğunda **Dağıt'ı** seçin. Bağlı hizmet başarıyla dağıtıldıktan sonra Draft-1 penceresi kaybolur. Soldaki ağaç görünümünde **AzureStorageLinkedService** öğesini görürsünüz.

#### <a name="create-an-hdinsight-linked-service"></a>HDInsight bağlı hizmeti oluşturma
Bu adımda, HDInsight Spark kümenizi veri fabrikasına bağlamak için BIR HDInsight bağlantılı hizmet oluşturursunuz. HDInsight kümesi, bu örnekteki ardışık ardışık ardışık lının Spark etkinliğinde belirtilen Spark programını çalıştırmak için kullanılır.

1. Veri Fabrikası Düzenleyicisi'nde **Daha Fazla** > **Yeni bilgi işlem** > **HDInsight kümesini**seçin.

    ![HDInsight bağlı hizmeti oluşturma](media/data-factory-spark/new-hdinsight-linked-service.png)

1. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON düzenleyicisinde aşağıdaki adımları atın:

    a. HDInsight Spark kümesi için URI'yi belirtin. Örneğin: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Kıvılcım kümesine erişimi olan kullanıcının adını belirtin.

    c. Kullanıcının parolasını belirtin.

    d. HDInsight Spark kümesiyle ilişkili Depolama bağlantılı hizmeti belirtin. Bu örnekte, AzureStorageLinkedService'dir.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Spark etkinliği, Birincil depolama alanı olarak Azure Veri Gölü Deposu'nu kullanan HDInsight Spark kümelerini desteklemez.
    > - Spark etkinliği yalnızca varolan (kendi) HDInsight Spark kümelerini destekler. İsteğe bağlı HDInsight bağlantılı bir hizmeti desteklemez.

    HDInsight bağlantılı hizmet hakkında daha fazla bilgi için [HDInsight bağlantılı hizmete](data-factory-compute-linked-services.md#azure-hdinsight-linked-service)bakın.

1. Bağlantılı hizmeti dağıtmak için komut çubuğunda **Dağıt'ı** seçin.

### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Çıktı veri kümesi zamanlamayı (saatlik, günlük) yönlendiren şeydir. Bu nedenle, etkinlik herhangi bir çıktı üretmese bile, ardışık ardışık taki spark etkinliği için bir çıktı veri kümesi belirtmeniz gerekir. Etkinlik için bir giriş veri kümesi belirtmek isteğe bağlıdır.

1. Veri Fabrikası Düzenleyicisi'nde **Daha Fazla** > **Yeni veri kümesi** > **Azure Blob depolama alanını**seçin.

1. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON snippet **OutputDataset**adlı bir veri kümesi tanımlar. Buna ek olarak, sonuçların **adfspark** adı verilen blob kapsayıcısında ve **pyFiles/output**adlı klasörde depolanır. Daha önce de belirtildiği gibi, bu veri kümesi sahte bir veri kümesidir. Bu örnekteki Kıvılcım programı herhangi bir çıktı üretmez. **Kullanılabilirlik** bölümü, çıktı veri kümesinin günlük olarak üretildiğini belirtir.

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
1. Veri kümesini dağıtmak için komut çubuğunda **Dağıt'ı** seçin.


### <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, HDInsightSpark etkinliği olan bir ardışık hatlar oluşturursunuz. Şu anda, zamanlama çıktı veri kümesi tarafından yönetildiğinden, etkinlik hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Bu nedenle, bu örnekte hiçbir giriş veri kümesi belirtilmemiştir.

1. Veri Fabrikası Düzenleyicisi'nde **Daha Fazla** > **Yeni ardışık hat lar**seçin.

1. Draft-1 penceresindeki komut dosyasını aşağıdaki komut dosyasıyla değiştirin:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Aşağıdaki noktalara dikkat edin:

    a. **Tür** özelliği **HDInsightSpark**olarak ayarlanır.

    b. **RootPath** özelliği adfspark blob konteyner ve pyFiles bu kapsayıcıda dosya klasörü olduğu **adfspark\\pyFiles** ayarlanır. Bu örnekte, blob depolama Kıvılcım kümesi ile ilişkili biridir. Dosyayı farklı bir depolama hesabına yükleyebilirsiniz. Bunu yaparsanız, bu depolama hesabını veri fabrikasına bağlamak için Depolama bağlantılı bir hizmet oluşturun. Ardından, bağlı hizmetin adını **sparkJobLinkedService** özelliği için bir değer olarak belirtin. Bu özellik ve Spark etkinliği tarafından desteklenen diğer özellikler hakkında daha fazla bilgi [için](#spark-activity-properties)Bkz.

    c. **entryFilePath** özelliği Python dosyası olan **test.py**olarak ayarlanır.

    d. **getDebugInfo** özelliği Her **Zaman**olarak ayarlanır, bu da günlük dosyalarının her zaman oluşturulduğu (başarı veya hata) anlamına gelir.

    > [!IMPORTANT]
    > Sorun gidermediğiniz sürece bu `Always` özelliği üretim ortamında ayarlamamanızı öneririz.

    e. **Çıktılar** bölümünde bir çıktı veri kümesi vardır. Spark programı çıktı üretmese bile bir çıktı veri kümesi belirtmeniz gerekir. Çıktı veri kümesi, ardışık alan için zamanlamayı (saatlik, günlük) yönlendirir.

    Kıvılcım etkinliği tarafından desteklenen özellikler hakkında daha fazla bilgi için, [Bkz.](#spark-activity-properties)

1. Ardışık komut u dağıtmak için komut çubuğunda **Dağıt'ı** seçin.

### <a name="monitor-a-pipeline"></a>İşlem hattını izleme
1. Veri **fabrika** bıçağında, izleme uygulamasını başka bir sekmede başlatmak için Monitör & Yönet'i'ni seçin. **Monitor & Manage**

    ![İzleme ve Yönetme kutucuğu](media/data-factory-spark/monitor-and-manage-tile.png)

1. En üstteki **Başlangıç saati** filtresini **2/1/2017**olarak değiştirin ve **Uygula'yı**seçin.

1. Boru hattının başlangıç (2017-02-01) ile bitiş saatleri (2017-02-02) arasında yalnızca bir gün olduğundan yalnızca bir etkinlik penceresi görüntülenir. Veri diliminin **Hazır** durumunda olduğunu doğrulayın.

    ![İşlem hattını izleme](media/data-factory-spark/monitor-and-manage-app.png)

1. Etkinlik **pencereleri** listesinde, bu konudaki ayrıntıları görmek için bir etkinlik çalıştır'ı seçin. Bir hata varsa, bunun ayrıntılarını sağ bölmede görürsünüz.

### <a name="verify-the-results"></a>Sonuçları doğrulayın

1. Bu web sitesine giderek HDInsight Spark kümeniz için Jupyter [Notebook'u](https://CLUSTERNAME.azurehdinsight.net/jupyter)başlatın. Ayrıca HDInsight Spark kümeniz için bir küme panosu açabilir ve ardından Jupyter Notebook'u başlatabilirsiniz.

1. Yeni bir dizüstü bilgisayar başlatmak için **Yeni** > **PySpark'ı** seçin.

    ![Jupyter yeni dizüstü bilgisayar](media/data-factory-spark/jupyter-new-book.png)

1. Metni kopyalayıp yapıştırarak ve ikinci ifadenin sonunda Shift+Enter tuşuna basarak aşağıdaki komutu çalıştırın:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
1. İklimlendirme tablosundaki verileri gördüğünüzü doğrulayın.

    ![Jupyter sorgu sonuçları](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Ayrıntılı talimatlar için, [bir Kıvılcım SQL sorgusu çalıştır](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md)bölümüne bakın.

### <a name="troubleshooting"></a>Sorun giderme
GetDebugInfo'yu **Her Zaman**olarak ayarladığınızda, blob kapsayıcınızda pyFiles klasöründe bir günlük alt klasörü görürsünüz. Günlük klasöründeki günlük dosyası ek bilgiler sağlar. Bu günlük dosyası özellikle bir hata olduğunda kullanışlıdır. Üretim ortamında, **bunu Hata**olarak ayarlamak isteyebilirsiniz.

Daha fazla sorun giderme için aşağıdaki adımları izleyin:


1. `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster` kısmına gidin.

    ![İplik UI uygulaması](media/data-factory-spark/yarnui-application.png)

1. Çalıştırma denemelerinden biri için **Günlükler'i** seçin.

    ![Başvuru sayfası](media/data-factory-spark/yarn-applications.png)

1. Günlük sayfasında aşağıdaki ek hata bilgilerini görürsünüz:

    ![Günlük hatası](media/data-factory-spark/yarnui-application-error.png)

Aşağıdaki bölümler, veri fabrikanızda Spark kümesi ve Kıvılcım etkinliğini kullanmak için veri fabrikası varlıkları hakkında bilgi sağlar.

## <a name="spark-activity-properties"></a>Kıvılcım etkinlik özellikleri
Burada bir Kıvılcım etkinliği ile bir boru hattı örnek JSON tanımı:

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    },
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| ad | Boru hattındaki etkinliğin adı. | Evet |
| açıklama | Etkinliğin ne yaptığını açıklayan metin. | Hayır |
| type | Bu özellik HDInsightSpark olarak ayarlanmalıdır. | Evet |
| linkedServiceName | Spark programının çalıştığı HDInsight bağlantılı hizmetin adı. | Evet |
| rootPath | Spark dosyasını içeren blob kapsayıcısı ve klasörü. Dosya adı büyük/küçük harf duyarlıdır. | Evet |
| entryFilePath | Kıvılcım kodunun/paketinin kök klasörüne göreli yol. | Evet |
| Classname | Uygulamanın Java/Spark ana sınıfı. | Hayır |
| Bağımsız değişken | Spark programına komut satırı bağımsız değişkenlerinin listesi. | Hayır |
| proxyKullanıcı | Spark programını yürütmek için taklit etmek için kullanıcı hesabı. | Hayır |
| kıvılcımConfig | Spark yapılandırmasında listelenen Spark yapılandırma özellikleri için değerleri [belirtin: Uygulama özellikleri.](https://spark.apache.org/docs/latest/configuration.html#available-properties) | Hayır |
| getDebugInfo | Kıvılcım günlük dosyalarının sparkJobLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan depolama alanına kopyalandığında belirtir. İzin verilen değerler Yok, Her Zaman veya Hatadır. Varsayılan değer, Yok'tur. | Hayır |
| sparkJobLinkedService | Spark iş dosyasını, bağımlılıkları ve günlükleri tutan Depolama bağlantılı hizmet. Bu özellik için bir değer belirtmezseniz, HDInsight kümesiyle ilişkili depolama kullanılır. | Hayır |

## <a name="folder-structure"></a>Klasör yapısı
Spark etkinliği, Pig and Hive etkinliklerinin desteklediği gibi satır lı bir komut dosyasını desteklemez. Kıvılcım işleri de Pig / Hive işleri daha genişletilebilir. Kıvılcım işleri için, kavanoz paketleri (java CLASSPATH'e yerleştirilir), Python dosyaları (PYTHONPATH'e yerleştirilir) ve diğer dosyalar gibi birden çok bağımlılık sağlayabilirsiniz.

HDInsight bağlantılı hizmet tarafından başvurulan blob depolama aşağıdaki klasör yapısını oluşturun. Daha sonra, bağımlı dosyaları **entryFilePath**tarafından temsil edilen kök klasöründe uygun alt klasörlere yükleyin. Örneğin, Python dosyalarını pyFiles alt klasörüne ve kavanoz dosyalarına kök klasörün kavanoz alt klasörüne yükleyin. Çalışma zamanında, Veri Fabrikası hizmeti blob depolamada aşağıdaki klasör yapısını bekler:

| Yol | Açıklama | Gerekli | Tür |
| ---- | ----------- | -------- | ---- |
| . | Depolama bağlantılı hizmetteki Spark işinin kök yolu. | Evet | Klasör |
| &lt;kullanıcı tanımlı&gt; | Kıvılcım işinin giriş dosyasına işaret eden yol. | Evet | Dosya |
| ./kavanozlar | Bu klasörün altındaki tüm dosyalar yüklenir ve kümenin Java sınıf yoluna yerleştirilir. | Hayır | Klasör |
| ./pyFiles | Bu klasörün altındaki tüm dosyalar yüklenir ve kümenin PYTHONPATH'ine yerleştirilir. | Hayır | Klasör |
| ./dosyalar | Bu klasörün altındaki tüm dosyalar yüklenir ve uygulayıcı çalışma dizinine yerleştirilir. | Hayır | Klasör |
| ./arşivler | Bu klasörün altındaki tüm dosyalar sıkıştırılmamış. | Hayır | Klasör |
| ./günlükleri | Kıvılcım kümesinden günlüklerin depolandığı klasör.| Hayır | Klasör |

HDInsight bağlantılı hizmet tarafından başvurulan blob depolama iki Spark iş dosyaları içeren depolama için bir örnek aşağıda verilmiştir:

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
