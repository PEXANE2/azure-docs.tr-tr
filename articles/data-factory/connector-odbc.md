---
title: Azure Data Factory kullanarak ODBC kaynaklarından veri kopyalama | Microsoft Docs
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak OData kaynaklarından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 9ee0f4ccfcd75504be6bb636e7ee54a845a10280
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966912"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve ODBC veri depolarından veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odbc-connector.md)
> * [Geçerli sürüm](connector-odbc.md)

Bu makalede, verileri bir ODBC veri deposuna ve ' den kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

ODBC kaynağından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan ODBC havuzuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu ODBC Bağlayıcısı **temel** veya **anonim** kimlik doğrulaması kullanarak **ODBC ile uyumlu tüm veri depolarından** veri kopyalamayı destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu ODBC bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir Integration Runtime ayarlayın. Bkz: [şirket içinde barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.
- Integration Runtime makinesindeki veri deposu için ODBC sürücüsünü yükler.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, ODBC bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

ODBC bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **İsti** | Evet |
| connectionString | Kimlik bilgisi bölümünü dışlayarak bağlantı dizesi. Bağlantı dizesini gibi `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`bir düzende belirtebilir veya Integration Runtime `"DSN=<name of the DSN on IR machine>;"` makinede ayarladığınız Sistem DSN 'sini (veri kaynağı adı) kullanabilirsiniz (yine de bağlı hizmette kimlik bilgisi bölümünü belirtmeniz gerekir).<br>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md).| Evet |
| authenticationType | ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulaması türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **anonim**. | Evet |
| userName | Temel kimlik doğrulaması kullanıyorsanız Kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabı için parola belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |
| credential | Sürücüye özgü özellik-değer biçiminde belirtilen bağlantı dizesinin erişim kimlik bilgisi kısmı. Örnek: `"RefreshToken=<secret refresh token>;"`. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşul](#prerequisites)bölümünde belirtildiği gibi, kendinden konak Integration Runtime gereklidir. |Evet |

**Örnek 1: temel kimlik doğrulaması kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: anonim kimlik doğrulaması kullanma**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölüm, ODBC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

ODBC ile uyumlu veri deposundan veri kopyalamak için, veri kümesinin Type özelliğini **Relationaltable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **RelationalTable** | Evet |
| tableName | ODBC veri deposundaki tablonun adı. | Kaynak için Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse);<br/>Havuz için Evet |

**Örnek**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, ODBC kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="odbc-as-source"></a>Kaynak olarak ODBC

ODBC uyumlu veri deposundan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Relationalsource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **RelationalSource** | Evet |
| query | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Yok (veri kümesinde "TableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="odbc-as-sink"></a>Havuz olarak ODBC

ODBC uyumlu veri deposuna veri kopyalamak için kopyalama etkinliğindeki havuz türünü **Odbcsink**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği şu şekilde ayarlanmalıdır: **OdbcSink** | Evet |
| writeBatchTimeout |Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br/>İzin verilen değerler: TimeSpan. Örnek: "00:30:00" (30 dakika). |Hayır |
| writeBatchSize |Arabellek boyutu writeBatchSize ulaştığında verileri SQL tablosuna ekler.<br/>İzin verilen değerler: Integer (satır sayısı). |Hayır (varsayılan 0-otomatik olarak algılanır) |
| preCopyScript |Her çalıştırmada veri deposuna veri yazmadan önce yürütülecek kopyalama etkinliği için bir SQL sorgusu belirtin. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. |Hayır |

> [!NOTE]
> "WriteBatchSize" ayarlanmamışsa (otomatik algılanan), kopyalama etkinliği önce sürücünün Batch işlemlerini destekleyip desteklemediğini algılar ve bunu yoksa 10000 olarak ayarlayın veya yoksa 1 olarak ayarlayın. 0 dışında bir değer ayarlarsanız, kopyalama etkinliği değeri eşit kılar ve sürücü Batch işlemlerini desteklemiyorsa çalışma zamanında başarısız olur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="sap-hana-sink"></a>SAP HANA havuzu

>[!NOTE]
>SAP HANA veri deposundan verileri kopyalamak için yerel [SAP HANA Bağlayıcısı](connector-sap-hana.md)' na bakın. SAP HANA verileri kopyalamak için lütfen bu yönergeyi izleyerek ODBC bağlayıcısını kullanın. SAP HANA Bağlayıcısı ve ODBC Bağlayıcısı için bağlı hizmetlerin farklı türde olduğunu ve bu nedenle yeniden kullanılmamasını aklınızda olun.
>

Genel ODBC bağlayıcısını kullanarak verileri SAP HANA veritabanına kopyalayabilirsiniz.

Veri deponuza erişimi olan bir makinede şirket içinde barındırılan bir Integration Runtime ayarlayın. Integration Runtime, veri deposuna bağlanmak için SAP HANA ODBC sürücüsünü kullanır. Bu nedenle, aynı makinede zaten yüklü değilse sürücüyü yükleme. Ayrıntılar için [Önkoşullar](#prerequisites) bölümüne bakın.

Data Factory çözümünde SAP HANA havuzunu kullanmadan önce, Integration Runtime [bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues) bölümündeki yönergeleri kullanarak veri deposuna bağlanıp bağlanamayacağını doğrulayın.

Aşağıdaki örnekte gösterildiği gibi SAP HANA veri deposunu bir Azure Data Factory 'ye bağlamak için ODBC bağlantılı hizmeti oluşturun:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015"
            },
            "authenticationType": "Basic",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

ODBC veri depolarının bir kopyalama işleminde kaynak/havuz veri depoları olarak kullanılmasına ilişkin ayrıntılı bir genel bakış için makalenin başındaki makaleyi okuyun.

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bağlantı sorunlarını gidermek için **Integration Runtime Configuration Manager** **Tanılama** sekmesini kullanın.

1. **Integration Runtime Configuration Manager**başlatın.
2. **Tanılama** sekmesine geçin.
3. "Bağlantıyı Sına" bölümünde, veri deposu **türünü** (bağlı hizmet) seçin.
4. Veri deposuna bağlanmak için kullanılan **bağlantı dizesini** belirtin, **kimlik doğrulamasını** seçin ve **Kullanıcı adı**, **parola**ve/veya **kimlik bilgilerini**girin.
5. Veri deposuyla bağlantıyı test etmek için **Bağlantıyı Sına** ' ya tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
