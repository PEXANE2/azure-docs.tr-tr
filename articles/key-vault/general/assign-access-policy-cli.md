---
title: Azure Key Vault erişim ilkesi atama (CLı)
description: Azure CLı kullanarak bir hizmet sorumlusu veya uygulama kimliğine Key Vault erişim ilkesi atama.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 32a323e8cc56a8c45b18737bb55ebe19d4be27c0
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89381250"
---
# <a name="assign-a-key-vault-access-policy"></a>Key Vault erişim ilkesi atama

Key Vault erişim ilkesi, belirli bir hizmet sorumlusunun bir uygulama veya Kullanıcı grubu tarafından Key Vault [gizli](../secrets/index.yml)diziler, [anahtarlar](../keys/index.yml)ve [Sertifikalar](../certificates/index.yml)üzerinde farklı işlemler yapıp gerçekleştiremeyeceğini belirler. [Azure Portal](assign-access-policy-portal.md), Azure CLI (Bu makale) veya [Azure PowerShell](assign-access-policy-powershell.md)kullanarak erişim ilkeleri atayabilirsiniz.

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Azure CLı kullanarak Azure Active Directory Grup oluşturma hakkında daha fazla bilgi için, bkz. [az Ad Group Create](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-create) ve [az Ad Group member Add](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add).

## <a name="configure-the-azure-cli-and-sign-in"></a>Azure CLı 'yı yapılandırma ve oturum açma

1. Azure CLı komutlarını yerel olarak çalıştırmak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)'yı yüklemelisiniz.
 
    Komutları bulutta doğrudan çalıştırmak için [Azure Cloud Shell](/azure/cloud-shell/overview)kullanın.

1. Yalnızca yerel CLı: kullanarak Azure 'da oturum açın `az login` :

    ```bash
    az login
    ```

    Bu `az login` komut, gerekirse kimlik bilgilerini toplamak için bir tarayıcı penceresi açar.

## <a name="acquire-the-object-id"></a>Nesne KIMLIĞINI edinin

Erişim ilkesini atamak istediğiniz uygulamanın, grubun veya kullanıcının nesne KIMLIĞINI belirleme:

- Uygulamalar ve diğer hizmet sorumluları: hizmet sorumlularını almak için [az ad SP List](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-list) komutunu kullanın. Erişim ilkesini atamak istediğiniz güvenlik sorumlusunun nesne KIMLIĞINI belirleme komutunun çıkışını inceleyin.

    ```azurecli-interactive
    az ad sp list --show-mine
    ```

- Gruplar: sonuçları parametresiyle filtreleyerek [az Ad Group List](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-list) komutunu kullanın `--display-name` :

     ```azurecli-interactive
    az ad group list --display-name <search-string>
    ```

- Kullanıcılar: Kullanıcı e-posta adresini parametreye geçirerek [az ad User Show](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-show) komutunu kullanın `--id` :

    ```azurecli-interactive
    az ad user show --id <email-address-of-user>
    ```

## <a name="assign-the-access-policy"></a>Erişim ilkesini atama
    
İstenen izinleri atamak için [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu kullanın:

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <object-id> --secret-permissions <secret-permissions> --key-permissions <key-permissions> --certificate-permissions <certificate-permissions>
```

`<object-id>`Hizmet sorumlunun nesne kimliğiyle değiştirin.

Yalnızca `--secret-permissions` `--key-permissions` `--certificate-permissions` Bu belirli türlere izinler atarken, ve dahil etmeniz gerekir. , Ve için izin verilen değerler, `<secret-permissions>` `<key-permissions>` `<certificate-permissions>` [az keykasa Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) belgelerinde verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Key Vault güvenliği: kimlik ve erişim yönetimi](overview-security.md#identity-and-access-management)
- [Anahtar kasanızın güvenliğini sağlayın](secure-your-key-vault.md).
- [Geliştirici Kılavuzu Azure Key Vault](developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](best-practices.md)
