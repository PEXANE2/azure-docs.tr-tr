---
title: Azure Key Vault ve Azure CLı ile depolama hesabı anahtarlarını yönetme
description: Depolama hesabı anahtarları, Azure depolama hesabına Azure Key Vault ve anahtar tabanlı erişim arasında sorunsuz bir tümleştirme sağlar.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 03/01/2019
ms.openlocfilehash: 68c9279333e9ee92cbca1034f70801c153b6cdb8
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71000545"
---
# <a name="manage-storage-account-keys-with-azure-key-vault-and-the-azure-cli"></a>Azure Key Vault ve Azure CLı ile depolama hesabı anahtarlarını yönetme 

Azure Key Vault, Azure depolama hesapları ve klasik depolama hesapları için anahtarları yönetir. Key Vault yönetilen depolama hesabı özelliğini kullanarak sizin için çeşitli önemli yönetim işlevlerini tamamlayabilirsiniz.

[Azure depolama hesabı](/azure/storage/storage-create-storage-account) , hesap adı ve anahtar içeren bir kimlik bilgisi kullanır. Anahtar otomatik olarak oluşturulur ve şifreleme anahtarı olarak değil, parola görevi görür. Key Vault, depolama hesabı anahtarlarını [Key Vault gizli](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets)dizileri olarak depolayarak yönetir. Anahtarlar bir Azure Storage hesabıyla listelenir (eşitlenir) ve düzenli olarak yeniden oluşturulur veya _döndürülür_. 

Yönetilen depolama hesabı anahtar özelliğini kullandığınızda, aşağıdaki noktaları göz önünde bulundurun:

- Anahtar değerleri, bir çağırana yanıt olarak hiçbir şekilde döndürülmez.
- Depolama hesabı anahtarlarınızı yalnızca Key Vault yönetmelidir. Anahtarları kendiniz yönetmeyin ve Key Vault süreçleriyle kesintiye uğramayın.
- Depolama hesabı anahtarlarını yalnızca tek bir Key Vault nesnesi yönetmelidir. Birden çok nesneden anahtar yönetimine izin verme.
- Depolama hesabınızı bir Kullanıcı sorumlusu ile yönetmek için Key Vault isteyebilirsiniz, ancak hizmet sorumlusu ile kullanamazsınız.
- Anahtarları yalnızca Key Vault kullanarak yeniden oluşturun. Depolama hesabı anahtarlarınızı el ile yeniden üretme. 

> [!NOTE]
> Azure Active Directory (Azure AD) ile Azure depolama tümleştirmesi, Microsoft 'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir.
> Azure AD tümleştirmesi, [Azure Blobları ve kuyrukları](../storage/common/storage-auth-aad.md)için kullanılabilir.
> Kimlik doğrulaması ve yetkilendirme için Azure AD 'yi kullanın.
> Azure AD, Azure Key Vault gibi Azure depolama 'ya OAuth2 belirteç tabanlı erişim sağlar.
>
> Azure AD, depolama hesabı kimlik bilgileri yerine bir uygulama veya Kullanıcı kimliği kullanarak istemci uygulamanızın kimliğini doğrulayabilmeniz için izin verir.
> Azure 'da çalıştırdığınızda [Azure ad yönetilen kimliğini](/azure/active-directory/managed-identities-azure-resources/) kullanabilirsiniz. Yönetilen kimlikler, istemci kimlik doğrulaması gereksinimini ortadan kaldırır ve uygulamanızda veya uygulamanızdaki kimlik bilgilerini depolar.
> Azure AD, Key Vault tarafından da desteklenen yetkilendirmeyi yönetmek için rol tabanlı erişim denetimi 'ni (RBAC) kullanır.

### <a name="service-principal-application-id"></a>Hizmet sorumlusu uygulama KIMLIĞI

Bir Azure AD kiracısı, kayıtlı her uygulamayı bir [hizmet sorumlusu](/azure/active-directory/develop/developer-glossary#service-principal-object)ile sağlar. Hizmet sorumlusu uygulama kimliği (KIMLIK) görevi görür. Uygulama KIMLIĞI, RBAC aracılığıyla diğer Azure kaynaklarına erişim için yetkilendirme kurulumu sırasında kullanılır.

Key Vault, tüm Azure AD kiracılarında önceden kaydedilmiş bir Microsoft uygulamasıdır. Key Vault, aynı uygulama KIMLIĞI ve her bir Azure bulutu dahilinde kaydedilir.

| Kira | Bulut | Uygulama Kimliği |
| --- | --- | --- |
| Azure AD | Azure Kamu | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure genel | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Diğer  | Any | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="prerequisites"></a>Önkoşullar

Depolama hesabı anahtarınızı yönetmek için Key Vault kullanmadan önce önkoşulları gözden geçirin:

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)’yi yükleyin.
- Bir [Azure depolama hesabı](https://azure.microsoft.com/services/storage/)oluşturun. [Bu adımları](../storage/index.yml)izleyin.
- Depolama hesabı adı yalnızca küçük harfler ve rakamlar kullanmalıdır. Adın uzunluğu 3 ile 24 karakter arasında olmalıdır.        
      
## <a name="manage-storage-account-keys"></a>Depolama hesabı anahtarlarını yönetme

Depolama hesabı anahtarlarını yönetmek için Key Vault kullanmanın dört temel adımı vardır:

1. Mevcut bir depolama hesabı alın.
1. Var olan bir anahtar kasasını getir.
1. Kasaya Key Vault yönetilen bir depolama hesabı ekleyin. Yeniden `key1` oluşturma süresi 90 gün olan etkin anahtar olarak ayarlayın.
1. Belirtilen `key1` depolama hesabı için bir depolama bağlamı ayarlamak üzere kullanın.

> [!NOTE]
> Hizmet olarak Key Vault depolama hesabınızda operatör izinleri atanmıştır.
> 
> Azure Key Vault yönetilen depolama hesabı anahtarlarını ayarladıktan sonra, anahtarları yalnızca Key Vault kullanarak değiştirin.
> Yönetilen depolama hesabı anahtarları için, Key Vault depolama hesabı anahtarının dönüşünü yönetir.

1. Bir depolama hesabı oluşturduktan sonra, yönetilecek depolama hesabının kaynak KIMLIĞINI almak için aşağıdaki komutu çalıştırın:
    ```
    az storage account show -n storageaccountname
    ```

    Komut çıktısından kaynak KIMLIĞI değerini kopyalayın:
    ```
    /subscriptions/<subscription ID>/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```

    Örnek çıktı:
    ```
    "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    ```
    
1. Key Vault için "depolama hesabı anahtar operatörü hizmeti rolü" RBAC rolünü atayın. Bu rol, erişim kapsamını depolama hesabınızla sınırlandırır. Klasik depolama hesabı için "klasik depolama hesabı anahtar Işletmeni hizmet rolü" rolünü kullanın.

    ```
    az role assignment create --role "Storage Account Key Operator Service Role" --assignee-object-id 93c27d83-f79b-4cb2-8dd4-4aa716542e74 --scope "/subscriptions/<subscriptionID>/resourceGroups/<StorageAccountResourceGroupName>/providers/Microsoft.Storage/storageAccounts/<StorageAccountName>"
    ```
    
    `93c27d83-f79b-4cb2-8dd4-4aa716542e74`, Azure genel bulutundaki Key Vault için nesne KIMLIĞIDIR. Azure Kamu bulutundaki Key Vault nesne KIMLIĞINI almak için bkz. [hizmet sorumlusu uygulama kimliği](#service-principal-application-id).
    
1. Key Vault yönetilen bir depolama hesabı oluşturun:

    Yeniden oluşturma dönemini 90 gün olarak ayarlayın. 90 gün sonra, Key Vault yeniden `key1` üretir ve etkin anahtarı ' `key1`den `key2` ' a değiştirir. `key1`ardından etkin anahtar olarak işaretlenir. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="create-and-generate-tokens"></a>Belirteç oluşturma ve oluşturma

Ayrıca, Key Vault paylaşılan erişim imzası belirteçleri oluşturmasını isteyebilirsiniz. Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. İstemci, hesap anahtarlarınızı paylaşmadan Depolama hesabınızdaki kaynaklara erişim izni verebilirsiniz. Paylaşılan erişim imzası, hesap anahtarlarınızla ödün vermeden depolama kaynaklarınızı paylaşmak için güvenli bir yol sağlar.

Bu bölümdeki komutlar aşağıdaki eylemleri tamamlar:

- Hesap paylaşılan erişim imzası tanımı `<YourSASDefinitionName>`ayarlayın. Tanım, anahtar kasaınızdaki `<YourStorageAccountName>` `<VaultName>`Key Vault yönetilen bir depolama hesabında ayarlanır.
- Blob, dosya, tablo ve kuyruk Hizmetleri için bir hesap paylaşılan erişim imza belirteci oluşturun. Belirteç, kaynak türleri hizmeti, kapsayıcısı ve nesnesi için oluşturulur. Belirteç, https üzerinden ve belirtilen başlangıç ve bitiş tarihleriyle birlikte tüm izinlerle oluşturulur.
- Kasada Key Vault yönetilen bir depolama paylaşılan erişim imzası tanımı ayarlayın. Tanım, oluşturulan paylaşılan erişim imzası belirtecinin şablon URI 'sine sahiptir. Tanım, paylaşılan erişim imzası türüne `account` sahiptir ve N gün için geçerlidir.
- Paylaşılan erişim imzası tanımına karşılık gelen Key Vault gizli anahtarından gerçek erişim belirtecini alın.

Önceki bölümde bulunan adımları tamamladıktan sonra, Key Vault paylaşılan erişim imzası belirteçleri oluşturmasını istemek için aşağıdaki komutları çalıştırın. 

1. Paylaşılan erişim imzası tanımı oluşturun. Paylaşılan erişim imzası tanımı oluşturulduktan sonra, Key Vault daha paylaşılan erişim imzası belirteçleri oluşturmasını isteyin. Bu işlem `storage` ve `setsas` izinlerini gerektirir.
    ```
    $sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
    ```

    İşlem hakkında yardım için, [az Storage Account Generate-SAS](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas) Reference belgelerine bakın.

    İşlem başarıyla çalıştıktan sonra çıktıyı kopyalayın.
    ```console
       "se=2020-01-01&sp=***"
    ```

1. Önceki komut tarafından oluşturulan öğesini kullanın ve paylaşılan erişim imzası tanımını oluşturun. `$sasToken` Komut parametreleri hakkında daha fazla bilgi için, bkz. [keykasası Storage SAS-Definition](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters) başvuru belgesi oluşturma.
    ```
    az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
    ```

    Kullanıcının depolama hesabı için izinleri olmadığında, önce kullanıcının nesne KIMLIĞINI alın:
    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

## <a name="fetch-tokens-in-code"></a>Koddaki belirteçleri getir

Key Vault, [paylaşılan erişim imzası belirteçleri](../storage/common/storage-dotnet-shared-access-signature-part-1.md) getirerek depolama hesabınızda işlemleri yürütün.

Key Vault doğrulamak için üç yol vardır:

- Yönetilen hizmet kimliğini kullanın. Bu yaklaşım kesinlikle önerilir.
- Hizmet sorumlusu ve sertifikası kullanın. 
- Hizmet sorumlusu ve parolası kullanın. Bu yaklaşım önerilmez.

Daha fazla bilgi için bkz [. Azure Key Vault: Temel kavramlar](basic-concepts.md).

Aşağıdaki örnekte, paylaşılan erişim imzası belirteçlerinin nasıl getirileceği gösterilmektedir. Paylaşılan erişim imzası tanımını oluşturduktan sonra belirteçleri getirin. 

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Paylaşılan erişim imza belirteciniz sona ermek üzereyken, paylaşılan erişim imza belirtecini Key Vault ' den yeniden getirin ve kodu güncelleştirin.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

<!-- Add closing sentences to summarize what the user accomplished in this section. -->

### <a name="azure-cli-commands"></a>Azure CLI komutları

Yönetilen depolama hesaplarıyla ilgili Azure CLı komutları hakkında daha fazla bilgi için, [az keykasa depolama](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest) başvurusu belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtarlar, gizli diziler ve sertifikalar](https://docs.microsoft.com/rest/api/keyvault/)hakkında daha fazla bilgi edinin.
- [Azure Key Vault ekip bloguna](https://blogs.technet.microsoft.com/kv/)ilişkin makaleleri gözden geçirin.
