---
title: Azure Veri Fabrikası - JSON Scripting Reference
description: Veri Fabrikası varlıkları için JSON şemasağlar.
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
ms.openlocfilehash: 3492f917be8116d0eed0c7ec03ed8aa9ff506520
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346590"
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Veri Fabrikası - JSON Scripting Reference
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir.


Bu makalede, Azure Veri Fabrikası varlıklarını (ardışık işlem, etkinlik, veri kümesi ve bağlantılı hizmet) tanımlamak için JSON şemaları ve örnekleri verilmektedir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="pipeline"></a>İşlem hattı
Bir boru hattı tanımının üst düzey yapısı aşağıdaki gibidir:

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
}
```

Aşağıdaki tabloda json ardışık hatlar hattı tanımı içindeki özellikler açıklanmaktadır:

| Özellik | Açıklama | Gerekli
-------- | ----------- | --------
| ad | İşlem hattının adı. Etkinliğin veya ardışık yapının yapmak üzere yapılandırıldığı eylemi temsil eden bir ad belirtin<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf numarası veya bir alt\_puan ile başlamanız gerekir ( )</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/", "<",">","","","","%","&",":","\\"</li></ul> |Evet |
| açıklama |Etkinlik veya ardışık nedenin ne için kullanıldığını açıklayan metin | Hayır |
| etkinlikler | Etkinliklerin listesini içerir. | Evet |
| start |Boru hattı için tarih-saat başlatın. [ISO formatında](https://en.wikipedia.org/wiki/ISO_8601)olmalıdır. Örneğin: 2014-10-14T16:32:41. <br/><br/>Yerel bir saat belirtmek mümkündür, örneğin bir EST zamanı. İşte bir örnek: `2016-02-27T06:00:00**-05:00`, 06 EST olduğunu.<br/><br/>Başlangıç ve bitiş özellikleri birlikte ardışık hatlar için etkin dönemi belirtir. Çıkış dilimleri sadece bu etkin dönemde üretilir. |Hayır<br/><br/>Son özellik için bir değer belirtirseniz, başlangıç özelliği için değer belirtmeniz gerekir.<br/><br/>Başlangıç ve bitiş saatleri, bir ardışık hat lar oluşturmak için boş olabilir. Ardışık hattın çalışması için etkin bir dönem ayarlamak için her iki değeri de belirtmeniz gerekir. Bir ardışık hatlar oluştururken başlangıç ve bitiş saatlerini belirtmezseniz, bunları Daha sonra Set-AzDataFactoryPipelineActivePeriod cmdlet kullanarak ayarlayabilirsiniz. |
| end |Boru hattı için bitiş tarihi. Belirtilen iso formatında olmalıdır. Örneğin: 2014-10-14T17:32:41 <br/><br/>Yerel bir saat belirtmek mümkündür, örneğin bir EST zamanı. İşte bir örnek: `2016-02-27T06:00:00**-05:00`, 06 EST olduğunu.<br/><br/>İşlem hattını süresiz olarak çalıştırmak için end özelliği değerini 9999-09-09 olarak ayarlayın. |Hayır <br/><br/>Başlangıç özelliği için bir değer belirtirseniz, bitiş özelliği için değer belirtmeniz gerekir.<br/><br/>**Başlangıç** özelliği için notlara bakın. |
| isPaused |Doğru ayarlanırsa, ardışık hatlar çalışmaz. Varsayılan değer = false. Etkinleştirmek veya devre dışı kalmak için bu özelliği kullanabilirsiniz. |Hayır |
| pipelineMode |Zamanlama yöntemi, ardışık hatlar için çalışır. İzin verilen değerler şunlardır: zamanlanmış (varsayılan), tek seferlik.<br/><br/>'Zamanlanmış' ardışık ardışık ardışık ardışık etkin döneme (başlangıç ve bitiş saati) göre belirli bir zaman aralığında çalıştığını gösterir. 'Onetime' boru hattının yalnızca bir kez çalıştığını gösterir. Bir kez oluşturulan tek seferlik ardışık ardışık lar şu anda değiştirilemez/güncelleştirilemez. Tek seferlik ayar la ilgili ayrıntılar için [Onetime ardışık hattına](data-factory-create-pipelines.md#onetime-pipeline) bakın. |Hayır |
| sona ermeZamanı |Boru hattının geçerli olduğu ve sağlanması gereken yaratılıştan sonraki süre. Etkin, başarısız veya bekleyen çalışır yoksa, son kullanma süresine ulaştığında ardışık alan otomatik olarak silinir. |Hayır |


## <a name="activity"></a>Etkinlik
Bir boru hattı tanımı (etkinlik öğesi) içindeki bir etkinlik için üst düzey yapı aşağıdaki gibidir:

```json
{
    "name": "ActivityName",
    "description": "description",
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    },
    "scheduler":
    {
    }
}
```

Aşağıdaki tabloda json tanımı ndaki özellikleri açıklayınız:

| Etiket | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinliğin adı. Etkinliğin yapmak üzere yapılandırıldığı eylemi temsil eden bir ad belirtin<br/><ul><li>En fazla karakter sayısı: 260</li><li>Bir harf numarası veya bir alt\_puan ile başlamanız gerekir ( )</li><li>Aşağıdaki karakterlere izin verilmez: ".", "+", "?", "/", "<",">","","","","%","&",":","\\"</li></ul> |Evet |
| açıklama |Etkinliğin ne için kullanıldığını açıklayan metin. |Hayır |
| type |Etkinliğin türünü belirtir. Farklı türde etkinlikler için [VERİ MAĞAZALARI](#data-stores) ve [VERİ DÖNÜŞÜM FAALİYETLerİ](#data-transformation-activities) bölümlerine bakın. |Evet |
| Giriş |Etkinlik tarafından kullanılan giriş tabloları<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |HDInsightStreaming ve SqlServerStoredProcedure etkinlikleri için hayır <br/> <br/> Evet diğerleri için |
| Çıkış |Etkinlik tarafından kullanılan çıktı tabloları.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Evet |
| linkedServiceName |Etkinlik tarafından kullanılan bağlı hizmetin adı. <br/><br/>Bir etkinlik için gerekli işlem ortamına bağlanan bağlı hizmeti belirtmeniz gerekebilir. |HDInsight etkinlikleri, Azure Machine Learning etkinlikleri ve Saklı Yordam Etkinliği için evet. <br/><br/>Diğer tümü için hayır |
| typeProperties |TypeProperties bölümündeki özellikler etkinliğin türüne bağlıdır. |Hayır |
| ilke |Etkinliğin çalışma zamanı davranışını etkileyen ilkeler. Belirtilmemişse, varsayılan ilkeler kullanılır. |Hayır |
| scheduler |Etkinlik için istenen zamanlamayı tanımlamak için "zamanlayıcı" özelliği kullanılır. Alt özellikleri, [bir veri kümesindeki kullanılabilirlik özelliğindeki özelliklerle](data-factory-create-datasets.md#dataset-availability)aynıdır. |Hayır |

### <a name="policies"></a>İlkeler
İlkeler, özellikle bir tablonun dilimi işlendiğinde, bir etkinliğin çalışma zamanı davranışını etkiler. Aşağıdaki tablo ayrıntıları sağlar.

| Özellik | İzin verilen değerler | Varsayılan Değer | Açıklama |
| --- | --- | --- | --- |
| eşzamanlılık |Tamsayı <br/><br/>Maksimum değer: 10 |1 |Etkinliğin eşzamanlı yürütme sayısı.<br/><br/>Farklı dilimlerde gerçekleşebilecek paralel etkinlik yürütmelerinin sayısını belirler. Örneğin, bir etkinliğin büyük bir kullanılabilir veri kümesinden geçmesi gerekiyorsa, daha büyük bir eşzamanlılık değerine sahip olmak veri işlemeyi hızlandırİr. |
| yürütmeÖncelikli Sipariş |En Yeni İlk<br/><br/>En Eskiİlk |En Eskiİlk |İşlenen veri dilimlerinin sırasını belirler.<br/><br/>Örneğin, 2 dilim (biri 16:00'da, diğeri 17:00'de oluyor) varsa ve her ikisi de yürütmeyi bekliyorsa. YürütmePriorityOrder'ı NewestFirst olarak ayarlarsanız, ilk olarak saat 17:00'deki dilim işlenir. Benzer şekilde, executionPriorityORder'ı En Eski FIrst olarak ayarlarsanız, saat 16:00'daki dilim işlenir. |
| retry |Tamsayı<br/><br/>Maksimum değer 10 olabilir |0 |Dilimiçin veri işleme önce yeniden deneme sayısı Hata olarak işaretlenir. Bir veri dilimiiçin etkinlik yürütme belirtilen yeniden deneme sayısına kadar yeniden denendir. Yeniden deneme, arızadan sonra mümkün olan en kısa sürede yapılır. |
| timeout |TimeSpan |00:00:00 |Etkinlik için zaman dilimi. Örnek: 00:10:00 (zaman 10 dakika anlamına gelir)<br/><br/>Bir değer belirtilmemişse veya 0 ise, zaman acısı sonsuzdur.<br/><br/>Bir dilimdeki veri işleme süresi zaman aşımı değerini aşarsa, iptal edilir ve sistem işlemeyi yeniden denemeye çalışır. Yeniden deneme sayısı yeniden deneme özelliğine bağlıdır. Zaman anına kadar zaman dilimi oluştuğunda, durum TimedOut olarak ayarlanır. |
| Gecikme |TimeSpan |00:00:00 |Dilimin veri işlemesi başlamadan önce gecikmeyi belirtin.<br/><br/>Bir veri dilimi için etkinliğin yürütülmesi, Gecikme beklenen yürütme süresini geçtikten sonra başlatılır.<br/><br/>Örnek: 00:10:00 (10 dakika gecikme anlamına gelir) |
| longRetry |Tamsayı<br/><br/>Maksimum değer: 10 |1 |Dilim yürütme başarısız olmadan önce uzun yeniden deneme denemelerinin sayısı.<br/><br/>longRetry girişimleri longRetryInterval tarafından aralıklı. Bu nedenle, yeniden deneme denemeleri arasında bir süre belirtmeniz gerekiyorsa, longRetry'yi kullanın. Hem Retry ve longRetry belirtilirse, her longRetry girişimi Retry girişimleri içerir ve girişimleri maksimum sayıda Retry * longRetry olduğunu.<br/><br/>Örneğin, etkinlik ilkesinde aşağıdaki ayarlar varsa:<br/>Yeniden deneme sayısı: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Yürütülecek tek bir dilim olduğunu varsayalım (durum Bekliyor) ve etkinlik yürütmesi her seferinde başarısız oluyor. Başlangıçta art arda 3 infaz girişimi olacaktır. Her denemeden sonra, dilim durumu Yeniden Deneme olacaktır. İlk 3 deneme bittikten sonra, dilim durumu LongRetry olacaktır.<br/><br/>Bir saat sonra (yani, longRetryInteval değeri), 3 ardışık yürütme girişimleri başka bir dizi olacaktır. Bundan sonra, dilim durumu Başarısız olur ve başka yeniden deneme denilmeye çalışılacak. Bu nedenle genel 6 girişimleri yapıldı.<br/><br/>Herhangi bir yürütme başarılı olursa, dilim durumu Hazır olur ve başka yeniden deneme denenir.<br/><br/>longRetry, bağımlı verilerin deterministik olmayan zamanlarda geldiği veya genel ortamın veri işlemenin gerçekleştiği durumlarda kullanılabilir. Bu gibi durumlarda, birbiri ardına yeniden denemeler yapmak yardımcı olmayabilir ve bunu bir süre sonra yapmak istenilen çıktıile sonuçlanır.<br/><br/>Dikkat Word: longRetry veya longRetryInterval için yüksek değerler ayarlamayın. Genellikle, daha yüksek değerler diğer sistemik sorunları ima. |
| longRetryInterval |TimeSpan |00:00:00 |Uzun yeniden deneme denemeleri arasındaki gecikme |

### <a name="typeproperties-section"></a>typeProperties bölümü
TypeProperties bölümü her etkinlik için farklıdır. Dönüştürme etkinlikleri yalnızca tür özelliklerine sahiptir. Bir boru hattındadönüşüm faaliyetlerini tanımlayan JSON örnekleri için bu makalede [VERİ DÖNÜŞÜM FAALİYETLerİ](#data-transformation-activities) bölümüne bakın.

**Kopyalama etkinliğinin** typeProperties bölümünde iki alt bölümü vardır: **kaynak** ve **lavabo.** Bir veri deposunun kaynak ve/veya lavabo olarak nasıl kullanılacağını gösteren JSON örnekleri için bu makalede [DATA STORES](#data-stores) bölümüne bakın.

### <a name="sample-copy-pipeline"></a>Örnek kopyalama işlem hattı
Aşağıdaki örnek işlem hattında, **Etkinlikler** bölümünde **Kopyalama** türünde olan bir etkinlik vardır. Bu örnekte, [Kopyalama etkinliği](data-factory-data-movement-activities.md) verileri bir Azure Blob depolama alanından Azure SQL veritabanına kopyalar.

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
}
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde, **türü****Copy** olarak ayarlanmış yalnızca bir etkinlik vardır.
* Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır.
* **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

Bir veri deposunun kaynak ve/veya lavabo olarak nasıl kullanılacağını gösteren JSON örnekleri için bu makalede [DATA STORES](#data-stores) bölümüne bakın.

Bu ardışık alanı oluşturmanın tam bir bölümü için [Bkz. Öğretici: Blob Depolama'dan SQL Veritabanı'na veri kopyalama.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

### <a name="sample-transformation-pipeline"></a>Örnek dönüştürme işlem hattı
Aşağıdaki örnek işlem hattında, **etkinlikler** bölümünde **HDInsightHive** türünde olan bir etkinlik vardır. Bu örnekte [HDInsight Hive etkinliği](data-factory-hive-activity.md), bir Azure HDInsight Hadoop kümesinde Hive betik dosyası çalıştırarak verileri bir Azure Blob depolamadan dönüştürür.

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Aşağıdaki noktalara dikkat edin:

* Etkinlikler bölümünde **türü****HDInsightHive** olarak ayarlanmış yalnızca bir etkinlik vardır.
* **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.
* **Tanımlar** bölümü, kovan komut dosyasına hive yapılandırma değerleri (örn. `${hiveconf:inputtable}` `${hiveconf:partitionedtable}`

Bir boru hattındadönüşüm faaliyetlerini tanımlayan JSON örnekleri için bu makalede [VERİ DÖNÜŞÜM FAALİYETLerİ](#data-transformation-activities) bölümüne bakın.

Bu ardışık yapıyı oluşturmanın tam bir bölümü için [Bkz. Öğretici: Hadoop kümesini kullanarak verileri işlemek için ilk ardışık lığınızı oluşturun.](data-factory-build-your-first-pipeline.md)

## <a name="linked-service"></a>Bağlı hizmet
Bağlı hizmet tanımı için üst düzey yapı aşağıdaki gibidir:

```json
{
    "name": "<name of the linked service>",
    "properties": {
        "type": "<type of the linked service>",
        "typeProperties": {
        }
    }
}
```

Aşağıdaki tabloda json tanımı ndaki özellikleri açıklayınız:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| ad | Bağlantılı hizmetin adı. | Evet |
| özellikleri - türü | Bağlantılı hizmettürü. Örneğin: Azure Depolama, Azure SQL Veritabanı. |
| typeProperties | typeProperties bölümünde her veri deposu veya bilgi işlem ortamı için farklı öğeler vardır. Tüm veri depolama bağlantılı hizmetler ve bilgi [işlem ortamları](#compute-environments) için veri depoları bölümüne bakın |

## <a name="dataset"></a>Veri kümesi
Azure Veri Fabrikası'ndaki bir veri kümesi aşağıdaki gibi tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
        "policy":
        {
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| ad | Veri kümesinin adı. Bkz. [Azure Veri Fabrikası - Adlandırma](data-factory-naming-rules.md) kuralları. |Evet |NA |
| type | Veri kümesinin türü. Azure Veri Fabrikası tarafından desteklenen türlerden birini belirtin (örneğin: AzureBlob, AzureSqlTable). Veri Fabrikası tarafından desteklenen tüm veri depoları ve veri kümesi türleri için [DATA STORES](#data-stores) bölümüne bakın. |
| yapı | Veri kümesinin şeması. Sütunlar, türleri vb. içerir. | Hayır |NA |
| typeProperties | Seçili türe karşılık gelen özellikler. Desteklenen türler ve özellikleri için [DATA STORES](#data-stores) bölümüne bakın. |Evet |NA |
| external | Bir veri kümesinin açıkça bir veri fabrikası ardışık hattı tarafından üretilip üretilmediğini belirtmek için Boolean bayrağı. |Hayır |yanlış |
| availability | Veri kümesi üretimi için işleme penceresini veya dilimleme modelini tanımlar. Veri kümesi dilimleme modeliyle ilgili ayrıntılar için [Zamanlama ve Yürütme](data-factory-scheduling-and-execution.md) makalesine bakın. |Evet |NA |
| ilke |Veri kümesi dilimlerinin yerine getirmesi gereken ölçütleri veya koşulu tanımlar. <br/><br/>Ayrıntılar için Dataset İlke bölümüne bakın. |Hayır |NA |

**Yapı** bölümündeki her sütun aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| kültür |.NET tabanlı kültür türü belirtildiğinde kullanılacak ve `Datetime` .NET türü veya `Datetimeoffset`. `en-us` varsayılan değerdir. |Hayır |
| biçim |Tür belirtildiğinde ve .NET türü `Datetime` veya `Datetimeoffset`. olduğunda kullanılacak biçim dizesi |Hayır |

Aşağıdaki örnekte, veri kümesinin `slicetimestamp`üç `projectname`sütunu vardır ve `pageviews` bunlar türdedir: String, String ve Ondalık sırasıyla.

```json
structure:
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Aşağıdaki tabloda **kullanılabilirlik** bölümünde kullanabileceğiniz özellikler açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilimi üretimi için zaman birimini belirtir.<br/><br/><b>Desteklenen frekans</b>: Dakika, Saat, Gün, Hafta, Ay |Evet |NA |
| interval |Frekans için çarpan belirtir<br/><br/>"Frekans x aralığı" dilimin ne sıklıkta üretildiğini belirler.<br/><br/>Veri kümesinin saatlik olarak dilimlemeye ihtiyacı varsa, <b>Sıklık</b> <b>saate</b>ve <b>1</b>aralık <b>1'e</b> ayarlarsınız.<br/><br/><b>Not</b>: Sıklığı Dakika olarak belirtirseniz, aralığı en az 15 olarak ayarlamanızı öneririz |Evet |NA |
|  stili |Dilimin aralığın başında/sonunda üretilip üretilmeyeceğini belirtir.<ul><li>Başlangıç Aralığı</li><li>EndofInterval</li></ul><br/><br/>Sıklık Ay olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim ayın son gününde üretilir. Stil StartOfInterval olarak ayarlanmışsa, dilim ayın ilk gününde üretilir.<br/><br/>Sıklık Gün olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim günün son saatinde üretilir.<br/><br/>Sıklık Saat olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim saatin sonunda üretilir. Örneğin, 13:00 - 14:00 dönemi için bir dilim için, dilim 14:00'te üretilir. |Hayır |EndofInterval |
| çapaDateTime |Veri kümesi dilimi sınırlarını hesaplamak için zamanlayıcı tarafından kullanılan zamandaki mutlak konumu tanımlar. <br/><br/><b>Not</b>: AnchorDateTime'da frekanstan daha parçalı tarih parçaları varsa, daha parçalı parçalar yoksayılır. <br/><br/>Örneğin, <b>aralık</b> <b>saatlikse</b> (sıklık: saat ve aralık: 1) ve <b>AnchorDateTime</b> <b>dakika ve saniye</b> içeriyorsa, AnchorDateTime'ın dakika ve <b>saniye</b> bölümleri yoksayılır. |Hayır |01/01/0001 |
| uzaklık |Tüm veri kümesi dilimlerinin başlangıç ve bitişinin kaydırıldığı zaman sonu. <br/><br/><b>Not</b>: Her iki anchorDateTime ve ofset belirtilirse, sonuç birleşik kaydırmadır. |Hayır |NA |

Aşağıdaki kullanılabilirlik bölümü, çıktı veri kümesinin saatlik (veya) giriş veri kümesinin saatlik olarak kullanılabildiğini belirtir:

```json
"availability":
{
    "frequency": "Hour",
    "interval": 1
}
```

Dataset tanımındaki **ilke** bölümü, veri kümesi dilimlerinin yerine getirmesi gereken ölçütleri veya koşulu tanımlar.

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |**Azure blob'undaki** verilerin minimum boyut gereksinimlerini (megabaytlarda) karşıladığını doğrular. |Azure Blob |Hayır |NA |
| minimum Satırlar |**Azure SQL veritabanındaki** veya **Azure tablosundaki** verilerin en az satır sayısını içerdiğini doğrular. |<ul><li>Azure SQL Veritabanı</li><li>Azure Tablosu</li></ul> |Hayır |NA |

**Örnek:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Azure Veri Fabrikası tarafından bir veri kümesi üretilmediği sürece, **dış .** Bu ayar, etkinlik veya boru hattı zincirleme kullanılmadığı sürece genellikle bir ardışık işlemdeki ilk etkinliğin girişleri için geçerlidir.

| Adı | Açıklama | Gerekli | Varsayılan Değer |
| --- | --- | --- | --- |
| veriGecikme |Verilen dilim için dış verilerin kullanılabilirliği üzerindeki denetimi geciktirme zamanı. Örneğin, veriler saatlik olarak kullanılabilirse, dış verileri görmek için denetim kullanılabilir ve ilgili dilim Hazır dataDelay kullanılarak gecikebilir.<br/><br/>Sadece şimdiki zaman için geçerlidir.  Örneğin, şu anda saat 13:00 ise ve bu değer 10 dakikaysa, doğrulama 13:10'da başlar.<br/><br/>Bu ayar geçmişteki dilimleri etkilemez (Dilim Bitiş Saati + dataDelay < Now olan dilimler) gecikmeden işlenir.<br/><br/>23:59 saatten büyük sürenin `day.hours:minutes:seconds` biçimi kullanılarak belirtilmiş olması gerekir. Örneğin, 24 saat belirtmek için 24:00:00'ı kullanmayın; bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanıyorsanız, 24 gün (24:00:00:00) olarak kabul edilir. 1 gün ve 4 saat için 1:04:00:00 belirtiniz. |Hayır |0 |
| retryInterval |Bir hata ve bir sonraki yeniden deneme girişimi arasındaki bekleme süresi. Deneme başarısız olursa, bir sonraki deneme retryInterval sonra. <br/><br/>Eğer saat 13:00 ise, ilk denemeye başlıyoruz. İlk doğrulama denetiminin tamamlanma süresi 1 dakika ysa ve işlem başarısız olursa, bir sonraki yeniden deneme 1:00 + 1 dk (süre) + 1 dk (yeniden deneme aralığı) = 1:02 PM'dir. <br/><br/>Geçmişteki dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 dakika) |
| yeniden denemeTimeout |Her yeniden deneme denemesi için zaman arası.<br/><br/>Bu özellik 10 dakika olarak ayarlanmışsa, doğrulamanın 10 dakika içinde tamamlanması gerekir. Doğrulamanın gerçekleşilmesi 10 dakikadan uzun süreyle sürüyorsa, yeniden deneme süreleri dışarısızdır.<br/><br/>Doğrulama için tüm denemeler zaman ları devre dışı kalırsa, dilim TimedOut olarak işaretlenir. |Hayır |00:10:00 (10 dakika) |
| maksimumRetry |Dış verilerin kullanılabilirliğini kontrol etmek için kaç kez. İzin verilen maksimum değer 10'dur. |Hayır |3 |


## <a name="data-stores"></a>VERİ MAĞAZALARI
[Bağlantılı hizmet](#linked-service) bölümü, tüm bağlantılı hizmetlerde ortak olan JSON öğeleri için açıklamalar sağladı. Bu bölümde, her veri deposuna özgü JSON öğeleri hakkında ayrıntılar verilmektedir.

[Dataset](#dataset) bölümü, tüm veri kümeleri türlerinde ortak olan JSON öğeleri için açıklamalar sağladı. Bu bölümde, her veri deposuna özgü JSON öğeleri hakkında ayrıntılar verilmektedir.

[Etkinlik](#activity) bölümünde, tüm etkinlik türlerinde ortak olan JSON öğeleri için açıklamalar sağlanmıştır. Bu bölümde, bir kopyalama etkinliğinde kaynak/lavabo olarak kullanıldığında her veri deposuna özgü JSON öğeleri hakkında ayrıntılı bilgi verilir.

Bağlantılı hizmet, veri kümesi ve kopyalama etkinliği için kaynak/lavabo için JSON şemalarını görmek için ilgilendiğiniz mağazanın bağlantısını tıklayın.

| Kategori | Veri deposu
|:--- |:--- |
| **Azure** |[Azure Blob depolama](#azure-blob-storage) |
| &nbsp; |Azure Data Lake Store |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Veritabanı](#azure-sql-database) |
| &nbsp; |[Azure SQL Veri Ambarı](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Bilişsel Arama](#azure-cognitive-search) |
| &nbsp; |[Azure Tablo depolama](#azure-table-storage) |
| **Veritaban -ları** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[Mysql](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **Dosya** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Dosya Sistemi](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Diğer** |[HTTP Kı.,](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |Web Tablosu |

## <a name="azure-blob-storage"></a>Azure Blob Depolama

### <a name="linked-service"></a>Bağlı hizmet
İki tür bağlantılı hizmet vardır: Azure Depolama bağlantılı hizmet ve Azure Depolama SAS bağlantılı hizmet.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
**Hesap anahtarını**kullanarak Azure depolama hesabınızı bir veri fabrikasına bağlamak için Azure Depolama bağlantılı bir hizmet oluşturun. Azure Depolama bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **AzureStorage**olarak ayarlayın. Ardından, **typeProperties** bölümünde aşağıdaki özellikleri belirtebilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Connectionstring |ConnectionString özelliği için Azure depolamasına bağlanmak için gereken bilgileri belirtin. |Evet |

##### <a name="example"></a>Örnek

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Depolama SAS Bağlantılı Hizmet
Azure Depolama SAS bağlantılı hizmeti, Paylaşılan Erişim İmzası (SAS) kullanarak bir Azure Depolama Hesabı'nı Azure veri fabrikasına bağlamanızı sağlar. Veri fabrikasına depolamadaki tüm/belirli kaynaklara (blob/konteyner) sınırlı/zamana bağlı erişim sağlar. Paylaşılan Erişim İmzası'nı kullanarak Azure depolama hesabınızı bir veri fabrikasına bağlamak için Bir Azure Depolama SAS bağlantılı hizmet oluşturun. Azure Depolama SAS bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **AzureStorageSas'a**ayarlayın. Ardından, **typeProperties** bölümünde aşağıdaki özellikleri belirtebilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Depolama kaynaklarına Paylaşılan Erişim İmza URI'sini belirtin. |Evet |

##### <a name="example"></a>Örnek

```json
{
    "name": "StorageSasLinkedService",
    "properties": {
        "type": "AzureStorageSas",
        "typeProperties": {
            "sasUri": "<storageUri>?<sasToken>"
        }
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için [Azure Blob Depolama bağlayıcısı](data-factory-azure-blob-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Azure Blob veri kümesini tanımlamak için veri kümesinin **türünü** **AzureBlob**olarak ayarlayın. Ardından, **typeProperties** bölümünde aşağıdaki Azure Blob belirli özelliklerini belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Blob depolama sındaki kapsayıcı ve klasöre giden yol. Örnek: myblobcontainer\myblobfolder\ |Evet |
| fileName |Lekenin adı. fileName isteğe bağlıdır ve büyük/küçük harf duyarlıdır.<br/><br/>Bir dosya adı belirtirseniz, etkinlik (Kopya dahil) belirli Blob'da çalışır.<br/><br/>fileName belirtilmediğinde, Copy giriş veri kümesi için folderPath'teki tüm Blob'ları içerir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı `Data.<Guid>.txt` aşağıdaki biçimde olacaktır: (örneğin: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| bölümlemeBy |partitionedBy isteğe bağlı bir özelliktir. Zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath her saatlik veri için parametreli olabilir. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
 ```


Daha fazla bilgi için [Azure Blob bağlayıcısı](data-factory-azure-blob-connector.md#dataset-properties) makalesine bakın.

### <a name="blobsource-in-copy-activity"></a>Kopyalama Etkinliğinde BlobSource
Azure Blob Depolama'dan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **BlobSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True (varsayılan değer), False |Hayır |

#### <a name="example-blobsource"></a>Örnek: **BlobSource**
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>Kopyalama Etkinliğinde BlobSink
Verileri bir Azure Blob Depolama alanına kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **BlobSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya FileSystem olduğunda kopyalama davranışını tanımlar. |<b>PreserveHiyerarşi</b>: hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/><b>FlattenHiyerarşiy</b>: kaynak klasöründen tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulan adı var. <br/><br/><b>Birleştirme Dosyaları (varsayılan):</b> kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya/Blob Adı belirtilirse, birleştirilen dosya adı belirtilen ad olur; aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

#### <a name="example-blobsink"></a>Örnek: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure Blob bağlayıcısı](data-factory-azure-blob-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Bağlı hizmet
Azure Veri Gölü Deposu'na bağlı bir hizmet tanımlamak için, bağlı hizmettürünü **AzureDataLakeStore'a**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **AzureDataLakeStore** | Evet |
| dataLakeStoreUri | Azure Veri Gölü Deposu hesabı yla ilgili bilgileri belirtin. Aşağıdaki biçimde: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store'un ait olduğu Azure abonelik kimliği. | Lavabo için gerekli |
| resourceGroupName | Data Lake Store'un ait olduğu azure kaynak grubu adı. | Lavabo için gerekli |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet (hizmet asıl kimlik doğrulaması için) |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. | Evet (hizmet asıl kimlik doğrulaması için) |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet (hizmet asıl kimlik doğrulaması için) |
| yetkilendirme | Veri **Authorize** Fabrikası Düzenleyicisi'ndeki Yetkilendirme düğmesini tıklatın ve bu özelliğe otomatik olarak oluşturulan yetkilendirme URL'sini atayan kimlik bilgilerinizi girin. **Data Factory Editor** | Evet (kullanıcı kimlik bilgisi kimlik doğrulaması için)|
| Sessionıd | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersizdir ve yalnızca bir kez kullanılabilir. Veri Fabrikası Düzenleyicisi'ni kullandığınızda bu ayar otomatik olarak oluşturulur. | Evet (kullanıcı kimlik bilgisi kimlik doğrulaması için) |

#### <a name="example-using-service-principal-authentication"></a>Örnek: hizmet temel kimlik doğrulamasını kullanma
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Örnek: kullanıcı kimlik bilgilerini kullanarak
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

Daha fazla bilgi için [Azure Veri Gölü Deposu bağlayıcısı](data-factory-azure-datalake-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir Azure Veri Gölü Deposu veri kümesi tanımlamak için, veri kümesinin **türünü** **AzureDataLakeStore**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| folderPath |Azure Veri Gölü deposundaki kapsayıcı ve klasöre giden yol. |Evet |
| fileName |Azure Veri Gölü deposundaki dosyanın adı. fileName isteğe bağlıdır ve büyük/küçük harf duyarlıdır. <br/><br/>Bir dosya adı belirtirseniz, etkinlik (Kopyadahil) belirli bir dosyaüzerinde çalışır.<br/><br/>fileName belirtilmediğinde, Copy giriş veri kümesi için folderPath'teki tüm dosyaları içerir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı `Data.<Guid>.txt` aşağıdaki biçimde olacaktır: (örneğin: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| bölümlemeBy |partitionedBy isteğe bağlı bir özelliktir. Zaman serisi verileri için dinamik bir klasörPath ve dosya adı belirtmek için kullanabilirsiniz. Örneğin, folderPath her saatlik veri için parametreli olabilir. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

#### <a name="example"></a>Örnek
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Azure Veri Gölü Deposu bağlayıcısı](data-factory-azure-datalake-connector.md#dataset-properties) makalesine bakın.

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Veri Gölü Deposu Kaynağı
Bir Azure Veri Gölü Deposu'ndan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **AzureDataLakeStoreSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

**AzureDataLakeStoreSource** aşağıdaki özellikleri **typeProperties** bölümünü destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True (varsayılan değer), False |Hayır |

#### <a name="example-azuredatalakestoresource"></a>Örnek: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure Veri Gölü Deposu bağlayıcısı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesine bakın.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Veri Gölü Deposu Bayı
Verileri bir Azure Veri Gölü Deposu'na kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **AzureDataLakeStoreSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kopyalama davranışını belirtir. |<b>PreserveHiyerarşi</b>: hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/><b>FlattenHiyerarşiy</b>: kaynak klasöründen tüm dosyalar hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar otomatik oluşturulan adla oluşturulur.<br/><br/><b>MergeFiles</b>: kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya/Blob Adı belirtilirse, birleştirilen dosya adı belirtilen ad olur; aksi takdirde, otomatik olarak oluşturulan dosya adı olacaktır. |Hayır |

#### <a name="example-azuredatalakestoresink"></a>Örnek: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure Veri Gölü Deposu bağlayıcısı](data-factory-azure-datalake-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="linked-service"></a>Bağlı hizmet
Azure Cosmos DB bağlantılı bir hizmet tanımlamak için **DocumentDb'ye**bağlı hizmet **türünü** ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| Connectionstring |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
Daha fazla bilgi için [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Azure Cosmos DB veri kümesini tanımlamak için, veri kümesinin **türünü** **DocumentDbCollection**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| Toplamaadı |Azure Cosmos DB koleksiyonunun adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
Daha fazla bilgi için [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#dataset-properties) makalesine bakın.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Cosmos DB Toplama Kaynağı
Bir Azure Cosmos DB'den veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **DocumentDbCollectionSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için sorguyu belirtin. |Azure Cosmos DB tarafından desteklenen sorgu dizesi. <br/><br/>Örnek: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, yürütülen SQL deyimi:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Belgenin iç içe olduğunu gösteren özel karakter |Herhangi bir karakter. <br/><br/>Azure Cosmos DB, Iç içe yapıların izin verildiği JSON belgeleri için bir NoSQL deposudur. Azure Veri Fabrikası, kullanıcının nestingSeparator aracılığıyla hiyerarşiyi belirtmesini sağlar, yani "." yukarıdaki örneklerde. Ayırıcı ile kopyalama etkinliği, tablo tanımındaki "Name.First", "Name.Middle" ve "Name.Last" ad,orta ve soyad öğelerinden "Ad" nesnesini oluşturur. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Cosmos DB Koleksiyonu
Verileri Azure Cosmos DB'ye kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **DocumentDbCollectionSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| nestingSeparator |İç içe belge nin gerekli olduğunu belirtmek için kaynak sütun adında özel bir karakter. <br/><br/>Yukarıdaki örnekte: `Name.First` çıktı tablosunda Cosmos DB belgesinde aşağıdaki JSON yapısını oluşturur:<br/><br/>"Adı": {<br/>    "İlk": "John"<br/>}, |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir.<br/><br/>Varsayılan değer `.` (nokta). |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. <br/><br/>Varsayılan değer `.` (nokta). |
| yazmaBatchSize |Belge oluşturmak için Azure Cosmos DB hizmetine paralel istek sayısı.<br/><br/>Azure Cosmos DB'ye/Azure Cosmos DB'ye veri kopyalarken bu özelliği kullanarak performansta ince ayar yapabilirsiniz. Azure Cosmos DB'ye daha fazla paralel istek gönderildiğinden, writeBatchSize'ı artırdığınızda daha iyi bir performans bekleyebilirsiniz. Ancak hata iletisi atabilir azaltma önlemek gerekir: "İstek oranı büyüktür".<br/><br/>Azaltma, belgelerin boyutu, belgelerdeki terimlerin sayısı, hedef toplama nın dizine eklenmesi vb. gibi bir dizi faktöre göre belirlenir. Kopyalama işlemleri için, en fazla iş elde etmek için daha iyi bir koleksiyon (örneğin, S3) kullanabilirsiniz (2.500 istek birimi/saniye). |Tamsayı |Hayır (varsayılan: 5) |
| yazmaBatchTimeout |Zaman geçmeden işlemin tamamlanması için zaman bekleyin. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

Daha fazla bilgi için [Azure Cosmos DB bağlayıcı](data-factory-azure-documentdb-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-sql-database"></a>Azure SQL Veritabanı

### <a name="linked-service"></a>Bağlı hizmet
Azure SQL Veritabanı'na bağlı bir hizmet tanımlamak için, bağlı **hizmettürünü** **AzureSqlDatabase'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Connectionstring |ConnectionString özelliği için Azure SQL Veritabanı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Daha fazla bilgi için [Azure SQL bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Azure SQL Veritabanı veri kümesini tanımlamak için, veri kümesinin **türünü** **AzureSqlTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure SQL Veritabanı örneğinde tablonun veya görünümün adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için [Azure SQL bağlayıcısı](data-factory-azure-sql-connector.md#dataset-properties) makalesine bakın.

### <a name="sql-source-in-copy-activity"></a>Kopyalama Etkinliğinde SQL Kaynağı
Azure SQL Veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **SqlSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için [Azure SQL bağlayıcısı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesine bakın.

### <a name="sql-sink-in-copy-activity"></a>Kopyalama Etkinliğinde SQL Lavabo
Verileri Azure SQL Veritabanı'na kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **SqlSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi ni gerçekleştirmek için Kopyalama Etkinliği için bir sorgu belirtin. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılan otomatik oluşturulan dilim tanımlayıcısıyla doldurmak için Kopyalama Etkinliği için bir sütun adı belirtin. |Veri türü ikili olan bir sütunun sütun adı(32). |Hayır |
| sqlWriterStoredProcedureNameName |Verileri hedef tabloya ekleyen (güncelleştirmeler/ekler) depolanan yordamın adı. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Tablo türü adı. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure SQL bağlayıcısı](data-factory-azure-sql-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

### <a name="linked-service"></a>Bağlı hizmet
Azure SQL Veri Ambarı bağlantılı bir hizmet tanımlamak için bağlı **hizmettürünü** **AzureSqlDW'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Connectionstring |ConnectionString özelliği için Azure SQL Veri Ambarı örneğine bağlanmak için gereken bilgileri belirtin. |Evet |



#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Daha fazla bilgi için [Azure SQL Veri Ambarı bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir Azure SQL Veri Ambarı veri kümesi tanımlamak için, veri kümesinin **türünü** **AzureSqlDWTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure SQL Veri Ambarı veritabanında tablonun veya görünümün adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Azure SQL Veri Ambarı bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) makalesine bakın.

### <a name="sql-dw-source-in-copy-activity"></a>Kopyalama Etkinliğinde SQL DW Kaynağı
Azure SQL Veri Ambarı'ndan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **SqlDWSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure SQL Veri Ambarı bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesine bakın.

### <a name="sql-dw-sink-in-copy-activity"></a>Kopyalama Etkinliğinde SQL DW Lavabo
Verileri Azure SQL Veri Ambarı'na kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **SqlDWSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi ni gerçekleştirmek için Kopyalama Etkinliği için bir sorgu belirtin. |Sorgu deyimi. |Hayır |
| Izin PolyBase |BULKINSERT mekanizması yerine PolyBase'in (varsa) kullanılıp kullanılmayacağını gösterir. <br/><br/> **PolyBase'i kullanmak, verileri SQL Veri Ambarı'na yüklemenin önerilen yoludur.** |True <br/>False (varsayılan) |Hayır |
| polyBaseSettings |**İzin veren Polybase** özelliği **doğru**ayarlandığında belirtilebilen özellikler grubu. |&nbsp; |Hayır |
| rejectValue |Sorgu başarısız olmadan önce reddedilebilen satırların sayısını veya yüzdesini belirtir. <br/><br/>[CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) konusunun **Bağımsız Değişkenler** bölümünde PolyBase'in reddetme seçenekleri hakkında daha fazla bilgi edinin. |0 (varsayılan), 1, 2, ... |Hayır |
| rejectType |RejectValue seçeneğinin gerçek bir değer mi yoksa yüzde olarak mı belirtildiğini belirtir. |Değer (varsayılan), Yüzde |Hayır |
| rejectSampleValue |PolyBase reddedilen satırların yüzdesini yeniden hesaplamadan önce alınacak satır sayısını belirler. |1, 2, ... |Evet, **rejectType** **yüzde** ise |
| useTypeDefault |PolyBase metin dosyasından veri aldığında sınırlı metin dosyalarındaki eksik değerlerin nasıl işleyeceğini belirtir.<br/><br/>[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)'deki Bağımsız Değişkenler bölümünden bu özellik hakkında daha fazla bilgi edinin. |True, False (varsayılan) |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize ulaştığında SQL tablosuna veri ekler |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure SQL Veri Ambarı bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

### <a name="linked-service"></a>Bağlı hizmet
Azure Bilişsel Arama bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **Azure Arama'ya**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| url | Arama hizmetinin URL'si. | Evet |
| anahtar | Arama hizmeti için yönetici anahtarı. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

Daha fazla bilgi için [Azure Bilişsel Arama bağlayıcısı](data-factory-azure-search-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Azure Bilişsel Arama veri kümesini tanımlamak için, veri kümesinin **türünü** **AzureSearchIndex**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Tür özelliği **AzureSearchIndex**olarak ayarlanmalıdır.| Evet |
| indexName | Arama dizininin adı. Veri Fabrikası dizin oluşturmaz. Dizin Azure Bilişsel Arama'da bulunmalıdır. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

Daha fazla bilgi için [Azure Bilişsel Arama bağlayıcısı](data-factory-azure-search-connector.md#dataset-properties) makalesine bakın.

### <a name="azure-cognitive-search-index-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Bilişsel Arama Dizini Batması
Verileri bir arama dizinine kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **AzureSearchIndexSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| Yazma Davranışı | Diziniçinde zaten bir belge varken birleşip değiştirilmeyeceğini belirtir. | Birleştirme (varsayılan)<br/>Karşıya Yükle| Hayır |
| WriteBatchSize | Arabellek boyutu writeBatchSize'a ulaştığında verileri arama dizinine yükler. | 1'den 1000'e kadar. Varsayılan değer 1000'dir. | Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Azure Bilişsel Arama bağlayıcısı](data-factory-azure-search-connector.md#copy-activity-properties) makalesine bakın.

## <a name="azure-table-storage"></a>Azure Table Storage

### <a name="linked-service"></a>Bağlı hizmet
İki tür bağlantılı hizmet vardır: Azure Depolama bağlantılı hizmet ve Azure Depolama SAS bağlantılı hizmet.

#### <a name="azure-storage-linked-service"></a>Azure Storage Bağlı Hizmeti
**Hesap anahtarını**kullanarak Azure depolama hesabınızı bir veri fabrikasına bağlamak için Azure Depolama bağlantılı bir hizmet oluşturun. Azure Depolama bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **AzureStorage**olarak ayarlayın. Ardından, **typeProperties** bölümünde aşağıdaki özellikleri belirtebilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureStorage** |Evet |
| Connectionstring |ConnectionString özelliği için Azure depolamasına bağlanmak için gereken bilgileri belirtin. |Evet |

**Örnek:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Azure Depolama SAS Bağlantılı Hizmet
Azure Depolama SAS bağlantılı hizmeti, Paylaşılan Erişim İmzası (SAS) kullanarak bir Azure Depolama Hesabı'nı Azure veri fabrikasına bağlamanızı sağlar. Veri fabrikasına depolamadaki tüm/belirli kaynaklara (blob/konteyner) sınırlı/zamana bağlı erişim sağlar. Paylaşılan Erişim İmzası'nı kullanarak Azure depolama hesabınızı bir veri fabrikasına bağlamak için Bir Azure Depolama SAS bağlantılı hizmet oluşturun. Azure Depolama SAS bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **AzureStorageSas'a**ayarlayın. Ardından, **typeProperties** bölümünde aşağıdaki özellikleri belirtebilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureStorageSas** |Evet |
| sasUri |Blob, kapsayıcı veya tablo gibi Azure Depolama kaynaklarına Paylaşılan Erişim İmza URI'sini belirtin. |Evet |

**Örnek:**

```json
{
    "name": "StorageSasLinkedService",
    "properties": {
        "type": "AzureStorageSas",
        "typeProperties": {
            "sasUri": "<storageUri>?<sasToken>"
        }
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için [Azure Tablo Depolama bağlayıcısı](data-factory-azure-table-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Azure Tablo veri kümesini tanımlamak için, veri kümesinin **türünü** **AzureTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure Tablo Veritabanı örneğindeki tablonun adı. |Evet. Bir tableName azureTableSourceQuery olmadan belirtildiğinde, tablodaki tüm kayıtlar hedefe kopyalanır. AzureTableSourceQuery de belirtilirse, sorguyu karşılayan tablodaki kayıtlar hedefe kopyalanır. |

#### <a name="example"></a>Örnek

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için [Azure Tablo Depolama bağlayıcısı](data-factory-azure-table-connector.md#dataset-properties) makalesine bakın.

### <a name="azure-table-source-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Tablo Kaynağı
Azure Tablo Depolama'dan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **AzureTableSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorguyu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName azureTableSourceQuery olmadan belirtildiğinde, tablodaki tüm kayıtlar hedefe kopyalanır. AzureTableSourceQuery de belirtilirse, sorguyu karşılayan tablodaki kayıtlar hedefe kopyalanır. |
| azureTableSourceIgnoreTableNotFound |Tablonun özel durum yutmak olup olmadığını belirtin. |TRUE<br/>FALSE |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Bu bağlantılı hizmetler hakkında daha fazla bilgi için [Azure Tablo Depolama bağlayıcısı](data-factory-azure-table-connector.md#copy-activity-properties) makalesine bakın.

### <a name="azure-table-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Azure Tablo Batması
Verileri Azure Tablo Depolama'ya kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **AzureTableSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Lavabo tarafından kullanılabilecek varsayılan bölüm anahtar değeri. |Dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütunun adını belirtin. Belirtilmemişse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableTableKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütunun adını belirtin. Belirtilmemişse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tablosuna veri ekleme modu.<br/><br/>Bu özellik, eşleşen bölüm ve satır anahtarları ile çıktı tablosunda varolan satırların değerlerinin değiştirilip değiştirilmediğini veya birleştirilip birleştirilmediğini denetler. <br/><br/>Bu ayarların (birleştirme ve değiştirme) nasıl çalıştığı hakkında bilgi edinmek için Varlık [Ekle veya Birleştir](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve Varlık Ekle veya [Değiştir](https://msdn.microsoft.com/library/azure/hh452242.aspx) konularına bakın. <br/><br> Bu ayar, tablo düzeyinde değil, satır düzeyinde geçerlidir ve her iki seçenek de girişte bulunmayan çıktı tablosundaki satırları siler. |birleştirme (varsayılan)<br/>Değiştirmek |Hayır |
| yazmaBatchSize |WriteBatchSize veya writeBatchTimeout vurulduğunda Azure tablosuna veri ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| yazmaBatchTimeout |WriteBatchSize veya writeBatchTimeout vurulduğunda Verileri Azure tablosuna ekler |Timespan<br/><br/>Örnek: "00:20:00" (20 dakika) |Hayır (Varsayılan depolama istemcisi varsayılan zaman kaybı değeri 90 sn) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Bu bağlantılı hizmetler hakkında daha fazla bilgi için [Azure Tablo Depolama bağlayıcısı](data-factory-azure-table-connector.md#copy-activity-properties) makalesine bakın.

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Bağlı hizmet
Bir Amazon Redshift bağlantılı hizmeti tanımlamak için, **AmazonRedshift'e**bağlı **hizmettürünü** ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Amazon Redshift sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| port |Amazon Redshift sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktasının sayısı. |Hayır, varsayılan değer: 5439 |
| database |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcının adı. |Evet |
| password |Kullanıcı hesabının şifresi. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

Daha fazla bilgi için Amazon Redshift konektör makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Amazon Redshift veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmet amazon Redshift veritabanında tablonun adı anlamına gelir. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |


#### <a name="example"></a>Örnek

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için Amazon Redshift konektör makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Amazon Redshift'ten veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Daha fazla bilgi için Amazon Redshift konektör makalesine bakın.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Bağlı hizmet
IBM DB2 bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **OnPremisesDB2**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |DB2 sunucusunun adı. |Evet |
| database |DB2 veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |DB2 veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi DB2 veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Daha fazla bilgi için IBM DB2 bağlayıcı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
DB2 veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin başvurulması gereken DB2 Veritabanı örneğindeki tablonun adı. Tablo Adı büyük/küçük harf duyarlıdır. |Hayır **(RelationalSource** **sorgusu** belirtilirse)

#### <a name="example"></a>Örnek
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için IBM DB2 bağlayıcı makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
IBM DB2'den veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from "MySchema"."MyTable""`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Daha fazla bilgi için IBM DB2 bağlayıcı makalesine bakın.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Bağlı hizmet
MySQL bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **OnPremisesMySql'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |MySQL sunucusunun adı. |Evet |
| database |MySQL veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. |Hayır |
| authenticationType |MySQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler `Basic`şunlardır: . |Evet |
| userName |MySQL veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password |Belirttiğiniz kullanıcı hesabıiçin parola belirtin. |Evet |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi MySQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

Daha fazla bilgi için [MySQL bağlayıcısı](data-factory-onprem-mysql-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
MySQL veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |MySQL Veritabanı örneğinde bağlı hizmetin başvurulması gereken tablonun adı. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için [MySQL bağlayıcısı](data-factory-onprem-mysql-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
MySQL veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |


#### <a name="example"></a>Örnek
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için [MySQL bağlayıcısı](data-factory-onprem-mysql-connector.md#copy-activity-properties) makalesine bakın.

## <a name="oracle"></a>Oracle

### <a name="linked-service"></a>Bağlı hizmet
Oracle'a bağlı bir hizmet tanımlamak için, bağlı **hizmettürünü** **OnPremisesOracle'a**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| driverType | Oracle Veritabanından/Oracle Veritabanına veri kopyalamak için hangi sürücünün kullanılacağını belirtin. İzin verilen değerler **Microsoft** veya **ODP** 'dir (varsayılan). Sürücü ayrıntılarıyla ilgili Desteklenen sürüm ve yükleme bölümüne bakın. | Hayır |
| Connectionstring | ConnectionString özelliği için Oracle Veritabanı örneğine bağlanmak için gereken bilgileri belirtin. | Evet |
| ağ geçidiAdı | Şirket içi Oracle sunucusuna bağlanmak için kullanılan ağ geçidinin adı |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Oracle veri kümesi tanımlamak için, veri kümesinin **türünü** **OracleTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Oracle Veritabanı'ndaki tablonun adı. |Hayır **(OracleSource** **oracleReaderQuery** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#dataset-properties) makalesine bakın.

### <a name="oracle-source-in-copy-activity"></a>Kopyalama Etkinliğinde Oracle Kaynağı
Bir Oracle veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **OracleSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| oracleReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin, `select * from MyTable` <br/><br/>Belirtilmemişse, yürütülen SQL deyimi:`select * from MyTable` |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesine bakın.

### <a name="oracle-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Oracle Lavabo
Verileri Oracle veritabanına kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **OracleSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: 00:30:00 (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 100) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi ni gerçekleştirmek için Kopyalama Etkinliği için bir sorgu belirtin. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılan otomatik oluşturulan dilim tanımlayıcısıyla doldurmak için Kopyalama Etkinliği için sütun adını belirtin. |Veri türü ikili olan bir sütunun sütun adı(32). |Hayır |

#### <a name="example"></a>Örnek
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için [Oracle bağlayıcı](data-factory-onprem-oracle-connector.md#copy-activity-properties) makalesine bakın.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Bağlı hizmet
PostgreSQL bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **OnPremisesPostgreSql'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |PostgreSQL sunucusunun adı. |Evet |
| database |PostgreSQL veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Daha fazla bilgi için [PostgreSQL bağlayıcısı](data-factory-onprem-postgresql-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
PostgreSQL veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Table**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin başvurulması olan PostgreSQL Veritabanı örneğindeki tablonun adı. Tablo Adı büyük/küçük harf duyarlıdır. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

#### <a name="example"></a>Örnek
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
Daha fazla bilgi için [PostgreSQL bağlayıcısı](data-factory-onprem-postgresql-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Bir PostgreSQL veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: "sorgu": \""MySchema'dan\"* seçin. \"MyTable\"". |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için [PostgreSQL bağlayıcısı](data-factory-onprem-postgresql-connector.md#copy-activity-properties) makalesine bakın.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Bağlı hizmet
SAP İş Ambarı (BW) bağlantılı bir hizmet tanımlamak için bağlı hizmet **türünü** **SapBw'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP BW örneğinin bulunduğu sunucunun adı. | string | Evet
systemNumber | SAP BW sisteminin sistem numarası. | Dize olarak temsil edilen iki basamaklı ondalık sayı. | Evet
clientId | SAP W sistemindeki istemci kimliği. | Dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcının adı | string | Evet
password | Kullanıcının parolası. | string | Evet
ağ geçidiAdı | Veri Fabrikası hizmetinin şirket içi SAP BW örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
şifreli Credential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

#### <a name="example"></a>Örnek

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için [SAP İş Ambarı bağlayıcısı](data-factory-sap-business-warehouse-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
SAP BW veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın. **İlişkisel Tablo**türü SAP BW veri kümesi için desteklenen türe özgü özellikler yoktur.

#### <a name="example"></a>Örnek

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için [SAP İş Ambarı bağlayıcısı](data-factory-sap-business-warehouse-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
SAP İş Ambarı'ndan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP BW örneğindeki verileri okumak için MDX sorgusunu belirtir. | MDX sorgusu. | Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Daha fazla bilgi için [SAP İş Ambarı bağlayıcısı](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) makalesine bakın.

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Bağlı hizmet
SAP HANA bağlantılı bir hizmet tanımlamak için, **sapHana'ya**bağlı hizmet **türünü** ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | --------
sunucu | SAP HANA örneğinin bulunduğu sunucunun adı. Sunucunuz özelleştirilmiş bir bağlantı noktası `server:port`kullanıyorsa, belirtin. | string | Evet
authenticationType | Kimlik doğrulama türü. | kullanabilirsiniz. "Temel" veya "Windows" | Evet
kullanıcı adı | SAP sunucusuna erişimi olan kullanıcının adı | string | Evet
password | Kullanıcının parolası. | string | Evet
ağ geçidiAdı | Veri Fabrikası hizmetinin şirket içi SAP HANA örneğine bağlanmak için kullanması gereken ağ geçidinin adı. | string | Evet
şifreli Credential | Şifrelenmiş kimlik bilgisi dizesi. | string | Hayır

#### <a name="example"></a>Örnek

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
Daha fazla bilgi için [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
SAP HANA veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın. **İlişkisel Tablo**türü SAP HANA veri kümesi için desteklenen türe özgü özellikler yoktur.

#### <a name="example"></a>Örnek

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Daha fazla bilgi için [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
SAP HANA veri deposundan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu | SAP HANA örneğindeki verileri okumak için SQL sorgusunu belirtir. | SQL sorgusu. | Evet |


#### <a name="example"></a>Örnek


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Daha fazla bilgi için [SAP HANA bağlayıcı](data-factory-sap-hana-connector.md#copy-activity-properties) makalesine bakın.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Bağlı hizmet
Şirket içi BIR SQL Server veritabanını bir veri fabrikasına bağlamak için **OnPremisesSqlServer** türünde bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tablo, şirket içi SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

Aşağıdaki tablo, SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| Connectionstring |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. |Evet |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| kullanıcı adı |Windows Kimlik Doğrulama'yı kullanıyorsanız kullanıcı adını belirtin. Örnek: **alan\\adı kullanıcı adı**. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |

Kimlik bilgilerini **New-AzDataFactoryEncryptValue** cmdlet kullanarak şifreleyebilir ve aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde kullanabilirsiniz **(EncryptedCredential** özelliği):

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Örnek: SQL Kimlik Doğrulaması kullanmak için JSON

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: Windows Kimlik Doğrulama'yı kullanmak için JSON

Kullanıcı adı ve parola belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabını taklit etmek için kullanır. Aksi takdirde, ağ geçidi doğrudan Ağ Geçidi'nin (başlangıç hesabı) güvenlik bağlamıyla SQL Server'a bağlanır.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için [SQL Server bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir SQL Server veri kümesi tanımlamak için, veri kümesinin **türünü** **SqlServerTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu SQL Server Veritabanı örneğinde tablo nun veya görünümün adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [SQL Server bağlayıcısı](data-factory-sqlserver-connector.md#dataset-properties) makalesine bakın.

### <a name="sql-source-in-copy-activity"></a>Kopyalama Etkinliğinde Sql Kaynağı
Bir SQL Server veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **SqlSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. Giriş veri kümesi tarafından başvurulan veritabanından birden çok tabloya başvuru yapabilir. Belirtilmemişse, yürütülen SQL deyimi: MyTable'dan seçin. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

SqlSource için **sqlReaderQuery** belirtilirse, Kopyalama Etkinliği verileri almak için bu sorguyu SQL Server Veritabanı kaynağına göre çalıştırır.

Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, yapı bölümünde tanımlanan sütunlar SQL Server Veritabanı'na karşı çalıştırmak için select sorgusu oluşturmak için kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

> [!NOTE]
> **sqlReaderStoredProcedureName**kullandığınızda, yine de veri kümesi JSON **tabloAdı** özelliği için bir değer belirtmeniz gerekir. Ancak bu tabloya karşı hiçbir doğrulama yoktur.


#### <a name="example"></a>Örnek
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Bu örnekte, **sqlReaderQuery** sqlSource için belirtilir. Kopyalama Etkinliği, verileri almak için bu sorguyu SQL Server Veritabanı kaynağına karşı çalıştırAr. Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz. sqlReaderQuery giriş veri kümesi tarafından başvurulan veritabanı içinde birden fazla tablo başvuru yapabilirsiniz. Yalnızca veri kümesinin tablo Adı typeProperty olarak ayarlanan tabloyla sınırlı değildir.

sqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, yapı bölümünde tanımlanan sütunlar SQL Server Veritabanı'na karşı çalıştırmak için select sorgusu oluşturmak için kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

Daha fazla bilgi için [SQL Server bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesine bakın.

### <a name="sql-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Sql Lavabo
Verileri bir SQL Server veritabanına kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **SqlSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesini sağlamak için Kopyalama Etkinliği sorgusubelirtin. Daha fazla bilgi için tekrarlanabilirlik bölümüne bakın. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılan otomatik oluşturulan dilim tanımlayıcısıyla doldurmak için Kopyalama Etkinliği için sütun adını belirtin. Daha fazla bilgi için tekrarlanabilirlik bölümüne bakın. |Veri türü ikili olan bir sütunun sütun adı(32). |Hayır |
| sqlWriterStoredProcedureNameName |Verileri hedef tabloya ekleyen (güncelleştirmeler/ekler) depolanan yordamın adı. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak tablo türü adını belirtin. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Tablo türü adı. |Hayır |

#### <a name="example"></a>Örnek
Ardışık iş, bu giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **SqlSink**olarak ayarlanır.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [SQL Server bağlayıcısı](data-factory-sqlserver-connector.md#copy-activity-properties) makalesine bakın.

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Bağlı hizmet
Sybase bağlantılı bir hizmet tanımlamak için, Bağlı hizmet **türünü** **OnPremisesSybase'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Sybase sunucusunun adı. |Evet |
| database |Sybase veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. |Hayır |
| authenticationType |Sybase veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi Sybase veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Daha fazla bilgi için Bkz. [Sybase bağlayıcısı](data-factory-onprem-sybase-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Sybase veri kümesitanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin başlettiği Sybase Veritabanı örneğindeki tablonun adı. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için Bkz. [Sybase bağlayıcısı](data-factory-onprem-sybase-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Bir Sybase veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için Bkz. [Sybase bağlayıcısı](data-factory-onprem-sybase-connector.md#copy-activity-properties) makalesine bakın.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Bağlı hizmet
Teradata bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **OnPremisesTeradata**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |Teradata sunucusunun adı. |Evet |
| authenticationType |Teradata veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi Teradata veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Daha fazla bilgi için [Teradata bağlayıcı makalesine](data-factory-onprem-teradata-connector.md#linked-service-properties) bakın.

### <a name="dataset"></a>Veri kümesi
Teradata Blob veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın. Şu anda, Teradata veri kümesi için desteklenen tür özellikleri yoktur.

#### <a name="example"></a>Örnek
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Teradata bağlayıcı makalesine](data-factory-onprem-teradata-connector.md#dataset-properties) bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Teradata veritabanından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi için [Teradata bağlayıcı makalesine](data-factory-onprem-teradata-connector.md#copy-activity-properties) bakın.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Bağlı hizmet
Cassandra bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **OnPremisesCassandra'ya**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| konak |Cassandra sunucularının bir veya daha fazla IP adresi veya ana bilgisayar adları.<br/><br/>Tüm sunuculara aynı anda bağlanmak için virgülle ayrılmış IP adresleri veya ana bilgisayar adlarının listesini belirtin. |Evet |
| port |Cassandra sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |Hayır, varsayılan değer: 9042 |
| authenticationType |Temel veya Anonim |Evet |
| kullanıcı adı |Kullanıcı hesabı için kullanıcı adını belirtin. |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| password |Kullanıcı hesabı için parola belirtin. |Evet, kimlik doğrulamaTürü Temel olarak ayarlanmışsa. |
| ağ geçidiAdı |Şirket içi Cassandra veritabanına bağlanmak için kullanılan ağ geçidinin adı. |Evet |
| şifreli Credential |Kimlik bilgileri ağ geçidi tarafından şifrelenir. |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Cassandra veri kümesi tanımlamak için, veri kümesinin **türünü** **CassandraTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| anahtar boşluğu |Cassandra veritabanındaki boşluk veya şema adı. |Evet **(CassandraSource** **sorgusu** tanımlanmamışsa). |
| tableName |Cassandra veritabanındaki tablonun adı. |Evet **(CassandraSource** **sorgusu** tanımlanmamışsa). |

#### <a name="example"></a>Örnek

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#dataset-properties) makalesine bakın.

### <a name="cassandra-source-in-copy-activity"></a>Kopyalama Etkinliğinde Cassandra Kaynak
Cassandra'dan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **CassandraSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL-92 sorgusu veya CQL sorgusu. [Bkz. CQL başvurusu.](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html) <br/><br/>SQL sorgusunu kullanırken, sorgulamak istediğiniz tabloyu temsil etmek için **keyspace name.table adını** belirtin. |Hayır (dataset'teki tablo Adı ve anahtar alanı tanımlanırsa). |
| tutarlılıkSeviye |Tutarlılık düzeyi, istemci uygulamasına veri döndürmeden önce okuma isteğine kaç yinelemenin yanıt vermesi gerektiğini belirtir. Cassandra, okundu isteğini karşılamak için veri için belirtilen çoğaltma sayısını denetler. |Bir, IKI, ÜÇ, ÇOĞUNLUK, HEPSI, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Ayrıntılar için [veri tutarlılığını yapılandırma](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) ya da yapılandırma ya bak. |Hayır. Varsayılan değer ONE'dır. |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Cassandra bağlayıcı](data-factory-onprem-cassandra-connector.md#copy-activity-properties) makalesine bakın.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Bağlı hizmet
MongoDB bağlantılı bir hizmeti tanımlamak için, Bağlı hizmet **türünü** **OnPremisesMongoDB'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| sunucu |MongoDB sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| port |MongoDB sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |İsteğe bağlı, varsayılan değer: 27017 |
| authenticationType |Temel veya Anonim. |Evet |
| kullanıcı adı |MongoDB'ye erişmek için kullanıcı hesabı. |Evet (temel kimlik doğrulama kullanılıyorsa). |
| password |Kullanıcının parolası. |Evet (temel kimlik doğrulama kullanılıyorsa). |
| authSource |Kimlik doğrulama için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |İsteğe bağlı (temel kimlik doğrulama kullanılıyorsa). varsayılan: yönetici hesabını ve databaseName özelliğini kullanarak belirtilen veritabanını kullanır. |
| Databasename |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| ağ geçidiAdı |Veri deposuna erişen ağ geçidinin adı. |Evet |
| şifreli Credential |Kimlik bilgileri ağ geçidi tarafından şifrelenir. |İsteğe bağlı |

#### <a name="example"></a>Örnek

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için Bkz. [MongoDB bağlayıcı sı](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Veri kümesi
Bir MongoDB veri kümesi tanımlamak için, veri kümesinin **türünü** **MongoDbCollection**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Toplamaadı |MongoDB veritabanındaki koleksiyonun adı. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

Daha fazla bilgi için Bkz. [MongoDB bağlayıcı sı](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>Kopyalama Etkinliğinde MongoDB Kaynağı
MongoDB'den veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **MongoDbSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL-92 sorgu dizesi. Örneğin: `select * from MyTable`. |Hayır **(koleksiyonVeri** **kümesinin** adı belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için Bkz. [MongoDB bağlayıcı sı](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Bağlı hizmet
Bir Amazon S3 bağlantılı hizmeti tanımlamak için, bağlı **hizmettürünü** **AwsAccessKey'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| accessKeyID |Gizli erişim anahtarının kimliği. |string |Evet |
| secretAccessKey |Gizli erişim anahtarının kendisi. |Şifreli gizli dize |Evet |

#### <a name="example"></a>Örnek
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

Daha fazla bilgi için [Amazon S3 konektör makalesine](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties)bakın.

### <a name="dataset"></a>Veri kümesi
Bir Amazon S3 veri kümesi tanımlamak için, veri kümesinin **türünü** **AmazonS3**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| bucketName |S3 kova adı. |Dize |Evet |
| anahtar |S3 nesne anahtarı. |Dize |Hayır |
| Önek |S3 nesne anahtarı için önek. Anahtarları bu önek ile başlayan nesneler seçilir. Yalnızca anahtar boşolduğunda geçerlidir. |Dize |Hayır |
| version |S3 sürümü etkinse S3 nesnesinin sürümü. |Dize |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır | |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeleri şunlardır: **Optimal** ve **En Hızlı**. Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır | |


> [!NOTE]
> bucketName + tuşu, kovanın S3 nesneleri için kök kapsayıcı olduğu ve anahtarın S3 nesnesine tam yol olduğu S3 nesnesinin konumunu belirtir.

#### <a name="example-sample-dataset-with-prefix"></a>Örnek: Önek ile örnek veri kümesi

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Örnek: Örnek veri seti (sürümile)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Örnek: S3 için dinamik yollar
Örnekte, Amazon S3 veri setinde anahtar ve bucketName özellikleri için sabit değerler kullanıyoruz.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

SliceStart gibi sistem değişkenlerini kullanarak Veri Fabrikası'nın çalışma zamanında anahtarı ve bucketName'yi dinamik olarak hesaplamasını sağlayabilirsiniz.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Aynı şeyi bir Amazon S3 veri kümesinin önek özelliği için de yapabilirsiniz. Desteklenen işlevler ve değişkenler listesi için [Veri Fabrikası işlevlerine ve sistem değişkenlerine](data-factory-functions-variables.md) bakın.

Daha fazla bilgi için [Amazon S3 konektör makalesine](data-factory-amazon-simple-storage-service-connector.md#dataset-properties)bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Kaynağı
Amazon S3'ten veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **FileSystemSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:


| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Dizin altında S3 nesnelerini özyinelemeli olarak listeleyip listelemeyeceğini belirtir. |doğru/yanlış |Hayır |


#### <a name="example"></a>Örnek


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

Daha fazla bilgi için [Amazon S3 konektör makalesine](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties)bakın.

## <a name="file-system"></a>Dosya Sistemi


### <a name="linked-service"></a>Bağlı hizmet
Şirket içi dosya sistemini, şirket içi **File Server** bağlantılı hizmetle bir Azure veri fabrikasına bağlayabilirsiniz. Aşağıdaki tablo, Şirket Içi Dosya Sunucusu bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliğinin **OnPremisesFileServer**olarak ayarlandığından emin olun. |Evet |
| konak |Kopyalamak istediğiniz klasörün kök yolunu belirtir. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın. |Evet |
| Userıd |Sunucuya erişimi olan kullanıcının kimliğini belirtin. |Hayır (şifrelenmiş Credential seçerseniz) |
| password |Kullanıcının parolasını belirtin (userid). |Hayır (şifrelenmiş Credential seçerseniz |
| şifreli Credential |New-AzDataFactoryEncryptValue cmdlet'i çalıştırarak elde edebileceğiniz şifrelenmiş kimlik bilgilerini belirtin. |Hayır (userid ve parolayı düz metinolarak belirtmeyi seçerseniz) |
| ağ geçidiAdı |Veri Fabrikası'nın şirket içi dosya sunucusuna bağlanmak için kullanması gereken ağ geçidinin adını belirtir. |Evet |

#### <a name="sample-folder-path-definitions"></a>Örnek klasör yolu tanımları

| Senaryo | Bağlantılı hizmet tanımında ana bilgisayar | folderDataset tanımında yol |
| --- | --- | --- |
| Veri Yönetimi Ağ Geçidi makinesinde yerel klasör: <br/><br/>Örnekler: D:\\ \* veya D:\folder\subfolder\\* |D:\\ \\ (Veri Yönetimi Ağ Geçidi 2.0 ve sonraki sürümler için) <br/><br/> localhost (Veri Yönetimi Ağ Geçidi 2.0'dan önceki sürümler için) |. veya\\\\klasör alt klasörü (Veri Yönetimi Ağ Geçidi 2.0 ve sonraki sürümler için) \\ \\ <br/><br/>D:\\ \\ veya\\\\D: klasör alt klasörü\\\\(2.0'ın altındaki ağ geçidi sürümü için) |
| Uzaktan paylaşılan klasör: <br/><br/>Örnekler: \\ \\\\myserver\\ \* \\ \\share\\veya\\\\myserver share folder alt klasörü\\* |\\\\\\\\myserver\\\\payı |. veya\\\\klasör alt klasörü \\ \\ |


#### <a name="example-using-username-and-password-in-plain-text"></a>Örnek: Kullanıcı adı ve parolayı düz metinde kullanma

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Örnek: Şifreli credential kullanma

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için [Dosya Sistemi bağlayıcısı makalesine](data-factory-onprem-file-system-connector.md#linked-service-properties)bakın.

### <a name="dataset"></a>Veri kümesi
Dosya Sistemi veri kümesini tanımlamak için, veri kümesinin **türünü** **FileShare**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt patina belirtir. Dizedeki özel karakterler için kaçış karakterini '\' kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimdedir: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| dosyaFiltre |Tüm dosyalar yerine folderPath'teki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin. <br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnek 1: "fileFilter": "*.log"<br/>Örnek 2: "fileFilter": 2016-1-?. txt"<br/><br/>FileFilter'in bir giriş FileShare veri kümesi için geçerli olduğunu unutmayın. |Hayır |
| bölümlemeBy |Zaman serisi verileri için dinamik bir folderPath/fileName belirtmek için partitionedBy'yi kullanabilirsiniz. Bir örnek, her saatlik veriler için parametrelendirilmiş klasörYol'dur. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**; ve desteklenen düzeyleri şunlardır: **Optimal** ve **En Hızlı**. bkz. [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimleri.](data-factory-supported-file-and-compression-formats.md#compression-support) |Hayır |

> [!NOTE]
> fileName ve fileFilter'i aynı anda kullanamazsınız.

#### <a name="example"></a>Örnek

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Dosya Sistemi bağlayıcısı makalesine](data-factory-onprem-file-system-connector.md#dataset-properties)bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Kaynağı
Dosya Sisteminden veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **FileSystemSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Daha fazla bilgi için [Dosya Sistemi bağlayıcısı makalesine](data-factory-onprem-file-system-connector.md#copy-activity-properties)bakın.

### <a name="file-system-sink-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Batması
Verileri Dosya Sistemi'ne kopyalıyorsanız, kopyalama etkinliğinin **lavabo türünü** **FileSystemSink**olarak ayarlayın ve **lavabo** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| copyBehavior |Kaynak BlobSource veya FileSystem olduğunda kopyalama davranışını tanımlar. |**Koruma Hiyerarşisi:** Hedef klasördeki dosya hiyerarşisini korur. Diğer bir diğer olarak, kaynak dosyanın kaynak klasöre göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><br/>**FlattenHiyerarşi:** Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyinde oluşturulur. Hedef dosyalar otomatik olarak oluşturulan bir adla oluşturulur.<br/><br/>**Birleştirme Dosyaları:** Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı/blob adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. |Hayır |

otomatik-

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Dosya Sistemi bağlayıcısı makalesine](data-factory-onprem-file-system-connector.md#copy-activity-properties)bakın.

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Bağlı hizmet
FTP bağlantılı bir hizmeti tanımlamak için, **ftpServer'a**bağlı hizmet **türünü** ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| konak |FTP Server'ın adı veya IP adresi |Evet |&nbsp; |
| authenticationType |Kimlik doğrulaması türünü belirtme |Evet |Temel, Anonim |
| kullanıcı adı |FTP sunucusuna erişimi olan kullanıcı |Hayır |&nbsp; |
| password |Kullanıcı nın şifresi (kullanıcı adı) |Hayır |&nbsp; |
| şifreli Credential |FTP sunucusuna erişmek için şifrelenmiş kimlik bilgisi |Hayır |&nbsp; |
| ağ geçidiAdı |Şirket içi FTP sunucusuna bağlanmak için Veri Yönetimi Ağ Geçidi'nin adı |Hayır |&nbsp; |
| port |FTP sunucusunun dinlediği bağlantı noktası |Hayır |21 |
| sağlarSsl |FTP'nin SSL/TLS kanalında kullanılıp kullanılmayacağını belirtin |Hayır |true |
| enableServerCertificateValidation |SSL/TLS kanalı üzerinden FTP kullanırken sunucu TLS/SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğini belirtin |Hayır |true |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamayı kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Örnek: Temel kimlik doğrulama için kullanıcı adı ve parolayı düz metin olarak kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Örnek: Port'u kullanarak, enableSsl, enableServerCertificateValidation

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Örnek: Kimlik doğrulama ve ağ geçidi için şifreli Credential kullanma

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

Daha fazla bilgi için [FTP bağlayıcısı](data-factory-ftp-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
FTP veri kümesini tanımlamak için, veri kümesinin **türünü** **FileShare**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt yol. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| dosyaFiltre |Tüm dosyalar yerine folderPath'teki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnekler 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik HDFS ile desteklenmez. |Hayır |
| bölümlemeBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, klasörPath veri her saat için parametreli. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**; ve desteklenen düzeyleri şunlardır: **Optimal** ve **En Hızlı**. Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağını belirtin. İkili mod ve yanlış ASCII için geçerlidir. Varsayılan değer: True. Bu özellik yalnızca ilişkili bağlantılı hizmet türü türünde olduğunda kullanılabilir: FtpServer. |Hayır |

> [!NOTE]
> dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için [FTP bağlayıcısı](data-factory-ftp-connector.md#dataset-properties) makalesine bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Kaynağı
Bir FTP sunucusundan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **FileSystemSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

Daha fazla bilgi için [FTP bağlayıcısı](data-factory-ftp-connector.md#copy-activity-properties) makalesine bakın.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Bağlı hizmet
HDFS bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **HdFS'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **Hdfs** |Evet |
| Url |HDFS'nin URL'si |Evet |
| authenticationType |Anonim veya Windows. <br><br> HDFS bağlayıcısı için **Kerberos kimlik doğrulamasını** kullanmak için, şirket içi ortamınızı buna göre ayarlamak için bu bölüme bakın. |Evet |
| userName |Windows kimlik doğrulaması için kullanıcı adı. |Evet (Windows Kimlik Doğrulama için) |
| password |Windows kimlik doğrulama için parola. |Evet (Windows Kimlik Doğrulama için) |
| ağ geçidiAdı |Veri Fabrikası hizmetinin HDFS'ye bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| şifreli Credential |[Yeni-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) erişim kimlik bilgisi çıktısı. |Hayır |

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamayı kullanma

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Örnek: Windows kimlik doğrulamakullanma

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
HDFS veri kümesini tanımlamak için, veri kümesinin **türünü** **FileShare**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre giden yol. Örnek: `myfolder`<br/><br/>Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örneğin: folder\subfolder için klasör\\\\alt klasörünü belirtin ve d:\samplefolder için d:\\\\samplefolder belirtin.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: <br/><br/>`Data.<Guid>.txt`(örneğin: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Hayır |
| bölümlemeBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örnek: klasörHer saat veri için parametreli yol. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

> [!NOTE]
> dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Kaynağı
HDFS'den veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **FileSystemSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

**FileSystemSource** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için HDFS bağlayıcı makalesine bakın.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Bağlı hizmet
Bir SFTP bağlantılı hizmeti tanımlamak için, Bağlı **hizmettürünü** **Sftp'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| konak | SFTP sunucusunun adı veya IP adresi. |Evet |
| port |SFTP sunucusunun dinlediği bağlantı noktası. Varsayılan değer: 21 |Hayır |
| authenticationType |Kimlik doğrulama türünü belirtin. İzin verilen değerler: **Temel**, **SshPublicKey**. <br><br> Temel kimlik doğrulamayı kullanma ve [sb ortak anahtar kimlik doğrulama](#using-ssh-public-key-authentication) bölümlerini sırasıyla daha fazla özellik ve JSON örneklerine göre kullanma bölümüne bakın. |Evet |
| skipHostKeyValidation | Ana bilgisayar anahtar doğrulaması atlayıp atlamayacağıbelirtin. | Hayır. Varsayılan değer: false |
| hostKeyFingerprint | Ana bilgisayar anahtarının parmak izini belirtin. | Evet eğer `skipHostKeyValidation` yanlış ayarlanırsa.  |
| ağ geçidiAdı |Şirket içi bir SFTP sunucusuna bağlanmak için Veri Yönetimi Ağ Geçidi'nin adı. | Şirket içi bir SFTP sunucusundan veri kopyalıyorsanız evet. |
| şifreli Credential | SFTP sunucusuna erişmek için şifrelenmiş kimlik bilgisi. Kopya sihirbazında veya ClickOnce açılır pencereiletişim kutusunda temel kimlik doğrulaması (kullanıcı adı + parola) veya SshPublicKey kimlik doğrulaması (kullanıcı adı + özel anahtar yolu veya içerik) belirttiğinde otomatik olarak oluşturulur. | Hayır. Yalnızca şirket içi bir SFTP sunucusundan veri kopyalarken uygulayın. |

#### <a name="example-using-basic-authentication"></a>Örnek: Temel kimlik doğrulamayı kullanma

Temel kimlik doğrulamasını `authenticationType` kullanmak `Basic`için, "olarak ayarlayın ve son bölümde tanıtılan SFTP bağlayıcısı jenerik özelliklerinin yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | SFTP sunucusuna erişimi olan kullanıcı. |Evet |
| password | Kullanıcı nın şifresi (kullanıcı adı). | Evet |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Örnek: **Şifreli kimlik bilgisi içeren temel kimlik doğrulaması**

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>**SSH ortak anahtar kimlik doğrulamasını kullanma:**

Temel kimlik doğrulamasını `authenticationType` kullanmak `SshPublicKey`için, "olarak ayarlayın ve son bölümde tanıtılan SFTP bağlayıcısı jenerik özelliklerinin yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı |SFTP sunucusuna erişimi olan kullanıcı |Evet |
| privateKeyPath | Ağ geçidinin erişebileceği özel anahtar dosyasına mutlak yol belirtin. | Belirtin `privateKeyPath` ya `privateKeyContent`da . <br><br> Yalnızca şirket içi bir SFTP sunucusundan veri kopyalarken uygulayın. |
| privateKeyContent | Özel anahtar içeriğinin serileştirilmiş dizesi. Kopyalama Sihirbazı özel anahtar dosyasını okuyabilir ve özel anahtar içeriğini otomatik olarak ayıklayabilir. Başka bir araç/SDK kullanıyorsanız, bunun yerine privateKeyPath özelliğini kullanın. | Belirtin `privateKeyPath` ya `privateKeyContent`da . |
| Parola | Anahtar dosyası bir geçiş tümceciği tarafından korunuyorsa, özel anahtarın şifresini çözmek için geçiş tümceciği/parolasını belirtin. | Özel anahtar dosyası bir geçiş tümceciği tarafından korunuyorsa evet. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Örnek: **Özel anahtar içeriğini kullanarak SshPublicKey kimlik doğrulaması**

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

Daha fazla bilgi için [SFTP bağlayıcısı](data-factory-sftp-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir SFTP veri kümesi tanımlamak için, veri kümesinin **türünü** **FileShare**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| folderPath |Klasöre alt yol. Dizedeki özel karakterler için ' \ ' kaçış karakterini kullanın. Örnekler için Örnek bağlantılı hizmet ve veri kümesi tanımlarına bakın.<br/><br/>Bu özelliği **partitionBy** ile birleştirerek dilim başlangıç/bitiş tarih saatlerine göre klasör yollarına sahip olabilirsiniz. |Evet |
| fileName |Tablonun klasördeki belirli bir dosyaya başvurmasını istiyorsanız **klasörDeki Dosyanın** adını belirtinPath. Bu özellik için herhangi bir değer belirtmezseniz, tablo klasördeki tüm dosyaları işaret edir.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde, oluşturulan dosyanın adı aşağıdaki biçimde olacaktır: <br/><br/>`Data.<Guid>.txt`(Örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Hayır |
| dosyaFiltre |Tüm dosyalar yerine folderPath'teki dosyaların bir alt kümesini seçmek için kullanılacak bir filtre belirtin.<br/><br/>İzin verilen `*` değerler şunlardır: `?` (birden çok karakter) ve (tek karakter).<br/><br/>Örnekler 1:`"fileFilter": "*.log"`<br/>Örnek 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter bir giriş FileShare veri kümesi için geçerlidir. Bu özellik HDFS ile desteklenmez. |Hayır |
| bölümlemeBy |partitionedBy dinamik bir folderPath, zaman serisi verileri için dosya adı belirtmek için kullanılabilir. Örneğin, klasörPath veri her saat için parametreli. |Hayır |
| biçim | Aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkeFormat**. Biçim altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. <br><br> Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında **olduğu gibi kopyalamak** istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |
| useBinaryTransfer |İkili aktarım modunu kullanıp kullanmayacağını belirtin. İkili mod ve yanlış ASCII için geçerlidir. Varsayılan değer: True. Bu özellik yalnızca ilişkili bağlantılı hizmet türü türünde olduğunda kullanılabilir: FtpServer. |Hayır |

> [!NOTE]
> dosya adı ve fileFilter aynı anda kullanılamaz.

#### <a name="example"></a>Örnek

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için [SFTP bağlayıcısı](data-factory-sftp-connector.md#dataset-properties) makalesine bakın.

### <a name="file-system-source-in-copy-activity"></a>Kopyalama Etkinliğinde Dosya Sistemi Kaynağı
Bir SFTP kaynağından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **FileSystemSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| Özyinelemeli |Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. |True, False (varsayılan) |Hayır |



#### <a name="example"></a>Örnek

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

Daha fazla bilgi için [SFTP bağlayıcısı](data-factory-sftp-connector.md#copy-activity-properties) makalesine bakın.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Bağlı hizmet
HTTP bağlantılı bir hizmet tanımlamak için, bağlı hizmet **türünü** **Http**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url | Web Sunucusuna TEMEL URL | Evet |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler şunlardır: **Anonim**, **Temel**, **Özet**, **Windows**, **ClientCertificate**. <br><br> Bu tablonun altındaki bölümlere bakın, sırasıyla bu kimlik doğrulama türleri için daha fazla özellik ve JSON örnekleri. | Evet |
| enableServerCertificateValidation | Kaynak HTTPS Web Server ise sunucu TLS/SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğini belirtin | Hayır, varsayılan değer doğrudur |
| ağ geçidiAdı | Şirket içi bir HTTP kaynağına bağlanmak için Veri Yönetimi Ağ Geçidi'nin adı. | Şirket içi bir HTTP kaynağından veri kopyalıyorsanız evet. |
| şifreli Credential | HTTP bitiş noktasına erişmek için şifrelenmiş kimlik bilgisi. Kopya sihirbazı veya ClickOnce açılır pencereiletişim kutusunda kimlik doğrulama bilgilerini yapılandırdığınızda otomatik olarak oluşturulur. | Hayır. Yalnızca şirket içi bir HTTP sunucusundan veri kopyalarken uygulayın. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Örnek: Temel, Özet veya Windows kimlik doğrulamayı kullanma
Yukarıda `authenticationType` `Basic`tanıtılan HTTP bağlayıcısı genel özelliklerinin yanı sıra aşağıdaki özellikleri , veya `Digest` `Windows`, olarak ayarlayın:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| kullanıcı adı | HTTP bitiş noktasına erişmek için kullanıcı adı. | Evet |
| password | Kullanıcı nın şifresi (kullanıcı adı). | Evet |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Örnek: ClientCertificate kimlik doğrulamasını kullanma

Temel kimlik doğrulamasını `authenticationType` kullanmak `ClientCertificate`için, "olarak ayarlayın ve yukarıda tanıtılan HTTP bağlayıcısı genel özelliklerinin yanı sıra aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| embeddedCertData | Kişisel Bilgi Değişimi (PFX) dosyasının temel 64 kodlu içeriği. | Belirtin `embeddedCertData` ya `certThumbprint`da . |
| certThumbprint | Ağ geçidi makinenizin sertifika deposuna yüklenen sertifikanın parmak izi. Verileri yalnızca şirket içi bir HTTP kaynağından kopyalarken uygulayın. | Belirtin `embeddedCertData` ya `certThumbprint`da . |
| password | Sertifikayla ilişkili parola. | Hayır |

Kimlik doğrulama `certThumbprint` için kullanıyorsanız ve sertifika yerel bilgisayarın kişisel deposuna yüklüyse, ağ geçidi hizmetine okuma izni vermeniz gerekir:

1. Microsoft Yönetim Konsolu'nun (MMC) başlatılmasını başlatın. **Yerel Bilgisayarı**hedefleyen **Sertifikaları** ekleme.
2. **Sertifikaları**Genişletin , **Kişisel**, ve **Sertifikaları**tıklatın.
3. Kişisel mağazadan sertifikaya sağ tıklayın ve **Tüm Görevler**->**In Özel Tuşları Yönet'i seçin...**
3. **Güvenlik** sekmesinde, sertifikaya okuma erişimiyle birlikte Veri Yönetimi Ağ Geçidi Ana Bilgisayarı Hizmetinin çalıştırıldığı kullanıcı hesabını ekleyin.

**Örnek: istemci sertifikasını kullanarak:** Bu bağlantılı hizmet, veri fabrikanızı şirket içi BIR HTTP web sunucusuna bağlar. Veri Yönetimi Ağ Geçidi yüklü makineye yüklenen bir istemci sertifikası kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Örnek: bir dosyada istemci sertifikası kullanma
Bu bağlantılı hizmet, veri fabrikanızı şirket içi BIR HTTP web sunucusuna bağlar. Veri Yönetimi Ağ Geçidi yüklü bir istemci sertifika dosyası makinede kullanır.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

Daha fazla bilgi için [HTTP bağlayıcı](data-factory-http-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
BIR HTTP veri kümesi tanımlamak için, veri kümesinin **türünü** **Http**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Relativeurl | Verileri içeren kaynağa göreli bir URL. Yol belirtilmediğinde, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. <br><br> Dinamik URL oluşturmak için [Veri Fabrikası işlevlerini ve sistem değişkenlerini](data-factory-functions-variables.md)kullanabilirsiniz, Örnek: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Hayır |
| requestMethod | Http yöntemi. İzin verilen değerler **GET** veya **POST'tür.** | Hayır. `GET` varsayılan değerdir. |
| ek Headers | Ek HTTP istek üstbilgi. | Hayır |
| requestBody | HTTP isteği için gövde. | Hayır |
| biçim | Verileri ayrıştırmadan **OLDUĞU gibi HTTP uç noktasından almak** istiyorsanız, bu biçim ayarlarını atlayın. <br><br> Kopyalama sırasında HTTP yanıt içeriğini ayrışdırmak istiyorsanız, aşağıdaki biçim türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParkquetFormat**. Daha fazla bilgi için [Metin Biçimi,](data-factory-supported-file-and-compression-formats.md#text-format) [Json Formatı,](data-factory-supported-file-and-compression-formats.md#json-format) [Avro Biçimi,](data-factory-supported-file-and-compression-formats.md#avro-format) [Ork Biçimi](data-factory-supported-file-and-compression-formats.md#orc-format)ve [Parke Biçimi](data-factory-supported-file-and-compression-formats.md#parquet-format) bölümlerine bakın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Desteklenen türleri şunlardır: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**. Desteklenen seviyeler şunlardır: **Optimal** ve **En Hızlı.** Daha fazla bilgi için [Azure Veri Fabrikası'ndaki Dosya ve sıkıştırma biçimlerine](data-factory-supported-file-and-compression-formats.md#compression-support)bakın. |Hayır |

#### <a name="example-using-the-get-default-method"></a>Örnek: GET (varsayılan) yöntemini kullanarak

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-the-post-method"></a>Örnek: POST yöntemini kullanarak

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
Daha fazla bilgi için [HTTP bağlayıcı](data-factory-http-connector.md#dataset-properties) makalesine bakın.

### <a name="http-source-in-copy-activity"></a>Kopyalama Etkinliğinde HTTP Kaynağı
Bir HTTP kaynağından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **HttpSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| httpRequestTimeout | Yanıt almak için HTTP isteği için zaman aralığı (TimeSpan). Yanıt verilerini okumak için zaman arası değil, yanıt almak için zaman aralarıdır. | Hayır. Varsayılan değer: 00:01:40 |


#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [HTTP bağlayıcı](data-factory-http-connector.md#copy-activity-properties) makalesine bakın.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Bağlı hizmet
OData bağlantılı bir hizmet tanımlamak için, bağlı **hizmettürünü** **OData'ya**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| url |OData hizmetinin url'si. |Evet |
| authenticationType |OData kaynağına bağlanmak için kullanılan kimlik doğrulama türü. <br/><br/> Bulut OData için olası değerler Anonim, Temel ve OAuth'dur (azure veri fabrikasının şu anda yalnızca Azure Active Directory tabanlı OAuth'u desteklediğini unutmayın). <br/><br/> Şirket içi OData için olası değerler Anonim, Temel ve Windows'lardır. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. |Evet (yalnızca Temel kimlik doğrulaması kullanıyorsanız) |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Evet (yalnızca Temel kimlik doğrulaması kullanıyorsanız) |
| yetkiliCredential |OAuth kullanıyorsanız, Veri Fabrikası Kopyalama Sihirbazı veya Düzenleyici'deki **Yetkilendirme** düğmesini tıklatın ve kimlik bilgilerinizi girin, bu özelliğin değeri otomatik olarak oluşturulur. |Evet (yalnızca OAuth kimlik doğrulaması kullanıyorsanız) |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi OData hizmetine bağlanmak için kullanması gereken ağ geçidinin adı. Yalnızca şirket içi OData kaynağından veri kopyalıyorsanız belirtin. |Hayır |

#### <a name="example---using-basic-authentication"></a>Örnek - Temel kimlik doğrulamayı kullanma
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Örnek - Anonim kimlik doğrulamayı kullanma

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Örnek - Şirket içi OData kaynağına erişen Windows kimlik doğrulamayı kullanma

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Örnek - Bulut OData kaynağına erişen OAuth kimlik doğrulamasını kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

Daha fazla bilgi için [OData bağlayıcısı](data-factory-odata-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir OData veri kümesi tanımlamak için, veri kümesinin **türünü** **ODataResource**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| yol |OData kaynağına giden yol |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Daha fazla bilgi için [OData bağlayıcısı](data-factory-odata-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Bir OData kaynağından veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Örnek | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |"?$select=İsim, Açıklama&$top=5" |Hayır |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

Daha fazla bilgi için [OData bağlayıcısı](data-factory-odata-connector.md#copy-activity-properties) makalesine bakın.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Bağlı hizmet
ODBC bağlantılı bir hizmeti tanımlamak için, bağlı **hizmettürünü** **OnPremisesOdbc'ye**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Connectionstring |Bağlantı dizesinin erişmeyen kimlik bilgileri bölümü ve isteğe bağlı olarak şifrelenmiş kimlik bilgisi. Aşağıdaki bölümlerdeki örneklere bakın. |Evet |
| kimlik bilgisi |Bağlantı dizesinin sürücüye özgü özellik değeri biçiminde belirtilen erişim kimlik bilgileri bölümü. Örnek: `“Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.` |Hayır |
| authenticationType |ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim ve Temel. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin ODBC veri deposuna bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

#### <a name="example---using-basic-authentication"></a>Örnek - Temel kimlik doğrulamayı kullanma

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Örnek - Şifreli kimlik bilgileriyle Temel kimlik doğrulamayı kullanma
[Yeni-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) cmdlet kullanarak kimlik bilgilerini şifreleyebilirsiniz.

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Örnek: Anonim kimlik doğrulamayı kullanma

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Daha fazla bilgi için [ODBC bağlayıcı](data-factory-odbc-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
ODBC veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |ODBC veri deposundaki tablonun adı. |Evet |


#### <a name="example"></a>Örnek

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [ODBC bağlayıcı](data-factory-odbc-connector.md#dataset-properties) makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Bir ODBC veri deposundan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `select * from MyTable`. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Daha fazla bilgi için [ODBC bağlayıcı](data-factory-odbc-connector.md#copy-activity-properties) makalesine bakın.

## <a name="salesforce"></a>Salesforce


### <a name="linked-service"></a>Bağlı hizmet
Salesforce'a bağlı bir hizmet tanımlamak için, bağlı hizmet **türünü** **Salesforce**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| çevreUrl | Salesforce örneğinin URL'sini belirtin. <br><br> - Varsayılan "https:\//login.salesforce.com"dir. <br> - Sanal andan verileri kopyalamakhttps://test.salesforce.comiçin " ". <br> - Özel etki alanından verileri kopyalamak için, örneğin "https://[domain].my.salesforce.com" adresini belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| Securitytoken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz. Bir güvenlik belirteci sıfırlama/alma hakkında talimatlar için [güvenlik belirteci alın.](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) Genel olarak güvenlik belirteçleri hakkında bilgi edinmek için [Güvenlik ve API'ye](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)bakın. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

Daha fazla bilgi için [Salesforce bağlayıcı](data-factory-salesforce-connector.md#linked-service-properties) sı makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Salesforce veri kümesini tanımlamak için, veri kümesinin **türünü** **İlişkisel Tablo**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce'taki tablonun adı. |Hayır **(İlişkisel Kaynak** **sorgusu** belirtilirse) |

#### <a name="example"></a>Örnek

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Daha fazla bilgi için [Salesforce bağlayıcı](data-factory-salesforce-connector.md#dataset-properties) sı makalesine bakın.

### <a name="relational-source-in-copy-activity"></a>Kopyalama Etkinliğinde İlişkisel Kaynak
Salesforce'tan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **RelationalSource**olarak ayarlayın ve **kaynak** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |Bir SQL-92 sorgusu veya [Salesforce Nesne Sorgu Dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgusu. Örneğin:  `select * from MyTable__c`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

#### <a name="example"></a>Örnek



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> API Adı'nın "__c" bölümü herhangi bir özel nesne için gereklidir.

Daha fazla bilgi için [Salesforce bağlayıcı](data-factory-salesforce-connector.md#copy-activity-properties) sı makalesine bakın.

## <a name="web-data"></a>Web Verileri

### <a name="linked-service"></a>Bağlı hizmet
Web'e bağlı bir hizmet tanımlamak için, bağlı **hizmettürünü** **Web'e**ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Url |Web kaynağına URL |Evet |
| authenticationType |Anonim. |Evet |


#### <a name="example"></a>Örnek


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

Daha fazla bilgi için [Web Tablosu bağlayıcı](data-factory-web-table-connector.md#linked-service-properties) makalesine bakın.

### <a name="dataset"></a>Veri kümesi
Bir Web veri kümesi tanımlamak için, veri kümesinin **türünü** **WebTable**olarak ayarlayın ve **typeProperties** bölümünde aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |veri kümesinin türü. **WebTable** olarak ayarlanmalıdır |Evet |
| yol |Tabloyu içeren kaynağa göreli bir URL. |Hayır. Yol belirtilmediğinde, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. |
| dizin |Kaynaktaki tablonun dizini. Bkz. HTML sayfasındaki tablonun dizinini alma adımları için HTML sayfa bölümündeki tablonun dizinini alın. |Evet |

#### <a name="example"></a>Örnek

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Daha fazla bilgi için [Web Tablosu bağlayıcı](data-factory-web-table-connector.md#dataset-properties) makalesine bakın.

### <a name="web-source-in-copy-activity"></a>Kopyalama Etkinliğinde Web Kaynağı
Bir web tablosundan veri kopyalıyorsanız, kopyalama etkinliğinin **kaynak türünü** **WebSource**olarak ayarlayın. Şu anda, kopyalama etkinliğindeki kaynak **WebSource**türünde olduğunda, ek özellikler desteklenmez.

#### <a name="example"></a>Örnek

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Daha fazla bilgi için [Web Tablosu bağlayıcı](data-factory-web-table-connector.md#copy-activity-properties) makalesine bakın.

## <a name="compute-environments"></a>HESAPLAMA ORTAMLARI
Aşağıdaki tabloda Veri Fabrikası tarafından desteklenen bilgi işlem ortamları ve bunlar üzerinde çalıştırılabilecek dönüşüm etkinlikleri listelenir. Bağlantılı hizmet için JSON şemalarını görmek için ilgilendiğiniz bilgi işlem için bağlantıyı tıklattınız.

| İşlem ortamı | Etkinlikler |
| --- | --- |
| [İsteğe bağlı HDInsight kümesi](#on-demand-azure-hdinsight-cluster) veya [kendi HDInsight kümeniz](#existing-azure-hdinsight-cluster) |[.NET özel etkinlik](#net-custom-activity), [Kovan etkinliği](#hdinsight-hive-activity), [Domuz etkinliği](#hdinsight-pig-activity), [MapReduce etkinliği](#hdinsight-mapreduce-activity), Hadoop akış etkinliği, Kıvılcım [etkinliği](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET özel etkinliği](#net-custom-activity) |
| [Azure Makine Öğrenimi](#azure-machine-learning) | [Makine Öğrenme Toplu Yürütme Etkinliği](#machine-learning-batch-execution-activity), Makine Öğrenme Güncelleme Kaynak [Etkinliği](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL Veritabanı](#azure-sql-database), [Azure SQL Veri Ambarı](#azure-sql-data-warehouse), [SQL Server](#sql-server-stored-procedure) |[Depolanan Yordam](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>İsteğe bağlı Azure HDInsight kümesi
Azure Veri Fabrikası hizmeti, verileri işlemek için otomatik olarak Windows/Linux tabanlı isteğe bağlı HDInsight kümesi oluşturabilir. Küme, kümeyle ilişkili depolama hesabıyla (JSON'daki linkedServiceName özelliği) aynı bölgede oluşturulur. Bu bağlantılı hizmette aşağıdaki dönüşüm etkinliklerini çalıştırabilirsiniz: [.NET özel etkinlik](#net-custom-activity), [Kovan etkinliği](#hdinsight-hive-activity), Domuz [etkinliği](#hdinsight-pig-activity), [MapReduce etkinliği](#hdinsight-mapreduce-activity), Hadoop akış etkinliği, [Kıvılcım etkinliği](#hdinsight-spark-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tablo, isteğe bağlı HDInsight bağlantılı bir hizmetin Azure JSON tanımında kullanılan özelliklerin açıklamalarını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği **HDInsightOnDemand**olarak ayarlanmalıdır. |Evet |
| clusterSize |Kümedeki alt/veri düğümlerinin sayısı. HDInsight kümesi, bu özellik için belirttiğiniz işçi düğümlerinin sayısıyla birlikte 2 kafa düğümüyle oluşturulur. Düğümler 4 çekirdekli boyut Standard_D3, bu nedenle 4 işçi düğüm kümesi 24\*çekirdek alır (4 4 = 16\*işçi düğümleri için çekirdek, artı 2 4 = kafa düğümleri için 8 çekirdek). Standard_D3 katmanı hakkında ayrıntılı bilgi için [HDInsight'ta Linux tabanlı Hadoop kümeleri oluşturun'a](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) bakın. |Evet |
| timetolive |İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. İsteğe bağlı HDInsight kümesinin, kümede başka etkin iş yoksa, bir etkinlik çalışması tamamlandıktan sonra ne kadar süre hayatta kalacağını belirtir.<br/><br/>Örneğin, bir etkinlik çalışması 6 dakika sürerse ve zaman 5 dakika olarak ayarlanırsa, küme, 6 dakikalık etkinlik çalışması işledikten sonra 5 dakika boyunca canlı kalır. Başka bir etkinlik çalışması 6 dakika penceresi ile yürütülür, aynı küme tarafından işlenir.<br/><br/>İsteğe bağlı HDInsight kümesi oluşturmak pahalı bir işlemdir (biraz zaman alabilir), bu nedenle isteğe bağlı HDInsight kümesini yeniden kullanarak bir veri fabrikasının performansını artırmak için bu ayarı gerektiği gibi kullanın.<br/><br/>Timetolive değerini 0 olarak ayarlarsanız, etkinlik işlenir çalışmaz küme silinir. Diğer taraftan, yüksek bir değer belirlerseniz, küme gereksiz yere yüksek maliyetlere neden olarak boşta kalabilir. Bu nedenle, gereksinimlerinize göre uygun değeri ayarlamanız önemlidir.<br/><br/>Zaman çarpan özellik değeri uygun şekilde ayarlanmışsa, birden çok ardışık ardışık alan, isteğe bağlı HDInsight kümesinin aynı örneğini paylaşabilir |Evet |
| version |HDInsight kümesinin sürümü. Ayrıntılar için [Azure Veri Fabrikası'nda desteklenen HDInsight sürümlerine](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory)bakın. |Hayır |
| linkedServiceName |Azure Depolama bağlantılı hizmet, verileri depolamak ve işlemek için isteğe bağlı küme tarafından kullanılacak. <p>Şu anda, depolama alanı olarak Bir Azure Veri Gölü Deposu kullanan isteğe bağlı bir HDInsight kümesi oluşturamazsınız. HDInsight işleminden elde edilen sonuç verilerini Bir Azure Veri Gölü Deposu'nda depolamak istiyorsanız, verileri Azure Blob Depolama'dan Azure Veri Gölü Deposu'na kopyalamak için bir Kopyalama Etkinliği kullanın.</p>  | Evet |
| ekLinkedServiceNames |Veri Fabrikası hizmetinin sizin adınıza kaydedilebilmeleri için HDInsight bağlantılı hizmet için ek depolama hesapları belirtir. |Hayır |
| osType |İşletim sistemi türü. İzin verilen değerler şunlardır: Windows (varsayılan) ve Linux |Hayır |
| hcatalogLinkedServiceName |HCatalog veritabanını gösteren Azure SQL bağlantılı hizmetin adı. İsteğe bağlı HDInsight kümesi, Azure SQL veritabanını metastore olarak kullanarak oluşturulur. |Hayır |

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON, Linux tabanlı isteğe bağlı HDInsight bağlantılı bir hizmeti tanımlar. Veri Fabrikası hizmeti, bir veri dilimini işlerken otomatik olarak **Linux tabanlı** bir HDInsight kümesi oluşturur.

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

Daha fazla bilgi için [bkz.](data-factory-compute-linked-services.md)

## <a name="existing-azure-hdinsight-cluster"></a>Mevcut Azure HDInsight kümesi
Kendi HDInsight kümenizi Veri Fabrikası'na kaydettirmek için Azure HDInsight bağlantılı bir hizmet oluşturabilirsiniz. Bu bağlantılı hizmette aşağıdaki veri dönüştürme etkinliklerini çalıştırabilirsiniz: [.NET özel etkinlik](#net-custom-activity), [Kovan etkinliği](#hdinsight-hive-activity), Domuz [etkinliği](#hdinsight-pig-activity), [MapReduce etkinliği](#hdinsight-mapreduce-activity), Hadoop akış etkinliği, [Kıvılcım etkinliği](#hdinsight-spark-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tablo, Azure HDInsight bağlantılı bir hizmetin Azure JSON tanımında kullanılan özelliklerin açıklamalarını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği **HDInsight**olarak ayarlanmalıdır. |Evet |
| clusterUri |HDInsight kümesinin URI'si. |Evet |
| kullanıcı adı |Varolan bir HDInsight kümesine bağlanmak için kullanılacak kullanıcının adını belirtin. |Evet |
| password |Kullanıcı hesabı için parola belirtin. |Evet |
| linkedServiceName | HDInsight kümesi tarafından kullanılan Azure blob depolamasını ifade eden Azure Depolama bağlantılı hizmetin adı. <p>Şu anda, bu özellik için Azure Veri Gölü Deposu'na bağlı bir hizmet belirtemezsiniz. HDInsight kümesi Nin Veri Gölü Deposu'na erişimi varsa, Azure Veri Gölü Deposu'ndaki verilere Hive/Pig komut dosyasından erişebilirsiniz. </p>  |Evet |

Desteklenen HDInsight kümelerinin sürümleri için [desteklenen HDInsight sürümlerine](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory)bakın.

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a>Azure Batch
Bir veri fabrikasına sanal makinelerden (VM) toplu iş havuzukaydetmek için Azure Toplu İşlem bağlantılı bir hizmet oluşturabilirsiniz. .NET özel etkinliklerini Azure Toplu İş veya Azure HDInsight'ı kullanarak çalıştırabilirsiniz. Bu bağlantılı hizmette [bir .NET özel etkinlik](#net-custom-activity) çalıştırabilirsiniz.

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tablo, Azure Toplu İş bağlantılı hizmetin Azure JSON tanımında kullanılan özelliklerin açıklamalarını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği **AzureBatch**olarak ayarlanmalıdır. |Evet |
| accountName |Azure Toplu İş hesabının adı. |Evet |
| Accesskey |Azure Toplu İş hesabı için erişim anahtarı. |Evet |
| poolName |Sanal makineler havuzunun adı. |Evet |
| linkedServiceName |Bu Azure Toplu İş bağlantılı hizmetle ilişkili Azure Depolama bağlantılı hizmetin adı. Bu bağlantılı hizmet, etkinliği çalıştırmak için gereken dosyaları evreleme ve etkinlik yürütme günlüklerini depolamak için kullanılır. |Evet |


#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a>Azure Machine Learning
Bir Machine Learning toplu iş sonu puanlama uç noktasını bir veri fabrikasına kaydetmek için Azure Machine Learning bağlantılı bir hizmet oluşturursunuz. Bu bağlantılı hizmette çalıştırılabilen iki veri dönüştürme etkinliği: [Machine Learning Toplu Yürütme Etkinliği](#machine-learning-batch-execution-activity), Machine Learning Update Kaynak [Etkinliği](#machine-learning-update-resource-activity).

### <a name="linked-service"></a>Bağlı hizmet
Aşağıdaki tablo, Azure Machine Learning bağlantılı bir hizmetin Azure JSON tanımında kullanılan özelliklerin açıklamalarını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Tür |Tür özelliği şu şekilde ayarlanmalıdır: **AzureML**. |Evet |
| mlEndpoint |Toplu puanlama URL'si. |Evet |
| apiKey |Yayınlanan çalışma alanı modelinin API'si. |Evet |

#### <a name="json-example"></a>JSON örneği

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Bir veri zincirindeki Data Lake [Analytics U-SQL etkinliğini](data-factory-usql-activity.md) kullanmadan önce bir Azure Veri Gölü Analytics bilgi işlem hizmetini Azure veri fabrikasına bağlamak için **Azure Veri Gölü Analytics** bağlantılı bir hizmet oluşturursunuz.

### <a name="linked-service"></a>Bağlı hizmet

Aşağıdaki tablo, Azure Veri Gölü Analizi bağlantılı bir hizmetin JSON tanımında kullanılan özelliklerin açıklamalarını sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| Tür |Tür özelliği şu şekilde ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| accountName |Azure Veri Gölü Analizi Hesap Adı. |Evet |
| dataLakeAnalyticsUri |Azure Veri Gölü Analytics URI. |Hayır |
| yetkilendirme |Yetkilendirme kodu, Veri Fabrikası **Düzenleyicisi'ndeki Yetkilendirme** düğmesini tıklattıktan ve OAuth oturumunu tamamladıktan sonra otomatik olarak alınır. |Evet |
| subscriptionId |Azure abonelik kimliği |Hayır (Belirtilmemişse, veri fabrikasıaboneliği kullanılır). |
| resourceGroupName |Azure kaynak grubu adı |Hayır (Belirtilmemişse, veri fabrikasının kaynak grubu kullanılır). |
| Sessionıd |OAuth yetkilendirme oturumundan oturum kimliği. Her oturum kimliği benzersizdir ve yalnızca bir kez kullanılabilir. Veri Fabrikası Düzenleyicisi'ni kullandığınızda, bu kimlik otomatik olarak oluşturulur. |Evet |


#### <a name="json-example"></a>JSON örneği
Aşağıdaki örnek, Azure Veri Gölü Analizi bağlantılı bir hizmet için JSON tanımı nı sağlar.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="sql-server-stored-procedure"></a>SQL Server Stored Yordam

Bir SQL Server bağlantılı hizmet oluşturur sunuz ve veri fabrikası ardışık hattından depolanan yordamı çağırmak için [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) ile birlikte kullanırsınız.

### <a name="linked-service"></a>Bağlı hizmet
Şirket içi BIR SQL Server veritabanını bir veri fabrikasına bağlamak için **OnPremisesSqlServer** türünde bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tablo, şirket içi SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

Aşağıdaki tablo, SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| Connectionstring |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. |Evet |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| kullanıcı adı |Windows Kimlik Doğrulama'yı kullanıyorsanız kullanıcı adını belirtin. Örnek: **alan\\adı kullanıcı adı**. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |

Kimlik bilgilerini **New-AzDataFactoryEncryptValue** cmdlet kullanarak şifreleyebilir ve aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde kullanabilirsiniz **(EncryptedCredential** özelliği):

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Örnek: SQL Kimlik Doğrulaması kullanmak için JSON

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Örnek: Windows Kimlik Doğrulama'yı kullanmak için JSON

Kullanıcı adı ve parola belirtilmişse, ağ geçidi bunları şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabını taklit etmek için kullanır. Aksi takdirde, ağ geçidi doğrudan Ağ Geçidi'nin (başlangıç hesabı) güvenlik bağlamıyla SQL Server'a bağlanır.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Daha fazla bilgi için [SQL Server bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makalesine bakın.

## <a name="data-transformation-activities"></a>VERI DÖNÜŞÜM FAALIYETLERI

Etkinlik | Açıklama
-------- | -----------
[HDInsight Kovan etkinliği](#hdinsight-hive-activity) | Veri Fabrikası ardışık işlonundaki HDInsight Hive etkinliği, Kovan sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür.
[HDInsight Pig etkinliği](#hdinsight-pig-activity) | Veri Fabrikası ardışık işakindeki HDInsight Pig etkinliği, Pig sorgularını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür.
[HDInsight MapReduce Etkinliği](#hdinsight-mapreduce-activity) | Bir Veri Fabrikası boru hattındaki HDInsight MapReduce etkinliği, MapReduce programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür.
[HDInsight Akış Etkinliği](#hdinsight-streaming-activity) | Veri Fabrikası boru hattındaki HDInsight Akış Etkinliği, Hadoop Streaming programlarını kendi veya isteğe bağlı Windows/Linux tabanlı HDInsight kümenizde yürütür.
[HDInsight Spark Etkinliği](#hdinsight-spark-activity) | Veri Fabrikası ardışık işakindeki HDInsight Spark etkinliği, Spark programlarını kendi HDInsight kümenizde yürütür.
[Machine Learning Batch Yürütme Etkinliği](#machine-learning-batch-execution-activity) | Azure Veri Fabrikası, tahmine dayalı analitik için yayınlanmış bir Azure Machine Learning web hizmetini kullanan ardışık hatlar oluşturmanıza olanak tanır. Toplu İşlem Yürütme Etkinliğini bir Azure Veri Fabrikası ardışık hattında kullanarak, toplu olarak veriler üzerinde öngörülerde bulunmak için bir Machine Learning web hizmetini çağırabilirsiniz.
[Machine Learning Kaynak Güncelleştirme Etkinliği](#machine-learning-update-resource-activity) | Zaman içinde, Machine Learning puanlama deneylerinde tahmine dayalı modellerin yeni giriş veri kümeleri kullanılarak yeniden eğitilmesi gerekir. Yeniden eğitim bittikten sonra, yeniden eğitilen Machine Learning modeli yle puanlama web hizmetini güncelleştirmek istersiniz. Web hizmetini yeni eğitilmiş modelle güncelleştirmek için Kaynak Etkinliğini Güncelleştir'i kullanabilirsiniz.
[Saklı Yordam Etkinliği](#stored-procedure-activity) | Aşağıdaki veri depolarından birinde depolanan yordamı çağırmak için Veri Fabrikası ardışık ardışık işlem de Stored yordamı etkinliğini kullanabilirsiniz: Azure SQL Veritabanı, Azure SQL Veri Ambarı, kuruluşunuzdaki SQL Server Veritabanı veya Azure VM.
[Data Lake Analytics U-SQL etkinliği](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL Etkinliği, Azure Veri Gölü Analizi kümesinde bir U-SQL komut dosyası çalıştırıyor.
[.NET özel etkinliği](#net-custom-activity) | Verileri Veri Fabrikası tarafından desteklenmeyen bir şekilde dönüştürmeniz gerekiyorsa, kendi veri işleme mantığınızla özel bir etkinlik oluşturabilir ve ardışık işlemde etkinliği kullanabilirsiniz. Özel .NET etkinliğini, Azure Toplu İş hizmeti veya Azure HDInsight kümesini kullanarak çalışacak şekilde yapılandırabilirsiniz.


## <a name="hdinsight-hive-activity"></a>HDInsight Hive Etkinliği
Kovan Etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik için tür özelliği olmalıdır: **HDInsightHive**. Önce BIR HDInsight bağlantılı hizmet oluşturmanız ve adını **linkedServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü HDInsightHive olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| betiğini çalıştırın |Hive komut dosyasını satır satır belirtin |Hayır |
| komut dosyası yolu |Hive komut dosyasını Azure blob depolama alanında saklayın ve dosyaya giden yolu sağlayın. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| Tanım -lar |'Kovan' kullanarak Hive komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin |Hayır |

Bu tür özellikler Kovan Etkinliği'ne özgür. Diğer özellikler (typeProperties bölümü dışında) tüm etkinlikler için desteklenir.

### <a name="json-example"></a>JSON örneği
Aşağıdaki JSON bir boru hattında bir HDInsight Hive etkinliği tanımlar.

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

Daha fazla bilgi için [Kovan Etkinliği](data-factory-hive-activity.md) makalesine bakın.

## <a name="hdinsight-pig-activity"></a>HDInsight Pig Etkinliği
Pig Activity JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik için tür özelliği olmalıdır: **HDInsightPig**. Önce BIR HDInsight bağlantılı hizmet oluşturmanız ve adını **linkedServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü HDInsightPig olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| betiğini çalıştırın |Domuz komut dosyasını satır satır belirtin |Hayır |
| komut dosyası yolu |Pig komut dosyasını Azure blob depolama alanında saklayın ve dosyaya giden yolu sağlayın. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| Tanım -lar |Pig komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin |Hayır |

Bu tür özellikler Domuz Etkinliği'ne özgür. Diğer özellikler (typeProperties bölümü dışında) tüm etkinlikler için desteklenir.

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

Daha fazla bilgi için Bkz. Pig Activity makalesine bakın.

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce Etkinliği
MapReduce Activity JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik için tür özelliği olmalıdır: **HDInsightMapReduce**. Önce BIR HDInsight bağlantılı hizmet oluşturmanız ve adını **linkedServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü HDInsightMapReduce olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| jarLinkedService | JAR dosyasını içeren Azure Depolama için bağlantılı hizmetin adı. | Evet |
| jarFilePath | Azure Depolama'daki JAR dosyasına giden yol. | Evet |
| Classname | JAR dosyasındaki ana sınıfın adı. | Evet |
| Bağımsız değişken | MapReduce programı için virgülle ayrılmış bağımsız değişkenlerin listesi. Çalışma zamanında MapReduce çerçevesinden birkaç ek bağımsız değişken (örneğin: mapreduce.job.tags) görürsünüz. MapReduce bağımsız değişkenleriyle bağımsız değişkenlerinizi ayırt etmek için, aşağıdaki örnekte gösterildiği gibi hem seçeneği hem de değeri bağımsız değişkenler olarak kullanmayı düşünün (-s, --giriş, --çıktı vb., değerleri tarafından hemen izlenen seçeneklerdir) | Hayır |

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix to determine the similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

Daha fazla bilgi için [MapReduce Etkinlik](data-factory-map-reduce.md) makalesine bakın.

## <a name="hdinsight-streaming-activity"></a>HDInsight Akış Etkinliği
Hadoop Akış Etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **HDInsightStreaming**. Önce BIR HDInsight bağlantılı hizmet oluşturmanız ve adını **linkedServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü HDInsightStreaming olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama |
| --- | --- |
| Eşleştiricisi | Yürütülebilir mapper adı. Örnekte, cat.exe mapper çalıştırılabilir.|
| Redüktör | Çalıştırılabilir indiricinin adı. Örnekte, wc.exe uygulayabilir indirgeyicidir. |
| giriş | Mapper için giriş dosyası (konum dahil). Örnekte: `"wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt"`: adfsample blob kapsayıcı, örnek / veri / Gutenberg klasör, ve davinci.txt blob olduğunu. |
| çıkış | Redüktör için çıkış dosyası (konum dahil). Hadoop Streaming işinin çıktısı bu özellik için belirtilen konuma yazılır. |
| Filepaths | Haritalayıcı ve azaltıcı çalıştırılabilir yollar. Örnekte: "adfsample/example/apps/wc.exe", adfsample blob kapsayıcı, örnek /uygulamalar klasör ve wc.exe yürütülebilir. |
| fileLinkedService | Dosya Yolları bölümünde belirtilen dosyaları içeren Azure depolamasını temsil eden Azure Depolama bağlantılı hizmet. |
| Bağımsız değişken | MapReduce programı için virgülle ayrılmış bağımsız değişkenlerin listesi. Çalışma zamanında MapReduce çerçevesinden birkaç ek bağımsız değişken (örneğin: mapreduce.job.tags) görürsünüz. MapReduce bağımsız değişkenleriyle bağımsız değişkenlerinizi ayırt etmek için, aşağıdaki örnekte gösterildiği gibi hem seçeneği hem de değeri bağımsız değişkenler olarak kullanmayı düşünün (-s, --giriş, --çıktı vb., değerleri tarafından hemen izlenen seçeneklerdir) |
| getDebugInfo | İsteğe bağlı bir öğe. Hata olarak ayarlandığında, günlükler yalnızca hata üzerine karşıdan yüklenir. Tümü olarak ayarlandığında, günlükler yürütme durumuna bakılmaksızın her zaman karşıdan yüklenir. |

> [!NOTE]
> Çıktılar özelliği için Hadoop Akış Etkinliği için bir çıktı veri kümesi **belirtmeniz** gerekir. Bu veri kümesi, ardışık işlem zamanlamasını (saatlik, günlük vb.) sürmek için gereken sahte bir veri kümesi olabilir. Etkinlik bir giriş almazsa, **giriş** özelliği için etkinlik için bir giriş veri kümesi belirtmeyi atlayabilirsiniz.

## <a name="json-example"></a>JSON örneği

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

Daha fazla bilgi için [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md) makalesine bakın.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark Etkinliği
Bir Kıvılcım Etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **HDInsightSpark**. Önce BIR HDInsight bağlantılı hizmet oluşturmanız ve adını **linkedServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü HDInsightSpark olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| rootPath | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harf duyarlıdır. | Evet |
| entryFilePath | Kıvılcım kodunun/paketinin kök klasörüne göreli yol. | Evet |
| Classname | Uygulamanın Java/Spark ana sınıfı | Hayır |
| Bağımsız değişken | Spark programına komut satırı bağımsız değişkenlerinin listesi. | Hayır |
| proxyKullanıcı | Spark programını yürütmek için taklit etmek için kullanıcı hesabı | Hayır |
| kıvılcımConfig | Yapılandırma özelliklerini kıvılcımlandırın. | Hayır |
| getDebugInfo | Kıvılcım günlük dosyalarının, sparkJobLinkedService tarafından belirtilen HDInsight kümesi (veya) tarafından kullanılan Azure depolama alanına kopyalandığında belirtir. İzin verilen değerler: Yok, Her Zaman veya Hata. Varsayılan değer: Hiçbiri. | Hayır |
| sparkJobLinkedService | Spark iş dosyasını, bağımlılıkları ve günlükleri tutan Azure Depolama bağlantılı hizmet.  Bu özellik için bir değer belirtmezseniz, HDInsight kümesiyle ilişkili depolama kullanılır. | Hayır |

### <a name="json-example"></a>JSON örneği

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
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Aşağıdaki noktalara dikkat edin:

- **Tür** özelliği **HDInsightSpark**olarak ayarlanır.
- **RootPath,** adfspark'ın Azure Blob kapsayıcısı olduğu **\\adfspark pyFiles'a** ayarlanır ve pyFiles bu kapsayıcıda ince klasördür. Bu örnekte, Azure Blob Depolama Kıvılcım kümesi ile ilişkili biridir. Dosyayı farklı bir Azure Depolama alanına yükleyebilirsiniz. Bunu yaparsanız, bu depolama hesabını veri fabrikasına bağlamak için Azure Depolama bağlantılı bir hizmet oluşturun. Ardından, bağlı hizmetin adını **sparkJobLinkedService** özelliği için bir değer olarak belirtin. Bu özellik ve Spark Etkinliği tarafından desteklenen diğer özellikler le ilgili ayrıntılar için Spark Etkinlik özelliklerine bakın.
- **entryFilePath** python dosyası olan **test.py**ayarlanır.
- **getDebugInfo** özelliği Her **Zaman**olarak ayarlanır, bu da günlük dosyalarının her zaman oluşturulduğu (başarı veya hata) anlamına gelir.

    > [!IMPORTANT]
    > Sorun gidermediğiniz sürece bu özelliği her zaman üretim ortamında olarak ayarlamamanızı öneririz.
- **Çıktılar** bölümünde bir çıktı veri kümesi vardır. Kıvılcım programı herhangi bir çıktı üretmese bile bir çıktı veri kümesi belirtmeniz gerekir. Çıktı veri kümesi, ardışık hatlar için zamanlamayı (saatlik, günlük vb.) yönlendirir.

Etkinlik hakkında daha fazla bilgi için, [Spark Etkinliği](data-factory-spark.md) makalesine bakın.

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning Batch Yürütme Etkinliği
Azure Machine Learning stüdyosu Toplu Yürütme Etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **AzureMLBatchExecution**. Önce Azure Machine Learning bağlantılı bir hizmet oluşturmanız ve adını **bağlıServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü AzureMLBatchExecution olarak ayarladığınızda, aşağıdaki özellikler **typeProperties** bölümünde desteklenir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
webServiceInput | Azure Machine Learning stüdyo web hizmeti için bir girdi olarak geçirilecek veri kümesi. Bu veri kümesi de etkinlik girişleridahil edilmelidir. |WebServiceInput veya webServiceInputs'i kullanın. |
webServiceInputs | Azure Machine Learning stüdyo web hizmetinin girdisi olarak geçirilecek veri kümelerini belirtin. Web hizmeti birden çok giriş alıyorsa, webServiceInput özelliğini kullanmak yerine webServiceInputs özelliğini kullanın. **webServiceInputs** tarafından başvurulan veri kümeleri de Etkinlik **girişleridahil**edilmelidir. | WebServiceInput veya webServiceInputs'i kullanın. |
webServiceOutputs | Azure Machine Learning stüdyo web hizmeti için çıktı olarak atanan veri kümeleri. Web hizmeti bu veri kümesinde çıktı verilerini döndürür. | Evet |
globalParametreler | Bu bölümde web hizmeti parametreleri için değerleri belirtin. | Hayır |

### <a name="json-example"></a>JSON örneği
Bu örnekte, etkinlik giriş olarak **MLSqlInput** ve çıkış olarak **MLSqlOutput** veri kümesi vardır. **MLSqlInput** **webServiceInput** JSON özelliği kullanılarak web hizmetine bir giriş olarak geçirilir. **MLSqlOutput** **webServiceOutputs** JSON özelliği kullanılarak Web hizmetine bir çıktı olarak geçirilir.

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

JSON örneğinde, dağıtılan Azure Machine Learning Web hizmeti, Azure SQL Veritabanı'ndan/Azure SQL Veritabanına veri okumak/yazmak için bir okuyucu ve yazar modülü kullanır. Bu Web hizmeti aşağıdaki dört parametreyi ortaya çıkarır: Veritabanı sunucu adı, Veritabanı adı, Sunucu kullanıcı hesabı adı ve Sunucu kullanıcı hesabı parolası.

> [!NOTE]
> Yalnızca AzureMLBatchExecution etkinliğinin giriş ve çıktıları Web hizmetine parametre olarak aktarılabilir. Örneğin, yukarıdaki JSON snippet'inde MLSqlInput, WebServiceInput parametresi üzerinden Web hizmetine giriş olarak geçirilen AzureMLBatchExecution etkinliğine giriştir.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning Kaynak Güncelleştirme Etkinliği
Azure Machine Learning studio Update Resource Activity JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **AzureMLUpdateResource**. Önce Azure Machine Learning bağlantılı bir hizmet oluşturmanız ve adını **bağlıServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü AzureMLUpdateResource olarak ayarladığınızda, aşağıdaki özellikler **typeProperties** bölümünde desteklenir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
eğitimliModelName | Yeniden eğitilen modelin adı. | Evet |
eğitimliModelDatasetName | Yeniden eğitim işlemi tarafından döndürülen iLearner dosyasına işaret eden veri kümesi. | Evet |

### <a name="json-example"></a>JSON örneği
Ardışık işlemde iki etkinlik vardır: **AzureMLBatchExecution** ve **AzureMLUpdateResource.** Azure Machine Learning studio Toplu Yürütme etkinliği, eğitim verilerini giriş olarak alır ve çıktı olarak bir iLearner dosyası üretir. Etkinlik, giriş eğitim verileriyle birlikte eğitim web hizmetini (web hizmeti olarak ortaya çıkarılan eğitim deneyi) çağırır ve web hizmetinden ilearner dosyasını alır. Yer tutucuBlob, azure veri fabrikası hizmetitarafından boru hattını çalıştırmak için gereken sahte bir çıktı veri kümesidir.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
U-SQL Activity JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **DataLakeAnalyticsU-SQL**. Azure Veri Gölü Analizi bağlantılı bir hizmet oluşturmanız ve adını **bağlıServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü DataLakeAnalyticsU-SQL olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| scriptPath |U-SQL komut dosyası içeren klasöre giden yol. Dosyanın adı büyük/küçük harf duyarlıdır. |Hayır (komut dosyası kullanıyorsanız) |
| komut dosyasıLinkedService |Komut dosyasını içeren depolamayı veri fabrikasına bağlayan bağlantılı hizmet |Hayır (komut dosyası kullanıyorsanız) |
| betiğini çalıştırın |ScriptPath ve scriptLinkedService belirtmek yerine satır içi komut dosyası belirtin. Örneğin: "script": "CREATE DATABASE testi". |Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| dereceOfParallelism |İşi çalıştırmak için aynı anda kullanılan en fazla düğüm sayısı. |Hayır |
| Öncelik |Önce çalıştırmak için sıraya giren tüm işlerin hangilerinin seçileceğini belirler. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. |Hayır |
| parametreler |U-SQL komut dosyası için parametreler |Hayır |

### <a name="json-example"></a>JSON örneği

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities":
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs":
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi için [Bkz. Veri Gölü Analytics U-SQL Etkinliği.](data-factory-usql-activity.md)

## <a name="stored-procedure-activity"></a>Saklı Yordam Etkinliği
Saklı Yordam Etkinliği JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinlik için tür özelliği olmalıdır: **SqlServerStoredProcedure**. Aşağıdaki bağlantılı hizmetlerden birini oluşturmanız ve bağlantılı Hizmetin adını bağlantılı **ServiceName** özelliği için bir değer olarak belirtmeniz gerekir:

- SQL Server
- Azure SQL Veritabanı
- Azure SQL Veri Ambarı

Etkinlik türünü SqlServerStoredProcedure olarak ayarladığınızda **typeProperties** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| depolanmışProcedureName |Azure SQL veritabanında veya çıktı tablosunun kullandığı bağlantılı hizmet tarafından temsil edilen Azure SQL Veri Ambarı'nda depolanan yordamın adını belirtin. |Evet |
| depolanmışProsedürParametreleri |Depolanan yordam parametreleri için değerleri belirtin. Bir parametre için null geçmeniz gerekiyorsa, sözdizimini kullanın: "param1": null (tüm küçük harf). Bu özelliği kullanma hakkında bilgi edinmek için aşağıdaki örneğe bakın. |Hayır |

Bir giriş veri kümesi belirtirseniz, depolanan yordam etkinliğinin çalışması için kullanılabilir ('Hazır' durumunda) olmalıdır. Giriş veri kümesi, depolanan yordamda parametre olarak tüketilemez. Yalnızca depolanan yordam etkinliğini başlatmadan önce bağımlılığı denetlemek için kullanılır. Depolanan yordam etkinliği için bir çıktı veri kümesi belirtmeniz gerekir.

Çıktı veri kümesi, depolanan yordam etkinliğinin (saatlik, haftalık, aylık vb.) **zamanlamasını** belirtir. Çıktı veri kümesi, bir Azure SQL Veritabanı veya Azure SQL Veri Ambarı veya depolanan yordamın çalışmasını istediğiniz bir SQL Server Veritabanı'na atıfta bulunan bağlantılı bir **hizmet** kullanmalıdır. Çıktı veri kümesi, ardışık işlem için depolanan yordamın sonucunu, ardışık ardışık işlemde[(zincirleme faaliyetler)](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline)aktarmanın bir yolu olarak kullanılabilir. Ancak, Veri Fabrikası bu veri kümesine depolanan yordamın çıktısını otomatik olarak yazmaz. Çıktı veri kümesinin işaret ettiği bir SQL tablosuna yazan depolanan yordamdır. Bazı durumlarda, çıktı veri kümesi yalnızca depolanan yordam etkinliğini çalıştırmak için zamanlamayı belirtmek için kullanılan sahte bir **veri kümesi**olabilir.

### <a name="json-example"></a>JSON örneği

```json
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
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

Daha fazla bilgi [için, Depolanan Yordam Etkinliği](data-factory-stored-proc-activity.md) makalesine bakın.

## <a name="net-custom-activity"></a>.NET özel etkinliği
Bir .NET özel etkinlik JSON tanımında aşağıdaki özellikleri belirtebilirsiniz. Etkinliğin türü özelliği şu olmalıdır: **DotNetActivity**. Azure HDInsight bağlantılı bir hizmet veya Azure Toplu Iş Bağlantılı bir hizmet oluşturmanız ve bağlı hizmetin adını **bağlantılı ServiceName** özelliği için bir değer olarak belirtmeniz gerekir. Etkinlik türünü DotNetActivity olarak ayarladığınızda, aşağıdaki özellikler **typeProperties** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| Assemblyname | Derlemenin adı. Örnekte, o: **MyDotnetActivity.dll**. | Evet |
| EntryPoint |IDotNetActivity arabirimini uygulayan sınıfın adı. Örnekte, öyledir: **MyDotNetActivityNS.MyDotNetActivity** nerede MyDotNetActivitynS namespace ve MyDotNetActivity sınıftır.  | Evet |
| PackageLinkedService | Özel etkinlik zip dosyasını içeren blob depolamasını işaret eden Azure Depolama bağlantılı hizmetin adı. Örnekte şudur: **AzureStorageLinkedService**.| Evet |
| Paket Dosyası | Zip dosyasının adı. Örnekte, öyle: **customactivitycontainer/MyDotNetActivity.zip**. | Evet |
| genişletilmiş Özellikler | Tanımlayabileceğiniz ve .NET koduna aktarabileceğiniz genişletilmiş özellikler. Bu örnekte, SliceStart değişkeni **SliceStart** sistem değişkenini temel alan bir değere ayarlanır. | Hayır |

### <a name="json-example"></a>JSON örneği

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Ayrıntılı bilgi için [bkz.](data-factory-use-custom-activities.md)

## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki öğreticilere bakın:

- [Öğretici: kopyalama etkinliği olan bir ardışık hatlar oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Öğretici: kovan etkinliği olan bir boru hattı oluşturmak](data-factory-build-your-first-pipeline.md)
