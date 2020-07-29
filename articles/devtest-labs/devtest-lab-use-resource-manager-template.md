---
title: Bir sanal makinenin Azure Resource Manager şablonunu görüntüleyin ve kullanın
description: Diğer VM 'Ler oluşturmak için bir sanal makineden Azure Resource Manager şablonunu nasıl kullanacağınızı öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: e58b54d61653f1edba3c3893edb902a94fbf9f2b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87272700"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak sanal makineler oluşturma 

DevTest Labs 'de [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)aracılığıyla bir sanal makıne (VM) oluştururken, VM 'yi kaydetmeden önce Azure Resource Manager şablonunu görüntüleyebilirsiniz. Şablon daha sonra aynı ayarlarla daha fazla laboratuvar VM 'Leri oluşturmak için temel olarak kullanılabilir.

Bu makalede çoklu VM ve tek VM Kaynak Yöneticisi şablonları açıklanmakta ve VM oluştururken bir şablonu nasıl görüntüleyebileceğiniz ve kaydedeceğiniz gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Çoklu VM ile tek VM Kaynak Yöneticisi şablonları
Kaynak Yöneticisi şablonu kullanarak DevTest Labs 'de sanal makine oluşturmanın iki yolu vardır: Microsoft. DevTestLab/Labs/virtualmachines kaynağını sağlama veya Microsoft. COMPUTE/virtualmachines kaynağını sağlama. Her biri farklı senaryolarda kullanılır ve farklı izinler gerektirir.

- Microsoft. DevTestLab/Labs/virtualmachines kaynak türü kullanan Kaynak Yöneticisi şablonlar (şablondaki "kaynak" özelliğinde bildirildiği gibi), tek tek Laboratuvar VM 'Leri sağlayabilir. Ardından her VM, DevTest Labs sanal makineler listesinde tek bir öğe olarak görünür:

   ![DevTest Labs sanal makineler listesinde tek öğe olarak VM 'lerin listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Bu tür Kaynak Yöneticisi şablonu, **New-AzResourceGroupDeployment** Azure PowerShell komutu ya da **az Group Deployment Create**Azure CLI komutu aracılığıyla sağlanabilir. Yönetici izinleri gerektirir, bu nedenle bir DevTest Labs kullanıcı rolüyle atanan kullanıcılar dağıtımı gerçekleştiremez. 

- Microsoft. COMPUTE/virtualmachines kaynak türü kullanan Kaynak Yöneticisi şablonlar, DevTest Labs sanal makineler listesinde tek bir ortam olarak birden çok VM sağlayabilir:

   ![DevTest Labs sanal makineler listesinde tek öğe olarak VM 'lerin listesi](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Aynı ortamdaki VM 'Ler birlikte yönetilebilir ve aynı yaşam döngüsünü paylaşabilir. DevTest Labs kullanıcı rolüyle atanan kullanıcılar, yöneticinin bu şekilde Laboratuvarı yapılandırdığı sürece bu şablonları kullanarak ortamlar oluşturabilir.

Bu makalenin geri kalanında, Microsoft. DevTestLab/Labs/virtualmachines kullanan Kaynak Yöneticisi şablonlar ele alınmaktadır. Bunlar, laboratuvar VM 'Leri oluşturmayı (örneğin, çakışan VM 'Ler) veya altın görüntü üretimini (örneğin, görüntü fabrikası) otomatik hale getirmek için laboratuar yöneticileri tarafından kullanılır.

[Azure Resource Manager şablonları oluşturmak Için en iyi uygulamalar](../azure-resource-manager/templates/template-best-practices.md) , güvenilir ve kullanımı kolay Azure Resource Manager şablonlar oluşturmanıza yardımcı olacak birçok yönerge ve öneri sunar.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Bir sanal makinenin Kaynak Yöneticisi şablonunu görüntüleyin ve kaydedin
1. Sanal makine oluşturmaya başlamak için [laboratuvara Ilk VM 'Nizi oluşturma bölümündeki](tutorial-create-custom-lab.md#add-a-vm-to-the-lab) adımları izleyin.
1. Sanal makineniz için gerekli bilgileri girin ve bu VM için istediğiniz yapıtları ekleyin.
1. **Gelişmiş ayarlar** sekmesine gidin. 
1. Ayarları Yapılandır penceresinin en altında **ARM şablonunu görüntüle**' yi seçin.
1. Daha sonra başka bir sanal makine oluşturmak için kullanmak üzere Kaynak Yöneticisi şablonunu kopyalayın ve kaydedin.

   ![Daha sonra kullanılmak üzere kaydedilecek Kaynak Yöneticisi şablonu](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Kaynak Yöneticisi şablonunu kaydettikten sonra, şablonu kullanmadan önce şablonun parametreler bölümünü güncelleştirmeniz gerekir. Gerçek Kaynak Yöneticisi şablonunun dışında yalnızca parametreleri özelleştiren bir parameter.jsoluşturabilirsiniz. 

![JSON dosyası kullanarak parametreleri özelleştirme](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

Kaynak Yöneticisi şablonu artık [VM oluşturmak](devtest-lab-create-environment-from-arm.md)için kullanıma hazırdır.

## <a name="set-expiration-date"></a>Sona erme tarihi ayarla
Eğitim, tanıtımlar ve denemeler gibi senaryolarda, sanal makineler oluşturmak ve bunları sabit bir süreden sonra otomatik olarak silmek isteyebilirsiniz, böylece gereksiz maliyetlere tabi kalmazsınız. VM için **ExpirationDate** özelliğini belirterek, bir sona erme tarihi olan bir laboratuar sanal makinesi oluşturabilirsiniz. [GitHub deponuzda](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-customimage-with-expiration)aynı kaynak yöneticisi şablonuna göz atın.



### <a name="next-steps"></a>Sonraki adımlar
* [Kaynak Yöneticisi şablonlarıyla çoklu VM ortamları oluşturmayı](devtest-lab-create-environment-from-arm.md)öğrenin.
* [VM oluşturmak için Kaynak Yöneticisi şablonu dağıtma](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments)
* [Ortak DevTest Labs GitHub deposundan](https://github.com/Azure/azure-quickstart-templates)DevTest Labs otomasyonu için daha fazla hızlı başlangıç Kaynak Yöneticisi şablonları bulun.
