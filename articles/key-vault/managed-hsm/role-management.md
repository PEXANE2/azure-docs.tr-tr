---
title: Yönetilen HSM veri düzlemi rol yönetimi-Azure Key Vault | Microsoft Docs
description: Yönetilen HSM 'nizin rol atamalarını yönetmek için bu makaleyi kullanın
services: key-vault
author: amitbapat
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 6654b97f914ce4c1e3e55d38f47bd5bde0a4891e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91000882"
---
# <a name="managed-hsm-role-management"></a>Yönetilen HSM rol yönetimi

> [!NOTE]
> Key Vault iki tür kaynak destekler: kasaların ve yönetilen HSM 'ler. Bu makale, **YÖNETILEN HSM**ile ilgilidir. Kasayı yönetme hakkında bilgi edinmek istiyorsanız lütfen bkz. [Azure CLI kullanarak Key Vault yönetme](../general/manage-with-cli2.md).

Yönetilen HSM 'ye genel bakış için bkz. [YÖNETILEN HSM nedir?](overview.md). Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu makalede, yönetilen bir HSM veri düzlemi için rolleri nasıl yöneteceğiniz gösterilmektedir. Yönetilen HSM erişim denetimi modeli hakkında bilgi edinmek için bkz. [YÖNETILEN HSM erişim denetimi](access-control.md).

Bir güvenlik sorumlusuna (örneğin, bir Kullanıcı, hizmet sorumlusu, Grup veya yönetilen kimlik) yönetilen HSM veri düzlemi işlemleri gerçekleştirmesini sağlamak için, bu işlemleri gerçekleştirmeye izin veren bir rol atanması gerekir. Örneğin, bir uygulamanın anahtar kullanarak bir imzalama işlemi gerçekleştirmesine izin vermek istiyorsanız, veri eylemlerinden biri olarak "Microsoft. Keykasası/managedHSM/Keys/Sign/Action" öğesini içeren bir rol atanması gerekir. Bir rol, belirli bir kapsamda atanabilir. Yönetilen HSM yerel RBAC, HSM genelinde iki kapsam ( `/` veya `/keys` ) ve anahtar başına () destekler `/keys/<keyname>` .

Tüm yönetilen HSM yerleşik rollerinin ve izin verdikleri işlemlerin listesi için bkz. [YÖNETILEN HSM yerleşik rolleri](built-in-roles.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki Azure CLı komutlarını kullanmak için aşağıdaki öğelere sahip olmanız gerekir:

* Bir Microsoft Azure aboneliği. Hesabınız yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/pricing/free-trial).
* Azure CLı sürüm 2.12.0 veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).
* Aboneliğinizde yönetilen bir HSM. Bkz. [hızlı başlangıç: Azure CLI kullanarak](quick-create-cli.md) yönetilen bir HSM sağlama ve etkinleştirme.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest&preserve-view=true)

## <a name="create-a-new-role-assignment"></a>Yeni bir rol ataması oluştur

### <a name="assign-roles-for-all-keys"></a>Tüm anahtarlar için roller atama

`az keyvault role assignment create` **Managed HSM Crypto Officer** **user2@contoso.com** ContosoHSM içindeki tüm **anahtarlar** (kapsam) için Kullanıcı asıl adı tarafından tanımlanan kullanıcıya yönetilen bir HSM şifre yetkilisi rolü atamak için komutunu kullanın `/keys` .

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys
```

### <a name="assign-role-for-a-specific-key"></a>Belirli bir anahtar için rol ata

`az keyvault role assignment create` **Managed HSM Crypto Officer** **user2@contoso.com** **Myrsakey**adlı belirli bir anahtar için Kullanıcı asıl ADıNA göre tanımlanan kullanıcıya yönetilen bir HSM şifre müdürü rolü atamak için komutunu kullanın.

```azurecli-interactive
az keyvault role assignment create --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey
```

## <a name="list-existing-role-assignments"></a>Varolan rol atamalarını listeleyin

`az keyvault role assignment list`Rol atamalarını listelemek için kullanın.

Tüm kullanıcılar için kapsam/(hiçbir kapsam belirtilmediğinde varsayılan) tüm rol atamaları (varsayılan olarak--atane belirtildiğinde)

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM
```

Tüm rol atamaları belirli bir kullanıcı için HSM düzeyinde **user1@contoso.com** .

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user@contoso.com
```

Belirli bir anahtar için belirli bir kullanıcı için tüm rol atamaları **user2@contoso.com** **myrsakey**.

```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey
```

Belirli bir **Managed HSM Crypto Officer** **user2@contoso.com** anahtar **myrsakey** için belirli BIR kullanıcı Için rol yönetimli HSM şifre müdürü için belirli bir rol ataması


```azurecli-interactive
az keyvault role assignment list --hsm-name ContosoMHSM --assignee user2@contoso.com --scope /keys/myrsakey --role "Managed HSM Crypto Officer"
```

## <a name="delete-a-role-assignment"></a>Rol atamasını silme

`az keyvault role assignment delete`Key myrsakey2 için kullanıcıya atanan bir **yönetilen HSM şifre Müdürü** rolünü silmek için komutunu kullanın **user2@contoso.com** . **myrsakey2**

```azurecli-interactive
az keyvault role assignment delete --hsm-name ContosoMHSM --role "Managed HSM Crypto Officer" --assignee user2@contoso.com  --scope /keys/myrsakey2
```

## <a name="list-all-available-role-definitions"></a>Tüm kullanılabilir rol tanımlarını listeleyin

`az keyvault role definition list`Tüm rol tanımlarını listelemek için komutunu kullanın.

```azurecli-interactive
az keyvault role definition list --hsm-name ContosoMHSM
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure rol tabanlı erişim denetimine (RBAC)](../../role-based-access-control/overview.md)genel bakış.
- [YÖNETILEN HSM rol yönetimi](role-management.md) hakkında öğreticiye bakın
- [YÖNETILEN HSM erişim denetimi modeli](access-control.md) hakkında daha fazla bilgi edinin
- [YÖNETILEN HSM yerel RBAC için yerleşik rollere](built-in-roles.md) bakın