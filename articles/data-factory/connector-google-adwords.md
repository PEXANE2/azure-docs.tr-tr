---
title: Azure Data Factory kullanarak Google AdWords 'tan veri kopyalama | Microsoft Docs
description: Azure Data Factory işlem hattındaki kopyalama etkinliğini kullanarak Google AdWords 'tan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: jingwang
ms.openlocfilehash: 65bf1fa37ecb8d9e862b0e5042bed29470d750e1
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935629"
---
# <a name="copy-data-from-google-adwords-using-azure-data-factory"></a>Azure Data Factory kullanarak Google AdWords 'tan veri kopyalama

Bu makalede, Google AdWords 'tan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Google AdWords Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)


Google AdWords 'tan desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Data Factory, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Google AdWords bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Google AdWords bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **GoogleAdWords** olarak ayarlanmalıdır | Yes |
| Clientcustomerıd | Rapor verilerini getirmek istediğiniz AdWords hesabının Istemci müşteri KIMLIĞI.  | Yes |
| developerToken | AdWords API 'sine erişim vermek için kullandığınız yönetici hesabıyla ilişkili geliştirici belirteci.  Bu alanı, ADF 'de güvenli bir şekilde depolamak veya Azure Key Vault parolayı depolamak için bir SecureString olarak işaretlemeyi seçebilir ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekmeye izin verebilir. [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Yes |
| authenticationType | Kimlik doğrulaması için kullanılan OAuth 2,0 kimlik doğrulama mekanizması. ServiceAuthentication yalnızca şirket içinde barındırılan IR üzerinde kullanılabilir. <br/>İzin verilen değerler: **Serviceauthentication**, **userauthentication** | Yes |
| refreshToken | Kullanıcı kimlik doğrulaması için AdWords erişimini yetkilendirmek üzere Google 'dan edinilen yenileme belirteci. Bu alanı, ADF 'de güvenli bir şekilde depolamak veya Azure Key Vault parolayı depolamak için bir SecureString olarak işaretlemeyi seçebilir ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekmeye izin verebilir. [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| ClientID | Yenileme belirtecini almak için kullanılan Google uygulamasının istemci KIMLIĞI. Bu alanı, ADF 'de güvenli bir şekilde depolamak veya Azure Key Vault parolayı depolamak için bir SecureString olarak işaretlemeyi seçebilir ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekmeye izin verebilir. [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| clientSecret | Yenileme belirtecini almak için kullanılan Google uygulamasının istemci gizli anahtarı. Bu alanı, ADF 'de güvenli bir şekilde depolamak veya Azure Key Vault parolayı depolamak için bir SecureString olarak işaretlemeyi seçebilir ve veri kopyalama işlemini gerçekleştirirken ADF kopyalama etkinliği çekmeye izin verebilir. [Key Vault mağaza kimlik bilgilerinden](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Hayır |
| e-posta | ServiceAuthentication için kullanılan ve yalnızca şirket içinde barındırılan IR üzerinde kullanılabilen hizmet hesabı e-posta KIMLIĞI.  | Hayır |
| Keyfilepath null | Hizmet hesabı e-posta adresinin kimliğini doğrulamak için kullanılan ve yalnızca şirket içinde barındırılan IR üzerinde kullanılabilen. p12 anahtar dosyasının tam yolu.  | Hayır |
| trustedCertPath | SSL üzerinden bağlanılırken sunucuyu doğrulamak için güvenilir CA sertifikaları içeren. ped dosyasının tam yolu. Bu özellik yalnızca, şirket içinde barındırılan IR üzerinde SSL kullanılırken ayarlanabilir. Varsayılan değer, IR ile birlikte yüklenen CAcert. Pez dosyasıdır.  | Hayır |
| useSystemTrustStore | Sistem güven deposundan veya belirtilen ped dosyasından bir CA sertifikası kullanılıp kullanılmayacağını belirtir. Varsayılan değer false 'dur.  | Hayır |

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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Google AdWords veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Google AdWords 'tan veri kopyalamak için veri kümesinin Type özelliğini **GoogleAdWordsObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **GoogleAdWordsObject** olarak ayarlanmalıdır | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Google AdWords kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="google-adwords-as-source"></a>Kaynak olarak Google AdWords

Google AdWords 'tan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **GoogleAdWordsSource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **GoogleAdWordsSource** olarak ayarlanmalıdır | Yes |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

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

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
