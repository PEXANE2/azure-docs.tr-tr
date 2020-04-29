---
title: Azure Resource Manager kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma & silme
description: Azure Resource Manager kullanarak Kullanıcı tarafından atanan Yönetilen kimlikler oluşturma ve silme hakkında adım adım yönergeler.
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
ms.date: 12/10/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 244965da4e22c0808fd1ea9088aa182b27eaf484
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79253383"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Azure Resource Manager kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme ve silme


Azure kaynakları için Yönetilen kimlikler, Azure Active Directory bir yönetilen kimliğe sahip Azure hizmetleri sağlar. Kodunuzda kimlik bilgileri gerekmeden Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulaması yapmak için bu kimliği kullanabilirsiniz. 

Bu makalede, bir Azure Resource Manager kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturursunuz.

Bir Azure Resource Manager şablonu kullanarak Kullanıcı tarafından atanan yönetilen kimliği listelemek ve silmek mümkün değildir.  Kullanıcı tarafından atanan yönetilen kimlik oluşturmak ve listelemek için aşağıdaki makalelere bakın:

- [Kullanıcı tarafından atanan yönetilen kimliği listeleme](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Kullanıcı tarafından atanan yönetilen kimliği Sil](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
  ## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. ** [Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenlemesi

Azure portal ve betikte olduğu gibi Azure Resource Manager şablonlar, bir Azure Kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Aşağıdakiler dahil olmak üzere hem yerel hem de portal tabanlı şablon düzenlemesi ve dağıtımı için çeşitli seçenekler mevcuttur:

- Sıfırdan bir şablon oluşturmanıza veya mevcut bir ortak ya da [hızlı başlangıç şablonuna](https://azure.microsoft.com/documentation/templates/)temeletmenize olanak tanıyan [Azure Marketi 'nden özel bir şablon](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanma.
- Varolan bir kaynak grubundan türeterek, [orijinal dağıtımdan](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates)veya [dağıtımın geçerli durumundan](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates)bir şablonu dışarı aktararak.
- Yerel bir [JSON Düzenleyicisi (vs Code gibi)](../../azure-resource-manager/resource-manager-create-first-template.md)kullanarak ve POWERSHELL veya CLI kullanarak karşıya yükleme ve dağıtma.
- Bir şablon oluşturmak ve dağıtmak için Visual Studio [Azure Kaynak grubu projesini](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanma. 

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için hesabınıza [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması gerekir.

Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için aşağıdaki şablonu kullanın. `<USER ASSIGNED IDENTITY NAME>` Değeri kendi değerlerinizle değiştirin:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2018-11-30",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Resource Manager şablonu kullanarak bir Azure VM 'sine Kullanıcı tarafından atanan yönetilen kimlik atama hakkında bilgi için, bkz. bir [Azure VM 'de Azure kaynakları için yönetilen kimlikleri şablonlar kullanarak yapılandırma](qs-configure-template-windows-vm.md).


 
