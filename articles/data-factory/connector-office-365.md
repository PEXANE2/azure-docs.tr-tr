---
title: Azure Veri Fabrikası'nı kullanarak Office 365'ten verileri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık ardışık alanda kopyalama etkinliğini kullanarak desteklenen lavabo veri depolarına Office 365'teki verileri nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/20/2019
ms.author: jingwang
ms.openlocfilehash: d97b3caccc92f0fdfeb229d94e30ee6499c26181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74912414"
---
# <a name="copy-data-from-office-365-into-azure-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Office 365'ten Azure'a veri kopyalama

Azure Veri [Fabrikası, Microsoft Graph veri bağlantısıyla](https://docs.microsoft.com/graph/data-connect-concept-overview)tümleşir ve Office 365 kiracınızdaki zengin kuruluş verilerini ölçeklenebilir bir şekilde Azure'a getirmenize ve bu değerli veri varlıklarına dayalı analitik uygulamalar oluşturmanıza ve öngörüler elde etmenize olanak tanır. Ayrıcalıklı Erişim Yönetimi ile tümleştirme, Office 365'teki değerli küratörlü veriler için güvenli erişim denetimi sağlar.  Microsoft Graph verilerine genel bir bakış için lütfen [bu bağlantıya](https://docs.microsoft.com/graph/data-connect-concept-overview) bakın ve lisans bilgileri için [bu bağlantıya](https://docs.microsoft.com/graph/data-connect-policies#licensing) bakın.

Bu makalede, Office 365'teki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler
ADF Office 365 bağlayıcısı ve Microsoft Graph veri bağlantısı, adres defteri kişileri, takvim etkinlikleri, e-posta iletileri, kullanıcı bilgileri, posta kutusu ayarları ve gibi devam edin.  Kullanılabilir veri kümelerinin tam listesini görmek için [buraya](https://docs.microsoft.com/graph/data-connect-datasets) bakın.

Şimdilik, tek bir kopyalama etkinliği içinde yalnızca **Office 365'teki verileri [Azure Blob Depolama](connector-azure-blob-storage.md), Azure Veri Gölü Depolama Gen1 ve Azure Veri Gölü Depolama [Gen2'den](connector-azure-data-lake-storage.md) JSON biçiminde [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)** kopyalayabilirsiniz (tür kümesiObjects). Office 365'i diğer veri depolarına veya diğer biçimlere yüklemek istiyorsanız, [desteklenen ADF hedef depolarından](copy-activity-overview.md#supported-data-stores-and-formats) herhangi biri içine veri daha fazla yüklemek için sonraki kopyalama etkinliğiyle ilk kopyalama etkinliğini zincirleyebilirsiniz ("Desteklenen veri depoları ve biçimleri" tablosundaki "lavabo olarak desteklenen" sütununa bakın).

>[!IMPORTANT]
>- Veri fabrikasını ve lavabo veri deposunu içeren Azure aboneliği, Office 365 kiracısı ile aynı Azure Etkin Dizin (Azure AD) kiracısının altında olmalıdır.
>- Kopyalama etkinliği için kullanılan Azure Tümleştirme Çalışma Zamanı bölgesinin yanı sıra hedefin Office 365 kiracı kullanıcı posta kutusunun bulunduğu aynı bölgede olduğundan emin olun. Azure IR konumunun nasıl belirlendiğini anlamak için [buraya](concepts-integration-runtime.md#integration-runtime-location) bakın. Desteklenen Office bölgeleri ve ilgili Azure bölgeleri listesi için [tabloya](https://docs.microsoft.com/graph/data-connect-datasets#regions) bakın.
>- Hizmet Sorumlusu kimlik doğrulaması, Hedef Mağazalar olarak Azure Blob Depolama, Azure Veri Gölü Depolama Gen1 ve Azure Veri Gölü Depolama Gen2 için desteklenen tek kimlik doğrulama mekanizmasıdır.

## <a name="prerequisites"></a>Ön koşullar

Office 365'teki verileri Azure'a kopyalamak için aşağıdaki ön koşul adımlarını tamamlamanız gerekir:

- Office 365 kiracı [yöneticiniz, burada](https://docs.microsoft.com/graph/data-connect-get-started)açıklandığı gibi biniş eylemlerini tamamlamalıdır.
- Azure Active Directory'de bir Azure AD web uygulaması oluşturun ve yapılandırıldı.  Yönergeler için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)
- Office 365'in bağlantılı hizmetini tanımlamak için kullanacağınız aşağıdaki değerlere dikkat edin:
    - Kiracı kimliği. Talimatlar için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)
    - Uygulama Kimliği ve Uygulama anahtarı.  Talimatlar için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)
- Azure AD web uygulamasının sahibi olarak veri erişim isteğini yapacak olan kullanıcı kimliğini ekleyin (Azure AD web uygulamasından ayarlar > Sahipleri > Add sahibi) >. 
    - Kullanıcı kimliği, veri aldığınız Office 365 kuruluşunda olmalı ve Konuk kullanıcı olmamalıdır.

## <a name="approving-new-data-access-requests"></a>Yeni veri erişim isteklerini onaylama

Bu bağlam için ilk kez veri talep ediyorsanız (hangi veri tablosuna erişiliyor, hangi hedef hesaba yüklenen veri ve hangi kullanıcı kimliği veri erişim isteği yapıyor), kopyalama etkinliği durumunu "Devam Ediyor" olarak görürsünüz ve yalnızca [Eylemler altındaki "Ayrıntılar" bağlantısına](copy-activity-overview.md#monitoring) tıkladığınızda durumu "İzin İstek" olarak görürsünüz.  Veri ayıklama işlemi nin devam edebilmesi için veri erişimi onaylayıcı grubunun bir üyesinin Ayrıcalıklı Erişim Yönetimi'ndeki isteği onaylaması gerekir.

Onaylayanın veri erişim isteğini nasıl onaylayacağına [buradan](https://docs.microsoft.com/graph/data-connect-tips#approve-pam-requests-via-office-365-admin-portal) bakın ve veri erişimi onaylayıcı grubunun nasıl ayarlayacağı da dahil olmak üzere Ayrıcalıklı Erişim Yönetimi ile genel tümleştirme yle ilgili bir açıklama için [buraya](https://docs.microsoft.com/graph/data-connect-pam) bakın.

## <a name="policy-validation"></a>Doğrulamayı yeniden deneme

Yönetilen bir uygulamanın parçası olarak ADF oluşturulursa ve Azure ilkeleri atamaları yönetim kaynak grubundaki kaynaklarda yapılırsa, her kopyalama etkinliği için ADF ilke atamalarının uygulandığından emin olmak için denetler. Desteklenen ilkelerin listesi için [buraya](https://docs.microsoft.com/graph/data-connect-policies#policies) bakın.

## <a name="getting-started"></a>Başlarken

>[!TIP]
>Office 365 bağlayıcısı kullanma nın bir walkthrough için [Bkz. Office 365 makalesinden veri yükle.](load-office-365-data.md)

Aşağıdaki araçlardan veya SDK'lardan birini kullanarak kopyalama etkinliğiyle bir ardışık hatlar oluşturabilirsiniz. Kopyalama etkinliği olan bir ardışık hatlar oluşturmak için adım adım yönergeleri içeren bir öğreticiye gitmek için bir bağlantı seçin. 

- [Azure portalında](quickstart-create-data-factory-portal.md)
- [.NET SDK](quickstart-create-data-factory-dot-net.md)
- [Python SDK'sı](quickstart-create-data-factory-python.md)
- [Azure PowerShell](quickstart-create-data-factory-powershell.md)
- [REST API](quickstart-create-data-factory-rest-api.md)
- [Azure Kaynak Yöneticisi şablonu](quickstart-create-data-factory-resource-manager-template.md). 

Aşağıdaki bölümler, Office 365 bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Office 365 bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Office365** | Evet |
| office365TenantId | Office 365 hesabının ait olduğu Azure kiracı kimliği. | Evet |
| hizmetPrincipalTenantId | Azure AD web uygulamanızın bulunduğu kiracı bilgilerini belirtin. | Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı Güvenli Bir Şekilde Veri Fabrikası'nda depolamak için SecureString olarak işaretleyin. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak Tümleştirme Çalışma Süresi.  Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır |

>[!NOTE]
> **Office365TenantId** ve **servicePrincipalTenantId** ve sağlamak için ilgili değer arasındaki fark:
>- Kendi kuruluşunuzun kullanımı için Office 365 verilerine karşı bir uygulama geliştiren bir kuruluş geliştiricisiyseniz, kuruluşunuzun AAD kiracı kimliği olan her iki özellik için de aynı kiracı kimliğini sağlamanız gerekir.
>- Müşterileriniz için bir uygulama geliştiren bir ISV geliştiricisiyseniz, office365TenantId müşterinizin (uygulama yükleyicisi) AAD kiracı kimliği ve hizmetPrincipalTenantId şirketinizin AAD kiracı kimliği olacaktır.

**Örnek:**

```json
{
    "name": "Office365LinkedService",
    "properties": {
        "type": "Office365",
        "typeProperties": {
            "office365TenantId": "<Office 365 tenant id>",
            "servicePrincipalTenantId": "<AAD app service principal tenant id>",
            "servicePrincipalId": "<AAD app service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<AAD app service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Office 365 veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

Office 365'teki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **Office365Table** | Evet |
| tableName | Office 365'ten çıkarılacak veri kümesinin adı. Çıkarma için kullanılabilir Office 365 veri kümelerinin listesi için [buraya](https://docs.microsoft.com/graph/data-connect-datasets#datasets) bakın. | Evet |

Eğer ayarı `dateFilterColumn` `startTime`olsaydı `endTime`, `userScopeFilterUri` , , ve veri kümesi, hala olduğu gibi desteklenir, ileriye dönük etkinlik kaynağında yeni modeli kullanmak için önerilir iken.

**Örnek**

```json
{
    "name": "DS_May2019_O365_Message",
    "properties": {
        "type": "Office365Table",
        "linkedServiceName": {
            "referenceName": "<Office 365 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [],
        "typeProperties": {
            "tableName": "BasicDataSet_v0.Event_v1"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Office 365 kaynağı tarafından desteklenen özelliklerin bir listesi sağlar.

### <a name="office-365-as-source"></a>Kaynak olarak Office 365

Office 365'teki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği şu şekilde ayarlanmalıdır: **Office365Source** | Evet |
| izin Verilen Gruplar | Grup seçimi yüklemi.  Verilerin alınacağı en fazla 10 kullanıcı grubu seçmek için bu özelliği kullanın.  Grup belirtilmemişse, tüm kuruluş için veriler döndürülür. | Hayır |
| userScopeFilterUri | Özellik `allowedGroups` belirtilmediğinde, Office 365'ten ayıklamak için belirli satırları filtrelemek için tüm kiracıya uygulanan bir yüklem ifadesi kullanabilirsiniz. Yüklem biçimi, Microsoft Graph API'lerinin sorgu biçimiyle `https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'`eşleşmelidir, örneğin. | Hayır |
| tarihFilterColumn | DateTime filtre sütununun adı. Office 365 verilerinin ayıklandığı zaman aralığını sınırlamak için bu özelliği kullanın. | Veri kümesinde bir veya daha fazla DateTime sütunu varsa evet. Bu DateTime filtresini gerektiren veri kümelerinin listesi için [buraya](https://docs.microsoft.com/graph/data-connect-filtering#filtering) bakın. |
| startTime | Filtreuygulanacak DateTime değerini başlatın. | Belirtilirse `dateFilterColumn` evet |
| endTime | Filtre uygulanacak DateTime değerini sonla. | Belirtilirse `dateFilterColumn` evet |
| outputColumns | Batmak için kopyalamak için sütundizi. | Hayır |

**Örnek:**

```json
"activities": [
    {
        "name": "CopyFromO365ToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Office 365 input dataset name>",
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
                "type": "Office365Source",
                "dateFilterColumn": "CreatedDateTime",
                "startTime": "2019-04-28T16:00:00.000Z",
                "endTime": "2019-05-05T16:00:00.000Z",
                "userScopeFilterUri": "https://graph.microsoft.com/v1.0/users?$filter=Department eq 'Finance'",
                "outputColumns": [
                    {
                        "name": "Id"
                    },
                    {
                        "name": "CreatedDateTime"
                    },
                    {
                        "name": "LastModifiedDateTime"
                    },
                    {
                        "name": "ChangeKey"
                    },
                    {
                        "name": "Categories"
                    },
                    {
                        "name": "OriginalStartTimeZone"
                    },
                    {
                        "name": "OriginalEndTimeZone"
                    },
                    {
                        "name": "ResponseStatus"
                    },
                    {
                        "name": "iCalUId"
                    },
                    {
                        "name": "ReminderMinutesBeforeStart"
                    },
                    {
                        "name": "IsReminderOn"
                    },
                    {
                        "name": "HasAttachments"
                    },
                    {
                        "name": "Subject"
                    },
                    {
                        "name": "Body"
                    },
                    {
                        "name": "Importance"
                    },
                    {
                        "name": "Sensitivity"
                    },
                    {
                        "name": "Start"
                    },
                    {
                        "name": "End"
                    },
                    {
                        "name": "Location"
                    },
                    {
                        "name": "IsAllDay"
                    },
                    {
                        "name": "IsCancelled"
                    },
                    {
                        "name": "IsOrganizer"
                    },
                    {
                        "name": "Recurrence"
                    },
                    {
                        "name": "ResponseRequested"
                    },
                    {
                        "name": "ShowAs"
                    },
                    {
                        "name": "Type"
                    },
                    {
                        "name": "Attendees"
                    },
                    {
                        "name": "Organizer"
                    },
                    {
                        "name": "WebLink"
                    },
                    {
                        "name": "Attachments"
                    },
                    {
                        "name": "BodyPreview"
                    },
                    {
                        "name": "Locations"
                    },
                    {
                        "name": "OnlineMeetingUrl"
                    },
                    {
                        "name": "OriginalStart"
                    },
                    {
                        "name": "SeriesMasterId"
                    }
                ]
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
