---
title: Azure Veri Gölü Depolama Gen2'de verileri kopyalama ve dönüştürme
description: Azure Veri Gölü Depolama Gen2'ye ve Bu Veriler'den nasıl kopyalayarak azure veri depolama gen2'deki verileri nasıl dönüştüreceklerini öğrenin ve Azure Veri Fabrikası'nı kullanarak verileri Azure Veri Depolama Gen2'de dönüştürün.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 3c7ff0061a57d1a1a7525ec03b4f77c117415ca5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80155859"
---
# <a name="copy-and-transform-data-in-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure Veri Gölü Depolama Gen2'deki verileri kopyalama ve dönüştürme

Azure Veri Gölü Depolama Gen2 (ADLS Gen2), [Azure Blob depolamada](../storage/blobs/storage-blobs-introduction.md)yerleşik büyük veri analitiğine adanmış bir dizi özelliktir. Hem dosya sistemi hem de nesne depolama paradigmalarını kullanarak verilerinizi arabirim de kullanabilirsiniz.

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin Azure Veri Gölü Deposu Gen2'den ve Azure Veri Gölü Deposu Gen2'den kopyalanması ve Azure Veri Gölü Depolama Gen2'deki verileri dönüştürmek için Veri Akışı'nı nasıl kullanacağı açıklanmaktadır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Veri Gölü Depolama Gen2 bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Kopyalama etkinliği için, bu bağlayıcı ile şunları yapabilirsiniz:

- Azure kaynakları kimlik doğrulaması için hesap anahtarını, hizmet sorumlusu veya yönetilen kimlikleri kullanarak verileri Azure Veri Gölü Depolama Gen2'den kopyalayın.
- Dosyaları olduğu gibi kopyalayın veya ayrıştırın veya [desteklenen dosya biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları oluşturun.
- [Kopya sırasında dosya meta verilerini koruyun.](#preserve-metadata-during-copy)
- Azure Veri Gölü Depolama Gen1/Gen2'den kopyalanırken [ATL'leri koruyun.](#preserve-acls)

>[!IMPORTANT]
>Güvenilen Microsoft hizmetlerinin Azure Depolama güvenlik duvarı ayarlarında **bu depolama hesabı seçeneğine erişmesini** etkinleştiriyorsanız ve Veri Gölü Depolama Gen2'nize bağlanmak için Azure tümleştirme çalışma süresini kullanmak istiyorsanız, ADLS Gen2 için yönetilen kimlik kimlik [doğrulamasını](#managed-identity) kullanmanız gerekir.

>[!TIP]
>Hiyerarşik ad alanını etkinleştiriyorsanız, şu anda Blob ve Data Lake Storage Gen2 API'leri arasında birlikte çalışabilirlik yok. "Belirtilen dosya sistemi yok" iletisi ile "ErrorCode=FilesystemNotFound" hatasına basarsanız, bunun nedeni, Başka bir yerde Veri Gölü Depolama Gen2 API yerine Blob API ile oluşturulan belirtilen lavabo dosya sisteminden kaynaklanır. Sorunu gidermek için, Blob kapsayıcısının adı olarak var olmayan bir ada sahip yeni bir dosya sistemi belirtin. Daha sonra Veri Fabrikası, veri kopyalama sırasında bu dosya sistemini otomatik olarak oluşturur.

## <a name="get-started"></a>Kullanmaya başlayın

>[!TIP]
>Veri Gölü Depolama Gen2 konektörünün nasıl kullanılacağına bir göz atmak için, [Verileri Azure Veri Gölü Depolama Gen2'ye Yükleyin'e](load-azure-data-lake-storage-gen2.md)bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Veri Gölü Depolama Gen2'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Veri Gölü Depolama Gen2 bağlayıcısı aşağıdaki kimlik doğrulama türlerini destekler. Ayrıntılar için ilgili bölümlere bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynakları kimlik doğrulaması için yönetilen kimlikler](#managed-identity)

>[!NOTE]
>Verileri SQL Veri Ambarı'na yüklemek için PolyBase'i kullanırken, kaynak Veri Gölü Depolama Gen2'niz Sanal Ağ bitiş noktasıyla yapılandırıldıysa, PolyBase tarafından gerekli olduğu şekilde yönetilen kimlik doğrulamasını kullanmanız gerekir. Daha fazla yapılandırma ön koşuluyla [yönetilen kimlik kimlik doğrulama](#managed-identity) bölümüne bakın.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtar kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobFS**olarak ayarlanmalıdır. |Evet |
| url | Veri Gölü Depolama Gen2 için bitiş `https://<accountname>.dfs.core.windows.net`noktası. | Evet |
| hesapAnahtar | Veri Gölü Depolama Gen2 için hesap anahtarı. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Veri mağazanız özel bir ağdaysa Azure tümleştirme çalışma zamanını veya kendi kendine barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Bu özellik belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

>[!NOTE]
>İkincil ADLS dosya sistemi bitiş noktası hesap anahtarı kimlik doğrulaması kullanırken desteklenmez. Diğer kimlik doğrulama türlerini kullanabilirsiniz.

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet temel kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. [Uygulamanızı azure AD kiracısıyla kaydettirme](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)adımlarını izleyerek bir uygulama kuruluşunu Azure Etkin Dizin'e (Azure AD) kaydedin. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet müdürüne uygun izin ver. [Dosyalar ve dizinler üzerindeki Access denetim listelerinden](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories) Veri Gölü Depolama Gen2'de iznin nasıl çalıştığına ilişkin örneklere bakın

    - **Kaynak olarak**: Depolama Gezgini'nde, dosyaların kopyalanması için **Okuma** izniyle birlikte en azından TÜM yukarı klasörler ve dosya sistemi için **Yürütme** izni ver. Alternatif olarak, Access denetiminde (IAM), en azından **Depolama Blob Veri Okuyucu** rolünü verir.
    - **Lavabo olarak**: Depolama Gezgini'nde, lavabo klasörü için **Yazma** izniyle birlikte en **azından** TÜM yukarı klasörler ve dosya sistemi için yürütme izni ver. Alternatif olarak, Access denetiminde (IAM), en azından **Depolama Blob Veri Katılımcısı** rolünü verir.

>[!NOTE]
>Yazar için Veri Fabrikası Kullanıcı Arabirimi'ni kullanıyorsanız ve hizmet ilkesi IAM'de "Depolama Blob Veri Okuyucu/Katılımcı" rolüyle ayarlanmamışsa, test bağlantısı yaparken veya klasörlerde gezinirken "Dosya yoluna test bağlantısı" veya "Belirtilen yoldan göz at" seçeneğini belirleyin ve devam etmek için **Oku + Yürüt** izni ile bir yol belirtin.

Bu özellikler bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobFS**olarak ayarlanmalıdır. |Evet |
| url | Veri Gölü Depolama Gen2 için bitiş `https://<accountname>.dfs.core.windows.net`noktası. | Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı Veri `SecureString` Fabrikası'nda güvenli bir şekilde depolamak için işaretleyin. Veya Azure [Key Vault'ta depolanan bir gizli ye başvuru](store-credentials-in-key-vault.md)da bulunabilirsiniz. | Evet |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Veri mağazanız özel bir ağdaysa Azure tümleştirme çalışma zamanını veya kendi kendine barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikler

Bir veri fabrikası, bu özel veri fabrikasını temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md)ilişkilendirilebilir. Bu yönetilen kimliği, kendi hizmet yöneticinizi kullanmaya benzer şekilde Veri Gölü Depolama Gen2 kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu, belirlenmiş fabrikanın Veri Gölü Depolama Gen2'nize veya verilerinize erişmesine ve bunları kopyalamasına olanak tanır.

Azure kaynak kimlik doğrulaması için yönetilen kimlikleri kullanmak için aşağıdaki adımları izleyin.

1. Fabrikanızla birlikte oluşturulan **yönetilen kimlik nesnesi kimliğinin** değerini kopyalayarak Veri Fabrikası tarafından yönetilen kimlik bilgilerini [alın.](data-factory-service-identity.md#retrieve-managed-identity)

2. Yönetilen kimlik uygun izni ni verir. [Dosyalar ve dizinler üzerindeki Access denetim listelerinden](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)Veri Gölü Depolama Gen2'de izin nasıl çalıştığına ilişkin örneklere bakın.

    - **Kaynak olarak**: Depolama Gezgini'nde, dosyaların kopyalanması için **Okuma** izniyle birlikte en azından TÜM yukarı klasörler ve dosya sistemi için **Yürütme** izni ver. Alternatif olarak, Access denetiminde (IAM), en azından **Depolama Blob Veri Okuyucu** rolünü verir.
    - **Lavabo olarak**: Depolama Gezgini'nde, lavabo klasörü için **Yazma** izniyle birlikte en **azından** TÜM yukarı klasörler ve dosya sistemi için yürütme izni ver. Alternatif olarak, Access denetiminde (IAM), en azından **Depolama Blob Veri Katılımcısı** rolünü verir.

>[!NOTE]
>Yazar için Veri Fabrikası Kullanıcı Arabirimi'ni kullanıyorsanız ve yönetilen kimlik IAM'de "Depolama Blob Veri Okuyucu/Katılımcı" rolüyle ayarlanmamışsa, test bağlantısı yaparken veya klasörlerde gezinirken "Dosya yoluna test bağlantısı" veya "Belirtilen yoldan göz at" seçeneğini belirleyin ve devam etmek için **Oku + Yürüt** izni ile bir yol belirtin.

>[!IMPORTANT]
>Veri Gölü Depolama Gen2'den verileri SQL Veri Ambarı'na yüklemek için PolyBase kullanıyorsanız, Veri Gölü Depolama Gen2 için yönetilen kimlik kimlik doğrulamasını kullanırken, [bu kılavuzdaki](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) 1 ve 2 adımlarını da takip ettiğinizden emin olun 1) SQL Veritabanı sunucunuzu Azure Active Directory (Azure AD) ile kaydedin ve 2) Depolama Blob Veri Katılımcısı rolünü SQL Veritabanı sunucunuza atayın; geri kalanı Veri Fabrikası tarafından işlenir. Veri Gölü Depolama Gen2'niz, ondan veri yüklemek için PolyBase'i kullanmak üzere bir Azure Sanal Ağ bitiş noktasıyla yapılandırıldıysa, PolyBase tarafından gerekli olduğu şekilde yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir.

Bu özellikler bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobFS**olarak ayarlanmalıdır. |Evet |
| url | Veri Gölü Depolama Gen2 için bitiş `https://<accountname>.dfs.core.windows.net`noktası. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Veri mağazanız özel bir ağdaysa Azure tümleştirme çalışma zamanını veya kendi kendine barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-datasets-linked-services.md)

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarlar altında Veri Gölü Depolama Gen2 için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri `location` kümesindeki tür özelliği **AzureBlobFSLocation**olarak ayarlanmalıdır. | Evet      |
| fileSystem | Veri Gölü Depolama Gen2 dosya sistem adı.                              | Hayır       |
| folderPath | Verilen dosya sistemi altında bir klasöre giden yol. Klasörlere filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarında belirtin. | Hayır       |
| fileName   | Verilen dosyaSistemi + folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarında belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin [ve](copy-activity-overview.md#configuration) özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) Bu bölümde, Veri Gölü Depolama Gen2 kaynağı ve lavabotarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Kaynak türü olarak Azure Veri Gölü Depolama Gen2

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama kaynağındaki `storeSettings` ayarlar altında Veri Gölü Depolama Gen2 için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureBlobFSReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olduğunda ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasör veya alt klasör kopyalanmaz veya lavaboda oluşturulmaz. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Verilen dosya sistemi altında joker karakter içeren klasör yolu, kaynak klasörleri filtrelemek üzere veri kümesinde yapılandırılır. <br>İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızı n içinizde joker karakter veya bu kaçış char varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Verilen dosya sistemi + folderPath/wildcardFolderPath altında joker karakter içeren dosya adı kaynak dosyaları filtrelemek için. <br>İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızı n içinizde joker karakter veya bu kaçış char varsa kaçmak için kullanın. Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, bu, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, bu, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
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
                    "type": "AzureBlobFSReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Lavabo türü olarak Azure Veri Gölü Depolama Gen2

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama lavabosundaki ayarlar altında `storeSettings` Veri Gölü Depolama Gen2 için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureBlobFSWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır       |
| blockSizeInMB | ADLS Gen2'ye veri yazmak için kullanılan MB'deki blok boyutunu belirtin. [Blobs Engelle hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)daha fazla bilgi edinin. <br/>İzin verilen değer **4 ile 100 MB arasındadır.** <br/>Varsayılan olarak, ADF kaynak mağaza türüve verilerinize bağlı olarak blok boyutunu otomatik olarak belirler. ADLS Gen2'ye ikili olmayan kopyaiçin varsayılan blok boyutu en fazla 4,95 TB veriye sığacak şekilde 100 MB'dır. Verileriniz büyük olmadığında, özellikle de çalışma zaman ayarı veya performans sorunuyla sonuçlanan kötü ağla Kendi kendine barındırılan Tümleştirme Çalışma Süresi'ni kullandığınızda en iyi olmayabilir. BlockSizeInMB*50000'in verileri depolayacak kadar büyük olduğundan emin olurken, bir blok boyutunu açıkça belirtebilirsiniz, aksi takdirde kopyalama etkinliği başarısız olur. | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobFSWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolunun ve dosya adının joker karakter filtreleriyle ortaya çıkan davranışı açıklanmaktadır.

| folderPath | fileName | Özyinelemeli | Kaynak klasör yapısı ve filtre sonucu **(kalın** renkteki dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Boş, varsayılan kullanın) | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | (Boş, varsayılan kullanın) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Bazı özyinelemeli ve copyBehavior örnekleri

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Elde çıkan hedef |
|:--- |:--- |:--- |:--- |
| true |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 kaynakla aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 |
| true |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulmuş ad |
| true |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 içeriği otomatik olarak oluşturulmuş bir dosya adı ile bir dosyada birleştirilir. |
| yanlış |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulmuş bir dosya adı ile bir dosyada birleştirilir. File1 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="preserve-metadata-during-copy"></a>Kopyalama sırasında meta verileri koruma

Dosyaları Amazon S3/Azure Blob/Azure Veri Gölü Depolama Gen2'den Azure Veri Gölü Depolama Gen2/Azure Blob'a kopyalarken, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz. [Meta verileri koru](copy-activity-preserve-metadata.md#preserve-metadata)adresinden daha fazla bilgi edinin.

## <a name="preserve-acls-from-data-lake-storage-gen1gen2"></a><a name="preserve-acls"></a>Veri Gölü Depolama Gen1/Gen2'den ATL'leri Koruyun

Dosyaları Azure Veri Gölü Depolama Gen1/Gen2'den Gen2'ye kopyalarken, verilerle birlikte POSIX erişim denetim listelerini (ACD'ler) korumayı seçebilirsiniz. [Data Lake Storage Gen1/Gen2'den Gen2'ye kadar Koruma ALA'larından](copy-activity-preserve-metadata.md#preserve-acls)daha fazla bilgi edinin.

>[!TIP]
>Azure Veri Gölü Depolama Gen1'deki verileri genel olarak Gen2'ye kopyalamak için, gezinme ve en iyi uygulamalar için [Azure Veri Fabrikası ile Azure Veri Gölü Depolama Gen1'den Gen2'ye verileri kopyala'ya](load-azure-data-lake-storage-gen2-from-gen1.md) bakın.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede verileri dönüştürürken, JSON, Avro, Sınırlı Metin veya Parke biçiminde Azure Veri Gölü Depolama Gen2'den dosyaları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için, eşleme veri akışı özelliğinde [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Kaynak dönüşümünde, Azure Veri Gölü Depolama Gen2'deki bir kapsayıcı, klasör veya tek tek dosyadan okuyabilirsiniz. **Kaynak seçenekleri** sekmesi, dosyaların nasıl okunduğunu yönetmenize olanak tanır. 

![Kaynak seçenekleri](media/data-flow/sourceOptions1.png "Kaynak seçenekleri")

**Joker karakter yolu:** Joker karakter deseni kullanmak, ADF'ye eşleşen her klasör ve dosyayı tek bir Kaynak dönüşümünde döngüye sokmasını emreder. Bu, tek bir akış içinde birden çok dosyayı işlemek için etkili bir yoldur. Varolan joker karakter deseninizin üzerinde gezinirken görünen + işaretiyle birden çok joker karakter eşleştirme deseni ekleyin.

Kaynak kapsayıcınızdan, desenle eşleşen bir dizi dosya seçin. Veri kümesinde yalnızca kapsayıcı belirtilebilir. Bu nedenle joker yolunuz, kök klasöründen klasör yolunuzu da içermelidir.

Joker karakter örnekleri:

* ```*```Herhangi bir karakter kümesini temsil eder
* ```**```Özyinelemeli dizin iç içe geçme temsil eder
* ```?```Bir karakterin yerine
* ```[]```Köşeli ayraçlarda daha fazla karakterden biriyle eşleşir

* ```/data/sales/**/*.csv```Tüm csv dosyalarını /data/satış altında alır
* ```/data/sales/20??/**/```20. yüzyıldaki tüm dosyaları alır
* ```/data/sales/*/*/*.csv```CSV dosyalarını /data/sales altında iki seviye alır
* ```/data/sales/2004/*/12/[XY]1?.csv```2004 yılında aralık ayında tüm csv dosyalarını iki basamaklı bir sayı ile önceden belirlenmiş X veya Y ile başlayarak alır

**Bölüm Kök Yolu:** Dosya kaynağınızda bir ```key=value``` biçimle bölümlenmiş klasörleriniz varsa (örneğin, yıl=2019), veri akışı veri akışınızdaki bir sütun adına bu bölüm klasörü ağacının üst düzeyini atayabilirsiniz.

İlk olarak, bölümlenmiş klasörler ve okumak istediğiniz yaprak dosyaları olan tüm yolları içerecek şekilde bir joker karakter ayarlayın.

![Bölüm kaynak dosya ayarları](media/data-flow/partfile2.png "Bölüm dosyası ayarı")

Klasör yapısının en üst düzeyinin ne olduğunu tanımlamak için Bölüm Kök Yolu ayarını kullanın. Verilerinizin içeriğini bir veri önizlemesi aracılığıyla görüntülediğinizde, ADF'nin klasör düzeylerinizin her birinde bulunan çözülmüş bölümleri ekleyeceğini görürsünüz.

![Bölüm kök yolu](media/data-flow/partfile1.png "Bölüm kök yolu önizlemesi")

**Dosya listesi:** Bu bir dosya kümesi. İşlenmesi gereken göreli yol dosyalarının listesini içeren bir metin dosyası oluşturun. Bu metin dosyasını işaret edin.

**Dosya adını depolamak için sütun:** Verilerinizdeki bir sütunda kaynak dosyanın adını depolayın. Dosya adı dizesini depolamak için buraya yeni bir sütun adı girin.

**Tamamlandıktan sonra:** Veri akışı çalıştırdıktan sonra kaynak dosyayla hiçbir şey yapmamayı, kaynak dosyayı silmeyi veya kaynak dosyayı taşımayı seçin. Taşıma yolları görecelidir.

Kaynak dosyalarını işleme sonrası başka bir konuma taşımak için önce dosya işlemi için "Taşı"yı seçin. Sonra, "from" dizinini ayarlayın. Yolunuz için herhangi bir joker karakter kullanmıyorsanız, "from" ayarı kaynak klasörünüzle aynı klasör olacaktır.

Joker karakterli bir kaynak yolunuz varsa, sözdiziminiz aşağıdaki gibi görünür:

```/data/sales/20??/**/*.csv```

"From" olarak belirtebilirsiniz

```/data/sales```

Ve "to" olarak

```/backup/priorSales```

Bu durumda, /data/sales altında nistemin alınan tüm dosyalar /backup/priorSales'e taşınır.

> [!NOTE]
> Dosya işlemleri yalnızca bir ardışık ardışık işlem hattında Veri Akışı etkinliğini kullanan bir ardışık hatlar veri ayıklama veya yürütme çalıştırıcısından (bir veri ayıklama veya yürütme çalıştır) veri akışını başlattığınızda çalışır. Dosya işlemleri Veri Akışı hata ayıklama modunda *çalışmaz.*

**Son değiştirilene göre filtrele:** En son ne zaman değiştirildiklerine ait bir tarih aralığı belirterek hangi dosyaları işlediğinizi filtreleyebilirsiniz. Tüm randevu saatleri UTC'dedir. 

### <a name="sink-properties"></a>Lavabo özellikleri

Lavabo dönüşümünde, Azure Veri Gölü Depolama Gen2'deki bir kapsayıcıya veya klasöre yazabilirsiniz. **Ayarlar** sekmesi, dosyaların nasıl yazıldığını yönetmenize olanak tanır.

![lavabo seçenekleri](media/data-flow/file-sink-settings.png "lavabo seçenekleri")

**Klasörü temizleyin:** Veriler yazılmadan önce hedef klasörün temizlenip temizlenmediğini belirler.

**Dosya adı seçeneği:** Hedef dosyaların hedef klasörde nasıl adlandırıldığını belirler. Dosya adı seçenekleri şunlardır:
   * **Varsayılan**: Spark'ın PART varsayılanlarına göre dosyaları adlandırmasına izin verin.
   * **Desen**: Çıktı dosyalarınızı bölüm başına sayısala getiren bir desen girin. Örneğin, **krediler[n].csv** loans1.csv, loans2.csv, ve benzeri yaratacaktır.
   * **Bölüm Başına**: Bölüm başına bir dosya adı girin.
   * **Sütundaki veriler olarak**: Çıktı dosyasını bir sütunun değerine ayarlayın. Yol, hedef klasöre değil, veri kümesi kapsayıcısına göredir. Veri setinizde bir klasör yolunuz varsa, bu yol geçersiz kılınacaktır.
   * **Çıktı tek bir dosyaya**: Bölümlenmiş çıktı dosyalarını tek bir adlandırılmış dosyada birleştirin. Yol veri kümesi klasörüne göredir. Te birleştirme işleminin düğüm boyutuna bağlı olarak başarısız olabileceğini lütfen unutmayın. Bu seçenek büyük veri kümeleri için önerilmez.

**Tüm alıntı:** Tüm değerlerin tırnak içine alıp almayacağını belirler

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="getmetadata-activity-properties"></a>Metaveri etkinlik özelliklerini alın

Özellikler hakkında daha fazla bilgi edinmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) kontrol edin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini silme

Özellikler hakkında ayrıntılı bilgi edinmek için [Sil etkinliğini denetleyin](delete-activity.md)

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Aşağıdaki modeller hala geriye dönük uyumluluk için olduğu gibi desteklenir. İleriye dönük yukarıdaki bölümlerde belirtilen yeni modeli kullanmanız önerilir ve ADF yazarlı UI yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski dataset modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **AzureBlobFSFile**olarak ayarlanmalıdır. |Evet |
| folderPath | Veri Gölü Depolama Gen2'deki klasöre giden yol. Belirtilmemişse, köke işaret eden. <br/><br/>Joker karakter filtresi desteklenir. İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın joker kartı varsa veya bu kaçış char'ı içindeyse kaçmak için kullanın. <br/><br/>Örnekler: filesystem/klasör/. Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosyalar için ad veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için, izin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı bir joker veya bu kaçış char içinde varsa kaçmak için kullanın.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, kopyalama etkinliği otomatik olarak aşağıdaki desenle dosya adını oluşturur: "*Data.[ etkinlik çalıştırın ID GUID]. [GUID if FlattenHiyerarşiy]. [yapılandırılırsa biçimlendirme]. [yapılandırılırsa sıkıştırma]*", örneğin, "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Sorgu yerine tablo adı kullanarak bir tablo kaynağından kopyalarsanız, ad deseni "*[tablo adı].[ biçimlendirin]. [yapılandırılırsa sıkıştırma]*", örneğin, "MyTable.csv". |Hayır |
| modifiyeDatetimeBaşlat | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. **Biçim** altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [JSON biçimi,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro formatı,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [ORC formatı](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke formatı](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlerine bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri **GZip,** **Deflate**, **BZip2**, ve **ZipDeflate**vardır.<br/>Desteklenen seviyeler **Optimal** ve **En Hızlı'dır.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath'i** klasör bölümüyle ve dosya adı içeren **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath'i** klasör bölümüyle ve joker karakter filtresiyle **fileName'yi** belirtin. 

**Örnek:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği **AzureBlobFSSource**olarak ayarlanmalıdır. |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olduğunda ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasör veya alt klasör kopyalanmaz veya lavaboda oluşturulmaz.<br/>İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Eski kopyalama etkinliği lavabo modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **AzureBlobFSSink**olarak ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
