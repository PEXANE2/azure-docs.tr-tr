---
title: Şablonu sanal makine ölçeği kümelerinde yönetilen kimlikleri kullanacak şekilde yapılandırın - Azure AD
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure kaynakları için yönetilen kimlikleri sanal makine ölçeğinde yapılandırmak için adım adım yönergeler.
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
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d5e324ea20b2ea82fac5b5132893d3558bd3b41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425570"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-virtual-machine-scale-using-a-template"></a>Azure sanal makine ölçeğinde azure kaynakları için yönetilen kimlikleri şablon kullanarak yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikler, Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlik le Azure hizmetlerine olanak sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz.

Bu makalede, Azure Kaynak Yöneticisi dağıtım şablonu kullanarak Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri bir Azure sanal makine ölçeğinde nasıl gerçekleştireceklerini öğreneceksiniz:
- Azure sanal makine ölçeğinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırak
- Azure sanal makine ölçeğinde kullanıcı tarafından atanan yönetilen bir kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklere aşina değilseniz, [genel bakış bölümüne](overview.md)göz atın. ** [Sistem tarafından atanan ve kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirin.**
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için, hesabınızda aşağıdaki Azure rol tabanlı erişim denetimi atamaları gerekir:

    > [!NOTE]
    > Ek Azure AD dizini rol atamaları gerekmez.

    - [Sanal Makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) sanal makine ölçeği kümesi oluşturmak ve sistemi ve/veya kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesinden etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen bir kimlik oluşturmak için [Yönetilen Kimlik Oluşturici](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan yönetilen bir kimliği sanal makine ölçeği kümesinden ve sanal makine ölçeğikümesine atamak ve kaldırmak için [Yönetilen Kimlik İşlemi](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure portalı ve komut dosyası oluşturmada olduğu gibi, [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md) şablonları da bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım için hem yerel hem de portal tabanlı çeşitli seçenekler mevcuttur:

   - Sıfırdan bir şablon oluşturmanıza veya varolan bir ortak veya [hızlı başlangıç şablonuna](https://azure.microsoft.com/documentation/templates/)dayandırmanızı sağlayan Azure [Marketi'nden özel](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)bir şablon kullanarak.
   - Özgün [dağıtımdan](../../azure-resource-manager/templates/export-template-portal.md)veya [dağıtımın geçerli durumundan](../../azure-resource-manager/templates/export-template-portal.md)bir şablon dışa aktararak varolan bir kaynak grubundan türeyen.
   - Yerel bir [JSON düzenleyicisi (VS Code gibi)](../../azure-resource-manager/resource-manager-create-first-template.md)kullanarak ve ardından PowerShell veya CLI kullanarak yükleme ve dağıtma.
   - Bir şablon oluşturmak ve dağıtmak için Visual Studio [Azure Kaynak Grubu projesini](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) kullanma.  

Seçtiğiniz seçenek ne olursa olsun, şablon sözdizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya varolan bir VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Kaynak Yöneticisi dağıtımlar için [artımlı](../../azure-resource-manager/templates/deployment-modes.md) bir güncelleştirme yapar.

## <a name="system-assigned-managed-identity"></a>Sistem le atanmış yönetilen kimlik

Bu bölümde, bir Azure Kaynak Yöneticisi şablonu kullanarak sistem tarafından atanan yönetilen kimliği etkinleştirecek ve devre dışı kacaksınız.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-an-existing-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesi veya varolan sanal makine ölçeği kümesi nin oluşturulması sırasında sistem tarafından atanan yönetilen kimliği etkinleştirme

1. İster yerel olarak Azure'da oturum açın ister Azure portalı üzerinden, sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın.
2. Sistem tarafından atanan yönetilen kimliği etkinleştirmek için şablonu `Microsoft.Compute/virtualMachinesScaleSets` bir düzenleyiciye yükleyin, kaynaklar `identity` bölümündeki ilgi kaynağını `"type": "Microsoft.Compute/virtualMachinesScaleSets"` bulun ve özelliği özellik ile aynı düzeyde ekleyin. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": {
       "type": "SystemAssigned"
   }
   ```

> [!NOTE]
> İsteğe bağlı olarak, şablonunsurunda `extensionProfile` belirterek Azure kaynakları sanal makine kümesi uzantısı için yönetilen kimlikleri sağlayabilirsiniz. Bu adım, belirteçleri almak için Azure Örneği Meta veri hizmeti (IMDS) kimlik bitiş noktasını kullanabileceğiniz için isteğe bağlıdır.  Daha fazla bilgi için, [kimlik doğrulaması için VM uzantısından Azure IMDS'ye geçir'e](howto-migrate-vm-extension.md)bakın.


4. İşinizi bitirdiğinizde, şablonunuzun kaynak bölümüne aşağıdaki bölümler eklenmelidir:

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan yönetilen kimliği Azure sanal makine ölçeği kümesinden devre dışı bırak

Artık sistem tarafından atanmış yönetilen bir kimliğe ihtiyaç dolmayan sanal bir makine ölçeği kümeniz varsa:

1. İster yerel olarak Azure'da oturum açın ister Azure portalı üzerinden, sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `Microsoft.Compute/virtualMachineScaleSets` `resources` bölümdeki ilgi kaynağını bulun. Yalnızca sistemle atanmış yönetilen kimliğe sahip bir VM'iniz varsa, kimlik `None`türünü 'ye değiştirerek devre dışı kullanabilirsiniz

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   ApiVersion'unuz `2018-06-01` ysa ve VM'nizde hem sistem hem `SystemAssigned` de kullanıcı tarafından `UserAssigned` atanmış yönetilen kimlikler varsa, kimlik türünden kaldırın ve userAssignedIdentities sözlük değerleriyle birlikte tutun.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   ApiVersion `2017-12-01` ve sanal makine ölçek küme hem sistem ve kullanıcı tarafından `SystemAssigned` atanan yönetilen kimlikleri `UserAssigned` varsa, `identityIds` kimlik türünden kaldırın ve kullanıcı tarafından atanan yönetilen kimliklerin dizi ile birlikte tutun.



   Aşağıdaki örnek, kullanıcı tarafından atanan yönetilen kimlikler olmadan sistem tarafından atanan yönetilen bir kimliği sanal makine ölçeğinden nasıl kaldırdığınızı gösterir:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Kaynak Yöneticisi şablonu kullanarak sanal makine ölçeğine kullanıcı tarafından atanmış yönetilen bir kimlik atamış olursunuz.

> [!Note]
> Azure Kaynak Yöneticisi Şablonu kullanarak kullanıcı tarafından atanmış yönetilen [bir](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity)kimlik oluşturmak için bkz.

### <a name="assign-a-user-assigned-managed-identity-to-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği sanal makine ölçeği kümesine atama

1. Öğenin `resources` altında, sanal makine ölçeği kümenize kullanıcı tarafından atanan yönetilen bir kimlik atamak için aşağıdaki girişi ekleyin.  Oluşturduğunuz kullanıcı `<USERASSIGNEDIDENTITY>` tarafından atanan yönetilen kimliğin adıyla değiştirdiğinden emin olun.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   ApiVersion ise, `2018-06-01`kullanıcı tarafından atanan yönetilen kimlikleri sözlük `userAssignedIdentities` biçiminde depolanır `<USERASSIGNEDIDENTITYNAME>` ve değer şablonunuzun `variables` bölümünde tanımlanan bir değişkende depolanmalıdır.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }

   }
   ```   

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2017-12-01**

   Daha `apiVersion` önce `2017-12-01` veya daha önceyseniz, kullanıcı tarafından atanan `identityIds` yönetilen kimlikleriniz dizide depolanır ve `<USERASSIGNEDIDENTITYNAME>` değer şablonunuzun değişkenler bölümünde tanımlanan bir değişkende depolanmalıdır.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ```
> [!NOTE]
> İsteğe bağlı olarak, şablonunsurunda `extensionProfile` belirterek Azure kaynakları sanal makine kümesi uzantısı için yönetilen kimlikleri sağlayabilirsiniz. Bu adım, belirteçleri almak için Azure Örneği Meta veri hizmeti (IMDS) kimlik bitiş noktasını kullanabileceğiniz için isteğe bağlıdır.  Daha fazla bilgi için, [kimlik doğrulaması için VM uzantısından Azure IMDS'ye geçir'e](howto-migrate-vm-extension.md)bakın.

3. Işiniz bittiğinde, şablonunuz aşağıdakilere benzer olmalıdır:

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```

   **Microsoft.Compute/virtualMachines API sürümü 2017-12-01**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)    
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```
   ### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği Azure sanal makine ölçeği kümesinden kaldırma

Artık kullanıcı tarafından atanmış yönetilen bir kimliğe ihtiyaç dolmayan sanal bir makine ölçeği kümeniz varsa:

1. İster yerel olarak Azure'da oturum açın ister Azure portalı üzerinden, sanal makine ölçeği kümesini içeren Azure aboneliğiyle ilişkili bir hesap kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `Microsoft.Compute/virtualMachineScaleSets` `resources` bölümdeki ilgi kaynağını bulun. Yalnızca kullanıcı tarafından atanan yönetilen kimliğe sahip sanal bir makine ölçeği kümeniz varsa, `None`kimlik türünü '' ye değiştirerek devre dışı bırakabilirsiniz.

   Aşağıdaki örnek, sistem tarafından atanmış yönetilen kimlikler olmadan, kullanıcı tarafından atanan tüm yönetilen kimlikleri vm'den nasıl kaldırdığınızı gösterir:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   Sanal makine ölçeği kümesinden kullanıcı tarafından atanan tek bir yönetilen `userAssignedIdentities` kimliği kaldırmak için, bu kimliği sözlükten kaldırın.

   Sistemle atanmış bir kimliğiniz varsa, onu `type` `identity` değerin altındaki değerde saklayın.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2017-12-01**

   Sanal makine ölçeği kümesinden kullanıcı tarafından atanan tek bir yönetilen `identityIds` kimliği kaldırmak için, bu kimliği diziden kaldırın.

   Sistem tarafından atanmış yönetilen bir kimliğiniz varsa, `type` bu kimliği `identity` değerin altındaki değerde saklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlikler.](overview.md)
