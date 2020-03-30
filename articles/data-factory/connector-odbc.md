---
title: Azure Veri Fabrikası'nı kullanarak ODBC kaynaklarından veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına OData kaynaklarından gelen verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/09/2020
ms.author: jingwang
ms.openlocfilehash: 6513cfc5432e969fc53aa72b075af194a064d178
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244374"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri ODBC veri depolarından ve ODBC veri depolarına kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-odbc-connector.md)
> * [Geçerli sürüm](connector-odbc.md)

Bu makalede, Bir ODBC veri deposundan ve verilerin kopyalanmasıiçin Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu ODBC bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri ODBC kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan ODBC lavabosuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu ODBC bağlayıcısı **Temel** veya **Anonim** kimlik doğrulaması kullanarak herhangi **bir ODBC uyumlu veri depolarından/tüm veri depolarına** veri kopyalamayı destekler. **64 bit ODBC sürücüsü** gereklidir.

## <a name="prerequisites"></a>Ön koşullar

Bu ODBC konektörünü kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan Tümleştirme Çalışma Zamanı'nı ayarlayın. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.
- Tümleştirme Runtime makinesinde veri deposu için 64 bit ODBC sürücüsünü yükleyin.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, ODBC bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

ODBC bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Odbc** | Evet |
| Connectionstring | Kimlik bilgisi bölümünü hariç alan bağlantı dizesi. Bağlantı dizesini, "Bağlı `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`hizmetteki kimlik bölümünü buna göre belirtmeniz gerekir) ile Tümleştirme `"DSN=<name of the DSN on IR machine>;"` Runtime makinesinde ayarladığınız DSN (Veri Kaynağı Adı) gibi desenle belirtebilirsiniz.<br>Ayrıca Azure Key Vault'a bir parola `password` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz.Azure [Anahtar Kasası'ndaki](store-credentials-in-key-vault.md) Mağaza kimlik bilgilerine daha fazla ayrıntı yla bakın.| Evet |
| authenticationType | ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler şunlardır: **Temel** ve **Anonim**. | Evet |
| userName | Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. | Hayır |
| password | Kullanıcı Adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| kimlik bilgisi | Bağlantı dizesinin sürücüye özgü özellik değeri biçiminde belirtilen erişim kimlik bilgileri bölümü. Örnek: `"RefreshToken=<secret refresh token>;"`. Bu alanı SecureString olarak işaretleyin. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek 1: Temel kimlik doğrulamasını kullanarak**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
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

**Örnek 2: Anonim kimlik doğrulaması kullanarak**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "<connection string>",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, ODBC veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri ODBC uyumlu veri deposundan kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **OdbcTable** | Evet |
| tableName | ODBC veri deposundaki tablonun adı. | Kaynak için hayır (etkinlik kaynağında "sorgu" belirtilirse);<br/>Lavabo için evet |

**Örnek**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "OdbcTable",
        "schema": [],
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

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, ODBC kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="odbc-as-source"></a>Kaynak olarak ODBC

ODBC uyumlu veri deposundan veri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **OdbcSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

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
                "type": "OdbcSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Dakti-yazılı `RelationalSource` kaynak kullanıyorsanız, ileriye dönük yeni bir kaynak kullanmanız önerilirken, yine de olduğu gibi desteklenir.

### <a name="odbc-as-sink"></a>Lavabo olarak ODBC

Verileri ODBC uyumlu veri deposuna kopyalamak için, kopyalama etkinliğindeki lavabo türünü **OdbcSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabo türü özelliği ayarlanmalıdır: **OdbcSink** | Evet |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi.<br/>İzin verilen değerler şunlardır: zaman pan. Örnek: "00:30:00" (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler.<br/>İzin verilen değerler şunlardır: insa (satır sayısı). |Hayır (varsayılan değer 0 - otomatik algılandı) |
| preCopyScript |Her çalıştırmada veri deposuna veri yazmadan önce yürütülmesi için Kopyalama Etkinliği için bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |

> [!NOTE]
> "writeBatchSize" için, ayarlanmamışsa (otomatik algılanmadıysa), kopyalama etkinliği önce sürücünün toplu iş işlemlerini destekleyip desteklemediğini algılar ve varsa 10000'e ayarlar veya yoksa 1 olarak ayarlar. Değeri 0'dan başka bir şekilde açıkça ayarlarsanız, kopyalama etkinliği değeri onurlandırır ve sürücü toplu iş işlemlerini desteklemiyorsa çalışma zamanında başarısız olur.

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

## <a name="sap-hana-sink"></a>SAP HANA lavabo

>[!NOTE]
>SAP HANA veri deposundan veri kopyalamak için yerel [SAP HANA bağlayıcısına](connector-sap-hana.md)bakın. Verileri SAP HANA'ya kopyalamak için, ODBC bağlayıcısını kullanmak için lütfen bu yönergeyi uygulayın. SAP HANA konektörü ve ODBC konektörü için bağlantılı hizmetlerin farklı türde olduğunu, bu nedenle yeniden kullanılamayacağını unutmayın.
>

Genel ODBC bağlayıcısını kullanarak verileri SAP HANA veritabanına kopyalayabilirsiniz.

Veri deponuza erişimi olan bir makinede kendi kendine barındırılan Bir Tümleştirme Çalışma Zamanı ayarlayın. Tümleştirme Runtime veri deposuna bağlanmak için SAP HANA için ODBC sürücüsünü kullanır. Bu nedenle, aynı makineye zaten yüklenmediyse sürücüyü yükleyin. Ayrıntılar için [Ön koşullar](#prerequisites) bölümüne bakınız.

Bir Veri Fabrikası çözümünde SAP HANA lavabosu kullanmadan önce, Sorun [Giderme bağlantısı sorunları](#troubleshoot-connectivity-issues) bölümündeki yönergeleri kullanarak Tümleştirme Runtime'ın veri deposuna bağlanıp bağlanamayacağını doğrulayın.

Bir SAP HANA veri deposunu aşağıdaki örnekte gösterildiği gibi bir Azure veri fabrikasına bağlamak için ODBC bağlantılı bir hizmet oluşturun:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015",
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

Bir kopyalama işleminde ODBC veri depolarını kaynak/lavabo veri depoları olarak kullanmanın ayrıntılı bir özeti için makaleyi baştan okuyun.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme sorunları

Bağlantı sorunlarını gidermek **için, Tümleştirme Runtime Configuration** **Manager'ın Tanılama** sekmesini kullanın.

1. **Başlat Tümleştirme Runtime Configuration Manager**.
2. **Tanılama** sekmesine geçin.
3. "Bağlantı Test" bölümünde, veri deposu **türünü** (bağlantılı hizmet) seçin.
4. Veri deposuna bağlanmak için kullanılan **bağlantı dizesini** belirtin, **kimlik doğrulamasını** seçin ve **kullanıcı adı,** **parola**ve/veya **kimlik bilgilerini**girin.
5. Veri deposuna olan bağlantıyı sınamak için **Test bağlantısını** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
