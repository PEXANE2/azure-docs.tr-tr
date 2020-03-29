---
title: Azure DevTest Labs'da talep özelliklerini kullanma | Microsoft Dokümanlar
description: Azure DevTest Labs'ın talep/haksız beyan özelliklerini kullanmak için farklı senaryolar hakkında bilgi edinin
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2019
ms.author: spelluru
ms.openlocfilehash: a15148260bccadc59966c86031100f0e0332b0f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67861436"
---
# <a name="use-claim-capabilities-in-azure-devtest-labs"></a>Azure DevTest Labs'da talep özelliklerini kullanma
Azure DevTest Labs hizmeti, geliştiricilerin ve test edenlerin etkinliğini ve verimliliğini artırır. Bu makale, Azure DevTest Labs'da sanal makineleri talep etme veya talep etme özelliğine odaklanın. Ayrıca, bu özelliğin kullanıcı deneyimini geliştirdiği çeşitli yolları da listeler. Bu özelliğin kullanılabildiği farklı senaryolara bakmadan önce, **iddianın** ne olduğuna ve nasıl çalıştığına bakalım.

## <a name="claimable-machines"></a>Talep edilebilir makineler
İddia edilebilir bir makine, sahibi olmayan bir laboratuvarda oluşturulan sanal bir makinedir (VM). Makine talep edildikten sonra, kullanıcının bu VM için tam bir seçenek yelpazesi vardır. Bir kullanıcı bir makine talep ettiğinde, birkaç değişiklik yapılır. VM, **Talep Edilebilir sanal makineler** listesinden Azure portalındaki Sanal **makinelerim** listesine taşınır. 

Kullanıcı VM'ye bağlanabilir, yapıları özelleştirebilir, makineyi yeniden başlatabilir, durdurabilir veya geri alabilir. VM'yi talep edilebilir hale getirmenin birkaç yolu vardır:

- Bir makine oluşturun ve talep edilebilir havuza taşınması için onu talep etme. 
- [Gelişmiş ayarları](https://azure.microsoft.com/updates/azure-devtest-labs-claim-lab-vms-from-a-shared-pool/)kullanarak paylaşılan havuzda bir VM oluşturun ve yerleştirin.

Talep/talep reddi yeteneklerinin etkin bir şekilde kullanılabildiği iki durum vardır. İlk durumda daha önplan ve planlama gerektirir, düzgün tasarlanmış ve yürütülecek. Ve ikincisi daha durumsal. Aşağıda farklı durumlarda bazı örnekler verilmiştir.

## <a name="designed-use-of-claimable-machines"></a>Talep edilebilir makinelerin tasarlanmış kullanımı

- **Yazılım geliştirme / test:** Yapılandırılmış makineleri hazır ve sahipsiz bir durumda yaparak geliştiricilerin veya sınayıcıların daha üretken olmasına izin verin. Farklı yapılandırmalara, gerekli araçlara ve en son koda sahip bir vm kümesine sahip olmak, kullanıcıların bir Makine kurmak için zaman harcamadan vm talep etmesine ve çalışmaya başlamasına olanak tanır. VM'ler talep edilmeden önce, makineler karşılanır, ancak daha az kullanılan makinelere sahip olmanın maliyetini en aza indirir. VM'lere ihtiyaç duyulduğunda, kullanıcı makineyi başlatan VM'yi talep ediyor. Yeni bir VM oluşturma genellikle daha kolay ve daha ucuz olduğundan, hak etme seçeneği bu durumda bu kadar yararlı değildir.
- **Sınıf/Laboratuvarlar:** Öğrencilerin Azure portalını kullanarak bir makineye hemen bağlanabilmeleri için VM'lerin bir sınıf veya laboratuvar için önceden yapılandırılmış olmasını sağlayabilir.  Bir öğrenci VM talep ettiği nde, laboratuvar kimsenin aynı makineyi talep edememesini sağlar. Bu işlemin otomatiklenmesi, belirtilen ortama sahip gerekli makine sayısının kullanılabilir olmasını sağlar. Öğrenciler gelmezse veya geç kalırsa, sahipsiz makineler minimum maliyetle oturum bitene kadar kullanılabilir durumda tutulabilir. VM önceki kullanıcı yapıldığında bilinmeyen bir durumda olduğundan, hak etme seçeneği bu senaryoda etkili değildir.
- **Gösteriler:** Laboratuvardaki makinelerin belirli ortamlarla ayarlandığı gösteriler için makineleri kullanın. Bu özellik, birden çok kişinin aynı anda veya konferans gibi rasgele zamanlarda bir gösteri veriyor olabileceği durumlarda yararlıdır. Demo makinenin durumunu değiştirmemesi gerektiği için, kullanıcıların bir sonraki gösteri için talep edilebilir havuza vm geri döndürmesine olanak sağlaması gerektiği için, hak reddi seçeneği bu durumda yararlı olabilir. Sahipsiz makinenin tedarikinin iptal edilmesi ve minimum maliyete tabi olması yla, VM'ler laboratuvarda daha uzun süreler bırakılabilir.
- **Geçici/Sözleşmeli İşçiler:** Kullanıcıların makine kullanmasına izin verin. Ayrıldıklarında, vm'yi veri kaybı olmadan talep edilebilir havuza geri verirler. VM sahipsiz, başka bir kullanıcı VM talep edebilir ve devam veya ek bilgi için makine gözden geçirebilirsiniz.
- **Genel olarak:** Tek bir kaynağın vm'leri belirli bir şekilde otomatik olarak yapılandırma ve dağıtma özelliği birçok farklı durumda yararlıdır. Talep/talep etme özelliğinin, vm'leri belirli bir yapılandırmayla sahipsiz bir durumda oluşturmak için otomatik bir işlem yaparak kullanıcıların daha verimli olmasına yardımcı olduğu birkaç farklı durum vardır. Yapılandırma(lar), gereksinimlerinize bağlı olarak farklı işletim sistemleri, diller, diskler veya [diğer yazılımları (yapılar)](devtest-lab-artifact-author.md) içerebilir. Laboratuvardan bir VM talep etme yeteneği, laboratuvar kullanıcısının makineyi yapılandırmak için zaman veya çaba harcamadan düzgün yapılandırılmış bir sistem elde etmesini sağlar. Laboratuvar yöneticisi, üretilen makine sayısını artırmak, makineleri temizlemek ve yapılandırmaların önceliğini belirlemek için VM'lerin talep edilen durumunu kullanabilir. [Görüntü fabrikası,](image-factory-create.md) birden çok laboratuvar için VM'ler ve görüntüler oluşturmak için otomatik bir sürecin iyi bir örneğidir. Komut dosyaları, uygun değişikliklerle aşağıdaki durumlardan herhangi birini yürütmek için değiştirilebilir veya özel bir sistem oluşturmak için başvuru olarak kullanılabilir.

## <a name="situational-use-of-claimable-machines"></a>Talep edilebilir makinelerin durumsal kullanımı

- Kullanıcıların makinelerin denetimini birinden diğerine geçirmesine ve makineyi kimin alacağına açıkça bilmesine gerek kalmadan talep/talep etmeme özelliğini kullanın.
- Belirli bir makine yapılandırmasının bir hatayı yeniden oluşturabileceği bir senaryonun geliştirilmesi, test edilmesi ve hata ayıklanması, makinenin sahipsiz olması ve başka bir geliştiricinin makineyi talep edip çalışmaya devam etmesine olanak sağlaması. Daha fazla insan dünyanın farklı bölgelerinde uzaktan çalışıyoruz gibi bu özellik özellikle yararlıdır. 
- Ekip üyeleri tek bir ortamla çalışabilir. Örneğin, otomatikleştirilemeyecek karmaşık bir ortamı el ile ayarlayabilir veya yalnızca görüntüler gibi tek bir giriş için değişiklikleri işleyebilen kaynaklar oluşturabilirsiniz. Geçmişte, bu sorun, özel bir makinenin çalışır hale gelip çalıştırılarak ele alınmıştır. Talep edilebilir özellik, dahili kullanıcı erişim denetimine ve kullanılabilir olduğunda görsel tanımlamaya sahip olarak el ile işlem üzerinde bir iyileştirmedir. Sahipsiz kaldığında, VM maliyetleri azaltmak için de-provisioned olduğunu.
- VM'ye bağlı bir veri diskine sahip. ~ 1 TB'a kadar olan her disk, verileri kopyalamak veya çoğaltmak zorunda kalmadan büyük miktarda verinin geçirilmesini sağlar. VM başlangıçta büyük veri hacmine sahip ekli bir diskle oluşturulur.  Herhangi bir kullanıcı daha sonra makineyi talep edebilir ve verilere erişebilir. Bittiğinde, diğer kullanıcıların makineye girmesine izin vermek için VM'yi reddedin.

İddia edilebilir makineleri kullanmak için bazı uyarılar vardır, en yaygın olarak makineye erişim elde etme etrafında. Makine etki alanı birleştirilmişse, makineye zaten erişim izni verildiğini iddia eden kullanıcının, genellikle VM oluşturulduğunda laboratuvardaki tüm kullanıcıları kapsayan bir gruba erişim izni vererek yapılması gerekir. Makine etki alanı birleştirilmiş değilse, ortak depodaki **Sıfırlama VM Parolası** nın kullanıcıyı yönetici olarak eklemek için çalıştırılması gerekir.  Eserler, makine başlatıldıktan veya talep edildikten sonra bile uygulanabilir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [Azure DevTest Labs'da talep edilebilir VM'ler oluşturun ve yönetin](devtest-lab-add-claimable-vm.md)
