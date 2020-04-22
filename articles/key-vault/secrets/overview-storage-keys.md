---
title: Azure Anahtar Kasası ve Azure CLI ile depolama hesabı anahtarlarını yönetme
description: Depolama hesabı anahtarları, Azure Anahtar Kasası ile Azure depolama hesabına anahtar tabanlı erişim arasında sorunsuz entegrasyon sağlar.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/18/2019
ms.openlocfilehash: 1125bafa43ce1752c58d1cce0bba66a6bbd32c32
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685428"
---
# <a name="manage-storage-account-keys-with-key-vault-and-the-azure-cli"></a>Key Vault ve Azure CLI ile depolama hesabı anahtarlarını yönetme

Azure depolama hesabı, bir hesap adı ve anahtardan oluşan kimlik bilgilerini kullanır. Anahtar otomatik olarak oluşturulur ve şifreleme anahtarı olarak değil, parola görevi görede hizmet eder. Key Vault, depolama hesabı anahtarlarını Key Vault sırları olarak saklayarak yönetir. 

Anahtar Kasası yönetilen depolama hesabı anahtar özelliğini kullanarak bir Azure depolama hesabıyla anahtarları listeleyebilir (eşitleyebilir) ve anahtarları düzenli aralıklarla yeniden oluşturabilir (döndürebilirsiniz). Hem depolama hesapları nın hem de Klasik depolama hesaplarının anahtarlarını yönetebilirsiniz.

Yönetilen depolama hesabı anahtarı özelliğini kullandığınızda, aşağıdaki noktaları göz önünde bulundurun:

- Anahtar değerler, arayana yanıt olarak asla döndürülür.
- Yalnızca Key Vault, depolama hesabı anahtarlarınızı yönetmelidir. Anahtarları kendiniz yönetmeyin ve Key Vault işlemlerine müdahale etmekten kaçının.
- Yalnızca tek bir Key Vault nesnesi depolama hesabı anahtarlarını yönetmelidir. Birden çok nesneden anahtar yönetimine izin verme.
- Key Vault'un depolama hesabınızı bir kullanıcı ilkesiyle yönetmesini isteyebilirsiniz, ancak servis sorumlusuyla değil.
- Yalnızca Key Vault'u kullanarak tuşları yeniden oluşturun. Depolama hesabı anahtarlarınızı el ile yenilemayın.

Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmeti olan Azure Etkin Dizin (Azure AD) ile Azure Depolama tümleştirmesi kullanmanızı öneririz. Azure AD tümleştirmesi [Azure lekeleri ve kuyrukları](../../storage/common/storage-auth-aad.md)için kullanılabilir ve Azure Depolama'ya (Azure Key Vault gibi) OAuth2 belirteci tabanlı erişim sağlar.

Azure AD, depolama hesabı kimlik bilgileri yerine bir uygulama veya kullanıcı kimliği kullanarak istemci uygulamanızın kimliğini doğrulamanızı sağlar. Azure'da çalışırken [Azure AD yönetilen bir kimlik](/azure/active-directory/managed-identities-azure-resources/) kullanabilirsiniz. Yönetilen kimlikler, istemci kimlik doğrulaması ve kimlik bilgilerini uygulamanızda veya uygulamanızda depolama gereksinimini ortadan kaldırır.

Azure AD, Yetkilendirmeyi yönetmek için rol tabanlı erişim denetimi (RBAC) kullanır ve bu denetim Key Vault tarafından da desteklenir.

## <a name="service-principal-application-id"></a>Hizmet ana başvuru kimliği

Azure AD kiracısı, kayıtlı her uygulamayı bir [hizmet ilkesiyle](/azure/active-directory/develop/developer-glossary#service-principal-object)sağlar. Hizmet sorumlusu, RBAC aracılığıyla diğer Azure kaynaklarına erişmek için yetkilendirme kurulumu sırasında kullanılan Uygulama Kimliği olarak hizmet vermektedir.

Key Vault, tüm Azure AD kiracılarında önceden kaydedilmiş bir Microsoft uygulamasıdır. Key Vault, her Azure bulutunda aynı Uygulama Kimliği altında kayıtlıdır.

| Kiracılar | Bulut | Uygulama Kimliği |
| --- | --- | --- |
| Azure AD | Azure Kamu | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure genel | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Diğer  | Herhangi biri | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzu tamamlamak için öncelikle aşağıdakileri yapmanız gerekir:

- [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
- [Anahtar kasası oluşturma](quick-create-cli.md)
- [Bir Azure depolama hesabı oluşturun.](../../storage/common/storage-account-create.md?tabs=azure-cli) Depolama hesabı adı yalnızca küçük harfleri ve sayıları kullanmalıdır. Adın uzunluğu 3 ile 24 karakter arasında olmalıdır.
      
## <a name="manage-storage-account-keys"></a>Depolama hesabı anahtarlarını yönetme

### <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma

[Az giriş](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutlarını kullanarak Azure CLI oturumunuzu doğrulatın.

```azurecli-interactive
az login
``` 

### <a name="give-key-vault-access-to-your-storage-account"></a>Key Vault'un depolama hesabınıza erişmesini sağlar

Key Vault'un depolama hesabınıza erişmesi için Azure CLI [az rol oluşturma](/cli/azure/role/assignment?view=azure-cli-latest) komutunu kullanın. Komuta aşağıdaki parametre değerlerini sağlayın:

- `--role`: "Depolama Hesabı Anahtar Operatörü Hizmet Rolü" RBAC rolünü geçirin. Bu rol, depolama hesabınıza erişim kapsamını sınırlar. Klasik bir depolama hesabı için bunun yerine "Klasik Depolama Hesabı Anahtar Operatörü Hizmeti Rolü"ni geçirin.
- `--assignee`: Azure genelhttps://vault.azure.netbulutundaki Key Vault'un url'si olan " " değerini geçirin. (Azure Goverment bulutu için '--asingee-object-id' yerine [Hizmet temel uygulama kimliğine](#service-principal-application-id)bakın.)
- `--scope`: Formdaki `/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>`depolama hesabı kaynak kimliğinizi iletin. Abonelik kimliğinizi bulmak için Azure CLI [az hesap listesi](/cli/azure/account?view=azure-cli-latest#az-account-list) komutunu kullanın; depolama hesabı adınızı ve depolama hesabı kaynak grubunu bulmak için Azure CLI [az depolama hesabı listesi](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) komutunu kullanın.

```azurecli-interactive
az role assignment create --role "Storage Account Key Operator Service Role" --assignee 'https://vault.azure.net' --scope "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>"
 ```
### <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Yönetilen depolama hesaplarına kullanıcı hesabınıza izin verme

Key Vault erişim ilkesini güncelleştirmek ve kullanıcı hesabınıza depolama hesabı izni vermek için Azure CLI [az keyvault ayarlı ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) cmdlet'i kullanın.

```azurecli-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --storage-permissions get list delete set update regeneratekey getsas listsas deletesas setsas recover backup restore purge
```

Azure portalındaki depolama hesabı "Erişim ilkeleri" sayfasında depolama hesapları için izinlerin kullanılolmadığını unutmayın.
### <a name="create-a-key-vault-managed-storage-account"></a>Key Vault Yönetilen depolama hesabı oluşturma

 Azure CLI [az keyvault depolama komutunu](/cli/azure/keyvault/storage?view=azure-cli-latest#az-keyvault-storage-add) kullanarak Bir Key Vault yönetilen depolama hesabı oluşturun. 90 günlük bir yenilenme süresi ayarlayın. 90 gün sonra, Key `key1` Vault aktif anahtarı yeniler `key1`ve 'den `key2` . `key1`sonra etkin anahtar olarak işaretlenir. Komuta aşağıdaki parametre değerlerini sağlayın:

- `--vault-name`: Anahtar kasanızın adını iletin. Anahtar kasanızın adını bulmak için Azure CLI [az keyvault listesi](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-list) komutunu kullanın.
- `-n`: Depolama hesabınızın adını iletin. Depolama hesabınızın adını bulmak için Azure CLI [az depolama hesabı listesi](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) komutunu kullanın.
- `--resource-id`: Formdaki `/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>`depolama hesabı kaynak kimliğinizi iletin. Abonelik kimliğinizi bulmak için Azure CLI [az hesap listesi](/cli/azure/account?view=azure-cli-latest#az-account-list) komutunu kullanın; depolama hesabı adınızı ve depolama hesabı kaynak grubunu bulmak için Azure CLI [az depolama hesabı listesi](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-list) komutunu kullanın.
   
 ```azurecli-interactive
az keyvault storage add --vault-name <YourKeyVaultName> -n <YourStorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<YourStorageAccountName>"
 ```

## <a name="shared-access-signature-tokens"></a>Paylaşılan erişim imza belirteçleri

Key Vault'dan paylaşılan erişim imza belirteçleri oluşturmasını da isteyebilirsiniz. Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Müşterilerin hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim izni verebilirsiniz. Paylaşılan erişim imzası, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanız için güvenli bir yol sağlar.

Bu bölümdeki komutlar aşağıdaki eylemleri tamamlar:

- Paylaşılan erişim imza tanımını `<YourSASDefinitionName>`hesapla' t Tanım, anahtar kasanızdaki `<YourStorageAccountName>` `<YourKeyVaultName>`Key Vault yönetilen depolama hesabında ayarlanır.
- Blob, File, Tablo ve Sıra hizmetleri için paylaşılan erişim imza belirteci oluşturun. Belirteç, kaynak türleri Hizmet, Kapsayıcı ve Nesne için oluşturulur. Belirteç, tüm izinlerle, https üzerinden ve belirtilen başlangıç ve bitiş tarihleriyle oluşturulur.
- Kasada key vault yönetilen depolama paylaşılan erişim imza tanımı ayarlayın. Tanım, oluşturulan paylaşılan erişim imza belirteci şablonu URI vardır. Tanım paylaşılan erişim imza `account` türüne sahiptir ve N günleri için geçerlidir.
- Paylaşılan erişim imzasının anahtar kasanıza gizli olarak kaydedildiğini doğrulayın.

### <a name="create-a-shared-access-signature-token"></a>Paylaşılan erişim imza belirteci oluşturma

Azure CLI az depolama hesabı [oluşturma-sas](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) komutunu kullanarak paylaşılan erişim imzası tanımı oluşturun. Bu işlem `storage` ve `setsas` izinler gerektirir.


```azurecli-interactive
az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name <YourStorageAccountName> --account-key 00000000
```
İşlem başarıyla çalıştırdıktan sonra çıktıyı kopyalayın.

```console
"se=2020-01-01&sp=***"
```

Bu çıktı bir sonraki `--template-id` adımda parametreye geçirilen olacaktır.

### <a name="generate-a-shared-access-signature-definition"></a>Paylaşılan erişim imzası tanımı oluşturma

Paylaşılan erişim imzası tanımı oluşturmak için önceki adımdaki çıktıyı `--template-id` parametreye geçirerek Azure CLI az [keyvault depolama sas tanım oluşturma](/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#az-keyvault-storage-sas-definition-create) komutunu kullanın.  Seçtiğiniz adı `-n` parametreye verebilirsiniz.

```azurecli-interactive
az keyvault storage sas-definition create --vault-name <YourKeyVaultName> --account-name <YourStorageAccountName> -n <YourSASDefinitionName> --validity-period P2D --sas-type account --template-uri <OutputOfSasTokenCreationStep>
```

### <a name="verify-the-shared-access-signature-definition"></a>Paylaşılan erişim imza tanımını doğrulama

Azure CLI [az keyvault gizli listesi](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-list) ve az [keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutlarını kullanarak paylaşılan erişim imza tanımının anahtar kasanızda depolandığını doğrulayabilirsiniz.

İlk olarak, [az keyvault gizli liste](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-list) komutunu kullanarak anahtar kasanızda paylaşılan erişim imza tanımını bulun.

```azurecli-interactive
az keyvault secret list --vault-name <YourKeyVaultName>
```

SAS tanımınıza karşılık gelen gizli şu özelliklere sahip olacaktır:

```console
    "contentType": "application/vnd.ms-sastoken-storage",
    "id": "https://<YourKeyVaultName>.vault.azure.net/secrets/<YourStorageAccountName>-<YourSASDefinitionName>",
```

Şimdi bu sırrın içeriğini görüntülemek için [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutunu ve `id` özelliğini kullanabilirsiniz.

```azurecli-interactive
az keyvault secret show --vault-name <YourKeyVaultName> --id <SasDefinitionID>
```

Bu komutun çıktısı SAS tanım`value`dizenizi .


## <a name="next-steps"></a>Sonraki adımlar

- [Anahtarlar, sırlar ve sertifikalar](https://docs.microsoft.com/rest/api/keyvault/)hakkında daha fazla bilgi edinin.
- Azure Key [Vault takım blogundaki](https://blogs.technet.microsoft.com/kv/)makaleleri inceleyin.
- Az [keyvault depolama](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) başvuru belgelerine bakın.
