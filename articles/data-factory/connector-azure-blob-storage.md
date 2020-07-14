---
title: Azure Blob depolamada verileri kopyalama ve dönüştürme
description: BLOB depolama alanına ve veri kopyalama ve Data Factory kullanarak blob depolamada veri dönüştürme hakkında bilgi edinin.
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/09/2020
ms.openlocfilehash: 8862de0ba36d0731fff0e47ab51f828ff69af201
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86220431"
---
# <a name="copy-and-transform-data-in-azure-blob-storage-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob depolamada verileri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-blob-connector.md)
> * [Güncel sürüm](connector-azure-blob-storage.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri Azure Blob depolama 'dan ve ' a kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Ayrıca, Azure Blob depolama alanındaki verileri dönüştürmek için veri akışı etkinliğinin nasıl kullanılacağını açıklar. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

>[!TIP]
>Bir veri Gölü veya veri ambarına yönelik bir geçiş senaryosu hakkında bilgi edinmek için bkz. [Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanma](data-migration-guidance-overview.md).

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Blob depolama Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)

Kopyalama etkinliği için bu blob Storage Bağlayıcısı şunları destekler:

- Blob 'ları genel amaçlı Azure depolama hesaplarına ve bu kaynaklardan kopyalama, sık/seyrek erişimli BLOB depolama. 
- Bir hesap anahtarı, hizmet paylaşılan erişim imzası (SAS), hizmet sorumlusu veya Azure Kaynak kimlik doğrulamaları için Yönetilen kimlikler kullanarak blob 'ları kopyalama.
- Blob 'ları blok, ekleme veya sayfa Bloblarından kopyalama ve yalnızca blok bloblarına veri kopyalama.
- Blob 'ları olduğu gibi kopyalama veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile blob ayrıştırma veya oluşturma.
- [Kopyalama sırasında dosya meta verileri koruma](#preserving-metadata-during-copy).

>[!IMPORTANT]
>Azure Storage güvenlik duvarı ayarlarında **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver** seçeneğini etkinleştirip blob depolamaya bağlanmak için Azure tümleştirme çalışma zamanını kullanmak istiyorsanız, [yönetilen kimlik kimlik doğrulamasını](#managed-identity)kullanmanız gerekir.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, blob depolamaya özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Bu BLOB depolama Bağlayıcısı aşağıdaki kimlik doğrulama türlerini destekler. Ayrıntılar için ilgili bölümlere bakın.

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Paylaşılan erişim imzası kimlik doğrulaması](#shared-access-signature-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure Kaynak kimlik doğrulaması için Yönetilen kimlikler](#managed-identity)

>[!NOTE]
>Azure SQL veri ambarı 'na veri yüklemek için PolyBase kullandığınızda, kaynak veya hazırlama BLOB depolama alanı bir Azure sanal ağ uç noktasıyla yapılandırılmışsa, PolyBase için gereken yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanını 3,18 veya üzeri bir sürümle de kullanmalısınız. Daha fazla yapılandırma önkoşulları için [yönetilen kimlik doğrulama](#managed-identity) bölümüne bakın.

>[!NOTE]
>Azure HDInsight ve Azure Machine Learning etkinlikleri yalnızca Azure Blob depolama hesabı anahtarlarını kullanan kimlik doğrulamasını destekler.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Data Factory, depolama hesabı anahtarı kimlik doğrulaması için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **AzureBlobStorage** (önerilen) veya **azurestorage** olarak ayarlanmalıdır (aşağıdaki notlara bakın). |Evet |
| Dizisi | **ConnectionString** özelliği için depolamaya bağlanmak için gereken bilgileri belirtin. <br/> Ayrıca hesap anahtarını Azure Key Vault yerleştirebilir ve `accountKey` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha fazla bilgi için, Azure Key Vault makalesinde aşağıdaki örneklere ve [Mağaza kimlik bilgilerine](store-credentials-in-key-vault.md) bakın. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolduğunuz özel bir ağda olması halinde) kullanabilirsiniz. Bu özellik belirtilmezse, hizmet varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

>[!NOTE]
>Hesap anahtarı kimlik doğrulaması kullanırken ikincil blob hizmeti uç noktası desteklenmez. Diğer kimlik doğrulama türlerini kullanabilirsiniz.

>[!NOTE]
>"AzureStorage" tür bağlı hizmetini kullanıyorsanız, hala olduğu gibi desteklenmektedir. Ancak, yeni "AzureBlobStorage" bağlı hizmet türünü ileride kullanmanızı öneririz.

**Örneğinde**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: hesap anahtarını Azure Key Vault depolayın**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
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

Paylaşılan erişim imzası, Depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Belirli bir süre boyunca Depolama hesabınızdaki nesnelere sınırlı bir izinler vermek için paylaşılan erişim imzasını kullanabilirsiniz. 

Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası, bir depolama kaynağına kimliği doğrulanmış erişim için gereken tüm bilgileri sorgu parametrelerinde kapsayan bir URI 'dir. Paylaşılan erişim imzasıyla depolama kaynaklarına erişmek için, istemcinin yalnızca paylaşılan erişim imzasını uygun oluşturucuya veya yönteme geçirmesi gerekir. 

Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. paylaşılan erişim [imzaları: paylaşılan erişim imzası modelini anlama](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory artık *hizmet paylaşılan erişim imzalarını* ve *Hesap paylaşılan erişim imzalarını*desteklemektedir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzalarını kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../storage/common/storage-sas-overview.md).
>- Sonraki veri kümesi yapılandırmalarında, klasör yolu kapsayıcı düzeyinden başlayan mutlak yoldur. SAS URI 'nizin yolunu kullanarak bir hizalanmış şekilde yapılandırmanız gerekir.

Data Factory, paylaşılan erişim imzası kimlik doğrulamasını kullanmak için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **AzureBlobStorage** (önerilen) veya **azurestorage** olarak ayarlanmalıdır (aşağıdaki nota bakın). |Evet |
| sasUri | Blob veya kapsayıcı gibi depolama kaynakları için paylaşılan erişim imzası URI 'sini belirtin. <br/>Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin. Ayrıca, Otomatik döndürmeyi kullanmak ve belirteç bölümünü kaldırmak için SAS belirtecini Azure Key Vault de yerleştirebilirsiniz. Daha fazla bilgi için, aşağıdaki örneklere bakın ve [Azure Key Vault kimlik bilgilerini depolayın](store-credentials-in-key-vault.md). |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolduğunuz özel bir ağda olması halinde) kullanabilirsiniz. Bu özellik belirtilmezse, hizmet varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" tür bağlı hizmetini kullanıyorsanız, hala olduğu gibi desteklenmektedir. Ancak, yeni "AzureBlobStorage" bağlı hizmet türünü ileride kullanmanızı öneririz.

**Örneğinde**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<accountname>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: hesap anahtarını Azure Key Vault depolayın**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<accountname>.blob.core.windows.net/>"
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
- URI, ihtiyacınıza göre doğru kapsayıcıda veya blob 'da oluşturulmalıdır. Blob 'a yönelik paylaşılan erişim imzası URI 'SI, Data Factory bu blob 'a erişmesine izin verir. BLOB depolama kapsayıcısına yönelik paylaşılan erişim imzası URI 'SI, Data Factory o kapsayıcıdaki Bloblar arasında yineleme yapmasına izin verir. Daha sonra daha fazla veya daha az nesneye erişim sağlamak veya paylaşılan erişim imzası URI 'sini güncelleştirmek için, bağlantılı hizmeti yeni URI ile güncelleştirmeyi unutmayın.

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Azure depolama hizmeti sorumlusu kimlik doğrulaması hakkında genel bilgi için bkz. [Azure Active Directory kullanarak Azure depolama 'ya erişim kimlik doğrulaması](../storage/common/storage-auth-aad.md).

Hizmet sorumlusu kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)kaydederek Azure Active Directory (Azure AD) uygulamasına bir uygulama varlığı kaydedin. Bağlı hizmeti tanımlamak için kullandığınız bu değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure Blob depolamada hizmet sorumlusu uygun iznini verin. Roller hakkında daha fazla bilgi için bkz. [RBAC Ile Azure depolama verilerine erişim haklarını yönetme](../storage/common/storage-auth-aad-rbac.md).

    - **Kaynak olarak**, **erişim denetımı 'nde (IAM)**, en azından **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**, **erişim denetımı 'nde (IAM)**, en azından **Depolama Blobu veri katılımcısı** rolüne izin verin.

Bu özellikler, Azure Blob depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Evet |
| Sözleşmeli | Düzenine sahip Azure Blob depolama hizmeti uç noktasını belirtin `https://<accountName>.blob.core.windows.net/` . |Evet |
| Serviceprincipalıd | Uygulamanın istemci KIMLIĞINI belirtin. | Evet |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet |
| Kiracı | Uygulamanızın altında bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı KIMLIĞI) belirtin. Azure portal, sağ üst köşenin üzerine gelerek alın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolduğunuz özel bir ağda olması halinde) kullanabilirsiniz. Bu özellik belirtilmezse, hizmet varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

>[!NOTE]
>Hizmet sorumlusu kimlik doğrulaması, önceki "AzureStorage" türü bağlı hizmeti değil, yalnızca "AzureBlobStorage" türüne bağlı hizmet tarafından desteklenir.

**Örneğinde**

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

### <a name="managed-identities-for-azure-resource-authentication"></a><a name="managed-identity"></a>Azure Kaynak kimlik doğrulaması için Yönetilen kimlikler

Veri Fabrikası, bu belirli veri fabrikasını temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md)ilişkilendirilebilir. Bu yönetilen kimliği, kendi hizmet sorumlunuzu kullanmaya benzer şekilde, BLOB depolama kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu belirlenen fabrikasının BLOB depolama alanına veya blob depolamaya veri kopyalamasına izin verir.

Azure depolama kimlik doğrulaması hakkında genel bilgi için bkz. [Azure Active Directory kullanarak Azure depolama 'ya erişim kimlik doğrulaması](../storage/common/storage-auth-aad.md). Azure Kaynak kimlik doğrulaması için yönetilen kimlikleri kullanmak için şu adımları izleyin:

1. Fabrikalarınızla birlikte oluşturulan yönetilen kimlik nesne KIMLIĞININ değerini kopyalayarak [Data Factory yönetilen kimlik bilgilerini alın](data-factory-service-identity.md#retrieve-managed-identity) .

2. Azure Blob depolama alanında yönetilen kimlik iznini verin. Roller hakkında daha fazla bilgi için bkz. [RBAC Ile Azure depolama verilerine erişim haklarını yönetme](../storage/common/storage-auth-aad-rbac.md).

    - **Kaynak olarak**, **erişim denetımı 'nde (IAM)**, en azından **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**, **erişim denetımı 'nde (IAM)**, en azından **Depolama Blobu veri katılımcısı** rolüne izin verin.

>[!IMPORTANT]
>BLOB depolama alanından (kaynak veya hazırlama olarak) verileri SQL veri ambarı 'na yüklemek için PolyBase kullanırsanız, blob depolaması için yönetilen kimlik kimlik doğrulamasını kullanırken, [Bu](../azure-sql/database/vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)kılavuzda 1 ve 2. adımları izlediğinizden emin olun. Bu adımlar, sunucunuzu Azure AD 'ye kaydeder ve Depolama Blobu veri katılımcısı rolünü sunucunuza atar. Data Factory Rest 'i işler. Blob depolamayı bir Azure sanal ağ uç noktası ile yapılandırdıysanız, verileri dosyadan yüklemek için PolyBase kullanmak istiyorsanız, PolyBase 'in gerektirdiği şekilde yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir.

Bu özellikler, Azure Blob depolama bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Evet |
| Sözleşmeli | Düzenine sahip Azure Blob depolama hizmeti uç noktasını belirtin `https://<accountName>.blob.core.windows.net/` . |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolduğunuz özel bir ağda olması halinde) kullanabilirsiniz. Bu özellik belirtilmezse, hizmet varsayılan Azure tümleştirme çalışma zamanını kullanır. |Hayır |

> [!NOTE]
> Azure Kaynak kimlik doğrulaması için Yönetilen kimlikler, önceki "AzureStorage" türü bağlı hizmeti değil, yalnızca "AzureBlobStorage" türünde bağlı hizmet tarafından desteklenir.

**Örneğinde**

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

Aşağıdaki özellikler, `location` Biçim tabanlı bir veri kümesindeki ayarlar altında Azure Blob depolama için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| tür       | Veri kümesindeki konumun **Type** özelliği **AzureBlobStorageLocation**olarak ayarlanmalıdır. | Evet      |
| kapsayıcı  | Blob kapsayıcısı.                                          | Evet      |
| folderPath | Verilen kapsayıcı altındaki klasörün yolu. Klasörü filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirleyin. | Hayır       |
| fileName   | Verilen kapsayıcı ve klasör yolu altındaki dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları ' nda belirleyin. | Hayır       |

**Örneğinde**

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, BLOB depolama kaynağı ve havuzunun desteklediği özelliklerin bir listesini sağlar.

### <a name="blob-storage-as-a-source-type"></a>Kaynak türü olarak BLOB depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, Azure Blob depolama için, `storeSettings` Biçim tabanlı bir kopyalama kaynağında ayarlar altında desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| tür                     | İçindeki **tür** özelliği `storeSettings` **AzureBlobStorageReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| ***Kopyalanacak dosyaları bulun:*** |  |  |
| SEÇENEK 1: statik yol<br> | Veri kümesinde belirtilen kapsayıcı veya klasör/dosya yolundan Kopyala. Tüm Blobları bir kapsayıcı veya klasörden kopyalamak istiyorsanız, ayrıca olarak öğesini belirtin `wildcardFileName` `*` . |  |
| Seçenek 2: blob ön eki<br>-önek | Kaynak bloblarını filtrelemek için bir veri kümesinde yapılandırılan, belirtilen kapsayıcının altındaki blob adı için ön ek. Adları ile başlayan blob 'lar `container_in_dataset/this_prefix` seçilidir. BLOB depolama için hizmet tarafı filtresinden yararlanır ve bu, joker karakter filtresinden daha iyi performans sağlar. | Hayır                                                          |
| Seçenek 3: joker karakter<br>-Yavaya Cardfolderpath | Kaynak klasörleri filtrelemek için bir veri kümesinde yapılandırılan, belirtilen kapsayıcının altındaki joker karakterleri içeren klasör yolu. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). `^`Klasör adınızın joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Seçenek 3: joker karakter<br>-Yavaya Cardfilename | Kaynak dosyalarını filtrelemek için verilen kapsayıcı ve klasör yolu (veya joker karakter klasörü yolu) altındaki joker karakterlerle dosya adı. <br>İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). `^`Klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Evet |
| 4. seçenek: dosya listesi<br>-fileListPath | Belirli bir dosya kümesinin kopyalanıp ayrılmadığını gösterir. Veri kümesinde yapılandırılan yolun göreli yolu olan, kopyalamak istediğiniz dosyaların listesini içeren bir metin dosyası üzerine gelin.<br/>Bu seçeneği kullandığınızda, veri kümesinde bir dosya adı belirtmeyin. [Dosya listesi örneklerinde](#file-list-examples)daha fazla örneğe bakın. |Hayır |
| ***Ek ayarlar:*** |  | |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. **Özyinelemeli** değeri **true** olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın. <br>İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir.<br>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . |Hayır |
| deleteFilesAfterCompletion | Hedef depoya başarıyla taşıdıktan sonra, ikili dosyaların kaynak depodan silinip silinmeyeceğini gösterir. Dosya silme dosya başına yapılır, bu nedenle kopyalama etkinliği başarısız olduğunda, bazı dosyaların hedefe zaten kopyalanmış ve kaynaktan silindiği görürsünüz, diğerleri ise kaynak deposunda hala kalır. <br/>Bu özellik yalnızca, veri kaynağı depolamadaki blob, ADLS 1., ADLS 2., S3, Google bulut depolama, dosya, Azure dosyası, SFTP veya FTP olan ikili kopyalama senaryosunda geçerlidir. Varsayılan değer: false. |Hayır |
| modifiedDatetimeStart    | Dosyalar şu özniteliğe göre filtrelenmiştir: son değiştirme. <br>Son değiştirilme zamanı ve arasındaki zaman aralığı içinde ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, "2018-12-01T05:00:00Z" biçiminde bir UTC saat dilimine uygulanır. <br> Özellikler **null**olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacağı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.<br/>Bu özellik, yapılandırdığınızda uygulanmaz `fileListPath` . | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolamaya yönelik eşzamanlı bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parquet/delimited metin biçimi için, sonraki bölümde bahsedilen kopyalama etkinliği kaynağı için **Blobsource** türü, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturmaya geçene kadar yeni modeli kullanmanızı öneririz.

**Örneğinde**

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
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
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

### <a name="blob-storage-as-a-sink-type"></a>Havuz türü olarak BLOB depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, `storeSettings` Biçim tabanlı bir kopya havuzunda ayarlar altında Azure Blob depolama için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| tür                     | İçindeki **tür** özelliği `storeSettings` **AzureBlobStorageWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya veya blob adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır       |
| Blocksizeınmb | Blok bloblarına veri yazmak için kullanılan blok boyutunu megabayt cinsinden belirtin. [Blok Blobları hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)daha fazla bilgi edinin. <br/>İzin verilen değer *4 MB ile 100 MB arasındadır*. <br/>Varsayılan olarak Data Factory, blok boyutunu kaynak depolama türü ve verilerinize göre otomatik olarak belirler. Blob depolamaya ikili olmayan kopya için, varsayılan blok boyutu 100 MB 'tır, bu nedenle en fazla 4,95 TB veri olabilir. Özellikle, işlem zaman aşımı veya performans sorunlarına neden olan kötü ağ bağlantılarıyla şirket içinde barındırılan tümleştirme çalışma zamanını kullandığınızda verileriniz büyük olmadığında en iyi durumda olmayabilir. Açıkça bir blok boyutu belirtebilirsiniz, bu da `blockSizeInMB*50000` verileri depolamaya yetecek büyüklükte bir değer sağlar. Aksi takdirde kopyalama etkinliği çalıştırması başarısız olur. | Hayır |
| maxConcurrentConnections | Depolamaya yönelik eşzamanlı bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde belirtin. | Hayır       |

**Örneğinde**

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
                    "type": "AzureBlobStorageWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| folderPath | fileName | öz | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (boş, Varsayılanı kullan) | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | (boş, Varsayılanı kullan) | true | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile2.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ÜzerindeFile4.js**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | true | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="file-list-examples"></a>Dosya listesi örnekleri

Bu bölümde, kopyalama etkinlik kaynağında bir dosya listesi yolu kullanmanın ortaya çıkan davranışı açıklanmaktadır.

Aşağıdaki kaynak klasör yapısına sahip olduğunuz ve dosyaları kalın yazı tipinde kopyalamak istediğinizi varsayalım:

| Örnek kaynak yapısı                                      | FileListToCopy.txt içerik                             | Data Factory Yapılandırması                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;Klasör a<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile2.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ÜzerindeFile4.js<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Veriyi<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy.txt | File1.csv<br>Subfolder1/File3.csv<br>Subfolder1/File5.csv | **Veri kümesinde:**<br>Kapsayıcı`container`<br>-Klasör yolu:`FolderA`<br><br>**Kopyalama etkinliği kaynağı:**<br>-Dosya listesi yolu:`container/Metadata/FileListToCopy.txt` <br><br>Dosya listesi yolu, veri kümesinde yapılandırılan yolun göreli yolu ile, kopyalamak istediğiniz dosyaların bir listesini, her satıra bir dosya içeren bir metin dosyasını gösterir. |

### <a name="some-recursive-and-copybehavior-examples"></a>Bazı özyinelemeli ve copyBehavior örnekleri

Bu bölümde, **özyinelemeli** ve **copybehavior** değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| öz | copyBehavior | Kaynak klasör yapısı | Sonuç hedefi |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;file3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 + File3 + File4 + File5 içerikler, otomatik olarak oluşturulan bir dosya adına sahip tek bir dosyada birleştirilir. |
| yanlış |preserveHierarchy | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>File3, File4 ve File5 ile Subfolder1. |
| yanlış |DÜZEDEN hiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, aşağıdaki yapıyla oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>File3, File4 ve File5 ile Subfolder1. |
| yanlış |mergeFiles | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef klasör, Klasör1, aşağıdaki yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adına sahip bir dosyada birleştirilir. Fıle1 için otomatik olarak oluşturulan ad<br/><br/>File3, File4 ve File5 ile Subfolder1. |

## <a name="preserving-metadata-during-copy"></a>Kopya sırasında meta verileri koruma

Dosyaları Amazon S3, Azure Blob Storage veya Azure Data Lake Storage 2. Azure Data Lake Storage 2. ya da Azure Blob depolama alanından kopyaladığınızda, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz. [Meta verileri koru](copy-activity-preserve-metadata.md#preserve-metadata)'dan daha fazla bilgi edinin.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışlarındaki verileri dönüştürürken, aşağıdaki biçimlerde Azure Blob depolamadan dosyaları okuyabilir ve yazabilirsiniz:
* [Avro](format-avro.md#mapping-data-flow-properties)
* [Sınırlandırılmış metin](format-delimited-text.md#mapping-data-flow-properties)
* [Delta](format-delta.md#mapping-data-flow-properties)
* [Excel](format-excel.md#mapping-data-flow-properties)
* [JSON](format-json.md#mapping-data-flow-properties)
* [Parquet](format-parquet.md#mapping-data-flow-properties)

Biçimlendirme özel ayarları, bu biçimdeki belgelerde bulunur. Daha fazla bilgi için bkz. eşleme veri akışı ve [Havuz dönüşümünde](data-flow-sink.md) [kaynak dönüştürme](data-flow-source.md) veri akışında.

### <a name="source-transformation"></a>Kaynak dönüştürme

Kaynak dönüşümünde, Azure Blob depolama alanındaki bir kapsayıcı, klasör veya tek dosyadan okuyabilirsiniz. Dosyaların nasıl okunacağını yönetmek için **kaynak seçenekleri** sekmesini kullanın. 

![Kaynak seçenekleri](media/data-flow/sourceOptions1.png "Kaynak seçenekleri")

**Joker karakter yolları:** Joker karakter deseninin kullanılması Data Factory, tek bir kaynak dönüşümünde eşleşen her klasör ve Dosya arasında döngü göstermesi talimatını verecektir. Bu, tek bir akış içinde birden çok dosyayı işlemek için etkili bir yoldur. Mevcut joker karakter deseniniz üzerine geldiğinizde görüntülenen artı işaretine sahip birden fazla joker karakterle eşleşen desenler ekleyin.

Kaynak kapsayıcısından bir düzeniyle eşleşen bir dosya serisi seçin. Veri kümesinde yalnızca bir kapsayıcı belirtilebilir. Bu nedenle, joker karakter yolunuzda Ayrıca, kök klasörden klasör yolunuzdan de yer verilmelidir.

Joker karakter örnekleri:

* ```*```Herhangi bir karakter kümesini temsil eder.
* ```**```Özyinelemeli dizin iç içe geçirmeyi temsil eder.
* ```?```Bir karakteri değiştirir.
* ```[]```Köşeli parantezdeki bir veya daha fazla karakterle eşleşir.

* ```/data/sales/**/*.csv```/Data/salesaltındaki tüm. csv dosyalarını alır.
* ```/data/sales/20??/**/```Tüm dosyaları 20. yüzyıl içinde alır.
* ```/data/sales/*/*/*.csv```/Data/salesaltında. csv dosyalarını iki düzey alır.
* ```/data/sales/2004/*/12/[XY]1?.csv```2 Aralık 2004 ' deki tüm. csv dosyalarını, iki basamaklı bir sayı tarafından önekli X veya Y ile başlayarak alır.

**Bölüm kök yolu:** Dosya kaynağınızda bir biçimde bölümlenmiş klasörler ```key=value``` (örneğin, `year=2019` ) varsa, bu bölüm klasör ağacının en üst düzeyini veri akışınızdaki veri akışındaki bir sütun adına atayabilirsiniz.

İlk olarak, bölümlenmiş klasörler ve okumak istediğiniz yaprak dosyaları olan tüm yolları içerecek şekilde bir joker karakter ayarlayın.

![Bölüm kaynak dosyası ayarları](media/data-flow/partfile2.png "Bölüm dosyası ayarı")

Klasör yapısının en üst düzeyinin ne olduğunu tanımlamak için **bölüm kök yolu** ayarını kullanın. Verilerinizin içeriğini bir veri önizleme aracılığıyla görüntülediğinizde, Data Factory klasör düzeylerinizde bulunan çözümlenmiş bölümlerin ekleneceğini görürsünüz.

![Bölüm kök yolu](media/data-flow/partfile1.png "Bölüm kök yolu önizlemesi")

**Dosya listesi:** Bu bir dosya kümesidir. İşlemek için göreli yol dosyalarının bir listesini içeren bir metin dosyası oluşturun. Bu metin dosyasına işaret edin.

**Depolanacak sütun dosya adı:** Kaynak dosyanın adını verilerinizin bir sütununda depolayın. Dosya adı dizesini depolamak için buraya yeni bir sütun adı girin.

**Tamamlandıktan sonra:** Veri akışı çalıştıktan sonra kaynak dosyayla ilgili hiçbir şey yapma seçeneğini belirleyin, kaynak dosyayı silin veya kaynak dosyayı taşıyın. Taşımanın yolları görelidir.

Kaynak dosyalarını başka bir konuma işleme sonrası taşımak için, önce dosya işlemi için "taşı" yı seçin. Sonra, "Kimden" dizinini ayarlayın. Yolunuzda herhangi bir joker karakter kullanmıyorsanız, "Kimden" ayarı kaynak klasörünüzün bulunduğu klasör olacaktır.

Joker karakter içeren bir kaynak yolunuz varsa, söz dizimi şöyle görünür:

```/data/sales/20??/**/*.csv```

"Kimden" i şu şekilde belirtebilirsiniz:

```/data/sales```

Ve "to" belirtebilirsiniz:

```/backup/priorSales```

Bu durumda,/Data/Sales altında kaynağı bulunan tüm dosyalar/Backup/priorsales' a taşınır.

> [!NOTE]
> Dosya işlemleri, bir işlem hattındaki veri akışını Yürüt etkinliğini kullanan bir işlem hattı çalıştırmasıyla (bir işlem hattı hata ayıklaması veya yürütme çalıştırması) veri akışını başlattığınızda çalışır. Dosya işlemleri veri akışı hata ayıklama *modunda çalışmaz.*

**Son değiştirme ölçütü:** Son değiştirilme tarihi için bir tarih aralığı belirterek hangi dosyaları işleyeistediğinizi filtreleyebilirsiniz.  Tüm tarih ve saatler UTC cinsindendir. 

### <a name="sink-properties"></a>Havuz özellikleri

Havuz dönüşümünde, bir kapsayıcıya veya Azure Blob depolama alanındaki bir klasöre yazabilirsiniz. Dosyaların nasıl yazıldığını yönetmek için **Ayarlar** sekmesini kullanın.

![Havuz seçenekleri](media/data-flow/file-sink-settings.png "havuz seçenekleri")

**Klasörü temizle:** Hedef klasörün, veriler yazılmadan önce temizlenip temizlenmeyeceğini belirler.

**Dosya adı seçeneği:** Hedef klasördeki hedef dosyaların nasıl adlandırıldığını belirler. Dosya adı seçenekleri şunlardır:
   * **Varsayılan**: Spark 'ın bölüm varsayılanlarına göre dosya adına erişmesine izin verin.
   * **Model**: bölüm başına çıktı dosyalarınızı numaralandırır bir model girin. Örneğin, **krediler [n]. csv** loans1.csv, loans2.csv vb. oluşturacak.
   * **Bölüm başına**: bölüm başına bir dosya adı girin.
   * **Sütunda veri olarak**: çıkış dosyasını bir sütunun değeri olarak ayarlayın. Yol, hedef klasöre değil, veri kümesi kapsayıcısına göredir. Veri kümenizde bir klasör yolunuz varsa, bu geçersiz kılınır.
   * **Tek bir dosyaya çıkış**: bölümlenmiş çıktı dosyalarını tek bir adlandırılmış dosyada birleştirin. Yol, veri kümesi klasörüne görelidir. Birleştirme işleminin düğüm boyutuna bağlı olarak başarısız olabileceğini unutmayın. Büyük veri kümeleri için bu seçeneği önermiyoruz.

**Tüm teklif:** Tüm değerlerin tırnak işaretleri içine alınacağını belirler.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md)inceleyin. 

## <a name="delete-activity-properties"></a>Etkinlik özelliklerini Sil

Özelliklerle ilgili ayrıntıları öğrenmek için [silme etkinliği](delete-activity.md)' ni işaretleyin.

## <a name="legacy-models"></a>Eski modeller

>[!NOTE]
>Geriye dönük uyumluluk için olduğu gibi aşağıdaki modeller de desteklenir. Daha önce bahsedilen yeni modeli kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi yeni modeli oluşturmaya geçti.

### <a name="legacy-dataset-model"></a>Eski veri kümesi modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin **Type** özelliği **AzureBlob**olarak ayarlanmalıdır. |Evet |
| folderPath | Blob depolamada kapsayıcı ve klasörün yolu. <br/><br/>Kapsayıcı adı hariç olmak üzere yol için bir joker karakter filtresi desteklenir. İzin verilen joker karakterler: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir). `^`Klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br/><br/>Örnek: myblobcontainer/myblobfolder/. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. |GetMetadata etkinliği için Hayır, kopyalama veya arama etkinliği için Evet |
| fileName | Belirtilen **FolderPath** değeri altındaki Blobların adı veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm Blobları gösterir. <br/><br/>Filtre için, izin verilen joker karakterler şunlardır: `*` (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1:`"fileName": "*.csv"`<br/>-Örnek 2:`"fileName": "???20180427.txt"`<br/>`^`Dosya adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın.<br/><br/>Bir çıkış veri kümesi için **dosya adı** belirtilmediğinde ve etkinlik havuzunda **Preservehierarchy** belirtilmemişse, kopyalama etkinliği otomatik olarak aşağıdaki Düzenle sahip blob adını oluşturur: "*Data. [ Etkinlik çalıştırma KIMLIĞI GUID 'SI]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]*". Örneğin: "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. gz". <br/><br/>Sorgu yerine tablo adı kullanarak tablosal kaynaktan kopyalama yaparsanız, ad deseninin "*[tablo adı]" olması gerekir. [ Biçim]. [yapılandırıldıysa sıkıştırma]*". Örneğin: "MyTable.csv". |Hayır |
| modifiedDatetimeStart | Dosyalar şu özniteliğe göre filtrelenmiştir: son değiştirme. Son değiştirilme zamanı ve arasındaki zaman aralığı içinde ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Bu ayarın etkinleştirilmesi, çok büyük miktarlarda dosya filtrelemeniz istediğinizde veri hareketinin genel performansını etkiler. <br/><br/> Özellikler **null**olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacağı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Dosyalar şu özniteliğe göre filtrelenmiştir: son değiştirme. Son değiştirilme zamanı ve arasındaki zaman aralığı içinde ise dosyalar seçilir `modifiedDatetimeStart` `modifiedDatetimeEnd` . Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Bu ayarın etkinleştirilmesi, çok büyük miktarlarda dosya filtrelemeniz istediğinizde veri hareketinin genel performansını etkiler. <br/><br/> Özellikler **null**olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmayacağı anlamına gelir.  Ne zaman `modifiedDatetimeStart` bir tarih saat değeri olduğunda `modifiedDatetimeEnd` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir.  Ne zaman `modifiedDatetimeEnd` bir tarih saat değeri olduğunda `modifiedDatetimeStart` , ancak **null**ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| biçim | Dosyaları dosya tabanlı depolarla (ikili kopya) olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **Orcformat**ve **parquetformat**. **Biçim** ' in altındaki **Type** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs-legacy.md#json-format), [avro Format](supported-file-formats-and-compression-codecs-legacy.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| sıkıştırma | Verilerin türünü ve sıkıştırma düzeyini belirtin. Daha fazla bilgi için bkz. [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs-legacy.md#compression-support).<br/>Desteklenen türler **gzip**, **söndür**, **bzip2**ve **zipsöndür**.<br/>Desteklenen düzeyler **en iyi** ve **en hızlardır**. |Hayır |

>[!TIP]
>Tüm Blobları bir klasör altına kopyalamak için yalnızca **FolderPath** ' i belirtin.<br>Belirli bir ada sahip tek bir blobu kopyalamak için dosya adı için klasör **bölümü ve dosya adı Için** **FolderPath** belirtin.<br>Bir klasör altındaki Blobların bir alt kümesini kopyalamak için, klasör bölümü ve **dosya adı** için bir joker karakter filtresi içeren **FolderPath** öğesini belirtin. 

**Örneğinde**

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

### <a name="legacy-source-model-for-the-copy-activity"></a>Kopyalama etkinliği için eski kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının **Type** özelliği **blobsource**olarak ayarlanmalıdır. |Evet |
| öz | Verilerin alt klasörlerden veya yalnızca belirtilen klasörden özyinelemeli olarak okunup okunmadığını gösterir. **Özyinelemeli** değeri **true** olarak ayarlandığında ve havuz dosya tabanlı bir depo olduğunda, havuzda boş bir klasör veya alt klasör kopyalanmadığını veya oluşturulamadığına unutmayın.<br/>İzin verilen değerler **true** (varsayılan) ve **false**şeklindedir. | Hayır |
| maxConcurrentConnections | Depolamaya yönelik eşzamanlı bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde belirtin. | Hayır |

**Örneğinde**

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

### <a name="legacy-sink-model-for-the-copy-activity"></a>Kopyalama etkinliği için eski havuz modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun **Type** özelliği **blobsink**olarak ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosya tabanlı bir veri deposundan dosyalar olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu hedef dosyanın hedef klasöre göreli yolu ile aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlara sahiptir. <br/><b>-Mergefiles</b>: kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya veya blob adı belirtilmişse, birleştirilmiş dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulan bir dosya adıdır. | Hayır |
| maxConcurrentConnections | Depolamaya yönelik eşzamanlı bağlantı sayısı. Yalnızca veri deposuyla eş zamanlı bağlantıları sınırlandırmak istediğinizde belirtin. | Hayır |

**Örneğinde**

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

## <a name="next-steps"></a>Sonraki adımlar

Data Factory içindeki kopyalama etkinliğinin kaynak ve havuz olarak desteklediği veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
