---
title: Şablonu, sanal makine ölçek kümelerinde yönetilen kimlikleri kullanacak şekilde Yapılandırma-Azure AD
description: Bir Azure Resource Manager şablonu kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için Yönetilen kimlikler yapılandırmaya yönelik adım adım yönergeler.
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
ms.openlocfilehash: 09c1e31664b94dd814b33b630dfa4f8e24d4600f
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547183"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-virtual-machine-scale-using-a-template"></a>Bir şablon kullanarak bir Azure sanal makine ölçeğinde Azure kaynakları için Yönetilen kimlikler yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak Azure sanal makine ölçek kümesindeki Azure kaynakları işlemleri için aşağıdaki yönetilen kimliklerin nasıl gerçekleştirileceğini öğreneceksiniz:
- Azure sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırakma
- Azure sanal makine ölçek kümesine Kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makaledeki yönetim işlemlerini gerçekleştirmek için, hesabınız aşağıdaki Azure rol tabanlı erişim denetimi atamalarına ihtiyaç duyuyor:

    > [!NOTE]
    > Ek Azure AD dizin rolü ataması gerekli değildir.

    - Sanal makine [katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturmak ve sistem ve/veya Kullanıcı tarafından atanan yönetilen kimliği bir sanal makine ölçek kümesinden etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan yönetilen kimliği ve sanal makine ölçek kümesine atamak ve kaldırmak için [yönetilen kimlik operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure portal ve betikte olduğu gibi [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonlar, bir Azure Kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Aşağıdakiler dahil olmak üzere hem yerel hem de portal tabanlı şablon düzenlemesi ve dağıtımı için çeşitli seçenekler mevcuttur:

   - Sıfırdan bir şablon oluşturmanıza veya mevcut bir ortak ya da [hızlı başlangıç şablonuna](https://azure.microsoft.com/documentation/templates/)temeletmenize olanak tanıyan [Azure Marketi 'nden özel bir şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template)kullanma.
   - Varolan bir kaynak grubundan türeterek, [orijinal dağıtımdan](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)veya [dağıtımın geçerli durumundan](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates)bir şablonu dışarı aktararak.
   - Yerel bir [JSON Düzenleyicisi (vs Code gibi)](../../azure-resource-manager/resource-manager-create-first-template.md)kullanarak ve POWERSHELL veya CLI kullanarak karşıya yükleme ve dağıtma.
   - Bir şablon oluşturmak ve dağıtmak için Visual Studio [Azure Kaynak grubu projesini](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) kullanma.  

Seçtiğiniz seçenekten bağımsız olarak, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya mevcut bir VM 'de Azure kaynakları için yönetilen kimliklerin etkinleştirilmesi aynı şekilde yapılır. Ayrıca, varsayılan olarak Azure Resource Manager dağıtımlar için [artımlı bir güncelleştirme](../../azure-resource-manager/deployment-modes.md) yapar.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, sistem tarafından atanan yönetilen kimliği bir Azure Resource Manager şablonu kullanarak etkinleştirip devre dışı bırakacaksınız.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-an-existing-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi veya var olan bir sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimliği etkinleştirin

1. Azure 'da yerel olarak veya Azure portal aracılığıyla oturum açıp etmeksizin, sanal makine ölçek kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanın.
2. Sistem tarafından atanan yönetilen kimliği etkinleştirmek için, şablonu bir düzenleyiciye yükleyin, kaynaklar bölümünde ilgilendiğiniz `Microsoft.Compute/virtualMachinesScaleSets` kaynağını bulun ve `"type": "Microsoft.Compute/virtualMachinesScaleSets"` özelliğiyle aynı düzeyde `identity` özelliğini ekleyin. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

> [!NOTE]
> İsteğe bağlı olarak, Azure kaynakları sanal makine ölçek kümesi uzantısının yönetilen kimliklerini şablonun `extensionProfile` öğesinde belirterek sağlayabilirsiniz. Bu adım, belirteçleri de almak için Azure Instance Metadata Service (IMDS) kimlik uç noktasını kullanabileceğiniz için isteğe bağlıdır.  Daha fazla bilgi için bkz. [kimlik doğrulaması IÇIN VM uzantısından Azure IMDS 'ye geçiş](howto-migrate-vm-extension.md).


4. İşiniz bittiğinde, şablonunuzun kaynak bölümüne aşağıdaki bölümler eklenmelidir ve şuna benzemelidir:

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

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Azure sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği devre dışı bırakma

Artık sistem tarafından atanan yönetilen kimliğe ihtiyacı olmayan bir sanal makine ölçek kümesine sahipseniz:

1. Azure 'da yerel olarak veya Azure portal aracılığıyla oturum açıp etmeksizin, sanal makine ölçek kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `resources` bölümü içinde ilgilendiğiniz `Microsoft.Compute/virtualMachineScaleSets` kaynağını bulun. Yalnızca sistem tarafından atanan yönetilen kimliğe sahip bir VM varsa, kimlik türünü `None`değiştirerek devre dışı bırakabilirsiniz.

   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2018-06-01**

   Apisürümünüz `2018-06-01` ve sanal makinenizde hem sistem hem de Kullanıcı tarafından atanan Yönetilen kimlikler varsa, kimlik türünden `SystemAssigned` kaldırın ve `UserAssigned`, Useratandıdentities sözlük değerleriyle birlikte tutun.

   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2018-06-01**

   Apisürümünüz `2017-12-01` ve sanal makine ölçek kümesinde hem sistem hem de Kullanıcı tarafından atanan Yönetilen kimlikler varsa, kimlik türünden `SystemAssigned` kaldırın ve Kullanıcı tarafından atanan yönetilen kimliklerin `identityIds` dizisiyle birlikte `UserAssigned` tutun. 
   
    

   Aşağıdaki örnek, Kullanıcı tarafından atanan yönetilen kimlikleri olmayan bir sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği nasıl kaldıragösterdiğini gösterir:
   
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

Bu bölümde, Azure Resource Manager şablonu kullanarak bir sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atarsınız.

> [!Note]
> Azure Resource Manager şablonu kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için, bkz. [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği bir sanal makine ölçek kümesine atama

1. `resources` öğesi altında, sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atamak için aşağıdaki girişi ekleyin.  `<USERASSIGNEDIDENTITY>`, oluşturduğunuz Kullanıcı tarafından atanan yönetilen kimliğin adıyla değiştirdiğinizden emin olun.
   
   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2018-06-01**

   ApiVersion `2018-06-01`ise, Kullanıcı tarafından atanan yönetilen kimlikleriniz `userAssignedIdentities` sözlük biçiminde depolanır ve `<USERASSIGNEDIDENTITYNAME>` değeri şablonunuzun `variables` bölümünde tanımlanan bir değişkende depolanmalıdır.

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

   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2017-12-01**
    
   `apiVersion` `2017-12-01` veya daha önceki bir sürümdeyse, Kullanıcı tarafından atanan yönetilen kimlikleriniz `identityIds` dizisinde depolanır ve `<USERASSIGNEDIDENTITYNAME>` değeri şablonunuzun değişkenler bölümünde tanımlanan bir değişkende depolanmalıdır.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2017-03-30",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
           ]
       }

   }
   ``` 
> [!NOTE]
> İsteğe bağlı olarak, Azure kaynakları sanal makine ölçek kümesi uzantısının yönetilen kimliklerini şablonun `extensionProfile` öğesinde belirterek sağlayabilirsiniz. Bu adım, belirteçleri de almak için Azure Instance Metadata Service (IMDS) kimlik uç noktasını kullanabileceğiniz için isteğe bağlıdır.  Daha fazla bilgi için bkz. [kimlik doğrulaması IÇIN VM uzantısından Azure IMDS 'ye geçiş](howto-migrate-vm-extension.md).

3. İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:
   
   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2018-06-01**   

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

   **Microsoft. COMPUTE/virtualMachines API sürümü 2017-12-01**

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
   ### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği bir Azure sanal makine ölçek kümesinden kaldır

Artık Kullanıcı tarafından atanan yönetilen kimliğe ihtiyacı olmayan bir sanal makine ölçek kümesine sahipseniz:

1. Azure 'da yerel olarak veya Azure portal aracılığıyla oturum açıp etmeksizin, sanal makine ölçek kümesini içeren Azure aboneliğiyle ilişkili bir hesabı kullanın.

2. Şablonu bir [düzenleyiciye](#azure-resource-manager-templates) yükleyin ve `resources` bölümü içinde ilgilendiğiniz `Microsoft.Compute/virtualMachineScaleSets` kaynağını bulun. Yalnızca Kullanıcı tarafından atanan yönetilen kimliğe sahip bir sanal makine ölçek kümesine sahipseniz, kimlik türünü `None`değiştirerek devre dışı bırakabilirsiniz.

   Aşağıdaki örnek, sistem tarafından atanan yönetilen kimlikleri olmayan bir VM 'den Kullanıcı tarafından atanan tüm yönetilen kimliklerin nasıl kaldırılacağını gösterir:

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
   
   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2018-06-01**
    
   Bir sanal makine ölçek kümesinden Kullanıcı tarafından atanan tek bir yönetilen kimliği kaldırmak için `userAssignedIdentities` sözlüğünden kaldırın.

   Sistem tarafından atanan bir kimliğiniz varsa, `identity` değerinin altındaki `type` değerinde saklayın.

   **Microsoft. COMPUTE/virtualMachineScaleSets API sürüm 2017-12-01**

   Bir sanal makine ölçek kümesinden Kullanıcı tarafından atanan tek bir yönetilen kimliği kaldırmak için `identityIds` dizisinden kaldırın.

   Sistem tarafından atanan bir yönetilen Kimliğiniz varsa, `identity` değeri altındaki `type` değerinde ' de saklayın.
   
## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış Için Yönetilen kimlikler](overview.md).

