---
title: Azure Data Lake Storage 1. veri kopyalama
description: Data Factory kullanarak desteklenen kaynak veri depolarından verileri Azure Data Lake Store veya Data Lake Store desteklenen havuz depolarına kopyalamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/12/2019
ms.openlocfilehash: 701695c849a7f94abdba83f962806ecab3f21282
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75440866"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake Storage 1. veri kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-datalake-connector.md)
> * [Geçerli sürüm](connector-azure-data-lake-store.md)

Bu makalede, Azure Data Lake Storage 1. verileri içine ve öğesinden kopyalama özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake Storage 1. Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md) 
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu bağlayıcıyla şunları yapabilirsiniz:

- Aşağıdaki kimlik doğrulama yöntemlerinden birini kullanarak dosyaları kopyalayın: Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler.
- Dosyaları olduğu gibi kopyalayın veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırır veya oluşturun.
- Azure Data Lake Storage 2. içine kopyalarken [ACL 'Leri koruyun](#preserve-acls-to-data-lake-storage-gen2) .

> [!IMPORTANT]
> Şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak verileri kopyalarsanız, 443 numaralı bağlantı noktasında giden trafiğe `<ADLS account name>.azuredatalakestore.net` ve `login.microsoftonline.com/<tenant>/oauth2/token` izin verecek şekilde kurumsal güvenlik duvarını yapılandırın. İkincisi, tümleştirme çalışma zamanının erişim belirtecini almak için iletişim kurması gereken Azure Güvenlik belirteci hizmetidir.

## <a name="get-started"></a>Kullanmaya Başlayın

> [!TIP]
> Azure Data Lake Store bağlayıcısını nasıl kullanacağınızı öğrenmek için bkz. [Azure Data Lake Store verileri yükleme](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Azure Data Lake Store özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında bilgiler sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake Store bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | `type` özelliği **AzureDataLakeStore**olarak ayarlanmalıdır. | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store hesabının ait olduğu Azure abonelik KIMLIĞI. | Havuz için gerekli |
| resourceGroupName | Data Lake Store hesabının ait olduğu Azure Kaynak grubu adı. | Havuz için gerekli |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri depolubir özel ağda yer alıyorsa Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

### <a name="use-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını kullanma

Hizmet sorumlusu kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. Azure Active Directory bir uygulama varlığını kaydedin ve Data Lake Store erişim izni verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun iznini verin. [Azure Data Lake Storage 1. erişim denetiminden](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions)Data Lake Storage 1. iznin nasıl çalıştığına ilişkin örneklere bakın.

    - **Kaynak olarak**: **veri gezgini** 'nde **ERIŞIM** > , kök dahil tüm yukarı akış klasörleri için, kopyalanacak dosyalar Için **okuma** izniyle birlikte en az **yürütme** izni verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Hesap düzeyinde erişim denetimi (ıAM) için gereksinim yoktur.
    - **Havuz olarak**: **veri gezgini** 'nde **ERIŞIM** > , kök dahil tüm yukarı akış klasörleri için havuz klasörü Için **yazma** izniyle birlikte en az **yürütme** izni verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de havuz bulutta ise), ıAM 'de Data Lake Store bölge Data Factory algılamasını sağlamak için en az **okuyucu** rolü verin. Bu ıAM rolünü önlemek istiyorsanız, açıkça Data Lake Store konumuyla [bir Azure tümleştirme çalışma zamanı oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Örneğin, Data Lake Store Batı Avrupa ise, konum "Batı Avrupa" olarak ayarlanmış bir Azure tümleştirme çalışma zamanı oluşturun. Bunları, aşağıdaki örnekte gösterildiği gibi Data Lake Store bağlantılı hizmette ilişkilendirin.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı bir `SecureString` olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet |
| tenant | Uygulamanızın bulunduğu etki alanı adı veya kiracı KIMLIĞI gibi kiracı bilgilerini belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler kullanma

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu yönetilen kimliği, kendi hizmet sorumlunuzu kullanmaya benzer şekilde Data Lake Store kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu belirlenen fabrikasının Data Lake Store erişimine ve veri kopyalamasına izin verir.

Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanmak için aşağıdaki adımları izleyin.

1. Fabrikalarınızla birlikte üretilen "hizmet kimliği uygulama KIMLIĞI" değerini kopyalayarak [Veri Fabrikası tarafından yönetilen kimlik bilgilerini alın](data-factory-service-identity.md#retrieve-managed-identity) .

2. Data Lake Store yönetilen kimliğe erişim izni verin. [Azure Data Lake Storage 1. erişim denetiminden](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions)Data Lake Storage 1. iznin nasıl çalıştığına ilişkin örneklere bakın.

    - **Kaynak olarak**: **veri gezgini** 'nde **ERIŞIM** > , kök dahil tüm yukarı akış klasörleri için, kopyalanacak dosyalar Için **okuma** izniyle birlikte en az **yürütme** izni verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Hesap düzeyinde erişim denetimi (ıAM) için gereksinim yoktur.
    - **Havuz olarak**: **veri gezgini** 'nde **ERIŞIM** > , kök dahil tüm yukarı akış klasörleri için havuz klasörü Için **yazma** izniyle birlikte en az **yürütme** izni verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de havuz bulutta ise), ıAM 'de Data Lake Store bölge Data Factory algılamasını sağlamak için en az **okuyucu** rolü verin. Bu ıAM rolünü önlemek istiyorsanız, açıkça Data Lake Store konumuyla [bir Azure tümleştirme çalışma zamanı oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Bunları, aşağıdaki örnekte gösterildiği gibi Data Lake Store bağlantılı hizmette ilişkilendirin.

Azure Data Factory, bağlantılı hizmette genel Data Lake Store bilgilerinin yanı sıra herhangi bir özellik belirtmeniz gerekmez.

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
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

Biçim tabanlı veri kümesindeki `location` ayarları altında Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik   | Açıklama                                                  | Gereklidir |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki `location` seçeneğinin altında bulunan tür özelliği **AzureDataLakeStoreLocation**olarak ayarlanmalıdır. | Evet      |
| folderPath | Bir klasörün yolu. Klasörleri filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |
| fileName   | Verilen folderPath altındaki dosya adı. Dosyaları filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen1 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureDataLakeStoreLocation",
                "folderPath": "root/folder/subfolder"
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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). Bu bölüm Azure Data Lake Store kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-store-as-source"></a>Kaynak olarak Azure Data Lake Store

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Biçim tabanlı kopyalama kaynağında `storeSettings` ayarları altında Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | `storeSettings` altındaki tür özelliği **AzureDataLakeStoreReadSetting**olarak ayarlanmalıdır. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| Yavaya Cardfolderpath       | Kaynak klasörlerin filtreleneceği joker karakter içeren klasör yolu. <br>İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). Asıl klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). Asıl klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Veri kümesinde `fileName` belirtilmemişse Evet |
| modifiedDatetimeStart    | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı `modifiedDatetimeStart` ve `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. `modifiedDatetimeStart` bir tarih saat değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyalar seçilir. `modifiedDatetimeEnd` bir tarih saat değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                            |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
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
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureDataLakeStoreReadSetting",
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

### <a name="azure-data-lake-store-as-sink"></a>Havuz olarak Azure Data Lake Store

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Biçim tabanlı kopya havuzunda `storeSettings` ayarları altında Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | `storeSettings` altındaki tür özelliği **AzureDataLakeStoreWriteSetting**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
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
                    "type": "AzureDataLakeStoreWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| folderPath | fileName | recursive | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Boş, Varsayılanı kullan) | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | (Boş, Varsayılanı kullan) | doğru | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | `*.csv` | yanlış | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | `*.csv` | doğru | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |

### <a name="examples-of-behavior-of-the-copy-operation"></a>Kopyalama işleminin davranış örnekleri

Bu bölümde, `recursive` ve `copyBehavior` değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| doğru |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| doğru |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| doğru |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 + dosya2 + File3 + File4 + File5 içerikleri, otomatik olarak oluşturulan bir dosya adı ile tek bir dosyada birleştirilir. |
| yanlış |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |
| yanlış |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |
| yanlış |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adıyla tek bir dosyada birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>ACL 'Leri Data Lake Storage 2. koruma

>[!TIP]
>Azure Data Lake Storage 1. verileri genel olarak Gen2 'e kopyalamak için bkz. bir adım adım ve en iyi uygulamalar için [Azure Data Factory ile Azure Data Lake Storage 1. verileri Gen2 'e kopyalama](load-azure-data-lake-storage-gen2-from-gen1.md) .

Data Lake Storage 1. 'den Data Lake Storage 2. sürümüne yükselttiğinizde erişim denetim listelerini (ACL 'Ler) ve veri dosyalarıyla çoğaltmak istiyorsanız bkz. [Data Lake Storage 1. ACL 'Leri koruma](copy-activity-preserve-metadata.md#preserve-acls).

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki verileri dönüştürürken JSON, avro, sınırlandırılmış metin veya Parquet biçimindeki Azure Data Lake Storage 1. dosyaları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için bkz. veri akışı eşleme özelliğinde [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüştürme](data-flow-sink.md) .

### <a name="source-transformation"></a>Kaynak dönüştürme

Kaynak dönüşümünde, Azure Data Lake Storage 1. bir kapsayıcı, klasör veya tek dosyadan okuyabilirsiniz. **Kaynak seçenekleri** sekmesi, dosyaların nasıl okunduğunu yönetmenizi sağlar. 

![Kaynak seçenekleri](media/data-flow/sourceOptions1.png "Kaynak seçenekleri")

**Joker karakter yolu:** Bir joker karakter deseninin kullanılması, ADF 'nin eşleşen her klasör ve dosyada tek bir kaynak dönüşümünde döngüye girmesine neden olur. Bu, tek bir akış içinde birden çok dosyayı işlemek için etkili bir yoldur. Mevcut joker karakter deseniniz üzerine getirildiğinde görüntülenen + işaretiyle birden çok joker karakter eşleştirme deseni ekleyin.

Kaynak kapsayıcısından bir düzeniyle eşleşen bir dosya serisi seçin. Yalnızca kapsayıcı, veri kümesinde belirtilebilir. Bu nedenle, joker karakter yolunuzda Ayrıca, kök klasörden klasör yolunuzdan de yer verilmelidir.

Joker karakter örnekleri:

* ```*``` herhangi bir karakter kümesini temsil eder
* ```**``` özyinelemeli dizin iç içe geçirmeyi temsil eder
* ```?``` bir karakter değiştirir
* ```[]``` parantez içindeki daha fazla karakterden biriyle eşleşiyor

* ```/data/sales/**/*.csv```/Data/Sales altındaki tüm CSV dosyalarını alır
* ```/data/sales/20??/**``` 20. yüzyıl içindeki tüm dosyaları alır
* ```/data/sales/2004/*/12/[XY]1?.csv```, iki basamaklı bir sayı tarafından önekli X veya Y ile başlayan Aralık içinde 2004 içindeki tüm CSV dosyalarını alır

**Bölüm kök yolu:** Dosya kaynağınızda bir ```key=value``` biçimiyle bölümlenmiş klasörler varsa (örneğin, Year = 2019), bu bölüm klasör ağacının en üst düzeyini veri akışı veri akışınızdaki bir sütun adına atayabilirsiniz.

İlk olarak, bölümlenmiş klasörler ve okumak istediğiniz yaprak dosyaları olan tüm yolları içerecek şekilde bir joker karakter ayarlayın.

![Bölüm kaynak dosyası ayarları](media/data-flow/partfile2.png "Bölüm dosyası ayarı")

Klasör yapısının en üst düzeyinin ne olduğunu tanımlamak için bölüm kök yolu ayarını kullanın. Veri önizleme aracılığıyla verilerinizin içeriğini görüntülediğinizde, ADF 'nin klasör düzeylerinizde bulunan çözümlenmiş bölümleri eklemesini görürsünüz.

![Bölüm kök yolu](media/data-flow/partfile1.png "Bölüm kök yolu önizlemesi")

**Dosya listesi:** Bu bir dosya kümesidir. İşlemek için göreli yol dosyalarının bir listesini içeren bir metin dosyası oluşturun. Bu metin dosyasına işaret edin.

**Depolanacak sütun dosya adı:** Kaynak dosyanın adını verilerinizin bir sütununda depolayın. Dosya adı dizesini depolamak için buraya yeni bir sütun adı girin.

**Tamamlandıktan sonra:** Veri akışı çalıştıktan sonra kaynak dosyayla ilgili hiçbir şey yapma seçeneğini belirleyin, kaynak dosyayı silin veya kaynak dosyayı taşıyın. Taşımanın yolları görelidir.

Kaynak dosyalarını başka bir konuma işleme sonrası taşımak için, önce dosya işlemi için "taşı" yı seçin. Sonra, "Kimden" dizinini ayarlayın. Yolunuzda herhangi bir joker karakter kullanmıyorsanız, "Kimden" ayarı kaynak klasörünüzün bulunduğu klasör olacaktır.

Joker karakter içeren bir kaynak yolunuz varsa söz dizinizin aşağıdaki gibi görünmesi gerekir:

```/data/sales/20??/**/*.csv```

"Kimden" olarak belirtebilirsiniz

```/data/sales```

Ve "to" as

```/backup/priorSales```

Bu durumda,/Data/Sales altında kaynağı bulunan tüm dosyalar/Backup/priorsales' a taşınır.

> [!NOTE]
> Dosya işlemleri, bir işlem hattındaki veri akışını Yürüt etkinliğini kullanan bir işlem hattı çalıştırmasıyla (bir işlem hattı hata ayıklaması veya yürütme çalıştırması) veri akışını başlattığınızda çalışır. Dosya işlemleri veri akışı hata ayıklama *modunda çalışmaz.*

**Son değiştirme ölçütü:** Son değiştirilme tarihi için bir tarih aralığı belirterek hangi dosyaları işleyeistediğinizi filtreleyebilirsiniz. Tüm tarih zamanları UTC olarak. 

### <a name="sink-properties"></a>Havuz özellikleri

Havuz dönüşümünde, Azure Data Lake Storage 1. bir kapsayıcı veya klasöre yazabilirsiniz. **Ayarlar** sekmesi, dosyaların nasıl yazıldığını yönetmenizi sağlar.

![havuz seçenekleri](media/data-flow/file-sink-settings.png "havuz seçenekleri")

**Klasörü temizle:** Hedef klasörün, veriler yazılmadan önce temizlenip temizlenmeyeceğini belirler.

**Dosya adı seçeneği:** Hedef klasördeki hedef dosyaların nasıl adlandırıldığını belirler. Dosya adı seçenekleri şunlardır:
   * **Varsayılan**: Spark 'ın bölüm varsayılanlarına göre dosya adına erişmesine izin verin.
   * **Model**: bölüm başına çıktı dosyalarınızı numaralandırır bir model girin. Örneğin, **[n]. csv** , loans1. csv, loans2. csv, vb. oluşturacaktır.
   * **Bölüm başına**: bölüm başına bir dosya adı girin.
   * **Sütunda veri olarak**: çıkış dosyasını bir sütunun değeri olarak ayarlayın. Yol, hedef klasöre değil, veri kümesi kapsayıcısına göredir.
   * **Tek bir dosyaya çıkış**: bölümlenmiş çıktı dosyalarını tek bir adlandırılmış dosyada birleştirin. Yol, veri kümesi klasörüne görelidir. Lütfen birleştirme işleminin düğüm boyutuna bağlı olarak başarısız olabileceğini unutmayın. Bu seçenek, büyük veri kümeleri için önerilmez.

**Tüm teklif:** Tüm değerlerin tırnak içine alınmış olup olmayacağını belirler

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md) onay

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Geriye dönük uyumluluk için aşağıdaki modeller hala desteklenmektedir. Yukarıdaki bölümlerde bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **AzureDataLakeStoreFile**olarak ayarlanmalıdır. |Evet |
| folderPath | Data Lake Store klasörünün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir. İzin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). Asıl klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. <br/><br/>Örneğin: RootFolder/alt klasör/. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosyalar için ad veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için, izin verilen joker karakterler `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Gerçek dosya adınızın bir joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın.<br/><br/>Bir çıkış veri kümesi için dosya adı belirtilmediğinde ve bir etkinlik havuzunda **Preservehierarchy** belirtilmemişse, Copy etkinliği dosya adını aşağıdaki Düzenle otomatik olarak oluşturur: "*Data. [ Etkinlik çalıştırma KIMLIĞI GUID 'SI]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]* "Örneğin," Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt. gz ". Sorgu yerine tablo adı kullanarak tablosal kaynaktan kopyalama yaparsanız, ad deseninin " *[tablo adı]" olması gerekir. [ Biçim]. [yapılandırıldıysa sıkıştırma]* "Örneğin," MyTable. csv ". |Hayır |
| modifiedDatetimeStart | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı `modifiedDatetimeStart` ve `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. `modifiedDatetimeStart` bir tarih saat değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyalar seçilir. `modifiedDatetimeEnd` bir tarih saat değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı `modifiedDatetimeStart` ve `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. `modifiedDatetimeStart` bir tarih saat değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyalar seçilir. `modifiedDatetimeEnd` bir tarih saat değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| biçim | Dosyaları dosya tabanlı depolarla (ikili kopya) olduğu gibi kopyalamak istiyorsanız hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Ayrıştırma veya belirli bir biçime sahip dosyaları oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParquetFormat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için, klasör **bölümü ve dosya** adı ile bir dosya adı ile **FolderPath** belirtin.<br>Bir klasör altındaki bir dosya alt kümesini kopyalamak için, bir klasör bölümü ve **dosya adı** ile bir joker karakter filtresi içeren **FolderPath** öğesini belirtin. 

**Örnek:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
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

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının `type` özelliği **AzureDataLakeStoreSource**olarak ayarlanmalıdır. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. `recursive` true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen1 input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Eski kopyalama etkinliği havuz modeli

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun `type` özelliği **AzureDataLakeStoreSink**olarak ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-(Varsayılan) PreserveHierarchy</b>: hedef klasördeki ise dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-FlattenHierarchy</b>: kaynak klasördeki tüm dosyaları ilk hedef klasörün içinde düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-MergeFiles</b>: tüm dosyaları kaynak klasörden bir dosya birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, dosya adı otomatik olarak oluşturulur. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen1 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
