---
title: Azure DevTest Labs'da sırları önemli bir kasada saklayın | Microsoft Dokümanlar
description: Bir Azure Anahtar Kasası'nda sırları nasıl depolayacağım ve VM, formül veya ortam oluştururken bunları nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 0ca36a7081aaf70ee2045ee7586184c89591df16
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461523"
---
# <a name="store-secrets-in-a-key-vault-in-azure-devtest-labs"></a>Azure DevTest Labs'da sırları önemli bir kasada saklayın
Azure DevTest Labs kullanırken karmaşık bir sır girmeniz gerekebilir: Windows VM'niz için parola, Linux VM'niz için herkese açık SSH anahtarı veya bir yapı aracılığıyla Git repo'nuzu klonlamak için kişisel erişim belirteci. Sırlar genellikle uzun ve rasgele karakterler var. Bu nedenle, bunları girerek zor ve rahatsız edici olabilir, özellikle aynı gizli birden çok kez kullanıyorsanız.

Bu sorunu çözmek ve aynı zamanda sırlarınızı güvenli bir yerde saklamak için DevTest Labs, [sırların Azure anahtar kasasında](../key-vault/general/overview.md)depolanmasını destekler. Bir kullanıcı ilk kez bir sır kaydettiğinde, DevTest Labs hizmeti otomatik olarak laboratuarı içeren aynı kaynak grubunda önemli bir kasa oluşturur ve sırrı anahtar kasasında saklar. DevTest Labs her kullanıcı için ayrı bir anahtar kasası oluşturur. 

Anahtar kasasında bir sır oluşturabilmek için önce laboratuvar kullanıcısının bir laboratuvar sanal makinesi oluşturması gerektiğini lütfen unutmayın. Bunun nedeni, DevTest Lab hizmetinin, anahtar kasalarında sır oluşturmalarına ve saklamalarına izin verilmeden önce laboratuvar kullanıcısını geçerli bir kullanıcı belgesiyle ilişkilendirmesi gerektiğidir. 


## <a name="save-a-secret-in-azure-key-vault"></a>Azure Anahtar Kasası'nda bir sırrı kaydetme
Sırrınızı Azure Anahtar Kasası'nda kaydetmek için aşağıdaki adımları yapın:

1. Sol menüden **sırlarımı** seçin.
2. Sır için bir **isim** girin. Bir VM, formül veya ortam oluştururken bu adı açılır listede görürsünüz. 
3. **Değeri**olarak gizli girin.

    ![Mağaza sırrı](media/devtest-lab-store-secrets-in-key-vault/store-secret.png)

## <a name="use-a-secret-from-azure-key-vault"></a>Azure Key Vault'tan bir sır kullanın
VM, formül veya ortam oluşturmak için bir sır girmeniz gerektiğinde, bir sırrı el ile girebilir veya anahtar kasasından kaydedilmiş bir sırrı seçebilirsiniz. Anahtar kasanızda depolanan bir sırrı kullanmak için aşağıdaki işlemleri yapın:

1. **Kaydedilmiş bir sırrı kullan'ı**seçin. 
2. **Bir sır seç**için açılan listeden sırrınızı seçin. 

    ![VM'de gizli kullanın](media/devtest-lab-store-secrets-in-key-vault/secret-store-pick-a-secret.png)

## <a name="use-a-secret-in-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunda gizli kullanma
Aşağıdaki örnekte gösterildiği gibi Bir VM oluşturmak için kullanılan bir Azure Kaynak Yöneticisi şablonunda gizli adınızı belirtebilirsiniz:

![Formül veya ortamda gizli kullanın](media/devtest-lab-store-secrets-in-key-vault/secret-store-arm-template.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Sırrı kullanarak bir VM oluşturma](devtest-lab-add-vm.md) 
- [Sırrı kullanarak bir formül oluşturma](devtest-lab-manage-formulas.md)
- [Sırrı kullanarak bir ortam oluşturma](devtest-lab-create-environment-from-arm.md)
