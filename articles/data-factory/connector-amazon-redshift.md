---
title: Amazon Redshift'ten veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak Amazon Redshift'ten desteklenen lavabo veri depolarına verileri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 4d729a0117c7c409d1a3e0c3fd440aed96153203
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243594"
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Amazon Redshift'ten veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-amazon-redshift-connector.md)
> * [Geçerli sürüm](connector-amazon-redshift.md)


Bu makalede, Bir Amazon Redshift verileri kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği nasıl kullanılacağı nı özetler. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Amazon Redshift konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Amazon Redshift'ten desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Amazon Redshift konektörü sorgu veya yerleşik Redshift UNLOAD desteği kullanarak Redshift veri alma destekler.

> [!TIP]
> Redshift'ten büyük miktarda veriyi kopyalarken en iyi performansı elde etmek için, yerleşik Redshift UNLOAD'ı Amazon S3 üzerinden kullanmayı düşünün. Bkz. Ayrıntılar [için Amazon Redshift bölümünden verileri kopyalamak için UNLOAD'ı kullanın.](#use-unload-to-copy-data-from-amazon-redshift)

## <a name="prerequisites"></a>Ön koşullar

* [Kendi kendine barındırılan Tümleştirme Runtime](create-self-hosted-integration-runtime.md)kullanarak bir şirket içi veri deposuna veri kopyalıyorsanız, Tümleştirme Runtime (makinenin IP adresini kullanın) Amazon Redshift küme erişimi verin. Bkz. Yönergeler için [kümeye erişimi yetkilendirme.](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html)
* Verileri bir Azure veri deposuna kopyalıyorsanız, Azure veri merkezleri tarafından kullanılan İşlem IP adresi ve SQL aralıkları için [Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653) bakın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Amazon Redshift konektörüne özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Amazon Redshift bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **AmazonRedshift** | Evet |
| sunucu |Amazon Redshift sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| port |Amazon Redshift sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktasının sayısı. |Hayır, varsayılan 5439 |
| database |Amazon Redshift veritabanının adı. |Evet |
| kullanıcı adı |Veritabanına erişimi olan kullanıcının adı. |Evet |
| password |Kullanıcı hesabının şifresi. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "<server name>",
            "database": "<database name>",
            "username": "<username>",
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

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Amazon Redshift veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Amazon Redshift'ten verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **AmazonRedshiftTable** | Evet |
| Şema | Şema adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Şema ile tablonun adı. Bu özellik geriye dönük uyumluluk için desteklenir. Kullanın `schema` `table` ve yeni iş yükü için. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "AmazonRedshiftDataset",
    "properties":
    {
        "type": "AmazonRedshiftTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Amazon Redshift linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Dakti-zimd veri kümesi kullanıyorsanız, `RelationalTable` ileriye dönük yenisini kullanmanız önerilirken, yine de olduğu gibi desteklenir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Amazon Redshift kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="amazon-redshift-as-source"></a>Kaynak olarak Amazon Redshift

Amazon Redshift verileri kopyalamak için, **AmazonRedshiftSource**için kopyalama etkinliğinde kaynak türünü ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **AmazonRedshiftSource** | Evet |
| sorgu |Verileri okumak için özel sorguyu kullanın. Örneğin: MyTable'dan * seçin. |Hayır (veri kümesinde "tablo Adı" belirtilirse) |
| redshiftUnloadAyarlar | Amazon Redshift UNLOAD kullanırken özellik grubu. | Hayır |
| s3LinkedServiceName | "AmazonS3" türünde bağlantılı bir hizmet adı belirterek geçici mağaza olarak kullanılacak bir Amazon S3 anlamına gelir. | UNLOAD kullanıyorsanız Evet |
| bucketName | Ara verileri depolamak için S3 kovasını gösterin. Sağlanmazsa, Veri Fabrikası hizmeti otomatik olarak oluşturur.  | UNLOAD kullanıyorsanız Evet |

**Örnek: UNLOAD kullanarak kopyalama etkinliğinde Amazon Redshift kaynak**

```json
"source": {
    "type": "AmazonRedshiftSource",
    "query": "<SQL query>",
    "redshiftUnloadSettings": {
        "s3LinkedServiceName": {
            "referenceName": "<Amazon S3 linked service>",
            "type": "LinkedServiceReference"
        },
        "bucketName": "bucketForUnload"
    }
}
```

Amazon Redshift'teki verileri verimli bir şekilde bir sonraki bölümden kopyalamak için UNLOAD'ı nasıl kullanacağınız hakkında daha fazla bilgi edinin.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Amazon Redshift'ten verileri kopyalamak için UNLOAD'ı kullanın

[UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) Amazon Basit Depolama Servisi (Amazon S3) bir veya daha fazla dosyaya bir sorgu sonuçlarını boşaltabilirsiniz Amazon Redshift tarafından sağlanan bir mekanizmadır. Bu şekilde Redshift büyük veri seti kopyalamak için Amazon tarafından tavsiye edilir.

**Örnek: Verileri Amazon Redshift'ten Azure SQL Veri Ambarı'na UNLOAD, aşamalı kopya ve PolyBase kullanarak kopyalama**

Bu örnek kullanım örneği için, kopyalama etkinliği Amazon Redshift'ten Amazon S3'e "redshiftUnloadSettings"de yapılandırıldığı şekilde verileri boşaltır ve ardından "evrelemeAyarları"nda belirtildiği gibi Amazon S3'ten Azure Blob'a verileri kopyalayın, son olarak verileri SQL Verilerine yüklemek için PolyBase'i kullanın Ambar. Tüm ara biçim kopyalama etkinliği tarafından düzgün bir şekilde işlenir.

![SQL DW kopya iş akışına redshift](media/copy-data-from-amazon-redshift/redshift-to-sql-dw-copy-workflow.png)

```json
"activities":[
    {
        "name": "CopyFromAmazonRedshiftToSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "AmazonRedshiftDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AmazonRedshiftSource",
                "query": "select * from MyTable",
                "redshiftUnloadSettings": {
                    "s3LinkedServiceName": {
                        "referenceName": "AmazonS3LinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "bucketName": "bucketForUnload"
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "AzureStorageLinkedService",
                "path": "adfstagingcopydata"
            },
            "dataIntegrationUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>Amazon Redshift için veri türü eşleme

Amazon Redshift'teki verileri kopyalarken, Aşağıdaki eşlemeler Amazon Redshift veri türlerinden Azure Veri Fabrikası geçici veri türlerine kadar kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| Amazon Redshift veri türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| Bigint |Int64 |
| Boolean |Dize |
| Char |Dize |
| DATE |DateTime |
| On -da -lık |Ondalık |
| ÇİFT HASSASLIK |Çift |
| TAMSAYI |Int32 |
| GERÇEK SAYI |Tek |
| Smallint |Int16 |
| TEXT |Dize |
| Zaman damgası |DateTime |
| Varchar |Dize |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
