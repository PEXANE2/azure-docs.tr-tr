---
title: Azure Kaynak Yöneticisi şablonlarını kullanarak laboratuvar oluşturma veya değiştirme
description: DevTest laboratuvarında laboratuvarları otomatik olarak oluşturmak veya değiştirmek için PowerShell ile Azure Kaynak Yöneticisi şablonlarını nasıl kullanacağınızı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170302"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Azure Kaynak Yöneticisi şablonlarını ve PowerShell'i kullanarak laboratuvarları otomatik olarak oluşturun veya değiştirin

DevTest Labs, hızlı ve otomatik olarak yeni laboratuvarlar oluşturmanıza veya varolan laboratuvarları değiştirmenize ve bu kaynakları dağıtmanıza yardımcı olabilecek birçok Azure Kaynak Yöneticisi şablonu ve PowerShell komut dosyası sağlar.

Bu makale, laboratuvarlarınızın oluşturulmasını, değiştirilmesini ve dağıtımını otomatikleştirmek için bu şablonları ve komut dosyalarını kullanma sürecinde size yol gösterir. Bu makalede, DevTest Labs'da bazı yaygın görevleri gerçekleştirmek için PowerShell'in nasıl kullanılacağı hakkında daha fazla bilgi bulabileceğiniz yerler de gösterilmektedir.

## <a name="step-1-gather-your-templates-and-scripts"></a>Adım 1: Şablonlarınızı ve komut dosyalarınızı toplama
Önceden yapılmış Azure [Kaynak Yöneticisi şablonlarını](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) ve [PowerShell komut dosyalarını](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts) herkese açık [GitHub depomuzda](https://github.com/Azure/azure-devtestlab)bulabilirsiniz. Bunları olduğu gibi kullanın veya ihtiyaçlarınız için özelleştirin ve bunları kendi [özel Git repo'nuzda](devtest-lab-add-artifact-repo.md)saklayın.

## <a name="step-2-modify-your-azure-resource-manager-template"></a>Adım 2: Azure Kaynak Yöneticisi şablonunuzu değiştirme
Daha önce hiç şablon oluşturmadıysanız, [ilk Azure Kaynak Yöneticisi şablonunuzu oluştur](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) adımlarını izleyebilirsiniz.

Ayrıca, [Azure Kaynak Yöneticisi şablonları oluşturmak için en iyi uygulamalar,](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) güvenilir ve kullanımı kolay Azure Kaynak Yöneticisi şablonları oluşturmanıza yardımcı olacak birçok yönerge ve öneri sunar. Genellikle, sağlanan yaklaşımlardan veya örneklerden birinin bir varyasyonunu kullanır ve şablonunuzu gereksinimlerinize göre değiştirirsiniz.

## <a name="step-3-deploy-resources-with-powershell"></a>Adım 3: PowerShell ile kaynakları dağıtma
Şablonlarınızı ve komut dosyalarınızı özelleştirdikten sonra, [Kaynak Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)için gereken adımları izleyin. Makale, kaynaklarınızı Azure'a dağıtmak için Azure Kaynak Yöneticisi şablonlarıyla Azure PowerShell'i kullanma hakkında genel bilgiler sağlar.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>PowerShell kullanarak DevTest laboratuvarlarında gerçekleştirebileceğiniz yaygın görevler
PowerShell'i kullanarak otomatikleştirebileceğiniz birçok yaygın görev vardır. Belgelerin aşağıdaki bölümleri, bu görevleri gerçekleştirmek için gereken adımları ana hatlarını gösterir.

* [PowerShell'i kullanarak Bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [PowerShell'i kullanarak VHD dosyasını laboratuvarın depolama hesabına yükleyin](devtest-lab-upload-vhd-using-powershell.md)
* [PowerShell'i kullanarak laboratuvara harici kullanıcı ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell'i kullanarak laboratuvarözel rolü oluşturma](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş şablonlarınızı veya komut dosyalarınızı depoladığınız özel bir [Git deposunu](devtest-lab-add-artifact-repo.md) nasıl oluşturacağınızı öğrenin.
* Azure [Quickstart şablon galerisinden Azure Kaynak Yöneticisi şablonlarını](https://github.com/Azure/azure-quickstart-templates)keşfedin.
