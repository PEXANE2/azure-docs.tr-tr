---
title: SQL Server Stored Yordam Etkinliği
description: Veri Fabrikası ardışık kaynaklarından Bir Azure SQL Veritabanı'nda veya Azure SQL Veri Ambarı'nda depolanan yordamı çağırmak için SQL Server Stored Yordam Etkinliği'ni nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
author: nabhishek
ms.author: abnarain
manager: anandsub
robots: noindex
ms.openlocfilehash: 45aa49de51f42b26c653b15e79c865e3f5647c39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931637"
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server Stored Yordam Etkinliği
> [!div class="op_single_selector" title1="Dönüşüm Faaliyetleri"]
> * [Kovan Etkinliği](data-factory-hive-activity.md)
> * [Domuz Aktivitesi](data-factory-pig-activity.md)
> * [MapReduce Etkinliği](data-factory-map-reduce.md)
> * [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Kıvılcım Etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET Özel Etkinlik](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nda depolanan yordam etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-stored-procedure.md)bakın.

## <a name="overview"></a>Genel Bakış
Ham verileri öngörülere ve öngörülere dönüştürmek ve işlemek için Veri Fabrikası [ardışık hattındaki](data-factory-create-pipelines.md) veri dönüştürme etkinliklerini kullanırsınız. Depolanan Yordam Etkinliği, Veri Fabrikası'nın desteklediği dönüşüm etkinliklerinden biridir. Bu makalede, veri dönüşümü ve Veri Fabrikası'nda desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi temel almaktadır.

İşletmenizdeki aşağıdaki veri depolarından birinde veya bir Azure sanal makinesinde (VM) depolanan yordamı çağırmak için Depolanan Yordam Etkinliği'ni kullanabilirsiniz:

- Azure SQL Veritabanı
- Azure SQL Veri Ambarı
- SQL Server Veritabanı. SQL Server kullanıyorsanız, Veritabanını barındıran aynı makineye veya veritabanına erişimi olan ayrı bir makineye Veri Yönetimi Ağ Geçidi'ni yükleyin. Veri Yönetimi Ağ Geçidi, Azure VM'deki veri kaynaklarını bulut hizmetleriyle şirket içinde/yerinde güvenli ve yönetilen bir şekilde birbirine bağlayan bir bileşendir. Ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın.

> [!IMPORTANT]
> Verileri Azure SQL Veritabanı'na veya SQL Server'a kopyalarken, **sqlWriterStoredProcedureName** özelliğini kullanarak depolanan yordamı çağırmak için kopyalama **etkinliğindeSqlSink'i** yapılandırabilirsiniz. Daha fazla bilgi için bkz. [kopya etkinliğinden Store saklı yordamı çağırın.](data-factory-invoke-stored-procedure-from-copy-activity.md) Özellik hakkında ayrıntılı bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), SQL [Server](data-factory-sqlserver-connector.md#copy-activity-properties). Bir kopyalama etkinliği kullanarak verileri Azure SQL Veri Ambarı'na kopyalarken depolanan yordamı çağırmak desteklenmez. Ancak, sql veri ambarında depolanan yordamı çağırmak için depolanan yordam etkinliğini kullanabilirsiniz.
>
> Azure SQL Veritabanı'ndan veya SQL Server veya Azure SQL Veri Ambarı'ndan veri kopyalarken, **sqlReaderStoredProcedureName** özelliğini kullanarak kaynak veritabanından verileri okumak için depolanmış bir yordam çağırmak için kopyalama etkinliğinde **SqlSource'u** yapılandırabilirsiniz. Daha fazla bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), SQL [Server](data-factory-sqlserver-connector.md#copy-activity-properties), Azure SQL [Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)

Aşağıdaki izbis, bir Azure SQL veritabanında depolanan yordamı çağırmak için bir ardışık ardışık işlemde Depolanan Yordam Etkinliği'ni kullanır.

## <a name="walkthrough"></a>Kılavuz
### <a name="sample-table-and-stored-procedure"></a>Örnek tablo ve depolanan yordam
1. SQL Server Management Studio'yu veya rahat ettiğiniz başka bir aracı kullanarak Azure SQL Veritabanınızda aşağıdaki **tabloyu** oluşturun. Datetimestamp sütunu, ilgili kimliğin oluşturulduğu tarih ve saattir.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    Kimlik benzersiz tanımlanır ve datetimestamp sütunu ilgili kimliğin oluşturulduğu tarih ve saattir.
    
    ![Örnek veriler](./media/data-factory-stored-proc-activity/sample-data.png)

    Bu örnekte, depolanan yordam bir Azure SQL Veritabanındadır. Depolanan yordam bir Azure SQL Veri Ambarı ve SQL Server Veritabanı'ndaysa, yaklaşım benzerdir. BIR SQL Server veritabanı için bir [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md)yüklemeniz gerekir.
2. **Örnek tabloya**veri ekleyen aşağıdaki **depolanmış yordamı** oluşturun.

    ```SQL
    CREATE PROCEDURE usp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > Parametrenin **adı** ve **kasası** (bu örnekte DateTime) ardışık hatlar/etkinlik JSON'da belirtilen parametreile eşleşmelidir. Depolanan yordam tanımında, **\@** parametre için önek olarak kullanıldığından emin olun.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. [Azure portalı](https://portal.azure.com/)’nda oturum açın.
2. Sol menüde **Yenİ'yi** tıklatın, **Intelligence + Analytics'e**tıklayın ve **Veri Fabrikası'nı**tıklatın.

    ![Yeni veri fabrikası](media/data-factory-stored-proc-activity/new-data-factory.png)
3. Yeni **veri fabrikası** bıçağına Ad için **SProcDF** girin. Azure Veri Fabrikası adları **genel olarak benzersizdir.** Fabrikanın başarılı bir şekilde oluşturulmasını sağlamak için veri fabrikasının adını adını adını sizin adınız ile önemeniz gerekir.

   ![Yeni veri fabrikası](media/data-factory-stored-proc-activity/new-data-factory-blade.png)
4. Azure **aboneliğinizi**seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
   1. **Yeni Oluştur'u** tıklatın ve kaynak grubu için bir ad girin.
   2. **Varolan'ı kullan'ı** tıklatın ve varolan bir kaynak grubunu seçin.
6. Data factory için **konum** seçin.
7. Bir sonraki oturum açtığınızda panodaki veri fabrikasını görebilmeniz için **panoya Pin'i** seçin.
8. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
9. Azure portalının **panosunda** oluşturulan veri fabrikasını görürsünüz. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.

   ![Veri Fabrikası ana sayfası](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Azure SQL bağlantılı bir hizmet oluşturma
Veri fabrikasını oluşturduktan sonra, örnek usp_sample tablo ve depolanan yordamı içeren Azure SQL veritabanınızı veri fabrikanıza bağlayan bir Azure SQL bağlantılı hizmet oluşturursunuz.

1. Veri Fabrikası Düzenleyicisi'ni başlatmak için **SProcDF** için **Yazar'ı** tıklatın ve **Veri Fabrikası** bıyığını dağıtın.
2. Komut çubuğunda **Yeni veri deposunu** tıklatın ve **Azure SQL Veritabanı'nı**seçin. Düzenleyicide Azure SQL bağlantılı bir hizmet oluşturmak için JSON komut dosyasını görmeniz gerekir.

   ![Yeni veri deposu](media/data-factory-stored-proc-activity/new-data-store.png)
3. JSON komut dosyasında aşağıdaki değişiklikleri yapın:

   1. Azure `<servername>` SQL Veritabanı sunucunuzun adıyla değiştirin.
   2. Tabloyu ve depolanan yordamı oluşturduğunuz veritabanıyla değiştirin. `<databasename>`
   3. Veritabanına erişimi olan kullanıcı hesabıyla değiştirin. `<username@servername>`
   4. Kullanıcı `<password>` hesabının parolasıyla değiştirin.

      ![Yeni veri deposu](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Bağlantılı hizmeti dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın. AzureSqlLinkedService'i soldaki ağaç görünümünde gördüğünüzden onaylayın.

    ![bağlantılı hizmet ile ağaç görünümü](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Depolanan yordam herhangi bir veri üretmese bile, depolanan yordam etkinliği için bir çıktı veri kümesi belirtmeniz gerekir. Bunun nedeni, etkinliğin zamanlamasını (etkinliğin ne sıklıkta çalıştırıldığı - saatlik, günlük vb.) çalıştıran çıktı veri kümesidir. Çıktı veri kümesi, bir Azure SQL Veritabanı veya Azure SQL Veri Ambarı veya depolanan yordamın çalışmasını istediğiniz bir SQL Server Veritabanı'na atıfta bulunan bağlantılı bir **hizmet** kullanmalıdır. Çıktı veri kümesi, başka bir etkinlik[(zincirleme faaliyetleri)](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) tarafından sonraki işleme için saklanan yordamın sonucunu geçirmek için bir yol olarak hizmet verebilir. Ancak, Veri Fabrikası bu veri kümesine depolanan yordamın çıktısını otomatik olarak yazmaz. Çıktı veri kümesinin işaret ettiği bir SQL tablosuna yazan depolanan yordamdır. Bazı durumlarda, çıktı veri kümesi **sahte** bir veri kümesi (depolanan yordamın çıktısını gerçekten tutmayan bir tabloya işaret eden bir veri kümesi) olabilir. Bu sahte veri kümesi yalnızca depolanan yordam etkinliğini çalıştırmak için zamanlamayı belirtmek için kullanılır.

1. Tıklayın **... **Araç çubuğunda daha fazla bilgi için **Yeni veri kümesini**tıklatın ve **Azure SQL'i**tıklatın. Komut çubuğunda **yeni veri kümesi** ve Azure **SQL'i**seçin.

    ![bağlantılı hizmet ile ağaç görünümü](media/data-factory-stored-proc-activity/new-dataset.png)
2. Aşağıdaki JSON komut dosyasını JSON düzenleyicisine kopyalayın/yapıştırın.

    ```JSON
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. Veri kümesini dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın. Veri kümesini ağaç görünümünde gördüğünüzden onaylayın.

    ![bağlantılı hizmetlerle ağaç görünümü](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>SqlServerStoredProcedure etkinliği ile bir ardışık işlem hattı oluşturma
Şimdi, depolanmış bir yordam etkinliği olan bir boru hattı oluşturalım.

Aşağıdaki özelliklere dikkat edin:

- **Tür** özelliği **SqlServerStoredProcedure**olarak ayarlanır.
- Tür özelliklerinde **depolanan YordamAdı** **usp_sample** olarak ayarlanır (depolanan yordamın adı).
- Depolanan **YordamParametreleri** bölümünde **DateTime**adlı bir parametre bulunmaktadır. JSON'daki parametrenin adı ve gövdesi, depolanan yordam tanımındaki parametrenin adı ve kasası ile eşleşmelidir. Bir parametre için null pass'e ihtiyacınız `"param1": null` varsa, sözdizimini kullanın: (tümü küçük harf).

1. Tıklayın **... **Komut çubuğunda daha fazla ve **Yeni ardışık tıklatın.**
2. Aşağıdaki JSON parçacığı kopyalayın/yapıştırın:

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "usp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
            "start": "2017-04-02T00:00:00Z",
            "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. Ardışık hattı dağıtmak için araç çubuğuna **Dağıt'ı** tıklatın.

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory dikey penceresine dönmek için **X** işaretine, sonra da **Diyagram**’a tıklayın.

    ![diyagram döşemesi](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. Diyagram **Görünümü'nde,** bu öğreticide kullanılan ardışık hatlar ve veri kümelerinin genel görünümünü görürsünüz.

    ![diyagram döşemesi](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Diyagram Görünümü'nde, veri kümesini `sprocsampleout`çift tıklatın. Dilimleri Hazır durumunda görüyorsunuz. JSON'dan başlangıç saati ile bitiş saati arasında her saat için bir dilim üretildiği için beş dilim olmalıdır.

    ![diyagram döşemesi](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Bir dilim **Hazır** durumundayken, `select * from sampletable` verilerin depolanan yordam tarafından tabloya eklendiğinden doğrulamak için Azure SQL veritabanına karşı bir sorgu çalıştırın.

   ![Çıktı verileri](./media/data-factory-stored-proc-activity/output.png)

   Azure Veri Fabrikası boru hatlarını izleme hakkında ayrıntılı bilgi için [boru hattını izleyin'e](data-factory-monitor-manage-pipelines.md) bakın.

## <a name="specify-an-input-dataset"></a>Giriş veri kümesini belirtin
Izbarada, depolanan yordam etkinliği herhangi bir giriş veri kümeleri yok. Bir giriş veri kümesi belirtirseniz, giriş veri kümesinin dilimi kullanılabilir duruma gelene kadar (Hazır durumda) depolanan yordam etkinliği çalışmaz. Veri kümesi harici bir veri kümesi (aynı ardışık işlemdeki başka bir etkinlik tarafından üretilmez) veya bir yukarı akış etkinliği (bu etkinlikten önce çalışan etkinlik) tarafından üretilen bir iç veri kümesi olabilir. Depolanan yordam etkinliği için birden çok giriş veri kümesi belirtebilirsiniz. Bunu yaparsanız, depolanan yordam etkinliği yalnızca tüm giriş veri kümesi dilimleri kullanılabilir olduğunda (Hazır durumda) çalışır. Giriş veri kümesi, depolanan yordamda parametre olarak tüketilemez. Yalnızca depolanan yordam etkinliğini başlatmadan önce bağımlılığı denetlemek için kullanılır.

## <a name="chaining-with-other-activities"></a>Diğer etkinliklerle zincirleme
Bu etkinlikle bir yukarı akım etkinliğini zincirlemek istiyorsanız, bu etkinliğin girişi olarak yukarı akış etkinliğinin çıktısını belirtin. Bunu yaptığınızda, depolanan yordam etkinliği, akış yukarı işlemi tamamlanana ve akış yukarı işleminin çıktı veri kümesi kullanılabilir olana (Hazır durumunda) kadar çalışmaz. Birden çok akış lı etkinliğin çıktı veri kümelerini, depolanan yordam etkinliğinin giriş veri kümeleri olarak belirtebilirsiniz. Bunu yaptığınızda, depolanan yordam etkinliği yalnızca tüm giriş veri kümesi dilimleri kullanılabilir olduğunda çalışır.

Aşağıdaki örnekte, kopyalama etkinliğinin çıktısı: Depolanan yordam etkinliğinin bir girişi olan OutputDataset'tir. Bu nedenle, kopya etkinliği tamamlanana ve OutputDataset dilimi kullanılabilir olana (Hazır durumda) depolanan yordam etkinliği çalışmaz. Birden çok giriş veri kümesi belirtirseniz, tüm giriş veri kümesi dilimleri kullanılabilir olana kadar (Hazır durumda) depolanan yordam etkinliği çalışmaz. Giriş veri kümeleri, depolanan yordam etkinliğine parametre olarak doğrudan kullanılamaz.

Zincirleme faaliyetleri hakkında daha fazla bilgi için, [bir boru hattındaki birden çok faaliyete](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline) bakın

```json
{
    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }
        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

Benzer şekilde, depo yordamı etkinliğini **alt akış etkinliklerine** (depolanan yordam etkinliği tamamlandıktan sonra çalışan etkinliklere) bağlamak için, depolanan yordam etkinliğinin çıktı veri kümesini, ardışık işlem deki akış aşağı etkinliğinin girişi olarak belirtin.

> [!IMPORTANT]
> Verileri Azure SQL Veritabanı'na veya SQL Server'a kopyalarken, **sqlWriterStoredProcedureName** özelliğini kullanarak depolanan yordamı çağırmak için kopyalama **etkinliğindeSqlSink'i** yapılandırabilirsiniz. Daha fazla bilgi için bkz. [kopya etkinliğinden Store saklı yordamı çağırın.](data-factory-invoke-stored-procedure-from-copy-activity.md) Özellik hakkında ayrıntılı bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), SQL [Server](data-factory-sqlserver-connector.md#copy-activity-properties).
> 
> Azure SQL Veritabanı'ndan veya SQL Server veya Azure SQL Veri Ambarı'ndan veri kopyalarken, **sqlReaderStoredProcedureName** özelliğini kullanarak kaynak veritabanından verileri okumak için depolanmış bir yordam çağırmak için kopyalama etkinliğinde **SqlSource'u** yapılandırabilirsiniz. Daha fazla bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), SQL [Server](data-factory-sqlserver-connector.md#copy-activity-properties), Azure SQL [Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)

## <a name="json-format"></a>JSON biçimi
Burada, Depolanan Yordam Etkinliği tanımlamak için JSON biçimi vereme biçimi vereme

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs": [ { "name": "inputtable" } ],
    "outputs": [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":
        {
            "param1": "param1Value"
            …
        }
    }
}
```

Aşağıdaki tabloda bu JSON özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad | Etkinliğin adı |Evet |
| açıklama |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| type | Ayarlanmalıdır: **SqlServerStoredProcedure** | Evet |
| Giriş | İsteğe bağlı. Bir giriş veri kümesi belirtirseniz, depolanan yordam etkinliğinin çalışması için kullanılabilir ('Hazır' durumunda) olmalıdır. Giriş veri kümesi, depolanan yordamda parametre olarak tüketilemez. Yalnızca depolanan yordam etkinliğini başlatmadan önce bağımlılığı denetlemek için kullanılır. |Hayır |
| Çıkış | Depolanan yordam etkinliği için bir çıktı veri kümesi belirtmeniz gerekir. Çıktı veri kümesi, depolanan yordam etkinliğinin (saatlik, haftalık, aylık vb.) **zamanlamasını** belirtir. <br/><br/>Çıktı veri kümesi, bir Azure SQL Veritabanı veya Azure SQL Veri Ambarı veya depolanan yordamın çalışmasını istediğiniz bir SQL Server Veritabanı'na atıfta bulunan bağlantılı bir **hizmet** kullanmalıdır. <br/><br/>Çıktı veri kümesi, başka bir etkinlik[(zincirleme faaliyetleri)](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) tarafından sonraki işleme için saklanan yordamın sonucunu geçirmek için bir yol olarak hizmet verebilir. Ancak, Veri Fabrikası bu veri kümesine depolanan yordamın çıktısını otomatik olarak yazmaz. Çıktı veri kümesinin işaret ettiği bir SQL tablosuna yazan depolanan yordamdır. <br/><br/>Bazı durumlarda, çıktı veri kümesi yalnızca depolanan yordam etkinliğini çalıştırmak için zamanlamayı belirtmek için kullanılan sahte bir **veri kümesi**olabilir. |Evet |
| depolanmışProcedureName |Azure SQL veritabanında veya çıktı tablosunun kullandığı bağlantılı hizmet tarafından temsil edilen Azure SQL Veri Ambarı veya SQL Server veritabanında depolanan yordamın adını belirtin. |Evet |
| depolanmışProsedürParametreleri |Depolanan yordam parametreleri için değerleri belirtin. Bir parametre için null geçmeniz gerekiyorsa, sözdizimini kullanın: "param1": null (tüm küçük harf). Bu özelliği kullanma hakkında bilgi edinmek için aşağıdaki örneğe bakın. |Hayır |

## <a name="passing-a-static-value"></a>Statik bir değer geçirme
Şimdi, tabloya 'Belge örneği' adlı statik bir değer içeren 'Senaryo' adlı başka bir sütun eklemeyi düşünelim.

![Örnek veriler 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tablo:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Depolanan yordam:**

```SQL
CREATE PROCEDURE usp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Şimdi, **Senaryo** parametresini ve depolanan yordam etkinliğinden değeri geçirin. Önceki örnekteki **typeProperties** bölümü aşağıdaki parçacıkgibi görünür:

```JSON
"typeProperties":
{
    "storedProcedureName": "usp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Veri Fabrikası veri kümesi:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Veri Fabrikası boru hattı**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "usp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```
