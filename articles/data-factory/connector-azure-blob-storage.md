---
title: Azure Blob depolama alanında verileri kopyalama ve dönüştürme
description: Veri Fabrikası'nı kullanarak Blob depolamaya gelen ve bu depolamadan gelen verileri nasıl kopyalayacağım ve Blob depolamadaki verileri nasıl dönüştüreceklerini öğrenin.
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/09/2020
ms.openlocfilehash: dd13a08b3c2f63baf509efbb730032edd4eba61a
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011557"
---
# <a name="copy-and-transform-data-in-azure-blob-storage-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure Blob depolamasında verileri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-blob-connector.md)
> * [Geçerli sürüm](connector-azure-blob-storage.md)

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin Azure Blob depolamadan ve Azure Blob depolamasından kopyalanması ve Azure Blob depolamasındaverileri dönüştürmek için Veri Akışı'nın nasıl kullanılacağı özetlenir. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

>[!TIP]
>Veri gölü veya veri ambarı geçiş senaryosu için, [veri gölünüzden veya veri ambarınızdaki verileri Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın'dan](data-migration-guidance-overview.md)daha fazla bilgi edinin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Blob bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
- [Etkinliği silme](delete-activity.md)

Kopyalama etkinliği için bu Blob depolama bağlayıcısı şunları destekler:

- Blob'ları genel amaçlı Azure depolama hesaplarına ve sıcak/havalı blob depolama sına kopyalama. 
- Azure kaynakları kimlik doğrulamaları için hesap anahtarı, hizmet paylaşılan erişim imzası, hizmet sorumlusu veya yönetilen kimlikler kullanarak blob'ları kopyalama.
- Blobs'ı blok, ek veya sayfa lekelerinden kopyalama ve yalnızca blobları engellemek için verileri kopyalama.
- Blobs'u olduğu gibi kopyalama veya desteklenen dosya [biçimleri ve sıkıştırma codec'leri](supported-file-formats-and-compression-codecs.md)ile blobs'ları ayrıştırma veya oluşturma.
- [Kopya sırasında dosya meta verilerini koruyun.](#preserve-metadata-during-copy)

>[!IMPORTANT]
>Güvenilen Microsoft hizmetlerinin Azure Depolama güvenlik duvarı ayarlarında **bu depolama hesabı seçeneğine erişmesini** etkinleştiriyorsanız ve Blob Depolama'nıza bağlanmak için Azure tümleştirme çalışma süresini kullanmak istiyorsanız, yönetilen kimlik kimlik [doğrulaması](#managed-identity)kullanmanız gerekir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Blob depolamasına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Blob bağlayıcısı aşağıdaki kimlik doğrulama türlerini destekler ve ayrıntılarla ilgili bölüme bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Paylaşılan erişim imzası kimlik doğrulaması](#shared-access-signature-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynakları kimlik doğrulaması için yönetilen kimlikler](#managed-identity)

>[!NOTE]
>Verileri SQL Veri Ambarı'na yüklemek için PolyBase'i kullanırken, kaynak veya evreleme Blob depolama alanınız Virtual Network bitiş noktasıyla yapılandırıldıysa, PolyBase tarafından gerekli olduğu şekilde yönetilen kimlik doğrulamasını kullanmalı ve sürüm 3.18 veya üzeri sürümle Kendi kendine barındırılan Tümleştirme Runtime'ı kullanmanız gerekir. Daha fazla yapılandırma ön koşuluyla [yönetilen kimlik kimlik doğrulama](#managed-identity) bölümüne bakın.

>[!NOTE]
>HDInsights ve Azure Machine Learning etkinlikleri yalnızca Azure Blob depolama hesabı anahtar kimlik doğrulamasını destekler.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtar kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobStorage** (önerilen) veya **AzureStorage** olarak ayarlanmalıdır (aşağıdaki notlara bakın). |Evet |
| Connectionstring | ConnectionString özelliği için Depolama'ya bağlanmak için gereken bilgileri belirtin. <br/> Hesap anahtarını Azure Key Vault'a `accountKey` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağdaysa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>İkincil Blob Service Endpoint hesap anahtarı kimlik doğrulaması kullanırken desteklenmez. Diğer kimlik doğrulama türlerini kullanabilirsiniz.

>[!NOTE]
>"AzureStorage" türüne bağlı hizmeti kullanıyorsanız, bu yeni "AzureBlobStorage" bağlantılı hizmet türünü ileriye dönük olarak kullanmanız önerilirken, bu hizmet yine de olduğu gibi desteklenir.

**Örnek:**

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

**Örnek: Azure Key Vault'ta mağaza hesabı anahtarı**

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

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir istemciye depolama hesabınızdaki nesnelere belirli bir süre için sınırlı izinler vermek için paylaşılan erişim imzasını kullanabilirsiniz. Hesap erişim anahtarlarınızı paylaşmak zorunda değilsiniz. Paylaşılan erişim imzası, sorgu parametrelerinde bir depolama kaynağına kimlik doğrulaması için gerekli tüm bilgileri kapsayan bir URI'dir. Paylaşılan erişim imzasıyla depolama kaynaklarına erişmek için istemcinin yalnızca paylaşılan erişim imzasını uygun oluşturucuya veya yönteme geçirmesi gerekir. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzaları: Paylaşılan erişim imza modelini anlayın.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!NOTE]
>- Veri Fabrikası artık hem **hizmet paylaşılan erişim imzalarını** hem de **hesap paylaşılan erişim imzalarını**destekler. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../storage/common/storage-sas-overview.md)ver.
>- Daha sonraki veri kümesi yapılandırmasında, klasör yolu kapsayıcı düzeyinden başlayarak mutlak yoldur. SAS URI'nizdeki yolla uyumlu bir yapıya sahip olanı yapılandırmanız gerekir.

Paylaşılan erişim imzası kimlik doğrulamasını kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobStorage** (önerilen) veya **AzureStorage** olarak ayarlanmalıdır (aşağıdaki notlara bakın). |Evet |
| sasUri | Blob/konteyner gibi Depolama kaynaklarına paylaşılan erişim imzası URI'yi belirtin. <br/>Bu alanı Güvenli Bir Şekilde Veri Fabrikası'nda depolamak için SecureString olarak işaretleyin. Otomatik döndürmeden yararlanmak ve belirteç kısmını kaldırmak için Azure Key Vault'a SAS belirteci de koyabilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>"AzureStorage" türüne bağlı hizmeti kullanıyorsanız, bu yeni "AzureBlobStorage" bağlantılı hizmet türünü ileriye dönük olarak kullanmanız önerilirken, bu hizmet yine de olduğu gibi desteklenir.

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

**Örnek: Azure Key Vault'ta mağaza hesabı anahtarı**

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

Paylaşılan bir erişim imzası URI oluşturduğunuzda, aşağıdaki noktaları göz önünde bulundurun:

- Veri fabrikanızda bağlantılı hizmetin (okuma, yazma, okuma/yazma) nasıl kullanıldığına bağlı olarak nesneler üzerinde uygun okuma/yazma izinleri ayarlayın.
- **Son kullanma süresini** uygun şekilde ayarlayın. Depolama nesnelerine erişimin ardışık ardışık etkin süre içinde sona ermediğinden emin olun.
- URI ihtiyaca göre doğru kap/blob da oluşturulmalıdır. Bir blob için paylaşılan erişim imzası URI Veri Fabrikası belirli bir blob erişmesine olanak sağlar. Blob depolama konteynerine ortak erişim imzası URI, Veri Fabrikası'nın bu kapsayıcıdaki lekeleri tekrarlamasına olanak tanır. Daha sonra daha fazla veya daha az nesneye erişim sağlamak veya paylaşılan erişim imzası URI'yi güncelleştirmek için bağlantılı hizmeti yeni URI ile güncelleştirmeyi unutmayın.

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Azure Depolama hizmeti nin temel kimlik doğrulaması için Azure [Etkin Dizin'i kullanarak Azure Depolama'ya kimlik doğrulama](../storage/common/storage-auth-aad.md)erişimine bakın.

Hizmet temel kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. Uygulamanızı [bir Azure AD kiracısıyla kaydederek](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)bir uygulama kuruluşunu Azure Etkin Dizin'e (Azure AD) kaydedin. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure Blob depolama alanında hizmet müdürüne uygun izin ver. Roller hakkında daha fazla ayrıntı içeren [RBAC ile Azure Depolama verilerine erişim haklarını yönet'e](../storage/common/storage-auth-aad-rbac.md) bakın.

    - **Kaynak olarak,** Access denetiminde (IAM), en azından **Depolama Blob Veri Okuyucu** rolünü verir.
    - **Lavabo olarak**, Access denetiminde (IAM), en azından **Depolama Blob Veri Katılımcısı** rolünü verin.

Bu özellikler, Azure Blob depolamasına bağlı bir hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Evet |
| Serviceendpoint | Azure Blob depolama hizmeti bitiş noktasını `https://<accountName>.blob.core.windows.net/`' deseni ile belirtin. |Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| Kiracı | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağdaysa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

>[!NOTE]
>Hizmet temel kimlik doğrulaması yalnızca "AzureBlobStorage" türüne bağlı hizmet tarafından desteklenir, ancak önceki "AzureStorage" türüne bağlı hizmet tarafından desteklenmez.

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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikler

Bir veri fabrikası, bu özel veri fabrikasını temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md)ilişkilendirilebilir. Blob depolama kimlik doğrulaması için bu yönetilen kimliği kendi hizmet yöneticinizi kullanmaya benzer şekilde doğrudan kullanabilirsiniz. Bu, belirlenmiş fabrikanın Blob depolamanıza/verilerine erişmesine ve kopyalanmasına olanak tanır.

Genel olarak Azure Depolama kimlik doğrulaması için [Azure Etkin Dizinini kullanarak Azure Depolama'ya kimlik doğrulama](../storage/common/storage-auth-aad.md) erişimine bakın. Azure kaynakları kimlik doğrulaması için yönetilen kimlikleri kullanmak için aşağıdaki adımları izleyin:

1. Fabrikanızla birlikte oluşturulan **yönetilen kimlik nesnesi kimliğinin** değerini kopyalayarak [veri fabrikası tarafından yönetilen kimlik bilgilerini alın.](data-factory-service-identity.md#retrieve-managed-identity)

2. Azure Blob depolama alanında yönetilen kimlik doğru iznini ver. Roller hakkında daha fazla ayrıntı içeren [RBAC ile Azure Depolama verilerine erişim haklarını yönet'e](../storage/common/storage-auth-aad-rbac.md) bakın.

    - **Kaynak olarak,** Access denetiminde (IAM), en azından **Depolama Blob Veri Okuyucu** rolünü verir.
    - **Lavabo olarak**, Access denetiminde (IAM), en azından **Depolama Blob Veri Katılımcısı** rolünü verin.

>[!IMPORTANT]
>Blob'dan (kaynak veya evreleme olarak) verileri SQL Veri Ambarı'na yüklemek için PolyBase kullanıyorsanız, Blob için yönetilen kimlik kimlik doğrulamasını kullanırken, [bu kılavuzdaki](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) 1 ve 2 adımlarını da takip ettiğinizden emin olun 1) SQL Veritabanı sunucunuzu Azure Active Directory (Azure AD) ile kaydedin ve 2) Depolama Blob Veri Katılımcısı rolünü SQL Veritabanı sunucunuza atayın; geri kalanı Veri Fabrikası tarafından işlenir. Blob depolama alanınız, ondan veri yüklemek için PolyBase'i kullanmak üzere bir Azure Sanal Ağ bitiş noktasıyla yapılandırıldıysa, PolyBase tarafından gerekli olduğu şekilde yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir.

Bu özellikler, Azure Blob depolamasına bağlı bir hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureBlobStorage**olarak ayarlanmalıdır. |Evet |
| Serviceendpoint | Azure Blob depolama hizmeti bitiş noktasını `https://<accountName>.blob.core.windows.net/`' deseni ile belirtin. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağdaysa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

> [!NOTE]
> Azure kaynakları kimlik doğrulaması için yönetilen kimlikler yalnızca "AzureBlobStorage" türüne bağlı hizmet tarafından desteklenir, ancak önceki "AzureStorage" türüne bağlı hizmet tarafından desteklenmez. 

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı veri `location` kümesinde ayarlar altında Azure Blob için desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesindeki konumun tür özelliği **AzureBlobStorageLocation**olarak ayarlanmalıdır. | Evet      |
| kapsayıcı  | Blob kabı.                                          | Evet      |
| folderPath | Verilen kapsayıcının altındaki klasöre giden yol. Klasöre filtre açmak için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |
| fileName   | Verilen kapsayıcı + folderPath altında dosya adı. Dosyaları filtrelemek için joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarlarını belirtin. | Hayır       |

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Blob depolama kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="blob-storage-as-a-source-type"></a>Kaynak türü olarak blob depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` kaynağındaki ayarlar altında Azure Blob için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureBlobStorageReadSettings**olarak ayarlanmalıdır. | Evet                                           |
| Özyinelemeli                | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın. İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır                                            |
| Önek                   | Kaynak lekeleri filtrelemek için veri kümesinde yapılandırılan verilen kapsayıcının altındaki blob adı için önek. Adı bu önek ile başlayan blobs seçilir. <br>Yalnızca özellikler `wildcardFolderPath` belirtilmediğinde geçerlidir. `wildcardFileName` | Hayır                                                          |
| joker KarakterFolderPath       | Kaynak klasörlerine filtre uygulayacak şekilde veri kümesinde yapılandırılan verilen kapsayıcının altındaki joker karakteriçeren klasör yolu. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br>Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Hayır                                            |
| jokerKarakterFileName         | Kaynak dosyaları filtrelemek için verilen kapsayıcı + folderPath/jokerKarakterFolderPath altında joker karakter içeren dosya adı. <br>İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.  Klasör ve [dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. | Veri `fileName` kümesinde belirtilmemişse evet |
| modifiyeDatetimeBaşlat    | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine dosya özniteliği filtresi uygulanamayacağı anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdakiyle aynı.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parke/sınırlı metin biçimi için, bir sonraki bölümde belirtilen **BlobSource** türü kopyalama etkinlik kaynağı geriye dönük uyumluluk için olduğu gibi desteklenir. İleriye dönük bu yeni modeli kullanmanız önerilir ve ADF yazarlı UI bu yeni türleri oluşturmaya geçti.

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

### <a name="blob-storage-as-a-sink-type"></a>Lavabo türü olarak blob depolama

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Aşağıdaki özellikler, biçim tabanlı kopyalama `storeSettings` lavabosunda ayarlar altında Azure Blob için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Altındaki tür `storeSettings` özelliği **AzureBlobStorageWriteSettings**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya veya blob adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır       |
| blockSizeInMB | Blobs engellemek için veri yazmak için kullanılan MB blok boyutunu belirtin. [Blobs Engelle hakkında](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)daha fazla bilgi edinin. <br/>İzin verilen değer **4 ile 100 MB arasındadır.** <br/>Varsayılan olarak, ADF kaynak mağaza türüve verilerinize bağlı olarak blok boyutunu otomatik olarak belirler. Blob'a ikili olmayan kopya için varsayılan blok boyutu en fazla 4,95 TB veriye sığacak şekilde 100 MB'dır. Verileriniz büyük olmadığında, özellikle de çalışma zaman ayarı veya performans sorunuyla sonuçlanan kötü ağla Kendi kendine barındırılan Tümleştirme Çalışma Süresi'ni kullandığınızda en iyi olmayabilir. BlockSizeInMB*50000'in verileri depolayacak kadar büyük olduğundan emin olurken, bir blok boyutunu açıkça belirtebilirsiniz, aksi takdirde kopyalama etkinliği başarısız olur. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır       |

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
                    "type": "AzureBlobStorageWriteSettings",
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
| `container/Folder*` | (boş, varsayılan kullanın) | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `container/Folder*` | (boş, varsayılan kullanın) | true | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `container/Folder*` | `*.csv` | yanlış | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |
| `container/Folder*` | `*.csv` | true | kapsayıcı<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Dosya5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Bazı özyinelemeli ve copyBehavior örnekleri

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için Kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| Özyinelemeli | copyBehavior | Kaynak klasör yapısı | Elde çıkan hedef |
|:--- |:--- |:--- |:--- |
| true |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 kaynakla aynı yapıyla oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 |
| true |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File3 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulmuş ad |
| true |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 içeriği otomatik olarak oluşturulmuş bir dosya adı ile bir dosyada birleştirilir. |
| yanlış |korumaHiyerarşisi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |flattenHiyerarşi | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapıile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 için otomatik olarak oluşturulmuş ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |
| yanlış |birleştirmeDosyaları | Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Alt klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya5 | Hedef klasör Folder1 aşağıdaki yapı ile oluşturulur<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 içeriği otomatik olarak oluşturulmuş bir dosya adı ile bir dosyada birleştirilir. File1 için otomatik olarak oluşturulmuş ad<br/><br/>Dosya3, File4 ve File5 ile alt klasör1 alınmaz. |

## <a name="preserve-metadata-during-copy"></a>Kopyalama sırasında meta verileri koruma

Dosyaları Amazon S3/Azure Blob/Azure Veri Gölü Depolama Gen2'den Azure Veri Gölü Depolama Gen2/Azure Blob'a kopyalarken, dosya meta verilerini verilerle birlikte korumayı seçebilirsiniz. [Meta verileri koru](copy-activity-preserve-metadata.md#preserve-metadata)adresinden daha fazla bilgi edinin.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede verileri dönüştürürken, JSON, Avro, Sınırlı Metin veya Parke biçimindeAzure Blob Depolama'dan dosyaları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için, eşleme veri akışı özelliğinde [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Kaynak dönüşümünde, Azure Blob Depolama'daki bir kapsayıcı, klasör veya tek tek dosyadan okuyabilirsiniz. **Kaynak seçenekleri** sekmesi, dosyaların nasıl okunduğunu yönetmenize olanak tanır. 

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

Lavabo dönüşümünde, Azure Blob Depolama'daki bir kapsayıcıya veya klasöre yazabilirsiniz. **Ayarlar** sekmesi, dosyaların nasıl yazıldığını yönetmenize olanak tanır.

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
| type | Veri kümesinin tür özelliği **AzureBlob**olarak ayarlanmalıdır. |Evet |
| folderPath | Blob depolama sındaki kapsayıcı ve klasöre giden yol. <br/><br/>Joker karakter filtresi, kapsayıcı adı hariç yol için desteklenir. İzin verilen joker `*` karakterler şunlardır: (sıfır `?` veya daha fazla karakterle eşleşir) ve (sıfır veya tek karakterle eşleşir); gerçek `^` klasör adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın. <br/><br/>Örnekler: myblobcontainer/myblobfolder/, Klasör [ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görün. |Kopyalama/Arama etkinliği için Evet, GetMetadata etkinliği için Hayır |
| fileName | Belirtilen "folderPath" altındaki blob(lar) için **ad veya joker karakter filtresi.** Bu özellik için bir değer belirtmezseniz, veri kümesi klasördeki tüm lekeleri işaret eder. <br/><br/>Filtre için izin verilen `*` joker karakterler şunlardır: (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>- Örnek 1:`"fileName": "*.csv"`<br/>- Örnek 2:`"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızı joker veya bu kaçış char içinde varsa kaçmak için kullanın.<br/><br/>Bir çıktı veri kümesi için dosya Adı belirtilmediğinde ve **korumaHiyerarşisi** etkinlik lavabosunda belirtilmediğinde, kopyalama etkinliği otomatik olarak aşağıdaki desenle blob adını oluşturur: "*Data.[ etkinlik çalıştırın ID GUID]. [GUID if FlattenHiyerarşiy]. [yapılandırılırsa biçimlendirme]. [yapılandırılırsa sıkıştırma]*"örneğin "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz"; sorgu yerine tablo adını kullanarak tablo kaynağından kopyalarsanız, ad deseni "*[tablo adı].[ biçimlendirin]. [yapılandırılırsa sıkıştırma]*", örneğin "MyTable.csv". |Hayır |
| modifiyeDatetimeBaşlat | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| modifiedDatetimeEnd | Dosyalar özniteliğe göre filtre: Son Değiştirildi. Son değiştirilen zamanları ile `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığında ysa, dosyalar seçilecektir. Bu süre UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Büyük miktarda dosya filtresi yapmak istediğinizde bu ayarı etkinleştirerek veri hareketinin genel performansının etkileneceğini unutmayın. <br/><br/> Özellikler, veri kümesine dosya özniteliği filtresi uygulanmayacak anlamına gelen NULL olabilir.  Datetime değeri ne zaman, `modifiedDatetimeStart` ancak `modifiedDatetimeEnd` NULL olduğunda, son değiştirilen özniteliği datetime değerinden büyük veya eşit olan dosyaların seçileceği anlamına gelir.  Datetime değeri ne zaman, `modifiedDatetimeEnd` ancak `modifiedDatetimeStart` NULL olduğunda, son değiştirilen özniteliği datetime değerinden daha az olan dosyaların seçileceği anlamına gelir.| Hayır |
| biçim | Dosyaları dosya tabanlı mağazalar (ikili kopya) arasında olduğu gibi kopyalamak istiyorsanız, hem giriş hem de çıktı veri kümesi tanımlarında biçim bölümünü atlayın.<br/><br/>Belirli bir biçimde dosyaları ayrışdırmak veya oluşturmak istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, ve **ParkquetFormat**. **Biçim** altındaki **tür** özelliğini bu değerlerden birine ayarlayın. Daha fazla bilgi için [Metin biçimi,](supported-file-formats-and-compression-codecs-legacy.md#text-format) [JSON biçimi,](supported-file-formats-and-compression-codecs-legacy.md#json-format) [Avro formatı,](supported-file-formats-and-compression-codecs-legacy.md#avro-format) [Ork formatı](supported-file-formats-and-compression-codecs-legacy.md#orc-format)ve [Parke formatında](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) bölümlere bakın. |Hayır (yalnızca ikili kopyalama senaryosu için) |
| sıkıştırma | Verilerin sıkıştırma türünü ve düzeyini belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve sıkıştırma codec'lerine](supported-file-formats-and-compression-codecs-legacy.md#compression-support)bakın.<br/>Desteklenen türleri **GZip,** **Deflate**, **BZip2**, ve **ZipDeflate**vardır.<br/>Desteklenen seviyeler **Optimal** ve **En Hızlı'dır.** |Hayır |

>[!TIP]
>Tüm blob'ları bir klasörün altında kopyalamak için **yalnızca folderPath'i** belirtin.<br>Belirli bir adla tek bir blob kopyalamak için **folderPath** klasörünü ve dosya adı içeren **dosya adı** ile birlikte belirtin.<br>Blobs alt kümesini bir klasörün altında kopyalamak için **folderPath** klasörünü klasör bölümü yle ve **fileName'yi** joker karakter filtresiile belirtin. 

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

### <a name="legacy-copy-activity-source-model"></a>Eski kopyalama etkinliği kaynak modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği **BlobSource**olarak ayarlanmalıdır. |Evet |
| Özyinelemeli | Verilerin alt klasörlerden mi yoksa yalnızca belirtilen klasörden mi özyinelemeli olarak okunduğunu gösterir. Özyineleme gerçek olarak ayarlandığında ve lavabo dosya tabanlı bir depo olduğunda, boş bir klasörün veya alt klasörün kopyalanmadığını veya lavaboda oluşturulmadığını unutmayın.<br/>İzin verilen değerler **doğru** (varsayılan) ve **yanlıştır.** | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

### <a name="legacy-copy-activity-sink-model"></a>Eski kopyalama etkinliği lavabo modeli

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabotürü özelliği **BlobSink**olarak ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosya tabanlı bir veri deposundan dosya olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>- Koruma Hiyerarşisi (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasörüne göreli yolu hedef klasöre hedef dosyanın göreli yolu ile aynıdır.<br/><b>- FlattenHiyerarşiy</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyaların otomatik olarak oluşturulmuş adları vardır. <br/><b>- Birleştirme Dosyaları</b>: Kaynak klasördeki tüm dosyaları tek bir dosyayla birleştirir. Dosya veya blob adı belirtilirse, birleştirilen dosya adı belirtilen addır. Aksi takdirde, otomatik olarak oluşturulmuş bir dosya adıdır. | Hayır |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantıların sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlamak istediğinizde belirtin. | Hayır |

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

## <a name="next-steps"></a>Sonraki adımlar

Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
