---
title: Azure DevTest Labs kullanımı için popüler senaryolar
description: Bu makalede, kuruluşunuzda hizmeti kullanmaya başlamak için Azure DevTest Labs ve iki genel yol kullanımına yönelik birincil senaryolar sağlanmaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60773817"
---
# <a name="popular-scenarios-for-using-azure-devtest-labs"></a>Azure DevTest Labs kullanımı için popüler senaryolar
Bir kuruluşun ihtiyaçlarına bağlı olarak, DevTest Labs farklı gereksinimleri karşılayacak şekilde yapılandırılabilir.  Bu makalede popüler senaryolar ele alınmaktadır. Her senaryo, bu senaryoları uygulamak için kullanılan DevTest Labs ve kaynakları kullanarak yapılan avantajları ele alır.  

- Geliştirici masaüstleri
- Test ortamları
- Eğitim oturumları, uygulamalı laboratuvarlar ve hack maratonları
- Korumalı araştırmalar
- Sınıf laboratuvarları

## <a name="developer-desktops"></a>Geliştirici masaüstleri
Geliştiriciler, farklı projelere yönelik geliştirme makineleri için genellikle farklı gereksinimlere sahiptir. DevTest Labs sayesinde, geliştiricilerin en yaygın senaryolarına uyacak şekilde yapılandırılmış isteğe bağlı sanal makinelere erişimi olabilir. DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, ekipler genelinde tutarlılık sağlayan ortak geliştirme makineleri sağlayabilir.
- Geliştiriciler geliştirme makinelerini isteğe bağlı olarak hızlı bir şekilde sağlayabilir veya [önceden yapılandırılmış mevcut bir makineyi talep](devtest-lab-add-claimable-vm.md)edebilir.
- Geliştiriciler, abonelik düzeyinde izinlere gerek duymadan kaynakları bir Self Servis yoluyla sağlayabilir.
- BT veya Yöneticiler [ağ topolojisini önceden tanımlayabilir](devtest-lab-configure-vnet.md) ve geliştiriciler özel erişim gerektirmeden basit ve sezgisel bir şekilde doğrudan kullanılabilir.
- Geliştiriciler geliştirme makinelerini gerektiği gibi kolayca [özelleştirebilir](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) .
- Yöneticiler, aşağıdakileri sağlayarak maliyetleri denetleyebilir:
    - Geliştiriciler, geliştirme için gereksiniminden [daha fazla VM alamaz](devtest-lab-set-lab-policy.md#set-virtual-machines-per-user)
    - [Sanal makineler](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanımda olmadığında kapatılır
    - Yalnızca belirli laboratuvarlara ait [VM örneği boyutlarının bir alt kümesine izin veriliyor](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes)
    - Her laboratuvar için [Maliyet hedeflerini ve bildirimleri yönetme](devtest-lab-configure-cost-management.md) .

Daha fazla bilgi için bkz. [geliştiriciler için Azure DevTest Labs kullanma](devtest-lab-developer-lab.md). 

## <a name="test-environments"></a>Test ortamları
Bir kuruluş genelinde test ortamları oluşturmak ve yönetmek için önemli bir çaba gerekebilir. DevTest Labs ile test ortamları kolayca oluşturulabilir, güncelleştirilir veya yinelenebilir. Ekiplerin gerektiğinde tam olarak yapılandırılmış bir ortama erişmesine olanak sağlar. Bu senaryoda, DevTest Labs aşağıdaki avantajları sağlar:

- Kuruluşlar, ekipler genelinde tutarlılık sağlayan ortak test ortamları sağlayabilir.
- Test edenler, yeniden kullanılabilir şablonları kullanarak Windows ve Linux ortamlarını hızlıca sağlayarak uygulamasının en son sürümünü test edebilir.
- Yöneticiler DevOps senaryolarını etkinleştirmek için Laboratuvarı Azure DevOps 'e bağlayabilirler
- Laboratuvar sahipleri şunları sağlayarak maliyetleri denetleyebilir:
    - [Ortamlarınızdaki VM 'ler](devtest-lab-set-lab-policy.md#set-auto-shutdown) kullanımda olmadığında kapatılır
    - Yalnızca belirli laboratuvarlara [AIT VM örneği boyutlarının bir alt kümesine izin veriliyor](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes)
    - Her laboratuvar için [Maliyet hedeflerini ve bildirimleri yönetme](devtest-lab-configure-cost-management.md) .

Daha fazla bilgi için bkz. [VM ve PaaS test ortamları için Azure DevTest Labs kullanma](devtest-lab-test-env.md).

## <a name="sandboxed-investigations"></a>Korumalı araştırmalar
Geliştiriciler genellikle farklı teknolojileri veya altyapı tasarımını araştırabilir. Varsayılan olarak, DevTest Labs ile oluşturulan tüm ortamlar kendi kaynak grupları içinde oluşturulur. DevTest Labs kullanıcısı bu kaynaklara yalnızca okuma erişimini alır. Ancak, daha fazla denetime ihtiyaç duyan geliştiriciler için, oluşturulan her ortam için kaynak DevTest Labs kullanıcısına [katkıda bulunan haklar](https://azure.microsoft.com/updates/azure-devtest-labs-view-and-set-access-rights-to-an-environment-rg/) vermek üzere laboratuvar genelinde bir ayar güncelleştirilebilecek.  DevTest Labs ile geliştiricilere, laboratuvarda oluşturdukları ortamlara otomatik olarak katkıda bulunan izin verilebilir.  Bu senaryo, geliştiricilerin geliştirme veya test ortamları için ihtiyaç duydukları Azure kaynaklarını eklemesini ve/veya değiştirmesini sağlar. [Kaynak maliyeti](devtest-lab-configure-cost-management.md#view-cost-by-resource) sayfası, laboratuvar sahiplerinin araştırmalar için kullanılan her bir ortamın maliyetini izlemesine olanak sağlar.

Daha fazla bilgi için bkz. [ortam kaynak grubuna erişim hakları ayarlama](https://aka.ms/dtl-sandbox).

## <a name="trainings-hands-on-labs-and-hackathons"></a>Seyahat, uygulamalı laboratuvarlar ve hack maratonları 
Azure DevTest Labs laboratuvar, atölyeler, uygulamalı laboratuvarlar, tratlar veya hack maratonları gibi geçici etkinlikler için harika bir kapsayıcı görevi görür.  Hizmet, eğitimin aynı ve yalıtılmış ortamlar oluşturmak için kullanabileceği özel şablonlar sağlayabileceğiniz bir laboratuvar oluşturmanıza olanak sağlar. Bu senaryoda, DevTest Labs aşağıdaki avantajları sağlar:

- [İlkeler](devtest-lab-set-lab-policy.md) , yalnızca ihtiyaç duydukları sanal makineler gibi kaynak sayısını almayı güvence altına alır.
- Önceden yapılandırılmış ve oluşturulan makineler, Trainee 'den tek bir eylemle [talep](devtest-lab-add-claimable-vm.md) edilir.
- Laboratuvarlar [, laboratuvarın URL 'sine](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab)erişerek traınele paylaşılır.
- Sanal makinelerdeki [süre sonu tarihleri](devtest-lab-add-vm.md#steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs) , makinelerin artık gerekli olmadıklarında silindiklerinden emin olun.
- Eğitim üzerine geldiğinde [bir laboratuvarı](devtest-lab-delete-lab-vm.md#delete-a-lab) ve [ilgili tüm kaynakları](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) kolayca silebilirsiniz.

Daha fazla bilgi için bkz. [eğitim için Azure DevTest Labs kullanma](devtest-lab-training-lab.md).  

## <a name="proof-of-concept-vs-scaled-deployment"></a>Kavram kanıtı ve ölçeklendirilmiş dağıtım karşılaştırması
DevTest Labs 'i keşfetmeye karar verdikten sonra iki genel yol vardır: kavram kanıtı ile ölçeklendirilmiş dağıtım.  

**Ölçeklenen bir dağıtım** , yüzlerce veya binlerce geliştirici içeren tüm kuruluşa DevTest Labs dağıtmaya yönelik bir amaç gözden geçirilmesi ve planlanmasından oluşur.

**Kavram kanıtı,** tek bir ekipten kurumsal değer oluşturmak için yüksek bir ekipten oluşan bir çaba üzerine yoğunlaşıyor. Ölçeklenen bir dağıtımı düşünürken yaklaşım, kavram kanıtı seçeneğinden daha sık başarısız olma eğilimi gösterir. Bu nedenle, küçük, ilk takımdan öğrenmenizi, iki ile üç ek ekiple aynı yaklaşımı tekrarlamanızı ve elde edilen bilgiye göre ölçeklenen bir dağıtım planlamanız önerilir. Kavram başarısı açısından, bir veya iki takım seçmenizi ve senaryolarını (geliştirme ortamı vs test ortamları) belirlemenizi, geçerli kullanım durumlarını belgelerinizi ve DevTest Labs dağıtmayı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun:

- [DevTest Labs kavramları](devtest-lab-concepts.md)
- [DevTest Labs SSS](devtest-lab-faq.md)

