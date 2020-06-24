---
title: Eğitim için Azure DevTest Labs kullanma | Microsoft Docs
description: Bu makalede, Azure DevTest Labs eğitim için laboratuvar ayarlamak üzere izleyebileceğiniz ayrıntılı adımlar sağlanmaktadır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 8f0a930d6e3c04548ade71f6d4e3294114eb60a2
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896224"
---
# <a name="use-azure-devtest-labs-for-training"></a>Eğitim için Azure DevTest Labs kullanma
Azure DevTest Labs, geliştirme ve test 'e ek olarak birçok temel senaryoyu uygulamak için kullanılabilir. Bu senaryolarından biri, eğitim için bir laboratuvar ayarlamaya yöneliktir. Azure DevTest Labs, eğitiminin aynı ve yalıtılmış ortamlar oluşturmak için kullanabileceği özel şablonlar sağlayabileceğiniz bir laboratuvar oluşturmanıza olanak sağlar. Eğitim ortamlarının yalnızca ihtiyaç duydukları ve sanal makineler gibi yeterli kaynak (eğitim için gerekli) içermesi durumunda her bir Trae tarafından kullanılabilir olmasını sağlamak için ilkeler uygulayabilirsiniz. Son olarak, laboratuvarı tek tıklamayla erişebilen traınees ile kolayca paylaşabilirsiniz.

![Eğitim için DevTest Labs kullanın](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs, herhangi bir sanal ortamda eğitim yapmak için gereken aşağıdaki gereksinimleri karşılar: 

* Diğer işlemlerde oluşturulan VM 'Leri göremez
* Her eğitim makinesi aynı olmalıdır
* Eğitim ortamlarını hızlı bir şekilde temin edebilirsiniz
* Eğitimin, eğitim için ihtiyaç duydukları miktardan daha fazla VM alamaz ve ayrıca onları kullandıklarında sanal makineleri kapatması için maliyeti denetleyin
* Eğitim laboratuvarı her bir Trae ile kolayca paylaşabilirsiniz
* Eğitim laboratuvarı yeniden kullanın ve tekrar

Bu makalede, daha önce açıklanan eğitim gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri ve eğitim için bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımları öğreneceksiniz.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Azure DevTest Labs ile eğitim uygulama
1. **Laboratuvar oluşturma** 
   
    Laboratuvarlar Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, laboratuvara Kullanıcı ekleme (trages) gibi görevleri gerçekleştirebilir, maliyetleri denetlemek için ilkeler ayarlayabilir, hızlı bir şekilde oluşturabileceğiniz VM görüntülerini tanımlayabilir ve daha fazlasını yapabilirsiniz.   
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portal Azure DevTest Labs ' de laboratuvar oluşturmayı öğrenin. |
2. **Kullanıma yönelik Market görüntülerini ve özel görüntüleri kullanarak dakikalar içinde eğitim VM 'Leri oluşturun** 
   
    Azure Market 'te çok çeşitli görüntülerden kullanıma hazır görüntüler seçebilir ve bunları laboratuvardaki kararlar için kullanılabilir hale getirebilirsiniz. Kullanıma alınan görüntüler gereksinimlerinizi karşılamıyorsa, Azure Marketi 'nden kullanıma yönelik bir görüntü kullanarak bir laboratuvar VM 'si oluşturup, eğitim için ihtiyacınız olan tüm yazılımları yükleyerek ve VM 'yi laboratuvarda özel görüntü olarak kaydederek özel bir görüntü oluşturabilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini nasıl beyaz listeye kullanabileceğinizi öğrenin; yalnızca eğitim için istediğiniz görüntüleri seçerek kullanılabilir hale getirme. |
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Özel görüntü kullanarak hızlı bir şekilde VM oluşturabilmek için eğitim için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun. |
3. **Eğitim makineleri için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs bir formül, bir VM oluşturmak için kullanılan varsayılan özellik değerlerinin bir listesidir. Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek laboratuvara bir formül oluşturabilirsiniz. Her bir seyahat, laboratuvardaki formülü görebilir ve bunu bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makine oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek nasıl formül oluşturabileceğinizi öğrenin. |
4. **Maliyetleri denetleme**
   
    Azure DevTest Labs, laboratuvarda laboratuarda oluşturulabilecek en fazla VM sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak sağlar. 
   
    Çok günlük eğitim yapıyorsanız ve günün belirli bir saatinde tüm VM 'Leri durdurmak ve ardından bu gün otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik kapatmalar ve otomatik başlatma ilkeleri ayarlayarak kolayca bunu yapabilirsiniz. 
   
    Son olarak, Eğitim tamamlandığında, tek bir PowerShell betiği çalıştırarak tüm VM 'Leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvardaki ilkeleri ayarlayarak maliyetleri denetleyin. |
   | [PowerShell betiği kullanarak tüm Laboratuvar sanal makinelerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Eğitim tamamlandığında tüm laboratuvarları tek bir işlemde silin. |
5. **Laboratuvarı her bir Trae ile paylaşma**
   
    Laboratuvarlarla paylaştığınız bir bağlantı kullanılarak laboratulara doğrudan erişilebilir. [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account)sahip oldukları sürece, sizin için de bir Azure hesabı olması gerekmez. Diğer taşlarla oluşturulan VM 'Leri göremezsiniz.  
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs bir laboratuvara yamuk ekleme](devtest-lab-add-devtest-user.md) |Eğitim laboratuvarınızda eğitim eklemek için Azure portal kullanın. |
   | [PowerShell betiği kullanarak laboratuvara ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Eğitim laboratuvarınızda daha fazla taş eklemeyi otomatikleştirmek için PowerShell 'i kullanın. |
   | [Laboratuvara bağlantı alın](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Bir laboratuvara doğrudan köprü yoluyla nasıl erişilebileceğini öğrenin. |
6. **Laboratuvarı yeniden kullanın ve yeniden deneyin** 
   
    Özel ayarlar dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilir, bir Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları yeniden oluşturmak için onu ve yeniden kullanabilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonları kullanarak Azure DevTest Labs laboratuvarları oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

