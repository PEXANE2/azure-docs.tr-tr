---
title: Azure Data Factory kullanarak SharePoint Online listesinden veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak SharePoint Online listesinden desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: jingwang
ms.openlocfilehash: 90ceb2b716df429eaf4541f13cfa96cb9e0eac7d
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745209"
---
# <a name="copy-data-from-sharepoint-online-list-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SharePoint Online listesinden veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, SharePoint Online listesinden veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Makale, kopyalama etkinliğine genel bir bakış sunan [Azure Data Factory kopyalama etkinliği](copy-activity-overview.md)üzerinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SharePoint Online listesi Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

SharePoint Online listesinden, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının bir listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).

Özellikle, bu SharePoint listesi çevrimiçi Bağlayıcısı hizmet sorumlusu kimlik doğrulaması kullanır ve OData protokolü aracılığıyla verileri alır.

> [!TIP]
> Bu bağlayıcı, SharePoint Online **listesinden** veri kopyalamayı destekler, ancak dosya değil. Dosyayı [SharePoint Online 'Dan kopyalama](#copy-file-from-sharepoint-online) dosyasından kopyalama hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

SharePoint listesi çevrimiçi Bağlayıcısı, SharePoint 'e bağlanmak için hizmet sorumlusu kimlik doğrulamasını kullanır. Ayarlamak için şu adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)kaydederek Azure Active Directory (Azure AD) uygulamasına bir uygulama varlığı kaydedin. Bağlı hizmeti tanımlamak için kullandığınız aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Kayıtlı uygulamanıza SharePoint Online sitesi izni verin: 

    > [!NOTE]
    > Bu işlem SharePoint Online site sahibi iznini gerektirir. Site giriş sayfasına giderek sahibini bulabilirsiniz-> sağ köşedeki "X üyeleri" ne >, "sahip" rolüne kimin sahip olduğunu denetleyin.

    1. SharePoint Online site bağlantısını açın, örneğin `https://[your_site_url]/_layouts/15/appinv.aspx` (kiracı ve site adını değiştirin).
    2. Kaydettiğiniz uygulama KIMLIĞINDE arama yapın, boş alanları doldurup "Oluştur" a tıklayın.

        - Uygulama etki alanı:`localhost.com`
        - Yeniden yönlendirme URL 'SI:`https://www.localhost.com`
        - İzin Isteği XML 'i:

        ```xml
        <AppPermissionRequests AllowAppOnlyPolicy="true">
            <AppPermissionRequest Scope="http://sharepoint/content/sitecollection/web" Right="Read"/>
        </AppPermissionRequests>
        ```

        ![SharePoint izni verme](media/connector-sharepoint-online-list/sharepoint-online-grant-permission.png)

    3. Bu uygulama için "Ona güvenin" düğmesine tıklayın.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SharePoint Online List Connector 'a özgü Data Factory varlıkları tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler bir SharePoint Online listesi bağlı hizmeti için desteklenir:

| **Özellik**        | **Açıklama**                                              | **Gerekli** |
| ------------------- | ------------------------------------------------------------ | ------------ |
| tür                | Type özelliği: **Sharepointonlinelist**olarak ayarlanmalıdır.  | Yes          |
| siteUrl             | SharePoint Online site URL 'si, `https://contoso.sharepoint.com/sites/siteName` ör. | Yes          |
| Serviceprincipalıd  | Azure Active Directory kayıtlı uygulamanın uygulama (istemci) KIMLIĞI. | Yes          |
| Servicesprincipalkey | Uygulamanın anahtarı. Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes          |
| Değerine            | Uygulamanızın bulunduğu kiracı KIMLIĞI.          | Yes          |
| connectVia          | Veri deposuna bağlanmak için kullanılacak [Integration Runtime](concepts-integration-runtime.md) . Bu makalenin önceki kısımlarında yer alarak [önkoşulları](#prerequisites)öğrenin. Belirtilmemişse, varsayılan Azure Integration Runtime kullanılır. | No           |

**Örneğinde**

```json
{
    "name": "SharePointOnlineList",
    "properties": {
        "type": "SharePointOnlineList",
        "typeProperties": {
            "siteUrl": "<site URL>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenantId": "<tenant ID>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md). Aşağıdaki bölüm, SAP tablo veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin **Type** özelliği, **Sharepointonlinelresource**olarak ayarlanmalıdır. | Yes |
| tanımlamalıdır | SharePoint Online listesinin adı. | Yes |

**Örnek**

```json
{
    "name": "SharePointOnlineListDataset",
    "properties":
    {
        "type": "SharePointOnlineListResource",
        "linkedServiceName": {
            "referenceName": "<SharePoint Online List linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "listName": "<name of the list>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliği özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md).  Aşağıdaki bölümde, SharePoint Online liste kaynağı tarafından desteklenen özelliklerin bir listesi verilmiştir.

### <a name="sharepoint-online-list-as-source"></a>Kaynak olarak SharePoint Online listesi

SharePoint Online listesinden veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının **Type** özelliği, **Sharepointonlinelistsource**olarak ayarlanmalıdır. | Yes |
| sorgu | Verileri filtrelemek için özel OData sorgu seçenekleri. Örnek: `"$top=10&$select=Title,Number"`. | No |
| httpRequestTimeout | HTTP isteğinin yanıt alması için zaman aşımı (saniye olarak). Varsayılan değer 300 ' dir (5 dakikadır). | No |

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromSharePointOnlineList",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SharePoint Online List input dataset name>",
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
            "source": {
                "type": "SharePointOnlineListSource",
                "query": "<OData query e.g. $top=10&$select=Title,Number>"
            }, 
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sharepoint-online-list"></a>SharePoint Online listesi için veri türü eşlemesi

SharePoint Online listesinden veri kopyaladığınızda, SharePoint Online listesi veri türleri ve Azure Data Factory geçici veri türleri arasında aşağıdaki eşlemeler kullanılır. 

| **SharePoint Online veri türü**                 | **OData veri türü**                                  | **Azure Data Factory geçici veri türü** |
| ----------------------------------------------- | ---------------------------------------------------- | ---------------------------------------- |
| Tek satırlı metin                             | Edm.String                                           | Dize                                   |
| Birden fazla satır metin                          | Edm.String                                           | Dize                                   |
| Seçim (arasından seçim yapabileceğiniz menü)                    | Edm.String                                           | Dize                                   |
| Sayı (1, 1,0, 100)                            | Edm.Double                                           | Çift                                   |
| Para birimi ($, ¥, €)                              | Edm.Double                                           | Çift                                   |
| Tarih ve Saat                                   | EDM. DateTime                                         | DateTime                                 |
| Arama (zaten bu sitede olan bilgiler)       | Edm.Int32                                            | Int32                                    |
| Evet/Hayır (onay kutusu)                              | Edm.Boolean                                          | Boole                                  |
| Kişi veya Grup                                 | Edm.Int32                                            | Int32                                    |
| Köprü veya resim                            | Edm.String                                           | Dize                                   |
| Hesaplanan (diğer sütunlara dayalı hesaplama) | EDM. String/Edm. Double/Edm. DateTime/Edm. Boolean | Dize/Double/DateTime/Boolean     |
| Ek                                      | Desteklenmiyor                                        |                                          |
| Görev Sonucu                                    | Desteklenmiyor                                        |                                          |
| Dış Veri                                   | Desteklenmiyor                                        |                                          |
| Yönetilen Meta Veriler                                | Desteklenmiyor                                        |                                          |

## <a name="copy-file-from-sharepoint-online"></a>SharePoint Online 'dan Dosya Kopyala

**Web etkinliğini** kullanarak SharePoint Online 'dan dosya KOPYALAYABILIR ve spo adresinden erişim belirteci alabilir ve ardından **http bağlayıcısıyla kaynak olarak**veri kopyalamak için sonraki **kopyalama etkinliğine** geçiş yapabilirsiniz.

![SharePoint kopya dosya akışı](media/connector-sharepoint-online-list/sharepoint-online-copy-file-flow.png)

1. AAD uygulaması oluşturmak ve SharePoint Online 'a izin vermek için [Önkoşullar](#prerequisites) bölümünü izleyin. 

2. SharePoint Online 'dan erişim belirtecini almak için bir **Web etkinliği** oluşturun:

    - **URL**: `https://accounts.accesscontrol.windows.net/[Tenant-ID]/tokens/OAuth/2` . Kiracı KIMLIĞINI değiştirin.
    - **Yöntem**: gönderi
    - **Üst bilgiler**:
        - Content-Type: application/x-www-form-urlencoded
    - **Gövde**: `grant_type=client_credentials&client_id=[Client-ID]@[Tenant-ID]&client_secret=[Client-Secret]&resource=00000003-0000-0ff1-ce00-000000000000/[Tenant-Name].sharepoint.com@[Tenant-ID]` . İstemci KIMLIĞI, gizli anahtar, kiracı KIMLIĞI ve kiracı adı ' nı değiştirin.

    > [!CAUTION]
    > Belirteç değerinin düz metin olarak kaydedilmesini engellemek için Web etkinliğinde güvenli çıkış seçeneğini true olarak ayarlayın. Bu değeri kullanan diğer etkinliklerin, güvenli giriş seçeneğinin true olarak ayarlanmış olması gerekir.

3. SharePoint Online dosya içeriğini kopyalamak için kaynak olarak HTTP bağlayıcısıyla **kopyalama etkinliği** olan zincir:

    - HTTP bağlı hizmeti:
        - **Temel URL**: `https://[site-url]/_api/web/GetFileByServerRelativeUrl('[relative-path-to-file]')/$value` . Site URL 'sini ve göreli yolu dosyanın yerini alır. Dosyaya göre örnek göreli yol `/sites/site2/Shared Documents/TestBook.xlsx` .
        - **Kimlik doğrulama türü:** Anonim *(daha sonra kopyalama etkinliği kaynağında yapılandırılan taşıyıcı belirtecini kullanmak için)*
    - Veri kümesi: istediğiniz biçimi seçin. Dosyayı olduğu gibi kopyalamak için "binary" türü ' nü seçin.
    - Etkinlik kaynağını Kopyala:
        - **İstek yöntemi**: Get
        - **Ek üst bilgi**: `@{concat('Authorization: Bearer ', activity('<Web-activity-name>').output.access_token)}` akış Web etkinliği tarafından Yetkilendirme üstbilgisi olarak oluşturulan taşıyıcı belirtecini kullanan aşağıdaki ifadeyi kullanın. Web etkinliği adını değiştirin.
    - Kopyalama etkinliği havuzunu her zamanki gibi yapılandırın.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Kopyalama etkinliğinin Azure Data Factory kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).
