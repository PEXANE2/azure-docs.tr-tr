---
title: Azure Veri Fabrikası'nı kullanarak Xero'dan veri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak Xero'daki verileri desteklenen lavabo veri depolarına nasıl kopyalaylayamamayı öğrenin.
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
ms.openlocfilehash: 1f6404da163e075b63a99a1d8474cdba4e064b06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930892"
---
# <a name="copy-data-from-xero-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Xero'dan veri kopyalama

Bu makalede, Xero'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Xero bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Xero'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Xero konektörü destekler:

- Xero [özel uygulama](https://developer.xero.com/documentation/getting-started/api-application-types) ama kamu uygulama.
- "Raporlar" hariç tüm Xero tabloları (API uç noktaları). 

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde Xero bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmıştır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Xero bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Xero** | Evet |
| konak | Xero sunucusunun bitiş noktası`api.xero.com`( ).  | Evet |
| consumerKey | Xero uygulamasıyla ilişkili tüketici anahtarı. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| privateKey | Xero özel uygulamanız için oluşturulan .pem dosyasından özel [anahtar,](https://developer.xero.com/documentation/api-guides/create-publicprivate-key)bkz. Kullanarak **512 numbits ile privatekey.pem** `openssl genrsa -out privatekey.pem 512`oluşturmak için not; 1024 desteklenmez. Unix satır uçları(\n) dahil olmak üzere .pem dosyasındaki tüm metni ekleyin, aşağıdaki örneğe bakın.<br/><br/>Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| useHostVerification | SSL üzerinden bağlanırken sunucunun ana bilgisayar adı ile eşleşecek şekilde sunucunun sertifikasında ana bilgisayar adının gerekli olup olmadığını belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucunun kimliğinin doğrulanıp doğrulamayacağı belirtilir. Varsayılan değer true şeklindedir.  | Hayır |

**Örnek:**

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

Unix satır uçları(\n) dahil olmak üzere .pem dosyasındaki tüm metni ekleyin.

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Xero dataset tarafından desteklenen özelliklerin bir listesini sağlar.

Xero'dan gelen verileri kopyalamak için, veri kümesinin tür özelliğini **XeroObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **XeroObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Xero kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="xero-as-source"></a>Kaynak olarak Xero

Xero'daki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **XeroSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği şu şekilde ayarlanmalıdır: **XeroSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM Contacts"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

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

Xero sorgusunu belirtirken aşağıdakilere dikkat edin:

- Karmaşık öğeleri olan tablolar birden çok tabloya bölünür. Örneğin, Banka hareketlerinin karmaşık bir veri yapısı "LineItems" vardır, bu `Bank_Transaction` nedenle `Bank_Transaction_Line_Items`banka `Bank_Transaction_ID` hareketinin verileri tabloya eşlenir ve yabancı anahtar olarak bunları birbirine bağlamak için kullanılır.

- Xero verileri iki şema aracılığıyla `Minimal` kullanılabilir: `Complete`(varsayılan) ve . Tam şema, istenen sorguyu yapmadan önce ek veri (örneğin kimlik sütunu) gerektiren ön koşul çağrı tabloları içerir.

Aşağıdaki tablolar, Minimal ve Tam şemada aynı bilgilere sahiptir. API çağrılarının sayısını azaltmak için Minimal şema (varsayılan) kullanın.

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
- Invoices_ Ön Ödemeleri 
- Invoices_Overpayments 
- Manual_Journals 
- Fazla ödemeler 
- Overpayments_Allocations 
- Peşinat 
- Prepayments_Allocations 
- Makbuz 
- Receipt_Validation_Errors 
- Tracking_Categories

Aşağıdaki tablolar yalnızca tam şema ile sorgulanabilir:

- Tamamlandı.Bank_Transaction_Line_Items 
- Tamamlandı.Bank_Transaction_Line_Item_Tracking 
- Tamamlandı.Contact_Group_Contacts 
- Komple.Contacts_Contact_ Kişiler 
- Tamamlandı.Credit_Note_Line_Items 
- Tamam.Credit_Notes_Line_Items_Tracking 
- Tam.Expense_Claim_ Ödemeleri 
- Tam.Expense_Claim_Receipts 
- Tamam.Invoice_Line_Items 
- Tamamlandı.Invoices_Line_Items_Tracking
- Tamamlandı.Manual_Journal_Lines 
- Tamam.Manual_Journal_Line_Tracking 
- Tamamlandı.Overpayment_Line_Items 
- Tamamlandı.Overpayment_Line_Items_Tracking 
- Tamam.Prepayment_Line_Items 
- Tamamlandı.Prepayment_Line_Item_Tracking 
- Tamamlandı.Receipt_Line_Items 
- Tamamlandı.Receipt_Line_Item_Tracking 
- Tamamlandı.Tracking_Category_Options

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliğine göre desteklenen veri depolarının listesi için [desteklenen veri depoları'na](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
