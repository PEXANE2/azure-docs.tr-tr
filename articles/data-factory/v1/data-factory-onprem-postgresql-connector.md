---
title: Azure Veri Fabrikasını kullanarak verileri PostgreSQL'den taşıma
description: Azure Veri Fabrikası'nı kullanarak PostgreSQL Veritabanı'ndan verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 37c83e77cadae002ff701a08c4b36a86f7cab9a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281242"
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri PostgreSQL'den taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-postgresql-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-postgresql.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki PostgreSQL konektörüne](../connector-postgresql.md)bakın.


Bu makalede, verileri şirket içi PostgreSQL veritabanından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Şirket içi PostgreSQL veri deposundaki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](data-factory-data-movement-activities.md#supported-data-stores-and-formats)bakın. Veri fabrikası şu anda verileri PostgreSQL veritabanından diğer veri depolarına taşımayı destekler, ancak verileri diğer veri depolarından PostgreSQL veritabanına taşımayı desteklemez.

## <a name="prerequisites"></a>Ön koşullar

Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi PostgreSQL kaynaklarına bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün.

PostgreSQL veritabanı bir Azure IaaS VM'de barındırılan olsa bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı IaaS VM'ye veya farklı bir VM'ye yükleyebilirsiniz.

> [!NOTE]
> Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum
Veri Yönetimi Ağ Geçidi'nin PostgreSQL Veritabanı'na bağlanması için, Veri Yönetimi Ağ Geçidi ile aynı sistemde 2.0.12 ile 3.1.9 arasında sürüm içeren [PostgreSQL için Ngpsql veri sağlayıcısını](https://go.microsoft.com/fwlink/?linkid=282716) yükleyin. PostgreSQL sürüm 7.4 ve üzeri desteklenir.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi PostgreSQL veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)
- Bir ardışık hatlar oluşturmak için aşağıdaki araçları da kullanabilirsiniz:
  - Visual Studio
  - Azure PowerShell
  - Azure Resource Manager şablonu
  - .NET API’si
  - REST API

    Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Şirket içi PostgreSQL veri deposundan verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Bu makalenin PostgreSQL'den Azure Blob bölümüne verileri kopyalayın.](#json-example-copy-data-from-postgresql-to-azure-blob)

Aşağıdaki bölümler, PostgreSQL veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, PostgreSQL bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesPostgreSql** |Evet |
| sunucu |PostgreSQL sunucusunun adı. |Evet |
| database |PostgreSQL veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. Şema adı büyük/küçük harf duyarlıdır. |Hayır |
| authenticationType |PostgreSQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim, Temel ve Windows. |Evet |
| kullanıcı adı |Temel veya Windows kimlik doğrulaması kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi PostgreSQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **Tür İlişkisel Table** veri kümesi için typeProperties bölümü (PostgreSQL veri kümesini içerir) aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin başvurulması olan PostgreSQL Veritabanı örneğindeki tablonun adı. Tablo Adı büyük/küçük harf duyarlıdır. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak **RelationalSource** türünden (PostgreSQL'i içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: `"query": "select * from \"MySchema\".\"MyTable\""`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

> [!NOTE]
> Şema ve masa adları büyük/küçük harf duyarlıdır. Sorguda `""` (çift tırnak) bunları içine alayın.

**Örnek:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>JSON örneği: PostgreSQL'den Azure Blob'a veri kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. PostgreSQL veritabanından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterirler. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [İlişkisel Tablo](data-factory-onprem-postgresql-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity ile [boru hattı.](data-factory-create-pipelines.md)

Örnek, PostgreSQL veritabanındaki bir sorgu sonucundaki verileri her saat başı bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini ayarlayın. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**PostgreSQL bağlantılı hizmet:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
**Azure Blob depolama bağlantılı hizmet:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```
**PostgreSQL giriş veri seti:**

Örnek, PostgreSQL'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası" adı verilen bir sütun içerir.

Ayar, `"external": true` Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama etkinliği olan Boru Hattı:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve saatlik çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, PostgreSQL veritabanındaki public.usstates tablosundaki verileri seçer.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>PostgreSQL için tür eşleme
[Veri hareketi etkinliklerinde](data-factory-data-movement-activities.md) belirtildiği gibi makale Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri PostgreSQL'e aktarırken, PostgreSQL türünden .NET türüne aşağıdaki eşlemeler kullanılır.

| PostgreSQL Veritabanı türü | PostgresSQL takma adları | .NET Framework türü |
| --- | --- | --- |
| abstime | |Tarih saat |
| bigint |int8 |Int64 |
| bigserial |seri8 |Int64 |
| bit [(n)] | |Bayt[], String |
| bit değişen [ (n) ] |varbit |Bayt[], String |
| boole |bool |Boole |
| Kutusu | |Bayt[], String |
| bytea | |Bayt[], String |
| karakter [(n)] |char [(n)] |Dize |
| karakter değişen [(n)] |varchar [(n)] |Dize |
| Cid | |Dize |
| cidr | |Dize |
| daire | |Bayt[], String |
| date | |Tarih saat |
| tarih aralığı | |Dize |
| çift hassasiyet |float8 |Çift |
| inet | |Bayt[], String |
| intarry | |Dize |
| int4range | |Dize |
| int8range | |Dize |
| integer |int, int4 |Int32 |
| aralık [alanlar] [(p)] | |Timespan |
| json | |Dize |
| jsonb | |Bayt[] |
| line | |Bayt[], String |
| lseg | |Bayt[], String |
| macaddr | |Bayt[], String |
| Para | |Ondalık |
| sayısal [(p, s)] |ondalık [(p, s)] |Ondalık |
| numrange | |Dize |
| Oıd | |Int32 |
| yol | |Bayt[], String |
| pg_lsn | |Int64 |
| Nokta | |Bayt[], String |
| Çokgen | |Bayt[], String |
| gerçek |float4 |Tek |
| smallint |int2 |Int16 |
| smallserial |seri2 |Int16 |
| Seri |seri4 |Int32 |
| metin | |Dize |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
