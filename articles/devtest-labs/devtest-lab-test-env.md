---
title: VM ve PaaS test ortamları için Azure DevTest Labs kullanın | Microsoft Docs
description: VM ve PaaS test ortamı senaryolarında Azure DevTest Labs nasıl kullanacağınızı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: c6b458091a8e5e22cca55d401e89e5e13bcf9de9
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896217"
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>VM ve PaaS test ortamları için Azure DevTest Labs kullanma

Azure DevTest Labs’i çok sayıda önemli senaryoyu uygulamak için kullanabilirsiniz ancak başlıca senaryolardan biri, test uzmanları için makineleri barındırmak üzere DevTest Labs kullanmayı içerir. 

Bu senaryoda DevTest Labs şu avantajları sağlar:

- Test uzmanları, yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayabilir ve bu sayede uygulamalarının en son sürümünü test edebilir.
- Test uzmanları birden fazla test aracısı sağlayarak yük testlerinin ölçeğini artırabilirler.
- Yöneticiler, aşağıdakileri sağlayarak maliyetleri denetleyebilir:
  - Test ediciler gerekenden daha fazla VM alamaz.
  - VM 'Ler kullanımda olmadığında kapanır.

![Eğitim için DevTest Labs kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, sınayıcı gereksinimlerini karşılamak için kullanılan çeşitli Azure DevTest Labs özellikleri ve bir laboratuvarı kurmak için izlenecek ayrıntılı adımlar hakkında bilgi edineceksiniz.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Azure DevTest Labs ile test ortamlarını uygulama
1. **Laboratuvar oluşturma** 
   
    Laboratuvarlar Azure DevTest Labs başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, laboratuvara Kullanıcı (test ediciler) ekleme, maliyetleri denetlemek için ilkeler ayarlama, hızlı bir şekilde oluşturabileceğiniz VM görüntülerini tanımlama ve daha fazlasını yapma gibi görevleri gerçekleştirebilirsiniz.  
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portal Azure DevTest Labs ' de laboratuvar oluşturmayı öğrenin. |
2. **Kullanıma yönelik Market görüntülerini ve özel görüntüleri kullanarak dakikalar içinde VM oluşturma** 
   
    Azure Market 'te çok çeşitli görüntülerden kullanıma hazır görüntüler seçebilir ve bunları laboratuvarda kullanılabilir hale getirebilirsiniz. Kullanıma alınan görüntüler gereksinimlerinizi karşılamıyorsa, Azure Marketi 'nden kullanıma hazırlanmış bir görüntü kullanarak, ihtiyacınız olan tüm yazılımları yükleyerek ve VM 'yi laboratuvarda özel bir görüntü olarak kaydederek bir laboratuvar VM 'si oluşturarak özel bir görüntü oluşturabilirsiniz.

    Özel görüntüler kullanacaksanız, görüntülerinizi oluşturmak ve dağıtmak için bir görüntü fabrikası kullanmayı düşünün. Görüntü fabrikası, yapılandırılmış görüntülerinizi düzenli olarak oluşturup dağıtan bir yapılandırma olarak yapılandırma çözümüdür. Bu, temel işletim sistemi ile bir VM oluşturulduktan sonra sistemi el ile yapılandırmak için gereken süreyi kaydeder.
  
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini beyaz listeye eklemeyi öğrenin ve yalnızca Sınayıcılar için istediğiniz görüntüleri seçilebilir hale getirebilirsiniz.|
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Test edicilerin özel görüntüyü kullanarak hızlı bir şekilde VM oluşturabilmesi için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun.|
   | [Image Factory hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir görüntü fabrikası ayarlamayı ve kullanmayı açıklayan bir video izleyin.|

3. **Test makineleri için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs bir formül, bir VM oluşturmak için kullanılan varsayılan özellik değerlerinin bir listesidir. Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek laboratuvara bir formül oluşturabilirsiniz. Her sınayıcı, laboratuvardaki formülü görebilir ve bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Sanal makine oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntü, VM boyutu (CPU ve RAM birleşimi) ve bir sanal ağ seçerek nasıl formül oluşturabileceğinizi öğrenin.|

3. **Çoklu VM testi ortamları oluşturma** 
   
    Azure Resource Manager şablonları kullanarak Azure çözümünüzün altyapısını ve yapılandırmasını tanımlayabilir ve tutarlı bir durumda birden çok test sanal makinelerini tekrar tekrar dağıtabilirsiniz.

    Azure PaaS kaynakları, bir Kaynak Yöneticisi şablonundan bir ortamda sağlanabilir ve maliyet izleme ' de görünür. Ancak, sanal makine otomatik kapatması PaaS kaynakları için de geçerlidir.

    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md) |Test ortamınız için tutarlı bir durumda birden çok VM dağıtımını nasıl sağlayabileceğinizi öğrenin.|

4. **Esnek VM özelleştirmesini etkinleştirmek için yapıt oluşturma**

   Yapıtlar, bir VM sağlandıktan sonra uygulamanızı dağıtmak ve yapılandırmak için kullanılır. Yapıtlar şu olabilir:

   - SANAL makineye yüklemek istediğiniz aracılar, Fiddler ve Visual Studio gibi araçlar.
   - VM 'de çalıştırmak istediğiniz eylemler (örneğin, bir depoyu kopyalama).
   - Test etmek istediğiniz uygulamalar.

   Birçok yapıt zaten kullanıma hazır. Ancak, özel gereksinimleriniz için daha fazla özelleştirme istiyorsanız kendi özel yapıtlarınızı oluşturabilirsiniz.

   Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM 'niz için özel yapılar oluşturma](devtest-lab-artifact-author.md) |Laboratuvarınızda sanal makineler için kendi özel yapılarınızı oluşturun.|
   | [Azure DevTest Labs kullanım için özel yapıtları ve Azure Resource Manager şablonlarını depolamak üzere bir git deposu ekleyin](devtest-lab-add-artifact-repo.md) |Özel yapıtlarınızı kendi özel git deponuzda nasıl depolayacağınızı öğrenin.|

5. **Maliyetleri denetleme**
   
    Azure DevTest Labs laboratuvarda laboratuarda bir sınayıcı tarafından oluşturulabilecek maksimum VM sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak sağlar. 
   
    Test takımınızın ayarlanmış bir iş zamanlaması varsa ve günün belirli bir saatinde tüm VM 'Leri durdurmak ve ardından bu gün otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik olarak başlatma ve otomatik başlatma ilkeleri ayarlayarak kolayca bunu yapabilirsiniz. 
   
    Son olarak, uygulama geliştirme tamamlandığında, tek bir PowerShell betiği çalıştırarak tüm VM 'Leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvardaki ilkeleri ayarlayarak maliyetleri denetleyin. |
   | [PowerShell betiği kullanarak tüm Laboratuvar sanal makinelerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Test tamamlandığında, tek bir işlemdeki tüm laboratuvarları silin.|

1. **Laboratuvara sanal ağ ekleme** 
   
    DevTest Labs her bir laboratuvar oluşturulduğunda yeni bir sanal ağ (VNET) oluşturur. Kendi sanal ağınızı yapılandırdıysanız (örneğin, ExpressRoute veya siteden siteye VPN kullanarak), bu VNET 'i laboratuvarın sanal ağ ayarlarına ekleyerek VM 'Ler oluşturulurken kullanılabilir hale getirebilirsiniz.

    Ayrıca, VM oluşturulurken VM 'yi bir etki alanına birleştiren Azure Active Directory etki alanına katılma yapıtı mevcuttur. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs bir sanal ağ yapılandırma](devtest-lab-configure-vnet.md) |Azure portal kullanarak Azure DevTest Labs bir sanal ağı nasıl yapılandıracağınızı öğrenin.|

6. **Laboratuvarı her sınayıcı ile paylaşma**
   
    Laboratuvarlara, test testlerinizi paylaştığınız bir bağlantı kullanılarak doğrudan erişilebilir. [Microsoft hesabı](devtest-lab-faq.md#what-is-a-microsoft-account)sahip oldukları sürece, bir Azure hesabı da olması gerekmez. Test edenler diğer test ediciler tarafından oluşturulan VM 'Leri göremez.  
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs laboratuvara bir test eden ekleme](devtest-lab-add-devtest-user.md) |Laboratuvara test ediciler eklemek için Azure portal kullanın.|
   | [PowerShell betiği kullanarak laboratuvara test edici ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Laboratuvara test ediciler eklemeyi otomatikleştirmek için PowerShell 'i kullanın. |
   | [Laboratuvara bağlantı alın](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Test edicilerin bir köprü aracılığıyla doğrudan laboratuvara nasıl erişebileceğini öğrenin.|

7. **Daha fazla ekip için laboratuvar oluşturmayı otomatikleştirin** 
   
    Özel ayarlar dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilir, bir Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları yeniden oluşturmak için onu ve yeniden kullanabilirsiniz. 
   
    Aşağıdaki tabloda yer alan bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonları kullanarak Azure DevTest Labs laboratuvarları oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

