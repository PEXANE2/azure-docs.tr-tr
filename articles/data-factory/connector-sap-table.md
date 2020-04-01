---
title: SAP tablosundan veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopya etkinliği kullanarak desteklenen lavabo veri depolarına SAP tablosundaki verileri nasıl kopyalayatılayış edin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/02/2019
ms.openlocfilehash: 65e51258b3a24b14b5171968645e88420e92dd5a
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421067"
---
# <a name="copy-data-from-an-sap-table-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak SAP tablosundan veri kopyalama

Bu makalede, bir SAP tablosundaki verileri kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Daha fazla bilgi için [bkz.](copy-activity-overview.md)

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SAP tablo bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri SAP tablosundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP tablo bağlayıcıdestekler:

- Sap tablosundaki verileri aşağıdaki ler arasında kopyalama:

  - SAP ERP Merkezi Bileşeni (SAP ECC) sürüm 7.01 veya sonraki sürüm (2015'ten sonra yayımlanan son SAP Destek Paketi Yığını'nda).
  - SAP Business Warehouse (SAP BW) sürüm 7.01 veya sonraki sürüm (2015'ten sonra yayımlanan son SAP Destek Paketi Yığını'nda).
  - SAP S/4HANA.
  - SAP Business Suite sürüm 7.01 veya sonraki diğer ürünler (2015'ten sonra yayımlanan son SAP Destek Paketi Yığını'nda).

- Hem SAP saydam tablosundan, hem havuza alınan tablodan, kümelenmiş tablodan ve görünümden veri kopyalama.
- SNC yapılandırılmışsa, temel kimlik doğrulama veya Güvenli Ağ İletişimi (SNC) kullanarak verileri kopyalama.
- BIR SAP uygulama sunucusuna veya SAP ileti sunucusuna bağlanma.

## <a name="prerequisites"></a>Ön koşullar

Bu SAP tablo konektörünü kullanmak için şunları yapmanız gerekir:

- Kendi kendine barındırılan bir tümleştirme çalışma zamanı (sürüm 3.17 veya daha sonra) ayarlayın. Daha fazla bilgi için [bkz.](create-self-hosted-integration-runtime.md)

- [Microsoft .NET 3.0 için](https://support.sap.com/en/product/connectors/msnet.html) 64 bit SAP Bağlayıcısını SAP'nin web sitesinden indirin ve kendi barındırılan tümleştirme çalışma zamanı makinesine yükleyin. Yükleme sırasında, **İsteğe Bağlı kurulum adımları** penceresinde **GAC'ye Montajları Yükle** seçeneğini seçtiğinizden emin olun.

  ![.NET için SAP Bağlayıcısı'nı yükleyin](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Veri Fabrikası SAP tablo konektöründe kullanılan SAP kullanıcısının aşağıdaki izinlere sahip olması gerekir:

  - Uzaktan İşlev Arama (RFC) hedeflerini kullanma yetkisi.
  - S_SDSAUTH yetkilendirme nesnesinin Yürütme etkinliği için izinler.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, SAP tablo bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

SAP BW Open Hub bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `SapTable`Özellik' `type` in . | Evet |
| `server` | SAP örneğinin bulunduğu sunucunun adı.<br/>SAP uygulama sunucusuna bağlanmak için kullanın. | Hayır |
| `systemNumber` | SAP sisteminin sistem numarası.<br/>SAP uygulama sunucusuna bağlanmak için kullanın.<br/>İzin verilen değer: Dize olarak temsil edilen iki basamaklı ondalık sayı. | Hayır |
| `messageServer` | SAP ileti sunucusunun ana bilgisayar adı.<br/>SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `messageServerService` | İleti sunucusunun servis adı veya bağlantı noktası numarası.<br/>SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `systemId` | Tablonun bulunduğu SAP sisteminin kimliği.<br/>SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `logonGroup` | SAP sisteminin oturum açma grubu.<br/>SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `clientId` | SAP sistemindeki istemcinin kimliği.<br/>İzin verilen değer: Dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| `language` | SAP sisteminin kullandığı dil.<br/>Varsayılan değer. `EN`| Hayır |
| `userName` | SAP sunucusuna erişimi olan kullanıcının adı. | Evet |
| `password` | Kullanıcının parolası. Bu alanı Veri `SecureString` Fabrikası'nda güvenli bir şekilde depolamak için türüyle işaretleyin veya [Azure Key Vault'ta depolanan bir gizli ye başvuruyapın.](store-credentials-in-key-vault.md) | Evet |
| `sncMode` | Tablonun bulunduğu SAP sunucusuna erişmek için SNC etkinleştirme göstergesi.<br/>SAP sunucusuna bağlanmak için SNC kullanmak istiyorsanız kullanın.<br/>İzin verilen `0` değerler (kapalı, `1` varsayılan) veya (çevrimiçi) | Hayır |
| `sncMyName` | Tablonun bulunduğu SAP sunucusuna erişmek için başlatıcının SNC adı.<br/>Anın `sncMode` ası olduğunda geçerlidir. | Hayır |
| `sncPartnerName` | Tablonun bulunduğu SAP sunucusuna erişmek için iletişim ortağının SNC adı.<br/>Anın `sncMode` ası olduğunda geçerlidir. | Hayır |
| `sncLibraryPath` | Tablonun bulunduğu SAP sunucusuna erişmek için dış güvenlik ürün kitaplığı.<br/>Anın `sncMode` ası olduğunda geçerlidir. | Hayır |
| `sncQop` | SNC Koruma Kalitesi düzeyi uygulanacak.<br/>Azaman `sncMode` olduğunda geçerlidir. <br/>İzin verilen `1` değerler (Kimlik `2` Doğrulama), `3` (Bütünlük), (Gizlilik), `8` (Varsayılan), `9` (Maksimum). | Hayır |
| `connectVia` | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Daha önce [Önkoşullar'da](#prerequisites)belirtildiği gibi, kendi kendine barındırılan bir tümleştirme çalışma süresi gereklidir. |Evet |

**Örnek 1: Sap uygulama sunucusuna bağlanma**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="example-2-connect-to-an-sap-message-server"></a>Örnek 2: SAP ileti sunucusuna bağlanma

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system ID>",
            "logonGroup": "<logon group>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="example-3-connect-by-using-snc"></a>Örnek 3: SNC kullanarak bağlanın

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "<SNC myname>",
            "sncPartnerName": "<SNC partner name>",
            "sncLibraryPath": "<SNC library path>",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri.](concepts-datasets-linked-services.md) Aşağıdaki bölümde SAP tablo veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SAP BW Open Hub bağlantılı hizmetten ve SAP BW Open Hub bağlantılı hizmetten verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `SapTableResource`Özellik' `type` in . | Evet |
| `tableName` | Verileri kopyalamak için SAP tablosunun adı. | Evet |

### <a name="example"></a>Örnek

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "typeProperties": {
            "tableName": "<SAP table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP table linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) Aşağıdaki bölümde SAP tablo kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-table-as-source"></a>Kaynak olarak SAP tablosu

SAP tablosundaki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik                         | Açıklama                                                  | Gerekli |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| `type`                             | `SapTableSource`Özellik' `type` in .         | Evet      |
| `rowCount`                         | Alınacak satır sayısı.                              | Hayır       |
| `rfcTableFields`                   | SAP tablosundan kopyalanması gereken alanlar (sütunlar). Örneğin, `column0, column1`. | Hayır       |
| `rfcTableOptions`                  | SAP tablosundaki satırları filtreleme seçenekleri. Örneğin, `COLUMN0 EQ 'SOMEVALUE'`. Bu makalenin ilerleyen saatlerinde SAP sorgu işleci tablosuna da bakın. | Hayır       |
| `customRfcReadTableFunctionModule` | SAP tablosundan verileri okumak için kullanılabilecek özel bir RFC işlev modülü.<br>Verilerin SAP sisteminizden nasıl alınıp Veri Fabrikası'na döndürülebileceğini tanımlamak için özel bir RFC işlev modülü kullanabilirsiniz. Özel işlev modülü, Veri Fabrikası tarafından kullanılan varsayılan arabirim olan `/SAPDS/RFC_READ_TABLE2`abirime benzer bir arabirime (alma, dışa aktarma, tablolar) uygulanmalıdır. | Hayır       |
| `partitionOption`                  | SAP tablosundan okunacak bölme mekanizması. Desteklenen seçenekler şunlardır: <ul><li>`None`</li><li>`PartitionOnInt`(normal tamsayı veya tamsayı değerleri, solda sıfır dolgu `0000012345`ile)</li><li>`PartitionOnCalendarYear`("YYYY" biçiminde 4 basamak)</li><li>`PartitionOnCalendarMonth`("YYYYMM" biçiminde 6 basamak)</li><li>`PartitionOnCalendarDate`("YYYYMMDD" biçiminde 8 basamak)</li></ul> | Hayır       |
| `partitionColumnName`              | Verileri bölmek için kullanılan sütunun adı.                | Hayır       |
| `partitionUpperBound`              | Bölümleme ile devam etmek `partitionColumnName` için kullanılacak olan sütunun en büyük değeri. | Hayır       |
| `partitionLowerBound`              | Bölümleme ile devam etmek `partitionColumnName` için kullanılacak tır. (Not: `partitionLowerBound` bölüm seçeneği olduğunda "0" `PartitionOnInt`olamaz) | Hayır       |
| `maxPartitionsNumber`              | Verileri bölmek için en fazla bölüm sayısı.     | Hayır       |

>[!TIP]
>SAP tablonuzda birkaç milyar satır gibi büyük bir veri `partitionOption` `partitionSetting` hacmi varsa, verileri daha küçük bölümlere bölmek ve kullanın. Bu durumda, veriler bölüm başına okunur ve her veri bölümü SAP sunucunuzdan tek bir RFC çağrısı yla alınır.<br/>
<br/>
>Örnek `partitionOption` `partitionOnInt` olarak ele alınan, her bölümdeki satır sayısı şu formülle hesaplanır: `partitionUpperBound` (toplam`maxPartitionsNumber`satırlar arasında düşen ve)/. `partitionLowerBound`<br/>
<br/>
>Kopyayı hızlandırmak için veri bölümlerini paralel olarak yüklemek için, paralel derece kopyalama etkinliği ndeki [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) ayar tarafından denetlenir. Örneğin, dörde `parallelCopies` ayarlarsanız, Veri Fabrikası aynı anda belirtilen bölüm seçeneğiniz ve ayarlarınızı temel alan dört sorgu oluşturur ve çalıştırZ ve her sorgu SAP tablonuzdan verilerin bir kısmını alır. Mülkün değerinin bir `maxPartitionsNumber` katını oluşturmanızı `parallelCopies` şiddetle öneririz. Verileri dosya tabanlı veri deposuna kopyalarken, bir klasöre birden çok dosya olarak yazmak (yalnızca klasör adını belirtin) komutu da verilir, bu durumda performans tek bir dosyaya yazmaktan daha iyidir.

Satırlara `rfcTableOptions`filtre uygulama için aşağıdaki ortak SAP sorgu işleçlerini kullanabilirsiniz:

| İşleç | Açıklama |
| :------- | :------- |
| `EQ` | Eşittir |
| `NE` | Eşit değildir |
| `LT` | Küçüktür |
| `LE` | Küçüktür veya eşittir |
| `GT` | Büyüktür |
| `GE` | Büyüktür veya eşittir |
| `LIKE` | Olduğu gibi`LIKE 'Emma%'` |

### <a name="example"></a>Örnek

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP table input dataset name>",
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
                "type": "SapTableSource",
                "partitionOption": "PartitionOnInt",
                "partitionSettings": {
                     "partitionColumnName": "<partition column name>",
                     "partitionUpperBound": "2000",
                     "partitionLowerBound": "1",
                     "maxPartitionsNumber": 500
                 }
            },
            "sink": {
                "type": "<sink type>"
            },
            "parallelCopies": 4
        }
    }
]
```

## <a name="data-type-mappings-for-an-sap-table"></a>SAP tablosu için veri türü eşlemeleri

Sap tablosundaki verileri kopyalarken, SAP tablo veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| SAP ABAP Tipi | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `C`(Dize) | `String` |
| `I`(Sonda) | `Int32` |
| `F`(Şamandıra) | `Double` |
| `D`(Tarih) | `String` |
| `T`(Zaman) | `String` |
| `P`(BCD Paketlenmiş, Para Birimi, Ondalık, Qty) | `Decimal` |
| `N`(Sayısal) | `String` |
| `X`(İkili ve Ham) | `String` |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
