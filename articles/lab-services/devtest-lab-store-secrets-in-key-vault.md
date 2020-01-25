---
title: Gizli dizileri Azure DevTest Labs bir anahtar kasasında depolayın | Microsoft Docs
description: Gizli dizileri bir Azure Key Vault nasıl depolayacağınızı ve VM, formül veya ortam oluştururken bunları nasıl kullanacağınızı öğrenin.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: 29a4d20390575778ccdecde390c257ccf6a48eb1
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720937"
---
# <a name="store-secrets-in-a-key-vault-in-azure-devtest-labs"></a>Gizli dizileri Azure DevTest Labs bir anahtar kasasında depolayın
Windows VM 'niz için parola, Linux VM 'niz için ortak SSH anahtarı veya bir yapıt aracılığıyla Git deponuzu kopyalamak için kişisel erişim belirteci kullanırken Azure DevTest Labs bir karmaşık parola girmeniz gerekebilir. Gizli dizileri genellikle uzunluğundadır ve rastgele karakterlere sahiptir. Bu nedenle, özellikle de aynı parolayı birden çok kez kullanıyorsanız, bunları girmeye benzer ve güvenilir bir şekilde girebilirsiniz.

Bu sorunu çözmek ve ayrıca gizli dizileri güvenli bir yerde tutmak için DevTest Labs bir [Azure anahtar kasasında](../key-vault/key-vault-overview.md)gizli dizileri depolamayı destekler. Bir Kullanıcı ilk kez gizli dizi kaydettiğinde, DevTest Labs hizmeti Laboratuvarı içeren aynı kaynak grubunda otomatik olarak bir Anahtar Kasası oluşturur ve gizli anahtarı anahtar kasasında depolar. DevTest Labs her bir kullanıcı için ayrı bir Anahtar Kasası oluşturur. 

Laboratuvar kullanıcısının, anahtar kasasında bir gizli dizi oluşturabilmeniz için önce bir laboratuar sanal makinesi oluşturması gerektiğini lütfen unutmayın. Bunun nedeni, DevTest Laboratuvarı hizmeti 'nin, kendi anahtar kasasında gizli dizileri oluşturmalarına ve depolamasına izin vermeden önce geçerli bir Kullanıcı belgesiyle ilişkilendirilmesi gerekir. 


## <a name="save-a-secret-in-azure-key-vault"></a>Gizli dizi Azure Key Vault kaydetme
Gizli anahtarı Azure Key Vault kaydetmek için aşağıdaki adımları uygulayın:

1. Sol menüdeki **gizli dizilerimi** seçin.
2. Gizli dizi için bir **ad** girin. Bu adı, sanal makine, formül veya ortam oluştururken açılan listede görürsünüz. 
3. Parolayı **değer**olarak girin.

    ![Saklama parolası](media/devtest-lab-store-secrets-in-key-vault/store-secret.png)

## <a name="use-a-secret-from-azure-key-vault"></a>Azure Key Vault bir gizli dizi kullanın
Bir VM, formül veya ortam oluşturmak için bir parola girmeniz gerektiğinde, el ile gizli anahtar girebilir ya da anahtar kasasından kaydedilmiş bir gizli dizi seçebilirsiniz. Anahtar Kasanızda depolanan bir gizli dizi kullanmak için aşağıdaki işlemleri yapın:

1. **Kaydedilmiş gizli dizi kullan**' ı seçin. 
2. **Gizli dizi Seç**listesinden gizli anahtarı seçin. 

    ![VM 'de gizli dizi kullanma](media/devtest-lab-store-secrets-in-key-vault/secret-store-pick-a-secret.png)

## <a name="use-a-secret-in-an-azure-resource-manager-template"></a>Azure Resource Manager şablonunda gizli dizi kullanma
Aşağıdaki örnekte gösterildiği gibi, bir VM oluşturmak için kullanılan bir Azure Resource Manager şablonunda gizli adınızı belirtebilirsiniz:

![Formülde veya ortamda gizli dizi kullanın](media/devtest-lab-store-secrets-in-key-vault/secret-store-arm-template.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Gizli dizi kullanarak VM oluşturma](devtest-lab-add-vm.md) 
- [Gizli dizi kullanarak bir formül oluşturma](devtest-lab-manage-formulas.md)
- [Gizli dizi kullanarak ortam oluşturma](devtest-lab-create-environment-from-arm.md)
