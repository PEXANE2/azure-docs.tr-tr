---
title: Azure Data Factory - Team Data Science Process ile SQL Server verileri için SQL Azure
description: Bulutta ve şirket veritabanları arasında verileri günlük olarak birlikte taşımak iki veri taşıma etkinlikleri ölçeklemesini ADF işlem hattı ayarlayın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8f696f1c6c414cd9db082e79e0f34c56156e1ee0
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722501"
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Azure Data Factory ile SQL Azure için bir şirket içi SQL Server'dan veri taşıma

Bu makalede, Azure Blob depolama aracılığıyla Azure Data Factory (ADF) kullanarak verileri şirket içi SQL Server veritabanından SQL Azure veritabanına taşıma işlemi gösterilmektedir: Bu yöntem, çoğaltılan bir hazırlama kopyasının avantajlarına sahip desteklenen bir eski yaklaşımdır, ancak [en son seçenekler için veri geçiş sayfamıza bakmak tavsiye ederiz](https://datamigration.microsoft.com/scenario/sql-to-azuresqldb?step=1).

Verileri bir Azure SQL veritabanına taşımaya yönelik çeşitli seçenekleri özetleyen bir tablo için bkz. [Azure Machine Learning için verileri Azure SQL veritabanına taşıma](move-sql-azure.md).

## <a name="intro"></a>Giriş: ADF nedir ve verileri geçirmek için ne zaman kullanılmalıdır?
Azure Data Factory, taşımayı ve dönüştürmeyi düzenleyen ve otomatikleştiren bir tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. ADF modelinde en önemli kavram, işlem hattı ' dir. Bir işlem hattı her biri veri kümelerinde bulunan veriler üzerinde gerçekleştirilecek eylemleri tanımlar. bir mantıksal etkinlik grubudur. Bağlı hizmetler için veri kaynaklarına bağlanmak Data Factory'ye gereken bilgileri tanımlamak için kullanılır.

ADF ile mevcut veri işleme Hizmetleri içinde bulutta yüksek oranda kullanılabilir ve yönetilen veri işlem hatları kullanılamayacağı. Bu veri işlem hatları, içe alma, hazırlama, dönüştürme, analiz ve veri yayımlama zamanlanabilir ve ADF yönetir ve işleme bağımlılıkları ve karmaşık veri düzenler. Çözümleri hızla oluşturulabilen ve bulutta, şirket içi giderek artan sayıda bağlanma dağıtılan ve bulut veri kaynakları.

ADF kullanmayı dikkate alın:

* sürekli olarak geçirilmesi verilere ihtiyaç duyduğunda erişen hem de karma bir senaryoda şirket içi ve bulut kaynakları
* veriler dönüşümlere ihtiyaç duyduğunda veya geçirildiğinde iş mantığı eklendiğinde.

Zamanlama ve düzenli aralıklarla veri taşıma işlemlerini yönetmek basit JSON betiklerini kullanarak işleri izlemek için ADF sağlar. ADF karmaşık işlemleri desteği gibi diğer özellikleri de vardır. ADF hakkında daha fazla bilgi için [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/)belgelerine bakın.

## <a name="scenario"></a>Senaryo
İki veri taşıma etkinlikleri ölçeklemesini bir ADF ardışık ayarladık. Bir arada, verileri şirket içi SQL veritabanı ve buluttaki bir Azure SQL veritabanı arasında günlük olarak taşır. İki etkinliği şunlardır:

* bir Azure Blob Depolama hesabı için bir şirket içi SQL Server veritabanından veri kopyalama
* verileri Azure Blob Depolama hesabından bir Azure SQL veritabanı'na kopyalayın.

> [!NOTE]
> Burada gösterilen adımlar, ADF ekibi tarafından sunulan daha ayrıntılı öğreticiden uyarlanmıştır: Şirket [içi SQL Server veritabanından Azure Blob depolama başvurularına veri kopyalama](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal/) ilgili konunun ilgili bölümlerine, uygun olduğunda verilmiştir.
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğreticide, sahip olduğunuz varsayılır:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabını kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [depolama hesabı erişim anahtarlarını yönetme](../../storage/common/storage-account-keys-manage.md).
* **Azure SQL veritabanına**erişim. Azure SQL veritabanı ayarlamanız gerekirse [Microsoft Azure SQL veritabanı Ile çalışmaya](../../sql-database/sql-database-get-started.md) başlama konusu, Azure SQL veritabanı 'nın yeni bir örneğini sağlama hakkında bilgi sağlar.
* **Azure PowerShell** yerel olarak yüklendi ve yapılandırıldı. Yönergeler için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/overview).

> [!NOTE]
> Bu yordam [Azure Portal](https://portal.azure.com/)kullanır.
>
>

## <a name="upload-data"></a>Verileri şirket içi SQL Server yükleyin
Geçiş işlemini göstermek için [NYC TAXI veri kümesini](https://chriswhong.com/open-data/foil_nyc_taxi/) kullanıyoruz. NYC TAXI veri kümesi, Azure Blob Storage [NYC TAXI verilerinde](https://www.andresmh.com/nyctaxitrips/)bu gönderde belirtildiği gibi kullanılabilir. Verileri iki dosya, seyahat ayrıntıları içeren trip_data.csv dosyasının ve her seyahat için ücretli taksi ayrıntılarını içeren trip_far.csv dosyası vardır. Bu dosyaların bir örneği ve açıklaması [NYC TAXI gezme veri kümesi açıklamasında](sql-walkthrough.md#dataset)verilmiştir.

Burada kendi veri kümesi için sağlanan yordamı uyarlamak veya NYC taksi veri kümesini kullanarak açıklanan adımları izleyin. NYC TAXI veri kümesini şirket içi SQL Server veritabanınıza yüklemek için, [verileri SQL Server veritabanına toplu Içeri aktarma](sql-walkthrough.md#dbload)bölümünde özetlenen yordamı izleyin. SQL Server üzerinde bir Azure sanal makine için bu yönergeleri yöneliktir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="create-adf"></a>Azure Data Factory oluşturma
Yeni bir Azure Data Factory ve [Azure Portal](https://portal.azure.com/) kaynak grubu oluşturma yönergeleri [Azure Data Factory oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-data-factory). Yeni ADF örneğini *adfdsp* olarak adlandırın ve kaynak grubunun adını *adfdsprg*olarak adlandırın.

## <a name="install-and-configure-azure-data-factory-integration-runtime"></a>Yükleme ve Azure Data Factory Integration Runtime'ı yapılandırma
Integration Runtime, farklı ağ ortamlarında veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan müşteri tarafından yönetilen bir veri tümleştirme altyapısıdır. Bu çalışma zamanı, eski adıyla "Veri yönetimi ağ geçidi" olarak adlandırılıyordu.

Ayarlamak için işlem [hattı oluşturma yönergelerini izleyin](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal#create-a-pipeline)

## <a name="adflinkedservices"></a>Veri kaynaklarına bağlanmak için bağlı hizmetler oluşturma
Bağlı hizmet için bir veri kaynağına bağlanmak Azure Data Factory'ye gereken bilgileri tanımlar. Bağlı hizmetler için gerekli olan bu senaryoda üç kaynak sunuyoruz:

1. Şirket içi SQL Server
2. Azure Blob Depolama
3. Azure SQL Veritabanı

Bağlı hizmetler oluşturmak için adım adım yordam, [bağlı hizmetler oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline)bölümünde verilmiştir.


## <a name="adf-tables"></a>Veri kümelerine nasıl erişecağınızı belirtmek için tablo tanımlama ve oluşturma
Şu betiği tabanlı yordamları yapısı, konumu ve kullanılabilirlik kümelerinin belirtin tabloları oluşturun. JSON dosyaları, tabloları tanımlamak için kullanılır. Bu dosyaların yapısı hakkında daha fazla bilgi için bkz. [veri kümeleri](../../data-factory/concepts-datasets-linked-services.md).

> [!NOTE]
> Komut yürütmesi için doğru Azure aboneliğinin seçili olduğunu onaylamak üzere [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet 'ini yürütmeden önce `Add-AzureAccount` cmdlet 'ini yürütmelisiniz. Bu cmdlet 'in belgeleri için bkz. [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

JSON tabanlı tanımları tablolarda aşağıdaki adlar kullanın:

* Şirket içi SQL Server 'daki **tablo adı** *nyctaxi_data*
* Azure Blob depolama hesabındaki **kapsayıcı adı** *ContainerName*

Bu ADF işlem hattı için üç tablo tanımları gerekir:

1. [SQL şirket içi tablosu](#adf-table-onprem-sql)
2. [Blob tablosu](#adf-table-blob-store)
3. [SQL Azure tablosu](#adf-table-azure-sql)

> [!NOTE]
> Bu yordamlar, tanımlamak ve ADF etkinlikleri oluşturmak için Azure PowerShell kullanırsınız. Ancak bu görevleri Azure portalını kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz. [veri kümeleri oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

### <a name="adf-table-onprem-sql"></a>SQL şirket içi tablosu
Şirket içi SQL Server için tablo tanımı aşağıdaki JSON dosyasında belirtilir:

```json
{
    "name": "OnPremSQLTable",
    "properties":
    {
        "location":
        {
            "type": "OnPremisesSqlServerTableLocation",
            "tableName": "nyctaxi_data",
            "linkedServiceName": "adfonpremsql"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1,
            "waitOnExternal":
            {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Sütun adlarını burada bulunmuyordu. Sütun adlarında bunları buraya ekleyerek seçebilirsiniz (Ayrıntılar için [ADF belgelerinin](../../data-factory/copy-activity-overview.md) konusunu inceleyin.

Tablonun JSON tanımını *onpremtabledef. JSON* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\onpremtabledef.exe*olarak kabul edilir). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>Blob tablosu
Çıktı blob konumu için tablo tanımıdır (Bu eşler alınan verileri şirket içinden Azure blob'a) aşağıdaki:

```json
{
    "name": "OutputBlobTable",
    "properties":
    {
        "location":
        {
            "type": "AzureBlobLocation",
            "folderPath": "containername",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": "\t"
            },
            "linkedServiceName": "adfds"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Tablonun JSON tanımını *bloi puttabledef. JSON* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\blomaputtabledef.exe*olarak kabul edilir). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json

### <a name="adf-table-azure-sql"></a>SQL Azure tablosu
SQL Azure tablosu için tanımı çıkış aşağıdaki (Bu şema eşler blobundan gelen veriler):

```json
{
    "name": "OutputSQLAzureTable",
    "properties":
    {
        "structure":
        [
            { "name": "column1", "type": "String"},
            { "name": "column2", "type": "String"}
        ],
        "location":
        {
            "type": "AzureSqlTableLocation",
            "tableName": "your_db_name",
            "linkedServiceName": "adfdssqlazure_linked_servicename"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Tablonun JSON tanımını *Azurestabtable. JSON* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada, *C:\temp\azuresoptable.exe*olarak kabul edilir). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json


## <a name="adf-pipeline"></a>İşlem hattını tanımlama ve oluşturma
Etkinlikler işlem hattına ait ve aşağıdaki betiği tabanlı yordamları ile işlem hattı oluşturma belirtin. Bir JSON dosyası, işlem hattı özelliklerini tanımlamak için kullanılır.

* Betik, işlem **hattı adının** *Amldsprocesspipeline*olduğunu varsayar.
* Ayrıca, günlük olarak yürütülür ve iş (12: 00 UTC) için varsayılan yürütme süresi kullanmak için işlem hattının dönemsellik ayarladığımız unutmayın.

> [!NOTE]
> Aşağıdaki yordamlar, tanımlamak ve ADF işlem hattını oluşturmak için Azure PowerShell kullanın. Ancak, bu görevi Azure portalını kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz. işlem [hattı oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

Daha önce sağlanan tablo tanımlarını kullanarak ADF için işlem hattı tanımını gibi belirtilir:

```json
{
    "name": "AMLDSProcessPipeline",
    "properties":
    {
        "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
        "activities":
        [
            {
                "name": "CopyFromSQLtoBlob",
                "description": "Copy data from on-premises SQL server to blob",
                "type": "CopyActivity",
                "inputs": [ {"name": "OnPremSQLTable"} ],
                "outputs": [ {"name": "OutputBlobTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from nyctaxi_data"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            },
            {
                "name": "CopyFromBlobtoSQLAzure",
                "description": "Push data to Sql Azure",
                "type": "CopyActivity",
                "inputs": [ {"name": "OutputBlobTable"} ],
                "outputs": [ {"name": "OutputSQLAzureTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "BlobSource"
                    },
                    "sink":
                    {
                        "type": "SqlSink",
                        "WriteBatchTimeout": "00:5:00",
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 2,
                    "timeout": "02:00:00"
                }
            }
        ]
    }
}
```

İşlem hattının bu JSON tanımını, *pipelinedef. JSON* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\pipelinedef.exe*olarak kabul edilir). İşlem hattı, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="adf-pipeline-start"></a>İşlem hattını başlatma
İşlem hattı artık aşağıdaki komutu kullanarak çalıştırılabilir:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*StartDate* ve *EndDate* parametre değerlerinin, işlem hattının çalışmasını istediğiniz gerçek tarihlerle değiştirilmeleri gerekir.

İşlem hattı yürütür sonra blob, günde bir dosya için seçilen kapsayıcıdaki görünmesini verileri görebildiğine olmalıdır.

ADF tarafından verileri artımlı olarak boru için sunulan işlevleri yararlanılabilir ettik. ADF 'nin sağladığı bu ve diğer yeteneklerin nasıl yapılacağı hakkında daha fazla bilgi için [ADF belgelerine](https://azure.microsoft.com/services/data-factory/)bakın.
