---
title: PostgreSQL için Azure veritabanı 'nda verileri kopyalama ve dönüştürme
description: Azure Data Factory kullanarak PostgreSQL için Azure veritabanı 'na verileri kopyalamayı ve dönüştürmeyi öğrenin.
ms.author: jingwang
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/25/2021
ms.openlocfilehash: ec4ea645e325ef48d4cb5951cd39fd4e9cbe1617
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101738064"
---
# <a name="copy-and-transform-data-in-azure-database-for-postgresql-by-using-azure-data-factory"></a>Azure Data Factory kullanarak PostgreSQL için Azure veritabanı 'nda verileri kopyalama ve dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, PostgreSQL için Azure veritabanı 'na veri kopyalamak için Azure Data Factory kopyalama etkinliğini kullanma ve PostgreSQL için Azure veritabanı 'ndaki verileri dönüştürmek üzere veri akışı kullanma açıklanmaktadır. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

Bu bağlayıcı [PostgreSQL Için Azure veritabanı hizmeti](../postgresql/overview.md)için özelleştirilmiştir. Şirket içinde veya bulutta bulunan genel bir PostgreSQL veritabanından veri kopyalamak için [PostgreSQL bağlayıcısını](connector-postgresql.md)kullanın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

PostgreSQL için Azure Veritabanı Bağlayıcısı, aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [desteklenen bir kaynak/havuz matrisi](copy-activity-overview.md) ile kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Şu anda veri akışı, PostgreSQL için Azure veritabanı 'nı destekler ancak esnek sunucu veya hiper ölçek (Citus) değil.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, PostgreSQL için Azure veritabanı bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi verilmektedir.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

PostgreSQL için Azure veritabanı bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği: **AzurePostgreSql** olarak ayarlanmalıdır. | Yes |
| Dizisi | PostgreSQL için Azure veritabanı 'na bağlanmak üzere bir ODBC bağlantı dizesi.<br/>Ayrıca, Azure Key Vault bir parola yerleştirebilir ve `password` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha fazla ayrıntı için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault](store-credentials-in-key-vault.md) . | Yes |
| connectVia | Bu özellik, veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanını](concepts-integration-runtime.md) temsil eder. Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel ağda yer alıyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

Tipik bir bağlantı dizesi `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>` . Servis talebi başına ayarlayabileceğiniz daha fazla özellik aşağıda verilmiştir:

| Özellik | Açıklama | Seçenekler | Gerekli |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Sürücünün sürücü ve veritabanı sunucusu arasında gönderilen verileri şifrelemek için kullandığı yöntem. Örneğin,  `EncryptionMethod=<0/1/6>;`| 0 (şifreleme yok) **(varsayılan)** /1 (SSL)/6 (requestssl) | Hayır |
| ValidateServerCertificate (VSC) | Sürücünün, SSL şifrelemesi etkinleştirildiğinde veritabanı sunucusu tarafından gönderilen sertifikayı doğrulayıp doğrulamayacağını belirler (şifreleme yöntemi = 1). Örneğin,  `ValidateServerCertificate=<0/1>;`| 0 (devre dışı) **(varsayılan)** /1 (etkin) | Hayır |

**Örnek**:

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        }
    }
}
```

**Örnek**:

***Parolayı Azure Key Vault içinde depola***

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [Azure Data Factory veri kümeleri](concepts-datasets-linked-services.md). Bu bölüm, veri kümelerinde PostgreSQL için Azure veritabanı 'nın desteklediği özelliklerin bir listesini sağlar.

PostgreSQL için Azure veritabanı 'ndan veri kopyalamak için veri kümesinin Type özelliğini **AzurePostgreSqlTable** olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin Type özelliği **AzurePostgreSqlTable** olarak ayarlanmalıdır | Yes |
| tableName | Tablonun adı | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

**Örnek**:

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için, bkz. Azure Data Factory işlem [hatları ve etkinlikleri](concepts-pipelines-activities.md). Bu bölüm, PostgreSQL için Azure veritabanı kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-database-for-postgresql-as-source"></a>Kaynak olarak PostgreSql için Azure veritabanı

PostgreSQL için Azure veritabanı 'ndan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **AzurePostgreSqlSource** olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği **AzurePostgreSqlSource** olarak ayarlanmalıdır | Yes |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `SELECT * FROM mytable` veya `SELECT * FROM "MyTable"` . Not PostgreSQL öğesinde, tırnak işareti yoksa, varlık adının büyük/küçük harf duyarlı olduğunu kabul ediyor. | Hayır (veri kümesindeki tableName özelliği belirtilmişse) |

**Örnek**:

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
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
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM mytable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-postgresql-as-sink"></a>Havuz olarak PostgreSQL için Azure veritabanı

PostgreSQL için Azure veritabanı 'na veri kopyalamak için, etkinlik **havuzunu** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun Type özelliği **AzurePostgreSQLSink** olarak ayarlanmalıdır. | Yes |
| Ön Copyscrıpt | Her çalıştırmada PostgreSQL için Azure veritabanı 'na veri yazmadan önce, kopyalama etkinliğinin yürütülmesi için bir SQL sorgusu belirtin. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. | Hayır |
| writeMethod | PostgreSQL için Azure veritabanı 'na veri yazmak için kullanılan yöntem.<br>İzin verilen değerler: **Copycommand** (Önizleme, daha iyi performans olan Önizleme), **bulkınsert** (varsayılan). | Hayır |
| writeBatchSize | Toplu iş başına PostgreSQL için Azure veritabanı 'na yüklenen satır sayısı.<br>İzin verilen değer, satır sayısını temsil eden bir tamsayıdır. | Hayır (varsayılan değer 1.000.000) |
| writeBatchTimeout | Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br>İzin verilen değerler TimeSpan dizeleridir. Örnek olarak 00:30:00 (30 dakika). | Hayır (varsayılan değer 00:30:00) |

**Örnek**:

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure PostgreSQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzurePostgreSQLSink",
                "preCopyScript": "<custom SQL script>",
                "writeMethod": "CopyCommand",
                "writeBatchSize": 1000000
            }
        }
    }
]
```

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki verileri dönüştürürken, PostgreSQL için Azure veritabanı 'ndan tabloları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için bkz. veri akışlarını eşleme içindeki [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüşümü](data-flow-sink.md) . PostgreSQL için Azure veritabanı veri kümesini veya kaynak ve havuz türü olarak [satır içi veri kümesini](data-flow-source.md#inline-datasets) kullanmayı seçebilirsiniz.

### <a name="source-transformation"></a>Kaynak dönüştürme

Aşağıdaki tabloda PostgreSQL için Azure veritabanı kaynağı tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **kaynak seçenekleri** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Tablo | Giriş olarak tablo ' yı seçerseniz veri akışı, veri kümesinde belirtilen tablodaki tüm verileri getirir. | Hayır | - |*(yalnızca satır içi veri kümesi için)*<br>tableName |
| Sorgu | Giriş olarak Sorgula ' yı seçerseniz, kaynaktan veri getirmek için bir SQL sorgusu belirtin. Bu, veri kümesinde belirttiğiniz tüm tabloları geçersiz kılar. Sorguları kullanmak, test veya arama için satırları azaltmanın harika bir yoludur.<br><br>**Order by** yan tümcesi desteklenmez, ancak BIR tam select from deyimi ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , veri akışında kullanabileceğiniz bir tablo döndüren SQL 'de bir UDF 'dir.<br>Sorgu örneği: `select * from mytable where customerId > 1000 and customerId < 2000` veya `select * from "MyTable"` . Not PostgreSQL öğesinde, tırnak işareti yoksa, varlık adının büyük/küçük harf duyarlı olduğunu kabul ediyor.| Hayır | Dize | sorgu |
| Toplu iş boyutu | Büyük verileri toplu işlemlere Bölmek için bir toplu iş boyutu belirtin. | Hayır | Tamsayı | batchSize |
| Yalıtım düzeyi | Aşağıdaki yalıtım düzeylerinden birini seçin:<br>-Kaydedilen okuma<br>-Read UNCOMMITTED (varsayılan)<br>-Yinelenebilir okuma<br>-Serileştirilebilir<br>-None (yalıtım düzeyini yoksay) | Hayır | <small>READ_COMMITTED<br/>READ_UNCOMMITTED<br/>REPEATABLE_READ<br/>SERI hale GETIRILEBILIR<br/>SEÇIM</small> |'Sinden |

#### <a name="azure-database-for-postgresql-source-script-example"></a>PostgreSQL için Azure veritabanı kaynak betiği örneği

Kaynak türü olarak PostgreSQL için Azure veritabanı 'nı kullandığınızda, ilişkili veri akışı betiği şu şekilde olur:

```
source(allowSchemaDrift: true,
    validateSchema: false,
    isolationLevel: 'READ_UNCOMMITTED',
    query: 'select * from mytable',
    format: 'query') ~> AzurePostgreSQLSource
```

### <a name="sink-transformation"></a>Havuz dönüştürme

Aşağıdaki tabloda PostgreSQL için Azure veritabanı havuzu tarafından desteklenen özellikler listelenmiştir. Bu özellikleri, **havuz seçenekleri** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Yöntemi Güncelleştir | Veritabanı Hedefinizdeki işlemlere izin verileceğini belirtin. Varsayılan değer yalnızca eklemeleri izin verir.<br>Satırları güncelleştirmek, kaldırmak veya silmek için, bu eylemler için satırları etiketlemek üzere bir [alter Row dönüşümü](data-flow-alter-row.md) gereklidir. | Yes | `true` veya `false` | siler <br/>eklenebilir <br/>güncellenebilir <br/>upsertable |
| Anahtar sütunlar | Güncelleştirmeler, üst üste ve siler için, hangi satırın ekleneceğini belirleyen anahtar sütunlar ayarlanmalıdır.<br>Anahtar olarak seçtiğiniz sütun adı, sonraki güncelleştirme, upsert, DELETE 'in bir parçası olarak kullanılacaktır. Bu nedenle, havuz eşlemesinde var olan bir sütun seçmeniz gerekir. | Hayır | Dizi | keys |
| Anahtar sütunları yazmayı atla | Değeri anahtar sütununa yazmak istiyorsanız "anahtar sütunlarını yazmayı atla" seçeneğini belirleyin. | Hayır | `true` veya `false` | Skipkeyyazmaları |
| Tablo eylemi |Yazmadan önce hedef tablodaki tüm satırların yeniden oluşturulup kaldırılacağını belirler.<br>- **Hiçbiri**: tabloya hiçbir eylem yapılmaz.<br>- **Yeniden oluştur**: tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuluyoruz gereklidir.<br>- **Kes**: hedef tablodaki tüm satırlar kaldırılacak. | Hayır | `true` veya `false` | Oluştur<br/>kesilemedi |
| Toplu iş boyutu | Her bir toplu işte kaç satır yazıldığını belirtin. Daha büyük toplu işlem boyutları sıkıştırma ve bellek iyileştirmeyi iyileştirir, ancak verileri önbelleğe alırken bellek dışında özel durumlar riskini ortadan kaldıracak. | Hayır | Tamsayı | batchSize |
| SQL betikleri öncesi ve sonrası | (Ön işleme) ve sonra (işlem sonrası) verileri havuz veritabanınıza yazıldıktan önce yürütülecek çok satırlı SQL betikleri belirtin. | Hayır | Dize | preSQLs<br>postSQLs |

#### <a name="azure-database-for-postgresql-sink-script-example"></a>PostgreSQL için Azure veritabanı havuz betiği örneği

PostgreSQL için Azure veritabanı 'nı havuz türü olarak kullandığınızda, ilişkili veri akışı betiği şu şekilde olur:

```
IncomingStream sink(allowSchemaDrift: true,
    validateSchema: false,
    deletable:false,
    insertable:true,
    updateable:true,
    upsertable:true,
    keys:['keyColumn'],
    format: 'table',
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> AzurePostgreSQLSink
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özellikler hakkında daha fazla bilgi için [Azure Data Factory arama etkinliği](control-flow-lookup-activity.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
