---
title: Azure DevTest Labs içindeki laboratuvar sanal makinelerinizdeki yönetilen kimlikleri etkinleştirin
description: Bu makalede, laboratuvar sahibinin laboratuvar sanal makinelerinizde Kullanıcı tarafından atanan yönetilen kimlikleri nasıl etkinleştirebilirim gösterilmektedir.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2020
ms.author: spelluru
ms.openlocfilehash: 5d70f83babcf53249f581230e72326d99a0533d6
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84898440"
---
# <a name="enable-user-assigned-managed-identities-on-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs 'da laboratuvar sanal makinelerinde Kullanıcı tarafından atanan yönetilen kimlikleri etkinleştirin
Laboratuvar sahibi olarak, Azure DevTest Labs ' de laboratuvar sanal makinelerinizde (VM) Kullanıcı tarafından atanan Yönetilen kimlikler sağlayabilirsiniz.

Yönetilen bir kimlik, kodda herhangi bir kimlik bilgisi geçirmeden Key Vault dahil Azure Active Directory (AD) kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanılabilir. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md).

Laboratuvar kullanıcıları, bu özellik ile Azure SQL veritabanı gibi Azure kaynaklarını laboratuvar bağlamında paylaşabilir. Kaynağın kimlik doğrulaması, kimliğin kendisi tarafından gerçekleştirilir. Yapılandırıldıktan sonra, tüm mevcut/yeni oluşturulan laboratuvar VM 'leri bu kimlikle etkinleştirilir. Laboratuvar kullanıcıları, makinelerinde oturum açtıktan sonra kaynaklara erişebilir.

> [!NOTE]
> Laboratuvar sanal makinelerinize etkinleştirilecek birden çok kullanıcı tarafından atanmış yönetilen kimlik ekleyebilirsiniz.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Laboratuvar VM 'Leri için Kullanıcı tarafından atanmış bir yönetilen kimlik eklemek için aşağıdaki adımları izleyin:

1. [Aboneliğinizde Kullanıcı tarafından atanan yönetilen kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)
1. Laboratuvarınızın **yapılandırma ve ilkeler** sayfasına gidin.
2. Sol menüdeki **kimlik (Önizleme)** öğesini seçin.
3. **Sanal makine** sekmesini seçin.
4. Önceden doldurulmuş bir açılan listeden var olan bir kimliği seçmek için **Ekle** ' yi seçin. 

    ![Kimlik Ekle düğmesi](./media/enable-managed-identities-lab-vms/add-identity-button.png)
5. Açılan listeden var olan bir **Kullanıcı tarafından yönetilen kimliği** seçin ve **Tamam**' ı seçin. 

    ![Kimlik Ekle](./media/enable-managed-identities-lab-vms/add-identity.png)

## <a name="use-api"></a>API kullanma

1.  Kimlik oluşturduktan sonra kimliğin kaynak KIMLIĞI ' ni aklınızda edin. Aşağıdaki örnekteki gibi görünmelidir: 

    `/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`.
2. Aşağıdaki örnekte gösterildiği gibi laboratuvara yeni bir **Servicerunner** kaynağı eklemek IÇIN BIR put https yöntemi çalıştırın. 

    Hizmet Çalıştırıcısı kaynağı, DevTest Labs 'de yönetilen kimlikleri yönetmek ve denetlemek için bir ara sunucu kaynağıdır. Hizmet Çalıştırıcısı adı geçerli bir ad olabilir, ancak yönetilen kimlik kaynağının adını kullanmanızı öneririz.

    ```json
    {
        "identity": {
            "type": "userAssigned",
            "userAssignedIdentities": { 
                "[userAssignedIdentityResourceId]": {}
            }
            },
        "location": "southeastasia",
        "properties": {
            "identityUsageType": "VirtualMachine"
        }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar
Yönetilen kimlikler hakkında daha fazla bilgi edinmek için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md).







