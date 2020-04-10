---
title: Azure Veri Fabrikası'nı kullanarak Google BigQuery'deki verileri kopyalama
description: Bir veri fabrikası ardışık alanında bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına Google BigQuery'deki verileri nasıl kopyalaylaydestekleyeceğini öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/04/2019
ms.openlocfilehash: 9fe6b494398337dd49bd8f0fe53b24666412a1b0
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991595"
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Google BigQuery'deki verileri kopyalama

Bu makalede, Google BigQuery'deki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı özetlanmaktadır. Kopyalama [etkinliğine](copy-activity-overview.md) genel bir genel bakış sunan Kopyalama Etkinliği genel bakış makalesine dayanıyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Google BigQuery bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri Google BigQuery'den desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar. Bu nedenle, bu bağlayıcıyı kullanmak için bir sürücüyü el ile yüklemeniz gerekmez.

>[!NOTE]
>Bu Google BigQuery bağlayıcısı BigQuery API'lerinin üzerine inşa edilmiştir. BigQuery'nin gelen isteklerin maksimum oranını sınırladığını ve proje başına uygun kotaları uyguladığını unutmayın, [Kotalar & Limitler - API isteklerine](https://cloud.google.com/bigquery/quotas#api_requests)bakın. Hesaba çok fazla eşzamanlı istek tetiklemediğinden emin olun.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Google BigQuery bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Google BigQuery bağlantılı hizmet için desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **GoogleBigQuery**olarak ayarlanmalıdır. | Evet |
| proje | Karşı sorgulanacak varsayılan BigQuery projesinin proje kimliği.  | Evet |
| ekProjeler | Erişmek için kamu BigQuery projelerinin proje ids'lerinin virgülle ayrılmış bir listesi.  | Hayır |
| İstekGoogleDriveScope | Google Drive'a erişim isteyip istemeyeceğiniz. Google Drive erişimine izin vermek, BigQuery verilerini Google Drive'daki verilerle birleştiren federe tablolar için destek sağlar. Varsayılan değer **false** şeklindedir.  | Hayır |
| authenticationType | Kimlik doğrulama için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca Kendi barındırılan Tümleştirme Çalışma Süresi'nde kullanılabilir. <br/>İzin verilen değerler **UserAuthentication** ve **ServiceAuthentication'dır.** Bu tablonun altındaki bölümlere bakın, sırasıyla bu kimlik doğrulama türleri için daha fazla özellik ve JSON örnekleri. | Evet |

### <a name="using-user-authentication"></a>Kullanıcı kimlik doğrulamasını kullanma

"AuthenticationType" özelliğini **UserAuthentication**olarak ayarlayın ve önceki bölümde açıklanan genel özelliklerle birlikte aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| clientId | Yenileme belirteci oluşturmak için kullanılan uygulamanın kimliği. | Hayır |
| clientSecret | Yenileme belirteci oluşturmak için kullanılan uygulamanın sırrı. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| yenilemeToken | Google'dan elde edilen yenileme belirteci BigQuery'ye erişim yetkisi vermek için kullanılır. [OAuth 2.0 erişim jetonları](https://developers.google.com/identity/protocols/OAuth2WebServer#obtainingaccesstokens) ve bu topluluk [blog](https://jpd.ms/getting-your-bigquery-refresh-token-for-azure-datafactory-f884ff815a59)edinme nasıl bir tane öğrenin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |

**Örnek:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "clientId": "<id of the application used to generate the refresh token>",
            "clientSecret": {
                "type": "SecureString",
                "value":"<secret of the application used to generate the refresh token>"
            },
            "refreshToken": {
                "type": "SecureString",
                "value": "<refresh token>"
            }
        }
    }
}
```

### <a name="using-service-authentication"></a>Hizmet kimlik doğrulamasını kullanma

"AuthenticationType" özelliğini **ServiceAuthentication**olarak ayarlayın ve önceki bölümde açıklanan genel özelliklerle birlikte aşağıdaki özellikleri belirtin. Bu kimlik doğrulama türü yalnızca Kendi barındırılan Tümleştirme Çalışma Süresi'nde kullanılabilir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| e-posta | ServiceAuthentication için kullanılan hizmet hesabı e-posta kimliği. Yalnızca Kendi kendine barındırılan Tümleştirme Çalışma Süresi'nde kullanılabilir.  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresinin kimliğini doğrulamak için kullanılan .p12 anahtar dosyasına tam yol. | Hayır |
| güvenilirCertPath | TLS üzerinden bağlandığınızda sunucuyu doğrulamak için kullanılan güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca KENDI barındırılan Tümleştirme Çalışma Süresi'nde TLS kullandığınızda ayarlanabilir. Varsayılan değer, tümleştirme çalışma süresiyle yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirtilen bir .pem dosyasından CA sertifikası kullanıp kullanılmayacağını belirtir. Varsayılan değer **false** şeklindedir.  | Hayır |

**Örnek:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project id>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "ServiceAuthentication",
            "email": "<email>",
            "keyFilePath": "<.p12 key path on the IR machine>"
        },
        "connectVia": {
            "referenceName": "<name of Self-hosted Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Google BigQuery veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Google BigQuery'den veri kopyalamak için, veri kümesinin tür özelliğini **GoogleBigQueryObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **GoogleBigQueryObject** | Evet |
| Dataset | Google BigQuery veri kümesinin adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tablo | Masanın adı. |Hayır (etkinlik kaynağında "sorgu" belirtilirse)  |
| tableName | Masanın adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `dataset` `table`için, kullanım ve . | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Google BigQuery kaynak türü tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="googlebigquerysource-as-a-source-type"></a>Kaynak türü olarak GoogleBigQuerySource

Google BigQuery'den veri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **GoogleBigQuerySource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği **GoogleBigQuerySource**olarak ayarlanmalıdır. | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. `"SELECT * FROM MyTable"` bunun bir örneğidir. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
                "query": "SELECT * FROM MyTable"
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

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
