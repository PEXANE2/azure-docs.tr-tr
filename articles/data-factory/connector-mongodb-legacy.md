---
title: Eski yi kullanarak MongoDB'deki verileri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına Mongo DB'deki verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 08/12/2019
ms.openlocfilehash: 0bdd8d454b979250b57cf657d347309b99a86ede
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75892565"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak MongoDB'deki verileri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-on-premises-mongodb-connector.md)
> * [Geçerli sürüm](connector-mongodb.md)

Bu makalede, MongoDB veritabanındaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!IMPORTANT]
>ADF, bu ODBC tabanlı uygulama ile karşılaştırıldığında daha iyi yerli MongoDB desteği sağlayan yeni bir MongoDB bağlayıcısı yayımlayın, ayrıntılar la ilgili [MongoDB bağlayıcı](connector-mongodb.md) sı makaleye bakın. Bu eski MongoDB konektörü geriye dönük uyumluluk için olduğu gibi desteklenirken, yeni iş yükü için lütfen yeni bağlayıcıyı kullanın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

MongoDB veritabanındaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu MongoDB bağlayıcı destekler:

- MongoDB **sürümleri 2.4, 2.6, 3.0, 3.2, 3.4 ve 3.6**.
- **Temel** veya **Anonim** kimlik doğrulamasını kullanarak verileri kopyalama.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

Tümleştirme Runtime yerleşik bir MongoDB sürücüsü sağlar, bu nedenle MongoDB verileri kopyalarken herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, MongoDB bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

MongoDB bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği ayarlanmalıdır: **MongoDb** |Evet |
| sunucu |MongoDB sunucusunun IP adresi veya ana bilgisayar adı. |Evet |
| port |MongoDB sunucusunun istemci bağlantılarını dinlemek için kullandığı TCP bağlantı noktası. |Hayır (varsayılan değer 27017'dir) |
| Databasename |Erişmek istediğiniz MongoDB veritabanının adı. |Evet |
| authenticationType | MongoDB veritabanına bağlanmak için kullanılan kimlik doğrulama türü.<br/>İzin verilen değerler şunlardır: **Temel**ve **Anonim**. |Evet |
| kullanıcı adı |MongoDB'ye erişmek için kullanıcı hesabı. |Evet (temel kimlik doğrulama kullanılıyorsa). |
| password |Kullanıcının parolası. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet (temel kimlik doğrulama kullanılıyorsa). |
| authSource |Kimlik doğrulama için kimlik bilgilerinizi denetlemek için kullanmak istediğiniz MongoDB veritabanının adı. |Hayır. Temel kimlik doğrulama için varsayılan değer, yönetici hesabını ve databaseName özelliği kullanılarak belirtilen veritabanını kullanmaktır. |
| sağlarSsl | Sunucuya bağlantıların SSL kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer false'tur.  | Hayır |
| izinSelfSignedServerCert | Sunucudan kendi imzalı sertifikalara izin verip vermemeyi belirtir. Varsayılan değer false'tur.  | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

**Örnek:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDb",
        "typeProperties": {
            "server": "<server name>",
            "databaseName": "<database name>",
            "authenticationType": "Basic",
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) Aşağıdaki özellikler MongoDB veri kümesi için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği ayarlanmalıdır: **MongoDbCollection** | Evet |
| Toplamaadı |MongoDB veritabanındaki koleksiyonun adı. |Evet |

**Örnek:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<Collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde MongoDB kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="mongodb-as-source"></a>Kaynak olarak MongoDB

Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **MongoDbSource** | Evet |
| sorgu |Verileri okumak için özel SQL-92 sorgusunu kullanın. Örneğin: MyTable'dan * seçin. |Hayır (veri kümesinde "collectionName" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

> [!TIP]
> SQL sorgusunu belirtirken, DateTime biçimine dikkat edin. Örneğin: `SELECT * FROM Account WHERE LastModifiedDate >= '2018-06-01' AND LastModifiedDate < '2018-06-02'` veya parametre kullanmak için`SELECT * FROM Account WHERE LastModifiedDate >= '@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}' AND LastModifiedDate < '@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'`

## <a name="schema-by-data-factory"></a>Veri Fabrikası'na Göre Şema

Azure Veri Fabrikası hizmeti, koleksiyondaki **en son 100 belgeyi** kullanarak bir MongoDB koleksiyonundan şema çıkar. Bu 100 belge tam şema içermiyorsa, kopyalama işlemi sırasında bazı sütunlar yoksayılabilir.

## <a name="data-type-mapping-for-mongodb"></a>MongoDB için veri türü eşleme

MongoDB'den veri kopyalanırken, MongoDB veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| MongoDB veri türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| İkili |Bayt[] |
| Boole |Boole |
| Tarih |DateTime |
| SayıÇift |Çift |
| NumberInt |Int32 |
| Sayı Uzun |Int64 |
| ObjectID |Dize |
| Dize |Dize |
| Uuıd |Guid |
| Nesne |İç içe ayırıcı olarak "_" ile düzleştirilmiş sütunlar halinde yeniden normalleştirildi |

> [!NOTE]
> Sanal tabloları kullanarak dizidesteği hakkında bilgi edinmek [için, sanal tablolar](#support-for-complex-types-using-virtual-tables) bölümünü kullanarak karmaşık türler için Destek bölümüne bakın.
>
> Şu anda, aşağıdaki MongoDB veri türleri desteklenmez: DBPointer, JavaScript, Max/Min tuşu, Düzenli İfade, Sembol, Zaman Damgası, Tanımsız.

## <a name="support-for-complex-types-using-virtual-tables"></a>Sanal tabloları kullanarak karmaşık türleri için destek

Azure Veri Fabrikası, MongoDB veritabanınızdaki verilere bağlanmak ve kopyalamak için yerleşik bir ODBC sürücüsü kullanır. Belgeler arasında farklı türlerde diziler veya nesneler gibi karmaşık türler için sürücü verileri karşılık gelen sanal tablolara yeniden normalleştirir. Özellikle, bir tablo bu tür sütunlar içeriyorsa, sürücü aşağıdaki sanal tabloları oluşturur:

* Karmaşık tür sütunları dışında gerçek tabloyla aynı verileri içeren **bir taban tablosu.** Taban tablo, temsil ettiği gerçek tabloyla aynı adı kullanır.
* İç içe geçen verileri genişleten her karmaşık tür sütunu için sanal bir **tablo.** Sanal tablolar gerçek tablo, bir ayırıcı "_" ve dizi veya nesnenin adı kullanılarak adlandırılır.

Sanal tablolar, sürücünün normalden arındırılmış verilere erişmesini sağlayarak gerçek tablodaki verilere başvurur. Sanal tabloları sorgulayarak ve birleştirerek MongoDB dizilerinin içeriğine erişebilirsiniz.

### <a name="example"></a>Örnek

Örneğin, ExampleTable burada her hücrede nesnelerin bir dizi ile bir sütun olan bir MongoDB tablo - Faturalar ve Skaler türleri bir dizi ile bir sütun - Ratings.

| _id | Müşteri Adı | Faturalar | Hizmet Düzeyi | Derecelendirmeler |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id:"123", item:"tost makinesi", fiyat:"456", indirimli:"0.2"}, {invoice_id:"124", öğe:"fırın", fiyat: "1235", indirim: "0.2"}] |Silver |[5,6] |
| 2222 |XYZ |[{invoice_id:"135", item:"buzdolabı", fiyat: "12543", indirim: "0.0"}] |Gold |[1,2] |

Sürücü, bu tek tabloyu temsil edecek birden çok sanal tablo oluşturur. İlk sanal tablo, örnekte gösterilen "ExampleTable" adlı taban tablodur. Temel tablo özgün tablonun tüm verilerini içerir, ancak dizilerden gelen veriler atlanmıştır ve sanal tablolarda genişletilir.

| _id | Müşteri Adı | Hizmet Düzeyi |
| --- | --- | --- |
| 1111 |ABC |Silver |
| 2222 |XYZ |Gold |

Aşağıdaki tablolar, örnekteki özgün dizileri temsil eden sanal tabloları gösterir. Bu tablolar aşağıdakileri içerir:

* Özgün dizinin satırına karşılık gelen özgün birincil anahtar sütununa (_id sütun üzerinden) geri gönderme
* Orijinal dizi içindeki verilerin konumunun bir göstergesi
* Dizi içindeki her öğe için genişletilmiş veri

**Tablo "ExampleTable_Invoices":**

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | öğe | price | İndirim |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |Tost makinesi |456 |0.2 |
| 1111 |1 |124 |Fırın |1235 |0.2 |
| 2222 |0 |135 |Buzdolabı |12543 |0,0 |

**Tablo "ExampleTable_Ratings":**

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
