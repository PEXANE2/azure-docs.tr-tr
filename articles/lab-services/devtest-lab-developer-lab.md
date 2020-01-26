---
title: Geliştiriciler için Azure DevTest Labs kullanma | Microsoft Docs
description: Geliştirici gereksinimlerini karşılamak için kullanılabilecek Azure DevTest Labs özellikleri ve bir laboratuvarı kurmak için izleyebileceğiniz ayrıntılı adımları öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: ae44696f62a085bf1e798f0915e2bd1e27c3a78f
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760156"
---
# <a name="use-azure-devtest-labs-for-developers"></a>Geliştiriciler için Azure DevTest Labs kullanma
Azure DevTest Labs, çok sayıda önemli senaryoyu uygulamak için kullanılabilir ancak başlıca senaryolardan biri, geliştiriciler için geliştirme makinelerini barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Geliştiriciler geliştirme makinelerini isteğe bağlı olarak hızlı bir şekilde temin edebilir.
- Geliştiriciler, geliştirme makinelerini gerekli olan her durumda özelleştirebilir.
- Yöneticiler, aşağıdakileri sağlayarak maliyetleri denetleyebilir:
  - Geliştiriciler, geliştirme için gereksiniminden daha fazla VM alamaz.
  - VM 'Ler kullanımda olmadığında kapanır. 

![Eğitim için DevTest Labs kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, geliştirici gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri ve bir laboratuvarı kurmak için izleyebileceğiniz ayrıntılı adımlar hakkında bilgi edineceksiniz.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Azure DevTest Labs ile geliştirici ortamları uygulama
1. **Laboratuvar oluşturma** 
   
    Laboratuvarlar Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, laboratuvara Kullanıcı (geliştiriciler) ekleme, maliyetleri denetlemek için ilkeler ayarlama, hızlı bir şekilde oluşturabileceğiniz VM görüntülerini tanımlama ve daha birçok görev gerçekleştirebilirsiniz.  
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs Laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portal Azure DevTest Labs ' de laboratuvar oluşturmayı öğrenin. |
2. **Kullanıma yönelik Market görüntülerini ve özel görüntüleri kullanarak dakikalar içinde VM oluşturma** 
   
    Azure Market 'te çok çeşitli görüntülerden kullanıma hazır görüntüler seçebilir ve bunları laboratuvarda kullanılabilir hale getirebilirsiniz. Kullanıma alınan görüntüler gereksinimlerinizi karşılamıyorsa, Azure Marketi 'nden kullanıma hazırlanmış bir görüntü kullanarak, ihtiyacınız olan tüm yazılımları yükleyerek ve VM 'yi laboratuvarda özel bir görüntü olarak kaydederek bir laboratuvar VM 'si oluşturarak özel bir görüntü oluşturabilirsiniz.

    Özel görüntüler kullanacaksanız, görüntülerinizi oluşturmak ve dağıtmak için bir görüntü fabrikası kullanmayı düşünün. Görüntü fabrikası, yapılandırılmış görüntülerinizi düzenli olarak oluşturup dağıtan bir yapılandırma olarak yapılandırma çözümüdür. Bu, temel işletim sistemi ile bir VM oluşturulduktan sonra sistemi el ile yapılandırmak için gereken süreyi kaydeder.
  
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Marketi görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini beyaz listeye ekleme hakkında bilgi edinmek için, yalnızca geliştiriciler için istediğiniz görüntüleri seçime uygun hale getirebilirsiniz.|
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Geliştiricilerin özel görüntüyü kullanarak hızlı bir şekilde VM oluşturabilmesi için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun.|
   | [Image Factory hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir görüntü fabrikası ayarlamayı ve kullanmayı açıklayan bir video izleyin.|

3. **Geliştirici makineleri için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs bir formül, bir VM oluşturmak için kullanılan varsayılan özellik değerlerinin bir listesidir. Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek laboratuvara bir formül oluşturabilirsiniz. Her geliştirici, formülü laboratuvara görebilir ve bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makine oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek nasıl formül oluşturabileceğinizi öğrenin.|

4. **Esnek VM özelleştirmesini etkinleştirmek için yapıt oluşturma**

   Yapıtlar, bir VM sağlandıktan sonra uygulamanızı dağıtmak ve yapılandırmak için kullanılır. Yapıtlar şunlar olabilir:

   - SANAL makineye yüklemek istediğiniz aracılar, Fiddler ve Visual Studio gibi araçlar.
   - VM 'de çalıştırmak istediğiniz eylemler (örneğin, bir depoyu kopyalama).
   - Test etmek istediğiniz uygulamalar.

   Birçok yapıt zaten kullanıma hazır. Özel gereksinimleriniz için daha fazla özelleştirme istiyorsanız kendi özel yapıtlarınızı oluşturabilirsiniz.

   Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM 'niz için özel yapılar oluşturma](devtest-lab-artifact-author.md) |Laboratuvarınızda sanal makineler için kendi özel yapılarınızı oluşturun.|
   | [Azure DevTest Labs kullanım için özel yapıtları ve Azure Resource Manager şablonlarını depolamak üzere bir git deposu ekleyin](devtest-lab-add-artifact-repo.md) |Özel yapıtlarınızı kendi özel git deponuzda nasıl depolayacağınızı öğrenin.|

5. **Denetim maliyetleri**
   
    Azure DevTest Labs laboratuvarda laboratuarda bir geliştirici tarafından oluşturulabilecek maksimum VM sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak sağlar. 
   
    Geliştirici takımınızın bir iş zamanlaması kümesi varsa ve günün belirli bir saatinde tüm VM 'Leri durdurmak ve ardından bu gün sonra otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik olarak başlatma ve otomatik başlatma ilkeleri ayarlayarak kolayca bunu yapabilirsiniz. 
   
    Son olarak, uygulama geliştirme tamamlandığında, tek bir PowerShell betiği çalıştırarak tüm VM 'Leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvardaki ilkeleri ayarlayarak maliyetleri denetleyin. |
   | [PowerShell betiği kullanarak tüm Laboratuvar sanal makinelerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Geliştirme tamamlandığında tüm laboratuvarları tek bir işlemde silin.|

1. **VM 'ye sanal ağ ekleme** 
   
    DevTest Labs her bir laboratuvar oluşturulduğunda yeni bir sanal ağ (VNET) oluşturur. Kendi sanal ağınızı yapılandırdıysanız (örneğin, ExpressRoute veya siteden siteye VPN kullanarak), bu VNET 'i laboratuvarın sanal ağ ayarlarına ekleyerek VM 'Ler oluşturulurken kullanılabilir hale getirebilirsiniz.

    Ayrıca, VM oluşturulurken VM 'yi bir etki alanına katılacak bir Azure Active Directory etki alanına ekleme yapıtı mevcuttur. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs bir sanal ağ yapılandırma](devtest-lab-configure-vnet.md) |Azure portal kullanarak Azure DevTest Labs bir sanal ağı nasıl yapılandıracağınızı öğrenin.|

6. **Laboratuvarı her geliştirici ile paylaşma**
   
    Laboratuvarlara, geliştiricilerle paylaştığınız bir bağlantı kullanılarak doğrudan erişilebilir. [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account)sahip oldukları sürece, bir Azure hesabı da olması gerekmez. Geliştiriciler diğer geliştiriciler tarafından oluşturulan VM 'Leri göremez.  
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs laboratuvara geliştirici ekleme](devtest-lab-add-devtest-user.md) |Laboratuvarınızda geliştiriciler eklemek için Azure portal kullanın.|
   | [PowerShell betiği kullanarak geliştiricilere laboratuvar ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Geliştiricilere laboratuvar ekleme işlemini otomatik hale getirmek için PowerShell 'i kullanın. |
   | [Laboratuvara bağlantı alın](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Geliştiricilerin bir köprü aracılığıyla laboratuvara doğrudan nasıl erişebileceğini öğrenin.|

7. **Daha fazla ekip için laboratuvar oluşturmayı otomatikleştirin** 
   
    Özel ayarlar dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilir, bir Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları yeniden oluşturmak için onu ve yeniden kullanabilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonları kullanarak Azure DevTest Labs laboratuvarları oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

