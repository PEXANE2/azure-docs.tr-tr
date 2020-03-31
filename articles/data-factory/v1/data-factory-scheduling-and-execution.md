---
title: Veri Fabrikası ile Zamanlama ve Yürütme
description: Azure Veri Fabrikası uygulama modelinin zamanlama ve yürütme yönlerini öğrenin.
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
ms.openlocfilehash: 15a2d6ae5d8b80468ffcdd00d60b1f36843ed677
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281073"
---
# <a name="data-factory-scheduling-and-execution"></a>Veri Fabrikası zamanlama ve yürütme
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [bkz.](../concepts-pipeline-execution-triggers.md)

Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. Bu makalede, etkinlik, ardışık hatlar, bağlantılı hizmetler ve veri kümeleri de dahil olmak üzere Veri Fabrikası uygulama modeli kavramlarının temellerini anladığınızı varsayar. Azure Veri Fabrikası'nın temel kavramları için aşağıdaki makalelere bakın:

* [Data Factory'ye Giriş](data-factory-introduction.md)
* [İşlem hatları](data-factory-create-pipelines.md)
* [Veri kümeleri](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a>Boru hattının başlangıç ve bitiş saatleri
Bir ardışık ardışık hat, yalnızca **başlangıç** zamanı ile **bitiş** saati arasında etkindir. Başlangıç saatinden önce veya bitiş saatinden sonra yürütülmez. Ardışık hatlar duraklatılırsa, başlangıç ve bitiş saatine bakılmaksızın yürütülmez. Bir boru hattının çalışması için duraklatmamalıdır. Bu ayarları (başlangıç, bitiş, duraklatılmış) ardışık hatlar tanımında bulursunuz: 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

Bu özellikler hakkında daha fazla bilgi için [bkz.](data-factory-create-pipelines.md) 


## <a name="specify-schedule-for-an-activity"></a>Bir etkinlik için zamanlama belirtin
Yürütülen boru hattı değildir. Bu, boru hattının genel bağlamında yürütülen boru hattındaki faaliyetlerdir. JSON etkinliğinin **zamanlayıcı** bölümünü kullanarak bir etkinlik için yinelenen bir zamanlama belirtebilirsiniz. Örneğin, bir etkinliği saatlik olarak çalışacak şekilde zamanlayabilirsiniz:  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

Aşağıdaki diyagramda gösterildiği gibi, bir etkinlik için bir zamanlama belirterek, ardışık hatlar başlangıç ve bitiş saatlerinde bir dizi yuvarlanan pencere oluşturur. Yuvarlanan pencereler, çakışmayan, bitişik zaman aralıkları olan bir dizi sabit boyutlu penceredir. Bir etkinlik için bu mantıksal yuvarlanan **pencerelere etkinlik pencereleri**denir.

![Etkinlik zamanlayıcısı örneği](media/data-factory-scheduling-and-execution/scheduler-example.png)

Bir etkinliğin **zamanlayıcı** özelliği isteğe bağlıdır. Bu özelliği belirtirseniz, etkinlik için çıktı veri kümesinin tanımında belirttiğiniz cadence eşleşmesi gerekir. Şu anda zamanlamayı çıktı veri kümesi yürütmektedir. Bu nedenle, etkinlik herhangi bir çıktı üretmese bile bir çıktı veri kümesi oluşturmanız gerekir. 

## <a name="specify-schedule-for-a-dataset"></a>Veri kümesi için zamanlama belirtin
Veri Fabrikası ardışık bir etkinlikte sıfır veya daha fazla giriş **veri kümesi** alabilir ve bir veya daha fazla çıktı veri kümesi üretebilir. Bir etkinlik için, veri kümesi tanımlarında **kullanılabilirlik** bölümü kullanılarak giriş verilerinin kullanılabilir olduğu veya çıktı verilerinin üretildiği cadence'i belirtebilirsiniz. 

**Kullanılabilirlik** bölümündeki **frekans** zaman birimini belirtir. Sıklık için izin verilen değerler şunlardır: Dakika, Saat, Gün, Hafta ve Ay. Kullanılabilirlik bölümündeki **aralık** özelliği, sıklık çarpanı belirtir. Örneğin: bir çıktı veri kümesi için frekans Gün olarak ayarlanırsa ve aralık 1 olarak ayarlanırsa, çıktı verileri günlük olarak üretilir. Sıklığı dakika olarak belirtirseniz, aralığı en az 15 olarak ayarlamanızı öneririz. 

Aşağıdaki örnekte, giriş verileri saatlik olarak kullanılabilir ve çıktı verileri`"frequency": "Hour", "interval": 1`saatlik olarak üretilir ( ). 

**Giriş veri seti:** 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


**Çıktı veri kümesi**

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Şu anda, **çıktı veri kümesi zamanlamayı yönlendirir.** Başka bir deyişle, çıktı veri kümesi için belirtilen zamanlama çalışma zamanında bir etkinliği çalıştırmak için kullanılır. Bu nedenle, etkinlik herhangi bir çıktı üretmese bile bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. 

Aşağıdaki ardışık hatlar tanımında, **zamanlayıcı** özelliği etkinlik için zamanlama belirtmek için kullanılır. Bu özellik isteğe bağlıdır. Şu anda, etkinlik için zamanlama çıktı veri kümesi için belirtilen zamanlama eşleşmesi gerekir.
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

Bu örnekte, etkinlik ardışık hattın başlangıç ve bitiş saatleri arasında saatlik çalışır. Çıktı verileri üç saatlik pencereler için saatlik olarak üretilir (8 - 9, 9 - 10 ve 10 - 11 AM). 

Bir etkinlik çalışması tarafından tüketilen veya üretilen her veri birimine **veri dilimi**denir. Aşağıdaki diyagram, bir giriş veri kümesi ve bir çıktı veri kümesi ile bir etkinlik örneği gösterir: 

![Kullanılabilirlik zamanlayıcısı](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Diyagram, giriş ve çıktı veri kümesi için saatlik veri dilimlerini gösterir. Diyagram, işleme hazır üç giriş dilimini gösterir. 10-11 etkinliği devam ediyor, 10-11 çıkış dilimi üreten. 

Değişkenleri kullanarak veri kümesi JSON'daki geçerli dilimle ilişkili zaman aralığına erişebilirsiniz: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) ve [SliceEnd.](data-factory-functions-variables.md#data-factory-system-variables) Benzer şekilde, WindowStart ve WindowEnd'i kullanarak etkinlik penceresiyle ilişkili zaman aralığına erişebilirsiniz. Bir etkinliğin zamanlaması, etkinlik için çıktı veri kümesinin zamanlamasıyla eşleşmelidir. Bu nedenle, SliceStart ve SliceEnd değerleri sırasıyla WindowStart ve WindowEnd değerleriyle aynıdır. Bu değişkenler hakkında daha fazla bilgi için [Bkz. Veri Fabrikası işlevleri ve sistem değişkenleri](data-factory-functions-variables.md#data-factory-system-variables) makaleleri.  

Bu değişkenleri etkinlik JSON'unuzda farklı amaçlar için kullanabilirsiniz. Örneğin, bunları zaman serisi verilerini temsil eden giriş ve çıktı veri kümelerinden (örneğin: 8 ila 9 AM) veri seçmek için kullanabilirsiniz. Bu örnek, bir etkinlik çalışması için ilgili verileri seçmek ve uygun **folderPath**ile bir blob kopyalamak için **WindowStart** ve **WindowEnd'i** de kullanır. **FolderPath** her saat için ayrı bir klasör olacak şekilde parametreye işlenir.  

Önceki örnekte, giriş ve çıktı veri kümeleri için belirtilen zamanlama aynıdır (saatlik). Etkinlik için giriş veri kümesi farklı bir frekansta kullanılabilirse, örneğin her 15 dakikada bir, bu çıktı veri kümesini oluşturan etkinlik, etkinlik zamanlamasını yönlendiren şey çıktı veri kümesi olduğu için hala saatte bir çalışır. Daha fazla bilgi için farklı [frekanslara sahip Model veri kümeleri'ne](#model-datasets-with-different-frequencies)bakın.

## <a name="dataset-availability-and-policies"></a>Dataset kullanılabilirliği ve ilkeleri
Dataset tanımının kullanılabilirlik bölümünde sıklık ve aralık özelliklerinin kullanımını gördünüz. Bir etkinliğin zamanlamasını ve yürütülmesini etkileyen birkaç özellik daha vardır. 

### <a name="dataset-availability"></a>Dataset kullanılabilirliği 
Aşağıdaki tabloda **kullanılabilirlik** bölümünde kullanabileceğiniz özellikler açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilimi üretimi için zaman birimini belirtir.<br/><br/><b>Desteklenen frekans</b>: Dakika, Saat, Gün, Hafta, Ay |Evet |NA |
| interval |Frekans için çarpan belirtir<br/><br/>"Frekans x aralığı" dilimin ne sıklıkta üretildiğini belirler.<br/><br/>Veri kümesinin saatlik olarak dilimlemeye ihtiyacı varsa, <b>Sıklık</b> <b>saate</b>ve <b>1</b>aralık <b>1'e</b> ayarlarsınız.<br/><br/><b>Not</b>: Sıklığı Dakika olarak belirtirseniz, aralığı en az 15 olarak ayarlamanızı öneririz |Evet |NA |
|  stili |Dilimin aralığın başında/sonunda üretilip üretilmeyeceğini belirtir.<ul><li>Başlangıç Aralığı</li><li>EndofInterval</li></ul><br/><br/>Sıklık Ay olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim ayın son gününde üretilir. Stil StartOfInterval olarak ayarlanmışsa, dilim ayın ilk gününde üretilir.<br/><br/>Sıklık Gün olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim günün son saatinde üretilir.<br/><br/>Sıklık Saat olarak ayarlanırsa ve stil EndOfInterval olarak ayarlanırsa, dilim saatin sonunda üretilir. Örneğin, 13:00 - 14:00 dönemi için bir dilim için, dilim 14:00'te üretilir. |Hayır |EndofInterval |
| çapaDateTime |Veri kümesi dilimi sınırlarını hesaplamak için zamanlayıcı tarafından kullanılan zamandaki mutlak konumu tanımlar. <br/><br/><b>Not</b>: AnchorDateTime'da frekanstan daha parçalı tarih parçaları varsa, daha parçalı parçalar yoksayılır. <br/><br/>Örneğin, <b>aralık</b> <b>saatlikse</b> (sıklık: saat ve aralık: 1) ve <b>AnchorDateTime</b> <b>dakika ve saniye</b>içeriyorsa, AnchorDateTime'ın dakika ve <b>saniye</b> bölümleri yoksayılır. |Hayır |01/01/0001 |
| uzaklık |Tüm veri kümesi dilimlerinin başlangıç ve bitişinin kaydırıldığı zaman sonu. <br/><br/><b>Not</b>: Her iki anchorDateTime ve ofset belirtilirse, sonuç birleşik kaydırmadır. |Hayır |NA |

### <a name="offset-example"></a>mahsup örneği
Varsayılan olarak,`"frequency": "Day", "interval": 1`günlük ( ) dilimler UTC saat12'de (gece yarısı) başlar. Başlangıç saatinin 6 UTC zamanı olmasını istiyorsanız, ofset'i aşağıdaki parçacıkta gösterildiği gibi ayarlayın: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesi her 23 saatte bir üretilir. İlk dilim, `2017-04-19T08:00:00` (UTC zamanı) olarak ayarlanan anchorDateTime tarafından belirtilen anda başlar.

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a>ofset/stil Örneği
Aşağıdaki veri kümesi aylık bir veri kümesidir ve her ayın 3'ünde`3.08:00:00`08:00 'de ():

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a>Dataset ilkesi
Bir veri kümesi, bir dilim yürütme tarafından oluşturulan verilerin tüketime hazır olmadan önce nasıl doğrulanabileceğini belirten bir doğrulama ilkesi tanımlanabilir. Bu gibi durumlarda, dilim yürütme tamamlandıktan sonra, çıkış dilimi durumu **Doğrulama**alt durumu ile **Bekleme** olarak değiştirilir. Dilimler doğrulandıktan sonra dilim durumu **Hazır**olarak değişir. Bir veri dilimi üretildiyse ancak doğrulamayı geçemediyse, bu dilime bağlı olan akış aşağı dilimleri için etkinlik çalışır işlenmez. [İzleme ve yönetme boru hatları,](data-factory-monitor-manage-pipelines.md) Veri Fabrikası'ndaki çeşitli veri dilimleri durumlarını kapsar.

Dataset tanımındaki **ilke** bölümü, veri kümesi dilimlerinin yerine getirmesi gereken ölçütleri veya koşulu tanımlar. Aşağıdaki tabloda **ilke** bölümünde kullanabileceğiniz özellikler açıklanmaktadır:

| İlke Adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB | **Azure blob'undaki** verilerin minimum boyut gereksinimlerini (megabaytlarda) karşıladığını doğrular. |Azure Blob |Hayır |NA |
| minimum Satırlar | **Azure SQL veritabanındaki** veya **Azure tablosundaki** verilerin en az satır sayısını içerdiğini doğrular. |<ul><li>Azure SQL Veritabanı</li><li>Azure Tablosu</li></ul> |Hayır |NA |

#### <a name="examples"></a>Örnekler
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimum Satırlar**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

Bu özellikler ve örnekler hakkında daha fazla bilgi için [bkz.](data-factory-create-datasets.md) 

## <a name="activity-policies"></a>Etkinlik ilkeleri
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

Daha fazla bilgi için [Bkz. Pipelines](data-factory-create-pipelines.md) makalesine bakın. 

## <a name="parallel-processing-of-data-slices"></a>Veri dilimlerinin paralel işleme
Geçmişte boru hattının başlangıç tarihini ayarlayabilirsiniz. Bunu yaptığınızda, Veri Fabrikası geçmişteki tüm veri dilimlerini otomatik olarak hesaplar (geri doldurur) ve bunları işlemeye başlar. Örneğin: başlangıç tarihi 2017-04-01 olan ve geçerli tarihi 2017-04-10 olan bir ardışık hat lar oluşturursanız. Çıktı veri kümesinin cadence günlük ise, daha sonra Veri Fabrikası başlangıç tarihi geçmişte olduğu için hemen 2017-04-01 için 2017-04-09 tüm dilimleri işlemeye başlar. Kullanılabilirlik bölümündeki stil özelliğinin değeri varsayılan olarak EndOfInterval olduğundan, 2017-04-10'daki dilim henüz işlenmedi. YürütmePriorityOrder varsayılan değeri OldestFirst olduğu gibi en eski dilim ilk işlenir. Stil özelliğinin açıklaması için [veri kümesi kullanılabilirlik](#dataset-availability) bölümüne bakın. YürütmePriorityOrder bölümünün açıklaması için [etkinlik ilkeleri](#activity-policies) bölümüne bakın. 

JSON etkinliğinin **ilke** bölümünde **eşzamanlılık** özelliğini ayarlayarak, arka dolgulu veri dilimlerini paralel olarak işlenecek şekilde yapılandırabilirsiniz. Bu özellik, farklı dilimlerde gerçekleşebilecek paralel etkinlik yürütmelerinin sayısını belirler. Eşzamanlılık özelliği için varsayılan değer 1'dir. Bu nedenle, bir dilim varsayılan olarak bir defada işlenir. Maksimum değer 10'dur. Bir ardışık işlem biriminin kullanılabilir büyük bir veri kümesinden geçmesi gerektiğinde, daha büyük bir eşzamanlılık değerine sahip olmak veri işlemeyi hızlandırRın. 

## <a name="rerun-a-failed-data-slice"></a>Başarısız bir veri dilimini yeniden çalıştırma
Bir veri dilimini işlerken bir hata oluştuğunda, Azure portal bıçaklarını veya Uygulamayı İzle ve Yönet'i kullanarak bir dilimin işlenmesinin neden başarısız olduğunu öğrenebilirsiniz. Ayrıntılar için Azure portal bıçaklarını veya [İzleme ve Yönetim uygulamasını](data-factory-monitor-manage-app.md) [kullanarak boru hatlarını izleme](data-factory-monitor-manage-pipelines.md) ve yönetme bilgisine bakın.

İki etkinlik gösteren aşağıdaki örneği göz önünde bulundurun. Etkinlik1 ve Etkinlik 2. Activity1, Dataset1'in bir dilimini tüketir ve Son Veri Kümesi'nin bir dilimini oluşturmak için Activity2 tarafından girdi olarak tüketilen bir veri kümesi2 dilimi üretir.

![Başarısız dilim](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diyagram, son üç dilimin içinde Dataset2 için 9-10 dilimini üreten bir hata olduğunu gösterir. Veri Fabrikası, zaman serisi veri kümesine olan bağımlılığı otomatik olarak izler. Sonuç olarak, 9-10 aşağı dilim için etkinlik çalışmasını başlatmaz.

Veri Fabrikası izleme ve yönetim araçları, sorunun temel nedenini kolayca bulmak ve düzeltmek için başarısız dilimin tanı günlüklerini delmenize olanak sağlar. Sorunu giderdikten sonra, başarısız olan dilimi oluşturmak için etkinlik çalışmasını kolayca başlatabilirsiniz. Veri dilimleri için durum geçişlerini yeniden çalıştırma ve anlama hakkında daha fazla bilgi için, Azure portal bıçaklarını veya İzleme ve [Yönetim uygulamasını](data-factory-monitor-manage-app.md)kullanarak ardışık hatları izleme ve [yönetme'ye](data-factory-monitor-manage-pipelines.md) bakın.

**Dataset2**için 9-10 dilimini yeniden çalıştırdıktan sonra, Veri Fabrikası son veri kümesindeki 9-10 bağımlı dilimin çalışmasını başlatır.

![Başarısız dilimin yeniden çalıştırılması](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a>Bir işlem hattında birden çok etkinlik
Bir işlem hattında birden fazla etkinliğiniz olabilir. Bir ardışık ardışık alanda birden çok etkinliğiniz varsa ve bir etkinliğin çıktısı başka bir etkinliğin girişi değilse, etkinlikler için giriş veri dilimleri hazırsa, etkinlikler paralel olarak çalışabilir.

Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Etkinlikler aynı ardışık boru hattında veya farklı boru hatlarında olabilir. İkinci etkinlik yalnızca ilki başarıyla bittiğinde yürütülür.

Örneğin, bir ardışık boru hattının iki faaliyeti olduğu aşağıdaki örneği göz önünde bulundurun:

1. D1 dış giriş veri seti gerektiren ve çıkış veri kümesi D2 üreten Etkinlik A1.
2. D2 veri kümesinden giriş gerektiren ve çıktı veri kümesi D3 üreten Etkinlik A2.

Bu senaryoda, A1 ve A2 etkinlikleri aynı ardışık ardışık ardışık lıktadır. A1 etkinliği, dış veriler kullanılabilir olduğunda ve zamanlanan kullanılabilirlik sıklığına ulaşıldığında çalışır. D2'den zamanlanan dilimler kullanılabilir olduğunda ve zamanlanan kullanılabilirlik sıklığına ulaşıldığında A2 etkinliği çalışır. D2 veri kümesindeki dilimlerden birinde bir hata varsa, Kullanılabilir hale gelene kadar A2 bu dilim için çalışmaz.

Aynı ardışık ardışık alanda her iki etkinlik le Diyagram görünümü aşağıdaki diyagram gibi görünür:

![Aynı boru hattında zincirleme faaliyetleri](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

Daha önce de belirtildiği gibi, faaliyetleri farklı boru hatları nda olabilir. Böyle bir senaryoda, diyagram görünümü aşağıdaki diyagram gibi görünür:

![İki boru hattında zincirleme faaliyetleri](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Bir örnek için ekteki kopya bölümüne bakın.

## <a name="model-datasets-with-different-frequencies"></a>Farklı frekanslara sahip veri kümelerini modelleme
Örneklerde, giriş ve çıktı veri kümeleri ve etkinlik zamanlama penceresi için frekanslar aynıydı. Bazı senaryolar, bir veya daha fazla girişin frekanslarından farklı bir frekansta çıktı üretme özelliği gerektirir. Veri Fabrikası bu senaryoları modellemeyi destekler.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Örnek 1: Her saat kullanılabilen giriş verileri için günlük çıktı raporu oluşturma
Azure Blob depolama alanında her saat başı kullanılabilen sensörlerden giriş ölçüm verilerine sahip olduğunuz bir senaryo düşünün. [Veri Fabrikası kovan etkinliği](data-factory-hive-activity.md)ile gün için ortalama, maksimum ve minimum gibi istatistikler içeren bir günlük toplu rapor oluşturmak istiyorsunuz.

Veri Fabrikası ile bu senaryoyu şu şekilde modelleyebilirsiniz:

**Giriş veri kümesi**

Saatlik giriş dosyaları, verilen gün için klasöre bırakılır. Giriş için kullanılabilirlik **Saat** olarak ayarlanır (sıklık: Saat, aralık: 1).

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Çıktı veri kümesi**

Günün klasöründe her gün bir çıktı dosyası oluşturulur. Çıktının kullanılabilirliği **Gün** olarak ayarlanır (sıklık: Gün ve aralık: 1).

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Etkinlik: Bir boru hattında kovan aktivitesi**

Kovan komut dosyası, aşağıdaki parçacıkta gösterildiği gibi **WindowStart** değişkenini kullanan parametreler olarak uygun *DateTime* bilgilerini alır. Kovan komut dosyası, verileri gün için doğru klasörden yüklemek ve çıktıoluşturmak için toplamayı çalıştırmak için bu değişkeni kullanır.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
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
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
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

Aşağıdaki diyagram, senaryoyu veri bağımlılığı açısından gösterir.

![Veri bağımlılığı](./media/data-factory-scheduling-and-execution/data-dependency.png)

Her gün için çıktı dilimi, giriş veri kümesinden gelen 24 saatlik dilime bağlıdır. Veri Fabrikası, üretilecek çıktı dilimiyle aynı zaman diliminde düşen giriş veri dilimlerini hesaplayarak bu bağımlılıkları otomatik olarak hesaplar. 24 giriş diliminden herhangi biri kullanılamıyorsa, Veri Fabrikası günlük etkinlik çalışmasına başlamadan önce giriş diliminin hazır olmasını bekler.

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Örnek 2: İfadeler ve Veri Fabrikası işlevleriile bağımlılık belirtin
Başka bir senaryo düşünelim. İki giriş veri kümesini işleyen bir kovan etkinliğiniz olduğunu varsayalım. Bunlardan biri her gün yeni verilere sahip, ama bir tanesi her hafta yeni veri alır. İki giriş arasında birleştirme yapmak ve her gün bir çıktı üretmek istediğinizi varsayalım.

Veri Fabrikası'nın çıktı veri diliminin zaman dilimine hizalayarak işlemek için doğru giriş dilimlerini otomatik olarak oluşturduğu basit yaklaşım çalışmaz.

Her etkinlik çalışması için Veri Fabrikası'nın haftalık giriş veri kümesi için geçen haftanın veri dilimini kullanması gerektiğini belirtmeniz gerekir. Bu davranışı uygulamak için aşağıdaki parçacıkta gösterildiği gibi Azure Veri Fabrikası işlevlerini kullanırsınız.

**Giriş1: Azure blob**

İlk giriş, her gün güncellenen Azure blob'udur.

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Giriş2: Azure blob**

Input2, haftalık olarak güncellenen Azure blob'udur.

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

**Çıktı: Azure blob**

Gün için klasörde her gün bir çıktı dosyası oluşturulur. Çıktının kullanılabilirliği **güne** ayarlanır (sıklık: Gün, aralık: 1).

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Etkinlik: Bir boru hattında kovan aktivitesi**

Kovan etkinliği iki girişi alır ve her gün bir çıkış dilimi üretir. Haftalık giriş için önceki haftanın giriş dilimine bağlı olarak her günün çıktı dilimini aşağıdaki gibi belirtebilirsiniz.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
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

Data Factory'nin desteklediği işlevler ve sistem değişkenlerinin listesi için [Veri Fabrikası işlevlerine ve sistem değişkenlerine](data-factory-functions-variables.md) bakın.

## <a name="appendix"></a>Ek

### <a name="example-copy-sequentially"></a>Örnek: sırayla kopyalayın
Birden çok kopyalama işlemi sıralı/sıralı bir şekilde birbiri ardına çalıştırmak mümkündür. Örneğin, aşağıdaki giriş veri çıktısı veri kümelerini içeren bir ardışık alanda iki kopyalama etkinliğiniz (CopyActivity1 ve CopyActivity2) olabilir:   

KopyalamaEtkinliği1

Giriş: Dataset. Çıktı: Dataset2.

KopyalamaEtkinliği2

Giriş: Dataset2.  Çıktı: Dataset3.

CopyActivity2 yalnızca CopyActivity1 başarılı bir şekilde çalıştırılırsa ve Dataset2 kullanılabilirse çalışır.

Burada örnek boru hattı JSON:

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
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
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

Örnekte, ilk kopyalama etkinliğinin çıktı veri kümesinin (Dataset2) ikinci etkinlik için giriş olarak belirtildiğine dikkat edin. Bu nedenle, ikinci etkinlik yalnızca ilk etkinlikten çıktı veri kümesi hazır olduğunda çalışır.  

Örnekte, CopyActivity2'nin Dataset3 gibi farklı bir girişi olabilir, ancak Dataset2'yi CopyActivity2'ye giriş olarak belirtirsiniz, bu nedenle CopyActivity1 bitene kadar etkinlik çalışmaz. Örnek:

KopyalamaEtkinliği1

Giriş: Dataset1. Çıktı: Dataset2.

KopyalamaEtkinliği2

Girişler: Dataset3, Dataset2. Çıktı: Dataset4.

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
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
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

Örnekte, ikinci kopyalama etkinliği için iki giriş veri kümesinin belirtildiğine dikkat edin. Birden çok giriş belirtildiğinde, verileri kopyalamak için yalnızca ilk giriş veri kümesi kullanılır, ancak diğer veri kümeleri bağımlılık olarak kullanılır. CopyActivity2 yalnızca aşağıdaki koşullar yerine getirildiğinde başlar:

* CopyActivity1 başarıyla tamamlandı ve Dataset2 kullanılabilir. Bu veri kümesi, verileri Dataset4'e kopyalarken kullanılmaz. Yalnızca CopyActivity2 için bir zamanlama bağımlılığı görevi görür.   
* Dataset3 kullanılabilir. Bu veri kümesi, hedefe kopyalanan verileri temsil eder. 
