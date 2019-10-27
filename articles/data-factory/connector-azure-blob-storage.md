---
title: Data Factory kullanarak Azure Blob depolama alanına veya buradan veri kopyalama | Microsoft Docs
description: Data Factory kullanarak, desteklenen kaynak veri depolarından verileri Azure Blob depolama alanına veya blob depolamadan desteklenen havuz veri depolarına nasıl kopyalayacağınızı öğrenin.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: a68549622972bfa031bc2934473dc65f0a656231
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935707"
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob depolama alanına veya veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-blob-connector.md)
> * [Geçerli sürüm](connector-azure-blob-storage.md)

Bu makalede, Azure Blob depolama alanına ve bu kaynaktan veri kopyalama konusu özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Blob Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Özellikle, bu blob Storage Bağlayıcısı şunları destekler:

- Blob 'ları genel amaçlı Azure depolama hesaplarına ve bu kaynaklardan kopyalama, sık/seyrek erişimli BLOB depolama. 
- Azure kaynakları kimlik doğrulamaları için hesap anahtarı, hizmet paylaşılan erişim imzası, hizmet sorumlusu veya yönetilen kimlikler kullanarak blobları kopyalama.
- Blob 'ları blok, ekleme veya sayfa Bloblarından kopyalama ve yalnızca blok bloblarına veri kopyalama.
- Blob 'ları, [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile olduğu gibi kopyalama veya ayrıştırma veya oluşturma.

>[!IMPORTANT]
>Azure Storage güvenlik duvarı ayarlarında **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver** seçeneğini etkinleştirip blob depolamaya bağlanmak için Azure tümleştirme çalışma zamanı 'nı kullanmak istiyorsanız, [yönetilen kimlik kimlik doğrulamasını](#managed-identity)kullanmanız gerekir.

## <a name="get-started"></a>Kullanmaya Başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, blob depolamaya özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Azure Blob Bağlayıcısı aşağıdaki kimlik doğrulama türlerini destekler, Ayrıntılar bölümünde ilgili bölüme bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Paylaşılan erişim imzası kimlik doğrulaması](#shared-access-signature-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler](#managed-identity)

>[!NOTE]
>SQL veri ambarı 'na veri yüklemek için PolyBase kullanılırken, kaynak veya hazırlama blobu depolama alanı sanal ağ uç noktasıyla yapılandırılmışsa, PolyBase için gereken yönetilen kimlik kimlik doğrulamasını kullanmanız ve şirket içinde barındırılan Integration Runtime sürümle kullanmanız gerekir 3,18 veya üzeri. Daha fazla yapılandırma önkoşulları ile [yönetilen kimlik doğrulama](#managed-identity) bölümüne bakın.

>[!NOTE]
>Hdınsights ve Azure Machine Learning etkinlikleri yalnızca Azure Blob depolama hesabı anahtarı kimlik doğrulamasını destekler.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtarı kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureBlobStorage** (önerilen) veya **azurestorage** olarak ayarlanmalıdır (aşağıdaki notlara bakın). |Yes |
| Dizisi | ConnectionString özelliği için depolamaya bağlanmak için gereken bilgileri belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca hesap anahtarını Azure Key Vault yerleştirebilir ve `accountKey` yapılandırmasını bağlantı dizesinden dışarı çekebilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . |Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel bir ağda ise) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Hesap anahtarı kimlik doğrulaması kullanılırken ikincil blob hizmeti uç noktası desteklenmez. Diğer kimlik doğrulama türlerini kullanabilirsiniz.

>[!NOTE]
>"AzureStorage" türünde bağlı hizmet kullanıyorsanız, bu yeni "AzureBlobStorage" bağlı hizmet türünü ileride kullanmanız önerilirken olduğu gibi hala desteklenmektedir.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault hesap anahtarını depolayın**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;"
            },
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }            
    }
}
```

### <a name="shared-access-signature-authentication"></a>Paylaşılan erişim imzası kimlik doğrulaması

Paylaşılan erişim imzası, Depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Belirli bir süre boyunca Depolama hesabınızdaki nesnelere sınırlı bir izinler vermek için paylaşılan erişim imzasını kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası, bir depolama kaynağına kimliği doğrulanmış erişim için gereken tüm bilgileri sorgu parametrelerinde kapsayan bir URI 'dir. Paylaşılan erişim imzasıyla depolama kaynaklarına erişmek için, istemcinin yalnızca paylaşılan erişim imzasını uygun oluşturucuya veya yönteme geçirmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. paylaşılan erişim [imzaları: paylaşılan erişim imzası modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory artık **hizmet paylaşılan erişim imzalarını** ve **Hesap paylaşılan erişim imzalarını**desteklemektedir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../storage/common/storage-sas-overview.md).
>- Sonraki veri kümesi yapılandırmasında, klasör yolu kapsayıcı düzeyinden başlayan mutlak yoldur. SAS URI 'nizin yolunu kullanarak bir hizalanmış şekilde yapılandırmanız gerekir.

> [!TIP]
> Depolama hesabınız için bir hizmet paylaşılan erişim imzası oluşturmak için aşağıdaki PowerShell komutlarını çalıştırabilirsiniz. Yer tutucuları değiştirin ve gerekli izinleri verin.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Paylaşılan erişim imzası kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureBlobStorage** (önerilen) veya **azurestorage** olarak ayarlanmalıdır (aşağıdaki notlara bakın). |Yes |
| sasUri | Blob/kapsayıcı gibi depolama kaynakları için paylaşılan erişim imzası URI 'sini belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, otomatik dönüşten yararlanmak ve belirteç bölümünü kaldırmak için Azure Key Vault SAS belirtecini de koyabilirsiniz. Daha ayrıntılı bilgi için aşağıdaki örneklere bakın ve [kimlik bilgilerini Azure Key Vault makalesine depolayın](store-credentials-in-key-vault.md) . |Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel bir ağda yer alıyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türünde bağlı hizmet kullanıyorsanız, bu yeni "AzureBlobStorage" bağlı hizmet türünü ileride kullanmanız önerilirken olduğu gibi hala desteklenmektedir.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<container>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault hesap anahtarını depolayın**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<container>.blob.core.windows.net/>"
            },
            "sasToken": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Paylaşılan erişim imzası URI 'SI oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Veri fabrikanızdaki bağlantılı hizmetin (okuma, yazma, okuma/yazma) nasıl kullanıldığını temel alarak nesneler üzerinde uygun okuma/yazma izinleri ayarlayın.
- **Süre sonu süresini** uygun şekilde ayarlayın. Depolama nesneleri erişiminin işlem hattının etkin döneminde dolamamasını sağlayın.
- URI, ihtiyacınıza göre doğru kapsayıcıda/blobda oluşturulmalıdır. Blob 'a yönelik paylaşılan erişim imzası URI 'SI, Data Factory bu blob 'a erişmesine izin verir. BLOB depolama kapsayıcısına yönelik paylaşılan erişim imzası URI 'SI, Data Factory o kapsayıcıdaki Bloblar arasında yineleme yapmasına izin verir. Daha sonra daha fazla veya daha az nesneye erişim sağlamak veya paylaşılan erişim imzası URI 'sini güncelleştirmek için, bağlantılı hizmeti yeni URI ile güncelleştirmeyi unutmayın.

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Azure depolama hizmeti sorumlusu kimlik doğrulaması için genel olarak, [Azure Active Directory kullanarak Azure depolama 'ya erişimi doğrulama](../storage/common/storage-auth-aad.md)bölümüne bakın.

Hizmet sorumlusu kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)kaydederek Azure Active Directory (Azure AD) uygulamasına bir uygulama varlığı kaydedin. Bağlı hizmeti tanımlamak için kullandığınız aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure Blob depolamada hizmet sorumlusu uygun iznini verin. Roller hakkında daha fazla ayrıntı içeren [RBAC Ile Azure depolama verilerine yönelik erişim haklarını yönetme](../storage/common/storage-auth-aad-rbac.md) bölümüne bakın.

    - **Kaynak olarak**, erişim denetimi 'NDE (IAM), en az **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**, erişim denetimi 'NDE (IAM), en az **Depolama Blobu veri katılımcısı** rolü verin.

Bu özellikler, Azure Blob depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Yes |
| Sözleşmeli | `https://<accountName>.blob.core.windows.net/`düzeniyle Azure Blob Storage hizmeti uç noktasını belirtin. |Yes |
| Serviceprincipalıd | Uygulamanın istemci KIMLIĞINI belirtin. | Yes |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |
| Kiracı | Uygulamanızın altında bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı KIMLIĞI) belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alın. | Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel bir ağda ise) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

>[!NOTE]
>Hizmet sorumlusu kimlik doğrulaması yalnızca "AzureBlobStorage" türüne bağlı hizmet ile desteklenir, ancak önceki "AzureStorage" türünde bağlı hizmet değildir.

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
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

### <a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler

Veri Fabrikası, bu belirli veri fabrikasını temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md)ilişkilendirilebilir. Bu yönetilen kimliği, kendi hizmet sorumlunuzu kullanmaya benzer şekilde BLOB depolama kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu belirlenen fabrikasının BLOB depolama verilerinize erişmesine ve veri kopyalamasına izin verir.

Azure Storage kimlik doğrulaması için [Azure Active Directory kullanarak genel olarak Azure depolama 'ya erişim kimlik doğrulaması](../storage/common/storage-auth-aad.md) konusuna bakın. Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanmak için şu adımları izleyin:

1. Fabrikalarınızla birlikte üretilen "HIZMET KIMLIĞI uygulama KIMLIĞI" değerini kopyalayarak [Veri Fabrikası tarafından yönetilen kimlik bilgilerini alın](data-factory-service-identity.md#retrieve-managed-identity) .

2. Azure Blob depolama alanında yönetilen kimlik uygun iznini verin. Roller hakkında daha fazla ayrıntı içeren [RBAC Ile Azure depolama verilerine yönelik erişim haklarını yönetme](../storage/common/storage-auth-aad-rbac.md) bölümüne bakın.

    - **Kaynak olarak**, erişim denetimi 'NDE (IAM), en az **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**, erişim denetimi 'NDE (IAM), en az **Depolama Blobu veri katılımcısı** rolü verin.

>[!IMPORTANT]
>Blob 'dan (kaynak veya hazırlama olarak) verileri SQL veri ambarı 'na yüklemek için PolyBase kullanırsanız, blob için yönetilen kimlik kimlik doğrulaması kullanılırken [1 ile 2](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) arasındaki adımları da izlediğinizden emin olun. SQL veritabanı sunucunuzu Azure etkin ile kaydetme Dizin (Azure AD) ve 2) SQL veritabanı sunucunuza Depolama Blobu veri katılımcısı rolünü atayın; Rest Data Factory tarafından işlenir. BLOB depolama alanı bir Azure sanal ağ uç noktası ile yapılandırılmışsa, verileri dosyadan yüklemek için PolyBase kullanmak üzere PolyBase 'in gerektirdiği şekilde yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir.

Bu özellikler, Azure Blob depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Yes |
| Sözleşmeli | `https://<accountName>.blob.core.windows.net/`düzeniyle Azure Blob Storage hizmeti uç noktasını belirtin. |Yes |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan Integration Runtime (veri depolduğunuz özel bir ağda ise) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!NOTE]
> Azure kaynakları için Yönetilen kimlikler kimlik doğrulaması yalnızca "AzureBlobStorage" türüne bağlı hizmet ile desteklenir, ancak önceki "AzureStorage" türünde bağlı hizmet değildir. 

**Örnek:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri kümesindeki `location` ayarları altında Azure blobu için desteklenir:

| Özellik   | Açıklama                                                  | Gereklidir |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki konumun Type özelliği **AzureBlobStorageLocation**olarak ayarlanmalıdır. | Yes      |
| kapsayıcı  | Blob kapsayıcısı.                                          | Yes      |
| folderPath | Verilen kapsayıcı altındaki klasörün yolu. Klasörü filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |
| fileName   | Verilen kapsayıcı + folderPath altındaki dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirtin. | Hayır       |

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
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

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

>[!NOTE]
>Aşağıdaki veri kümesi modeli, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Yukarıdaki bölümde bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **AzureBlob**olarak ayarlanmalıdır. |Yes |
| folderPath | BLOB depolama alanındaki kapsayıcının ve klasörün yolu. <br/><br/>Joker karakter filtresi, kapsayıcı adı dışında bir yol için desteklenir. İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. <br/><br/>Örnekler: myblobcontainer/myblobfolder/, [klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. |Kopyala/ara etkinliği için Evet, GetMetadata etkinliği için Hayır |
| fileName | Belirtilen "folderPath" altındaki blob (lar) için **ad veya joker karakter filtresi** . Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm Blobları gösterir. <br/><br/>Filtre için, izin verilen joker karakterler şunlardır: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Gerçek dosya adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın.<br/><br/>Bir çıkış veri kümesi için dosya adı belirtilmediğinde ve etkinlik havuzunda **Preservehierarchy** belirtilmemişse, kopyalama etkinliği otomatik olarak aşağıdaki Düzenle sahip blob adını oluşturur: "*Data. [ Etkinlik çalıştırma KIMLIĞI GUID 'SI]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]* ", ör." Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt. gz "; sorgu yerine tablo adını kullanarak tablo kaynağından kopyalama yaparsanız, ad deseninin adı " *[tablo adı]. [ Biçim]. [yapılandırıldıysa sıkıştırma]* ", ör." MyTable. csv ". |Hayır |
| modifiedDatetimeStart | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Veri kümesine hiçbir dosya özniteliği filtresinin uygulanamadığını gösteren Özellikler NULL olabilir.  `modifiedDatetimeStart` DateTime değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  `modifiedDatetimeEnd` DateTime değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Çok büyük miktarlarda dosyadan dosya filtresi yapmak istediğinizde, bu ayarın etkinleştirilmesi durumunda veri hareketinin genel performansını unutmayın. <br/><br/> Veri kümesine hiçbir dosya özniteliği filtresinin uygulanamadığını gösteren Özellikler NULL olabilir.  `modifiedDatetimeStart` DateTime değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  `modifiedDatetimeEnd` DateTime değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| formatını | Dosyaları dosya tabanlı depolarla (ikili kopya) olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **Orcformat**ve **parquetformat**. **Biçim** ' in altındaki **Type** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs.md#json-format), [avro Format](supported-file-formats-and-compression-codecs.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| masıyla | Verilerin türünü ve sıkıştırma düzeyini belirtin. Daha fazla bilgi için bkz. [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **gzip**, **söndür**, **bzip2**ve **zipsöndür**.<br/>Desteklenen düzeyler **en iyi** ve **en hızlardır**. |Hayır |

>[!TIP]
>Tüm Blobları bir klasör altına kopyalamak için yalnızca **FolderPath** ' i belirtin.<br>Belirli bir ada sahip tek bir blobu kopyalamak için klasör bölümü ve dosya adı ile dosya **adı Ile** **FolderPath** belirtin.<br>Bir klasör altındaki Blobların bir alt kümesini kopyalamak için klasör bölümü ve **dosya adı** joker karakter filtresi ile **FolderPath** öğesini belirtin. 

**Örnek:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, BLOB depolama kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="blob-storage-as-a-source-type"></a>Kaynak türü olarak BLOB depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, Azure Blob 'un biçim tabanlı kopyalama kaynağında `storeSettings` ayarları altında desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | `storeSettings` altındaki tür özelliği **AzureBlobStorageReadSetting**olarak ayarlanmalıdır. | Yes                                           |
| öz                | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın. İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir. | Hayır                                            |
| Yavaya Cardfolderpath       | Kaynak klasörleri filtrelemek için veri kümesinde yapılandırılan, belirtilen kapsayıcının altında joker karakter olan klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen kapsayıcı ve folderPath/yaya Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir); gerçek klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa çıkmak için `^` kullanın.  [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Veri kümesinde `fileName` belirtilmemişse Evet |
| modifiedDatetimeStart    | Öznitelikleri temel alan dosya filtresi: son değiştirme. Son değiştirilme zamanı, `modifiedDatetimeStart` ve `modifiedDatetimeEnd` arasındaki zaman aralığı içinde ise bu dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacak anlamına gelir.  `modifiedDatetimeStart` DateTime değeri olduğunda ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği DateTime değeri ile eşit veya daha büyük olan dosyalar seçilir.  `modifiedDatetimeEnd` DateTime değeri olduğunda ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parquet/delimited metin biçimi için, sonraki bölümde bahsedilen **Blobsource** türü kopyalama etkinliği kaynağı, geriye dönük uyumluluk için olduğu gibi desteklenmeye devam etmektedir. Bu yeni modeli ileriye doğru kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçti.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
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
                    "type": "AzureBlobStorageReadSetting",
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

#### <a name="legacy-source-model"></a>Eski kaynak modeli

>[!NOTE]
>Aşağıdaki kopya kaynak modeli, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Yukarıdaki ileri giderek bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği **Blobsource**olarak ayarlanmalıdır. |Yes |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın.<br/>İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
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
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Havuz türü olarak BLOB depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopya havuzunda `storeSettings` ayarları altında Azure blobu için desteklenir:

| Özellik                 | Açıklama                                                  | Gereklidir |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | `storeSettings` altındaki tür özelliği **AzureBlobStorageWriteSetting**olarak ayarlanmalıdır. | Yes      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya veya blob adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır       |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
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
                    "type": "AzureBlobStorageWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="legacy-sink-model"></a>Eski havuz modeli

>[!NOTE]
>Aşağıdaki kopya havuzu modeli, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Yukarıdaki ileri giderek bahsedilen yeni modeli kullanmanız önerilir ve ADF yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği **Blobsink**olarak ayarlanmalıdır. |Yes |
| copyBehavior | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya veya blob adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| folderPath | fileName | öz | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (boş, Varsayılanı kullan) | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `container/Folder*` | (boş, Varsayılanı kullan) | doğru | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `container/Folder*` | `*.csv` | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `container/Folder*` | `*.csv` | doğru | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Bazı özyinelemeli ve copyBehavior örnekleri

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| öz | copyBehavior | Kaynak klasör yapısı | Sonuç hedefi |
|:--- |:--- |:--- |:--- |
| doğru |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| doğru |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>Fıle1 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/>dosya2 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/>file3 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/>File4 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/>File5 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad |
| doğru |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 + dosya2 + File3 + File4 + File5 içerikleri, otomatik olarak oluşturulan bir dosya adına sahip tek bir dosyada birleştirilir. |
| yanlış |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/><br/>File3, File4 ve File5 ile Subfolder1. |
| yanlış |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>Fıle1 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/>dosya2 için &nbsp;&nbsp;&nbsp;&nbsp;otomatik olarak ad<br/><br/>File3, File4 ve File5 ile Subfolder1. |
| yanlış |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Klasör1 hedef klasörü aşağıdaki yapıyla oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adına sahip tek bir dosyada birleştirilir. Fıle1 için otomatik olarak oluşturulan ad<br/><br/>File3, File4 ve File5 ile Subfolder1. |

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki [kaynak dönüşümden](data-flow-source.md) ve [Havuz dönüşümünde](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md) onay

## <a name="next-steps"></a>Sonraki adımlar

Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
