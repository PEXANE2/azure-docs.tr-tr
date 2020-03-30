---
title: Veri kümeleri
description: Veri Fabrikası'ndaki veri kümeleri hakkında bilgi edinin. Veri kümeleri giriş/çıktı verilerini temsil emzdir.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 878ad98b118fa02a6659584ac60e3343a948cd20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246272"
---
# <a name="datasets-in-azure-data-factory"></a>Azure Data Factory'de veri kümeleri
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-create-datasets.md)
> * [Geçerli sürüm](concepts-datasets-linked-services.md)

Bu makalede, veri kümelerinin ne olduğu, JSON biçiminde nasıl tanımlandığı ve Azure Veri Fabrikası ardışık hatlarında nasıl kullanıldığı açıklanmaktadır.

Veri Fabrikası'nda yeniyseniz, genel bakış için [Azure Veri Fabrikası'na Giriş'e](introduction.md) bakın.

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. **Ardışık işlem,** bir görevi birlikte gerçekleştiren **etkinliklerin** mantıksal bir gruplandırmasIdır. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Şimdi, **veri kümesi,** **etkinliklerinizde** kullanmak istediğiniz verileri giriş ve çıktı olarak yalnızca işaretleyen veya başvuran verilerin adlandırılmış görünümüdür. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin Azure Blob veri kümesi, etkinliğin verileri okuması için gereken blob kapsayıcısını ve Blob depolama klasörünü belirtir.

Bir veri kümesi oluşturmadan önce, veri deponuzu veri fabrikasına bağlamak için bağlantılı bir [**hizmet**](concepts-linked-services.md) oluşturmanız gerekir. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün; veri kümesi, bağlı veri depoları içindeki verilerin yapısını temsil eder ve bağlantılı hizmet veri kaynağına olan bağlantıyı tanımlar. Örneğin, Azure Depolama bağlantılı bir hizmet, bir depolama hesabını veri fabrikasına bağlar. Azure Blob veri kümesi, işlenecek giriş bloblarını içeren bu Azure depolama hesabındaki blob kapsayıcısını ve klasörünü temsil eder.

İşte örnek bir senaryo. Blob depolamasundaki verileri bir SQL veritabanına kopyalamak için iki bağlantılı hizmet oluşturursunuz: Azure Depolama ve Azure SQL Veritabanı. Ardından, iki veri kümesi oluşturun: Azure Blob veri kümesi (Azure Depolama bağlantılı hizmete başvurur) ve Azure SQL Table veri kümesi (Azure SQL Veritabanı bağlantılı hizmete başvurur). Azure Depolama ve Azure SQL Veritabanı bağlantılı hizmetler, Veri Fabrikası'nın sırasıyla Azure Depolama ve Azure SQL Veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi, Blob depolama alanınızdaki giriş bloblarını içeren blob kapsayıcısını ve blob klasörünü belirtir. Azure SQL Tablo veri kümesi, verilerin kopyalanış olacağı SQL veritabanınızdaki SQL tablosunu belirtir.

Aşağıdaki diyagram, Veri Fabrikası'nda ardışık hatlar, etkinlik, veri kümesi ve bağlantılı hizmet arasındaki ilişkileri gösterir:

![Boru hattı, etkinlik, veri kümesi, bağlantılı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)


## <a name="dataset-json"></a>Veri Seti JSON
Veri Fabrikası'ndaki bir veri kümesi aşağıdaki JSON biçiminde tanımlanır:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```
Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Veri kümesinin adı. Bkz. [Azure Veri Fabrikası - Adlandırma kuralları.](naming-rules.md) |  Evet |
type | Veri kümesinin türü. Veri Fabrikası tarafından desteklenen türlerden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için [Bkz. Dataset türleri.](#dataset-type) | Evet |
yapı | Veri kümesinin şeması. Ayrıntılar için [Dataset şemasına](#dataset-structure-or-schema)bakın. | Hayır |
typeProperties | Tür özellikleri her tür için farklıdır (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında ayrıntılı bilgi için [Dataset türüne](#dataset-type)bakın. | Evet |

### <a name="data-flow-compatible-dataset"></a>Veri akışı uyumlu veri seti



[Veri Akışı](concepts-data-flow-overview.md) uyumlu veri kümesi türlerinin listesi için desteklenen veri [kümesi türlerine](#dataset-type) bakın. Veri Akışı için uyumlu olan veri kümeleri, dönüşümler için ince taneli veri kümesi tanımları gerektirir. Böylece, JSON tanımı biraz farklıdır. _Bir yapı_ özelliği yerine, Veri Akışı uyumlu olan veri kümeleri bir _şema_ özelliğine sahiptir.

Veri Akışı'nda veri kümeleri kaynak ve lavabo dönüşümlerinde kullanılır. Veri kümeleri temel veri şemalarını tanımlar. Verilerinizin şeması yoksa, kaynağınız ve lavabonuz için şema kayması kullanabilirsiniz. Veri kümesindeki şema fiziksel veri türünü ve şeklini temsil eder.

Şema'yı veri kümesinden tanımlayarak, ilgili veri türlerini, veri biçimlerini, dosya konumunu ve bağlantı bilgilerini ilişkili Bağlantılı hizmetten alırsınız. Veri kümelerinden gelen meta veriler kaynak dönüşümünüzde kaynak *projeksiyonu*olarak görünür. Kaynak dönüşümündeki projeksiyon, tanımlanan adlar ve türleri içeren Veri Akışı verilerini temsil eder.

Veri Akışı veri kümesinin şemasını içe aktarırken, **Schema'yı içe aktar'ı** seçin ve kaynaktan veya yerel bir dosyadan alma seçeneğini belirleyin. Çoğu durumda, şema doğrudan kaynaktan almak gerekir. Ancak zaten yerel bir şema dosyanız (üstbilgileri olan bir Parke dosyası veya CSV) varsa, Veri Fabrikası'nı bu dosyadaki şemayı temel almak için yönlendirebilirsiniz.


```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                "type": "LinkedServiceReference",
        },
        "schema": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Veri kümesinin adı. Bkz. [Azure Veri Fabrikası - Adlandırma kuralları.](naming-rules.md) |  Evet |
type | Veri kümesinin türü. Veri Fabrikası tarafından desteklenen türlerden birini belirtin (örneğin: AzureBlob, AzureSqlTable). <br/><br/>Ayrıntılar için [Bkz. Dataset türleri.](#dataset-type) | Evet |
Şema | Veri kümesinin şeması. Ayrıntılar için [Bkz. Veri Akışı uyumlu veri kümeleri.](#dataset-type) | Hayır |
typeProperties | Tür özellikleri her tür için farklıdır (örneğin: Azure Blob, Azure SQL tablosu). Desteklenen türler ve özellikleri hakkında ayrıntılı bilgi için [Dataset türüne](#dataset-type)bakın. | Evet |


## <a name="dataset-example"></a>Dataset örneği
Aşağıdaki örnekte, veri kümesi BIR SQL veritabanında MyTable adlı bir tabloyu temsil eder.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Aşağıdaki noktalara dikkat edin:

- türü AzureSqlTable olarak ayarlanır.
- tableName türü özelliği (AzureSqlTable türüne özgü) MyTable olarak ayarlanır.
- linkedServiceName, bir sonraki JSON parçacığında tanımlanan AzureSqlDatabase türünde bağlantılı bir hizmet anlamına gelir.

## <a name="dataset-type"></a>Dataset türü
Kullandığınız veri deposuna bağlı olarak birçok farklı türde veri kümesi vardır. [Connector genel bakış](connector-overview.md) makalesinden Veri Fabrikası tarafından desteklenen depolanan verilerin listesini bulabilirsiniz. Bağlantılı bir hizmetin ve bu veri deposu için bir veri kümesinin nasıl oluşturulabildiğini öğrenmek için bir veri deposuna tıklayın.

Önceki bölümdeki örnekte, veri kümesinin türü **AzureSqlTable**olarak ayarlanır. Benzer şekilde, bir Azure Blob veri kümesi için, veri kümesinin türü aşağıdaki JSON'da gösterildiği gibi **AzureBlob**olarak ayarlanır:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                "type": "LinkedServiceReference",
        },

        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```

## <a name="dataset-structure-or-schema"></a>Dataset yapısı veya şeması
**Yapı** bölümü veya **şema** (Veri Akışı uyumlu) bölümü veri kümeleri isteğe bağlıdır. Adlar ve veri türleri sütunlarının bir koleksiyon içeren veri kümesinin şema tanımlar. Türleri ve harita sütunlarını kaynaktan hedefe dönüştürmek için kullanılan tür bilgilerini sağlamak için yapı bölümünü kullanırsınız.

Yapıdaki her sütun aşağıdaki özellikleri içerir:

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Sütunun adı. | Evet
type | Sütunun veri türü. Veri Fabrikası izin verilen değerler olarak aşağıdaki ara veri türlerini destekler: **Int16, Int32, Int64, Single, Double, Ondalık, Bayt[], Boolean, String, Guid, Datetime, Datetimeoffset ve Timespan** | Hayır
kültür | . Tür .NET türü olduğunda kullanılacak NET tabanlı `Datetime` kültür: `Datetimeoffset`veya . Varsayılan değer: `en-us`. | Hayır
biçim | Tür bir .NET türü olduğunda kullanılacak biçim `Datetime` `Datetimeoffset`dizesi: veya . Datetime'ın nasıl biçimlendirilenözel [Tarih ve Saat Biçim Dizeleri'ne](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) bakın. | Hayır

### <a name="example"></a>Örnek
Aşağıdaki örnekte, kaynak Blob verilerinin CSV biçiminde olduğunu ve üç sütun içerdiğini varsayalım: userid, ad ve lastlogindate. Bunlar, haftanın günü için kısaltılmış Fransızca adlar kullanarak özel bir datetime biçimine sahip Int64, String ve Datetime türündendir.

Sütunlar için yazı tanımları ile birlikte Blob veri kümesi yapısını aşağıdaki gibi tanımlayın:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Rehber

Aşağıdaki yönergeler, yapı bilgilerinin ne zaman ekleyip **yapı** bölümüne ne eklendirileceğini anlamanıza yardımcı olur. Veri fabrikasının kaynak verilerini nasıl batması ve [Şema ve tür eşlemesinden](copy-activity-schema-and-type-mapping.md)yapı bilgilerini ne zaman belirtecekleri hakkında daha fazla bilgi edinin.

- **Güçlü şema veri kaynakları için**yapı bölümünü yalnızca harita kaynak sütunlarının sütunları batırmasını istiyorsanız belirtin ve adları aynı değildir. Bu tür yapılandırılmış veri kaynağı, veri şema larını ve türü bilgileri, verilerin kendisiyle birlikte depolar. Yapılandırılmış veri kaynaklarına örnek olarak SQL Server, Oracle ve Azure SQL Veritabanı verilebilir.<br/><br/>Tür bilgileri yapılandırılmış veri kaynakları için zaten mevcut olduğundan, yapı bölümünü eklediğinizde tür bilgilerini eklememelisiniz.
- **Blob depolama metin dosyası gibi no / zayıf şema veri kaynakları**için, veri kümesi bir kopyalama etkinliği için bir giriş olduğunda yapısı içerir ve kaynak veri kümesi veri türleri lavabo için yerel türlere dönüştürülmelidir. Ve kaynak sütunları batmak için eşlemek istediğinizde yapıyı ekleyin...

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu araçlardan veya SDK'lardan birini kullanarak veri kümeleri oluşturabilirsiniz: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST API,](quickstart-create-data-factory-rest-api.md)Azure Kaynak Yöneticisi Şablonu ve Azure portalı

## <a name="current-version-vs-version-1-datasets"></a>Geçerli sürüm ve sürüm 1 veri setleri

Veri Fabrikası ve Veri Fabrikası sürüm 1 veri kümeleri arasındaki bazı farklar şunlardır:

- Dış özellik geçerli sürümde desteklenmez. Yerine [bir tetikleyici.](concepts-pipeline-execution-triggers.md)
- İlke ve kullanılabilirlik özellikleri geçerli sürümde desteklenmez. Bir ardışık için başlangıç zamanı [tetikleyicilere](concepts-pipeline-execution-triggers.md)bağlıdır.
- Kapsamlı veri kümeleri (ardışık alanda tanımlanan veri kümeleri) geçerli sürümde desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
Bu araçlardan veya SDK'lardan birini kullanarak ardışık hatlar ve veri kümeleri oluşturmak için adım adım talimatlar için aşağıdaki öğreticiye bakın.

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Quickstart: PowerShell kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md)
- [Quickstart: REST API kullanarak bir veri fabrikası oluşturmak](quickstart-create-data-factory-rest-api.md)
- [Quickstart: Azure portalLarını kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-portal.md)
