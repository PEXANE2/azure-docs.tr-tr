---
title: Veri Fabrikası Fonksiyonları ve Sistem Değişkenleri
description: Azure Veri Fabrikası işlevlerinin ve sistem değişkenlerinin bir listesini sağlar
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 9acc369e24d1bac92dea3fb6ae391a410e5f6c3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73667661"
---
# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Veri Fabrikası - Fonksiyonlar ve Sistem Değişkenleri
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Sistem değişkenlerine](../control-flow-system-variables.md)bakın.

Bu makalede, Azure Veri Fabrikası tarafından desteklenen işlevler ve değişkenler hakkında bilgi verilmektedir.

## <a name="data-factory-system-variables"></a>Veri Fabrikası sistem değişkenleri

| Değişken Adı | Açıklama | Nesne Kapsamı | JSON Kapsam ve Kullanım Örnekleri |
| --- | --- | --- | --- |
| Pencere Başlangıcı |Geçerli etkinlik çalıştırma penceresi için zaman aralığının başlangıcı |etkinlik |<ol><li>Veri seçim sorgularını belirtin. [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesinde başvurulan bağlayıcı makalelere bakın.</li> |
| Pencere Sonu |Geçerli etkinlik çalıştırma penceresi için zaman sonu aralığı |etkinlik |WindowStart ile aynıdır. |
| SliceStart |Veri dilimi nin üretildiği zaman aralığının başlangıcı |etkinlik<br/>Dataset |<ol><li>[Azure Blob](data-factory-azure-blob-connector.md) ve [Dosya Sistemi veri kümeleriyle](data-factory-onprem-file-system-connector.md)çalışırken dinamik klasör yollarını ve dosya adlarını belirtin.</li><li>Etkinlik girişleri koleksiyonunda veri fabrikası işlevleriyle giriş bağımlılıklarını belirtin.</li></ol> |
| DilimUcu |Geçerli veri dilimi için zaman sonu aralığı. |etkinlik<br/>Dataset |SliceStart ile aynıdır. |

> [!NOTE]
> Şu anda veri fabrikası, etkinlikte belirtilen zamanlamanın çıktı veri kümesinin kullanılabilirliğinde belirtilen zamanlamayla tam olarak eşleşmesini gerektirir. Bu nedenle, WindowStart, WindowEnd ve SliceStart ve SliceEnd her zaman aynı zaman dilimi ve tek bir çıkış dilimi eşleme.
> 

### <a name="example-for-using-a-system-variable"></a>Bir sistem değişkeni kullanmak için örnek
Aşağıdaki örnekte, **SliceStart'ın** yılı, ayı, günü ve saati **folderPath** ve **fileName** özellikleri tarafından kullanılan ayrı değişkenlere ayıklanır.

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

## <a name="data-factory-functions"></a>Veri Fabrikası fonksiyonları
Aşağıdaki amaçlar için sistem değişkenleri ile birlikte veri fabrikasında işlevleri kullanabilirsiniz:

1. Veri seçim sorgularını belirtme [(Bkz. Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesinde başvurulan bağlayıcı makaleler.
   
   Veri fabrikası işlevini çağırmak için sözdizimi şudur: ** $$ \<** etkinlik ve veri kümelerinde veri seçimi sorguları ve diğer özellikler için>işlev.  
2. Etkinlik girdileri koleksiyonunda veri fabrikası işlevleri ile giriş bağımlılıkları belirtilmesi.
   
    Giriş bağımlılık ifadelerini belirtmek için $$ gerekmez.     

Aşağıdaki örnekte, JSON dosyasındaki **sqlReaderQuery** özelliği işlev tarafından `Text.Format` döndürülen bir değere atanır. Bu örnek, etkinlik çalıştırma penceresinin başlangıç saatini temsil eden **WindowStart**adlı bir sistem değişkeni de kullanır.

```json
{
    "Type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
}
```

Kullanabileceğiniz farklı biçimlendirme seçeneklerini açıklayan [Özel Tarih ve Saat Biçim Dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) konusuna bakın (örneğin: ay vs. yyyy). 

### <a name="functions"></a>İşlevler
Aşağıdaki tablolar Azure Veri Fabrikası'ndaki tüm işlevleri listelemektedir:

| Kategori | İşlev | Parametreler | Açıklama |
| --- | --- | --- | --- |
| Zaman |Ek Saat(X,Y) |X: DateTime <br/><br/>Y: int |Verilen süreye Y saatlerini ekler X. <br/><br/>Örnek: `9/5/2013 12:00:00 PM + 2 hours = 9/5/2013 2:00:00 PM` |
| Zaman |AddMinutes(X,Y) |X: DateTime <br/><br/>Y: int |X'e Y dakikası ekler.<br/><br/>Örnek: `9/15/2013 12: 00:00 PM + 15 minutes = 9/15/2013 12: 15:00 PM` |
| Zaman |Başlangıç Saati(x) |X: Datetime |X'in saat bileşeni tarafından temsil edilen saatin başlangıç saatini alır. <br/><br/>Örnek: `StartOfHour of 9/15/2013 05: 10:23 PM is 9/15/2013 05: 00:00 PM` |
| Tarih |AddDays(X,Y) |X: DateTime<br/><br/>Y: int |X'e Y gün ekler. <br/><br/>Örnek: 15.09.2013 12:00:00 PM + 2 gün = 17.09.2013 12:00:00<br/><br/>Y'yi negatif sayı olarak belirterek günleri de çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 2 days = 9/13/2013 12:00:00 PM`. |
| Tarih |AddMonths(X,Y) |X: DateTime<br/><br/>Y: int |X'e Y ayı ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 month = 10/15/2013 12:00:00 PM`.<br/><br/>Y'yi negatif sayı olarak belirterek ayları da çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 month = 8/15/2013 12:00:00 PM`.|
| Tarih |AddQuarters(X,Y) |X: DateTime <br/><br/>Y: int |X'e Y * 3 ay ekler.<br/><br/>Örnek: `9/15/2013 12:00:00 PM + 1 quarter = 12/15/2013 12:00:00 PM` |
| Tarih |AddWeeks(X,Y) |X: DateTime<br/><br/>Y: int |X'e Y * 7 gün ekler<br/><br/>Örnek: 15.09.2013 12:00:00 PM + 1 hafta = 22.09.2013 12:00:00<br/><br/>Y'yi negatif sayı olarak belirterek haftaları da çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 week = 9/7/2013 12:00:00 PM`. |
| Tarih |AddYears(X,Y) |X: DateTime<br/><br/>Y: int |X'e Y yılları ekler.<br/><br/>`Example: 9/15/2013 12:00:00 PM + 1 year = 9/15/2014 12:00:00 PM`<br/><br/>Y'yi negatif sayı olarak belirterek yılları da çıkarabilirsiniz.<br/><br/>Örnek: `9/15/2013 12:00:00 PM - 1 year = 9/15/2012 12:00:00 PM`. |
| Tarih |Gün(X) |X: DateTime |X'in gün bileşenini alır.<br/><br/>Örnek: `Day of 9/15/2013 12:00:00 PM is 9`. |
| Tarih |Gün Içi(X) |X: DateTime |X'in hafta bileşeninin gününü alır.<br/><br/>Örnek: `DayOfWeek of 9/15/2013 12:00:00 PM is Sunday`. |
| Tarih |DayOfYear(x) |X: DateTime |X'in yıl bileşeni tarafından temsil edilen yıldaki günü alır.<br/><br/>Örnekler:<br/>`12/1/2015: day 335 of 2015`<br/>`12/31/2015: day 365 of 2015`<br/>`12/31/2016: day 366 of 2016 (Leap Year)` |
| Tarih |DaysInMonth(x) |X: DateTime |X parametresi ay bileşeni tarafından temsil edilen aydaki günleri alır.<br/><br/>Örnek: `DaysInMonth of 9/15/2013 are 30 since there are 30 days in the September month`. |
| Tarih |Endofday(x) |X: DateTime |X'in gün sonunu (gün bileşeni) temsil eden tarih saatini alır.<br/><br/>Örnek: `EndOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 11:59:59 PM`. |
| Tarih |Ay Sonu(x) |X: DateTime |X parametresinin ay bileşeniile temsil edilen ay sonunu alır. <br/><br/>Örnek: `EndOfMonth of 9/15/2013 05:10:23 PM is 9/30/2013 11:59:59 PM` (Eylül ayı sonunu temsil eden tarih saati) |
| Tarih |Başlangıç Günü(x) |X: DateTime |X parametresi gün bileşeni ile temsil edilen günün başlangıcını alır.<br/><br/>Örnek: `StartOfDay of 9/15/2013 05:10:23 PM is 9/15/2013 12:00:00 AM`. |
| DateTime |Gönderen(x) |X: Dize |X dizesini tarih saatine ayrıştın. |
| DateTime |Keneler(X) |X: DateTime |X parametresinin kene özelliğini alır. Bir kene 100 nanosaniye eşittir. Bu özelliğin değeri, 1 Ocak 0001 gece yarısı 12:00:00'den bu yana geçen keneler sayısını temsil eder. |
| Metin |Biçim(X) |X: String değişkeni |Metni biçimlendir `\\'` (karakterden `'` kaçmak için birleşimi kullanın).|

> [!IMPORTANT]
> Başka bir işlev içinde bir işlev kullanırken, iç işlev için önek kullanmanız **$$** gerekmez. Örneğin: \\$$Text.Format('PartitionKey eq\\'my_pkey_filter_value' ve \\RowKey ge '{0: yyyy-MM-dd\\HH:mm:ss} '', Time.AddHours(SliceStart, -6)). Bu örnekte, **$$** önek **Time.AddHours** işlevi için kullanılmaz dikkat edin. 

#### <a name="example"></a>Örnek
Aşağıdaki örnekte, Hive aktivitesi için giriş ve çıkış parametreleri işlev ve SliceStart sistem değişkeni `Text.Format` kullanılarak belirlenir. 

```json  
{
    "name": "HiveActivitySamplePipeline",
        "properties": {
    "activities": [
            {
            "name": "HiveActivitySample",
            "type": "HDInsightHive",
            "inputs": [
                    {
                    "name": "HiveSampleIn"
                    }
            ],
            "outputs": [
                    {
                    "name": "HiveSampleOut"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                }
            }
            }
    ]
    }
}
```

### <a name="example-2"></a>Örnek 2

Aşağıdaki örnekte, Depolanan Yordam Etkinliği için DateTime parametresi Metin kullanılarak belirlenir. Biçim lendirme işlevi ve SliceStart değişkeni. 

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
            "start": "2016-08-02T00:00:00Z",
            "end": "2016-08-02T05:00:00Z",
        "isPaused": false
    }
}
```

### <a name="example-3"></a>Örnek 3
SliceStart tarafından temsil edilen gün yerine önceki güne ait verileri okumak için aşağıdaki örnekte gösterildiği gibi AddDays işlevini kullanın: 

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-01-01T08:00:00",
        "end": "2017-01-01T11:00:00",
        "description": "hive activity",
        "activities": [
            {
                "name": "SampleHiveActivity",
                "inputs": [
                    {
                        "name": "MyAzureBlobInput",
                        "startTime": "Date.AddDays(SliceStart, -1)",
                        "endTime": "Date.AddDays(SliceEnd, -1)"
                    }
                ],
                "outputs": [
                    {
                        "name": "MyAzureBlobOutput"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adftutorial\\hivequery.hql",
                    "scriptLinkedService": "StorageLinkedService",
                    "defines": {
                        "Year": "$$Text.Format('{0:yyyy}',WindowsStart)",
                        "Month": "$$Text.Format('{0:MM}',WindowStart)",
                        "Day": "$$Text.Format('{0:dd}',WindowStart)"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 2,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

Kullanabileceğiniz farklı biçimlendirme seçeneklerini açıklayan [Özel Tarih ve Saat Biçim Dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx) konusuna bakın (örneğin: yy vs. yyyy). 

