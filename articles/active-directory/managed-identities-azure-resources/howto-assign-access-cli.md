---
title: Azure CLı kullanarak bir kaynağa yönetilen kimlik erişimi atama-Azure AD
description: Azure CLı kullanarak bir kaynakta yönetilen kimlik atamaya, başka bir kaynağa erişime yönelik adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurecli
ms.openlocfilehash: b0437308a0495281e364d42199cc84d9a291cb58
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89263424"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLı kullanarak bir kaynağa yönetilen kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimliğe sahip bir Azure kaynağı yapılandırdıktan sonra, herhangi bir güvenlik sorumlusu gibi, yönetilen kimliğe başka bir kaynağa erişim izni verebilirsiniz. Bu örnek, Azure CLı kullanarak bir Azure sanal makinesine veya sanal makine ölçek kümesinin yönetilen kimlik erişiminin Azure depolama hesabına nasıl ekleneceğini gösterir.

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#managed-identity-types)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- CLı betiği örneklerini çalıştırmak için üç seçeneğiniz vardır:
    - Azure portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell her kod bloğunun sağ üst köşesinde bulunan "dene" düğmesini kullanarak kullanın.
    - Yerel bir CLı konsolu kullanmayı tercih ediyorsanız [, en son Azure CLI sürümünü yükleyebilirsiniz](/cli/azure/install-azure-cli) . 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için Azure RBAC kullanma

Azure [sanal makinesi](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra: 

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. VM veya sanal makine ölçek kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte, bir depolama hesabına Azure sanal makine erişimi veriyoruz. İlk olarak, myVM adlı sanal makine için hizmet sorumlusunu almak üzere [az Resource List](/cli/azure/resource/#az-resource-list) kullanıyoruz:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için, komut burada, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu alırsınız:
   
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