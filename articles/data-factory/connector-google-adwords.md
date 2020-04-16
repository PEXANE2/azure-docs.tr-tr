---
title: Google AdWords'ten veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına Google AdWords'teki verileri nasıl kopyalaylayamamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/25/2019
ms.openlocfilehash: 79e277cb3d512ee17da866a61e9d6d66a50cd902
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415015"
---
# <a name="copy-data-from-google-adwords-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Google AdWords'ten veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]


Bu makalede, Google AdWords'teki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı özetlanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Google AdWords bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)


Google AdWords'ten gelen verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Google AdWords bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Google AdWords bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **GoogleAdWords** | Evet |
| müşteriMüşteri Kimliği | Rapor verilerini almak istediğiniz AdWords hesabının Müşteri müşteri kimliği.  | Evet |
| geliştiriciToken | AdWords API'sine erişim izni vermek için kullandığınız yönetici hesabıyla ilişkili geliştirici belirteci.  Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Evet |
| authenticationType | Kimlik doğrulama için kullanılan OAuth 2.0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca kendi barındırılan IR'de kullanılabilir. <br/>İzin verilen değerler şunlardır: **ServiceAuthentication**, **UserAuthentication** | Evet |
| yenilemeToken | Kullanıcı Kimliği Için AdWords'e erişim yetkisi vermek için Google'dan alınan yenileme belirteci. Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| clientId | Yenileme belirteci elde etmek için kullanılan Google uygulamasının istemci kimliği. Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| clientSecret | Yenileme belirteci elde etmek için kullanılan google uygulamasının istemci sırrı. Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| e-posta | ServiceAuthentication için kullanılan ve yalnızca kendi barındırılan IR'de kullanılabilen servis hesabı e-posta kimliği.  | Hayır |
| keyFilePath | Hizmet hesabı e-posta adresini doğrulamak için kullanılan ve yalnızca kendi barındırılan IR'de kullanılabilen .p12 anahtar dosyasına giden tam yol.  | Hayır |
| güvenilirCertPath | TLS üzerinden bağlanırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren .pem dosyasının tam yolu. Bu özellik yalnızca kendi barındırılan IR'de TLS kullanırken ayarlanabilir. Varsayılan değer, IR ile yüklü cacerts.pem dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirli bir PEM dosyasından CA sertifikası kullanıp kullanılmayacağını belirtir. Varsayılan değer false'tur.  | Hayır |

**Örnek:**

```json
{
    "name": "GoogleAdWordsLinkedService",
    "properties": {
        "type": "GoogleAdWords",
        "typeProperties": {
            "clientCustomerID" : "<clientCustomerID>",
            "developerToken": {
                "type": "SecureString",
                "value": "<developerToken>"
            },
            "authenticationType" : "ServiceAuthentication",
            "refreshToken": {
                "type": "SecureString",
                "value": "<refreshToken>"
            },
            "clientId": {
                "type": "SecureString",
                "value": "<clientId>"
            },
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            },
            "email" : "<email>",
            "keyFilePath" : "<keyFilePath>",
            "trustedCertPath" : "<trustedCertPath>",
            "useSystemTrustStore" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Google AdWords veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Google AdWords'teki verileri kopyalamak için, veri kümesinin tür özelliğini **GoogleAdWordsObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **GoogleAdWordsObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "GoogleAdWordsDataset",
    "properties": {
        "type": "GoogleAdWordsObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<GoogleAdWords linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Google AdWords kaynağı tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="google-adwords-as-source"></a>Kaynak olarak Google AdWords

Google AdWords'teki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **GoogleAdWordsSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği şu şekilde ayarlanmalıdır: **GoogleAdWordsSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromGoogleAdWords",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleAdWords input dataset name>",
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
                "type": "GoogleAdWordsSource",
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
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
