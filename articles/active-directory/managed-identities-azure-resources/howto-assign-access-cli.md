---
title: Azure CLı kullanarak bir kaynağa yönetilen kimlik erişimi atama-Azure AD
description: Adım adım yönergeler bir kaynakta bir yönetilen kimlik atamak için Azure CLI kullanarak başka bir kaynağa erişin.
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
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547369"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLI kullanarak bir kaynak için bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu örnek Azure CLI kullanarak Azure depolama hesabınız için bir Azure sanal makine veya sanal makine ölçek kümesi'nin yönetilen kimlik erişim vermek nasıl gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Azure portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - Yerel bir CLı konsolu kullanmayı tercih ediyorsanız [, en son Azure CLI sürümünü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli) . 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Azure [sanal makinesi](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra: 

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine veya sanal makine ölçek kümesini dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte biz bir depolama hesabı için bir Azure sanal makine erişimini vermiş olursunuz. İlk olarak, myVM adlı sanal makine için hizmet sorumlusunu almak üzere [az Resource List](/cli/azure/resource/#az-resource-list) kullanıyoruz:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için komutu burada dışında aynıdır, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu Al:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Hizmet sorumlusu KIMLIĞI ' ne sahip olduktan sonra, sanal makine veya sanal makine ölçek kümesinin "okuyucu" ' ı "myStorageAcct" adlı bir depolama hesabına erişimini sağlamak için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create) komutunu kullanın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için Yönetilen kimlikler](overview.md)
- Azure sanal makinesinde yönetilen kimliği etkinleştirmek için bkz. Azure [CLI kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vm.md).
- Azure sanal makine ölçek kümesinde yönetilen kimliği etkinleştirmek için bkz. [Azure CLI kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vmss.md).
