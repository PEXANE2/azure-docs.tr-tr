---
title: Azure CLI - Azure AD'yi kullanarak kaynağa yönetilen bir kimlik erişimi atama
description: Azure CLI'yi kullanarak bir kaynağa yönetilen bir kimlik atama, başka bir kaynağa erişim için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: b241ac223fd1eb9df2b0a914726d8f37df5f4d88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74547369"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLI'yi kullanarak kaynağa yönetilen bir kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağını yönetilen bir kimliğe göre yapılandırdıktan sonra, yönetilen kimlik erişimini herhangi bir güvenlik ilkesi gibi başka bir kaynağa verebilirsiniz. Bu örnek, Azure CLI kullanarak bir Azure sanal makineveya sanal makine ölçeği kümesinin yönetilen kimlik erişimini Azure depolama hesabına nasıl vereceğinizgösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI komut dosyası örneklerini çalıştırmak için üç seçeneğiniz var:
    - Azure portalından [Azure Bulut Shell'i](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesi aracılığıyla gömülü Azure Bulut Kabuğu'nu kullanın.
    - Yerel bir CLI konsolu kullanmayı tercih ediyorsanız [Azure CLI'nin en son sürümünü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli) 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için RBAC'ı kullanma

[Azure sanal makine](qs-configure-cli-windows-vm.md) veya Azure sanal [makine ölçeği kümesi](qs-configure-cli-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimliği etkinleştirdikten sonra: 

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM veya sanal makine ölçeği kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesap kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte, bir depolama hesabına Azure sanal makine erişimi veriyoruz. Öncelikle myVM adlı sanal makinenin servis sorumlusunu almak için [az kaynak listesini](/cli/azure/resource/#az-resource-list) kullanıyoruz:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Azure sanal makine ölçeği kümesi için komut aynıdır, ancak burada "DevTestVMSS" adlı sanal makine ölçeği kümesinin servis ilkesini alırsınız:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Hizmet ana kimliğini aldıktan sonra, sanal makineye veya sanal makine ölçeğine "Reader" kümesi "Reader" erişimini "myStorageAcct" adlı bir depolama hesabına erişmek için [az rol ataması oluşturun'](/cli/azure/role/assignment#az-role-assignment-create) kullanın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler](overview.md)
- Azure sanal makinesinde yönetilen kimliği etkinleştirmek için Azure [CLI'yi kullanarak Azure VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vm.md)'ya bakın.
- Azure sanal makine ölçeğinde yönetilen kimliği etkinleştirmek [için, Azure CLI'yi kullanarak sanal makine ölçeğinde Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](qs-configure-cli-windows-vmss.md)bakın.
