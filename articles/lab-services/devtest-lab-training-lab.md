---
title: Eğitim için Azure DevTest Laboratuvarlarını kullanın | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs'da eğitim için bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımlar sağlanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759986"
---
# <a name="use-azure-devtest-labs-for-training"></a>Eğitim için Azure DevTest Laboratuvarlarını kullanma
Azure DevTest Labs, geliştirme/teste ek olarak birçok önemli senaryoyu uygulamak için kullanılabilir. Bu senaryolardan biri eğitim için bir laboratuvar kurmaktır. Azure DevTest Labs, her kursiyerin eğitim için aynı ve yalıtılmış ortamlar oluşturmak için kullanabileceği özel şablonlar sağlayabileceğiniz bir laboratuvar oluşturmanıza olanak tanır. Eğitim ortamlarının her kursiyer için yalnızca ihtiyaç duyduklarında ve eğitim için gerekli sanal makineler gibi yeterli kaynakları içerdiğinden emin olmak için ilkeler uygulayabilirsiniz. Son olarak, tek bir tıklamayla erişebilecekleri laboratuvarla kolayca kursiyerler arasında paylaşabilirsiniz.

![Eğitim için DevTest Laboratuvarlarını kullanın](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs, herhangi bir sanal ortamda eğitim yapmak için gereken aşağıdaki gereksinimleri karşılar: 

* Kursiyerler diğer kursiyerler tarafından oluşturulan VM'leri göremez
* Her eğitim makinesi aynı olmalıdır
* Kursiyerler eğitim ortamlarını hızlı bir şekilde sağlayabilirler
* Kursiyerlerin eğitim için ihtiyaç duyduklarından daha fazla VM alamamasını ve kullanmadıkları nda VM'leri kapatmalarını sağlayarak maliyeti kontrol edin
* Eğitim laboratuarını her kursiyerle kolayca paylaşın
* Eğitim laboratuarını tekrar tekrar yeniden kullanın

Bu makalede, daha önce açıklanan eğitim gereksinimlerini ve eğitim için bir laboratuvar kurmak için izleyebileceğiniz ayrıntılı adımları karşılamak için kullanılabilecek çeşitli Azure DevTest Labs özellikleri hakkında bilgi edinebilirsiniz.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Azure DevTest Labs ile eğitim uygulama
1. **Laboratuvarı oluşturun** 
   
    Laboratuvarlar, Azure DevTest Labs'ın başlangıç noktasıdır. Bir laboratuvar oluşturduktan sonra, kullanıcılarını (kursiyerler) laboratuvara ekleme, maliyetleri denetlemek için ilkeler ayarlama, hızlı bir şekilde oluşturabilen VM görüntülerini tanımlama ve daha fazlası gibi görevleri gerçekleştirebilirsiniz.   
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'de laboratuvar oluşturma](devtest-lab-create-lab.md) |Azure portalındaki Azure DevTest Labs'da nasıl bir laboratuvar oluşturabilirsiniz öğrenin. |
2. **Hazır pazar görüntüleri ve özel görüntüler kullanarak dakikalar içinde eğitim VM'leri oluşturun** 
   
    Azure Marketi'ndeki çok çeşitli resimlerden hazır görüntüler seçebilir ve bunları laboratuvardaki kursiyerler için kullanılabilir hale getirebilirsiniz. Hazır resimler gereksinimlerinizi karşılamazsa, Azure Marketi'nden hazır bir görüntü kullanarak bir laboratuvar VM oluşturarak, eğitim için ihtiyacınız olan tüm yazılımları yükleyerek ve VM'yi laboratuvarda özel görüntü olarak kaydederek özel bir görüntü oluşturabilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure Market görüntülerini yapılandırma](devtest-lab-configure-marketplace-images.md) |Azure Marketi görüntülerini nasıl beyaz listeye alabildiğinizi öğrenin; yalnızca eğitim için istediğiniz görüntüleri seçime sunar. |
   | [Özel görüntü oluşturma](devtest-lab-create-template.md) |Eğitim için ihtiyacınız olan yazılımı önceden yükleyerek özel bir görüntü oluşturun, böylece kursiyerler özel görüntüyü kullanarak hızlı bir şekilde VM oluşturabilirler. |
3. **Eğitim makineleri için yeniden kullanılabilir şablonlar oluşturun** 
   
    Azure DevTest Labs'daki formül, VM oluşturmak için kullanılan varsayılan özellik değerlerinin listesidir. Bir görüntü, VM boyutu (CPU ve RAM kombinasyonu) ve sanal ağ seçerek laboratuvarda bir formül oluşturabilirsiniz. Her kursiyer formülü laboratuvarda görebilir ve bir VM oluşturmak için kullanabilir. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [VM'ler oluşturmak için DevTest Labs formüllerini yönetme](devtest-lab-manage-formulas.md) |Bir görüntüyü, VM boyutunu (CPU ve RAM kombinasyonu) ve sanal ağ alarak nasıl bir formül oluşturabileceğinizi öğrenin. |
4. **Kontrol maliyetleri**
   
    Azure DevTest Labs, laboratuvardaki bir kursiyer tarafından oluşturulabilecek maksimum VM sayısını belirtmek için laboratuvarda bir ilke belirlemenize olanak tanır. 
   
    Çok günlük eğitim yapıyorsanız ve tüm VM'leri günün belirli bir saatinde durdurmak ve ertesi gün otomatik olarak yeniden başlatmak istiyorsanız, laboratuvarda otomatik kapatma ve otomatik başlatma ilkeleri ayarlayarak bunu kolayca gerçekleştirebilirsiniz. 
   
    Son olarak, eğitim tamamlandığında tek bir PowerShell komut dosyası çalıştırarak tüm VM'leri aynı anda silebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md) |Laboratuvarda ilkeler ayarlayarak maliyetleri kontrol edin. |
   | [PowerShell komut dosyası kullanarak tüm laboratuvar VM'lerini silme](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Eğitim tamamlandığında tüm laboratuarları tek bir işlemde silin. |
5. **Laboratuarı her kursiyerle paylaşın**
   
    Laboratuvarlara doğrudan kursiyerlerinizle paylaştığınız bir bağlantı dan erişilebilir. Kursiyerlerin, [Microsoft hesapları](devtest-lab-faq.md#what-is-a-microsoft-account)olduğu sürece Azure hesabı olması bile gerekmez. Kursiyerler diğer kursiyerler tarafından oluşturulan VM'leri göremezler.  
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Azure DevTest Labs'daki bir laboratuvara stajyer ekleme](devtest-lab-add-devtest-user.md) |Eğitim laboratuvarınıza kursiyer eklemek için Azure portalını kullanın. |
   | [PowerShell komut dosyası kullanarak kursiyerlerin laboratuvara eklenmesi](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |Eğitim laboratuvarınıza kursiyer eklemeyi otomatikleştirmek için PowerShell'i kullanın. |
   | [Laboratuvara bağlantı getirin](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Bir laboratuvara köprü üzerinden nasıl doğrudan erişilebileceğini öğrenin. |
6. **Laboratuarı tekrar tekrar yeniden kullanın** 
   
    Kaynak Yöneticisi şablonu oluşturup aynı laboratuvarları tekrar tekrar oluşturmak için kullanarak, özel ayarlar da dahil olmak üzere laboratuvar oluşturmayı otomatikleştirebilirsiniz. 
   
    Aşağıdaki tablodaki bağlantılara tıklayarak daha fazla bilgi edinin:
   
   | Görev | Öğrenecekleriniz |
   | --- | --- |
   | [Kaynak Yöneticisi şablonu kullanarak laboratuvar oluşturma](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Kaynak Yöneticisi şablonlarını kullanarak Azure DevTest Labs'da laboratuvarlar oluşturun. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

