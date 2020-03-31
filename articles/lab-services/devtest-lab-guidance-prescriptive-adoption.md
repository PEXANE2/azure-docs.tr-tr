---
title: Kuruluşunuz için Azure DevTest Labs'ı benimseyin
description: Bu makalede, kuruluşunuzda Azure DevTest Labs kullanmak için açıklayıcı yönergeler sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 32e828f420d41d7a67e4f41ac8a4a0698711ce2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64916399"
---
# <a name="devtest-labs-in-the-enterprise"></a>Kuruluşta DevTest Labs
İşletmeler çeviklik, esneklik ve ekonomi gibi [avantajlar](/azure/architecture/cloud-adoption/business-strategy/cloud-migration-business-case) nedeniyle bulutu hızla benimsiyor. İlk adımlar genellikle geliştirme ve test iş yükleridir. Azure DevTest Labs, kuruluşa yarar sağlayan ve [anahtar geliştirme/test senaryolarını](devtest-lab-guidance-get-started.md)destekleyen [özellikler](devtest-lab-concepts.md) sağlar.

İş yüklerini buluta geçiren işletmeler için sık karşılaşılan endişeler şunlardır:

- [Geliştirme/test kaynaklarının güvenliğini sağlama](devtest-lab-guidance-governance-policy-compliance.md)
- [Maliyetleri yönetme ve anlama](devtest-lab-guidance-governance-cost-ownership.md)
- Kurumsal güvenlik ve uyumluluködün vermeden geliştiriciler için self servis etkinleştirme
- DevTest Laboratuvarlarını ek senaryoları kapsayacak şekilde otomatikleştirmek ve genişletmek
- [Binlerce kaynağa DevTest Labs tabanlı çözüm ölçekleme](devtest-lab-guidance-scale.md)
- [DevTest Labs'ın büyük ölçekli dağıtımları](devtest-lab-guidance-orchestrate-implementation.md)
- [Konseptin kanıtıyla başlamak](devtest-lab-guidance-orchestrate-implementation.md)

## <a name="intended-audience"></a>Hedef kitle
Bu dokümantasyon, dağıtımların oluşturulması ve gözden geçirilmesi ve operasyonların denetlenmesiyle sorumlu kurumsal BT planlamacıları, mimarlar ve yöneticiler içindir. Bu makaleler genel süreci ve önerilen tasarım ilkelerini vurgular. Amaç, azure devtest labs'ın bir kuruluş içinde benimsenmesini sağlayan güvenli ve istikrarlı bir geliştirme/test ortamını teşvik etmektir.

## <a name="enterprise-customers"></a>Kurumsal müşteriler

Birçok mevcut DevTest Labs kurumsal müşteri başarıyla geliştirme ve kuruluşlarında iş yüklerini test etmek için DevTest Labs kullanın. [Daha fazla bilgi edinin](https://azure.microsoft.com/case-studies/?term=DevTest+labs).

## <a name="next-steps"></a>Sonraki adımlar
- [Bir kuruluş için referans mimarisi](devtest-lab-reference-architecture.md)
