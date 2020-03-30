---
title: VM ve PaaS test ortamları için Azure DevTest Laboratuvarlarını kullanın | Microsoft Dokümanlar
description: VM ve PaaS test ortamı senaryoları için Azure DevTest Laboratuvarlarını nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60623196"
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a>VM ve PaaS test ortamları için Azure DevTest Labs'ı kullanma

Azure DevTest Labs’i çok sayıda önemli senaryoyu uygulamak için kullanabilirsiniz ancak başlıca senaryolardan biri, test uzmanları için makineleri barındırmak üzere DevTest Labs kullanmayı içerir. 

Bu senaryoda DevTest Labs şu avantajları sağlar:

- Test uzmanları, yeniden kullanılabilir şablonları ve yapıtları kullanarak Windows ve Linux ortamlarını hızla sağlayabilir ve bu sayede uygulamalarının en son sürümünü test edebilir.
- Test uzmanları birden fazla test aracısı sağlayarak yük testlerinin ölçeğini artırabilirler.
- Yöneticiler maliyetleri şu şekilde kontrol edebilirler:
  - Test edenler, ihtiyaç duyduklarından daha fazla VM alamazlar.
  - VM'ler kullanılmadığında kapatılır.

![Eğitim için DevTest Laboratuvarlarını kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, test eden gereksinimlerini karşılamak için kullanılan çeşitli Azure DevTest Labs özellikleri ve bir laboratuvar kurmak için izninizlen ayrıntılı adımlar hakkında bilgi edinebilirsiniz.

## <a name="implementing-test-environments-with-azure-devtest-labs"></a>Azure DevTest Labs ile test ortamları uygulama
1. **Laboratuvarı oluşturun** 
   
    Laboratuvarlar, Azure DevTest Labs'ın başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, laboratuvara kullanıcı (sınayıcılar) ekleme, maliyetleri denetlemek için ilkeler ayarlama, hızlı bir şekilde oluşturabilecek VM görüntüleri tanımlama ve daha fazlası gibi görevleri gerçekleştirebilirsiniz.  
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portalındaki Azure DevTest Labs'da nasıl bir laboratuvar oluşturabilirsiniz öğrenin. |
2. **Hazır pazar görüntüleri ve özel görüntüler kullanarak dakikalar içinde VM'ler oluşturun** 
   
    Azure Marketi'ndeki çok çeşitli resimlerden hazır resimleri seçebilir ve bunları laboratuvarda kullanıma sunabilirsiniz. Hazır resimler gereksinimlerinizi karşılamazsa, Azure Marketi'nden hazır bir görüntü kullanarak bir laboratuvar VM oluşturarak, ihtiyacınız olan tüm yazılımları yükleyerek ve VM'yi laboratuvarda özel bir görüntü olarak kaydederek özel bir görüntü oluşturabilirsiniz.

    Özel görüntüler kullanacaksanız, resimlerinizi oluşturmak ve dağıtmak için bir resim fabrikası kullanmayı düşünün. Görüntü fabrikası, yapılandırılan görüntülerinizi düzenli olarak oluşturan ve otomatik olarak dağıtan bir yapılandırma kodu olarak çözümdür. Bu, temel işletim sistemi yle bir VM oluşturulduktan sonra sistemi el ile yapılandırmak için gereken zamandan tasarruf sağlar.
  
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini nasıl beyaz listeye alabildiğinizi ve yalnızca test edenler için istediğiniz görüntüleri seçebileceğinizi öğrenin.|
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Test edenlerin özel görüntüyü kullanarak hızlı bir şekilde Bir VM oluşturabilmeleri için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun.|
   | [Resim fabrikası hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir resim fabrikasının nasıl kurulduğuve kullanılacağını açıklayan bir video izleyin.|

3. **Test makineleri için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs'daki formül, VM oluşturmak için kullanılan varsayılan özellik değerlerinin listesidir. Bir görüntü, VM boyutu (CPU ve RAM kombinasyonu) ve sanal ağ seçerek laboratuvarda bir formül oluşturabilirsiniz. Her test leyici formülü laboratuvarda görebilir ve bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [VM'ler oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntüyü, VM boyutunu (CPU ve RAM kombinasyonu) ve sanal ağ alarak nasıl bir formül oluşturabileceğinizi öğrenin.|

3. **Çoklu VM test ortamları oluşturma** 
   
    Azure çözümünüzün altyapısını ve yapılandırmasını tanımlamak ve tutarlı bir durumda sürekli olarak birden çok test VM'si dağıtmak için Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.

    Azure PaaS kaynakları bir ortamda Kaynak Yöneticisi şablonundan sağlanabilir ve maliyet izlemede görünebilir. Ancak, VM otomatik kapatma PaaS kaynakları için geçerli değildir.

    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md) |Test ortamınız için tutarlı bir durumda birden çok VM'yi nasıl dağıtabileceğinizi öğrenin.|

4. **Esnek VM özelleştirmesini sağlamak için yapı oluşturma**

   Yapılar, bir VM sağlandıktan sonra uygulamanızı dağıtmak ve yapılandırmak için kullanılır. Eserler olabilir:

   - Aracılar, Kemancı ve Visual Studio gibi VM'ye yüklemek istediğiniz araçlar.
   - VM'de çalıştırmak istediğiniz eylemler - örneğin bir repo klonlama.
   - Test etmek istediğiniz uygulamalar.

   Birçok eser zaten kullanıma hazır. Ancak, özel gereksinimleriniz için daha fazla özelleştirme istiyorsanız, kendi özel yapılarınızı oluşturabilirsiniz.

   Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM'iniz için özel yapılar oluşturun](devtest-lab-artifact-author.md) |Laboratuarınızdaki sanal makineler için kendi özel yapılarınızı oluşturun.|
   | [Azure DevTest Labs'da kullanılmak üzere özel yapıları ve Azure Kaynak Yöneticisi şablonlarını depolamak için Git deposu ekleme](devtest-lab-add-artifact-repo.md) |Özel yapılarınızı kendi özel Git repo'nuzda nasıl depolayacağız öğrenin.|

5. **Kontrol maliyetleri**
   
    Azure DevTest Labs, laboratuvardaki bir test oluşturucu tarafından oluşturulabilecek maksimum VM sayısını belirtmek için laboratuvarda bir ilke ayarlamanıza olanak tanır. 
   
    Test ekibinizin belirli bir çalışma zamanlaması varsa ve tüm VM'leri günün belirli bir saatinde durdurmak ve ertesi gün otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik kapatma ve otomatik başlatma ilkeleri ayarlayarak bunu kolayca gerçekleştirebilirsiniz. 
   
    Son olarak, uygulama geliştirme tamamlandığında, tek bir PowerShell komut dosyası çalıştırarak tüm VM'leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvarda ilkeler ayarlayarak maliyetleri kontrol edin. |
   | [PowerShell komut dosyası kullanarak tüm laboratuvar VM'lerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Test tamamlandığında tüm laboratuarları tek bir işlemde silin.|

1. **Laboratuvara sanal ağ ekleme** 
   
    DevTest Labs, bir laboratuvar oluşturulduğunda yeni bir sanal ağ (VNET) oluşturur. Kendi VNET'inizi yapılandırdıysanız -örneğin, ExpressRoute veya siteden siteye VPN kullanarak- bu VNET'i laboratuvarınızın sanal ağ ayarlarına ekleyerek VM'ler oluştururken kullanılabilir hale getirebilirsiniz.

    Ayrıca, VM oluşturulurken bir etki alanına VM'yi birleştiren bir Azure Active Directory join artifakı vardır. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'da sanal ağı yapılandırma](devtest-lab-configure-vnet.md) |Azure portalını kullanarak Azure DevTest Labs'da sanal ağı nasıl yapılandırılayacağınızı öğrenin.|

6. **Laboratuarı her test edenle paylaşın**
   
    Laboratuvarlara, test edenlerle paylaştığınız bir bağlantıyı kullanarak doğrudan erişilebilir. [Microsoft hesapları](devtest-lab-faq.md#what-is-a-microsoft-account)olduğu sürece Azure hesapları olması bile gerekmez. Sınayıcılar, diğer sınayıcılar tarafından oluşturulan VM'leri göremez.  
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'daki bir laboratuvara test cihazı ekleme](devtest-lab-add-devtest-user.md) |Laboratuarınıza test eden eklemek için Azure portalını kullanın.|
   | [PowerShell komut dosyası kullanarak laboratuvara test etme ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Test layıcıları laboratuvarınıza eklemeyi otomatikleştirmek için PowerShell'i kullanın. |
   | [Laboratuvara bağlantı getirin](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Test edenlerin bir köprü aracılığıyla laboratuvara doğrudan nasıl erişebileceğini öğrenin.|

7. **Daha fazla ekip için laboratuvar oluşturmayı otomatikleştirin** 
   
    Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları tekrar tekrar oluşturmak için kullanarak, özel ayarlar da dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonlarını kullanarak Azure DevTest Labs'da laboratuvarlar oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

