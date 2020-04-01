---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure CLI'yi kullanın
titleSuffix: Azure Storage
description: Azure Depolama şifrelemesi için Azure Key Vault ile müşteri tarafından yönetilen anahtarları yapılandırmak için Azure CLI'yi nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/31/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: c20fcdf6a725da9d7b053f0ad98efec6aca88f81
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478204"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-azure-cli"></a>Azure CLI'yi kullanarak müşteri tarafından yönetilen anahtarları Azure Anahtar Kasası ile yapılandırın

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, Azure CLI kullanarak müşteri tarafından yönetilen anahtarlarla bir Azure Anahtar Kasası nasıl yapılandırılanın gösterilmektedir. Azure CLI kullanarak anahtar kasası oluşturmayı öğrenmek için [Quickstart: Azure CLI'yi kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../../key-vault/quick-create-cli.md)

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabına kimlik atama

Depolama hesabınız için müşteri tarafından yönetilen anahtarları etkinleştirmek için, önce depolama hesabına sistem tarafından atanmış yönetilen bir kimlik atayın. Bu yönetilen kimliği, depolama hesabının anahtar kasasına erişme izni vermek için kullanırsınız.

Azure CLI kullanarak yönetilen bir kimlik atamak için [az depolama hesabı güncelleştirmesini](/cli/azure/storage/account#az-storage-account-update)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Sistem tarafından atanan yönetilen kimlikleri Azure CLI ile yapılandırma hakkında daha fazla bilgi için, [Azure CLI'yi kullanarak Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)'ya bakın.

## <a name="create-a-new-key-vault"></a>Yeni bir anahtar kasası oluşturma

Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak için kullandığınız anahtar kasasının iki anahtar koruma ayarı etkin leştirilmiş olmalıdır: **Yumuşak Silme** ve **Temizleme).** PowerShell veya Azure CLI'yi kullanarak bu ayarları etkinleştiren yeni bir anahtar kasası oluşturmak için aşağıdaki komutları uygulayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

Azure CLI kullanarak yeni bir anahtar tonozu oluşturmak için [az keyvault oluştur'u](/cli/azure/keyvault#az-keyvault-create)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

Azure CLI ile varolan bir anahtar kasasında **Yumuşak Silme** ve **Temizleme'yi** etkinleştirmeyi öğrenmek için, [CLI ile yumuşak silmeyi etkinleştirme](../../key-vault/key-vault-soft-delete-cli.md)ve Temizleme **Korumasını Etkinleştirme** başlıklı bölümlere bakın. **Enabling soft-delete**

## <a name="configure-the-key-vault-access-policy"></a>Anahtar kasa erişim ilkesini yapılandırma

Ardından, depolama hesabının erişim izinleri olacak şekilde anahtar kasasının erişim ilkesini yapılandırın. Bu adımda, depolama hesabına daha önce atadığınız yönetilen kimliği kullanırsınız.

Anahtar kasasının erişim ilkesini ayarlamak için [az keyvault ayar-ilkesini](/cli/azure/keyvault#az-keyvault-set-policy)arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)
az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group>
    --object-id $storage_account_principal \
    --key-permissions get recover unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Yeni bir anahtar oluşturma

Ardından, anahtar kasasında bir anahtar oluşturun. Bir anahtar oluşturmak için [az keyvault tuşunu arayın.](/cli/azure/keyvault/key#az-keyvault-key-create) Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault key create
    --name <key> \
    --vault-name <key-vault>
```

Azure Depolama şifrelemesi ile yalnızca 2048 bit RSA ve RSA-HSM anahtarları desteklenir. Anahtarlar hakkında daha fazla bilgi için [Azure Key Vault tuşları, sırlar ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys)Key Vault **tuşlarına** bakın.

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Varsayılan olarak, Azure Depolama şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Azure Depolama hesabınızı müşteri tarafından yönetilen anahtarlar için yapılandırın ve depolama hesabıyla ilişkilendirilen anahtarı belirtin.

Depolama hesabının şifreleme ayarlarını güncelleştirmek için aşağıdaki örnekte gösterildiği gibi [az depolama hesabı güncelleştirmesini](/cli/azure/storage/account#az-storage-account-update)arayın. Parametreyi `--encryption-key-source` ekleyin ve `Microsoft.Keyvault` depolama hesabı için müşteri tarafından yönetilen anahtarları etkinleştirecek şekilde ayarlayın. Örnek, anahtarı depolama hesabıyla ilişkilendirmek için her iki değerde de gerekli olan anahtar tonoz u ve en son anahtar sürümünü de sorgular. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
key_version=$(az keyvault key list-versions \
    --name <key> \
    --vault-name <key-vault> \
    --query [-1].kid \
    --output tsv | cut -d '/' -f 6)
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-version $key_version \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için depolama hesabını güncelleştirmeniz gerekir. İlk olarak, [az keyvault show'u](/cli/azure/keyvault#az-keyvault-show)arayarak uri anahtar kasası ve az [keyvault anahtar listesi sürümlerini](/cli/azure/keyvault/key#az-keyvault-key-list-versions)arayarak anahtar sürümü için sorgula. Ardından, önceki bölümde gösterildiği gibi anahtarın yeni sürümünü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmek için [az depolama hesabı güncelleştirmesini](/cli/azure/storage/account#az-storage-account-update) arayın.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure Depolama şifrelemesi için kullanılan anahtarı değiştirmek için, [müşteri tarafından yönetilen anahtarlarla yapılandırma şifrelemesinde](#configure-encryption-with-customer-managed-keys) gösterildiği gibi az depolama hesabı [güncelleştirmesini](/cli/azure/storage/account#az-storage-account-update) arayın ve yeni anahtar adı ve sürümünü sağlayın. Yeni anahtar farklı bir anahtar kasasındaysa, uri anahtar kasasını da güncelleyin.

## <a name="revoke-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları iptal edin

Bir anahtarın ele geçirildiğini düşünüyorsanız, anahtar kasa erişim ilkesini kaldırarak müşteri tarafından yönetilen anahtarları iptal edebilirsiniz. Müşteri tarafından yönetilen bir anahtarı iptal etmek için aşağıdaki örnekte gösterildiği gibi [az keyvault silme ilkesi](/cli/azure/keyvault#az-keyvault-delete-policy) komutunu arayın. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```azurecli-interactive
az keyvault delete-policy \
    --name <key-vault> \
    --object-id $storage_account_principal
```

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı

Müşteri tarafından yönetilen anahtarları devre dışı dışı bettiğinizde, depolama hesabınız bir kez daha Microsoft tarafından yönetilen anahtarlarla şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı katmak için `--encryption-key-source parameter` az `Microsoft.Storage`depolama [hesabı güncelleştirmesini](/cli/azure/storage/account#az-storage-account-update) arayın ve aşağıdaki örnekte gösterildiği gibi , Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```azurecli-interactive
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-source Microsoft.Storage
```

## <a name="next-steps"></a>Sonraki adımlar

- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md) 
- [Azure Anahtar Kasası Nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
