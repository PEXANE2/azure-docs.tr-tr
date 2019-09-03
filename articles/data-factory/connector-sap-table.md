---
title: Azure Data Factory kullanarak SAP tablosundan veri kopyalama | Microsoft Docs
description: Bir Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak SAP tablosundan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: jingwang
ms.openlocfilehash: 94bdeebcecdf924468f022ac95867bd82df5a793
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70231326"
---
# <a name="copy-data-from-an-sap-table-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SAP tablosundan veri kopyalama

Bu makalede, bir SAP tablosundan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Daha fazla bilgi için bkz. [kopyalama etkinliğine genel bakış](copy-activity-overview.md).

>[!TIP]
>ADF 'nin SAP veri tümleştirme senaryosunda genel desteğini öğrenmek için ayrıntılı giriş, comparme ve kılavuzla [Azure Data Factory Teknik İnceleme kullanarak SAP veri tümleştirme](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) konusuna bakın.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bir SAP tablosundan desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SAP tablo Bağlayıcısı şunları destekler:

- İçindeki bir SAP tablosundan veri kopyalama:

  - SAP ERP merkezi bileşeni (SAP ECC) sürüm 7,01 veya üzeri (2015 sonrasında yayınlanan son SAP desteği paket yığınında).
  - SAP Business Warehouse (SAP BW) sürüm 7,01 veya üzeri (2015 sonrasında yayınlanan son SAP desteği paket yığınında).
  - SAP S/4HANA.
  - SAP Business Suite sürüm 7,01 veya üzeri (2015 ' den sonra yayınlanan son SAP desteği paket yığınında) diğer ürünler.

- SAP saydam tablosundan, havuza alınmış bir tablodan, kümelenmiş bir tablodan ve bir görünümden veri kopyalama.
- SNC yapılandırıldıysa, temel kimlik doğrulaması veya güvenli ağ Iletişimleri (SNC) kullanarak verileri kopyalama.
- SAP uygulama sunucusuna veya SAP ileti sunucusuna bağlanma.

## <a name="prerequisites"></a>Önkoşullar

Bu SAP tablosu bağlayıcısını kullanmak için şunları yapmanız gerekir:

- Şirket içinde barındırılan bir tümleştirme çalışma zamanı ayarlayın (sürüm 3,17 veya üzeri). Daha fazla bilgi için, bkz. [Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma ve yapılandırma](create-self-hosted-integration-runtime.md).

- 64-bit SAP bağlayıcısını SAP web sitesinden [Microsoft .NET 3,0 için](https://support.sap.com/en/product/connectors/msnet.html) indirin ve şirket içinde barındırılan tümleştirme çalışma zamanı makinesine yükleyin. Yükleme sırasında, **Isteğe bağlı kurulum adımları** penceresindeki **GAC 'ye derlemeleri yükleme** seçeneğini seçtiğinizden emin olun.

  ![.NET için SAP bağlayıcısını yükler](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Data Factory SAP tablosu Bağlayıcısı 'nda kullanılmakta olan SAP kullanıcısının aşağıdaki izinlere sahip olması gerekir:

  - Uzak Işlev çağrısı (RFC) hedeflerini kullanma yetkilendirmesi.
  - S_SDSAUTH yetkilendirme nesnesinin yürütme etkinliğinin izinleri.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SAP tablo bağlayıcısına özgü Data Factory varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler SAP BW açık hub bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Özelliğin olarak`SapTable`ayarlanması gerekir. | Evet |
| `server` | SAP örneğinin bulunduğu sunucunun adı.<br/>SAP uygulama sunucusuna bağlanmak için kullanın. | Hayır |
| `systemNumber` | SAP sisteminin sistem numarası.<br/>SAP uygulama sunucusuna bağlanmak için kullanın.<br/>İzin verilen değer: Dize olarak temsil edilen iki basamaklı ondalık sayı. | Hayır |
| `messageServer` | SAP ileti sunucusunun ana bilgisayar adı.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `messageServerService` | İleti sunucusunun hizmet adı veya bağlantı noktası numarası.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `systemId` | Tablonun bulunduğu SAP sisteminin KIMLIĞI.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `logonGroup` | SAP sistemi için oturum açma grubu.<br/>Bir SAP ileti sunucusuna bağlanmak için kullanın. | Hayır |
| `clientId` | SAP sistemindeki istemcinin KIMLIĞI.<br/>İzin verilen değer: Dize olarak temsil edilen üç basamaklı ondalık sayı. | Evet |
| `language` | SAP sisteminin kullandığı dil.<br/>Varsayılan değer `EN`.| Hayır |
| `userName` | SAP sunucusuna erişimi olan kullanıcının adı. | Evet |
| `password` | Kullanıcının parolası. Bu alanı, `SecureString` Data Factory güvenli bir şekilde depolamak için yazın veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)için işaretleyin. | Evet |
| `sncMode` | Tablonun bulunduğu SAP sunucusuna erişmek için SNC etkinleştirme göstergesi.<br/>SAP sunucusuna bağlanmak için SNC kullanmak istiyorsanız kullanın.<br/>İzin verilen değerler `0` (kapalı, varsayılan) veya `1` (açık). | Hayır |
| `sncMyName` | Tablonun bulunduğu SAP sunucusuna erişmek için başlatıcının SNC adı.<br/>Açık olduğunda `sncMode` geçerlidir. | Hayır |
| `sncPartnerName` | Tablonun bulunduğu SAP sunucusuna erişmek için iletişim ortağının SNC adı.<br/>Açık olduğunda `sncMode` geçerlidir. | Hayır |
| `sncLibraryPath` | Tablonun bulunduğu SAP sunucusuna erişmek için dış güvenlik ürününün kitaplığı.<br/>Açık olduğunda `sncMode` geçerlidir. | Hayır |
| `sncQop` | Uygulanacak SNC koruma düzeyi kalitesi.<br/>Açık olduğunda `sncMode` geçerlidir. <br/>İzin verilen değerler `1` (kimlik doğrulaması) `2` , (bütünlük) `3` , (Gizlilik) `8` , (varsayılan) `9` , (en fazla). | Hayır |
| `connectVia` | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullardan](#prerequisites)daha önce belirtildiği gibi, şirket içinde barındırılan bir tümleştirme çalışma zamanı gereklidir. |Evet |

**Örnek 1: SAP uygulama sunucusuna bağlanma**

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

### <a name="example-2-connect-to-an-sap-message-server"></a>Örnek 2: SAP ileti sunucusuna bağlan

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

### <a name="example-3-connect-by-using-snc"></a>Örnek 3: SNC kullanarak bağlanma

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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md). Aşağıdaki bölüm, SAP tablo veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve SAP BW açık hub bağlantılı hizmetine veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| `type` | `type` Özelliğin olarak`SapTableResource`ayarlanması gerekir. | Evet |
| `tableName` | Verilerin kopyalanacağı SAP tablosunun adı. | Evet |

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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). Aşağıdaki bölüm, SAP tablo kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-table-as-source"></a>Kaynak olarak SAP tablosu

Bir SAP tablosundan veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik                         | Açıklama                                                  | Gerekli |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| `type`                             | `type` Özelliğin olarak`SapTableSource`ayarlanması gerekir.         | Evet      |
| `rowCount`                         | Alınacak satır sayısı.                              | Hayır       |
| `rfcTableFields`                   | SAP tablosundan kopyalanacak alanlar (sütunlar). Örneğin: `column0, column1`. | Hayır       |
| `rfcTableOptions`                  | SAP tablosundaki satırları filtrelemeye yönelik seçenekler. Örneğin: `COLUMN0 EQ 'SOMEVALUE'`. Bu makalenin ilerleyen kısımlarında Ayrıca bkz. SAP Query operator tablosu. | Hayır       |
| `customRfcReadTableFunctionModule` | Bir SAP tablosundan veri okumak için kullanılabilen özel bir RFC işlev modülü.<br>Verilerin SAP sisteminizden nasıl alındığını tanımlamak ve Data Factory ' a geri dönmek için özel bir RFC işlevi modülü kullanabilirsiniz. Özel işlev modülünün, Data Factory tarafından kullanılan varsayılan arabirim olan öğesine `/SAPDS/RFC_READ_TABLE2`benzer bir arabirimi (içeri aktarma, dışarı aktarma, tablolar) uygulanmış olması gerekir. | Hayır       |
| `partitionOption`                  | Bir SAP tablosundan okunacak bölüm mekanizması. Desteklenen seçenekler şunlardır: <ul><li>`None`</li><li>`PartitionOnInt`(gibi, `0000012345`sol tarafta sıfır dolgusu olan normal tamsayı veya tamsayı değerleri)</li><li>`PartitionOnCalendarYear`("YYYY" biçiminde 4 basamak)</li><li>`PartitionOnCalendarMonth`("YYYYMM" biçiminde 6 basamak)</li><li>`PartitionOnCalendarDate`("YYYYMMDD" biçiminde 8 basamak)</li></ul> | Hayır       |
| `partitionColumnName`              | Verileri bölümlemek için kullanılan sütunun adı.                | Hayır       |
| `partitionUpperBound`              | Bölümlendirmeye devam etmek için, içinde `partitionColumnName` belirtilen sütunun en büyük değeri. | Hayır       |
| `partitionLowerBound`              | Bölümlendirmeye devam etmek için, içinde `partitionColumnName` belirtilen sütunun en küçük değeri. | Hayır       |
| `maxPartitionsNumber`              | Verilerin bölüneceği en fazla bölüm sayısı.     | Hayır       |

>[!TIP]
>SAP tablonuzda birkaç milyar satır gibi büyük miktarda veri varsa, verileri daha küçük bölümlere bölmek için ve `partitionOption` `partitionSetting` kullanın. Bu durumda, veriler bölüm başına okunurdur ve her bir veri bölümü, tek bir RFC çağrısıyla SAP sunucusundan alınır.<br/>
<br/>
>`partitionLowerBound``maxPartitionsNumber` `partitionUpperBound` Örnek olarak, her bölümdeki satır sayısı şu formül ile hesaplanır: (ve arasında kalan toplam satır)/. `partitionOnInt` `partitionOption`<br/>
<br/>
>Kopyayı hızlandırmak için veri bölümlerini paralel olarak yüklemek için, paralel derece kopyalama etkinliğindeki [`parallelCopies`](copy-activity-performance.md#parallel-copy) ayarla denetlenir. Örneğin, dört olarak ayarlarsanız `parallelCopies` , Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır ve her sorgu, SAP tabloınızdan verilerin bir kısmını alır. Özelliğin değerinin birden çok `maxPartitionsNumber` değerini yapmanızı kesinlikle öneririz. `parallelCopies` Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adını belirt) olarak yazmak da daha da iyidir. Bu durumda, performans tek bir dosyaya yazılmasından daha iyidir.

İçinde `rfcTableOptions`, satırları filtrelemek için aşağıdaki genel SAP sorgu işleçlerini kullanabilirsiniz:

| Operator | Açıklama |
| :------- | :------- |
| `EQ` | Eşittir |
| `NE` | Eşit değildir |
| `LT` | Küçüktür |
| `LE` | Küçüktür veya eşittir |
| `GT` | Büyüktür |
| `GE` | Büyüktür veya eşittir |
| `LIKE` | İtibariyle`LIKE 'Emma%'` |

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

Bir SAP tablosundan veri kopyalarken aşağıdaki eşlemeler, SAP tablosu veri türlerinden Azure Data Factory geçici veri türlerine kadar kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| SAP ABAP türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| `C`Dizisinde | `String` |
| `I`Gir | `Int32` |
| `F`Float | `Double` |
| `D`Güncel | `String` |
| `T`Işınızda | `String` |
| `P`(BCD paketlenmiş, para birimi, ondalık, miktar) | `Decimal` |
| `N`Rakamlardan | `String` |
| `X`(İkili ve ham) | `String` |

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
