---
title: Otomasyon 'da güvenli varlıkların şifrelenmesi
description: Azure Otomasyonu, birden çok şifreleme düzeyi kullanarak güvenli varlıkları korur. Varsayılan olarak, şifreleme Microsoft tarafından yönetilen anahtarlar kullanılarak yapılır. Müşteriler, Otomasyon hesaplarını, şifreleme için müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırabilir. Bu makalede her iki şifreleme modunun ayrıntıları ve iki arasında nasıl geçiş yapabilirsiniz açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: snehithm
ms.author: snmuvva
ms.date: 01/11/2020
ms.topic: conceptual
manager: kmadnani
ms.openlocfilehash: fa8ea40d827807565e71d1e790c8c52986b85ec8
ms.sourcegitcommit: d48afd9a09f850b230709826d4a5cd46e57d19fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75904960"
---
# <a name="secure-assets-in-azure-automation"></a>Azure Otomasyonu 'nda varlıkları güvenli hale getirme

Azure Otomasyonu 'nda güvenli varlıklar, kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar, birden çok şifreleme düzeyi kullanılarak Azure Otomasyonu 'nda korunur. Şifreleme için kullanılan en üst düzey anahtara bağlı olarak, şifreleme için iki model vardır:
-   Microsoft tarafından yönetilen anahtarları kullanma
-   Müşteri tarafından yönetilen anahtarları kullanma

### <a name="microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlar

Varsayılan olarak, Azure Otomasyonu hesabınız Microsoft tarafından yönetilen anahtarları kullanır.

Her bir güvenli varlık şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar (veri şifreleme anahtarı) kullanılarak Azure Otomasyonu 'nda depolanır. Bu anahtarların kendisi, bir hesap şifreleme anahtarı (AEK) olarak adlandırılan her bir hesap için oluşturulan başka bir benzersiz anahtar kullanılarak Azure Otomasyonu 'nda şifrelenir ve depolanır. Microsoft tarafından yönetilen anahtarlar kullanılarak bu hesap şifreleme anahtarları şifrelenir ve Azure Otomasyonu 'nda depolanır. 

### <a name="customer-managed-keys-with-key-vault-preview"></a>Key Vault ile müşteri tarafından yönetilen anahtarlar (Önizleme)

Azure Automation 'da güvenli varlıkların şifrelemesini, kendi anahtarlarınızla bir Otomasyon hesabı düzeyinde yönetebilirsiniz. Otomasyon hesabı düzeyinde müşteri tarafından yönetilen bir anahtar belirttiğinizde, bu anahtar, Otomasyon hesabının hesap şifreleme anahtarına erişimi korumak ve denetlemek için kullanılır ve bu, tüm güvenli varlıkları şifrelemek ve şifresini çözmek için kullanılır. Müşteri tarafından yönetilen anahtarlar, erişim denetimlerini oluşturma, döndürme, devre dışı bırakma ve iptal etme için daha fazla esneklik sunar. Ayrıca, güvenli varlıklarınızı korumak için kullanılan şifreleme anahtarlarını da denetleyebilirsiniz. 

Müşteri tarafından yönetilen anahtarları depolamak için Azure Key Vault kullanmanız gerekir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz.  Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../key-vault/key-vault-overview.md)

## <a name="enable-customer-managed-keys-for-an-automation-account"></a>Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirin

Otomasyon hesabı için müşteri tarafından yönetilen anahtarlarla şifrelemeyi etkinleştirdiğinizde Azure Otomasyonu, hesap şifreleme anahtarını ilişkili anahtar kasasındaki müşteri tarafından yönetilen anahtarla sarar. Müşteri tarafından yönetilen anahtarların etkinleştirilmesi performansı etkilemez ve hesap, her zaman gecikmesi olmadan yeni anahtarla şifrelenir.

Yeni bir Otomasyon hesabı her zaman Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Hesap oluşturulduğu sırada müşteri tarafından yönetilen anahtarları etkinleştirmek mümkün değildir. Müşteri tarafından yönetilen anahtarlar Azure Key Vault depolanır ve Anahtar Kasası, Otomasyon hesabıyla ilişkili yönetilen kimliğe anahtar izinleri veren erişim ilkeleriyle sağlanmalıdır. Yönetilen kimlik yalnızca depolama hesabı oluşturulduktan sonra kullanılabilir.

Müşteri tarafından yönetilen anahtarları etkinleştirerek veya devre dışı bırakarak, anahtar sürümünü güncelleştirerek veya farklı bir anahtar belirterek, Azure Otomasyonu güvenli varlık şifrelemesi için kullanılan anahtarı değiştirdiğinizde, kök anahtar şifrelemesi değişir ancak Azure 'daki güvenli varlıklar Otomasyon hesabının yeniden şifrelenmesi gerekmez.

Aşağıdaki üç bölümde, bir Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirme mekanizması açıklanır. 

> [!NOTE] 
> Müşteri tarafından yönetilen anahtarları etkinleştirmek için şu anda API sürüm 2020-01-13-önizleme kullanarak Azure Otomasyonu REST API 'Leri yapmanız gerekir.

### <a name="pre-requisites-for-using-customer-managed-keys-in-azure-automation"></a>Azure Otomasyonu 'nda müşteri tarafından yönetilen anahtarları kullanma önkoşulları

Bir Otomasyon hesabı için müşteri tarafından yönetilen anahtarları etkinleştirmeden önce, aşağıdaki önkoşulların karşılandığından emin olmanız gerekir

 - Müşteri tarafından yönetilen anahtar, bir Azure Key Vault depolanır. 
 - Anahtar kasasında hem **geçici silme** hem de **Temizleme** özelliklerini etkinleştirmeniz gerekir. Bu özellikler, yanlışlıkla silme durumunda anahtarların kurtarılmasına izin vermek için gereklidir.
 - Yalnızca RSA anahtarları Azure Otomasyonu şifrelemesi ile desteklenir. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).
- Otomasyon hesabı ve Anahtar Kasası farklı aboneliklerde olabilir, ancak aynı Azure Active Directory kiracısında olması gerekir.

### <a name="assign-an-identity-to-the-automation-account"></a>Otomasyon hesabına bir kimlik atayın

Müşteri tarafından yönetilen anahtarları bir Otomasyon hesabıyla birlikte kullanmak için Otomasyon hesabınızın, müşteri tarafından yönetilen anahtarları depolayan anahtar kasasında kimlik doğrulaması yapması gerekir. Azure Otomasyonu, Key Vault hesabın kimliğini doğrulamak için sistem tarafından atanmış Yönetilen kimlikler kullanır. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Aşağıdaki REST API çağrısını kullanarak, sistem tarafından atanmış bir yönetilen kimliği Otomasyon hesabına yapılandırın

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```
İstek gövdesi
```json
{ 
 "identity": 
 { 
  "type": "SystemAssigned" 
  } 
}
```    

Otomasyon hesabı için sistem tarafından atanan kimlik yanıt olarak döndürüldü

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

Otomasyon hesabına yönetilen bir kimlik atandıktan sonra, müşteri tarafından yönetilen anahtarları depolayan Key Vault erişimi yapılandırırsınız. Azure Otomasyonu, müşteri tarafından yönetilen anahtarlar üzerinde **Get**, **Recover**, **wrapKey**, **UnwrapKey** gerektirir.

Bu tür bir erişim ilkesi aşağıdaki REST API çağrısı kullanılarak ayarlanabilir.

```http
PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sample-group/providers/Microsoft.KeyVault/vaults/sample-vault/accessPolicies/add?api-version=2018-02-14
```
İstek gövdesi

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
> Tenantıd ve objectID alanları, Otomasyon hesabı için yönetilen kimliğin yanıtından Identity. Tenantıd ve Identity. PrincipalId değerleri ile sağlanmalıdır.

### <a name="change-the-configuration-of-automation-account-to-use-customer-managed-key"></a>Otomasyon hesabının yapılandırmasını, müşteri tarafından yönetilen anahtarı kullanacak şekilde değiştirin

Son olarak, aşağıdaki REST API çağrısını kullanarak Otomasyon hesabınızı Microsft tarafından yönetilen anahtarlardan müşteri tarafından yönetilen anahtarlara geçirebilirsiniz.

```http
PATCH https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Automation/automationAccounts/automation-account-name?api-version=2020-01-13-preview
```
İstek gövdesi

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

## <a name="manage-customer-managed-keys-lifecycle"></a>Müşteri tarafından yönetilen anahtarların yaşam döngüsünü yönetme

### <a name="rotate-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları döndür

Azure Key Vault, müşteri tarafından yönetilen bir anahtarı uyumluluk ilkelerinize göre döndürebilirsiniz. Anahtar döndürüldüğünde, Otomasyon hesabını yeni anahtar URI 'sini kullanacak şekilde güncelleştirmeniz gerekir. 

Anahtarın döndürülmesi, depolama hesabındaki verilerin yeniden şifrelenmesini tetiklemez. Kullanıcıdan başka bir eylem gerekli değildir.

## <a name="revoke-access-to-customer-managed-keys"></a>Müşterinin yönettiği anahtarlara erişimi iptal etme

Müşteri tarafından yönetilen anahtarlara erişimi iptal etmek için PowerShell veya Azure CLı kullanın. Daha fazla bilgi için bkz. PowerShell veya [Azure Key Vault clı](https://docs.microsoft.com/cli/azure/keyvault) [Azure Key Vault](https://docs.microsoft.com/powershell/module/az.keyvault/) . Erişimi iptal etmek, şifreleme anahtarına Azure depolama tarafından erişilemediğinden, depolama hesabındaki tüm verilere erişimi etkin bir şekilde engeller.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault nedir?](../key-vault/key-vault-overview.md) 
- [Azure Otomasyonu'ndaki sertifika varlıkları](shared-resources/certificates.md)
- [Azure Otomasyonu'ndaki kimlik bilgileri varlıkları](shared-resources/credentials.md)
- [Azure Otomasyonu'ndaki değişken varlıkları](shared-resources/variables.md)
