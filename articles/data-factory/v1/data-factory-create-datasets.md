---
title: Azure Veri Fabrikası'nda veri kümeleri oluşturma
description: Ofset ve anchorDateTime gibi özellikleri kullanan örneklerle Azure Veri Fabrikası'nda nasıl veri kümeleri oluşturup oluşturabilirsiniz öğrenin.
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
ms.openlocfilehash: 18a5e11d2341fb020fc442d2f9ce7c1d44de9d0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260442"
---
# <a name="datasets-in-azure-data-factory"></a>Azure Data Factory'de veri kümeleri
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-create-datasets.md)
> * [Sürüm 2 (geçerli sürüm)](../concepts-datasets-linked-services.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Veri Kümeleri'ne](../concepts-datasets-linked-services.md)bakın.

Bu makalede, veri kümelerinin ne olduğu, JSON biçiminde nasıl tanımlandığı ve Azure Veri Fabrikası ardışık hatlarında nasıl kullanıldığı açıklanmaktadır. Veri kümesi JSON tanımındaki her bölüm (örneğin, yapı, kullanılabilirlik ve ilke) hakkında ayrıntılı bilgi sağlar. Makale ayrıca bir dataset JSON tanımında **ofset**, **anchorDateTime**ve **stil** özelliklerini kullanmak için örnekler sağlar.

> [!NOTE]
> Veri Fabrikası'nda yeniyseniz, genel bakış için [Azure Veri Fabrikası'na Giriş'e](data-factory-introduction.md) bakın. Veri fabrikaları oluşturma konusunda uygulamalı deneyiminiz yoksa, [veri dönüştürme öğreticisini](data-factory-build-your-first-pipeline.md) ve veri hareketi [öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)okuyarak daha iyi bir anlayış elde edebilirsiniz.

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. **Ardışık işlem,** bir görevi birlikte gerçekleştiren **etkinliklerin** mantıksal bir gruplandırmasIdır. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, şirket içi bir SQL Server'dan Azure Blob depolama alanına veri kopyalamak için bir kopyalama etkinliği kullanabilirsiniz. Ardından, çıktı verileri üretmek için Blob depolamadan gelen verileri işlemek için Azure HDInsight kümesinde Hive komut dosyası çalıştıran bir Kovan etkinliği kullanabilirsiniz. Son olarak, çıktı verilerini Azure SQL Veri Ambarı'na kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz ve bunun üzerine iş zekası (BI) raporlama çözümleri oluşturulur. Boru hatları ve etkinlikler hakkında daha fazla bilgi için [Azure Veri Fabrikası'ndaki Boru Hatları ve etkinliklere](data-factory-create-pipelines.md)bakın.

Bir etkinlik sıfır veya daha fazla giriş **veri kümesi**alabilir ve bir veya daha fazla çıktı veri kümesi üretebilir. Giriş veri kümesi, ardışık ardışık iştihattaki bir etkinliğin girdisini, çıktı veri kümesi ise etkinlik çıktısını temsil eder. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin, bir Azure Blob veri kümesi, ardışık aygıtın verileri okuması gereken Blob depolamasındaki blob kapsayıcısını ve klasörünü belirtir.

Bir veri kümesi oluşturmadan önce, veri deponuzu veri fabrikasına bağlamak için bağlantılı bir **hizmet** oluşturun. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Veri kümeleri, SQL tabloları, dosyalar, klasörler ve belgeler gibi bağlantılı veri depolarında bulunan verileri tanımlar. Örneğin, Azure Depolama bağlantılı bir hizmet, bir depolama hesabını veri fabrikasına bağlar. Azure Blob veri kümesi, blob kapsayıcısını ve işlenecek giriş bloblarını içeren klasörü temsil eder.

İşte örnek bir senaryo. Blob depolamasundaki verileri bir SQL veritabanına kopyalamak için iki bağlantılı hizmet oluşturursunuz: Azure Depolama ve Azure SQL Veritabanı. Ardından, iki veri kümesi oluşturun: Azure Blob veri kümesi (Azure Depolama bağlantılı hizmete başvurur) ve Azure SQL Table veri kümesi (Azure SQL Veritabanı bağlantılı hizmete başvurur). Azure Depolama ve Azure SQL Veritabanı bağlantılı hizmetler, Veri Fabrikası'nın sırasıyla Azure Depolama ve Azure SQL Veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi, Blob depolama alanınızdaki giriş bloblarını içeren blob kapsayıcısını ve blob klasörünü belirtir. Azure SQL Tablo veri kümesi, verilerin kopyalanış olacağı SQL veritabanınızdaki SQL tablosunu belirtir.

Aşağıdaki diyagram, Veri Fabrikası'nda ardışık hatlar, etkinlik, veri kümesi ve bağlantılı hizmet arasındaki ilişkileri gösterir:

![Boru hattı, etkinlik, veri kümesi, bağlantılı hizmetler arasındaki ilişki](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>Veri Seti JSON
Veri Fabrikası'ndaki bir veri kümesi JSON formatında aşağıdaki gibi tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": "<boolean flag to indicate external data. only for input datasets>",
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
| ad |Veri kümesinin adı. Bkz. [Azure Veri Fabrikası - Adlandırma](data-factory-naming-rules.md) kuralları. |Evet |NA |
| type |Veri kümesinin türü. Veri Fabrikası tarafından desteklenen türlerden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için [Bkz. Dataset türü.](#Type) |Evet |NA |
| yapı |Veri kümesinin şeması.<br/><br/>Ayrıntılar için [Dataset yapısına](#Structure)bakın. |Hayır |NA |
| typeProperties | Tür özellikleri her tür için farklıdır (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında ayrıntılı bilgi için [Dataset türüne](#Type)bakın. |Evet |NA |
| external | Bir veri kümesinin açıkça bir veri fabrikası ardışık hattı tarafından üretilip üretilmediğini belirtmek için Boolean bayrağı. Bir etkinliğin giriş veri kümesi geçerli ardışık işlem tarafından üretilmezse, bu bayrağı doğru ayarlayın. Bu bayrağı, ardışık işlemdeki ilk etkinliğin giriş veri kümesi için doğru olarak ayarlayın.  |Hayır |yanlış |
| availability | Veri kümesi üretimi için işleme penceresini (örneğin, saatlik veya günlük) veya dilimleme modelini tanımlar. Bir etkinlik çalışması tarafından tüketilen ve üretilen her veri birimine veri dilimi denir. Çıktı veri kümesinin kullanılabilirliği günlük olarak ayarlanırsa (sıklık - Gün, aralık - 1), günlük bir dilim üretilir. <br/><br/>Ayrıntılar için Dataset kullanılabilirliği'ne bakın. <br/><br/>Veri kümesi dilimleme modeliyle ilgili ayrıntılar için [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) makalesine bakın. |Evet |NA |
| ilke |Veri kümesi dilimlerinin yerine getirmesi gereken ölçütleri veya koşulu tanımlar. <br/><br/>Ayrıntılar için [Dataset ilkesi](#Policy) bölümüne bakın. |Hayır |NA |

## <a name="dataset-example"></a>Dataset örneği
Aşağıdaki örnekte, veri kümesi BIR SQL veritabanında **MyTable** adlı bir tabloyu temsil eder.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Aşağıdaki noktalara dikkat edin:

* **türü** AzureSqlTable olarak ayarlanır.
* **tableName** türü özelliği (AzureSqlTable türüne özgü) MyTable olarak ayarlanır.
* **linkedServiceName,** bir sonraki JSON parçacığında tanımlanan AzureSqlDatabase türünde bağlantılı bir hizmet anlamına gelir.
* **kullanılabilirlik sıklığı** Gün olarak ayarlanır ve **aralık** 1 olarak ayarlanır. Bu, veri kümesi diliminin günlük olarak üretildiği anlamına gelir.

**AzureSqlLinkedService** aşağıdaki gibi tanımlanır:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

Önceki JSON snippet olarak:

* **türü** AzureSqlDatabase olarak ayarlanır.
* **connectionString** türü özelliği, bir SQL veritabanına bağlanmak için bilgileri belirtir.

Gördüğünüz gibi, bağlantılı hizmet bir SQL veritabanına nasıl bağlanılmayı tanımlar. Veri kümesi, bir ardışık ardışık alandaki etkinlik için giriş ve çıktı olarak hangi tablonun kullanıldığını tanımlar.

> [!IMPORTANT]
> Bir veri kümesi boru hattı tarafından üretilmediği sürece, **dış**olarak işaretlenmelidir. Bu ayar genellikle bir ardışık işlemdeki ilk etkinlik girişleri için geçerlidir.

## <a name="dataset-type"></a><a name="Type"></a>Dataset türü
Veri kümesinin türü kullandığınız veri deposuna bağlıdır. Veri Fabrikası tarafından desteklenen veri depolarının listesi için aşağıdaki tabloya bakın. Bağlantılı bir hizmetin ve bu veri deposu için bir veri kümesinin nasıl oluşturulabildiğini öğrenmek için bir veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * içeren veri depoları şirket içinde veya Hizmet olarak Azure altyapısında (IaaS) olabilir. Bu veri [depoları, Veri Yönetimi Ağ Geçidi'ni](data-factory-data-management-gateway.md)yüklemenizi gerektirir.

Önceki bölümdeki örnekte, veri kümesinin türü **AzureSqlTable**olarak ayarlanır. Benzer şekilde, bir Azure Blob veri kümesi için, veri kümesinin türü aşağıdaki JSON'da gösterildiği gibi **AzureBlob**olarak ayarlanır:

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

## <a name="dataset-structure"></a><a name="Structure"></a>Dataset yapısı
**Yapı** bölümü isteğe bağlıdır. Adlar ve veri türleri sütunlarının bir koleksiyon içeren veri kümesinin şema tanımlar. Türleri ve harita sütunlarını kaynaktan hedefe dönüştürmek için kullanılan tür bilgilerini sağlamak için yapı bölümünü kullanırsınız. Aşağıdaki örnekte, veri kümesinin üç `slicetimestamp` `projectname`sütunu `pageviews`vardır: , ve . Bunlar sırasıyla String, String ve Ondalık vardır.

```json
structure:
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Yapıdaki her sütun aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü.  |Hayır |
| kültür |. Tür .NET türü olduğunda kullanılacak NET tabanlı `Datetime` kültür: `Datetimeoffset`veya . Varsayılan değer: `en-us`. |Hayır |
| biçim |Tür bir .NET türü olduğunda kullanılacak biçim `Datetime` `Datetimeoffset`dizesi: veya . |Hayır |

Aşağıdaki yönergeler, yapı bilgilerinin ne zaman ekleyip **yapı** bölümüne ne eklendirileceğini belirlemenize yardımcı olur.

* **Yapılandırılmış veri kaynakları için**yapı bölümünü yalnızca harita kaynak sütunlarının sütunları batırmasını istiyorsanız belirtin ve adları aynı değildir. Bu tür yapılandırılmış veri kaynağı, veri şema larını ve türü bilgileri, verilerin kendisiyle birlikte depolar. Yapılandırılmış veri kaynaklarına örnek olarak SQL Server, Oracle ve Azure tablosu verilebilir.
  
    Tür bilgileri yapılandırılmış veri kaynakları için zaten mevcut olduğundan, yapı bölümünü eklediğinizde tür bilgilerini eklememelisiniz.
* **Okuma veri kaynakları (özellikle Blob depolama) üzerinde şema için,** veri ile herhangi bir şema veya tür bilgi depolamadan veri depolamak için seçebilirsiniz. Bu tür veri kaynakları için, kaynak sütunları batmaya istediğiniz yapıyı ekleyin. Ayrıca, veri kümesi bir kopyalama etkinliği için bir giriş olduğunda yapıyı da içerir ve kaynak veri kümesiveri türlerinin lavabo için yerel türlere dönüştürülmesi gerekir.
    
    Veri Fabrikası yapı da tür bilgisi sağlamak için aşağıdaki değerleri destekler: **Int16, Int32, Int64, Single, Double, Ondalık, Bayt[], Boolean, String, Guid, Datetime, Datetimeoffset ve Timespan.** Bu değerler Ortak Dil Belirtimi (CLS) uyumludur. NET tabanlı tür değerleri.

Veri Fabrikası, verileri bir kaynak veri deposundan lavabo veri deposuna taşınırken tür dönüşümlerini otomatik olarak gerçekleştirir.

## <a name="dataset-availability"></a>Dataset kullanılabilirliği
Veri kümesindeki **kullanılabilirlik** bölümü, veri kümesiiçin işleme penceresini (örneğin, saatlik, günlük veya haftalık) tanımlar. Etkinlik pencereleri hakkında daha fazla bilgi için [Zamanlama ve yürütme'ye](data-factory-scheduling-and-execution.md)bakın.

Aşağıdaki kullanılabilirlik bölümü, çıktı veri kümesinin saatlik olarak üretildiğini veya giriş veri kümesinin saatlik olarak kullanılabildiğini belirtir:

```json
"availability":
{
    "frequency": "Hour",
    "interval": 1
}
```

Ardışık hatlar aşağıdaki başlangıç ve bitiş saatlerine sahipse:

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

Çıktı veri kümesi, ardışık hatlar başlangıç ve bitiş saatleri içinde saatlik olarak üretilir. Bu nedenle, bu boru hattı tarafından üretilen beş veri kümesi dilimi vardır, her etkinlik penceresi için bir tane (12 - 1, 1 - 2, 2 - 3, 3 - 4, 4 - 5 AM).

Aşağıdaki tabloda kullanılabilirlik bölümünde kullanabileceğiniz özellikler açıklanmaktadır:

| Özellik | Açıklama | Gerekli | Varsayılan |
| --- | --- | --- | --- |
| frequency |Veri kümesi dilimi üretimi için zaman birimini belirtir.<br/><br/><b>Desteklenen frekans</b>: Dakika, Saat, Gün, Hafta, Ay |Evet |NA |
| interval |Frekans için çarpan belirtir.<br/><br/>"Frekans x aralığı" dilimin ne sıklıkta üretildiğini belirler. Örneğin, veri kümesinin saatlik olarak dilimletilmesi gerekiyorsa, <b>sıklığı</b> <b>Saat</b>ve <b>aralık</b> <b>1</b>olarak ayarlarsınız.<br/><br/>**Sıklığı** **Dakika**olarak belirtirseniz, aralığı en az 15 olarak ayarlamanız gerektiğini unutmayın. |Evet |NA |
|  stili |Dilimin aralığın başında mı yoksa sonunda mı üretilmesi gerektiğini belirtir.<ul><li>Başlangıç Aralığı</li><li>EndofInterval</li></ul>**Sıklık** **Ay**olarak ayarlanırsa ve **stil** **EndOfInterval**olarak ayarlanırsa, dilim ayın son gününde üretilir. **Stil** **StartOfInterval**olarak ayarlanmışsa, dilim ayın ilk gününde üretilir.<br/><br/>**Sıklık** **Gün**olarak ayarlanırsa ve **stil** **EndOfInterval**olarak ayarlanmışsa, dilim günün son saatinde üretilir.<br/><br/>**Frekans** **Saat**olarak ayarlanırsa ve **stil** **EndOfInterval**olarak ayarlanırsa, dilim saatin sonunda üretilir. Örneğin, 13:00 - 14:00 dönemi için bir dilim için dilim 14:00'te üretilir. |Hayır |EndofInterval |
| çapaDateTime |Veri kümesi dilimi sınırlarını hesaplamak için zamanlayıcı tarafından kullanılan zamandaki mutlak konumu tanımlar. <br/><br/>Bu özellik, belirtilen sıklıktan daha parçalı tarih parçaları varsa, daha taneli parçalar yoksayılır unutmayın. Örneğin, **aralık** **saatlikse** (sıklık: saat ve aralık: 1) ve **anchorDateTime** **dakika ve saniye**içeriyorsa, **anchorDateTime'ın** dakika ve saniye bölümleri yoksayılır. |Hayır |01/01/0001 |
| uzaklık |Tüm veri kümesi dilimlerinin başlangıç ve bitişinin kaydırıldığı zaman sonu. <br/><br/>Her iki **anchorDateTime** ve **ofset** belirtilirse, sonuç kombine kaydırma olduğunu unutmayın. |Hayır |NA |

### <a name="offset-example"></a>mahsup örneği
Varsayılan olarak,`"frequency": "Day", "interval": 1`günlük ( ) dilimler 12 (gece yarısı) Eşgüdümlü Evrensel Saat (UTC) olarak başlar. Başlangıç saatinin 6 UTC zamanı olmasını istiyorsanız, ofset'i aşağıdaki parçacıkta gösterildiği gibi ayarlayın:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>anchorDateTime örneği
Aşağıdaki örnekte, veri kümesi her 23 saatte bir üretilir. İlk dilim **anchorDateTime**tarafından belirtilen anda başlar `2017-04-19T08:00:00` , hangi ayarlanır (UTC).

```json
"availability":
{
    "frequency": "Hour",
    "interval": 23,
    "anchorDateTime":"2017-04-19T08:00:00"
}
```

### <a name="offsetstyle-example"></a>ofset/stil örneği
Aşağıdaki veri kümesi aylıktır ve her ayın 3'ünde 08:00`3.08:00:00`'de ():

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="dataset-policy"></a><a name="Policy"></a>Dataset ilkesi
Veri kümesi tanımındaki **ilke** bölümü, veri kümesi dilimlerinin yerine getirmesi gereken ölçütleri veya koşulu tanımlar.

### <a name="validation-policies"></a>Doğrulama ilkeleri
| İlke adı | Açıklama | Uygulanan | Gerekli | Varsayılan |
| --- | --- | --- | --- | --- |
| minimumSizeMB |**Azure Blob depolama sındaki** verilerin minimum boyut gereksinimlerini (megabaytlarda) karşıladığını doğrular. |Azure Blob depolama |Hayır |NA |
| minimum Satırlar |**Azure SQL veritabanındaki** veya **Azure tablosundaki** verilerin en az satır sayısını içerdiğini doğrular. |<ul><li>Azure SQL veritabanı</li><li>Azure tablosu</li></ul> |Hayır |NA |

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

**minimum Satırlar:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Dış veri kümeleri
Dış veri kümeleri, veri fabrikasında çalışan bir ardışık hatlar tarafından üretilmeyenlerdir. Veri kümesi **dış olarak**işaretlenmişse, veri kümesi dilimi kullanılabilirliğinin davranışını etkilemek için **ExternalData** ilkesi tanımlanabilir.

Veri Kümesi Veri Fabrikası tarafından üretilmediği sürece, **dış.** Bu ayar, etkinlik veya boru hattı zincirleme kullanılmadığı sürece, genellikle bir ardışık işlemdeki ilk etkinliğin girdileri için geçerlidir.

| Adı | Açıklama | Gerekli | Varsayılan değer |
| --- | --- | --- | --- |
| veriGecikme |Verilen dilim için dış verilerin kullanılabilirliği üzerindeki denetimi geciktirme zamanı. Örneğin, bu ayarı kullanarak saatlik denetimi geciktirebilirsiniz.<br/><br/>Ayar yalnızca şimdiki zaman için geçerlidir. Örneğin, şu anda saat 13:00 ise ve bu değer 10 dakikaysa, doğrulama 13:10'da başlar.<br/><br/>Bu ayarın geçmişteki dilimleri etkilemediğini unutmayın. **Slice End Time** + **verisi** < Olan DilimlerGecikme**Şimdi** gecikmeden işlenir.<br/><br/>23:59 saatten büyük saatler `day.hours:minutes:seconds` biçimi kullanılarak belirtilmelidir. Örneğin, 24 saat belirtmek için 24:00:00'ı kullanmayın. Bunun yerine, 1.00:00:00 kullanın. 24:00:00 kullanıyorsanız, 24 gün (24:00:00:00) olarak kabul edilir. 1 gün ve 4 saat için 1:04:00:00 belirtiniz. |Hayır |0 |
| retryInterval |Bir hata ve bir sonraki deneme arasındaki bekleme süresi. Bu ayar şimdiki zaman için geçerlidir. Önceki deneme başarısız olduysa, bir sonraki deneme **retryInterval** döneminden sonradır. <br/><br/>Eğer saat 13:00 ise, ilk denemeye başlıyoruz. İlk doğrulama denetiminin tamamlanma süresi 1 dakika ysa ve işlem başarısız olursa, bir sonraki yeniden deneme 1:00 + 1dk (süre) + 1dk (yeniden deneme aralığı) = 1:02 PM'dir. <br/><br/>Geçmişteki dilimler için gecikme yoktur. Yeniden deneme hemen gerçekleşir. |Hayır |00:01:00 (1 dakika) |
| yeniden denemeTimeout |Her yeniden deneme denemesi için zaman arası.<br/><br/>Bu özellik 10 dakika olarak ayarlanırsa, doğrulama 10 dakika içinde tamamlanmalıdır. Doğrulamanın gerçekleşilmesi 10 dakikadan uzun süreyle sürüyorsa, yeniden deneme süreleri dışarısızdır.<br/><br/>Doğrulama süresi nin tüm denemeleri ise, dilim **TimedOut**olarak işaretlenir. |Hayır |00:10:00 (10 dakika) |
| maksimumRetry |Harici verilerin kullanılabilirliğini denetleme sayısı. İzin verilen en büyük değer 10'dur. |Hayır |3 |


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçlardan veya SDK'lardan birini kullanarak veri kümeleri oluşturabilirsiniz:

- Kopyalama Sihirbazı
- Visual Studio
- PowerShell
- Azure Resource Manager şablonu
- REST API
- .NET API’si

Aşağıdaki araçlardan veya SDK'lardan birini kullanarak ardışık hatlar ve veri kümeleri oluşturmak için adım adım talimatlar için aşağıdaki öğreticilere bakın:

- [Veri dönüştürme etkinliğine sahip işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)
- [Veri hareketi etkinliği içeren bir ardışık hatlar oluşturma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Bir ardışık kaynak oluşturulduktan ve dağıtıldıktan sonra, Azure portal bıçaklarını veya İzleme ve Yönetim uygulamasını kullanarak ardışık hatlarınızı yönetebilir ve izleyebilirsiniz. Adım adım talimatlar için aşağıdaki konulara bakın:

- [Azure portal bıçaklarını kullanarak boru hatlarını izleme ve yönetme](data-factory-monitor-manage-pipelines.md)
- [İzleme ve Yönetim uygulamasını kullanarak boru hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md)

## <a name="scoped-datasets"></a>Kapsamlı veri kümeleri
**Veri kümeleri** özelliğini kullanarak bir ardışık lık kapsamına giren veri kümeleri oluşturabilirsiniz. Bu veri kümeleri, diğer ardışık alanlardaki etkinliklertarafından değil, yalnızca bu ardışık alandaki etkinliklertarafından kullanılabilir. Aşağıdaki örnekte, ardışık hatiçinde kullanılacak iki veri kümesi (InputDataset-rdc ve OutputDataset-rdc) bulunan bir ardışık nokta tanımlanır.

> [!IMPORTANT]
> Kapsamlı veri kümeleri yalnızca tek seferlik ardışık hatlar ile desteklenir **(pipelineMode** **OneTime**olarak ayarlanır). Ayrıntılar için [Onetime ardışık hattına](data-factory-create-pipelines.md#onetime-pipeline) bakın.
>
>

```json
{
    "name": "CopyPipeline-rdc",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Ardışık işlerle ilgili daha fazla bilgi için [bkz.](data-factory-create-pipelines.md)
- Ardışık hatların nasıl zamanlanıp yürütüldedildiği hakkında daha fazla bilgi için Azure [Veri Fabrikası'nda Zamanlama ve yürütme](data-factory-scheduling-and-execution.md)ye bakın.
