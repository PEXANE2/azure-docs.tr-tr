---
title: Geliştiriciler için Azure DevTest Labs'ı kullanın | Microsoft Dokümanlar
description: Geliştirici gereksinimlerini karşılamak için kullanılabilecek Azure DevTest Labs özellikleri ve bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımlar hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760156"
---
# <a name="use-azure-devtest-labs-for-developers"></a>Geliştiriciler için Azure DevTest Labs'ı kullanma
Azure DevTest Labs, çok sayıda önemli senaryoyu uygulamak için kullanılabilir ancak başlıca senaryolardan biri, geliştiriciler için geliştirme makinelerini barındırmak üzere DevTest Labs kullanmayı içerir. Bu senaryoda DevTest Labs şu avantajları sağlar:

- Geliştiriciler, geliştirme makinelerini talep üzerine hızlı bir şekilde sağlayabilirler.
- Geliştiriciler, geliştirme makinelerini gerekli olan her durumda özelleştirebilir.
- Yöneticiler maliyetleri şu şekilde kontrol edebilirler:
  - Geliştiriciler geliştirme için gerekenden daha fazla VM alamaz.
  - VM'ler kullanılmadığında kapatılır. 

![Eğitim için DevTest Laboratuvarlarını kullanın](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

Bu makalede, geliştirici gereksinimlerini karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri ve bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımlar hakkında bilgi edinebilirsiniz.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Azure DevTest Labs ile geliştirici ortamları uygulama
1. **Laboratuvarı oluşturun** 
   
    Laboratuvarlar, Azure DevTest Labs'ın başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, laboratuvara kullanıcı (geliştiriciler) ekleme, maliyetleri denetlemek için ilkeler ayarlama, hızlı bir şekilde oluşturabilen VM görüntülerini tanımlama ve daha fazlası gibi görevleri gerçekleştirebilirsiniz.  
   
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
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini nasıl beyaz listeye alabildiğinizi ve yalnızca geliştiriciler için istediğiniz görüntüleri seçebileceğinizi öğrenin.|
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Geliştiricilerin özel görüntüyü kullanarak hızlı bir şekilde bir VM oluşturabilmeleri için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun.|
   | [Resim fabrikası hakkında bilgi edinin](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bir resim fabrikasının nasıl kurulduğuve kullanılacağını açıklayan bir video izleyin.|

3. **Geliştirici makineleri için yeniden kullanılabilir şablonlar oluşturma** 
   
    Azure DevTest Labs'daki formül, VM oluşturmak için kullanılan varsayılan özellik değerlerinin listesidir. Bir görüntü, VM boyutu (CPU ve RAM kombinasyonu) ve sanal ağ seçerek laboratuvarda bir formül oluşturabilirsiniz. Her geliştirici formülü laboratuvarda görebilir ve bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [VM'ler oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntüyü, VM boyutunu (CPU ve RAM kombinasyonu) ve sanal ağ alarak nasıl bir formül oluşturabileceğinizi öğrenin.|

4. **Esnek VM özelleştirmesini sağlamak için yapı oluşturma**

   Yapılar, bir VM sağlandıktan sonra uygulamanızı dağıtmak ve yapılandırmak için kullanılır. Eserler olabilir:

   - Aracılar, Kemancı ve Visual Studio gibi VM'ye yüklemek istediğiniz araçlar.
   - VM'de çalıştırmak istediğiniz eylemler - örneğin bir repo klonlama.
   - Test etmek istediğiniz uygulamalar.

   Birçok eser zaten kullanıma hazır. Özel gereksinimleriniz için daha fazla özelleştirme istiyorsanız, kendi özel yapılarınızı oluşturabilirsiniz.

   Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [DevTest Labs VM'iniz için özel yapılar oluşturun](devtest-lab-artifact-author.md) |Laboratuarınızdaki sanal makineler için kendi özel yapılarınızı oluşturun.|
   | [Azure DevTest Labs'da kullanılmak üzere özel yapıları ve Azure Kaynak Yöneticisi şablonlarını depolamak için Git deposu ekleme](devtest-lab-add-artifact-repo.md) |Özel yapılarınızı kendi özel Git repo'nuzda nasıl depolayacağız öğrenin.|

5. **Kontrol maliyetleri**
   
    Azure DevTest Labs, laboratuvardaki bir geliştirici tarafından oluşturulabilecek maksimum VM sayısını belirtmek için laboratuvarda bir ilke belirlemenize olanak tanır. 
   
    Geliştirici ekibinizin belirli bir çalışma zamanlaması varsa ve tüm VM'leri günün belirli bir saatinde durdurmak ve ertesi gün otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik kapatma ve otomatik başlatma ilkeleri ayarlayarak bunu kolayca gerçekleştirebilirsiniz. 
   
    Son olarak, uygulama geliştirme tamamlandığında, tek bir PowerShell komut dosyası çalıştırarak tüm VM'leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvarda ilkeler ayarlayarak maliyetleri kontrol edin. |
   | [PowerShell komut dosyası kullanarak tüm laboratuvar VM'lerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Geliştirme tamamlandığında tek bir işlemdeki tüm laboratuarları silin.|

1. **VM'ye sanal ağ ekleme** 
   
    DevTest Labs, bir laboratuvar oluşturulduğunda yeni bir sanal ağ (VNET) oluşturur. Kendi VNET'inizi yapılandırdıysanız -örneğin, ExpressRoute veya siteden siteye VPN kullanarak- bu VNET'i laboratuvarınızın sanal ağ ayarlarına ekleyerek VM'ler oluştururken kullanılabilir hale getirebilirsiniz.

    Ayrıca, VM oluşturulurken bir etki alanına VM'e katılacak bir Azure Active Directory etki alanı kullanılabilir. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'da sanal ağı yapılandırma](devtest-lab-configure-vnet.md) |Azure portalını kullanarak Azure DevTest Labs'da sanal ağı nasıl yapılandırılayacağınızı öğrenin.|

6. **Laboratuarı her geliştiriciyle paylaşın**
   
    Laboratuvarlara, geliştiricilerinizle paylaştığınız bir bağlantıyı kullanarak doğrudan erişilebilir. [Microsoft hesapları](devtest-lab-faq.md#what-is-a-microsoft-account)olduğu sürece Azure hesapları olması bile gerekmez. Geliştiriciler diğer geliştiriciler tarafından oluşturulan VM'leri göremez.  
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'daki bir laboratuvara geliştirici ekleme](devtest-lab-add-devtest-user.md) |Laboratuvarınıza geliştiriciler eklemek için Azure portalını kullanın.|
   | [PowerShell komut dosyası kullanarak geliştiricileri laboratuvara ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Geliştiricileri laboratuarınıza eklemek için PowerShell'i kullanın. |
   | [Laboratuvara bağlantı getirin](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Geliştiricilerin bir köprü aracılığıyla bir laboratuvara nasıl doğrudan erişebileceğini öğrenin.|

7. **Daha fazla ekip için laboratuvar oluşturmayı otomatikleştirin** 
   
    Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları tekrar tekrar oluşturmak için kullanarak, özel ayarlar da dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonlarını kullanarak Azure DevTest Labs'da laboratuvarlar oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

