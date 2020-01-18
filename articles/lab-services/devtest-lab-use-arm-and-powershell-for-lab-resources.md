---
title: Azure Resource Manager şablonları kullanarak Labs oluşturma veya değiştirme
description: Bir DevTest laboratuvarında otomatik olarak laboratuvar oluşturmak veya değiştirmek için PowerShell ile Azure Resource Manager şablonlarını nasıl kullanacağınızı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 180081829387afeb7e63defe7a22378aa8d417d6
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170302"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Azure Resource Manager şablonlarını ve PowerShell 'i kullanarak laboratuvarları otomatik olarak oluşturma veya değiştirme

DevTest Labs, hızla ve otomatik olarak yeni laboratuvarlar oluşturmanıza veya mevcut laboratuvarlara ve ardından bu kaynakları dağıtmanıza yardımcı olabilecek birçok Azure Resource Manager şablon ve PowerShell komut dosyası sağlar.

Bu makale, laboratuvarlarınızın oluşturulmasını, değiştirilmesini ve dağıtımını otomatik hale getirmek için bu şablonları ve betikleri kullanma sürecinde size kılavuzluk eder. Bu makalede ayrıca, PowerShell kullanarak DevTest Labs 'de bazı yaygın görevleri gerçekleştirmek için nasıl daha fazla bilgi bulabileceğiniz gösterilmektedir.

## <a name="step-1-gather-your-templates-and-scripts"></a>Adım 1: şablonlarınızı ve betikleri toplayın
Genel [GitHub deponuzda](https://github.com/Azure/azure-devtestlab)önceden oluşturulmuş [Azure Resource Manager şablonları](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) ve [PowerShell betikleri](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts) bulabilirsiniz. Bunları olduğu gibi kullanın veya gereksinimlerinize göre özelleştirin ve kendi [özel git](devtest-lab-add-artifact-repo.md)deponuzda depolayın.

## <a name="step-2-modify-your-azure-resource-manager-template"></a>2\. Adım: Azure Resource Manager şablonunuzu değiştirme
Daha önce hiç şablon oluşturmadıysanız [ilk Azure Resource Manager şablonunuzu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) bölümündeki adımları izleyebilirsiniz.

Ayrıca, [Azure Resource Manager şablonları oluşturmak Için en iyi yöntemler](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) , güvenilir ve kullanımı kolay Azure Resource Manager şablonlar oluşturmanıza yardımcı olacak birçok yönerge ve öneri sunar. Genellikle, belirtilen yaklaşımlardan veya örneklerden birinin varyasyonunu kullanacaksınız ve gereksinimlerinize göre şablonunuzu değiştirirsiniz.

## <a name="step-3-deploy-resources-with-powershell"></a>3\. Adım: PowerShell ile kaynak dağıtma
Şablonlarınızı ve betikleri özelleştirdikten sonra, [Kaynak Yöneticisi şablonlar ve Azure PowerShell sahip kaynakları dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)için gereken adımları izleyin. Bu makalede, kaynaklarınızı Azure 'a dağıtmak için Azure Resource Manager şablonlarla Azure PowerShell kullanma hakkında genel bilgiler sağlanmaktadır.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>PowerShell kullanarak DevTest Labs 'de gerçekleştirebileceğiniz ortak görevler
PowerShell kullanarak otomatikleştirebileceğiniz diğer birçok ortak görev vardır. Belgelerde aşağıdaki bölümlerde, bu görevleri gerçekleştirmek için gereken adımlar ana hatlarıyla verilmiştir.

* [PowerShell kullanarak bir VHD dosyasından özel görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [PowerShell kullanarak VHD dosyasını laboratuvar depolama hesabına yükleyin](devtest-lab-upload-vhd-using-powershell.md)
* [PowerShell kullanarak bir laboratuvara dış Kullanıcı ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell kullanarak laboratuvar özel rolü oluşturma](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş şablonlarınızı veya betiklerinizi depoladığınız [özel bir git deposu](devtest-lab-add-artifact-repo.md) oluşturmayı öğrenin.
* [Azure hızlı başlangıç şablonu galerisinden Azure Resource Manager şablonlarını](https://github.com/Azure/azure-quickstart-templates)gezin.
