---
title: Azure Kaynak Yöneticisi'ni kullanarak kullanıcı tarafından atanan yönetilen bir kimliği & silme
description: Azure Kaynak Yöneticisi'ni kullanarak kullanıcı tarafından atanan yönetilen kimliklerin nasıl oluşturulacağına ve silinene ilişkin adım adım yönergeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253383"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak kullanıcı tarafından atanan yönetilen bir kimlik oluşturma, listele ve silme


Azure kaynakları için yönetilen kimlikler Azure hizmetlerine Azure Etkin Dizini'nde yönetilen bir kimlik sağlar. Bu kimliği, kodunuzda kimlik bilgilerine gerek kalmadan Azure AD kimlik doğrulamasını destekleyen hizmetlere kimlik doğrulamak için kullanabilirsiniz. 

Bu makalede, Azure Kaynak Yöneticisi kullanarak kullanıcı tarafından atanmış yönetilen bir kimlik oluşturursunuz.

Azure Kaynak Yöneticisi şablonu kullanarak kullanıcı tarafından atanan yönetilen bir kimliği listelemek ve silmek mümkün değildir.  Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak ve listelemek için aşağıdaki makalelere bakın:

- [Kullanıcı tarafından atanan yönetilen kimliği listele](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Kullanıcı tarafından atanan yönetilen kimliği silme](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
  ## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenleme

Azure portalı ve komut dosyası oluşturmada olduğu gibi, Azure Kaynak Yöneticisi şablonları da bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım için hem yerel hem de portal tabanlı çeşitli seçenekler mevcuttur:

- Sıfırdan bir şablon oluşturmanıza veya varolan bir ortak veya [QuickStart şablonuna](https://azure.microsoft.com/documentation/templates/)dayandırmanızı sağlayan [Azure Marketi'nden özel](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)bir şablon kullanarak.
- Özgün [dağıtımdan](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates)veya [dağıtımın geçerli durumundan](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates)bir şablon dışa aktararak varolan bir kaynak grubundan türeyen.
- Yerel bir [JSON düzenleyicisi (VS Code gibi)](../../azure-resource-manager/resource-manager-create-first-template.md)kullanarak ve ardından PowerShell veya CLI kullanarak yükleme ve dağıtma.
- Bir şablon oluşturmak ve dağıtmak için Visual Studio [Azure Kaynak Grubu projesini](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanma. 

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için, hesabınızın [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol atamasına ihtiyacı vardır.

Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için aşağıdaki şablonu kullanın. Değeri `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle değiştirin:

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

Azure Kaynak Yöneticisi şablonu kullanarak kullanıcı tarafından atanan yönetilen bir kimliği niçin Azure Kaynak Yöneticisi şablonuna atayacağımhakkında bilgi için, [bir Azure VM'de bir Azure Kaynakları için yönetilen kimlikleri şablonkullanarak yapılandırın.](qs-configure-template-windows-vm.md)


 
