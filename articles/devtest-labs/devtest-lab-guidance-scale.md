---
title: Azure DevTest Labs altyapınızı büyütme
description: Bu makalede Azure DevTest Labs altyapınızı ölçeklendirmeye yönelik rehberlik sunulmaktadır.
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
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84899217"
---
# <a name="scale-up-your-azure-devtest-labs-infrastructure"></a>Azure DevTest Labs altyapınızı büyütme
Kuruluş ölçeğinde DevTest Labs 'i uygulamadan önce, birkaç temel karar noktası vardır. Bu karar noktalarını yüksek düzeyde anlamak, bir kuruluşun gelecekte tasarım kararları almasına yardımcı olur. Ancak, bu noktaların bir kuruluşun bir kavram kanıtı başlatmalarından geri tutmamalıdır. İlk ölçek artırma planlaması için en üstteki üç alan şunlardır:

- Ağ ve güvenlik
- Abonelik topolojisi
- Roller ve sorumluluklar

## <a name="networking-and-security"></a>Ağ ve güvenlik
Ağ ve güvenlik tüm kuruluşlar için temel taşlardır. Kurumsal çapta bir dağıtım daha derin bir Analize ihtiyaç duyurken, kavram kanıtı 'nı başarılı bir şekilde gerçekleştirmek için az sayıda gereksinim vardır. Odağın birkaç temel alanı şunlardır:

- **Azure aboneliği** – DevTest Labs 'i dağıtmak için kaynak oluşturmak üzere uygun haklara sahip bir Azure aboneliğine erişiminizin olması gerekir. Kurumsal Anlaşma ve kullandıkça öde gibi Azure aboneliklerine erişim kazanmak için çeşitli yollar vardır. Azure aboneliğine erişim elde etme hakkında daha fazla bilgi için bkz. [kuruluş Için Azure 'U lisanslama](https://azure.microsoft.com/pricing/enterprise-agreement/).
- Şirket **içi kaynaklara erişim** – bazı kuruluşlar, DevTest Labs içindeki kaynakların şirket içi kaynaklara erişimine sahip olmasını gerektirir. Şirket içi ortamınızdan Azure 'a güvenli bir bağlantı kurulması gerekir. Bu nedenle, başlamadan önce bir VPN veya Express Route bağlantısı ayarlamanız veya yapılandırmanız önemlidir. Daha fazla bilgi için bkz. [sanal ağlara genel bakış](../virtual-network/virtual-networks-overview.md).
- **Ek güvenlik gereksinimleri** : makine ilkeleri, genel IP adreslerine erişim, internet 'e bağlanma gibi diğer güvenlik gereksinimleri, kavram kanıtı uygulanmadan önce incelenmesi gerekebilecek senaryolardır. 

## <a name="subscription-topology"></a>Abonelik topolojisi
Abonelik topolojisi, DevTest Labs kuruluşa dağıtıldığında önemli bir tasarım konusudur. Ancak, bir kavram kanıtı tamamlanana kadar tüm kararların kullanılması gerekli değildir. Kurumsal uygulama için gereken aboneliklerin sayısını değerlendirirken iki aşırı uç vardır: 

- Kuruluşun tamamı için bir abonelik
- Kullanıcı başına abonelik

Sonra, her yaklaşımın profesyonelleri vurgularız.

### <a name="one-subscription"></a>Bir abonelik
Genellikle, bir aboneliğin yaklaşımı büyük bir kuruluşta yönetilemez. Ancak, abonelik sayısını kısıtlamak aşağıdaki avantajları sağlar:

- Kurumsal maliyet **Tahmini** .  Tüm kaynaklar tek bir havuzda olduğundan, tek bir abonelikte bütçeleme çok daha kolay hale gelir. Bu yaklaşım, bir fatura döngüsündeki belirli bir zamanda maliyet denetim ölçülerinin ne zaman çalıştırılabileceğine ilişkin daha basit bir karar sağlar.
- Birçok abonelik üzerinde güncelleştirme yapma aksine, tüm güncelleştirmeler yalnızca bir abonelikte gerektiğinden VM 'Ler, yapıtlar, formüller, ağ yapılandırması, izinler, ilkeler vb. **yönetilebilirlik** daha kolay.
- **Ağ** çabası, şirket içi bağlantının gereksinim olduğu kuruluşlar için tek bir abonelikte büyük ölçüde basitleştirilmiştir. Ek yapılandırma, yönetim, IP adresi alanları vb. gerektiren ek abonelikler ile sanal ağları abonelikler arasında bağlama (hub-ışınsal modeli) gereklidir.
- **Ekip işbirliği** , herkes aynı abonelikte çalışırken daha kolay hale gelir. Örneğin, bir VM 'yi bir ortak çalışana yeniden atamak, ekip kaynaklarını paylaşmak ve vb.

### <a name="subscription-per-user"></a>Kullanıcı başına abonelik
Kullanıcı başına ayrı bir abonelik, alternatif bir spekte eşit fırsat sağlar. Birçok aboneliğe sahip olmanın avantajları şunlardır:

- **Azure ölçeklendirme kotaları** , benimsemeyi engeller. Örneğin, Azure bu yazma itibariyle, abonelik başına 200 depolama hesabına izin verir. Azure 'da çoğu hizmet için işlemsel kotalar vardır (birçoğu özelleştirilebilir, bazıları yapılamaz). Kullanıcı başına bir aboneliğin bu modelinde, en fazla kotaların ulaştığı çok önemli ölçüde düşüktür. Geçerli Azure ölçeklendirme kotaları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Gruplara veya tek tek geliştiricilere geri **dönme** olanağı, kuruluşların geçerli modellerini kullanarak maliyetleri hesaba getirmelerine olanak tanımak çok daha kolay hale gelir.
- DevTest Labs ortamlarının **sahip & izinleri** basittir. Geliştiricilere abonelik düzeyinde erişim verirsiniz ve ağ yapılandırması, Laboratuvar ilkeleri ve VM yönetimi dahil her şeyden önce %100 sorumludur.

Kuruluşta, SPI 'nin aşırı uç üzerinde yeterli kısıtlama olabilir. Bu nedenle, abonelikleri bu aşırı uç ortasında kalan bir şekilde ayarlamanız gerekebilir. En iyi uygulama olarak, bir kuruluşun hedefi, toplam abonelik sayısını artıran zorlamak için en az sayıda aboneliğin kullanılması gerekir. Yeniden yinelemek için, abonelik topolojisi bir DevTest Labs kurumsal dağıtımı için kritiktir, ancak kavram kanıtı 'nı geciktirmemelidir. Kuruluşta abonelik ve laboratuvar ayrıntı düzeyi hakkında nasıl karar vereceğiniz konusunda [idare](devtest-lab-guidance-governance-policy-compliance.md) makalesinde ek ayrıntılar vardır.

## <a name="roles-and-responsibilities"></a>Roller ve sorumluluklar
DevTest Labs kavram kanıtı, tanımlı sorumluluklara sahip üç birincil role sahiptir: abonelik sahibi, DevTest Labs Owner, DevTest Labs kullanıcısı ve isteğe bağlı olarak katkıda bulunan.

- **Abonelik sahibi** : aboneliğin sahibi, kullanıcı atama, ilkeleri yönetme, ağ topolojisini yönetme & oluşturma, kota artışı isteği vb. dahil olmak üzere bir Azure aboneliğini yönetme haklarına sahiptir. Daha fazla bilgi için [Bu makaleye](../role-based-access-control/rbac-and-directory-admin-roles.md)bakın.
- **DevTest Labs Owner** – DevTest Labs sahibinin laboratuvara tam yönetim erişimi vardır. Bu kişi, Kullanıcı ekleme/kaldırma, maliyet ayarlarını yönetme, genel laboratuvar ayarları ve diğer VM/yapıt tabanlı görevleri sağlamaktan sorumludur. Laboratuvar sahibi ayrıca bir DevTest Labs kullanıcısının tüm haklarına sahiptir.
- **DevTest Labs kullanıcısı** – DevTest Labs kullanıcısı laboratuarda sanal makineler oluşturabilir ve kullanabilir. Bu bireyler, oluşturdukları VM 'lerde bazı en az yönetim özelliğine sahiptir (sanal makinelerini Başlat/Durdur/Sil/Yapılandır). Kullanıcılar diğer kullanıcıların sanal makinelerini yönetemez.

## <a name="next-steps"></a>Sonraki adımlar
Bu serinin sonraki makalesine bakın: [Azure DevTest Labs uygulanmasını](devtest-lab-guidance-orchestrate-implementation.md) düzenleme