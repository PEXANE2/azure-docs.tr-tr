---
title: Azure DevTest Labs'da laboratuvar VM'lerinizde yönetilen kimlikleri etkinleştirme
description: Bu makalede, laboratuvar sahibinin laboratuvar sanal makinelerinizde kullanıcı tarafından atanan yönetilen kimlikleri nasıl etkinleştirebileceği gösterilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75692673"
---
# <a name="enable-user-assigned-managed-identities-on-lab-virtual-machines-in-azure-devtest-labs"></a>Azure DevTest Labs'daki laboratuvar sanal makinelerinde kullanıcı tarafından atanan yönetilen kimlikleri etkinleştirme
Laboratuvar sahibi olarak, Azure DevTest Laboratuvarlarında laboratuvar sanal makinelerinizde (VM) kullanıcı tarafından atanan yönetilen kimlikleri etkinleştirebilirsiniz.

Yönetilen kimlik, kodda herhangi bir kimlik belgesi girmeden Key Vault da dahil olmak üzere Azure Etkin Dizin (AD) kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanılabilir. Yönetilen kimlikler hakkında daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)

Bu özellik sayesinde, laboratuvar kullanıcıları Azure SQL Veritabanı gibi Azure kaynaklarını laboratuvar bağlamında paylaşabilir. Kaynağa kimlik doğrulama, kimliğin kendisi tarafından halledilir. Yapılandırıldıktan sonra, varolan/yeni oluşturulan her laboratuvar VM bu kimlikle etkinleştirilir. Laboratuvar kullanıcıları, makinelerine giriş yaptıktan sonra kaynaklara erişebilir.

> [!NOTE]
> Laboratuvar VM'lerinizde etkinleştirilecek birden çok kullanıcı atanmış yönetilen kimlik ekleyebilirsiniz.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Laboratuvar VM'leri için yönetilen bir kullanıcı atanmış bir kullanıcı eklemek için aşağıdaki adımları izleyin:

1. [Aboneliğinizde kullanıcı tarafından atanmış yönetilen bir kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity)
1. Laboratuvarınız için **Yapılandırma ve ilkeler** sayfasına gidin.
2. Sol menüde **Kimlik (Önizleme)** seçeneğini belirleyin.
3. Sanal **makine** sekmesini seçin.
4. Önceden doldurulmuş bir açılır listeden varolan bir kimliği seçmek için **Ekle'yi** seçin. 

    ![Kimlik ekle düğmesi](./media/enable-managed-identities-lab-vms/add-identity-button.png)
5. Açılan lar listesinden varolan **kullanıcı tarafından yönetilen** bir kimlik seçin ve **Tamam'ı**seçin. 

    ![Kimlik ekleme](./media/enable-managed-identities-lab-vms/add-identity.png)

## <a name="use-api"></a>API'yi kullanma

1.  Kimlik oluşturduktan sonra, kimliğin kaynak kimliğine dikkat edin. Aşağıdaki örnek gibi görünmelidir: 

    `/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`.
2. Aşağıdaki örnekte gösterildiği gibi laboratuvara yeni bir **ServiceRunner** kaynağı eklemek için PUT HTTPS yöntemiçalıştırın. 

    Servis koşucusu kaynağı, DevTest Labs'da yönetilen kimlikleri yönetmek ve denetlemek için bir proxy kaynağıdır. Servis koşucusu adı geçerli bir ad olabilir, ancak yönetilen kimlik kaynağının adını kullanmanızı öneririz.

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
Yönetilen kimlikler hakkında daha fazla bilgi edinmek için Azure [kaynakları için yönetilen kimlikler nedir?](../active-directory/managed-identities-azure-resources/overview.md)







