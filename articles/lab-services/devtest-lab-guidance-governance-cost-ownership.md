---
title: Azure DevTest Labs'da maliyet ve sahip olma maliyetini ve sahipliği yönetme
description: Bu makalede, maliyet için optimize ve çevrenizde mülkiyet hizalamak yardımcı olan bilgiler sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: b82d338f85f1b43712296ac7f27bdad55f8f1919
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74561672"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---manage-cost-and-ownership"></a>Azure DevTest Labs altyapısının yönetimi - Maliyet ve sahipolma
Geliştirme ve test ortamlarınızı oluşturmayı düşündüğünüzde maliyet ve sahip olma birincil kaygılardır. Bu bölümde, maliyet için optimizasyon ve çevrenizdeki sahipliği hizalamanıza yardımcı olan bilgiler bulabilirsiniz.

## <a name="optimize-for-cost"></a>Maliyet için optimize edin

### <a name="question"></a>Soru
DevTest Labs ortamımda maliyet için nasıl optimizasyon yapabilirim?

### <a name="answer"></a>Yanıt
DevTest Labs'ın maliyet için optimizasyon yaptığınız bir dizi yerleşik özelliği vardır. Kullanıcılarınızın faaliyetlerini sınırlamak için [maliyet yönetimi, eşikler](devtest-lab-configure-cost-management.md) [ve ilkeler](devtest-lab-set-lab-policy.md) makalelerine bakın. 

Geliştirme ve test iş yükleri için DevTest Labs'ı kullanırken, Kurumsal Sözleşmenizin bir parçası olarak [Kurumsal Geliştirme/Test Abonelik Avantajı'nı](https://azure.microsoft.com/offers/ms-azr-0148p/)kullanmayı düşünebilirsiniz. Alternatif olarak, müşteriye gittiğinizde Bir Ödeme'yseniz, [Gidergibi Öde DevTest teklifini](https://azure.microsoft.com/offers/ms-azr-0023p/)göz önünde bulundurabilirsiniz.

Bu yaklaşım size sayısız avantaj sağlar:

- Windows sanal makinelerde, bulut hizmetlerinde, HDInsight'ta, Uygulama Hizmeti'nde ve Logic Apps'ta özel düşük Geliştirme/Test oranları
- Diğer Azure hizmetlerinde Büyük Kurumsal Sözleşme (EA) oranları
- Windows 8.1 ve Windows 10 dahil olmak üzere Galeri'deki özel Geliştirme/Test görüntülerine erişim
 
Yalnızca etkin Visual Studio aboneleri (standart abonelikler, yıllık bulut abonelikleri ve aylık bulut abonelikleri) kurumsal Geliştirme/Test aboneliği nde çalışan Azure kaynaklarını kullanabilir. Ancak, son kullanıcılar geri bildirim sağlamak veya kabul testleri gerçekleştirmek için uygulamaya erişebilir. Bu abonelik içindeki kaynakların kullanımı, uygulama geliştirme ve test etme ile sınırlıdır ve çalışma süresi garantisi sunulmaz.

DevTest teklifini kullanmaya karar verirseniz, bu avantajın yalnızca uygulamalarınızı geliştirme ve test etmek için olduğunu unutmayın. Abonelik teki kullanım, Azure DevOps ve HockeyApp kullanımı dışında mali destekli bir SLA taşımaz.

## <a name="define-a-role-based-access-across-your-organization"></a>Kuruluşunuz genelinde rol tabanlı bir erişim tanımlama
### <a name="question"></a>Soru
Geliştiriciler/test işlerini yaparken BT'nin yönetebilmesini sağlamak için DevTest Labs ortamlarım için rol tabanlı erişim denetimini nasıl tanımlayabilirim? 

### <a name="answer"></a>Yanıt
Geniş bir desen vardır, ancak ayrıntı kuruluşunuza bağlıdır.

Merkezi BT yalnızca gerekli olana sahip olmalı ve proje ve uygulama ekiplerinin gerekli denetim düzeyine sahip olmasını sağlamalıdır. Genellikle, merkezi BT aboneliğin sahibi ve ağ yapılandırmaları gibi temel BT işlevlerini işlediği anlamına gelir. Abonelik **sahipleri** kümesi küçük olmalıdır. Bu sahipler, ihtiyaç olduğunda ek sahipler atayabilir veya abonelik düzeyi ilkeleri uygulayabilir, örneğin "Genel IP Yok".

Bir abonelik genelinde erişim gerektiren tier1 veya Tier 2 desteği gibi bir kullanıcı alt kümesi olabilir. Bu durumda, kaynakları yönetebilmeleri için bu kullanıcılara **katkıda bulunanlara** erişim sağlamanızı, ancak kullanıcı erişimi sağlamamanızı veya ilkeleri ayarlamamanızı öneririz.

DevTest Labs kaynağı, proje/uygulama ekibine yakın olan sahiplere ait olmalıdır. Bunun nedeni, makinelerin ve gerekli yazılımların gereksinimlerini anlamalarıdır. Çoğu kuruluşta, bu DevTest Labs kaynağının sahibi genellikle proje/geliştirme lideridir. Bu sahip, laboratuvar ortamındaki kullanıcıları ve ilkeleri yönetebilir ve DevTest Labs ortamındaki tüm VM'leri yönetebilir.

Proje/uygulama ekibi üyeleri DevTest Labs Kullanıcıları rolüne eklenmelidir. Bu kullanıcılar sanal makineler oluşturabilir (laboratuvar ve abonelik düzeyindeki ilkelerle uyumlu). Ayrıca kendi sanal makinelerini de yönetebilirler. Diğer kullanıcılara ait sanal makineleri yönetemezler.

Daha fazla bilgi için Azure [kurumsal iskelesi – önceden yazılmış abonelik yönetim](/azure/architecture/cloud-adoption/appendix/azure-scaffold) belgelerine bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Şirket politikası ve uyumluluk.](devtest-lab-guidance-governance-policy-compliance.md)
