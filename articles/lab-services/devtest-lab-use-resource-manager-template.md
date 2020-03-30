---
title: Sanal makinenin Azure Kaynak Yöneticisi şablonuna görüntüleyin ve kullanın
description: Diğer SANAL'lar oluşturmak için sanal bir makineden Azure Kaynak Yöneticisi şablonu nasıl kullanacağınızı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 7064fdeec04f4dc5ae2c73c1a3896cf2d10dd01d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169108"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak sanal makineler oluşturma 

[Azure portalı](https://go.microsoft.com/fwlink/p/?LinkID=525040)üzerinden DevTest Labs'da sanal bir makine (VM) oluştururken, VM'yi kaydetmeden önce Azure Kaynak Yöneticisi şablonunu görüntüleyebilirsiniz. Şablon daha sonra aynı ayarlara sahip daha fazla laboratuvar VM oluşturmak için bir temel olarak kullanılabilir.

Bu makalede, Multi-VM ve tek VM Kaynak Yöneticisi şablonları açıklanır ve vm oluştururken şablonu nasıl görüntüleyip kaydedebilirsiniz gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Multi-VM ve tek VM Kaynak Yöneticisi şablonları
Bir Kaynak Yöneticisi şablonu kullanarak DevTest Labs'da VM oluşturmanın iki yolu vardır: Microsoft.DevTestLab/labs/virtualmachines kaynağını sağlama veya Microsoft.Compute/virtualmachines kaynağını sağlama. Her biri farklı senaryolarda kullanılır ve farklı izinler gerektirir.

- Microsoft.DevTestLab/labs/virtualmachines kaynak türü (şablondaki "kaynak" özelliğinde belirtildiği gibi) kullanan Kaynak Yöneticisi şablonları tek tek laboratuvar VM'leri sağlayabilir. Her VM daha sonra DevTest Labs sanal makineler listesinde tek bir öğe olarak gösterir:

   ![DevTest Labs sanal makineler listesinde tek öğe olarak VM listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Bu tür Kaynak Yöneticisi şablonu Azure PowerShell komutu **Yeni-AzResourceGroupDeployment** veya Azure CLI komut **az grup dağıtımı oluşturma**yoluyla sağlanabilir. Yönetici izinleri gerektirdiğinden, DevTest Labs kullanıcı rolüyle atanan kullanıcılar dağıtımı gerçekleştiremez. 

- Microsoft.Compute/virtualmachines kaynak türü kullanan Kaynak Yöneticisi şablonları, DevTest Labs sanal makineler listesinde tek bir ortam olarak birden çok VM sağlayabilir:

   ![DevTest Labs sanal makineler listesinde tek öğe olarak VM listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Aynı ortamdaki VM'ler birlikte yönetilebilir ve aynı yaşam döngüsünü paylaşabilir. DevTest Labs kullanıcı rolüyle atanan kullanıcılar, yönetici nin laboratuarı bu şekilde yapılandırdığı sürece bu şablonları kullanarak ortamlar oluşturabilir.

Bu makalenin geri kalanı, Microsoft.DevTestLab/labs/virtualmachines kullanan Kaynak Yöneticisi şablonlarını tartışır. Bunlar, laboratuvar yöneticileri tarafından laboratuvar VM oluşturma (örneğin, iddia edilebilir VM'ler) veya altın görüntü oluşturma (örneğin, görüntü fabrikası) otomatikleştirmek için kullanılır.

[Azure Kaynak Yöneticisi şablonları oluşturmak için en iyi uygulamalar,](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) güvenilir ve kullanımı kolay Azure Kaynak Yöneticisi şablonları oluşturmanıza yardımcı olacak birçok yönerge ve öneri sunar.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Sanal makinenin Kaynak Yöneticisi şablonuna görüntüleme ve kaydetme
1. Sanal bir makine oluşturmaya başlamak için [laboratuvardaki ilk VM'inizi oluştur](tutorial-create-custom-lab.md#add-a-vm-to-the-lab) adımlarını izleyin.
1. Sanal makineniz için gerekli bilgileri girin ve bu VM için istediğiniz tüm yapıları ekleyin.
1. **Gelişmiş Ayarlar** sekmesine swtich. 
1. Yapı ayarları penceresinin alt kısmında **ARM şablonu görüntüle'yi**seçin.
1. Başka bir sanal makine oluşturmak için daha sonra kullanmak üzere Kaynak Yöneticisi şablonuna kopyalayıp kaydedin.

   ![Daha sonra kullanmak üzere kaydacak Kaynak Yöneticisi şablonu](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Kaynak Yöneticisi şablonu kaydedildikten sonra, kullanmadan önce şablonun parametreler bölümünü güncelleştirmeniz gerekir. Gerçek Kaynak Yöneticisi şablonunun dışında sadece parametreleri özelleştiren bir parameter.json oluşturabilirsiniz. 

![JSON dosyakullanarak parametreleri özelleştirme](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

Kaynak Yöneticisi şablonu artık [bir VM oluşturmak](devtest-lab-create-environment-from-arm.md)için kullanıma hazır.

## <a name="set-expiration-date"></a>Son kullanma tarihini ayarlama
Eğitim, demolar ve denemeler gibi senaryolarda, gereksiz maliyetlere maruz kalmak sizin için sanal makineler oluşturmak ve sabit bir süre sonra bunları otomatik olarak silmek isteyebilirsiniz. VM için **son kullanma Tarihi** özelliğini belirterek son kullanma tarihi olan bir laboratuvar VM'si oluşturabilirsiniz. [GitHub depomuzda](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-customimage-with-expiration)aynı Kaynak Yöneticisi şablonuna göz atın.



### <a name="next-steps"></a>Sonraki adımlar
* [Kaynak Yöneticisi şablonları ile çoklu VM ortamları](devtest-lab-create-environment-from-arm.md)oluşturmayı öğrenin.
* [VM oluşturmak için Kaynak Yöneticisi şablonu dağıtma](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments)
* [Kamu DevTest Labs GitHub repo](https://github.com/Azure/azure-quickstart-templates)DevTest Labs otomasyon için daha hızlı başlat Kaynak Yöneticisi şablonları keşfedin.
