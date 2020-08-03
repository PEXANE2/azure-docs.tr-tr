---
title: Müşteri tarafından yönetilen anahtarları yapılandırmak için Azure CLı 'yi kullanma
titleSuffix: Azure Storage
description: Azure CLı kullanarak müşteri tarafından yönetilen anahtarları Azure depolama şifrelemesi için Azure Key Vault ile yapılandırma hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/13/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-azurecli
ms.openlocfilehash: 351fe5acd8d607b5b60817c235161ac09e530e99
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495021"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-azure-cli"></a>Azure CLı kullanarak, müşteri tarafından yönetilen anahtarları Azure Key Vault ile yapılandırma

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

Bu makalede, Azure CLı kullanarak müşteri tarafından yönetilen anahtarlarla bir Azure Key Vault nasıl yapılandırılacağı gösterilmektedir. Azure CLı kullanarak bir Anahtar Kasası oluşturmayı öğrenmek için bkz. [hızlı başlangıç: Azure CLI kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](../../key-vault/secrets/quick-create-cli.md).

## <a name="assign-an-identity-to-the-storage-account"></a>Depolama hesabına bir kimlik atayın

Depolama hesabınız için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere öncelikle depolama hesabına sistem tarafından atanan bir yönetilen kimlik atayın. Bu yönetilen kimliği, depolama hesabı izinlerini anahtar kasasına erişim vermek için kullanacaksınız.

Azure CLı kullanarak yönetilen bir kimlik atamak için [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update)' i çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az login
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Azure CLı ile sistem tarafından atanan yönetilen kimlikleri yapılandırma hakkında daha fazla bilgi için bkz. Azure [CLI kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Yeni bir Anahtar Kasası oluşturun

Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları depolamak üzere kullandığınız anahtar kasasında iki anahtar koruma ayarı etkinleştirilmiş, **geçici silme** ve **Temizleme işlemi**yapılmalıdır. PowerShell veya Azure CLı kullanarak bu ayarların etkinleştirildiği yeni bir Anahtar Kasası oluşturmak için aşağıdaki komutları yürütün. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

Azure CLı kullanarak yeni bir Anahtar Kasası oluşturmak için [az keykasa Create komutunu](/cli/azure/keyvault#az-keyvault-create)çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

**Geçici silme** özelliğini etkinleştirme ve Azure CLI ile mevcut bir anahtar kasasında **Temizleme** hakkında BILGI edinmek için, [CLI ile geçici silme özelliğini kullanma](../../key-vault/general/soft-delete-cli.md)bölümünde **geçici** silme ve **Temizleme korumasını** etkinleştirme başlıklı bölümlere bakın.

## <a name="configure-the-key-vault-access-policy"></a>Anahtar Kasası erişim ilkesini yapılandırma

Daha sonra, depolama hesabının bu hesaba erişim izinleri olması için Anahtar Kasası için erişim ilkesini yapılandırın. Bu adımda, daha önce depolama hesabına atadığınız yönetilen kimliği kullanacaksınız.

Anahtar Kasası için erişim ilkesini ayarlamak için [az keykasa Set-Policy komutunu](/cli/azure/keyvault#az-keyvault-set-policy)çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

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
    --key-permissions get unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Yeni anahtar oluştur

Ardından, anahtar kasasında bir anahtar oluşturun. Bir anahtar oluşturmak için [az keykasa Key Create komutunu](/cli/azure/keyvault/key#az-keyvault-key-create)çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az keyvault key create \
    --name <key> \
    --vault-name <key-vault>
```

Azure depolama şifrelemesi, 2048, 3072 ve 4096 boyutlarının RSA ve RSA-HSM anahtarlarını destekler. Anahtarlar hakkında daha fazla bilgi için bkz. [Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys) **Key Vault anahtarlar** .

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Varsayılan olarak, Azure depolama şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure depolama hesabınızı Azure Key Vault ile müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve ardından depolama hesabıyla ilişkilendirilecek anahtarı belirtin.

Şifrelemeyi müşteri tarafından yönetilen anahtarlarla yapılandırdığınızda, ilişkili anahtar kasasındaki sürüm değiştiğinde şifreleme için kullanılan anahtarı otomatik olarak döndürmeyi tercih edebilirsiniz. Alternatif olarak, anahtar sürümü el ile güncelleştirilene kadar şifreleme için kullanılacak bir anahtar sürümünü açıkça belirtebilirsiniz.

### <a name="configure-encryption-for-automatic-rotation-of-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarların otomatik olarak dönmesi için şifrelemeyi yapılandırma

Müşteri tarafından yönetilen anahtarların otomatik dönüşüyle ilgili şifrelemeyi yapılandırmak için [Azure CLI sürüm 2.4.0](/cli/azure/release-notes-azure-cli#april-21-2020) veya üstünü yükler. Daha fazla bilgi için bkz. [Azure CLI 'Yı yüklerken](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Müşteri tarafından yönetilen anahtarları otomatik olarak döndürmek için, depolama hesabı için müşteri tarafından yönetilen anahtarları yapılandırırken anahtar sürümünü atlayın. Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) ' i çağırın. Parametresi dahil edin `--encryption-key-source` ve `Microsoft.Keyvault` hesap için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere olarak ayarlayın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

### <a name="configure-encryption-for-manual-rotation-of-key-versions"></a>Anahtar sürümlerinin el ile dönmesi için şifrelemeyi yapılandırma

Şifreleme için kullanılacak bir anahtar sürümünü açıkça belirtmek için, depolama hesabı için müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırırken anahtar sürümünü sağlayın. Aşağıdaki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını güncelleştirmek için [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) ' i çağırın. Parametresi dahil edin `--encryption-key-source` ve `Microsoft.Keyvault` hesap için müşteri tarafından yönetilen anahtarları etkinleştirmek üzere olarak ayarlayın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

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

Anahtar sürümünü el ile döndürdüğünüzde, yeni sürümü kullanmak için depolama hesabının şifreleme ayarlarını güncelleştirmeniz gerekir. İlk olarak, az keykasa [Show](/cli/azure/keyvault#az-keyvault-show)komutunu çağırarak Anahtar Kasası URI 'sini sorgulama, [az keykasa anahtar listesi-sürümlerini](/cli/azure/keyvault/key#az-keyvault-key-list-versions)çağırarak anahtar sürümü için. Ardından, önceki örnekte gösterildiği gibi, depolama hesabının şifreleme ayarlarını anahtarın yeni sürümünü kullanacak şekilde güncelleştirmek için [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) ' i çağırın.

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Azure depolama şifrelemesi için kullanılan anahtarı değiştirmek için, [müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma](#configure-encryption-with-customer-managed-keys) bölümünde gösterildiği gibi [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) ' i çağırın ve yeni anahtar adını ve sürümünü sağlayın. Yeni anahtar farklı bir anahtar kasasında ise, Anahtar Kasası URI 'sini de güncelleştirin.

## <a name="revoke-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları iptal et

Anahtar Kasası erişim ilkesini kaldırarak müşteri tarafından yönetilen anahtarları iptal edebilirsiniz. Müşteri tarafından yönetilen bir anahtarı iptal etmek için, aşağıdaki örnekte gösterildiği gibi [az keykasa Delete-Policy](/cli/azure/keyvault#az-keyvault-delete-policy) komutunu çağırın. Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```azurecli-interactive
az keyvault delete-policy \
    --name <key-vault> \
    --object-id $storage_account_principal
```

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı bırak

Müşteri tarafından yönetilen anahtarları devre dışı bıraktığınızda, depolama hesabınız Microsoft tarafından yönetilen anahtarlarla yeniden şifrelenir. Müşteri tarafından yönetilen anahtarları devre dışı bırakmak için, aşağıdaki örnekte gösterildiği gibi [az Storage Account Update](/cli/azure/storage/account#az-storage-account-update) 'i çağırın ve `--encryption-key-source parameter` öğesini olarak ayarlayın `Microsoft.Storage` . Köşeli ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi ve önceki örneklerde tanımlanan değişkenleri kullanmayı unutmayın.

```azurecli-interactive
az storage account update
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-source Microsoft.Storage
```

## <a name="next-steps"></a>Sonraki adımlar

- [Bekleyen veri için Azure Depolama şifrelemesi](storage-service-encryption.md) 
- [Azure Key Vault nedir](https://docs.microsoft.com/azure/key-vault/key-vault-overview)?
