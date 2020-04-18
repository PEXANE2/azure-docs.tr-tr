---
title: Azure Otomasyonu’nda güvenli varlıkları şifreleme
description: Azure Otomasyonu, birden çok şifreleme düzeyi kullanarak güvenli varlıkları korur. Varsayılan olarak, şifreleme Microsoft tarafından yönetilen anahtarlar kullanılarak yapılır. Müşteriler otomasyon hesaplarını şifreleme için müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırabilir. Bu makalede, her iki şifreleme modunun ayrıntıları ve ikisi arasında nasıl geçiş yapabileceğiniz açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: 594bac257c2b9739f1ece276c881348b35d2f704
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604808"
---
# <a name="encrypt-secure-assets-in-azure-automation"></a>Azure Otomasyonu’nda güvenli varlıkları şifreleme

Azure Otomasyonu'ndaki güvenli varlıklar kimlik bilgilerini, sertifikaları, bağlantıları ve şifreli değişkenleri içerir. Bu varlıklar Azure Otomasyonu'nda birden çok şifreleme düzeyi kullanılarak korunur. Şifreleme için kullanılan üst düzey anahtara göre şifreleme için iki model vardır:
-    Microsoft tarafından yönetilen anahtarları kullanma
-    Müşteri tarafından yönetilen anahtarları kullanma

## <a name="microsoft-managed-keys"></a>Microsoft tarafından yönetilen Anahtarlar

Varsayılan olarak, Azure Otomasyon hesabınız Microsoft tarafından yönetilen anahtarları kullanır.

Her güvenli varlık, her otomasyon hesabı için oluşturulan benzersiz bir anahtar (Veri Şifreleme anahtarı) kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu anahtarlar, Hesap Şifreleme Anahtarı (AEK) adı verilen her hesap için oluşturulan başka bir benzersiz anahtar kullanılarak şifrelenir ve Azure Otomasyonu'nda depolanır. Bu hesap şifreleme anahtarları Microsoft tarafından yönetilen Keys kullanılarak Azure Otomasyonu'nda şifrelenir ve depolanır. 

## <a name="customer-managed-keys-with-key-vault-preview"></a>Anahtar Kasası ile Müşteri Tarafından Yönetilen Anahtarlar (önizleme)

Otomasyon hesabınız için güvenli varlıkların şifrelemesini kendi anahtarlarınızla yönetebilirsiniz. Otomasyon hesabı düzeyinde müşteri tarafından yönetilen bir anahtar belirttiğiniz zaman, bu anahtar Otomasyon hesabının hesap şifreleme anahtarını korumak ve bu anahtara erişimi denetlemek için kullanılır. Bu da tüm güvenli varlıkları şifrelemek ve şifresini çözmek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimleri oluşturmak, döndürmek, devre dışı kalmak ve iptal etmek için daha fazla esneklik sunar. Güvenli varlıklarınızı korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz.

Müşteri tarafından yönetilen anahtarları depolamak için Azure Key Vault'u kullanın. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz.  Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../key-vault/general/overview.md)

## <a name="enable-customer-managed-keys-for-an-automation-account"></a>Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirme

Bir Otomasyon hesabı için müşteri tarafından yönetilen anahtarlarla şifrelemeyi etkinleştirdiğinizde, Azure Otomasyonu hesap şifreleme anahtarını ilişkili anahtar kasasındamüşteri tarafından yönetilen anahtarla birlikte sarar. Müşteri tarafından yönetilen anahtarları etkinleştirmek performansı etkilemez ve hesap gecikmeden yeni anahtarla hemen şifrelenir.

Yeni bir Otomasyon hesabı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Hesabın oluşturulduğu anda müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Anahtar Kasası'nda depolanır ve anahtar kasası, Otomasyon hesabıyla ilişkili yönetilen kimliğe önemli izinler veren erişim ilkeleriyle birlikte sağlanmalıdır. Yönetilen kimlik yalnızca depolama hesabı oluşturulduktan sonra kullanılabilir.

Azure Automation için kullanılan anahtarı değiştirdiğinizde, müşteri tarafından yönetilen anahtarları etkinleştirerek veya devre dışı bırakarak, anahtar sürümünü güncelleştirerek veya farklı bir anahtar belirterek, hesap şifreleme anahtarının şifrelemesi değişir, ancak Azure Otomasyon hesabınızdaki güvenli varlıkların yeniden şifrelenmesi gerekmez.

Aşağıdaki üç bölüm, bir Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirme mekaniğini açıklar. 

> [!NOTE] 
> Müşteri tarafından yönetilen anahtarları etkinleştirmek için, api sürüm 2020-01-13-preview'u kullanarak Azure Automation REST API aramaları yapmanız gerekir

### <a name="pre-requisites-for-using-customer-managed-keys-in-azure-automation"></a>Azure Otomasyonu'nda Müşteri tarafından yönetilen anahtarları kullanmak için ön koşullar

Bir Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmeden önce, aşağıdaki ön koşulların karşılandığından emin olmalısınız:

 - Müşteri destekli anahtar Azure Anahtar Kasası'nda depolanır. 
 - Anahtar kasasındahem **Yumuşak Silme** hem de **Temizleme özelliklerini** etkinleştirin. Bu özellikler, yanlışlıkla silme durumunda anahtarların kurtarılmasına izin vermek için gereklidir.
 - Azure Otomasyon şifrelemesi ile yalnızca RSA anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Anahtar Kasası anahtarları, sırlar ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys)bilgi alabiliyorum.
- Otomasyon hesabı ve anahtar kasası farklı aboneliklerde olabilir, ancak aynı Azure Active Directory kiracısında olması gerekir.

### <a name="assign-an-identity-to-the-automation-account"></a>Otomasyon hesabına kimlik atama

Otomasyon hesabıyla müşteri tarafından yönetilen anahtarları kullanmak için, Otomasyon hesabınızın müşteri tarafından yönetilen anahtarları depolayan anahtar kasasına karşı kimlik doğrulaması yapması gerekir. Azure Automation, Azure Key Vault ile hesabın kimliğini doğrulamak için yönetilen kimlikler atanmış sistemi kullanır. Yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için yönetilen kimlikler nelerdir bkz.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Yönetilen kimliği Otomasyon hesabına atanan bir sistemi aşağıdaki REST API çağrısını kullanarak yapılandırın:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

İstek gövdesi:

```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```

Otomasyon hesabı için atanan sistem kimliği aşağıdakilere benzer bir yanıtla döndürülür:

```json
{
 "name": "automation-account-name",
 "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
 ..
 "identity": {
    "type": "SystemAssigned",
    "principalId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
    "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb"
 },
..
}
```

### <a name="configure-the-key-vault-access-policy"></a>Key Vault erişim ilkesini yapılandırma

Otomasyon hesabına yönetilen bir kimlik atandıktan sonra, müşteri tarafından yönetilen anahtarları depolayan anahtar kasasına erişimi yapılandırMış sınız. Azure Otomasyonu, müşteri tarafından yönetilen anahtarlarda **al,** **kurtarma**, **wrapKey**, **UnwrapKey** gerektirir.

Böyle bir erişim ilkesi aşağıdaki REST API çağrısı kullanılarak ayarlanabilir:

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```

İstek gövdesi:

```json
{
  "properties": {
    "accessPolicies": [
      {
        "tenantId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
        "objectId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
        "permissions": {
          "keys": [
            "get",
            "recover",
            "wrapKey",
            "unwrapKey"
          ],
          "secrets": [],
          "certificates": []
        }
      }
    ]
  }
}
```

> [!NOTE]
> **TenantId** ve **objectId** alanları, Otomasyon hesabı için yönetilen kimlik yanıtından sırasıyla **identity.tenantId** ve **identity.principalId** değerleri ile sağlanmalıdır.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarı kullanmak için Otomasyon hesabının yapılandırmasını değiştirme

Son olarak, aşağıdaki REST API çağrısını kullanarak Otomasyon hesabınızı Microsoft tarafından yönetilen anahtarlardan müşteri tarafından yönetilen anahtarlara geçirebilirsiniz:

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```

İstek gövdesi:

```json
 {
    "properties": {
      "encryption": {
        "keySource": "Microsoft.Keyvault",
        "keyvaultProperties": {
          "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
        }
      }
    }
  }
```

Örnek yanıt

```json
{
  "name": "automation-account-name",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name",
  ..
  "properties": {
    ..
    "encryption": {
      "keyvaultProperties": {
         "keyName": "sample-vault-key",
          "keyvaultUri": "https://sample-vault-key12.vault.azure.net",
          "keyVersion": "7c73556c521340209371eaf623cc099d"
      },
      "keySource": "Microsoft.Keyvault"
    },
    ..
  }
}
```

## <a name="manage-customer-managed-keys-lifecycle"></a>Müşteri tarafından yönetilen anahtarlar yaşam döngüsünü yönetme

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndürme

Azure Key Vault'ta müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, yeni URI anahtarını kullanmak için Otomasyon hesabını güncelleştirmeniz gerekir.

Anahtarı döndürmek, Otomasyon hesabındaki güvenli varlıkların yeniden şifresini tetiklemez. Başka bir işlem gerekli değildir.

### <a name="revoke-access-to-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLI'yi kullanın. Daha fazla bilgi için Azure [Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault/) veya [Azure Key Vault CLI'ye](https://docs.microsoft.com/cli/azure/keyvault)bakın. Şifreleme anahtarına Azure Otomasyonu tarafından erişilemeden erişilebildiğinden, erişimi iptal etmek Otomasyon hesabındaki tüm güvenli varlıklara erişimi etkin bir şekilde engeller.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası nedir?](../key-vault/general/overview.md)
- [Azure Otomasyonu'ndaki sertifika varlıkları](shared-resources/certificates.md)
- [Azure Otomasyonu'ndaki kimlik bilgileri varlıkları](shared-resources/credentials.md)
- [Azure Otomasyonu'ndaki değişken varlıkları](shared-resources/variables.md)
