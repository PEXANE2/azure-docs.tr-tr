---
title: Azure Data Factory kullanarak Xero 'tan veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak Xero 'tan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: jingwang
ms.openlocfilehash: 8a704c3891c687edbb7c5aac206f4b6c7766fa8c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81409999"
---
# <a name="copy-data-from-xero-using-azure-data-factory"></a>Azure Data Factory kullanarak Xero 'tan veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Xero ' den veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Xero Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Xero 'ten desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Xero Bağlayıcısı şunları destekler:

- Xero [özel uygulama](https://developer.xero.com/documentation/getting-started/api-application-types) ancak ortak uygulama değil.
- Tüm Xero tabloları (API uç noktaları), "raporlar" hariç. 

Azure Data Factory, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde Xero bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler Xero bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği: **Xero** olarak ayarlanmalıdır | Yes |
| konak | Xero sunucusunun uç noktası (`api.xero.com`).  | Yes |
| consumerKey | Xero uygulamasıyla ilişkili tüketici anahtarı. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |
| privateKey | Xero özel uygulamanız için oluşturulan. ped dosyasındaki özel anahtar, bkz. [ortak/özel anahtar çifti oluşturma](https://developer.xero.com/documentation/api-guides/create-publicprivate-key). Using `openssl genrsa -out privatekey.pem 512` **numbits of 512 ile PrivateKey. pek oluşturma** 1024 desteklenmiyor. . Ped dosyasındaki, Unix satır sonları (\n) dahil tüm metni ekleyin, aşağıdaki örneğe bakın.<br/><br/>Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| Usehostdoğrulaması | TLS üzerinden bağlanırken sunucunun ana bilgisayar adıyla eşleşmesi için Sunucu sertifikasında ana bilgisayar adının gerekli olup olmadığını belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| Usepeerdoğrulaması | TLS üzerinden bağlanılırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true şeklindedir.  | Hayır |

**Örneğinde**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "host" : "api.xero.com",
            "consumerKey": {
                 "type": "SecureString",
                 "value": "<consumerKey>"
            },
            "privateKey": {
                 "type": "SecureString",
                 "value": "<privateKey>"
            }
        }
    }
}
```

**Örnek özel anahtar değeri:**

. Ped dosyasındaki, Unix satır sonları (\n) dahil tüm metni ekleyin.

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Xero veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Xero öğesinden veri kopyalamak için, veri kümesinin Type özelliğini **XeroObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **XeroObject** olarak ayarlanmalıdır | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

**Örneğinde**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm Xero kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="xero-as-source"></a>Kaynak olarak Xero

Xero adresinden veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **XeroSource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **XeroSource** olarak ayarlanmalıdır | Yes |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM Contacts"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

**Örneğinde**

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
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
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Xero sorgusunu belirtirken aşağıdakileri aklınızda edin:

- Karmaşık öğeler içeren tablolar birden çok tabloya bölünecektir. Örneğin, banka işlemleri "LineItems" karmaşık veri yapısına sahiptir, bu nedenle banka işleminin verileri tabloyla `Bank_Transaction` eşlenir ve `Bank_Transaction_Line_Items`bunları birbirine bağlamak için yabancı anahtar `Bank_Transaction_ID` olarak.

- Xero verileri iki şema aracılığıyla kullanılabilir: `Minimal` (varsayılan) ve. `Complete` Tüm şema, istenen sorguyu yapmadan önce ek veriler (ör. ID sütunu) gerektiren önkoşul çağrı tabloları içerir.

Aşağıdaki tablolar, en küçük ve tamamen şema ile aynı bilgilere sahiptir. API çağrılarının sayısını azaltmak için en az şemayı kullanın (varsayılan).

- Bank_Transactions
- Contact_Groups 
- Kişiler 
- Contacts_Sales_Tracking_Categories 
- Contacts_Phones 
- Contacts_Addresses 
- Contacts_Purchases_Tracking_Categories 
- Credit_Notes 
- Credit_Notes_Allocations 
- Expense_Claims 
- Expense_Claim_Validation_Errors
- Faturalar 
- Invoices_Credit_Notes
- Peşinatlar Invoices_ 
- Invoices_Overpayments 
- Manual_Journals 
- Fazla ödemeler 
- Overpayments_Allocations 
- Peşinatları 
- Prepayments_Allocations 
- Lü 
- Receipt_Validation_Errors 
- Tracking_Categories

Aşağıdaki tablolar yalnızca tüm şemayla sorgulanabilir:

- Tamam. Bank_Transaction_Line_Items 
- Tamam. Bank_Transaction_Line_Item_Tracking 
- Tamam. Contact_Group_Contacts 
- Contacts_Contact_. kişileri doldurun 
- Tamam. Credit_Note_Line_Items 
- Tamam. Credit_Notes_Line_Items_Tracking 
- Tam. Expense_Claim_ ödemeleri 
- Tamam. Expense_Claim_Receipts 
- Tamam. Invoice_Line_Items 
- Tamam. Invoices_Line_Items_Tracking
- Tamam. Manual_Journal_Lines 
- Tamam. Manual_Journal_Line_Tracking 
- Tamam. Overpayment_Line_Items 
- Tamam. Overpayment_Line_Items_Tracking 
- Tamam. Prepayment_Line_Items 
- Tamam. Prepayment_Line_Item_Tracking 
- Tamam. Receipt_Line_Items 
- Tamam. Receipt_Line_Item_Tracking 
- Tamam. Tracking_Category_Options

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
