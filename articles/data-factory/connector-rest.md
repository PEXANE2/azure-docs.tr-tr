---
title: Azure Veri Fabrikası'nı kullanarak verileri REST kaynağından kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık kaynakta kopyalama etkinliği kullanarak desteklenen lavabo veri depolarına bulut veya şirket içi REST kaynağından verileri nasıl kopyalaylaydestekleyeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: jingwang
ms.openlocfilehash: 2657f1998e3ca908bc52166154ac3353e1e5a66b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415041"
---
# <a name="copy-data-from-a-rest-endpoint-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri REST bitiş noktasından kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Bir REST bitiş noktasından verileri kopyalamak için Azure Veri Fabrikası'nda Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Makale, Azure [Veri Fabrikası'ndaki Kopyalama Etkinliği'ne](copy-activity-overview.md)dayanmaktadır ve bu da Kopyalama Etkinliğine genel bir genel bakış sunar.

Bu REST bağlayıcısı, [HTTP bağlayıcısı](connector-http.md) ve [Web tablosu bağlayıcısı](connector-web-table.md) arasındaki fark şunlardır:

- **REST bağlayıcısı** özellikle RESTful API'lerden veri kopyalamayı destekler; 
- **HTTP bağlayıcısı,** herhangi bir HTTP bitiş noktasından (örneğin dosyayı indirmek için) veri almak için geneldir. Bu REST konektörü kullanıma sunulmadan önce, REST konektörüyle karşılaştırıldığında desteklenen ancak daha az işlevsel olan RESTful API'den verileri kopyalamak için HTTP bağlayıcısını kullanabilirsiniz.
- **Web tablosu bağlayıcısı** tablo içeriğini bir HTML web sayfasından ayıklar.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Verileri bir REST kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama Etkinliği'nin kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Özellikle, bu genel REST konektörü destekler:

- **GET** veya **POST** yöntemlerini kullanarak BIR REST bitiş noktasından veri alma.
- Aşağıdaki kimlik doğrulamalarından birini kullanarak veri alma: **Anonim**, **Temel**, **AAD hizmet sorumlusu**ve **Azure kaynakları için yönetilen kimlikler.**
- REST API'larda **[pagination.](#pagination-support)**
- REST JSON yanıtını [olduğu gibi](#export-json-response-as-is) kopyalamak veya [şema eşlemi](copy-activity-schema-and-type-mapping.md#schema-mapping)kullanarak ayrıştırmak. Yalnızca **JSON'daki** yanıt yükü desteklenir.

> [!TIP]
> Veri Fabrikası'nda REST bağlayıcısını yapılandırmadan önce veri alma isteğini sınamak için üstbilgi ve gövde gereksinimleri için API belirtimi hakkında bilgi edinin. Doğrulamak için Postacı veya web tarayıcısı gibi araçları kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, REST bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

REST bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **RestService**olarak ayarlanmalıdır. | Evet |
| url | REST hizmetinin temel URL'si. | Evet |
| enableServerCertificateValidation | Bitiş noktasına bağlanırken sunucu tarafındaki TLS/SSL sertifikasını doğrulayıp doğrulamayacağı. | Hayır<br /> (varsayılan **değer doğrudur)** |
| authenticationType | REST hizmetine bağlanmak için kullanılan kimlik doğrulama türü. İzin verilen değerler **Anonim**, **Temel**, **AadServicePrincipal** ve **ManagedServiceIdentity'dir.** Daha fazla özellik ve örnekler sırasıyla aşağıdaki ilgili bölümlere bakın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, bu özellik varsayılan Azure Tümleştirme Çalışma Zamanı'nı kullanır. |Hayır |

### <a name="use-basic-authentication"></a>Temel kimlik doğrulamayı kullanma

Kimlik **doğrulamaTürü** özelliğini **Temel**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| userName | REST bitiş noktasına erişmek için kullanılacak kullanıcı adı. | Evet |
| password | Kullanıcının şifresi **(kullanıcı Adı** değeri). Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** türü olarak işaretleyin. [Azure Key Vault'ta depolanan bir gizli ye de başvuruda](store-credentials-in-key-vault.md)bulunabilirsiniz. | Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<REST endpoint>",
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

### <a name="use-aad-service-principal-authentication"></a>AAD hizmet temel kimlik doğrulamasını kullanma

Kimlik **doğrulamaTürü** özelliğini **AadServicePrincipal**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| hizmetPrincipalId | Azure Etkin Dizin uygulamasının istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Azure Active Directory uygulamasının anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet |
| aadResourceId | Yetkilendirme için talep ettiğiniz AAD kaynağını belirtin, `https://management.core.windows.net`örneğin.| Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanma

Kimlik **doğrulamaTürü** özelliğini **ManagedServiceIdentity**olarak ayarlayın. Önceki bölümde açıklanan genel özelliklere ek olarak, aşağıdaki özellikleri belirtin:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| aadResourceId | Yetkilendirme için talep ettiğiniz AAD kaynağını belirtin, `https://management.core.windows.net`örneğin.| Evet |

**Örnek**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "ManagedServiceIdentity",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bu bölümde, REST veri kümesinin desteklediği özelliklerin bir listesi sağlar. 

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md) 

REST'teki verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **RestResource**olarak ayarlanmalıdır. | Evet |
| Relativeurl | Verileri içeren kaynağa göreli bir URL. Bu özellik belirtilmediği zaman, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. HTTP bağlayıcısı verileri birleştirilmiş URL'den kopyalar: `[URL specified in linked service]/[relative URL specified in dataset]`. | Hayır |

`requestMethod` `additionalHeaders`Yeni modeli ileriye dönük olarak etkinlik kaynağında kullanmanız önerilirken, veri kümesinde olduğu gibi yine de `requestBody` `paginationRules` desteklenir.

**Örnek:**

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "typeProperties": {
            "relativeUrl": "<relative url>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, REST kaynağının desteklediği özelliklerin bir listesi sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) 

### <a name="rest-as-source"></a>KAYNAK OLARAK REST

Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **türü** özelliği **RestSource**olarak ayarlanmalıdır. | Evet |
| requestMethod | HTTP yöntemi. İzin verilen değerler **Al** (varsayılan) ve **Post'dur.** | Hayır |
| ek Headers | Ek HTTP istek üstbilgi. | Hayır |
| requestBody | HTTP isteği için gövde. | Hayır |
| paginationKurallar | Pagination kuralları sonraki sayfa isteklerioluşturmak için. Ayrıntılar la ilgili [pagination destek](#pagination-support) bölümüne bakın. | Hayır |
| httpRequestTimeout | Yanıt almak için HTTP isteğiiçin zaman aralığı **(TimeSpan** değeri). Bu değer, yanıt verilerini okumak için zaman arası değil, yanıt almak için zaman aramıdır. Varsayılan değer **00:01:40'dır.**  | Hayır |
| requestInterval | Sonraki sayfa için istek göndermeden önce bekleme zamanı. Varsayılan değer **00:00:01** |  Hayır |

>[!NOTE]
>REST bağlayıcısı, .' de belirtilen `additionalHeaders`herhangi bir "Kabul Et" üstbilgisini yoksayılsa. REST konektörü yalnızca JSON'da yanıtı desteklediğinden, `Accept: application/json`otomatik olarak bir üstbilgi oluşturur.

**Örnek 1: Pagination ile Get yöntemini kullanma**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "additionalHeaders": {
                    "x-user-defined": "helloworld"
                },
                "paginationRules": {
                    "AbsoluteUrl": "$.paging.next"
                },
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Örnek 2: Post yöntemini kullanma**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "requestMethod": "Post",
                "requestBody": "<body for POST REST request>",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="pagination-support"></a>Pagination desteği

Normalde, REST API makul bir sayı altında tek bir istek yanıt yükü boyutunu sınırlar; büyük miktarda veri döndürmek için, sonucu birden çok sayfaya böler ve arayanların sonucun bir sonraki sayfasını almak için ardışık istekler göndermesini gerektirir. Genellikle, bir sayfa için istek dinamik tir ve önceki sayfanın yanıtından döndürülen bilgilerden oluşur.

Bu genel REST konektörü aşağıdaki pagination desenleri destekler: 

* Sonraki isteğin mutlak veya göreli URL ' si = geçerli yanıt gövdesindeki özellik değeri
* Sonraki isteğin mutlak veya göreli URL = geçerli yanıt üstbilgilerinde üstbilgi değeri
* Sonraki isteğin sorgu parametresi = geçerli yanıt gövdesindeki özellik değeri
* Sonraki isteğin sorgu parametresi = geçerli yanıt üstbilgilerinde üstbilgi değeri
* Sonraki isteğin üstbilgisi = geçerli yanıt gövdesindeki özellik değeri
* Sonraki isteğin üstbilgi = geçerli yanıt üstbilgisinde üstbilgi değeri

**Pagination kuralları,** bir veya daha fazla büyük/küçük harf duyarlı anahtar değeri çiftleri içeren veri kümesinde bir sözlük olarak tanımlanır. Yapılandırma, ikinci sayfadan başlayarak isteği oluşturmak için kullanılır. Bağlayıcı, HTTP durum kodu 204 (İçerik Yok) aldığında veya "paginationRules" daki JSONPath ifadelerinden herhangi biri geçersiz olduğunda yinelenmeyi durdurur.

Pagination kurallarında **desteklenen anahtarlar:**

| Anahtar | Açıklama |
|:--- |:--- |
| AbsoluteUrl | Bir sonraki isteği vermek için URL'yi gösterir. Bu **mutlak URL veya göreli URL**olabilir. |
| SorguParametreleri. *request_query_parameter* OR QueryParameters['request_query_parameter'] | "request_query_parameter", bir sonraki HTTP istek URL'sinde bir sorgu parametre adı referansı olan kullanıcı tarafından tanımlanır. |
| Üstbilgi. *request_header* VEYA Başlıklar['request_header'] | "request_header", bir sonraki HTTP isteğinde bir üstbilgi adına başvurulan kullanıcı tanımlıdır. |

Pagination kurallarında **desteklenen değerler:**

| Değer | Açıklama |
|:--- |:--- |
| Üstbilgi. *response_header* VEYA Başlıklar['response_header'] | "response_header", geçerli HTTP yanıtında bir üstbilgi adına başvurulan kullanıcı tanımlıdır ve değeri bir sonraki isteği vermek için kullanılacaktır. |
| "$" ile başlayan bir JSONPath ifadesi (yanıt gövdesinin kökünü temsil eder) | Yanıt gövdesi yalnızca bir JSON nesnesi içermelidir. JSONPath ifadesi, sonraki isteği vermek için kullanılacak tek bir ilkel değer döndürmelidir. |

**Örnek:**

Facebook Graph API yanıtı aşağıdaki yapıda döndürür, bu durumda sonraki sayfanın URL'si sayfalama da temsil ***edilir:sonraki:***

```json
{
    "data": [
        {
            "created_time": "2017-12-12T14:12:20+0000",
            "name": "album1",
            "id": "1809938745705498_1809939942372045"
        },
        {
            "created_time": "2017-12-12T14:14:03+0000",
            "name": "album2",
            "id": "1809938745705498_1809941802371859"
        },
        {
            "created_time": "2017-12-12T14:14:11+0000",
            "name": "album3",
            "id": "1809938745705498_1809941879038518"
        }
    ],
    "paging": {
        "cursors": {
            "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
            "before": "NDMyNzQyODI3OTQw"
        },
        "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw",
        "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
    }
}
```

İlgili REST kopya etkinliği kaynak `paginationRules` yapılandırması özellikle aşağıdaki gibidir:

```json
"typeProperties": {
    "source": {
        "type": "RestSource",
        "paginationRules": {
            "AbsoluteUrl": "$.paging.next"
        },
        ...
    },
    "sink": {
        "type": "<sink type>"
    }
}
```

## <a name="use-oauth"></a>OAuth kullanma
Bu bölümde, OAuth kullanarak REST bağlayıcısından JSON formatında Azure Veri Gölü Depolamasına veri kopyalamak için bir çözüm şablonu nasıl kullanılacağı açıklanmaktadır. 

### <a name="about-the-solution-template"></a>Çözüm şablonu hakkında

Şablon iki etkinlik içerir:
- **Web** etkinliği taşıyıcı belirteci alır ve sonra yetkilendirme olarak sonraki Kopyalama etkinliğine geçirin.
- **Etkinlik** kopyalarını REST'ten Azure Veri Gölü Depolamasına kopyalayın.

Şablon iki parametre tanımlar:
- **SinkContainer,** Azure Veri Gölü Depolamaalanınızda verilerin kopyalandığı kök klasör yoludur. 
- **SinkDirectory,** Azure Veri Gölü Depolamanızda verilerin kopyalandığı kök altındaki dizin yoludur. 

### <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. OAuth şablonu **kullanarak REST veya HTTP'den Kopya'ya** gidin. Kaynak Bağlantı için yeni bir bağlantı oluşturun. 
    ![Yeni bağlantılar oluşturma](media/solution-template-copy-from-rest-or-http-using-oauth/source-connection.png)

    Yeni bağlantılı hizmet (REST) ayarları için önemli adımlar aşağıda verilmiştir:
    
     1. **Temel URL**altında, kendi kaynak REST hizmetinizin url parametresini belirtin. 
     2. **Kimlik Doğrulama türü için** *Anonymous'u*seçin.
        ![Yeni REST bağlantısı](media/solution-template-copy-from-rest-or-http-using-oauth/new-rest-connection.png)

2. Hedef Bağlantısı için yeni bir bağlantı oluşturun.  
    ![Yeni Gen2 bağlantısı](media/solution-template-copy-from-rest-or-http-using-oauth/destination-connection.png)

3. **Bu şablonu kullan'ı**seçin.
    ![Bu şablonu kullan](media/solution-template-copy-from-rest-or-http-using-oauth/use-this-template.png)

4. Oluşturulan ardışık hattı aşağıdaki örnekte gösterildiği ![gibi görürsünüz: Boru Hattı](media/solution-template-copy-from-rest-or-http-using-oauth/pipeline.png)

5. **Web** etkinliğini seçin. **Ayarlar'da,** verileri **URL**kopyalamak istediğiniz hizmetin **Body** giriş API'sinden OAuth taşıyıcı belirteci belirtin. **Method** **Headers** Şablondaki yer tutucu, Azure Active Directory (AAD) OAuth'un bir örneğini sergiler. Not AAD kimlik doğrulaması, REST bağlayıcısı tarafından yerel olarak desteklenir, burada OAuth akışı için sadece bir örnektir. 

    | Özellik | Açıklama |
    |:--- |:--- |:--- |
    | URL'si |OAuth taşıyıcı belirteci belirtene almak için url'yi belirtin. örneğin buradaki örnektehttps://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token |. 
    | Yöntem | HTTP yöntemi. İzin verilen değerler **Post** ve **Get**'tir. | 
    | Üst bilgiler | Üstbilgi, HTTP isteğinde bir üstbilgi adına başvuran kullanıcı tanımlıdır. | 
    | Gövde | HTTP isteği için gövde. | 

    ![İşlem hattı](media/solution-template-copy-from-rest-or-http-using-oauth/web-settings.png)

6. **Veri Kopyalama** etkinliğinde *Kaynak* sekmesini seçin, önceki adımdan alınan taşıyıcı belirteci (access_token) Ek üstbilgiler altında **Yetkilendirme** olarak veri etkinliğini kopyala'ya geçirileceğini görebilirsiniz. Bir ardışık hatlar çalışmasını başlatmadan önce aşağıdaki özelliklerin ayarlarını onaylayın.

    | Özellik | Açıklama |
    |:--- |:--- |:--- | 
    | İstek yöntemi | HTTP yöntemi. İzin verilen değerler **Al** (varsayılan) ve **Post'dur.** | 
    | Ek üstbilgi | Ek HTTP istek üstbilgi.| 

   ![Kaynak Kimlik Doğrulaması'nı kopyalama](media/solution-template-copy-from-rest-or-http-using-oauth/copy-data-settings.png)

7. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.
   ![Boru hattı çalıştır](media/solution-template-copy-from-rest-or-http-using-oauth/pipeline-run.png) 

8. Ardışık hatlar çalışırca başarıyla tamamlandığında, aşağıdaki örneğe ![benzer sonucu görürsünüz: Pipeline run sonucu](media/solution-template-copy-from-rest-or-http-using-oauth/run-result.png) 

9. **Eylemler** sütununda WebAktivite'nin "Çıktı" simgesini tıklatın, hizmet tarafından döndürülen access_token görürsünüz.

   ![Belirteç çıktısı](media/solution-template-copy-from-rest-or-http-using-oauth/token-output.png) 

10. **Eylemler'de** CopyActivity sütununun "Giriş" simgesine tıkladığınızda, WebActivity tarafından alınan access_token kimlik doğrulaması için CopyActivity'e geçirildiğini görürsünüz. 

    ![Belirteç girişi](media/solution-template-copy-from-rest-or-http-using-oauth/token-input.png)
        
    >[!CAUTION] 
    >Belirteci düz metinde günlüğe kaydedilmesini önlemek için, Web etkinliğinde "Güvenli çıktı" ve Kopya etkinliğinde "Güvenli giriş" 'yi" etkinleştirin.


## <a name="export-json-response-as-is"></a>İhracat JSON yanıtı olarak-is

Bu REST konektörünü, çeşitli dosya tabanlı mağazalara olduğu gibi REST API JSON yanıtını dışa aktarmak için kullanabilirsiniz. Bu tür şema-agnostik kopya elde etmek için, veri kümesindeki "yapı" *(şema*olarak da adlandırılır) bölümünü ve kopyalama etkinliğinde şema eşleciliğini atlayın.

## <a name="schema-mapping"></a>Şema haritalama

REST uç noktasından tabular lavaboya kadar verileri kopyalamak için [şema eşleciliği'ne](copy-activity-schema-and-type-mapping.md#schema-mapping)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)
