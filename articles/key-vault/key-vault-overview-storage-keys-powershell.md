---
title: Azure Key Vault yönetilen depolama hesabı - PowerShell sürümü
description: Yönetilen depolama hesabı özelliği, Azure Anahtar Kasası ile Azure depolama hesabı arasında sorunsuz bir tümleştirme sağlar.
ms.topic: conceptual
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: 833f78d89a1a9033e62c10c3b16c5adfc65e1da4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78195133"
---
# <a name="manage-storage-account-keys-with-key-vault-and-azure-powershell"></a>Key Vault ve Azure PowerShell ile depolama hesabı anahtarlarını yönetme

Azure depolama hesabı, bir hesap adı ve anahtardan oluşan kimlik bilgilerini kullanır. Anahtar otomatik olarak oluşturulur ve şifreleme anahtarı olarak değil, parola görevi görededir. Key Vault, depolama hesabı anahtarlarını [Key Vault sırları](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets)olarak saklayarak yönetir. 

Anahtar Kasası yönetilen depolama hesabı anahtar özelliğini kullanarak bir Azure depolama hesabıyla anahtarları listeleyebilir (eşitleyebilir) ve anahtarları düzenli aralıklarla yeniden oluşturabilir (döndürebilirsiniz). Hem depolama hesapları nın hem de Klasik depolama hesaplarının anahtarlarını yönetebilirsiniz.

Yönetilen depolama hesabı anahtarı özelliğini kullandığınızda, aşağıdaki noktaları göz önünde bulundurun:

- Anahtar değerler, arayana yanıt olarak asla döndürülür.
- Yalnızca Key Vault, depolama hesabı anahtarlarınızı yönetmelidir. Anahtarları kendiniz yönetmeyin ve Key Vault işlemlerine müdahale etmekten kaçının.
- Yalnızca tek bir Key Vault nesnesi depolama hesabı anahtarlarını yönetmelidir. Birden çok nesneden anahtar yönetimine izin verme.
- Key Vault'un depolama hesabınızı bir kullanıcı ilkesiyle yönetmesini isteyebilirsiniz, ancak servis sorumlusuyla değil.
- Yalnızca Key Vault'u kullanarak tuşları yeniden oluşturun. Depolama hesabı anahtarlarınızı el ile yenilemayın.

Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmeti olan Azure Etkin Dizin (Azure AD) ile Azure Depolama tümleştirmesi kullanmanızı öneririz. Azure AD tümleştirmesi [Azure lekeleri ve kuyrukları](../storage/common/storage-auth-aad.md)için kullanılabilir ve Azure Depolama'ya (Azure Key Vault gibi) OAuth2 belirteci tabanlı erişim sağlar.

Azure AD, depolama hesabı kimlik bilgileri yerine bir uygulama veya kullanıcı kimliği kullanarak istemci uygulamanızın kimliğini doğrulamanızı sağlar. Azure'da çalışırken [Azure AD yönetilen bir kimlik](/azure/active-directory/managed-identities-azure-resources/) kullanabilirsiniz. Yönetilen kimlikler, istemci kimlik doğrulaması ve kimlik bilgilerini uygulamanızda veya uygulamanızda depolama gereksinimini ortadan kaldırır.

Azure AD, Yetkilendirmeyi yönetmek için rol tabanlı erişim denetimi (RBAC) kullanır ve bu denetim Key Vault tarafından da desteklenir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="service-principal-application-id"></a>Hizmet ana başvuru kimliği

Azure AD kiracısı, kayıtlı her uygulamayı bir [hizmet ilkesiyle](/azure/active-directory/develop/developer-glossary#service-principal-object)sağlar. Hizmet sorumlusu, RBAC aracılığıyla diğer Azure kaynaklarına erişmek için yetkilendirme kurulumu sırasında kullanılan uygulama kimliği olarak hizmet vermektedir.

Key Vault, tüm Azure AD kiracılarında önceden kaydedilmiş bir Microsoft uygulamasıdır. Key Vault, her Azure bulutunda aynı Uygulama Kimliği altında kayıtlıdır.

| Kiracılar | Bulut | Uygulama Kimliği |
| --- | --- | --- |
| Azure AD | Azure Kamu | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure genel | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Diğer  | Herhangi biri | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzu tamamlamak için öncelikle aşağıdakileri yapmanız gerekir:

- [Azure PowerShell modüllerini yükleyin.](/powershell/azure/install-az-ps?view=azps-2.6.0)
- [Anahtar kasası oluşturma](quick-create-powershell.md)
- [Bir Azure depolama hesabı oluşturun.](../storage/common/storage-account-create.md?tabs=azure-powershell) Depolama hesabı adı yalnızca küçük harfleri ve sayıları kullanmalıdır. Adın uzunluğu 3 ile 24 karakter arasında olmalıdır.
      

## <a name="manage-storage-account-keys"></a>Depolama hesabı anahtarlarını yönetme

### <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

PowerShell oturumunuzu [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) cmdlet kullanarak doğrulayın. 

```azurepowershell-interactive
Connect-AzAccount
```
Birden çok Azure aboneliğiniz varsa, bunları [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0) cmdlet'ini kullanarak listeleyebilir ve [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) cmdlet ile kullanmak istediğiniz aboneliği belirtebilirsiniz. 

```azurepowershell-interactive
Set-AzContext -SubscriptionId <subscriptionId>
```

### <a name="set-variables"></a>Değişkenleri ayarlama

İlk olarak, aşağıdaki adımlarda PowerShell cmdlets tarafından kullanılacak değişkenleri ayarlayın. <YourResourceGroupName>,, <YourStorageAccountName>ve <YourKeyVaultName> yer tutucuları güncelleştirip $keyVaultSpAppId `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` olarak ayarladıklarına emin olun [(Yukarıda Hizmet temel başvuru kimliğinde](#service-principal-application-id)belirtildiği gibi).

Ayrıca, kullanıcı kimliğinizi ve Azure depolama hesabınızın bağlamını almak için Azure PowerShell [Get-AzContext](/powershell/module/az.accounts/get-azcontext?view=azps-2.6.0) ve [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount?view=azps-2.6.0) cmdlet'lerini de kullanırız.

```azurepowershell-interactive
$resourceGroupName = <YourResourceGroupName>
$storageAccountName = <YourStorageAccountName>
$keyVaultName = <YourKeyVaultName>
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093"
$storageAccountKey = "key1"

# Get your User Id
$userId = (Get-AzContext).Account.Id

# Get a reference to your Azure storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName
```

### <a name="give-key-vault-access-to-your-storage-account"></a>Key Vault'un depolama hesabınıza erişmesini sağlar

Key Vault'un depolama hesabı anahtarlarınıza erişebilmeleri ve yönetmeden önce, bu hesap için depolama hesabınıza erişim yetkisi vermelisiniz. Key Vault uygulaması, depolama hesabınız için anahtarları *listelemek* ve *yenilemek* için izinler gerektirir. Bu izinler yerleşik RBAC rol [Depolama Hesabı Anahtar İşleç Hizmeti Rolü](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role)aracılığıyla etkinleştirilir. 

Azure PowerShell [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment?view=azps-2.6.0) cmdlet'ini kullanarak kapsamı depolama hesabınızla sınırlayarak bu rolü Key Vault hizmet ilkesine atayın.

```azurepowershell-interactive
# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Başarılı bir rol ataması üzerine, aşağıdaki örneğe benzer çıktı görmeniz gerekir:

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

Anahtar Vault depolama hesabınızdaki role zaten eklenmiştirsa, *"Rol ataması zaten var"* diye bir alan alırsınız. hatası döndürür. Azure portalındaki depolama hesabı "Access denetimi (IAM)" sayfasını kullanarak rol atamasını da doğrulayabilirsiniz.  

### <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Yönetilen depolama hesaplarına kullanıcı hesabınıza izin verme

Key Vault erişim ilkesini güncelleştirmek ve kullanıcı hesabınıza depolama hesabı izni vermek için Azure PowerShell [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.6.0) cmdlet'ini kullanın.

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userId -PermissionsToStorage get, list, delete, set, update, regeneratekey, getsas, listsas, deletesas, setsas, recover, backup, restore, purge
```

Azure portalındaki depolama hesabı "Erişim ilkeleri" sayfasında depolama hesapları için izinlerin kullanılolmadığını unutmayın.

### <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Key Vault örneğinize yönetilen bir depolama hesabı ekleme

Key Vault örneğinde yönetilen bir depolama hesabı oluşturmak için Azure PowerShell [Add-AzKeyVaultManagedStorageAccount](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount?view=azps-2.6.0) cmdlet'ini kullanın. Anahtar, `-DisableAutoRegenerateKey` depolama hesabı anahtarlarını yeniden oluşturmak için Değİl belirtir.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts

Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Anahtar rejenerasyonu olmadan depolama hesabının başarılı bir şekilde eklenmesiyle, aşağıdaki örneğe benzer çıktı görmeniz gerekir:

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

### <a name="enable-key-regeneration"></a>Anahtar rejenerasyonuna olanak sağlama

Key Vault'un depolama hesap anahtarlarınızı düzenli aralıklarla yeniden oluşturmasını istiyorsanız, yenilenme dönemi ayarlamak için Azure PowerShell [Add-AzKeyVaultManagedStorageAccount](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount?view=azps-2.6.0) cmdlet'ini kullanabilirsiniz. Bu örnekte, üç günlük bir yenilenme dönemi belirledik. Üç gün sonra, Key Vault 'key2'yi yeniden oluşturur ve etkin anahtarı 'key2'den 'key1' ile değiştirir.

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)

Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Depolama hesabının anahtar rejenerasyonuyla başarılı bir şekilde eklenmesinden sonra, aşağıdaki örneğe benzer çıktıgörmeniz gerekir:

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

## <a name="shared-access-signature-tokens"></a>Paylaşılan erişim imza belirteçleri

Key Vault'dan paylaşılan erişim imza belirteçleri oluşturmasını da isteyebilirsiniz. Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Müşterilerin hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim izni verebilirsiniz. Paylaşılan erişim imzası, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanız için güvenli bir yol sağlar.

Bu bölümdeki komutlar aşağıdaki eylemleri tamamlar:

- Paylaşılan bir hesap erişim imzası tanımı ayarlayın. 
- Blob, File, Tablo ve Sıra hizmetleri için paylaşılan erişim imza belirteci oluşturun. Belirteç, kaynak türleri Hizmet, Kapsayıcı ve Nesne için oluşturulur. Belirteç, tüm izinlerle, https üzerinden ve belirtilen başlangıç ve bitiş tarihleriyle oluşturulur.
- Kasada key vault yönetilen depolama paylaşılan erişim imza tanımı ayarlayın. Tanım, oluşturulan paylaşılan erişim imza belirteci şablonu URI vardır. Tanım paylaşılan erişim imza `account` türüne sahiptir ve N günleri için geçerlidir.
- Paylaşılan erişim imzasının anahtar kasanıza gizli olarak kaydedildiğini doğrulayın.
- 
### <a name="set-variables"></a>Değişkenleri ayarlama

İlk olarak, aşağıdaki adımlarda PowerShell cmdlets tarafından kullanılacak değişkenleri ayarlayın. Yer tutucuları <YourStorageAccountName> ve <YourKeyVaultName> yer tutucuları güncellediğinden emin olun.

Azure depolama hesabınızın bağlamını almak için Azure PowerShell [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext?view=azps-2.6.0) cmdlets'i de kullanacağız.

```azurepowershell-interactive
$storageAccountName = <YourStorageAccountName>
$keyVaultName = <YourKeyVaultName>

$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -Protocol Https -StorageAccountKey Key1
```

### <a name="create-a-shared-access-signature-token"></a>Paylaşılan erişim imza belirteci oluşturma

Azure PowerShell [New-AzStorageAccountSASToken](/powershell/module/az.storage/new-azstorageaccountsastoken?view=azps-2.6.0) cmdlets'i kullanarak paylaşılan erişim imzası tanımı oluşturun.
 
```azurepowershell-interactive
$start = [System.DateTime]::Now.AddDays(-1)
$end = [System.DateTime]::Now.AddMonths(1)

$sasToken = New-AzStorageAccountSasToken -Service blob,file,Table,Queue -ResourceType Service,Container,Object -Permission "racwdlup" -Protocol HttpsOnly -StartTime $start -ExpiryTime $end -Context $storageContext
```
$sasToken değeri buna benzer.

```console
?sv=2018-11-09&sig=5GWqHFkEOtM7W9alOgoXSCOJO%2B55qJr4J7tHQjCId9S%3D&spr=https&st=2019-09-18T18%3A25%3A00Z&se=2019-10-19T18%3A25%3A00Z&srt=sco&ss=bfqt&sp=racupwdl
```

### <a name="generate-a-shared-access-signature-definition"></a>Paylaşılan erişim imzası tanımı oluşturma

Paylaşılan erişim imzası tanımı oluşturmak için Azure PowerShell [Set-AzKeyVaultManagedStorageSasDefinition](/powershell/module/az.keyvault/set-azkeyvaultmanagedstoragesasdefinition?view=azps-2.6.0) cmdlet'i kullanın.  Seçtiğiniz adı `-Name` parametreye verebilirsiniz.

```azurepowershell-interactive
Set-AzKeyVaultManagedStorageSasDefinition -AccountName $storageAccountName -VaultName $keyVaultName -Name <YourSASDefinitionName> -TemplateUri $sasToken -SasType 'account' -ValidityPeriod ([System.Timespan]::FromDays(30))
```

### <a name="verify-the-shared-access-signature-definition"></a>Paylaşılan erişim imza tanımını doğrulama

Azure PowerShell [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret?view=azps-2.6.0) cmdlet'i kullanarak paylaşılan erişim imzası tanımının anahtar kasanızda depolandığını doğrulayabilirsiniz.

İlk olarak, anahtar kasanızda paylaşılan erişim imza tanımını bulun.

```azurepowershell-interactive
Get-AzKeyVaultSecret -VaultName <YourKeyVaultName>
```

SAS tanımınıza karşılık gelen gizli şu özelliklere sahip olacaktır:

```console
Vault Name   : <YourKeyVaultName>
Name         : <SecretName>
...
Content Type : application/vnd.ms-sastoken-storage
Tags         :
```

Artık [get-AzKeyVaultSecret](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) cmdlet ve gizli `Name` özelliği bu sırrın içeriğini görüntülemek için kullanabilirsiniz.

```azurepowershell-interactive
$secret = Get-AzKeyVaultSecret -VaultName <YourKeyVaultName> -Name <SecretName>

Write-Host $secret.SecretValueText
```

Bu komutun çıktısı SAS tanım dizenizi gösterir.


## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Anahtarlar, sırlar ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
- [Anahtar Vault PowerShell referans](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
