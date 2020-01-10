---
title: Azure Data Factory kullanarak bir HTTP kaynağından veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliği kullanarak bir bulut veya şirket içi HTTP kaynağından desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
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
ms.openlocfilehash: d6305a6e68f435c009fdfdea371e88f4a73c3d92
ms.sourcegitcommit: 8b37091efe8c575467e56ece4d3f805ea2707a64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75830403"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Azure Data Factory kullanarak bir HTTP uç noktasından veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-http-connector.md)
> * [Geçerli sürüm](connector-http.md)

Bu makalede, bir HTTP uç noktasından veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makaleyi yapılar [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md), kopyalama etkinliği genel bir bakış sunar.

Bu HTTP Bağlayıcısı arasındaki fark, [rest Bağlayıcısı](connector-rest.md) ve [Web tablosu Bağlayıcısı](connector-web-table.md) :

- **Rest Bağlayıcısı** , verilerin yeniden oluşturulmuş API 'lerden kopyalanmasını özellikle destekler; 
- **Http Bağlayıcısı** , örneğin dosyayı indirmek için HERHANGI bir HTTP uç noktasından veri almak için geneldir. REST Bağlayıcısı kullanılabilir hale gelmeden önce, desteklenen ancak REST bağlayıcısıyla daha az işlevsel bir işlem olan API 'den veri kopyalamak için HTTP bağlayıcısını kullanabilirsiniz.
- **Web tablosu Bağlayıcısı** , tablo IÇERIĞINI bir HTML Web sayfasından ayıklar.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu HTTP Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Bir HTTP kaynağından, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği kaynak ve havuz olarak desteklediğini veri listesini depolar için bkz: [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).

Bu HTTP bağlayıcısını kullanarak şunları yapabilirsiniz:

- Http **Get** veya **Post** yöntemlerini kullanarak bir HTTP/S uç noktasından veri alma.
- Şu kimlik doğrulamalarından birini kullanarak verileri alın: **Anonymous**, **Basic**, **Digest**, **Windows**veya **ClientCertificate**.
- HTTP yanıtını olduğu gibi kopyalayın veya [Desteklenen dosya biçimlerini ve sıkıştırma codec bileşenlerini](supported-file-formats-and-compression-codecs.md)kullanarak ayrıştırın.

> [!TIP]
> HTTP bağlayıcısını Data Factory ' de yapılandırmadan önce veri alımı için bir HTTP isteğini test etmek için, üst bilgi ve gövde gereksinimlerine yönelik API belirtimi hakkında bilgi edinin. Doğrulamak için Postman veya bir Web tarayıcısı gibi araçları kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Kullanmaya Başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, HTTP bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

HTTP bağlantılı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | **Type** özelliği **httpServer**olarak ayarlanmalıdır. | Evet |
| url | Web sunucusunun temel URL 'SI. | Evet |
| enableServerCertificateValidation | Bir HTTP uç noktasına bağlanırken sunucu SSL sertifika doğrulamasının etkinleştirilip etkinleştirilmeyeceğini belirtin. HTTPS sunucunuz otomatik olarak imzalanan bir sertifika kullanıyorsa, bu özelliği **false**olarak ayarlayın. | Hayır<br /> (varsayılan değer **true**'dur) |
| authenticationType | Kimlik doğrulama türünü belirtir. İzin verilen değerler **anonim**, **temel**, **Özet**, **Windows**ve **ClientCertificate**'tir. <br><br> Bu kimlik doğrulama türleri için daha fazla özellik ve JSON örnekleri için bu tabloyu izleyen bölümlere bakın. | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

### <a name="using-basic-digest-or-windows-authentication"></a>Temel, Özet veya Windows kimlik doğrulamasını kullanma

**AuthenticationType** özelliğini **Basic**, **Digest**veya **Windows**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| userName adı | HTTP uç noktasına erişmek için kullanılacak Kullanıcı adı. | Evet |
| parola | Kullanıcının parolasını ( **userName** değeri). Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |

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

ClientCertificate kimlik doğrulamasını kullanmak için **AuthenticationType** özelliğini **ClientCertificate**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| embeddedCertData | Base64 kodlu sertifika verileri. | **Embeddedcertdata** veya **certparmak izi**belirtin. |
| certThumbprint | Kendi kendine barındırılan Integration Runtime makinenizin sertifika deposunda yüklü olan sertifikanın parmak izi. Yalnızca kendi kendine barındırılan Integration Runtime türü **Connectvia** özelliğinde belirtildiğinde geçerlidir. | **Embeddedcertdata** veya **certparmak izi**belirtin. |
| parola | Sertifikayla ilişkili parola. Bu alan olarak işaretlemek bir **SecureString** Data Factory'de güvenle depolamak için türü. Ayrıca [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Hayır |

Kimlik doğrulaması için **Certparmak izi** kullanırsanız ve sertifika yerel bilgisayarın Kişisel deposunda yüklüyse, şirket içinde barındırılan Integration Runtime okuma izinleri verin:

1. Microsoft Yönetim Konsolu 'nu (MMC) açın. **Yerel bilgisayarı**hedefleyen **Sertifikalar** ek bileşenini ekleyin.
2. **Sertifikalar** > **Kişisel**' i genişletin ve ardından **Sertifikalar**' ı seçin.
3. Kişisel mağazadan sertifikaya sağ tıklayın ve sonra **özel anahtarları yönet** > **Tüm görevler** ' i seçin.
3. **Güvenlik** sekmesinde, sertifikaya okuma erişimi Ile Integration Runtime ana bilgisayar hizmetinin (diahostservice) çalıştığı kullanıcı hesabını ekleyin.

**Örnek 1: Certparmak Izi kullanma**

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

**Örnek 2: embeddedCertData kullanma**

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarları altında HTTP için desteklenir:

| Özellik    | Açıklama                                                  | Gereklidir |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | Veri kümesindeki `location` seçeneğinin altında bulunan tür özelliği **Httpserverlocation**olarak ayarlanmalıdır. | Evet      |
| relativeUrl 'Si | Verileri içeren kaynağın göreli URL 'SI. HTTP Bağlayıcısı, verileri birleştirilmiş URL 'den kopyalar: `[URL specified in linked service]/[relative URL specified in dataset]`.   | Hayır       |

> [!NOTE]
> Desteklenen HTTP isteği yük boyutu 500 KB 'ın üzerinde. Web uç noktanıza geçirmek istediğiniz yük boyutu 500 KB 'tan büyükse, yükü daha küçük öbeklerdeki toplu işlemeyi düşünün.

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bu bölüm, HTTP kaynağının desteklediği özelliklerin bir listesini sağlar.

Bölümleri ve etkinlikleri tanımlamak için kullanılabilir olan özellikleri tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>Kaynak olarak HTTP

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama kaynağında `storeSettings` ayarları altında HTTP için desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | `storeSettings` altındaki tür özelliği **Httpreadsettings**olarak ayarlanmalıdır. | Evet      |
| requestMethod            | HTTP yöntemi. <br>İzin verilen değerler **Al** (varsayılan) ve **Post**. | Hayır       |
| Addtionalüstbilgileri         | Ek HTTP istek üstbilgileri.                             | Hayır       |
| Istek gövdesi              | HTTP isteğinin gövdesi.                               | Hayır       |
| httpRequestTimeout           | HTTP isteğinin yanıt almak için zaman aşımı ( **TimeSpan** değeri). Bu değer, yanıt verilerinin okunması için zaman aşımı değil, yanıt almaya yönelik zaman aşımı değeridir. Varsayılan değer **00:01:40**' dir. | Hayır       |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

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

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Geriye dönük uyumluluk için aşağıdaki modeller hala desteklenmektedir. Yukarıdaki bölümlerde bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Veri kümesinin **Type** özelliği **httpfile**olarak ayarlanmalıdır. | Evet |
| relativeUrl 'Si | Verileri içeren kaynağın göreli URL 'SI. Bu özellik belirtilmediğinde, yalnızca bağlı hizmet tanımında belirtilen URL kullanılır. | Hayır |
| requestMethod | HTTP yöntemi. İzin verilen değerler **Al** (varsayılan) ve **Post**. | Hayır |
| additionalHeaders | Ek HTTP istek üstbilgileri. | Hayır |
| Istek gövdesi | HTTP isteğinin gövdesi. | Hayır |
| biçim | HTTP uç noktasından verileri ayrıştırmadan olduğu gibi almak ve sonra verileri dosya tabanlı bir depoya kopyalamak istiyorsanız hem giriş hem de çıkış veri kümesi tanımlarının **Biçim** bölümünü atlayın.<br/><br/>Kopyalama sırasında HTTP yanıt içeriğini ayrıştırmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**ve **parquetformat**. **Biçim**altında, **Type** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [JSON biçimi](supported-file-formats-and-compression-codecs-legacy.md#json-format), [metin biçimi](supported-file-formats-and-compression-codecs-legacy.md#text-format), [avro Format](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parquet biçimi](supported-file-formats-and-compression-codecs-legacy.md#parquet-format). |Hayır |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/><br/>Desteklenen türler: **gzip**, **söndür**, **bzip2**ve **zipsöndür**.<br/>Desteklenen düzeyler: **en iyi** ve **en hızlı**. |Hayır |

> [!NOTE]
> Desteklenen HTTP isteği yük boyutu 500 KB 'ın üzerinde. Web uç noktanıza geçirmek istediğiniz yük boyutu 500 KB 'tan büyükse, yükü daha küçük öbeklerdeki toplu işlemeyi düşünün.

**Örnek 1: Get metodunu kullanma (varsayılan)**

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

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının **Type** özelliği **httpsource**olarak ayarlanmalıdır. | Evet |
| httpRequestTimeout | HTTP isteğinin yanıt almak için zaman aşımı ( **TimeSpan** değeri). Bu değer, yanıt verilerinin okunması için zaman aşımı değil, yanıt almaya yönelik zaman aşımı değeridir. Varsayılan değer **00:01:40**' dir.  | Hayır |

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

Kopyalama etkinliği kaynak olarak destekler ve şu havuzlar Azure Data Factory'de veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
