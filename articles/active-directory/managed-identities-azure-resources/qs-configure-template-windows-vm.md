---
title: Şablonu kullanarak Azure VM'de yönetilen kimlikleri yapılandırma - Azure AD
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure kaynakları için yönetilen kimlikleri bir Azure VM'de yapılandırmak için adım adım yönergeler.
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
ms.date: 09/26/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5540697e8e64586d73e34d253fb95e549fc0301
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972142"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-a-templates"></a>Azure VM'deki Azure kaynaklarıiçin yönetilen kimlikleri şablonlar kullanarak yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz.

Bu makalede, Azure Kaynak Yöneticisi dağıtım şablonu kullanılarak, Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure VM'de nasıl gerçekleştireceğimiz öğrenilir:

## <a name="prerequisites"></a>Ön koşullar

- Azure Kaynak Yöneticisi dağıtım şablonunu kullanmayı bilmiyorsanız, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure portalı ve komut dosyası oluşturmada olduğu gibi, [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md) şablonları da bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım için hem yerel hem de portal tabanlı çeşitli seçenekler mevcuttur:

   - Sıfırdan bir şablon oluşturmanıza veya varolan bir ortak veya [hızlı başlangıç şablonuna](https://azure.microsoft.com/documentation/templates/)dayandırmanızı sağlayan Azure [Marketi'nden özel](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)bir şablon kullanarak.
   - Özgün [dağıtımdan](../../azure-resource-manager/templates/export-template-portal.md)veya [dağıtımın geçerli durumundan](../../azure-resource-manager/templates/export-template-portal.md)bir şablon dışa aktararak varolan bir kaynak grubundan türeyen.
   - Yerel bir [JSON düzenleyicisi (VS Code gibi)](../../azure-resource-manager/resource-manager-create-first-template.md)kullanarak ve ardından PowerShell veya CLI kullanarak yükleme ve dağıtma.
   - Bir şablon oluşturmak ve dağıtmak için Visual Studio [Azure Kaynak Grubu projesini](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanma.  

Seçtiğiniz seçenek ne olursa olsun, şablon sözdizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya varolan bir VM'de bir sistemi veya kullanıcı tarafından atanan yönetilen kimliği etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Kaynak Yöneticisi dağıtımlar için [artımlı](../../azure-resource-manager/templates/deployment-modes.md) bir güncelleştirme yapar.

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi şablonu kullanarak sistem tarafından atanmış yönetilen bir kimliği etkinleştirecek ve devre dışı kacaksınız.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Azure VM'si oluşturma sırasında veya varolan bir VM'de sistem tarafından atanmış yönetilen kimliği etkinleştirme

VM'de sistem tarafından atanan yönetilen kimliği etkinleştirmek için hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. İster yerel olarak ister Azure portalı üzerinden Azure'da oturum açın, VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Sistem tarafından atanan yönetilen kimliği etkinleştirmek için şablonu `Microsoft.Compute/virtualMachines` bir düzenleyiciye `resources` yükleyin, `"identity"` bölüm içindeki ilgi `"type": "Microsoft.Compute/virtualMachines"` kaynağını bulun ve özelliği özellik ile aynı düzeyde ekleyin. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": {
       "type": "SystemAssigned"
   },
   ```



3. İşinizi bitirdiğinizde, şablonunuzun bölümüne `resource` aşağıdaki bölümler eklenmelidir:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },

            //The following appears only if you provisioned the optional VM extension (to be deprecated)
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-managed-identity"></a>VM'nin sistem le atanmış yönetilen kimliğini bir rol atama

VM'nizde sistem le atanmış yönetilen kimliği etkinleştirdikten sonra, oluşturulduğu kaynak grubuna **Okuyucu** erişimi gibi bir rol vermek isteyebilirsiniz.

VM'nizin sistemle atanmış kimliğine bir rol atamak için hesabınızın [Kullanıcı Erişim Yöneticisi](/azure/role-based-access-control/built-in-roles#user-access-administrator) rol atamasına ihtiyacı vardır.

1. İster yerel olarak ister Azure portalı üzerinden Azure'da oturum açın, VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve VM **Reader'ınıza** oluşturulduğu kaynak grubuna erişim sağlamak için aşağıdaki bilgileri ekleyin.  Şablon yapınız düzenleyiciye ve seçtiğiniz dağıtım modeline bağlı olarak değişebilir.

   `parameters` Bölümün altında aşağıdakileri ekleyin:

    ```JSON
    "builtInRoleType": {
        "type": "string",
        "defaultValue": "Reader"
    },
    "rbacGuid": {
        "type": "string"
    }
    ```

    `variables` Bölümün altında aşağıdakileri ekleyin:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    `resources` Bölümün altında aşağıdakileri ekleyin:

    ```JSON
    {
        "apiVersion": "2017-09-01",
        "type": "Microsoft.Authorization/roleAssignments",
        "name": "[parameters('rbacGuid')]",
        "properties": {
            "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
            "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
            "scope": "[resourceGroup().id]"
        },
         "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
        ]
    }
    ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-vm"></a>Sistem tarafından atanan yönetilen kimliği Azure VM'den devre dışı

Sistem tarafından atanan yönetilen kimliği Bir VM'den kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır.  Ek Azure AD dizin idemi rol atamaları gerekmez.

1. İster yerel olarak ister Azure portalı üzerinden Azure'da oturum açın, VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `Microsoft.Compute/virtualMachines` `resources` bölümdeki ilgi kaynağını bulun. Yalnızca sistemle atanmış yönetilen kimliğe sahip bir VM'iniz varsa, kimlik `None`türünü 'ye değiştirerek devre dışı kullanabilirsiniz  

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   VM'nizde hem sistem hem de kullanıcı tarafından `SystemAssigned` atanmış yönetilen kimlikler varsa, kimlik türünden kaldırın ve `UserAssigned` `userAssignedIdentities` sözlük değerlerini koruyun.

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   Sizin `apiVersion` `2017-12-01` ve VM'nizde hem sistem hem de kullanıcı `SystemAssigned` tarafından atanmış yönetilen `UserAssigned` kimlikler `identityIds` varsa, kimlik türünden kaldırın ve kullanıcı tarafından atanan yönetilen kimliklerin dizisini devam edin.  

Aşağıdaki örnek, kullanıcı tarafından atanan yönetilen kimlikler olmadan bir VM'den sistem tarafından atanmış yönetilen bir kimliğin nasıl kaldırını gösterir:

 ```JSON
 {
     "apiVersion": "2018-06-01",
     "type": "Microsoft.Compute/virtualMachines",
     "name": "[parameters('vmName')]",
     "location": "[resourceGroup().location]",
     "identity": {
         "type": "None"
     }
 }
 ```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi şablonu kullanarak bir Azure VM'ye kullanıcı tarafından atanmış yönetilen bir kimlik atamış olursunuz.

> [!Note]
> Azure Kaynak Yöneticisi Şablonu kullanarak kullanıcı tarafından atanmış yönetilen [bir](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity)kimlik oluşturmak için bkz.

### <a name="assign-a-user-assigned-managed-identity-to-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen bir kimliği Azure VM'ye atama

Bir VM'ye kullanıcı tarafından atanan bir kimlik atamak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [Yönetilen Kimlik Operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. Öğenin `resources` altında, VM'nize kullanıcı tarafından atanan yönetilen bir kimlik atamak için aşağıdaki girişi ekleyin.  Oluşturduğunuz kullanıcı `<USERASSIGNEDIDENTITY>` tarafından atanan yönetilen kimliğin adıyla değiştirdiğinden emin olun.

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   Kullanıcı `apiVersion` tarafından `2018-06-01`atanan yönetilen kimlikleriniz `userAssignedIdentities` sözlük biçiminde depolanıyorsa, `<USERASSIGNEDIDENTITYNAME>` değer şablonunuzun `variables` bölümünde tanımlanan bir değişkende depolanmalıdır.

   ```JSON
    {
        "apiVersion": "2018-06-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
            }
        }
    }
   ```

   **Microsoft.Compute/virtualMachines API sürümü 2017-12-01**

   Kullanıcı `apiVersion` tarafından `2017-12-01`atanan yönetilen kimlikleriniz `identityIds` dizide depolanıyorsa `<USERASSIGNEDIDENTITYNAME>` ve değer şablonunuzun `variables` bölümünde tanımlanan bir değişkende depolanmalıdır.

   ```JSON
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```

3. İşinizi bitirdiğinizde, şablonunuzun bölümüne `resource` aşağıdaki bölümler eklenmelidir:

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        //The following appears only if you provisioned the optional VM extension (to be deprecated)                  
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]   
   ```
   **Microsoft.Compute/virtualMachines API sürümü 2017-12-01**

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },

        //The following appears only if you provisioned the optional VM extension (to be deprecated)                   
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
       }
    ]
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure VM'den kaldırma

Kullanıcı tarafından atanan bir kimliği VM'den kaldırmak için, hesabınızın [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol atamasına ihtiyacı vardır. Ek Azure AD dizin idemi rol atamaları gerekmez.

1. İster yerel olarak ister Azure portalı üzerinden Azure'da oturum açın, VM içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `Microsoft.Compute/virtualMachines` `resources` bölümdeki ilgi kaynağını bulun. Yalnızca kullanıcı tarafından atanan yönetilen kimliğe sahip bir VM'iniz varsa, kimlik `None`türünü 'de değiştirerek devre dışı kalabilirsiniz.

   Aşağıdaki örnek, sistem tarafından atanmış yönetilen kimlikler olmadan, kullanıcı tarafından atanan tüm yönetilen kimlikleri vm'den nasıl kaldırdığınızı gösterir:

   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": {
          "type": "None"
          },
    }
   ```

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   Kullanıcı tarafından atanan tek bir yönetilen kimliği VM'den `useraAssignedIdentities` kaldırmak için, bu kimliği sözlükten kaldırın.

   Sistem tarafından atanmış yönetilen bir kimliğiniz varsa, `type` bu kimliği `identity` değerin altındaki değerde saklayın.

   **Microsoft.Compute/virtualMachines API sürümü 2017-12-01**

   Kullanıcı tarafından atanan tek bir yönetilen kimliği VM'den `identityIds` kaldırmak için, onu diziden kaldırın.

   Sistem tarafından atanmış yönetilen bir kimliğiniz varsa, `type` bu kimliği `identity` değerin altındaki değerde saklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler.](overview.md)
