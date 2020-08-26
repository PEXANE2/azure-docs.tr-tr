---
title: Şifreleme kapsamları oluşturma ve yönetme (Önizleme)
description: Blob verilerini kapsayıcı veya blob düzeyinde yalıtmak için bir şifreleme kapsamı oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.date: 08/25/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 32b46d21228bcd84fc3da11cc6ed42c740fece39
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88870264"
---
# <a name="create-and-manage-encryption-scopes-preview"></a>Şifreleme kapsamları oluşturma ve yönetme (Önizleme)

Şifreleme kapsamları (Önizleme), tek bir blob veya kapsayıcı düzeyinde şifrelemeyi yönetmenizi sağlar. Şifreleme kapsamı, blob verilerini bir depolama hesabındaki güvenli bir kuşatmada yalıtır. Aynı depolama hesabında bulunan ancak farklı müşterilere ait olan veriler arasında güvenli sınırlar oluşturmak için şifreleme kapsamlarını kullanabilirsiniz. Şifreleme kapsamları hakkında daha fazla bilgi için bkz. [BLOB depolama (Önizleme) Için şifreleme kapsamları](../common/storage-service-encryption.md#encryption-scopes-for-blob-storage-preview).

Bu makalede, bir şifreleme kapsamının nasıl oluşturulacağı gösterilmektedir. Ayrıca bir blob veya kapsayıcı oluştururken bir şifreleme kapsamının nasıl gösterileceğini gösterir.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="create-an-encryption-scope"></a>Şifreleme kapsamı oluşturma

Microsoft tarafından yönetilen bir anahtarla veya Azure Key Vault depolanan, müşteri tarafından yönetilen bir anahtarla şifreleme kapsamları oluşturabilirsiniz. Müşteri tarafından yönetilen bir anahtarla bir şifreleme kapsamı oluşturmak için, önce bir Azure Anahtar Kasası oluşturmanız ve kapsam için kullanmayı düşündüğünüz anahtarı eklemeniz gerekir. Anahtar kasasının hem **geçici silme** hem de **Temizleme koruma** özelliklerinin etkinleştirilmiş olması ve depolama hesabıyla aynı bölgede olması gerekir. Daha fazla bilgi için bkz. [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](../common/encryption-customer-managed-keys.md).

Bir şifreleme kapsamı, oluşturduğunuzda otomatik olarak etkinleştirilir. Şifreleme kapsamını oluşturduktan sonra, bir blob oluştururken belirtebilirsiniz. Ayrıca, kapsayıcıdaki tüm Bloblar için otomatik olarak uygulanan bir kapsayıcı oluşturduğunuzda varsayılan bir şifreleme kapsamı belirtebilirsiniz.

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal bir şifreleme kapsamı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalda depolama hesabınıza gidin.
1. **Şifreleme** ayarını seçin.
1. **Şifreleme kapsamları** sekmesini seçin.
1. Yeni bir şifreleme kapsamı eklemek için **Ekle** düğmesine tıklayın.
1. **Şifreleme kapsamı** oluştur bölmesinde, yeni kapsam için bir ad girin.
1. **Microsoft tarafından yönetilen anahtarlar** veya **müşteri tarafından yönetilen anahtarlar**için şifreleme türünü seçin.
    - **Microsoft tarafından yönetilen anahtarlar**' ı seçtiyseniz, şifreleme kapsamını oluşturmak için **Oluştur** ' a tıklayın.
    - **Müşteri tarafından yönetilen anahtarlar**' ı seçtiyseniz, aşağıdaki görüntüde gösterildiği gibi, bu şifreleme kapsamı için kullanılacak bir Anahtar Kasası, anahtar ve anahtar sürümü belirtin.

    :::image type="content" source="media/encryption-scope-manage/create-encryption-scope-customer-managed-key-portal.png" alt-text="Azure portal 'de şifreleme kapsamı oluşturmayı gösteren ekran görüntüsü":::

Azure depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırma hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault Ile yapılandırma](../common/storage-encryption-keys-portal.md).

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile bir şifreleme kapsamı oluşturmak için önce az. Storage Preview Module sürümünü yüklemeniz gerekir. En son önizleme sürümünü kullanmanız önerilir, ancak şifreleme kapsamları sürüm 1.13.4-Preview ve sonraki sürümlerde desteklenir. Az. Storage modülünün diğer sürümlerini kaldırın.

Aşağıdaki komut az. Storage [2.1.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/2.1.1-preview) modülünü yüklüyor:

```powershell
Install-Module -Name Az.Storage -RequiredVersion 2.1.1-preview -AllowPrerelease
```

### <a name="create-an-encryption-scope-protected-by-microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlarla korunan bir şifreleme kapsamı oluşturun

Microsoft tarafından yönetilen anahtarlarla korunan yeni bir şifreleme kapsamı oluşturmak için, parametresiyle **New-AzStorageEncryptionScope** komutunu çağırın `-StorageEncryption` .

Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$scopeName1 = "customer1scope"

New-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -AccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -StorageEncryption
```

### <a name="create-an-encryption-scope-protected-by-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla korunan bir şifreleme kapsamı oluşturun

Azure Key Vault ile müşteri tarafından yönetilen anahtarlarla korunan yeni bir şifreleme kapsamı oluşturmak için önce depolama hesabı için müşteri tarafından yönetilen anahtarları yapılandırın. Depolama hesabına yönetilen bir kimlik atamanız ve ardından, depolama hesabının bu hesaba erişim izinleri olması için Anahtar Kasası için erişim ilkesini yapılandırmak üzere yönetilen kimliği kullanmanız gerekir. Daha fazla bilgi için bkz. [PowerShell kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma](../common/storage-encryption-keys-powershell.md).

Müşteri tarafından yönetilen anahtarları bir şifreleme kapsamıyla kullanılmak üzere yapılandırmak için, anahtar kasasında hem **geçici silme** hem de **Temizleme koruma** özelliklerinin etkinleştirilmiş olması gerekir. Anahtar Kasası, depolama hesabıyla aynı bölgede olmalıdır. Daha fazla bilgi için bkz. [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](../common/encryption-customer-managed-keys.md).

Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$keyVaultName = "<key-vault>"
$keyUri = "<key-uri-with-version>"
$scopeName2 = "customer2scope"


# Assign a system managed identity to the storage account.
$storageAccount = Set-AzStorageAccount -ResourceGroupName $rgName `
    -Name $accountName `
    -AssignIdentity

# Configure the access policy for the key vault.
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get
```

Ardından, **New-AzStorageEncryptionScope** komutunu `-KeyvaultEncryption` parametresiyle ÇAĞıRıN ve anahtar URI 'sini belirtin. Anahtar URI 'sine anahtar sürümünü eklediğinizden emin olun. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
New-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -AccountName $accountName `
    -EncryptionScopeName $scopeName2 `
    -KeyUri $keyUri `
    -KeyvaultEncryption
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Azure CLı ile bir şifreleme kapsamı oluşturmak için önce Azure CLı sürüm 2.4.0 veya üstünü yüklemeniz gerekir.

### <a name="create-an-encryption-scope-protected-by-microsoft-managed-keys"></a>Microsoft tarafından yönetilen anahtarlarla korunan bir şifreleme kapsamı oluşturun

Microsoft tarafından yönetilen anahtarlarla korunan yeni bir şifreleme kapsamı oluşturmak için, parametresini olarak belirterek [az Storage Account ENCRYPTION Create](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-create) komutunu çağırın `--key-source` `Microsoft.Storage` . Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage account encryption-scope create \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.Storage
```

### <a name="create-an-encryption-scope-protected-by-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla korunan bir şifreleme kapsamı oluşturun

Microsoft tarafından yönetilen anahtarlarla korunan yeni bir şifreleme kapsamı oluşturmak için, parametresini olarak belirterek [az Storage Account ENCRYPTION Create](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-create) komutunu çağırın `--key-source` `Microsoft.Storage` . Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

Azure Key Vault ile müşteri tarafından yönetilen anahtarlarla korunan yeni bir şifreleme kapsamı oluşturmak için önce depolama hesabı için müşteri tarafından yönetilen anahtarları yapılandırın. Depolama hesabına yönetilen bir kimlik atamanız ve ardından, depolama hesabının bu hesaba erişim izinleri olması için Anahtar Kasası için erişim ilkesini yapılandırmak üzere yönetilen kimliği kullanmanız gerekir. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure Key Vault müşteri tarafından yönetilen anahtarları yapılandırma](../common/storage-encryption-keys-cli.md).

Müşteri tarafından yönetilen anahtarları bir şifreleme kapsamıyla kullanılmak üzere yapılandırmak için, anahtar kasasında hem **geçici silme** hem de **Temizleme koruma** özelliklerinin etkinleştirilmiş olması gerekir. Anahtar Kasası, depolama hesabıyla aynı bölgede olmalıdır. Daha fazla bilgi için bkz. [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](../common/encryption-customer-managed-keys.md).

Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az login
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity

storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)

az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group> \
    --object-id $storage_account_principal \
    --key-permissions get unwrapKey wrapKey
```

Sonra, parametresini kullanarak **az Storage Account ENCRYPTION Create** komutunu çağırın `--key-uri` ve anahtar URI 'sini belirtin. Anahtar URI 'sine anahtar sürümünü eklediğinizden emin olun. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage account encryption-scope create \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.KeyVault \
    --key-uri <key-uri>
```

---

## <a name="list-encryption-scopes-for-storage-account"></a>Depolama hesabı için şifreleme kapsamlarını Listele

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal bir depolama hesabının şifreleme kapsamlarını görüntülemek için, depolama hesabı için **şifreleme kapsamları** ayarına gidin. Bu bölmeden, bir şifreleme kapsamını etkinleştirebilir veya devre dışı bırakabilir ya da bir şifreleme kapsamının anahtarını değiştirebilirsiniz.

:::image type="content" source="media/encryption-scope-manage/list-encryption-scopes-portal.png" alt-text="Azure portal 'de şifreleme kapsamları listesini gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile bir depolama hesabı için kullanılabilir şifreleme kapsamlarını listelemek için Get-AzStorageEncryptionScope komutunu çağırın. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
Get-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

Bir kaynak grubundaki tüm şifreleme kapsamlarını depolama hesabına göre listelemek için aşağıdaki gibi ardışık düzen sözdizimini kullanın:

```powershell
Get-AzStorageAccount -ResourceGroupName $rgName | Get-AzStorageEncryptionScope
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Azure CLı ile bir depolama hesabı için kullanılabilen şifreleme kapsamlarını listelemek için [az Storage Account ENCRYPTION List](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-list) komutunu çağırın. Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage account encryption-scope list \
    --account-name <storage-account> \
    --resource-group <resource-group>
```

---

## <a name="create-a-container-with-a-default-encryption-scope"></a>Varsayılan şifreleme kapsamıyla kapsayıcı oluşturma

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal varsayılan şifreleme kapsamıyla bir kapsayıcı oluşturmak için, önce şifreleme kapsamı [oluşturma](#create-an-encryption-scope)bölümünde açıklandığı gibi şifreleme kapsamını oluşturun. Sonra, kapsayıcıyı oluşturmak için aşağıdaki adımları izleyin:

1. Depolama hesabınızdaki kapsayıcı listesine gidin ve yeni bir kapsayıcı oluşturmak için **Ekle** düğmesini seçin.
1. **Yeni kapsayıcı** bölmesinde **Gelişmiş** ayarlar ' ı genişletin.
1. **Şifreleme kapsamı** açılır penceresinde kapsayıcının varsayılan şifreleme kapsamını seçin.
1. Kapsayıcıdaki tüm Blobların varsayılan şifreleme kapsamını kullanmasını gerektirmek için, **Bu şifreleme kapsamını kapsayıcıdaki tüm Bloblar Için kullanmak**üzere onay kutusunu seçin. Bu onay kutusu işaretliyse, kapsayıcıdaki tek bir blob varsayılan şifreleme kapsamını geçersiz kılamaz.

    :::image type="content" source="media/encryption-scope-manage/create-container-default-encryption-scope.png" alt-text="Varsayılan şifreleme kapsamıyla kapsayıcıyı gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell ile varsayılan şifreleme kapsamıyla bir kapsayıcı oluşturmak için, parametresinin kapsamını belirterek [New-AzRmStorageContainer](/powershell/module/az.storage/new-azrmstoragecontainer) komutunu çağırın `-DefaultEncryptionScope` . **New-AzRmStorageContainer** komutu, şifreleme kapsamlarını ve diğer kaynak yönetimi işlemlerini yapılandırmaya Izin veren Azure depolama kaynak sağlayıcısı 'nı kullanarak bir kapsayıcı oluşturur.

Kapsayıcı tüm Blobların varsayılan kapsamını kullanmasını gerektirecek şekilde yapılandırılmadığı takdirde, tek bir blob kendi şifreleme kapsamı ile oluşturulabilir. Bir kapsayıcıdaki tüm Blobları kapsayıcının varsayılan kapsamını kullanacak şekilde zorlamak için `-PreventEncryptionScopeOverride` parametresini olarak ayarlayın `true` .

```powershell
$containerName1 = "container1"
$containerName2 = "container2"

# Create a container with a default encryption scope that cannot be overridden.
New-AzRmStorageContainer -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -Name $containerName1 `
    -DefaultEncryptionScope $scopeName1 `
    -PreventEncryptionScopeOverride $true
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Azure CLı ile varsayılan şifreleme kapsamıyla bir kapsayıcı oluşturmak için, parametresinin kapsamını belirterek [az Storage Container Create](/cli/azure/storage/container#az-storage-container-create) komutunu çağırın `--default-encryption-scope` . Kapsayıcı tüm Blobların varsayılan kapsamını kullanmasını gerektirecek şekilde yapılandırılmadığı takdirde, tek bir blob kendi şifreleme kapsamı ile oluşturulabilir. Bir kapsayıcıdaki tüm Blobları kapsayıcının varsayılan kapsamını kullanacak şekilde zorlamak için `--prevent-encryption-scope-override` parametresini olarak ayarlayın `true` .

Aşağıdaki örnek, kapsayıcıyı oluşturma işlemini yetkilendirmek için Azure AD hesabınızı kullanır. Hesap erişim anahtarını da kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md).

```azurecli-interactive
az storage container create \
    --account-name <storage-account> \
    --resource-group <resource-group> \
    --name <container> \
    --default-encryption-scope <scope> \
    --prevent-encryption-scope-override true \
    --auth-mode login
```

---

Bir istemci, bir blobu varsayılan şifreleme kapsamına sahip bir kapsayıcıya yüklerken bir kapsam belirtmeyi denerse ve kapsayıcı, blob 'ların varsayılan kapsamı geçersiz kılmasını önleyecek şekilde yapılandırıldıysa, işlem, isteğin kapsayıcı şifreleme ilkesi tarafından yasaklanmış olduğunu belirten bir iletiyle başarısız olur.

## <a name="upload-a-blob-with-an-encryption-scope"></a>Şifreleme kapsamına sahip bir blobu karşıya yükleme

Bir blobu karşıya yüklediğinizde, o blob için bir şifreleme kapsamı belirtebilir veya bir tane belirtilmişse kapsayıcı için varsayılan şifreleme kapsamını kullanabilirsiniz. 

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal belirtilen bir şifreleme kapsamına sahip bir blobu karşıya yüklemek için önce şifreleme kapsamını [oluşturma](#create-an-encryption-scope)bölümünde açıklandığı gibi şifreleme kapsamını oluşturun. Sonra, blobu oluşturmak için aşağıdaki adımları izleyin:

1. Blobu karşıya yüklemek istediğiniz kapsayıcıya gidin.
1. **Karşıya yükle** düğmesini seçin ve karşıya yüklenecek blobu bulun.
1. **Karşıya yükleme blobu** bölmesinde **Gelişmiş** ayarlar ' ı genişletin.
1. **Şifreleme kapsamı** açılan bölümünü bulun. Varsayılan olarak, blob belirtilmişse kapsayıcı için varsayılan şifreleme kapsamıyla oluşturulur. Kapsayıcı, Blobların varsayılan şifreleme kapsamını kullanmasını gerektiriyorsa, bu bölüm devre dışıdır.
1. Karşıya yüklediğiniz blob için farklı bir kapsam belirtmek üzere, **var olan bir kapsam**Seç ' i seçin ve ardından açılan listeden istediğiniz kapsamı seçin.

    :::image type="content" source="media/encryption-scope-manage/upload-blob-encryption-scope.png" alt-text="Bir blob 'u şifreleme kapsamıyla karşıya yüklemeyi gösteren ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell kullanarak belirtilen bir şifreleme kapsamına sahip bir blobu karşıya yüklemek için [set-AzStorageBlobContent](/powershell/module/az.storage/set-azstorageblobcontent) komutunu çağırın ve BLOB için şifreleme kapsamını belirtin.

```powershell
$containerName2 = "container2"
$localSrcFile = "C:\temp\helloworld.txt"
$ctx = (Get-AzStorageAccount -ResourceGroupName $rgName -StorageAccountName $accountName).Context

# Create a new container with no default scope defined.
New-AzStorageContainer -Name $containerName2 -Context $ctx
# Upload a block upload with an encryption scope specified.
Set-AzStorageBlobContent -Context $ctx -Container $containerName2 -File $localSrcFile -Blob "helloworld.txt" -BlobType Block -EncryptionScope $scopeName2
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Azure CLı kullanarak belirtilen bir şifreleme kapsamına sahip bir blobu karşıya yüklemek için [az Storage blob upload](/cli/azure/storage/blob#az-storage-blob-upload) komutunu çağırın ve BLOB için şifreleme kapsamını belirtin.

Azure Cloud Shell kullanıyorsanız, kök dizinde bir dosya oluşturmak için [blobu karşıya yükleme](storage-quickstart-blobs-cli.md#upload-a-blob) bölümünde açıklanan adımları izleyin. Ardından, aşağıdaki örneği kullanarak bu dosyayı bir bloba yükleyebilirsiniz.

```azurecli-interactive
az storage blob upload \
    --account-name <storage-account> \
    --container-name <container> \
    --file <file> \
    --name <file> \
    --encryption-scope <scope>
```

---

## <a name="change-the-encryption-key-for-a-scope"></a>Bir kapsamın şifreleme anahtarını değiştirme

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal bir kapsamı koruyan anahtarı değiştirmek için şu adımları izleyin:

1. Depolama hesabı için şifreleme kapsamları listesini görüntülemek için **şifreleme kapsamları** sekmesine gidin.
1. Değiştirmek istediğiniz kapsamın yanındaki **diğer** düğmesini seçin.
1. **Şifreleme kapsamını Düzenle** bölmesinde, şifreleme türünü Microsoft tarafından yönetilen anahtardan müşteri tarafından yönetilen anahtara veya bunun tersini değiştirebilirsiniz.
1. Müşteri tarafından yönetilen yeni bir anahtar seçmek için **Yeni bir anahtar kullan** ' ı seçin ve Anahtar Kasası, anahtar ve anahtar sürümünü belirtin.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Bir şifreleme kapsamını müşteri tarafından yönetilen anahtardan PowerShell ile Microsoft tarafından yönetilen bir anahtara koruyan anahtarı değiştirmek için, **Update-AzStorageEncryptionScope** komutunu çağırın ve `-StorageEncryption` parametresini geçirin:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName2 `
    -StorageEncryption
```

Bir şifreleme kapsamını Microsoft tarafından yönetilen anahtardan müşteri tarafından yönetilen bir anahtarla koruyan anahtarı değiştirmek için, önce depolama hesabı için Azure Key Vault ile müşteri tarafından yönetilen anahtarları etkinleştirdiğinizden emin olun. Daha fazla bilgi için bkz. [PowerShell kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma](../common/storage-encryption-keys-powershell.md). Ardından, **Update-AzStorageEncryptionScope** komutunu çağırın ve `-KeyUri` ve `-KeyvaultEncryption` parametrelerini geçirin:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -KeyUri $keyUri `
    -KeyvaultEncryption
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Bir şifreleme kapsamını müşteri tarafından yönetilen anahtardan Microsoft tarafından yönetilen bir anahtara Azure CLı ile koruyan anahtarı değiştirmek için [az Storage Account ENCRYPTION-Scope Update](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-update) komutunu çağırın ve `--key-source` parametreyi şu değerle geçirin `Microsoft.Storage` :

```azurecli-interactive
az storage account encryption-scope update \
    --account-name <storage-account> \
    --resource-group <resource-group>
    --name <scope> \
    --key-source Microsoft.Storage
```

Bir şifreleme kapsamını Microsoft tarafından yönetilen anahtardan müşteri tarafından yönetilen bir anahtarla koruyan anahtarı değiştirmek için, önce depolama hesabı için Azure Key Vault ile müşteri tarafından yönetilen anahtarları etkinleştirdiğinizden emin olun. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure Key Vault müşteri tarafından yönetilen anahtarları yapılandırma](../common/storage-encryption-keys-cli.md). Ardından, **az Storage Account ENCRYPTION-Scope Update** komutunu çağırın, `--key-uri` parametresini geçirin ve `--key-source` parametresini şu değerle geçirin `Microsoft.KeyVault` :

```powershell
az storage account encryption-scope update \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.KeyVault \
    --key-uri <key-uri>
```

---

## <a name="disable-an-encryption-scope"></a>Şifreleme kapsamını devre dışı bırakma

# <a name="portal"></a>[Portal](#tab/portal)

Azure portal bir şifreleme kapsamını devre dışı bırakmak için depolama hesabı için **şifreleme kapsamları** ayarına gidin, istenen şifreleme kapsamını seçin ve **devre dışı bırak**' ı seçin.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Bir şifreleme kapsamını PowerShell ile devre dışı bırakmak için, `-State` `disabled` Aşağıdaki örnekte gösterildiği gibi Update-AzStorageEncryptionScope komutunu çağırın ve değeri ile parametresini ekleyin. Bir şifreleme kapsamını yeniden etkinleştirmek için, parametresiyle aynı komutu çağırın `-State` `enabled` . Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -State disabled
```

# <a name="azure-cli"></a>[Azure CLI](#tab/cli)

Bir şifreleme kapsamını Azure CLı ile devre dışı bırakmak için, [az Storage Account ENCRYPTION-Scope Update](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-update) komutunu çağırın ve `--state` `Disabled` Aşağıdaki örnekte gösterildiği gibi, değerini içeren parametresini ekleyin. Bir şifreleme kapsamını yeniden etkinleştirmek için, parametresiyle aynı komutu çağırın `--state` `Enabled` . Örnekteki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
az storage account encryption-scope update \
    --account-name <storage-account> \
    --resource-group <resource-group> \
    --name <scope> \
    --state Disabled
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](../common/storage-service-encryption.md)
- [Azure depolama şifrelemesini yönetmek için Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanma](../common/encryption-customer-managed-keys.md)
