---
title: Azure Veri Gölü Depolama Gen1'e veya bu verilerden kopyala
description: Desteklenen kaynak veri mağazalarından Azure Veri Gölü Deposu'na veya Data Lake Store'dan desteklenen lavabo depolarına veri fabrikasını kullanarak nasıl kopyalayılacak yapılacağını öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/12/2020
ms.openlocfilehash: 61bb8fe950de8cd9be91bc76bd24aa0151f3fb79
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415408"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri Azure Veri Depolama Gen1'e veya Azure Veri Gölü Depolama Gen1'den kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Veri Fabrikası sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-datalake-connector.md)
> * [Geçerli sürüm](connector-azure-data-lake-store.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Veri Gölü Depolama Gen1'e ve bu verilerden nasıl kopyalanır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Veri Gölü Depolama Gen1 bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama 
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Özellikle, bu bağlayıcı ile şunları yapabilirsiniz:

- Dosyaları aşağıdaki kimlik doğrulama yöntemlerinden birini kullanarak kopyalayın: Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler.
- Dosyaları olduğu gibi kopyalayın veya desteklenen [dosya biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırın veya oluşturun.
- Azure Veri Gölü Depolama Gen2'ye kopyalanırken [ACD'leri koruyun.](#preserve-acls-to-data-lake-storage-gen2)

> [!IMPORTANT]
> Kendi kendine barındırılan tümleştirme çalışma zamanını kullanarak verileri kopyalarsanız, şirket `<ADLS account name>.azuredatalakestore.net` güvenlik `login.microsoftonline.com/<tenant>/oauth2/token` duvarını giden trafiğin bağlantı noktasına ve 443 bağlantı noktasına girmesine izin verecek şekilde yapılandırın. İkincisi, erişim belirteci almak için tümleştirme çalışma zamanının iletişim kurması gereken Azure Güvenlik Belirteci Hizmetidir.

## <a name="get-started"></a>başlarken

> [!TIP]
> Azure Veri Gölü Deposu bağlayıcısının nasıl kullanılacağına bir göz atmak için, [verileri Azure Veri Gölü Deposu'na Yükle'ye](load-azure-data-lake-store.md)bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Azure Veri Gölü Deposu'na özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Veri Gölü Deposu bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Özellik `type` **AzureDataLakeStore**olarak ayarlanmalıdır. | Evet |
| dataLakeStoreUri | Azure Veri Gölü Deposu hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini `https://[accountname].azuredatalakestore.net/webhdfs/v1` `adl://[accountname].azuredatalakestore.net/`alır: veya . | Evet |
| subscriptionId | Data Lake Store hesabının ait olduğu Azure abonelik kimliği. | Lavabo için gerekli |
| resourceGroupName | Veri Gölü Deposu hesabının ait olduğu Azure kaynak grubu adı. | Lavabo için gerekli |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Veri mağazanız özel bir ağda bulunuyorsa Azure tümleştirme çalışma zamanını veya kendi kendine barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Bu özellik belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

### <a name="use-service-principal-authentication"></a>Hizmet temel kimlik doğrulamasını kullanma

Hizmet temel kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. Azure Active Directory'ye bir uygulama varlığı kaydedin ve Veri Gölü Deposu'na erişim izni verin. Ayrıntılı adımlar için [hizmete kimlik doğrulamasına](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet müdürüne uygun izin ver. Azure Veri Gölü Depolama Gen1'de Access denetiminden Veri Gölü Depolama [Gen1'de](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions)izin nasıl çalıştığına ilişkin örneklere bakın.

    - **Kaynak Olarak**: **Veri gezgini** > **Access'te,** dosyaların kopyalanması için **Okuma** izniyle birlikte, kök de dahil olmak üzere tüm yukarı klasörler için en azından **Yürütme** izni verin. Bu klasöre ve özyinelemeli **tüm alt katmanlara** eklemeyi ve erişim izni ve varsayılan izin **girişi**olarak eklemeyi seçebilirsiniz. Hesap düzeyi erişim denetimi (IAM) için bir gereklilik yoktur.
    - **Lavabo olarak**: **Veri gezgini** > **Access'** te, en **azından** kök dahil tüm upstream klasörleri için, lavabo klasörü için **yazma** izniyle birlikte İzin verin. Bu klasöre ve özyinelemeli **tüm alt katmanlara** eklemeyi ve erişim izni ve varsayılan izin **girişi**olarak eklemeyi seçebilirsiniz. IAM'de kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de lavabo bulutta), Veri Fabrikası'nın Veri Gölü Deposu'nun bölgesini algılamasına izin vermek için en azından **Reader** rolünü verin. Bu IAM rolünden kaçınmak istiyorsanız, Data Lake Store'un konumuyla birlikte açıkça [bir Azure tümleştirme çalışma zamanı oluşturun.](create-azure-integration-runtime.md#create-azure-ir) Örneğin, Veri Gölü Mağazanız Batı Avrupa'daysa, konum "Batı Avrupa" olarak ayarlanmış bir Azure tümleştirme çalışma zamanı oluşturun. Aşağıdaki örnekte gösterildiği gibi, bunları Data Lake Store bağlantılı hizmette ilişkilendirin.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı Veri `SecureString` Fabrikası'nda güvenli bir şekilde depolamak için işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| Kiracı | Uygulamanızın bulunduğu alan adı veya kiracı kimliği gibi kiracı bilgilerini belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet |

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

### <a name="use-managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanma

Bir veri fabrikası, bu özel veri fabrikasını temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md)ilişkilendirilebilir. Bu yönetilen kimliği, kendi hizmet yöneticinizi kullanmaya benzer şekilde Veri Gölü Deposu kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu, belirlenmiş fabrikanın Data Lake Store'a veya Data Lake Store'dan gelen verilere erişmesine ve bunları kopyalamasına olanak tanır.

Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanmak için aşağıdaki adımları izleyin.

1. Fabrikanızla birlikte oluşturulan "Hizmet Kimliği Uygulama Kimliği"nin değerini kopyalayarak [veri fabrikası tarafından yönetilen kimlik bilgilerini alın.](data-factory-service-identity.md#retrieve-managed-identity)

2. Yönetilen kimlik erişimini Data Lake Store'a verin. Azure Veri Gölü Depolama Gen1'de Access denetiminden Veri Gölü Depolama [Gen1'de](../data-lake-store/data-lake-store-access-control.md#common-scenarios-related-to-permissions)izin nasıl çalıştığına ilişkin örneklere bakın.

    - **Kaynak Olarak**: **Veri gezgini** > **Access'te,** dosyaların kopyalanması için **Okuma** izniyle birlikte, kök de dahil olmak üzere tüm yukarı klasörler için en azından **Yürütme** izni verin. Bu klasöre ve özyinelemeli **tüm alt katmanlara** eklemeyi ve erişim izni ve varsayılan izin **girişi**olarak eklemeyi seçebilirsiniz. Hesap düzeyi erişim denetimi (IAM) için bir gereklilik yoktur.
    - **Lavabo olarak**: **Veri gezgini** > **Access'** te, en **azından** kök dahil tüm upstream klasörleri için, lavabo klasörü için **yazma** izniyle birlikte İzin verin. Bu klasöre ve özyinelemeli **tüm alt katmanlara** eklemeyi ve erişim izni ve varsayılan izin **girişi**olarak eklemeyi seçebilirsiniz. IAM'de kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de lavabo bulutta), Veri Fabrikası'nın Veri Gölü Deposu'nun bölgesini algılamasına izin vermek için en azından **Reader** rolünü verin. Bu IAM rolünden kaçınmak istiyorsanız, Data Lake Store'un konumuyla birlikte açıkça [bir Azure tümleştirme çalışma zamanı oluşturun.](create-azure-integration-runtime.md#create-azure-ir) Aşağıdaki örnekte gösterildiği gibi, bunları Data Lake Store bağlantılı hizmette ilişkilendirin.

Azure Veri Fabrikası'nda, bağlantılı hizmetteki genel Data Lake Store bilgilerinin dışında herhangi bir özellik belirtmeniz gerekmez.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarlar altında Azure Data Lake Store Gen1 için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri `location` kümesindeki tür özelliği **AzureDataLakeStoreLocation**olarak ayarlanmalıdır. | Evet      |
| folderPath | Bir klasöre giden yol. Klasörlere filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarında belirtin. | Hayır       |
| fileName   | Verilen folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarında belirtin. | Hayır       |

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) Bu bölümde, Azure Veri Gölü Deposu kaynağı ve lavabosu tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="azure-data-lake-store-as-source"></a>Kaynak Olarak Azure Veri Gölü Deposu

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopya kaynağındaki `storeSettings` ayarlar altında Azure Data Lake Store Gen1 için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureDataLakeStoreReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olduğunda ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasör veya alt klasör kopyalanmaz veya lavaboda oluşturulmaz. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| joker KarakterFolderPath       | Kaynak klasörleri filtrelemek için joker karakteriçeren klasör yolu. <br>İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızı n içinizde joker karakter veya bu kaçış char varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen folderPath/jokerKarakterIn altında joker karakter içeren dosya adı. <br>İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızı n içinizde joker karakter veya bu kaçış char varsa kaçmak için kullanın. Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

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
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureDataLakeStoreReadSettings",
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

### <a name="azure-data-lake-store-as-sink"></a>Lavabo olarak Azure Veri Gölü Deposu

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama lavabosundaki `storeSettings` ayarlar altında Azure Data Lake Store Gen1 için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureDataLakeStoreWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır       |
| son kullanmaTarihi Zaman | Yazılı dosyaların son kullanma süresini belirtir. Zaman UTC saatine "2020-03-01T08:00:00Z" biçiminde uygulanır. Varsayılan olarak NULL'dur, bu da yazılı dosyaların süresinin dolmaması anlamına gelir. | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

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
                    "type": "AzureDataLakeStoreWriteSettings",
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

### <a name="examples-of-behavior-of-the-copy-operation"></a>Kopyalama işleminin davranış örnekleri

Bu bölümde, farklı birleşimler ve `recursive` `copyBehavior` değerler için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Elde çıkan hedef |
|:--- |:--- |:--- |:--- |
| true |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 kaynakla aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5. |
| true |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulmuş ad |
| true |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 içerikleri otomatik olarak oluşturulmuş bir dosya adı ile tek bir dosyada birleştirilir. |
| yanlış |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulan dosya adı içeren bir dosyada birleştirilir. File1 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>APARA'ları Veri Gölü Depolama Gen2'ye Koruyun

>[!TIP]
>Azure Veri Gölü Depolama Gen1'deki verileri genel olarak Gen2'ye kopyalamak için, gezinme ve en iyi uygulamalar için [Azure Veri Fabrikası ile Azure Veri Gölü Depolama Gen1'den Gen2'ye verileri kopyala'ya](load-azure-data-lake-storage-gen2-from-gen1.md) bakın.

Veri Gölü Depolama Gen1'den Data Lake Storage Gen2'ye yükselttidiğinizde erişim denetim listelerini (ACD'ler) veri dosyalarıyla birlikte çoğaltmak istiyorsanız, [bkz.](copy-activity-preserve-metadata.md#preserve-acls)

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede verileri dönüştürürken, JSON, Avro, Sınırlı Metin veya Parke biçiminde Azure Veri Gölü Depolama Gen1'den dosyaları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için, eşleme veri akışı özelliğinde [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Kaynak dönüşümünde, Azure Veri Gölü Depolama Gen1'deki bir kapsayıcı, klasör veya tek tek dosyadan okuyabilirsiniz. **Kaynak seçenekleri** sekmesi, dosyaların nasıl okunduğunu yönetmenize olanak tanır. 

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

Lavabo dönüşümünde, Azure Veri Gölü Depolama Gen1'deki bir kapsayıcıya veya klasöre yazabilirsiniz. **Ayarlar** sekmesi, dosyaların nasıl yazıldığını yönetmenize olanak tanır.

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
| type | Veri kümesinin tür özelliği **AzureDataLakeStoreFile**olarak ayarlanmalıdır. |Evet |
| folderPath | Data Lake Store'daki klasöre giden yol. Belirtilmemişse, köke işaret eden. <br/><br/>Joker karakter filtresi desteklenir. İzin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızı n içinizde joker karakter veya bu kaçış char varsa kaçmak için kullanın. <br/><br/>Örneğin: rootfolder/subfolder/. Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosyalar için ad veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları işaret edir. <br/><br/>Filtre için, izin verilen `*` joker karakterler (sıfır veya `?` daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı bir joker veya bu kaçış char içinde varsa kaçmak için kullanın.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, kopyalama etkinliği otomatik olarak aşağıdaki desenle dosya adını oluşturur: "*Data.[ etkinlik çalıştırın ID GUID]. [GUID if FlattenHiyerarşiy]. [yapılandırılırsa biçimlendirme]. [yapılandırılırsa sıkıştırma]*", örneğin, "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". Sorgu yerine tablo adı kullanarak bir tablo kaynağından kopyalarsanız, ad deseni "*[tablo adı].[ biçimlendirin]. [yapılandırılırsa sıkıştırma]*", örneğin, "MyTable.csv". |Hayır |
| modifiyeDatetimeBaşlat | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Son Değiştirilen özniteliğe göre dosyalar filtreuygulayın. Dosyalar, son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığındaysa seçilir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanmadığı anlamına gelir. Bir `modifiedDatetimeStart` datetime değeri `modifiedDatetimeEnd` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçildiği anlamına gelir. Bir `modifiedDatetimeEnd` datetime değeri `modifiedDatetimeStart` ne zaman, ancak NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçildiği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. **Biçim** altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [JSON biçimi,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro formatı,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork formatı](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke formatında](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlere bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri **GZip,** **Deflate**, **BZip2**, ve **ZipDeflate**vardır.<br/>Desteklenen seviyeler **Optimal** ve **En Hızlı'dır.** |Hayır |

>[!TIP]
>Tüm dosyaları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için **folderPath'i** klasör bölümüyle ve dosya adı içeren **dosya adı** ile belirtin.<br>Bir klasörün altındaki dosyaların alt kümesini kopyalamak için **folderPath'i** klasör bölümüyle ve joker karakter filtresiyle **fileName'yi** belirtin. 

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

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama `type` etkinlik kaynağının özelliği **AzureDataLakeStoreSource**olarak ayarlanmalıdır. |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Doğru `recursive` ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasör veya alt klasör kopyalanmaz veya lavaboda oluşturulmaz. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

### <a name="legacy-copy-activity-sink-model"></a>Eski kopyalama etkinliği lavabo modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama `type` etkinliği lavabonun özelliği **AzureDataLakeStoreSink**olarak ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, dosya adı otomatik olarak oluşturulur. | Hayır |
| maxConcurrentConnections | Veri deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
