---
title: SQL Server verileri Azure Veri Fabrikası ile SQL Azure'a - Takım Veri Bilimi Süreci
description: Verileri şirket içinde ve bulutta veritabanları arasında günlük olarak hareket ettiren iki veri geçiş aktivitesi oluşturan bir ADF ardışık hattı ayarlayın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722501"
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Azure Veri Fabrikası ile verileri şirket içi SQL sunucusundan SQL Azure'a taşıma

Bu makalede, Azure Veri Fabrikası (ADF) kullanarak verileri şirket içi SQL Server Veritabanından Azure Blob Depolama üzerinden SQL Azure Veritabanına nasıl taşıyacağınızı gösterir: Bu yöntem, yinelenen bir evreleme kopyasının avantajlarına sahip desteklenen bir eski yaklaşımdır, ancak [en son seçenekler için veri geçiş sayfamıza bakmanızı öneririz.](https://datamigration.microsoft.com/scenario/sql-to-azuresqldb?step=1)

Verileri Azure SQL Veritabanına taşımak için çeşitli seçenekleri özetleyen bir tablo [için](move-sql-azure.md)bkz.

## <a name="introduction-what-is-adf-and-when-should-it-be-used-to-migrate-data"></a><a name="intro"></a>Giriş: ADF nedir ve verileri geçirmek için ne zaman kullanılmalıdır?
Azure Veri Fabrikası, verilerin hareketini ve dönüşümunu düzenleyen ve otomatikleştiren tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. ADF modelinde anahtar kavram boru hattıdır. Ardışık iş, her biri Veri Kümeleri'nde bulunan verilerüzerinde gerçekleşecek eylemleri tanımlayan mantıksal bir Etkinlik grubudur. Bağlantılı hizmetler, Veri Fabrikası'nın veri kaynaklarına bağlanması için gereken bilgileri tanımlamak için kullanılır.

ADF ile, varolan veri işleme hizmetleri bulutta yüksek oranda kullanılabilir ve yönetilen veri ardışık hatlarında oluşturulabilir. Bu veri ardışık hatları verileri yutmak, hazırlamak, dönüştürmek, analiz etmek ve yayımlamak için zamanlanabilir ve ADF karmaşık verileri ve işleme bağımlılıklarını yönetir ve yönetir. Çözümler, giderek artan sayıda şirket içi ve bulut veri kaynağını birbirine bağlayarak bulutta hızla oluşturulabilir ve dağıtılabilir.

ADF kullanmayı düşünün:

* verilerin hem şirket içi hem de bulut kaynaklarına erişen karma bir senaryoda sürekli olarak geçirilmesi gerektiğinde
* verilerin dönüşümlere ihtiyacı olduğunda veya geçirilirken iş mantığının eklenmesigerektiğinde.

ADF, verilerin hareketini periyodik olarak yöneten basit JSON komut dosyalarını kullanarak işlerin zamanlanmasına ve izlenmesine olanak tanır. ADF'nin karmaşık işlemler için destek gibi başka yetenekleri de vardır. ADF hakkında daha fazla bilgi için [Azure Veri Fabrikası'ndaki (ADF)](https://azure.microsoft.com/services/data-factory/)belgelere bakın.

## <a name="the-scenario"></a><a name="scenario"></a>Senaryo
İki veri geçiş aktivitesi oluşturan bir ADF ardışık hattı kurduk. Birlikte, verileri şirket içi BIR SQL Veritabanı ile buluttaki bir Azure SQL Veritabanı arasında günlük olarak taşırlar. İki etkinlik şunlardır:

* verileri şirket içi SQL Server veritabanından Azure Blob Depolama hesabına kopyalama
* verileri Azure Blob Depolama hesabından bir Azure SQL Veritabanına kopyalayın.

> [!NOTE]
> Burada gösterilen adımlar, ADF ekibi tarafından sağlanan daha ayrıntılı öğreticiden uyarlanmıştır: [Şirket içi bir SQL Server veritabanından Azure Blob depolama](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal/) Başvuruları'na verileri uygun olduğunda sağlanmıştır.
>
>

## <a name="prerequisites"></a><a name="prereqs"></a>Ön koşullar
Bu öğretici var sayıyor:

* **Azure aboneliği.** Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı.** Verileri bu öğreticide depolamak için bir Azure depolama hesabı kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz. [Depolama hesabı erişim anahtarlarını yönet.](../../storage/common/storage-account-keys-manage.md)
* **Azure SQL Veritabanına**erişim. Bir Azure SQL Veritabanı ayarlamanız gerekiyorsa, [Microsoft Azure SQL Veritabanı ile Başlarken](../../sql-database/sql-database-get-started.md) konusu, Azure SQL Veritabanı'nın yeni bir örneğini nasıl sağbırakabileceğiniz hakkında bilgi sağlar.
* **Azure PowerShell'i** yerel olarak yükledi ve yapılandırıldı. Talimatlar için Azure [PowerShell'i nasıl yükleyip yapılandıracağınıza](/powershell/azure/overview)bakın.

> [!NOTE]
> Bu yordam, [Azure portalını](https://portal.azure.com/)kullanır.
>
>

## <a name="upload-the-data-to-your-on-premises-sql-server"></a><a name="upload-data"></a>Verileri şirket içi SQL Server'ınıza yükleyin
Geçiş işlemini göstermek için [NYC Taxi veri kümesini](https://chriswhong.com/open-data/foil_nyc_taxi/) kullanıyoruz. NYC Taksi veri seti, bu yazı belirtildiği gibi, Azure blob depolama [NYC Taksi Veri](https://www.andresmh.com/nyctaxitrips/)mevcuttur. Veriler, gezi ayrıntılarını içeren trip_data.csv dosyası ve her yolculuk için ödenen ücretin ayrıntılarını içeren trip_far.csv dosyası olmak üzere iki dosyaya sahiptir. Bir örnek ve bu dosyaların açıklaması [NYC Taksi Gezileri Dataset Açıklama](sql-walkthrough.md#dataset)sağlanmaktadır.

Burada sağlanan yordamı kendi verilerinizin kümesine uyarlayabilir veya NYC Taxi veri kümesini kullanarak açıklanan adımları izleyebilirsiniz. NYC Taxi veri kümesini şirket içi SQL Server veritabanınıza yüklemek için, Toplu Alma Verilerinde özetlenen yordamı [SQL Server Veritabanı'na](sql-walkthrough.md#dbload)uygulayın. Bu yönergeler Azure Sanal Makine'deki bir SQL Server içindir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="create-an-azure-data-factory"></a><a name="create-adf"></a>Azure Veri Fabrikası Oluşturma
[Azure portalında](https://portal.azure.com/) yeni bir Azure Veri Fabrikası ve kaynak grubu oluşturma yönergeleri [bir Azure Veri Fabrikası Oluştur](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-data-factory)sağlanır. Yeni ADF örneği *adfdsp* adı ve kaynak grubu *adfdsprg*oluşturulan adı .

## <a name="install-and-configure-azure-data-factory-integration-runtime"></a>Azure Veri Fabrikası Tümleştirme Çalışma Süresini yükleme ve yapılandırma
Tümleştirme Runtime, Azure Veri Fabrikası tarafından farklı ağ ortamlarında veri tümleştirme yetenekleri sağlamak için kullanılan müşteri tarafından yönetilen bir veri tümleştirme altyapısıdır. Bu çalışma süresi eskiden "Veri Yönetimi Ağ Geçidi" olarak adlandırılıyordu.

Kurmak [için, bir boru hattı oluşturmak için yönergeleri izleyin](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal#create-a-pipeline)

## <a name="create-linked-services-to-connect-to-the-data-resources"></a><a name="adflinkedservices"></a>Veri kaynaklarına bağlanmak için bağlantılı hizmetler oluşturma
Bağlantılı bir hizmet, Azure Veri Fabrikası'nın bir veri kaynağına bağlanması için gereken bilgileri tanımlar. Bu senaryoda, bağlantılı hizmetlere ihtiyaç duyulan üç kaynağımız vardır:

1. Şirket Içi SQL Server
2. Azure Blob Depolama
3. Azure SQL Veritabanı

Bağlantılı hizmetler oluşturmak için adım adım [yordam, Bağlantılı Hizmetler Oluştur'da](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline)sağlanır.


## <a name="define-and-create-tables-to-specify-how-to-access-the-datasets"></a><a name="adf-tables"></a>Veri kümelerine nasıl erişilisüreceğini belirtmek için tabloları tanımlama ve oluşturma
Aşağıdaki komut dosyası tabanlı yordamlarla veri kümelerinin yapısını, konumunu ve kullanılabilirliğini belirten tablolar oluşturun. JSON dosyaları tabloları tanımlamak için kullanılır. Bu dosyaların yapısı hakkında daha fazla bilgi için [Datasets bölümüne](../../data-factory/concepts-datasets-linked-services.md)bakın.

> [!NOTE]
> Komut yürütme `Add-AzureAccount` için doğru Azure aboneliğinin seçildiğini doğrulamak için [Yeni AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet'i çalıştırmadan önce cmdlet'i çalıştırmalısınız. Bu cmdlet'in belgeleri için Bkz. [Azure Hesabı Ekle.](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-3.7.0)
>
>

Tablolardaki JSON tabanlı tanımlar aşağıdaki adları kullanır:

* şirket içi SQL sunucusundaki **tablo adı** *nyctaxi_data*
* Azure Blob Depolama hesabındaki **kapsayıcı adı** *kapsayıcı adıdır*

Bu ADF ardışık hattı için üç tablo tanımı gereklidir:

1. [SQL şirket içi Tablo](#adf-table-onprem-sql)
2. [Blob Tablo](#adf-table-blob-store)
3. [SQL Azure Tablosu](#adf-table-azure-sql)

> [!NOTE]
> Bu yordamlar, ADF etkinliklerini tanımlamak ve oluşturmak için Azure PowerShell'i kullanır. Ancak bu görevler Azure portalı kullanılarak da gerçekleştirilebilir. Ayrıntılar için bkz. [veri kümelerini oluştur.](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline)
>
>

### <a name="sql-on-premises-table"></a><a name="adf-table-onprem-sql"></a>SQL şirket içi Tablo
Şirket içi SQL Server için tablo tanımı aşağıdaki JSON dosyasında belirtilmiştir:

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

Sütun adları burada yer almadı. Sütun adlarını buraya ekleyerek alt seçim yapabilirsiniz (ayrıntılar için [ADF dokümantasyon](../../data-factory/copy-activity-overview.md) konusunu kontrol edin.

Tablonun JSON tanımını *onpremtabledef.json* adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\onpremtabledef.json*olarak kabul edilir). Aşağıdaki Azure PowerShell cmdlet ile ADF tablo oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="blob-table"></a><a name="adf-table-blob-store"></a>Blob Tablo
Çıktı blob konumu için tablo tanımı aşağıdaki gibidir (bu haritalar şirket içinde Azure blob için yutulan verileri eşler):

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

Tablonun JSON tanımını *bloboutputtabledef.json* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\bloboutputtabledef.json*olarak kabul edilir). Aşağıdaki Azure PowerShell cmdlet ile ADF tablo oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json

### <a name="sql-azure-table"></a><a name="adf-table-azure-sql"></a>SQL Azure Tablosu
SQL Azure çıktısı için tablo tanımı aşağıdaki gibidir (bu şema blob gelen verileri eşler):

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

Tablonun JSON tanımını *AzureSqlTable.json* adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\AzureSqlTable.json*olarak kabul edilir). Aşağıdaki Azure PowerShell cmdlet ile ADF tablo oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json


## <a name="define-and-create-the-pipeline"></a><a name="adf-pipeline"></a>Boru hattını tanımlayın ve oluşturun
Ardışık ardışık aygıta ait etkinlikleri belirtin ve aşağıdaki komut dosyası tabanlı yordamlarla ardışık ardışık ardışık ardışık Bir JSON dosyası, ardışık hat özelliklerini tanımlamak için kullanılır.

* Komut **dosyası, ardışık işlem adı** *AMLDSProcessPipeline*olduğunu varsayar.
* Ayrıca, günlük olarak yürütülecek boru hattının periyodikliğini ayarladiğimizı ve iş için varsayılan yürütme süresini kullandığımızı da unutmayın (12:00 UTC).

> [!NOTE]
> Aşağıdaki yordamlar, ADF ardışık hattını tanımlamak ve oluşturmak için Azure PowerShell'i kullanır. Ancak bu görev Azure portalı kullanılarak da gerçekleştirilebilir. Ayrıntılar için [bkz.](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline)
>
>

Daha önce sağlanan tablo tanımları kullanılarak, ADF için boru hattı tanımı aşağıdaki gibi belirtilir:

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

Pipeline'ın bu JSON tanımını *pipelinedef.json* dosyası adlı bir dosyaya kopyalayın ve bilinen bir konuma kaydedin (burada *C:\temp\pipelinedef.json*olarak kabul edilir). Aşağıdaki Azure PowerShell cmdlet ile ADF'de ardışık hatlar oluştur:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="start-the-pipeline"></a><a name="adf-pipeline-start"></a>Boru Hattını Başlat
Ardışık hatlar artık aşağıdaki komut kullanılarak çalıştırılabilir:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*Başlangıç tarihi* ve bitiş *tarihi* parametre değerlerinin, ardışık hattın çalışmasını istediğiniz gerçek tarihlerle değiştirilmesi gerekir.

Ardışık iş tonu yürütüldünden sonra, veri blob için seçilen kapsayıcıda, günde bir dosya olarak görünebilirsiniz.

Verileri aşamalı olarak aktarmak için ADF tarafından sağlanan işlevselliği kullanamayacağız. Bunun nasıl yapılacılacıyla ilgili daha fazla bilgi ve ADF tarafından sağlanan diğer yürütücüler hakkında daha fazla bilgi için [ADF belgelerine](https://azure.microsoft.com/services/data-factory/)bakın.
