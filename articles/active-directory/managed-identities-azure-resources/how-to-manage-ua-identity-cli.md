---
title: Kullanıcı tarafından atanan yönetilen kimliği yönetme - Azure CLI - Azure AD
description: Azure CLI'yi kullanarak kullanıcı tarafından atanan yönetilen bir kimliğin nasıl oluşturulacağına, listelenene ve silinmeye ilişkin adım adım yönergeler.
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
ms.date: 10/15/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1e8587562ff452373fe2ee3b98fa20309e77cc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266591"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Azure CLI'yi kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturun, listelenin veya silin


Azure kaynakları için yönetilen kimlikler Azure hizmetlerine Azure Etkin Dizini'nde yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgilerine gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure CLI kullanarak kullanıcı tarafından atanan yönetilen bir kimliği nasıl oluşturabileceğinizi, listeleyip silebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI komut dosyası örneklerini çalıştırmak için üç seçeneğiniz var:
    - Azure portalından [Azure Bulut Shell'i](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
    - Yerel bir CLI konsolu kullanmak isterseniz [Azure CLI'nin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonraki) en son sürümünü yükleyin. Kullanıcı tarafından atanan `az login`yönetilen kimliği dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesabı kullanarak Azure'da oturum açın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için, hesabınızın [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) komutunu kullanın. Parametre, `-g` kullanıcı tarafından atanan yönetilen kimliğin oluşturulacağı kaynak grubunu `-n` belirtir ve parametre adını belirtir. `<RESOURCE GROUP>` Ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirin:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listele

Kullanıcı tarafından atanan yönetilen bir kimliği listelemek/okumak için hesabınızın [Yönetilen Kimlik Operatörü'ne](/azure/role-based-access-control/built-in-roles#managed-identity-operator) veya Yönetilen [Kimlik Katkıda Bulunan](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için [az kimlik listesi](/cli/azure/identity#az-identity-list) komutunu kullanın. Kendi `<RESOURCE GROUP>` değerinizle değiştirin:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
json yanıtında, kullanıcı tarafından atanan `"Microsoft.ManagedIdentity/userAssignedIdentities"` yönetilen kimliklerin `type`değeri anahtar için döndürülür, .

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği silme

Kullanıcı tarafından atanan yönetilen bir kimliği silmek için, hesabınızın [Yönetilen Kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen bir kimliği silmek için [az identity delete](/cli/azure/identity#az-identity-delete) komutunu kullanın.  -n parametresi adını belirtir ve -g parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtir. `<USER ASSIGNED IDENTITY NAME>` Ve `<RESOURCE GROUP>` parametreler değerlerini kendi değerlerinizle değiştirin:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Kullanıcı tarafından atanan yönetilen bir kimliğisiletmek, atandığı herhangi bir kaynaktan başvuruyu kaldırmaz. Lütfen komutu `az vm/vmss identity remove` kullanarak VM/VMSS'den kaldırın

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI kimlik komutlarının tam listesi için [az kimliğine](/cli/azure/identity)bakın.

Kullanıcı tarafından atanan yönetilen bir kimliği Azure VM'ye nasıl atayacağıma ilişkin bilgi için, [Azure CLI'yi kullanarak Azure VM'deki Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 
