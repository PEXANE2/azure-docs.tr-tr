---
title: Azure DevTest Labs'ı kullanmak için popüler senaryolar
description: Bu makalede, Azure DevTest Labs'ı kullanmak için birincil senaryolar ve kuruluşunuzdaki hizmeti kullanmaya başlamak için iki genel yol sağlanmaktadır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 8736ba4c24ac4c8f8d84345028d1cadfdef38697
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60773817"
---
# <a name="popular-scenarios-for-using-azure-devtest-labs"></a>Azure DevTest Labs'ı kullanmak için popüler senaryolar
Bir işletmenin ihtiyaçlarına bağlı olarak, DevTest Labs farklı gereksinimleri karşılayacak şekilde yapılandırılabilir.  Bu makalede, popüler senaryolar anlatılmaktadır. Her senaryo, DevTest Labs'ı ve bu senaryoları uygulamak için kullanılacak kaynakları kullanarak getirilen avantajları kapsar.  

- Geliştirici masaüstü bilgisayarlar
- Test ortamları
- Eğitim seansları, uygulamalı laboratuvarlar ve hackathons
- Kumlu soruşturmalar
- Sınıf laboratuvarları

## <a name="developer-desktops"></a>Geliştirici masaüstü bilgisayarlar
Geliştiriciler genellikle farklı projeler için geliştirme makineleri için farklı gereksinimleri vardır. DevTest Labs ile geliştiriciler, en yaygın senaryolarına uyacak şekilde yapılandırılan isteğe bağlı sanal makinelere erişebilirler. DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, ekipler arasında tutarlılık sağlayan ortak geliştirme makineleri sağlayabilir.
- Geliştiriciler, geliştirme makinelerini talep üzerine hızlı bir şekilde sağlayabilir veya [önceden yapılandırılmış mevcut bir makineyi talep](devtest-lab-add-claimable-vm.md)edebilir.
- Geliştiriciler, abonelik düzeyinde izinlere gerek kalmadan kaynakları self servis bir şekilde sağlayabilir.
- BT veya yöneticiler [ağ topolojisini önceden tanımlayabilir](devtest-lab-configure-vnet.md) ve geliştiriciler herhangi bir özel erişim gerektirmeden doğrudan basit ve sezgisel bir şekilde kullanabilirler.
- Geliştiriciler gerektiğinde geliştirme makinelerini kolayca [özelleştirebilirler.](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm)
- Yöneticiler maliyetleri şu şekilde kontrol edebilirler:
    - Geliştiriciler geliştirme için ihtiyaç duyduklarından [daha fazla VM alamaz](devtest-lab-set-lab-policy.md#set-virtual-machines-per-user)
    - [VM'ler](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanılmadığında kapatılır
    - Yalnızca belirli laboratuvarlar için [VM örnek boyutlarının bir alt kümesine izin](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes)
    - Her laboratuvar için [maliyet hedeflerini ve bildirimlerini yönetme.](devtest-lab-configure-cost-management.md)

Daha fazla okuma için geliştiriciler [için Azure DevTest Laboratuvarlarını Kullanın'a](devtest-lab-developer-lab.md)bakın. 

## <a name="test-environments"></a>Test ortamları
Bir kuruluş genelinde test ortamları oluşturmak ve yönetmek önemli bir çaba gerektirebilir. DevTest Labs ile test ortamları kolayca oluşturulabilir, güncellenebilir veya çoğaltılabilir. Ekiplerin ihtiyaç duyulduğunda tam olarak yapılandırılmış bir ortama erişmesine olanak tanır. Bu senaryoda, DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, ekipler arasında tutarlılık sağlayan ortak test ortamları sağlayabilir.
- Test edenler, yeniden kullanılabilir şablonlar kullanarak Windows ve Linux ortamlarını hızla sağlayarak uygulamalarının en son sürümünü sınayabilirler.
- Yöneticiler, DevOps senaryolarını etkinleştirmek için laboratuarı Azure DevOps'lere bağlayabilir
- Laboratuvar Sahipleri, şunları sağlayarak maliyetleri kontrol edebilir:
    - [Ortamlardaki VM'ler](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanılmadığında kapatılır
    - Yalnızca belirli [laboratuvarlar için VM örnek boyutlarının bir alt kümesine izin](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes)
    - Her laboratuvar için [maliyet hedeflerini ve bildirimlerini yönetme.](devtest-lab-configure-cost-management.md)

Daha fazla bilgi için [vm ve PaaS test ortamları için Azure DevTest Laboratuvarlarını Kullanın'a](devtest-lab-test-env.md)bakın.

## <a name="sandboxed-investigations"></a>Kumlu soruşturmalar
Geliştiriciler genellikle farklı teknolojileri veya altyapı tasarımını araştırır. Varsayılan olarak, DevTest Labs ile oluşturulan tüm ortamlar kendi kaynak grubunda oluşturulur. DevTest Labs kullanıcısı yalnızca bu kaynaklara okuma erişimi sağlar. Ancak, daha fazla denetime ihtiyaç duyan geliştiriciler için, oluşturdukları her ortam için kaynak olan DevTest Labs kullanıcısına [katkıda bulunan lara haklar](https://azure.microsoft.com/updates/azure-devtest-labs-view-and-set-access-rights-to-an-environment-rg/) vermek için laboratuvar çapında bir ayar güncellenebilir.  DevTest Labs ile geliştiricilere laboratuvarda oluşturdukları ortamlara otomatik olarak katkıda bulunma izni verilebilir.  Bu senaryo, geliştiricilerin geliştirme veya test ortamları için ihtiyaç duydukları azure kaynaklarını eklemelerine ve/veya değiştirmelerine olanak tanır. Kaynak sayfasına [göre maliyet,](devtest-lab-configure-cost-management.md#view-cost-by-resource) Laboratuvar Sahiplerinin soruşturmalar için kullanılan her ortamın maliyetini izlemesine olanak tanır.

Daha fazla bilgi için [bkz.](https://aka.ms/dtl-sandbox)

## <a name="trainings-hands-on-labs-and-hackathons"></a>Eğitimler, uygulamalı laboratuvarlar ve hackathons 
Azure DevTest Labs'daki bir laboratuvar, atölye çalışmaları, uygulamalı laboratuvarlar, eğitimler veya hackathon'lar gibi geçici etkinlikler için harika bir kapsayıcı görevi görür.  Hizmet, her kursiyerin eğitim için aynı ve yalıtılmış ortamlar oluşturmak için kullanabileceği özel şablonlar sağlayabileceğiniz bir laboratuvar oluşturmanıza olanak tanır. Bu senaryoda, DevTest Labs aşağıdaki avantajları sağlar:

- [İlkeler,](devtest-lab-set-lab-policy.md) kursiyerlerin yalnızca sanal makineler gibi ihtiyaç duydukları kaynak sayısını elde edilmesini sağlar.
- Önceden yapılandırılmış ve oluşturulan makineler, kursiyerden tek bir eylemle [talep](devtest-lab-add-claimable-vm.md) edilir.
- [Laboratuvarlar, laboratuvar URL'sine](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab)erişerek kursiyerler ile paylaşılır.
- Sanal makinelerdeki [son kullanma tarihleri,](devtest-lab-add-vm.md#steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs) artık ihtiyaç duyulmadıktan sonra makinelerin silinmesini sağlar.
- Eğitim bittiğinde bir laboratuarı ve [ilgili](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) tüm kaynakları [silmek](devtest-lab-delete-lab-vm.md#delete-a-lab) kolaydır.

Daha fazla bilgi için eğitim [için Azure DevTest Laboratuvarlarını Kullanın'a](devtest-lab-training-lab.md)bakın.  

## <a name="proof-of-concept-vs-scaled-deployment"></a>Kavram ve ölçekli dağıtım kanıtı
DevTest Labs'ı keşfetmeye karar verdikten sonra, ileriye dönük iki genel yol vardır: Kavram ve Ölçeklenmiş Dağıtım Kanıtı.  

**Ölçeklenmiş bir dağıtım,** DevTest Labs'ı yüzlerce veya binlerce geliştiricisi olan tüm kuruluşa dağıtmak amacıyla haftalarca/aylarca gözden geçirme ve planlamadan oluşur.

**Kavram dağıtımının kanıtı,** tek bir ekibin kuruluş değeri oluşturmak için yoğunbir çabasına odaklanır. Ölçeklenmiş bir dağıtım düşünmek cazip olsa da, yaklaşım kavram seçeneğinin kanıtından daha sık başarısız olma eğilimindedir. Bu nedenle, küçük başlatmanızı, ilk takımdan öğrenmenizi, aynı yaklaşımı iki veya üç ek ekiple yinelemenizi ve sonra kazanılan bilgilere dayalı ölçekli bir dağıtım planlamanızı öneririz. Başarılı bir kavram kanıtı için, bir veya iki takım seçmenizi ve senaryolarını (geliştirme ortamı vs test ortamları) belirlemenizi, geçerli kullanım durumlarını belgelemenizi ve DevTest Laboratuvarlarını dağıtmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun:

- [DevTest Labs kavramları](devtest-lab-concepts.md)
- [Devtest Labs SSS](devtest-lab-faq.md)

