---
title: Azure DevTest Labs içindeki laboratuvar sanal makinelerinizdeki yönetilen kimlikleri etkinleştirin
description: Bu makalede, laboratuvar sahibinin laboratuvar sanal makinelerinizde Kullanıcı tarafından atanan yönetilen kimlikleri nasıl etkinleştirebilirim gösterilmektedir.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: b4bf2900acebaeecd5cbc4cb65635aee6de87dda
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88717650"
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
1. Sol menüdeki **kimlik (Önizleme)** öğesini seçin.
1. **Sanal makine** sekmesini seçin.
1. Önceden doldurulmuş bir açılan listeden var olan bir kimliği seçmek için **Ekle** ' yi seçin. 

    > [!div class="mx-imgBorder"]
    > ![Kimlik Ekle düğmesi](./media/enable-managed-identities-lab-vms/add-identity-button.png)
1. Açılan listeden var olan bir **Kullanıcı tarafından yönetilen kimliği** seçin ve **Tamam**' ı seçin. 

    > [!div class="mx-imgBorder"]
    > ![Kimlik Ekle](./media/enable-managed-identities-lab-vms/add-identity.png)

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







