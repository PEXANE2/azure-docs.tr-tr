---
title: Azure DevTest Labs altyapınızı ölçeklendirin
description: Bu makalede, Azure DevTest Labs altyapınızı ölçekleme için kılavuz sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 3a48cef2210721bf7116b1c4ad1169779288f47d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75644843"
---
# <a name="scale-up-your-azure-devtest-labs-infrastructure"></a>Azure DevTest Labs altyapınızı ölçeklendirin
DevTest Labs'ı kuruluş ölçeğinde uygulamadan önce birkaç önemli karar noktası vardır. Bu karar noktalarını yüksek düzeyde anlamak, gelecekte tasarım kararları alan bir kuruluşa yardımcı olur. Ancak, bu noktalar bir kuruluşun kavram kanıtı başlatmayı geciktirmemelidir. İlk ölçeklendirme planlaması için ilk üç alan şunlardır:

- Ağ ve güvenlik
- Abonelik topolojisi
- Roller ve sorumluluklar

## <a name="networking-and-security"></a>Ağ ve güvenlik
Ağ ve güvenlik tüm kuruluşlar için temel taşıdır. Kuruluş çapında bir dağıtım çok daha derin bir analiz gerektirirken, kavram kanıtını başarılı bir şekilde gerçekleştirmek için daha az sayıda gereksinim vardır. Odak birkaç önemli alanları şunlardır:

- **Azure Aboneliği** – DevTest Laboratuvarlarını dağıtmak için kaynak oluşturmak için uygun haklara sahip bir Azure Aboneliği'ne erişiminiz olması gerekir. Kurumsal Sözleşme ve Giderolarak Öde dahil olmak üzere Azure aboneliklerine erişmenin birkaç yolu vardır. Azure Aboneliği'ne erişim hakkında daha fazla bilgi için [kuruluş için Azure Lisansı'na](https://azure.microsoft.com/pricing/enterprise-agreement/)bakın.
- **Şirket içi kaynaklara erişim** – Bazı kuruluşlar, DevTest Labs'daki kaynaklarının şirket içi kaynaklara erişebmesi gerekir. Şirket ortamınızdan Azure'a güvenli bir bağlantı gereklidir. Bu nedenle, başlamadan önce vpn veya Express Route bağlantısı ayarlamanız/yapılandırmanız önemlidir. Daha fazla bilgi için [Sanal Ağlar'a genel bakış](../virtual-network/virtual-networks-overview.md)bilgisi ne rendeleyin.
- **Ek güvenlik gereksinimleri** – Makine ilkeleri, genel IP adreslerine erişim, internete bağlanma gibi diğer güvenlik gereksinimleri, bir kavram kanıtı uygulamadan önce gözden geçirilmesi gereken senaryolardır. 

## <a name="subscription-topology"></a>Abonelik topolojisi
Abonelik Topolojisi, DevTest Laboratuvarlarını Kuruluşa dağıtırken önemli bir tasarım konusudur. Ancak, bir kavram kanıtı tamamlanana kadar tüm kararları sağlamlaştırması gerekmez. Kurumsal bir uygulama için gereken abonelik sayısını değerlendirirken, iki uç nokta vardır: 

- Tüm kuruluş için bir abonelik
- Kullanıcı başına abonelik

Daha sonra, her yaklaşımın Artılarını vurgulayacağız.

### <a name="one-subscription"></a>Tek abonelik
Genellikle bir aboneliğin yaklaşımı büyük bir kuruluşta yönetilemez. Ancak, abonelik sayısını sınırlamak aşağıdaki avantajları sağlar:

- Kuruluş için **tahmin** maliyetleri.  Tüm kaynaklar tek bir havuzda olduğundan, tek bir abonelikte bütçeleme çok daha kolay hale gelir. Bu yaklaşım, bir faturalandırma döngüsünde herhangi bir zamanda maliyet kontrol önlemlerinin ne zaman uygulanıp uygulanacakdaha basit bir karar alınmasını sağlar.
- Tüm güncelleştirmeler birçok abonelik arasında güncelleştirme ler yapmak yerine yalnızca tek bir abonelik te gerekli olduğundan, VM'lerin, yapı teşekküllerin, formüllerin, ağ yapılandırmasının, izinlerin, ilkelerin vb. **yönetilebilirliği** daha kolaydır.
- **Ağ** çalışması, şirket içi bağlantının bir gereklilik olduğu işletmeler için tek bir abonelikte büyük ölçüde basitleştirilir. Abonelikler arasında sanal ağları bağlamak (hub-spoke modeli) ek yapılandırma, yönetim, IP adres alanları, vb gerektiren ek abonelikler ile gereklidir.
- **Ekip işbirliği,** herkes aynı abonelikte çalışırken daha kolaydır – örneğin, bir iş arkadaşınıza VM'yi yeniden atamak, ekip kaynaklarını paylaşmak vb.

### <a name="subscription-per-user"></a>Kullanıcı başına abonelik
Kullanıcı başına ayrı bir abonelik, alternatif spektruma eşit fırsatlar sağlar. Birçok aboneye sahip olmanın avantajları şunlardır:

- **Azure ölçekleme kotaları** benimsenmesini engellemeyecaz. Örneğin, bu yazı itibariyle Azure abonelik başına 200 depolama hesabı sağlar. Azure'daki çoğu hizmet için operasyonel kotalar vardır (çoğu özelleştirilebilir, bazıları kullanılamaz). Kullanıcı başına abonelik bu modelde, çoğu kotaya ulaşılabiliyor olması son derece düşüktür. Geçerli Azure ölçeklendirme kotaları hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- Gruplara veya bireysel geliştiricilere **geri dönüşler,** kuruluşların geçerli modellerini kullanarak maliyetleri hesaba katmalarına olanak tanır.
- DevTest Labs ortamlarının **sahiplik & izinleri** basittir. Geliştiricilere abonelik düzeyinde erişim sağlarsınız ve ağ yapılandırması, laboratuvar ilkeleri ve VM yönetimi dahil olmak üzere her şeyden %100 sorumludurlar.

Atılgan'da, spektrumun uç noktalarında yeterince kısıtlama olabilir. Bu nedenle, abonelikleri bu uç noktaların ortasına denk gelecek şekilde ayarlamanız gerekebilir. En iyi uygulama olarak, bir kuruluşun amacı, toplam abonelik sayısını artıran zorlama işlevlerini göz önünde bulundurarak mümkün olduğunca en az sayıda abonelik kullanmak olmalıdır. Yinelemek gerekirse, abonelik topolojisi DevTest Labs'ın kurumsal dağıtımı için çok önemlidir, ancak konsept inisi geciktirmemelidir. [Kuruluştaki](devtest-lab-guidance-governance-policy-compliance.md) abonelik ve laboratuvar ayrıntılılığına nasıl karar verilebilen yönetim makalesinde ek ayrıntılar vardır.

## <a name="roles-and-responsibilities"></a>Roller ve sorumluluklar
Bir DevTest Labs kavram kanıtı tanımlı sorumlulukları ile üç birincil rolleri vardır - Abonelik sahibi, DevTest Labs sahibi, DevTest Labs kullanıcı, ve isteğe bağlı olarak bir Katılımcı.

- **Abonelik sahibi** – Abonelik sahibi, kullanıcıları atama, ilkeleri yönetme, ağ topolojisi oluşturma & yönetme, kota artışları vb. dahil olmak üzere bir Azure Aboneliği yönetme hakkına sahiptir. Daha fazla bilgi için [bu makaleye](../role-based-access-control/rbac-and-directory-admin-roles.md)bakın.
- **DevTest Labs sahibi** - DevTest Labs sahibi laboratuvara tam yönetim erişimi vardır. Bu kişi, kullanıcı ekleme/kaldırma, maliyet ayarlarını, genel laboratuvar ayarlarını ve diğer VM/artefakt tabanlı görevleri yönetmekle sorumludur. Bir laboratuvar sahibi de Bir DevTest Labs Kullanıcı tüm haklarına sahiptir.
- **DevTest Labs kullanıcı** - DevTest Labs kullanıcı oluşturmak ve laboratuvarda sanal makineleri tüketmek. Bu kişilerin oluşturdukları VM'lerde (VM'lerini başlat/durdur/silme/yapılandırma) bazı minimum yönetim yetenekleri vardır. Kullanıcılar diğer kullanıcıların VM'lerini yönetemez.

## <a name="next-steps"></a>Sonraki adımlar
Bu serinin bir sonraki makalesine bakın: [Azure DevTest Labs'ın uygulanmasını düzenleme](devtest-lab-guidance-orchestrate-implementation.md)