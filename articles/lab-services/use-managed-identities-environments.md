---
title: DevTest Labs'da ortam oluşturmak için Azure yönetilen kimlikleri kullanma | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvarda ortamdağıtmak için Azure'da yönetilen kimlikleri nasıl kullanacağınızı öğrenin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2019
ms.author: spelluru
ms.openlocfilehash: a4ba4206c01e492f2ae980c5806de1e72c7051c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73931149"
---
# <a name="use-azure-managed-identities-to-deploy-environments-in-a-lab"></a>Ortamları laboratuvarda dağıtmak için Azure yönetilen kimlikleri kullanma 
Laboratuvar sahibi olarak, ortamları bir laboratuarda dağıtmak için yönetilen bir kimlik kullanabilirsiniz. Bu özellik, ortamın önemli kasalar, paylaşılan görüntü galerileri ve ortamın kaynak grubunun dışındaki ağları gibi Azure kaynaklarına atıfta bulunduğu veya bunlara atıfta bulunduğu senaryolarda yararlıdır. Bu, o ortamın kaynak grubuyla sınırlı olmayan kum havuzu ortamlarının oluşturulmasını sağlar.

> [!NOTE]
> Şu anda, laboratuvar başına tek bir kullanıcı tarafından atanan kimlik desteklenir. 

## <a name="prerequisites"></a>Ön koşullar
- [Azure portalını kullanarak kullanıcı tarafından atanan yönetilen bir kimliğe rol oluşturun, listelayın, silinveya atayın.](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) 

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir laboratuvar sahibi olarak, laboratuvara kullanıcı tarafından yönetilen bir kimlik eklemek için Azure portalını kullanırsınız. 

1. Laboratuvar sayfasında Yapılandırma **ve ilkeler'i**seçin. 
1. **Ayarlar** bölümünde **Kimlik'i** seçin.
1. Kullanıcı tarafından atanan bir kimlik eklemek için araç çubuğuna **Ekle'yi** seçin. 
1. Önceden doldurulmuş açılır listeden bir **kimlik** seçin.
1. **Tamam'ı**seçin.

    ![Kullanıcı tarafından yönetilen kimlik ekleme](./media/use-managed-identities-environments/add-user-managed-identity.png)
2. Listede eklenen kullanıcı tarafından yönetilen kimliği görürsünüz. 

    ![Listede kullanıcı tarafından yönetilen kimlik](./media/use-managed-identities-environments/identity-in-list.png)

Kaydedildikten sonra, laboratuvar tüm laboratuvar ortamlarını dağıtırken bu kimliği kullanır. Ayrıca, listeden kimliği seçerek Azure'daki kimlik kaynağına da erişebilirsiniz. 

Laboratuvar sahibi, laboratuvara eklenen kimlik ortamın erişmesi gereken dış kaynaklara izinleri olduğu sürece, ortamı dağıtırken özel bir şey yapmak zorunda değildir. 

Laboratuvara atanan kullanıcı tarafından yönetilen kimliği değiştirmek için önce laboratuvara iliştirilen kimliği kaldırın ve ardından laboratuvara bir kimlik daha ekleyin. Laboratuvara bağlı bir kimliği kaldırmak için, **... (elips)** ve **Kaldır'ı**tıklatın. 

![Listede kullanıcı tarafından yönetilen kimlik](./media/use-managed-identities-environments/replace-identity.png)  

## <a name="use-api"></a>API'yi kullanma

1. Bir kimlik oluşturduktan sonra, bu kimliğin kaynak kimliğine dikkat edin. Aşağıdaki örnek gibi görünmelidir: 

    `/subscriptions/0000000000-0000-0000-0000-00000000000000/resourceGroups/<RESOURCE GROUP NAME> /providers/Microsoft.ManagedIdentity/userAssignedIdentities/<NAME of USER IDENTITY>`.
1. Laboratuvara aşağıdaki örneğe benzer `ServiceRunner` yeni bir kaynak eklemek için PUT Https yöntemini gerçekleştirin. Servis koşucusu kaynağı, DevTest Labs'da yönetilen kimlikleri yönetmek ve denetlemek için bir proxy kaynağıdır. Servis koşucusu adı geçerli bir ad olabilir, ancak yönetilen kimlik kaynağının adını kullanmanızı öneririz. 
 
    ```json
    PUT https://management.azure.com/subscriptions/{subId}/resourceGroups/{rg}/providers/Microsoft.Devtestlab/labs/{yourlabname}/serviceRunners/{serviceRunnerName}

    {
        "location": "{location}",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "[userAssignedIdentityResourceId]":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
          
    }
    ```
 
    Bir örneği aşağıda verilmiştir: 

    ```json
    PUT https://management.azure.com/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.Devtestlab/labs/mylab/serviceRunners/sampleuseridentity

    {
        "location": "eastus",
        "identity":{
            "type": "userAssigned",
            "userAssignedIdentities":{
                "/subscriptions/0000000000-0000-0000-0000-000000000000000/resourceGroups/exampleRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/sampleuseridentity":{}
            }
        }
        "properties":{
            "identityUsageType":"Environment"
                     }
    }
    ```
 
Kullanıcı tarafından atanan kimlik laboratuvara eklendikten sonra, Azure DevTest Labs hizmeti Azure Kaynak Yöneticisi ortamlarını dağıtırken bu kimliği kullanır. Örneğin, harici paylaşılan resim galerisi görüntüsüne erişmek için Kaynak Yöneticisi şablonunuza ihtiyacınız varsa, laboratuvara eklediğiniz kimliğin paylaşılan resim galerisi kaynağı için en az gerekli izinlere sahip olduğundan emin olun. 
