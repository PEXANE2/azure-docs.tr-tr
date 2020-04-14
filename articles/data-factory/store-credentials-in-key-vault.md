---
title: Kimlik bilgilerini Azure Key Vault’ta depolama
description: Azure Veri Fabrikası'nın çalışma zamanında otomatik olarak alabilecekleri Bir Azure anahtar kasasında kullanılan veri depoları için kimlik bilgilerini nasıl depolayabileceğinizi öğrenin.
services: data-factory
author: linda33wj
manager: shwang
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: jingwang
ms.openlocfilehash: 6720a018cdc3fff95192b0956b3d1040be263ab2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261904"
---
# <a name="store-credential-in-azure-key-vault"></a>Azure Anahtar Kasası'nda mağaza kimlik bilgisi

Veri depoları ve bilgi işlem kimlik bilgilerini Azure [Anahtar Kasası'nda](../key-vault/key-vault-overview.md)depolayabilirsiniz. Azure Veri Fabrikası, veri deposu/bilgi işlem'ini kullanan bir etkinliği yürütürken kimlik bilgilerini alır.

Şu anda, özel etkinlik dışındaki tüm etkinlik türleri bu özelliği destekler. Özellikle bağlayıcı yapılandırması için, ayrıntılar için [her bağlayıcı konusundaki](copy-activity-overview.md#supported-data-stores-and-formats) "bağlantılı hizmet özellikleri" bölümünü kontrol edin.

## <a name="prerequisites"></a>Ön koşullar

Bu özellik, veri fabrikası yönetilen kimliğine dayanır. [Veri fabrikası için Yönetilen kimlikten](data-factory-service-identity.md) nasıl çalıştığını öğrenin ve veri fabrikanızın ilişkili bir kimliği olduğundan emin olun.

## <a name="steps"></a>Adımlar

Azure Key Vault'ta depolanan bir kimlik bilgisine başvurmak için şunları yapmanız gerekir:

1. Fabrikanızla birlikte oluşturulan "Yönetilen Kimlik Nesnekimliği" değerini kopyalayarak **veri fabrikası yönetilen kimliğini alın.** ADF yazarlı UI kullanıyorsanız, yönetilen kimlik nesnesi kimliği Azure Key Vault bağlantılı hizmet oluşturma penceresinde gösterilir; Azure portalından da alabilirsiniz, [veri fabrikası tarafından yönetilen kimliği al'a](data-factory-service-identity.md#retrieve-managed-identity)bakın.
2. **Azure Anahtar Kasanıza yönetilen kimlik erişimi izni ni tanıyın.** Anahtar kasa -> Erişim ilkelerinizde -> Erişim Ekle İlkesi'nde, Gizli izinler açılır durumda izin **almak** için bu yönetilen kimliği arayın. Bu belirlenmiş fabrikanın anahtar kasasında ki gizli lere erişmesine izin veriyor.
3. **Azure Anahtar Kasanızı gösteren bağlantılı bir hizmet oluşturun.** Azure [Key Vault bağlantılı hizmetine](#azure-key-vault-linked-service)bakın.
4. **Anahtar kasasında saklanan ilgili gizli referans içinde veri deposu bağlantılı hizmet oluşturun.** Anahtar [kasasında saklanan referans gizli](#reference-secret-stored-in-key-vault)bakın.

## <a name="azure-key-vault-linked-service"></a>Azure Key Vault bağlantılı hizmeti

Azure Key Vault bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **AzureKeyVault**. | Evet |
| baseUrl | Azure Anahtar Kasası URL'sini belirtin. | Evet |

**Yazma UI kullanma:**

**Bağlantıları** -> **Bağlantılı Hizmetler** -> **Yeni**seçin. Yeni bağlantılı hizmette "Azure Anahtar Kasası"nı arayın ve seçin:

![Azure Anahtar Kasası'nda Arama](media/store-credentials-in-key-vault/search-akv.png)

Kimlik bilgilerinizin depolandığı sağlanan Azure Anahtar Kasası'nı seçin. AKV bağlantınızın geçerli olduğundan emin olmak için **Test Bağlantısı** yapabilirsiniz. 

![Azure Key Vault'u yapılandırma](media/store-credentials-in-key-vault/configure-akv.png)

**JSON örneği:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Anahtar kasasında depolanan gizli diziye başvurma

Bağlı hizmetteki bir alanı önemli bir kasa sırrına atıfta bulunarak yapılandırdığınızda aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Alanın tür özelliği şu şekilde ayarlanmalıdır: **AzureKeyVaultSecret**. | Evet |
| secretName | Azure Key Vault'ta gizli adı. | Evet |
| secretVersion | Azure Key Vault'ta gizli sürüm.<br/>Belirtilmemişse, her zaman sırrın en son sürümünü kullanır.<br/>Belirtilmişse, verilen sürüme yapışır.| Hayır |
| mağaza | Kimlik belgesini depolamak için kullandığınız Azure Anahtar Kasası bağlantılı bir hizmeti ifade eder. | Evet |

**Yazma UI kullanma:**

Veri deponuza/bilgi işlemnize bağlantı oluştururken gizli alanlar için **Azure Anahtar Kasası'nı** seçin. Sağlanan Azure Anahtar Kasası Bağlantılı Hizmeti seçin ve **Gizli adı**sağlayın. İsteğe bağlı olarak gizli bir sürüm de sağlayabilirsiniz. 

>[!TIP]
>SQL Server, Blob depolama, vb. gibi bağlantılı hizmette bağlantı dizesini kullanan bağlayıcılar için, parola gibi yalnızca gizli alanı AKV'de depolamayı veya tüm bağlantı dizesini AKV'de depolamayı seçebilirsiniz. Her iki seçeneği de UI'de bulabilirsiniz.

![Azure Anahtar Kasası sırrını yapılandırma](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON örneği: (bkz. "şifre" bölümü)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
