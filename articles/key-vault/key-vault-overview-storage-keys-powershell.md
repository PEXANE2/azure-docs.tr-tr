---
title: Azure Key Vault yönetilen depolama hesabı-PowerShell sürümü
description: Yönetilen depolama hesabı özelliği, Azure Key Vault ile Azure depolama hesabı arasında çok daha az bir tümleştirme sağlar.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 530f38289afb3fce85bbb025e7b699862eedbadc
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382689"
---
# <a name="azure-key-vault-managed-storage-account---powershell"></a>Azure Key Vault yönetilen depolama hesabı-PowerShell

> [!NOTE]
> [Azure Active Directory (Azure AD) Ile Azure depolama tümleştirmesi şimdi önizlemededir](../storage/common/storage-auth-aad.md). Azure Key Vault gibi Azure depolama 'ya OAuth2 belirteç tabanlı erişim sağlayan kimlik doğrulama ve yetkilendirme için Azure AD kullanmanızı öneririz. Bu sayede şunları yapabilirsiniz:
> - Depolama hesabı kimlik bilgileri yerine bir uygulama veya Kullanıcı kimliği kullanarak istemci uygulamanızın kimliğini doğrulayın. 
> - Azure 'da çalışırken bir [Azure ad yönetilen kimliği](/azure/active-directory/managed-identities-azure-resources/) kullanın. Yönetilen kimlikler, istemci kimlik doğrulaması gereksinimini tamamen kaldırır ve uygulamanızda veya uygulamanızdaki kimlik bilgilerini depolar.
> - Key Vault tarafından da desteklenen yetkilendirmeyi yönetmek için rol tabanlı Access Control (RBAC) kullanın.
> - Depolama hesabına AAD erişimi, henüz olduğu gibi tablolara erişmek için çalışmaz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure depolama hesabı](/azure/storage/storage-create-storage-account) , hesap adı ve anahtar içeren bir kimlik bilgisi kullanır. Anahtar otomatik olarak oluşturulur ve şifreleme anahtarının aksine "parola" olarak daha fazla hizmet verir. Key Vault, bu depolama hesabı anahtarlarını [Key Vault gizli](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets)dizileri olarak depolayarak yönetebilir. 

## <a name="overview"></a>Genel Bakış

Key Vault yönetilen depolama hesabı özelliği sizin adınıza çeşitli yönetim işlevleri gerçekleştirir:

- Bir Azure depolama hesabı ile anahtarları (eşitlemeler) listeler.
- Anahtarları düzenli olarak yeniden oluşturur (döndürür).
- Hem depolama hesapları hem de klasik depolama hesapları için anahtarları yönetir.
- Anahtar değerleri, çağırana yanıt olarak hiçbir şekilde döndürülmez.

Yönetilen depolama hesabı anahtar özelliğini kullandığınızda:

- **Depolama hesabı anahtarlarınızı yalnızca Key Vault yönetmesine izin verin.** Key Vault süreçleriyle karışabileceksiniz, bunları kendiniz yönetmeye çalışmayın.
- **Depolama hesabı anahtarlarının birden fazla Key Vault nesnesi tarafından yönetilmesine izin verme**.
- **Depolama hesabı anahtarlarınızı el ile yeniden üretme**. Key Vault aracılığıyla yeniden oluşturmanız önerilir.

Aşağıdaki örnek, depolama hesabı anahtarlarınızı Key Vault nasıl yönetebileceğinizi gösterir.

## <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) cmdlet 'Ini kullanarak PowerShell oturumunuzun kimliğini doğrulayın. 
```azurepowershell-interactive
Connect-AzAccount
```
Birden çok Azure aboneliğiniz varsa [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0) cmdlet 'ini kullanarak bunları listeleyebilir ve [set-azcontext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) cmdlet 'i ile kullanmak istediğiniz aboneliği belirtebilirsiniz. 

```azurepowershell-interactive
Set-AzContext -SubscriptionId <subscriptionId>
```

## <a name="authorize-key-vault-to-access-to-your-storage-account"></a>Depolama hesabınıza erişmek için Key Vault yetkilendirme

> [!IMPORTANT]
> Bir Azure AD kiracısı, kayıtlı her uygulamayı bir **[hizmet sorumlusu](/azure/active-directory/develop/developer-glossary#service-principal-object)** sağlar ve bu da uygulamanın kimliği olarak işlev görür. Hizmet sorumlusunun uygulama KIMLIĞI, rol tabanlı erişim denetimi (RBAC) aracılığıyla diğer Azure kaynaklarına erişim yetkisi verirken kullanılır. Key Vault bir Microsoft uygulaması olduğundan, her bir Azure bulutu dahilinde aynı uygulama KIMLIĞI altındaki tüm Azure AD kiracılarına önceden kaydedilir:
> - Azure Kamu bulutundaki Azure AD kiracılar uygulama KIMLIĞINI `7e7c393b-45d0-48b1-a35e-2905ddf8183c`kullanır.
> - Azure genel bulutundaki Azure AD kiracılar ve tüm diğerleri uygulama KIMLIĞINI `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`kullanır.

Key Vault depolama hesabı Anahtarlarınıza erişip yönetebilmeniz için depolama hesabınıza erişim yetkisi vermelisiniz. Key Vault uygulama, depolama hesabınıza yönelik anahtarları *listelemek* ve yeniden *oluşturmak* için izinler gerektirir. Bu izinler, yerleşik RBAC rol [depolama hesabı anahtar operatörü hizmeti rolü](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role)aracılığıyla etkinleştirilir. 

Aşağıdaki adımları kullanarak bu rolü, kapsamı depolama hesabınızla sınırlayan Key Vault hizmet sorumlusuna atayın. Betiği çalıştırmadan önce,, `$resourceGroupName`ve `$storageAccountName` `$keyVaultName` değişkenlerini `$storageAccountKey`güncelleştirdiğinizden emin olun:

```azurepowershell-interactive
# TODO: Update with the resource group where your storage account resides, your storage account name, the name of your active storage account key, and your Key Vault instance name
$resourceGroupName = "rgContoso"
$storageAccountName = "sacontoso"
$storageAccountKey = "key1"
$keyVaultName = "kvContoso"
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093" # See "IMPORTANT" block above for information on Key Vault Application IDs

# Get your User Id for later commands
$userId = (Get-AzContext).Account.Id

# Get a reference to your Azure storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName

# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Başarılı rol atamasından sonra, aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : storage account Key Operator Service Role
RoleDefinitionId   : 81a9662b-bebf-436f-a333-f67b29880f12
ObjectId           : 93c27d83-f79b-4cb2-8dd4-4aa716542e74
ObjectType         : ServicePrincipal
CanDelegate        : False
```

Depolama hesabınızdaki role Key Vault zaten eklendiyse *"rol ataması zaten var."* hatayla. Rol atamasını, Azure portal depolama hesabı "erişim denetimi (ıAM)" sayfasını kullanarak da doğrulayabilirsiniz.  

## <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Yönetilen depolama hesaplarına kullanıcı hesabınıza izin verin

>[!TIP] 
> Azure AD, uygulamanın kimliği için bir **hizmet sorumlusu** sağladığından, kullanıcının kimliği için bir **Kullanıcı sorumlusu** sağlanır. Daha sonra Kullanıcı sorumlusuna, Key Vault erişim ilkesi izinleri aracılığıyla Key Vault erişim yetkisi verilebilir.

Aynı PowerShell oturumunu kullanarak, yönetilen depolama hesapları için Key Vault erişim ilkesini güncelleştirin. Bu adım, Kullanıcı hesabınıza, yönetilen depolama hesabı özelliklerine erişebildiğinizden emin olmak için depolama hesabı izinlerini uygular: 

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userId -PermissionsToStorage get, list, delete, set, update, regeneratekey, getsas, listsas, deletesas, setsas, recover, backup, restore, purge
```

Depolama hesapları için izinler, Azure portal depolama hesabı "erişim ilkeleri" sayfasında kullanılamaz.

## <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Key Vault örneğinize yönetilen bir depolama hesabı ekleyin

Aynı PowerShell oturumunu kullanarak, Key Vault Örneğinizde yönetilen bir depolama hesabı oluşturun. `-DisableAutoRegenerateKey` Anahtar, depolama hesabı anahtarlarını yeniden üretmemelidir.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Anahtar yeniden oluşturma olmadan depolama hesabının başarılı bir şekilde eklenmesiyle, aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : False
Regeneration Period : 90.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

### <a name="enable-key-regeneration"></a>Anahtar yeniden oluşturmayı etkinleştir

Depolama hesabı anahtarlarınızı düzenli aralıklarla yeniden oluşturmak Key Vault isterseniz, yeniden oluşturma dönemi ayarlayabilirsiniz. Aşağıdaki örnekte, üç günün yeniden oluşturma dönemini ayarlayacağız. Üç gün sonra, Key Vault ' key2 ' öğesini yeniden oluşturacak ve etkin anahtarı ' key2 ' iken ' KEY1 ' olarak takas edecek.

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Anahtar yeniden oluşturma ile depolama hesabının başarılı bir şekilde eklenmesiyle, aşağıdaki örneğe benzer bir çıktı görmeniz gerekir:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : True
Regeneration Period : 3.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
- [PowerShell başvurusunu Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
