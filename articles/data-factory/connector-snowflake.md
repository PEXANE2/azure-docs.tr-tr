---
title: /' Dan kar tanesi 'nden veri kopyalama
description: Data Factory kullanarak kar/veya arasında veri kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/15/2020
ms.openlocfilehash: fd067a0a0e99291dd4cea924a15c1157bc3b9f9f
ms.sourcegitcommit: c052c99fd0ddd1171a08077388d221482026cd58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84425505"
---
# <a name="copy-data-from-and-to-snowflake-by-using-azure-data-factory"></a>Azure Data Factory kullanarak kar/veya kar tanesi arasında veri kopyalama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, ve ' den kar tanesi arasında veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu kar tanesi Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak/havuz matris](copy-activity-overview.md) tablosuyla Kopyala
- [Arama etkinliği](control-flow-lookup-activity.md)

Kopyalama etkinliği için, bu kar tanesi bağlayıcı şu işlevleri destekler:

- En iyi performansı elde etmek için kar \ ' ın [kopyasını [location]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html) komutuna kullanan kar grubundan verileri kopyalayın.
- En iyi performansı elde etmek için, kar \ ' ın [[tablo] ' dan bir kopyadan](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html) faydalanan verileri kar içine kopyalayın. Azure 'da kar.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, bir kar oluşturan bağlayıcıya özgü Data Factory varlıkları tanımlayan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler, bir kar tanesi bağlantılı hizmeti için desteklenir:

| Özellik         | Açıklama                                                  | Gerekli |
| :--------------- | :----------------------------------------------------------- | :------- |
| tür             | Tür özelliğinin **kar**tanesi olarak ayarlanması gerekir.              | Yes      |
| Dizisi | [Tam hesap adını](https://docs.snowflake.net/manuals/user-guide/connecting.html#your-snowflake-account-name) (bölge ve bulut platformunu tanımlayan ek segmentler dahil), Kullanıcı adını, parolayı, veritabanını ve ambarı yapılandırın. Kar tanesi örneğine bağlanmak için JDBC bağlantı dizesini belirtin. Parolayı Azure Key Vault de yerleştirebilirsiniz. Daha ayrıntılı bilgi için tablonun altındaki örneklere ve [Azure Key Vault makalesindeki kimlik bilgileri deposuna](store-credentials-in-key-vault.md) bakın.| Yes      |
| connectVia       | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolubir özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. | No       |

**Örnek:**

```json
{
    "name": "SnowflakeLinkedService",
    "properties": {
        "type": "Snowflake",
        "typeProperties": {
            "connectionString": "jdbc:snowflake://<accountname>.snowflakecomputing.com/?user=<username>&password=<password>&db=<database>&warehouse=<warehouse>(optional)"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Azure Key Vault parola:**

```json
{
    "name": "SnowflakeLinkedService",
    "properties": {
        "type": "Snowflake",
        "typeProperties": {
            "connectionString": "jdbc:snowflake://<accountname>.snowflakecomputing.com/?user=<username>&db=<database>&warehouse=<warehouse>(optional)",
            "password": {
                "type": "AzureKeyVaultSecret",
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>",
                    "type": "LinkedServiceReference"
                }, 
                "secretName": "<secretName>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

Aşağıdaki özellikler, kar tanesi veri kümesi için desteklenir:

| Özellik  | Açıklama                                                  | Gerekli                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| tür      | Veri kümesinin Type özelliği, **kar tablosu**olarak ayarlanmalıdır. | Yes                         |
| manızı | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| tablo | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |

**Örnek:**

```json
{
    "name": "SnowflakeDataset",
    "properties": {
        "type": "SnowflakeTable",
        "typeProperties": {
            "schema": "<Schema name for your Snowflake database>",
            "table": "<Table name for your Snowflake database>"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "linkedServiceName": {
            "referenceName": "<name of linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliği özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, kar kaynağı ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="snowflake-as-the-source"></a>Kaynak olarak kar tanesi

Kar tanesi Bağlayıcısı, en iyi performansı elde etmek için, ' ın altındaki kar/ [[konum]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html) komutunu kullanır.

* Havuz veri deposu ve biçimi, kar tanesi kopyalama komutu tarafından yerel olarak destekleniyorsa, kar/ç 'den havuza doğrudan kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar için bkz. [kar grubundan doğrudan kopyalama](#direct-copy-from-snowflake).
* Aksi takdirde, [kar tanesi ' nden yerleşik hazırlanmış kopyayı](#staged-copy-from-snowflake)kullanın.

Verileri kar 'lerden kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik                     | Açıklama                                                  | Gerekli |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| tür                         | Kopyalama etkinliği kaynağının Type özelliği **SnowflakeSource**olarak ayarlanmalıdır. | Yes      |
| sorgu          | Kar tanesi 'nden verileri okumak için SQL sorgusunu belirtir. | No       |
| exportSettings | Kar tanesi 'nden verileri almak için kullanılan gelişmiş ayarlar. ADF 'yi çağırdığınızda, ADF 'nin geçileye COPY komutuyla desteklenen olanları yapılandırabilirsiniz. | No       |
| ***Altında `exportSettings` :*** |  |  |
| tür | Dışa aktarma komutunun türü, **kar Keexportcopycommand**olarak ayarlanır. | Yes |
| additionalCopyOptions | Anahtar-değer çiftlerinin sözlüğü olarak belirtilen ek kopyalama seçenekleri. Örnekler: MAX_FILE_SIZE, ÜZERINE yaz. [Kar tanesi kopyalama seçeneklerinden](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html#copy-options-copyoptions)daha fazla bilgi edinin. | No |
| additionalFormatOptions | Anahtar-değer çiftlerinin bir sözlüğü olarak belirtilen COPY komutuna ek dosya biçimi seçenekleri verilmiştir. Örnekler: DATE_FORMAT, TIME_FORMAT, TIMESTAMP_FORMAT. [Kar tanesi biçim türü seçeneklerinden](https://docs.snowflake.com/en/sql-reference/sql/copy-into-location.html#format-type-options-formattypeoptions)daha fazla bilgi edinin. | No |

#### <a name="direct-copy-from-snowflake"></a>Kar tanesi 'nden doğrudan kopya

Havuz veri deprenizin ve formatı bu bölümde açıklanan ölçütlere uyuyorsa, kar/ç 'den havuza doğrudan kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Azure Data Factory, ayarları denetler ve ölçütler karşılanmazsa kopyalama etkinliğinin çalıştırılmasına başarısız olur.

1. **Havuz bağlantılı hizmeti** , **paylaşılan erişim Imzası** kimlik doğrulamasıyla [**Azure Blob**](connector-azure-blob-storage.md) türüdür.

2. **Havuz veri biçimi** , aşağıdaki yapılandırmalara sahip olan **Parquet** veya **sınırlandırılmış metindir**:

   - **Parquet** biçimi için, sıkıştırma codec 'i **none**, **Snappy**veya **LZO**olur.
   - **Sınırlandırılmış metin** biçimi için:
     - `rowDelimiter`**\r\n**veya herhangi bir tek karakter.
     - `compression`**sıkıştırma**, **gzip**, **bzip2**veya **söndür**olamaz.
     - `encodingName`Varsayılan olarak bırakılır veya **UTF-8**olarak ayarlanır.
     - `quoteChar`**çift tırnak**, **tek tırnak** veya **boş dize** (quote char yok).
3. Kopyalama etkinliği kaynağı ' nda `additionalColumns` belirtilmedi.
4. Sütun eşleme belirtilmedi.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Snowflake input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SnowflakeSource",
                "sqlReaderQuery": "SELECT * FROM MyTable",
                "exportSettings": {
                    "type": "SnowflakeExportCopyCommand",
                    "additionalCopyOptions": {
                        "MAX_FILE_SIZE": "64000000",
                        "OVERWRITE": true
                    },
                    "additionalFormatOptions": {
                        "DATE_FORMAT": "'MM/DD/YYYY'"
                    }
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="staged-copy-from-snowflake"></a>Kar tanesi üzerinden hazırlanan kopya

Havuz veri depoluğunuz veya biçimi son bölümde belirtildiği gibi kar tanesi kopyalama komutuyla yerel olarak uyumlu değilse, yerleşik hazırlanmış kopyayı geçici bir Azure Blob depolama örneği aracılığıyla etkinleştirin. Hazırlanan kopya özelliği ayrıca daha iyi bir aktarım hızı sağlar. Data Factory kar verileri hazırlama deposuna dışarı aktarır ve sonra verileri havuza kopyalayabilir, son olarak hazırlama depolamadaki geçici verilerinizi temizler. Hazırlama yoluyla veri kopyalama hakkındaki ayrıntılar için bkz. [aşamalı kopya](copy-activity-performance-features.md#staged-copy) .

Bu özelliği kullanmak için, Azure depolama hesabına ara hazırlama olarak başvuran bir [Azure Blob depolama bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties) oluşturun. Sonra `enableStaging` `stagingSettings` kopyalama etkinliğinde ve özelliklerini belirtin.

> [!NOTE]
>
> Hazırlama Azure Blob bağlı hizmetinin, kar tanesi kopyalama komutu için gereken şekilde paylaşılan erişim imzası kimlik doğrulamasını kullanması gerekir. 

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Snowflake input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SnowflakeSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "mystagingpath"
            }
        }
    }
]
```

### <a name="snowflake-as-sink"></a>Kanal olarak kar tanesi

Kar tanesi Bağlayıcısı, en iyi performansı elde etmek için, kar tanesine ait [kopyayı [tablo]](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html) komutuna göre kullanır. Azure 'da kar için veri yazmayı destekler.

* Kaynak veri deposu ve biçim, kar tanesi kopyalama komutu tarafından yerel olarak destekleniyorsa, kaynaktan kar \ ' a doğrudan kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar için bkz. [kar halinde doğrudan kopyalama](#direct-copy-to-snowflake).
* Aksi takdirde, [kar tanesi için yerleşik hazırlanmış kopyayı](#staged-copy-to-snowflake)kullanın.

Verileri kar 'a kopyalamak için, kopyalama etkinliği **havuzu** bölümünde aşağıdaki özellikler desteklenir:

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| tür              | Kopyalama etkinliği havuzunun Type özelliği **SnowflakeSink**olarak ayarlanmalıdır. | Yes                                           |
| Ön Copyscrıpt     | Kopyalama etkinliği için, her çalıştırmada verileri kar alanına yazmadan önce çalıştırılacak bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | No                                            |
| importSettings | *Kar ayarlarına veri yazmak için kullanılan gelişmiş ayarlar. ADF 'yi çağırdığınızda, ADF 'nin geçileye COPY komutuyla desteklenen olanları yapılandırabilirsiniz.* | *Hayır* |
| ***Altında `importSettings` :*** |                                                              |  |
| tür | Import komutunun türü, **kar Keımportcopycommand**olarak ayarlanır. | Yes |
| additionalCopyOptions | Anahtar-değer çiftlerinin sözlüğü olarak belirtilen ek kopyalama seçenekleri. Örnekler: ON_ERROR, zorla, LOAD_UNCERTAIN_FILES. [Kar tanesi kopyalama seçeneklerinden](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#copy-options-copyoptions)daha fazla bilgi edinin. | No |
| additionalFormatOptions | COPY komutuna, anahtar-değer çiftlerinin bir sözlüğü olarak sağlanmış ek dosya biçimi seçenekleri. Örnekler: DATE_FORMAT, TIME_FORMAT, TIMESTAMP_FORMAT. [Kar tanesi biçim türü seçeneklerinden](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table.html#format-type-options-formattypeoptions)daha fazla bilgi edinin. | No |

#### <a name="direct-copy-to-snowflake"></a>Kar için doğrudan kopya

Kaynak veri deprenizin ve formatı bu bölümde açıklanan ölçütlere uyuyorsa, kaynaktan kar/veya daha doğrudan kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Azure Data Factory, ayarları denetler ve ölçütler karşılanmazsa kopyalama etkinliğinin çalıştırılmasına başarısız olur.

1. **Kaynak bağlı hizmet** , **paylaşılan erişim Imzası** kimlik doğrulamasıyla [**Azure Blob**](connector-azure-blob-storage.md) türüdür.

2. **Kaynak veri biçimi** , aşağıdaki yapılandırmalara sahip olan **Parquet** veya **sınırlandırılmış metindir** :

   - **Parquet** biçimi için, sıkıştırma codec 'i **none**veya **Snappy**.

   - **Sınırlandırılmış metin** biçimi için:
     - `rowDelimiter`**\r\n**veya herhangi bir tek karakter. Satır sınırlayıcısı "\r\n" değilse, `firstRowAsHeader` **false**olması ve `skipLineCount` belirtilmemişse belirtilmemiş olması gerekir.
     - `compression`**sıkıştırma**, **gzip**, **bzip2**veya **söndür**olamaz.
     - `encodingName`Varsayılan olarak bırakılır veya "UTF-8", "UTF-16", "UTF-16TO", "UTF-32", "UTF-32AS", "BIG5", "EUC-JP", "EUC-KR", "GB18030", "ISO-2022-JP", "ISO-2022-KR", "olarak ayarlanır. ISO-8859-1", "ISO-8859-2", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255".
     - `quoteChar`**çift tırnak**, **tek tırnak** veya **boş dize** (quote char yok).

3. Kopyalama etkinliği kaynağı bölümünde, 

   -  `additionalColumns`belirtilmedi.
   - Kaynağınız bir klasörse, `recursive` true olarak ayarlanmalıdır.
   - `prefix`, `modifiedDateTimeStart` , `modifiedDateTimeEnd` belirtilmedi.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Snowflake output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SnowflakeSink",
                "importSettings": {
                    "type": "SnowflakeImportCopyCommand",
                    "copyOptions": {
                        "FORCE": "TRUE",
                        "ON_ERROR": "SKIP_FILE",
                    },
                    "fileFormatOptions": {
                        "DATE_FORMAT": "YYYY-MM-DD",
                    }
                }
            }
        }
    }
]
```

#### <a name="staged-copy-to-snowflake"></a>Kar için hazırlanan kopya

Havuz veri depoluğunuz veya biçimi son bölümde belirtildiği gibi kar tanesi kopyalama komutuyla yerel olarak uyumlu değilse, yerleşik hazırlanmış kopyayı geçici bir Azure Blob depolama örneği aracılığıyla etkinleştirin. Hazırlanan kopya özelliği ayrıca daha iyi bir aktarım hızı sağlar. Data Factory, verileri otomatik olarak kar tanesi veri biçimi gereksinimlerine uyacak şekilde dönüştürür. Ardından, verileri kar halinde yüklemek için kopyalama komutunu çağırır. Son olarak, blob depolamadan geçici verilerinizi temizler. Hazırlama yoluyla veri kopyalama hakkındaki ayrıntılar için bkz. [aşamalı kopya](copy-activity-performance-features.md#staged-copy) .

Bu özelliği kullanmak için, Azure depolama hesabına ara hazırlama olarak başvuran bir [Azure Blob depolama bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties) oluşturun. Sonra `enableStaging` `stagingSettings` kopyalama etkinliğinde ve özelliklerini belirtin.

> [!NOTE]
>
> Hazırlama Azure Blob bağlı hizmetinin, kar tanesi kopyalama komutu için gereken şekilde paylaşılan erişim imzası kimlik doğrulamasını kullanması gerekir.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSnowflake",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Snowflake output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SnowflakeSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                },
                "path": "mystagingpath"
            }
        }
    }
]
```


## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory ' de kopyalama etkinliğine göre kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).
