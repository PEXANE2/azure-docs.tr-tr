---
title: Azure Güvenlik Merkezi Hazırlığı Yol Haritası | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde kullanılacak bir hazırlık yol haritası sağlar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: fece670cc-df70-445d-9773-b32cbaba8d4a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2018
ms.author: memildin
ms.openlocfilehash: fcaf427c2e0ab275a5a6e08306dda785bca690d6
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92784176"
---
# <a name="azure-security-center-readiness-roadmap"></a>Azure Güvenlik Merkezi hazırlığı yol haritası
Bu belge, Azure Güvenlik Merkezi ile çalışmaya başlamanıza yardımcı olacak bir hazırlık yol haritası sağlar.

## <a name="understanding-security-center"></a>Güvenlik Merkezi’ni anlama
Azure Güvenlik Merkezi, Azure’da, şirket içinde ve diğer bulutlarda çalışan iş yükleri için birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. 

Güvenlik Merkezi ile çalışmaya başlamak için aşağıdaki kaynakları kullanın.

Makaleler
- [Azure Güvenlik Merkezi'ne Giriş](security-center-introduction.md)
- [Azure Güvenlik Merkezi hızlı başlangıç kılavuzu](security-center-get-started.md)

Videolar
- [Hızlı Tanıtım Videosu](https://azure.microsoft.com/resources/videos/introduction-to-azure-security-center/)
- [Güvenlik Merkezi Önleme, Algılama ve Yanıt Özelliklerine Genel Bakış](https://azure.microsoft.com/resources/videos/azurecon-2015-new-azure-security-center-helps-you-prevent-detect-and-respond-to-threats/)

## <a name="planning-and-operations"></a>Planlama ve işlemler

Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli çalışma, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir.

Planlama ve çalışma işlemleri sırasında size yardımcı olması için aşağıdaki kaynakları kullanın.

- [Azure Güvenlik Merkezi planlama ve işlemler kılavuzu](security-center-planning-and-operations-guide.md)


### <a name="onboarding-computers-to-security-center"></a>Güvenlik Merkezi’ne bilgisayar ekleme
Güvenlik Merkezi, Azure Defender tarafından korunmayan tüm Azure aboneliklerini veya çalışma alanlarını otomatik olarak algılar. Bu, Güvenlik Merkezi Ücretsiz ve güvenlik çözümü etkinleştirilmemiş çalışma alanlarını kullanarak Azure abonelikleri içerir.

Ekleme işlemleri sırasında size yardımcı olması için aşağıdaki kaynakları kullanın.

- [Azure dışı bilgisayarlar ekleme](quickstart-onboard-machines.md)
- [Azure Güvenlik Merkezi Karma - Genel Bakış](https://youtu.be/NMa4L_M597k)

## <a name="mitigating-security-issues-using-security-center"></a>Güvenlik Merkezi'ni kullanarak güvenlik sorunlarını azaltma
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir.

Güvenlik uyarılarını yönetmenize ve kaynaklarınızı korumanıza yardımcı olması için aşağıdaki kaynakları kullanın.

Makaleler    
- [Azure Güvenlik Merkezi 'nde güvenlik durumu izleme](./security-center-monitoring.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](./security-center-network-recommendations.md)
- [Azure Güvenlik Merkezi'nde Azure SQL hizmetini ve verilerini koruma](./security-center-remediate-recommendations.md)


Video    
- [Güvenlik Merkezi'ni Kullanarak Güvenlik Sorunlarını Azaltma](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Mitigating-Security-Issues-using-Azure-Security-Center)

### <a name="security-center-for-incident-response"></a>Olay yanıtlama için Güvenlik Merkezi
Maliyetleri ve zararları azaltmak için bir saldırı gerçekleşmeden önce bir olay yanıt planının olması önemlidir. Bir olay yanıtının farklı aşamalarında Azure Güvenlik Merkezi’ni kullanabilirsiniz.

Güvenlik Merkezi’nin olay yanıtlama işleminize nasıl dahil edilebileceğini anlamak için aşağıdaki kaynakları kullanın.

Videolar    
* [Olay Yanıtlamada Azure Güvenlik Merkezi](https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response)
* [Yeni nesil güvenlik işlemi ve araştırma ile tehditlere hızlı yanıt verme](https://youtu.be/e8iFCz5RM4g)

Makaleler    
* [Olay yanıtı için Azure Güvenlik Merkezi’ni kullanma](./tutorial-security-incident.md)
* [Güvenlik Merkezi tetikleyicilerine yanıt vermek için Otomasyonu kullanma](workflow-automation.md)

## <a name="advanced-cloud-defense"></a>Gelişmiş bulut savunması

Azure VM'ler, Güvenlik Merkezi’ndeki gelişmiş bulut savunma özelliklerinden yararlanabilir. Bu yetenekler, tam zamanında sanal makine (VM) erişimi ve Uyarlamalı uygulama denetimleri içerir.

Bu özelliklerin Güvenlik Merkezi’nde nasıl kullanılacağını öğrenmek için aşağıdaki kaynakları kullanın.

Videolar    
* [Azure Güvenlik Merkezi – tam zamanında VM erişimi](https://youtu.be/UOQb2FcdQnU)
* [Azure Güvenlik Merkezi - Uyarlamalı Uygulama Denetimleri](https://youtu.be/wWWekI1Y9ck)

Makaleler    
* [Tam zamanında sanal makine erişimini yönetme](./security-center-just-in-time.md)
* [Azure Güvenlik Merkezi'ndeki Uyarlamalı Uygulama Denetimleri](./security-center-adaptive-application.md)

## <a name="hands-on-activities"></a>Uygulamalı etkinlikler

* [Güvenlik Merkezi uygulamalı laboratuvarı](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=78871abf-6f35-4aa0-840f-d801f5cdbd72)
* [Güvenlik Merkezi'nde Web Uygulaması Güvenlik Duvarı (WAF) öneri playbook’u](https://gallery.technet.microsoft.com/ASC-Playbook-Protect-38bd47ff)
* [Azure Güvenlik Merkezi Playbook: Güvenlik Uyarıları](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)

## <a name="additional-resources"></a>Ek kaynaklar
* [Güvenlik Merkezi Belgeleri Sayfası](./index.yml)
* [Güvenlik Merkezi REST API’si Belgeleri Sayfası](/previous-versions/azure/reference/mt704034(v=azure.100))
* [Azure Güvenlik Merkezi hakkında sık sorulan sorular (SSS)](./faq-general.md)
* [Güvenlik Merkezi Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/security-center/)
* [Kimlik güvenliği için en iyi uygulamalar](../security/fundamentals/identity-management-best-practices.md)
* [Ağ güvenliği için en iyi uygulamalar](../security/fundamentals/network-best-practices.md)
* [PaaS önerileri](../security/fundamentals/paas-deployments.md)
* [Uyumluluk](https://www.microsoft.com/trustcenter/compliance/due-diligence-checklist)
* [Log Analytics müşterileri artık, karma bulut iş yüklerini korumak için Azure Güvenlik Merkezi 'ni kullanabilir](/archive/blogs/msoms/oms-customers-can-now-use-azure-security-center-to-protect-their-hybrid-cloud-workloads)

## <a name="community-resources"></a>Topluluk Kaynakları

* [Güvenlik Merkezi UserVoice](https://feedback.azure.com/forums/347535-azure-security-center)
* [Soru-cevap Güvenlik Merkezi için bir sayfa&](/answers/topics/azure-security-center.html)