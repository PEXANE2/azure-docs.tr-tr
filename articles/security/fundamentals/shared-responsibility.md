---
title: Bulutta paylaşılan sorumluluk - Microsoft Azure
description: Paylaşılan sorumluluk modelini ve hangi güvenlik görevlerinin bulut sağlayıcısı tarafından işleneceğini ve hangi görevlerin sizin yeriniz tarafından işleneceğini anlayın.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: na
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2019
ms.author: terrylan
ms.openlocfilehash: 8f16105d6bda1798828bc423ec8a158d49e0cf2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72518375"
---
# <a name="shared-responsibility-in-the-cloud"></a>Bulutta paylaşılan sorumluluk

Genel bulut hizmetlerini değerlendirirken ve değerlendirirken, paylaşılan sorumluluk modelini ve bulut sağlayıcısı tarafından hangi güvenlik görevlerinin yürütüldedildiğini ve hangi görevlerin sizin yeriniz tarafından işleneceğini anlamak çok önemlidir. İş yükü sorumlulukları, iş yükünün Hizmet Olarak Yazılım (SaaS), Hizmet Olarak Platform (PaaS), Hizmet Olarak Altyapı (IaaS) veya şirket içi veri merkezinde barındırılıp barındırılmayacağına bağlı olarak değişir

## <a name="division-of-responsibility"></a>Sorumluluk bölümü
Şirket içi veri merkezinde, tüm yığın aitsiniz. Buluta geçerken bazı sorumluluklar Microsoft'a aktarılır. Aşağıdaki diyagram, yığınınızın dağıtım türüne göre microsoft ile aranızdaki sorumluluk alanlarını göstermektedir.

![Sorumluluk bölgeleri](./media/shared-responsibility/shared-responsibility.png)

Tüm bulut dağıtım türlerinde verileriniz ve kimlikleriniz size aittir. Verilerinizin ve kimliklerinizin, şirket içi kaynakların ve kontrol ettiğiniz bulut bileşenlerinin (hizmet türüne göre değişir) güvenliğini korumaksizin sorumlusunuz.

Dağıtım türüne bakılmaksızın, aşağıdaki sorumluluklar her zaman sizin yeriniztarafından korunur:

- Veri
- Uç Noktalar
- Hesap
- Erişim yönetimi

## <a name="cloud-security-advantages"></a>Bulut güvenliği avantajları
Bulut, uzun süredir devam eden bilgi güvenliği sorunlarını çözmek için önemli avantajlar sunar. Şirket içi bir ortamda, kuruluşların karşılanmamış sorumlulukları ve güvenliğe yatırım yapmak için kullanabileceği sınırlı kaynaklara sahip olması, saldırganların tüm katmanlarda güvenlik açıklarından yararlanabildiği bir ortam oluşturur.

Aşağıdaki diyagram, sınırlı kaynaklar nedeniyle birçok güvenlik sorumluluklarının karşılanmadığı geleneksel bir yaklaşım gösterir. Bulut özellikli yaklaşımda, gün içinde güvenlik sorumluluklarını bulut sağlayıcınıza kaydırabilir ve kaynaklarınızı yeniden yeniden tahsis edebilirsiniz.

![Bulut çağının güvenlik avantajları](./media/shared-responsibility/cloud-enabled-security.png)

Bulut özellikli yaklaşımda, daha fazla etkinlik için bulut tabanlı güvenlik özelliklerinden yararlanabilir ve tehdit algılama ve yanıt sürenizi artırmak için bulut istihbaratını kullanabilirsiniz. Kuruluşlar, sorumluluklarını bulut sağlayıcısına kaydırarak, güvenlik kaynaklarını ve bütçeyi diğer iş önceliklerine göre yeniden tahsis etmelerini sağlayan daha fazla güvenlik kapsamı elde edebilir.

## <a name="next-steps"></a>Sonraki adımlar
SaaS, PaaS ve IaaS dağıtımında siz ve Microsoft arasındaki sorumluluk bölümü hakkında daha fazla bilgi için bulut [bilgi işlem için Paylaşılan sorumluluklara](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)bakın.
