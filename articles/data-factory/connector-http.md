---
title: Azure Veri Fabrikası'nı kullanarak bir HTTP kaynağından veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık kaynakta kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına bulutveya şirket içi HTTP kaynağından verileri nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: jingwang
ms.openlocfilehash: 4a05d955be88f68b3c0db1f4a29b3f6e1155aa0d
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992190"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri bir HTTP bitiş noktasından kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-http-connector.md)
> * [Geçerli sürüm](connector-http.md)

Bu makalede, http bitiş noktasından verileri kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

Bu HTTP bağlayıcısı, [REST bağlayıcısı](connector-rest.md) ve [Web tablosu bağlayıcısı](connector-web-table.md) arasındaki fark şunlardır:

- **REST bağlayıcısı** özellikle RESTful API'lerden veri kopyalamayı destekler; 
- **HTTP bağlayıcısı,** herhangi bir HTTP bitiş noktasından (örneğin dosyayı indirmek için) veri almak için geneldir. REST bağlayıcısı kullanıma sunulmadan önce, REST konektörüyle karşılaştırıldığında desteklenen ancak daha az işlevsel olan RESTful API'den verileri kopyalamak için HTTP bağlayıcısını kullanabilirsiniz.
- **Web tablosu bağlayıcısı** tablo içeriğini bir HTML web sayfasından ayıklar.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu HTTP bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri bir HTTP kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Bu HTTP konektörünü şu şekilde kullanabilirsiniz:

- HTTP **GET** veya **POST** yöntemlerini kullanarak bir HTTP/S bitiş noktasından veri alın.
- Aşağıdaki kimlik doğrulamalarından birini kullanarak verileri alın: **Anonim**, **Temel**, **Özet,** **Windows**veya **ClientCertificate**.
- [DESTEKLENEN dosya biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)kullanarak HTTP yanıtını olduğu gibi kopyalayın veya ayrıştırın.

> [!TIP]
> Veri Fabrikası'ndaki HTTP bağlayıcısını yapılandırmadan önce veri alma için http isteğini sınamak için üstbilgi ve gövde gereksinimleri için API belirtimi hakkında bilgi edinin. Doğrulamak için Postacı veya web tarayıcısı gibi araçları kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, HTTP bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler HTTP bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **HttpServer**olarak ayarlanmalıdır. | Evet |
| url | Web sunucusunun temel URL'si. | Evet |
| enableServerCertificateValidation | Bir HTTP bitiş noktasına bağlandığınızda sunucu TLS/SSL sertifika doğrulamasını etkinleştirip etkinleştirmeyeceğiniz belirtin. HTTPS sunucunuz kendi imzalı bir sertifika kullanıyorsa, bu özelliği **yanlış**olarak ayarlayın. | Hayır<br /> (varsayılan **değer doğrudur)** |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler **Anonim,** **Temel**, **Özet,** **Windows**ve **ClientCertificate'tır.** <br><br> Daha fazla özellik için bu tabloyu izleyen bölümlere ve bu kimlik doğrulama türleri için JSON örneklerine bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamayı kullanma

Kimlik **doğrulamaÖzelliğini** **Temel**, **Özet**veya **Windows**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| userName | HTTP bitiş noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının şifresi **(kullanıcı Adı** değeri). Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** türü olarak işaretleyin. [Azure Key Vault'ta depolanan bir gizli ye de başvuruda](store-credentials-in-key-vault.md)bulunabilirsiniz. | Evet |

**Örnek**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
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

### <a name="using-clientcertificate-authentication"></a>ClientCertificate kimlik doğrulamasını kullanma

ClientCertificate kimlik doğrulamasını kullanmak için **kimlik doğrulamaTürü** özelliğini **ClientCertificate**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| embeddedCertData | Base64 kodlanmış sertifika verileri. | **EmbeddedCertData** veya **certThumbprint'i belirtin.** |
| certThumbprint | Kendi barındırılan Tümleştirme Runtime makinenizin sertifika deposuna yüklenen sertifikanın parmak izi. Yalnızca **connectVia** özelliğinde kendi kendine barındırılan Tümleştirme Çalışma Süresi türü belirtildiğinde geçerlidir. | **EmbeddedCertData** veya **certThumbprint'i belirtin.** |
| password | Sertifikayla ilişkili parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** türü olarak işaretleyin. [Azure Key Vault'ta depolanan bir gizli ye de başvuruda](store-credentials-in-key-vault.md)bulunabilirsiniz. | Hayır |

Kimlik doğrulaması için **sertifikakullanırsanız** ve sertifika yerel bilgisayarın kişisel deposuna yüklenirse, kendi barındırılan Tümleştirme Çalışma Süresi'ne okuma izinleri verir:

1. Microsoft Yönetim Konsolu'nu (MMC) açın. **Yerel Bilgisayarı**hedefleyen **Sertifikaları** ekleme.
2. **Kişisel Sertifikaları** > **Personal**Genişletin ve ardından **Sertifikaları**seçin.
3. Kişisel mağazadan sertifikayı sağ tıklatın ve ardından **Tüm Görevler** > **Özel Tuşları Yönet'i**seçin.
3. **Güvenlik** sekmesinde, sertifikaya okuma erişimi olan, Tümleştirme Runtime Host Hizmetinin (DIAHostService) altında çalışan kullanıcı hesabını ekleyin.

**Örnek 1: CertThumbprint kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: EmbeddedCertData'yı kullanma**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler biçim tabanlı veri `location` kümesindeki ayarlar altında HTTP için desteklenir:

| Özellik    | Açıklama                                                  | Gerekli |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | Veri kümesinin `location` altındaki tür özelliği **HttpServerLocation**olarak ayarlanmalıdır. | Evet      |
| Relativeurl | Verileri içeren kaynağa göreli bir URL. HTTP bağlayıcısı verileri birleştirilmiş URL'den kopyalar: `[URL specified in linked service][relative URL specified in dataset]`.   | Hayır       |

> [!NOTE]
> Desteklenen HTTP istek yük boyutu yaklaşık 500 KB'dir. Web bitiş noktanıza geçirmek istediğiniz taşıma yükü boyutu 500 KB'den büyükse, taşıma yükünü daha küçük parçalar halinde toplu olarak kullanmayı düşünün.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HttpServerLocation",
                "relativeUrl": "<relative url>"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, HTTP kaynağının desteklediği özelliklerin bir listesi sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="http-as-source"></a>KAYNAK OLARAK HTTP

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler http için biçim `storeSettings` tabanlı kopyalama kaynağındaki ayarlar altında desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki tür `storeSettings` özelliği **HttpReadSettings**olarak ayarlanmalıdır. | Evet      |
| requestMethod            | HTTP yöntemi. <br>İzin verilen değerler **Al** (varsayılan) ve **Post'dur.** | Hayır       |
| addtionalHeaders         | Ek HTTP istek üstbilgi.                             | Hayır       |
| requestBody              | HTTP isteği için gövde.                               | Hayır       |
| httpRequestTimeout           | Yanıt almak için HTTP isteğiiçin zaman aralığı **(TimeSpan** değeri). Bu değer, yanıt verilerini okumak için zaman arası değil, yanıt almak için zaman aramıdır. Varsayılan değer **00:01:40'dır.** | Hayır       |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HttpReadSettings",
                    "requestMethod": "Post",
                    "additionalHeaders": "<header key: header value>\n<header key: header value>\n",
                    "requestBody": "<body for POST HTTP request>"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Aşağıdaki modeller hala geriye dönük uyumluluk için olduğu gibi desteklenir. İleriye dönük yukarıdaki bölümlerde belirtilen yeni modeli kullanmanız önerilir ve ADF yazarlı UI yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski dataset modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **HttpFile**olarak ayarlanmalıdır. | Evet |
| Relativeurl | Verileri içeren kaynağa göreli bir URL. Bu özellik belirtilmediği zaman, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **Al** (varsayılan) ve **Post'dur.** | Hayır |
| ek Headers | Ek HTTP istek üstbilgi. | Hayır |
| requestBody | HTTP isteği için gövde. | Hayır |
| biçim | Verileri ayrıştırmadan OLDUĞU gİbİ HTTP uç noktasından almak ve ardından verileri dosya tabanlı bir depoya kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında **biçim** bölümünü atlayın.<br/><br/>Kopyalama sırasında HTTP yanıt içeriğini ayrışdırmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. **Biçim**altında, **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [JSON formatı,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Metin biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [Avro formatı,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork formatı](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke formatına](supported-file-formats-and-compression-codecs-legacy.md#parquet-format)bakın. |Hayır |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/><br/>Desteklenen türleri: **GZip**, **Deflate**, **BZip2**, ve **ZipDeflate**.<br/>Desteklenen seviyeler: **Optimal** ve **En Hızlı**. |Hayır |

> [!NOTE]
> Desteklenen HTTP istek yük boyutu yaklaşık 500 KB'dir. Web bitiş noktanıza geçirmek istediğiniz taşıma yükü boyutu 500 KB'den büyükse, taşıma yükünü daha küçük parçalar halinde toplu olarak kullanmayı düşünün.

**Örnek 1: Get yöntemini kullanma (varsayılan)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Örnek 2: Post yöntemini kullanma**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **türü** özelliği **HttpSource**olarak ayarlanmalıdır. | Evet |
| httpRequestTimeout | Yanıt almak için HTTP isteğiiçin zaman aralığı **(TimeSpan** değeri). Bu değer, yanıt verilerini okumak için zaman arası değil, yanıt almak için zaman aramıdır. Varsayılan değer **00:01:40'dır.**  | Hayır |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)
