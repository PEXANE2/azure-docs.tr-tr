---
title: Kullanıcı tarafından atanan yönetilen kimliği yönetme-Azure CLı-Azure AD
description: Azure CLı kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme ve silme hakkında adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: be5defb85547e8750dea9ceaa481217aa40a004e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81639770"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Azure CLı kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme


Azure kaynakları için Yönetilen kimlikler, Azure Active Directory bir yönetilen kimliğe sahip Azure hizmetleri sağlar. Kodunuzda kimlik bilgileri gerekmeden Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulaması yapmak için bu kimliği kullanabilirsiniz. 

Bu makalede, Azure CLı kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturmayı, listeleyeceğinizi ve silmeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLı betiği örneklerini çalıştırmak için üç seçeneğiniz vardır:
    - Azure portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell her kod bloğunun sağ üst köşesinde bulunan "dene" düğmesini kullanarak kullanın.
    - Yerel bir CLı konsolu kullanmayı tercih ediyorsanız [Azure CLI 'nın en son sürümünü](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yükleyebilirsiniz. Kullanıcı tarafından atanan yönetilen kimliği `az login`dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesabı kullanarak Azure 'da oturum açın.


> [!NOTE]
> CLı kullanarak uygulama servivce sorumlusu kullanırken Kullanıcı izinlerini değiştirmek için, CLı 'nın Graph API yönelik GET istekleri gerçekleştirmesini sağlamak üzere Azure AD Graph API 'de hizmet sorumlusu ek izinlerini sağlamalısınız. Aksi takdirde, ' işlemi bitirmek için yeterli ayrıcalıklara sahip ' iletisini alabilirsiniz. Bunu yapmak için Azure Active Directory 'de uygulama kaydına gitmeniz gerekir, uygulamanızı seçin, API izinleri ' ne tıklayın, aşağı kaydırın ve Azure Active Directory Graf ' ı seçin. Uygulama izinlerini seçin ve ardından uygun izinleri ekleyin. 



[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için [az Identity Create](/cli/azure/identity#az-identity-create) komutunu kullanın. `-g` Parametresi, Kullanıcı tarafından atanan yönetilen kimliğin oluşturulacağı kaynak grubunu belirtir ve `-n` parametresi adını belirtir. `<RESOURCE GROUP>` Ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirin:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listeleme

Kullanıcı tarafından atanan yönetilen kimliği listelemek/okumak için hesabınızın [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü ataması gerekir.

Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için [az Identity List](/cli/azure/identity#az-identity-list) komutunu kullanın. Değerini kendi `<RESOURCE GROUP>` değeri ile değiştirin:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
JSON yanıtında, Kullanıcı tarafından atanan yönetilen kimliklerin anahtar için döndürülen `"Microsoft.ManagedIdentity/userAssignedIdentities"` değeri vardır `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan bir yönetilen kimliği silmek için [az Identity Delete](/cli/azure/identity#az-identity-delete) komutunu kullanın.  -N parametresi adını belirtir ve-g parametresi, Kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtir. `<USER ASSIGNED IDENTITY NAME>` Ve `<RESOURCE GROUP>` parametrelerinin değerlerini kendi değerlerinizle değiştirin:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Kullanıcı tarafından atanan bir yönetilen kimliğin silinmesi başvuruyu, atandığı herhangi bir kaynaktan kaldırmaz. Lütfen `az vm/vmss identity remove` komutunu kullanarak VM/VMSS 'den bunları kaldırın

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kimlik komutlarının tam listesi için, bkz. [az Identity](/cli/azure/identity).

Azure VM 'ye Kullanıcı tarafından atanan yönetilen kimlik atama hakkında daha fazla bilgi için bkz. Azure [CLI kullanarak Azure VM 'de Azure kaynakları için Yönetilen kimlikler yapılandırma](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 
