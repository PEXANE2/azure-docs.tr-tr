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
ms.openlocfilehash: e1383fc6cca34cac141ce9f1316b4df0879900aa
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92892007"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-azure-cli"></a>Azure CLı kullanarak bir kaynağa yönetilen kimlik erişimi atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen kimliğe sahip bir Azure kaynağı yapılandırdıktan sonra, herhangi bir güvenlik sorumlusu gibi, yönetilen kimliğe başka bir kaynağa erişim izni verebilirsiniz. Bu örnek, Azure CLı kullanarak bir Azure sanal makinesine veya sanal makine ölçek kümesinin yönetilen kimlik erişiminin Azure depolama hesabına nasıl ekleneceğini gösterir.

Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilgi sahibi değilseniz bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](overview.md). Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik türleri hakkında bilgi edinmek için bkz. [yönetilen kimlik türleri](overview.md#managed-identity-types).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="use-azure-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynağa yönetilen kimlik erişimi atamak için Azure RBAC kullanma

Azure [sanal makinesi](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md)gibi bir Azure kaynağında yönetilen kimlik etkinleştirildikten sonra: 

1. Bu örnekte, bir depolama hesabına Azure sanal makine erişimi veriyoruz. İlk olarak, myVM adlı sanal makine için hizmet sorumlusunu almak üzere [az Resource List](/cli/azure/resource/#az-resource-list) kullanıyoruz:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için, komut burada, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu alırsınız:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

1. Hizmet sorumlusu KIMLIĞI ' ne sahip olduktan sonra, sanal makine veya sanal makine ölçek kümesinin "okuyucu" ' ı "myStorageAcct" adlı bir depolama hesabına erişimini sağlamak için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create) komutunu kullanın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için Yönetilen kimlikler](overview.md)
- Azure sanal makinesinde yönetilen kimliği etkinleştirmek için bkz. Azure [CLI kullanarak Azure VM 'de Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vm.md).
- Azure sanal makine ölçek kümesinde yönetilen kimliği etkinleştirmek için bkz. [Azure CLI kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-cli-windows-vmss.md).
