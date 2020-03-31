---
title: Azure Veri Fabrikası'nı kullanarak verileri MySQL'den taşıma
description: Azure Veri Fabrikası'nı kullanarak mySQL veritabanından verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/06/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 90fccba016a3db9ff85f8ec7c8fd426ef3c896a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281294"
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri MySQL'den taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-onprem-mysql-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-mysql.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki MySQL konektörüne](../connector-mysql.md)bakın.


Bu makalede, verileri şirket içi MySQL veritabanından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri şirket içi MySQL veri deposundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca mysql veri deposundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından MySQL veri deposuna taşımak için değildir. 

## <a name="prerequisites"></a>Ön koşullar
Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi MySQL kaynaklarına bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün.

MySQL veritabanı bir Azure IaaS sanal makinede (VM) barındırılabilse bile ağ geçidi gereklidir. Ağ geçidi veritabanına bağlanabildiği sürece ağ geçidini veri deposuyla aynı VM'ye veya farklı bir VM'ye yükleyebilirsiniz.

> [!NOTE]
> Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

## <a name="supported-versions-and-installation"></a>Desteklenen sürümler ve kurulum
Veri Yönetimi Ağ Geçidi'nin MySQL Veritabanı'na bağlanması için, Veri Yönetimi Ağ Geçidi ile aynı sisteme [Microsoft Windows için MySQL Bağlayıcısı/NET](https://dev.mysql.com/downloads/connector/net/) 'i (6.6.5 ile 6.10.7 arasındaki sürüm) yüklemeniz gerekir. Bu 32 bit sürücü 64 bit Veri Yönetimi Ağ Geçidi ile uyumludur. MySQL sürüm 5.1 ve üzeri desteklenir.

> [!TIP]
> "Uzak taraf aktarım akışını kapattığı için kimlik doğrulama başarısız oldu" hatasına ulaştıysanız, MySQL Bağlayıcısı/NET'i daha yüksek sürüme yükseltmeyi düşünün.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi Cassandra veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz. 

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Şirket içi MySQL veri deposundaki verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: MySQL'den Azure Blob bölümüne verileri kopyalayın.](#json-example-copy-data-from-mysql-to-azure-blob) 

Aşağıdaki bölümler, MySQL veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, MySQL bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesMySql** |Evet |
| sunucu |MySQL sunucusunun adı. |Evet |
| database |MySQL veritabanının adı. |Evet |
| Şema |Veritabanındaki şemanın adı. |Hayır |
| authenticationType |MySQL veritabanına bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler `Basic`şunlardır: . |Evet |
| userName |MySQL veritabanına bağlanmak için kullanıcı adını belirtin. |Evet |
| password |Belirttiğiniz kullanıcı hesabıiçin parola belirtin. |Evet |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi MySQL veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **Tür İlişkisel Table** veri kümesi için typeProperties bölümü (MySQL dataset içeren) aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |MySQL Veritabanı örneğinde bağlı hizmetin başvurulması gereken tablonun adı. |Hayır **(RelationalSource** **sorgusu** belirtilirse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları gibi özellikler, tüm etkinlik türleri için ilkeler mevcuttur.

Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğindeki kaynak **RelationalSource** türünden (MySQL içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: MyTable'dan * seçin. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a>JSON örneği: MySQL'den Azure Blob'a veri kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Şirket içi MySQL veritabanından Azure Blob Depolama'ya verilerin nasıl kopyalanır şekilde kopyalanırolduğunu gösterir. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

> [!IMPORTANT]
> Bu örnek JSON parçacıkları sağlar. Veri fabrikası oluşturmak için adım adım yönergeler içermez. Adım adım yönergeler için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında hareketli veri bakın.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [İlişkisel Tablo](data-factory-onprem-mysql-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, MySQL veritabanındaki bir sorgu sonucundaki verileri saatlik bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini kur. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**MySQL bağlantılı hizmet:**

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

**Azure Depolama bağlantılı hizmet:**

```JSON
    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }
```

**MySQL giriş veri seti:**

Örnek, MySQL'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir.

"Dış"ı ayarlamak: "true" tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, kopyalanacak son saatteki verileri seçer.

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a>MySQL için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri MySQL'e aktarırken, MySQL türlerinden .NET türlerine aşağıdaki eşlemeler kullanılır.

| MySQL Veritabanı türü | .NET Framework türü |
| --- | --- |
| bigint imzasız |Ondalık |
| bigint |Int64 |
| bit |Ondalık |
| blob |Bayt[] |
| bool |Boole |
| char |Dize |
| date |Tarih saat |
| datetime |Tarih saat |
| decimal |Ondalık |
| çift hassasiyet |Çift |
| double |Çift |
| enum |Dize |
| float |Tek |
| int imzasız |Int64 |
| int |Int32 |
| imzasız sonda |Int64 |
| integer |Int32 |
| uzun varbinary |Bayt[] |
| uzun varchar |Dize |
| longblob |Bayt[] |
| Longtext |Dize |
| mediumblob |Bayt[] |
| mediumint imzasız |Int64 |
| mediumint |Int32 |
| mediumtext |Dize |
| sayısal |Ondalık |
| gerçek |Çift |
| set |Dize |
| küçük imzasız |Int32 |
| smallint |Int16 |
| metin |Dize |
| time |TimeSpan |
| timestamp |Tarih saat |
| miniblob |Bayt[] |
| tinyint imzasız |Int16 |
| tinyint |Int16 |
| tinytext |Dize |
| varchar |Dize |
| yıl |int |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
