---
title: Azure Active Directory karma kimlik tasarımı ile ilgili hususlar - genel bakış | Microsoft Dokümanlar
description: Karma Kimlik tasarımı konuları kılavuzuna genel bakış ve içerik haritası
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 100509c4-0b83-4207-90c8-549ba8372cf7
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7f8dd49f3668b8f68753681123a04d21edac46c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60381488"
---
# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory Karma Kimlik Tasarımı ile İlgili Dikkat Edilmesi Gerekenler
Tüketici tabanlı cihazlar kurumsal dünyayı çoğaltır ve bulut tabanlı hizmet olarak yazılım (SaaS) uygulamalarını benimsemek kolaydır. Sonuç olarak, kullanıcıların uygulama erişimlerinin iç veri merkezleri ve bulut platformları arasında denetimini sağlamak zordur.  

Microsoft’un kimlik çözümleri şirket içi ve bulut tabanlı çözümleri birleştirerek konumdan bağımsız olarak tüm kaynaklara kimlik doğrulaması ve yetkilendirme sağlamak üzere tek bir kullanıcı kimliği oluşturur. Bu kavram Hibrid Kimlik olarak bilinir. Microsoft çözümlerini kullanarak karma kimlik için farklı tasarım ve yapılandırma seçenekleri vardır ve bazı durumlarda hangi kombinasyonun kuruluşunuzun gereksinimlerini en iyi şekilde karşılaacağını belirlemek zor olabilir. 

Bu Karma Kimlik Tasarımı Konuları Kılavuzu, kuruluşunuz için iş ve teknoloji gereksinimlerine en uygun karma kimlik çözümünüzü nasıl tasarlayacağınızı anlamanıza yardımcı olur.  Bu kılavuz, kuruluşunuzun benzersiz gereksinimlerini karşılayan karma kimlik çözümü tasarlamanıza yardımcı olmak için izleyebileceğiniz bir dizi adımı ve görevi ayrıntılarıyla anlatır. Kılavuz, adımlar ve görevler boyunca, işlevsel ve hizmet kalitesini (kullanılabilirlik, ölçeklenebilirlik, performans, yönetilebilirlik ve güvenlik gibi) karşılamak üzere kuruluşların kullanabileceği ilgili teknolojileri ve özellik seçeneklerini sunacak Gereksinim -leri. 

Özellikle, hibrid kimlik tasarımı konuları kılavuz hedefleri aşağıdaki soruları cevaplamak için vardır: 

* Gereksinimlerimi en iyi karşılayan bir teknoloji veya sorunlu etki alanı için hibrit kimliğe özgü bir tasarım sağlamak için hangi soruları sormam ve yanıtlamam gerekir?
* Teknoloji veya sorunlu etki alanı için karma kimlik çözümü tasarlamak için hangi etkinlik dizisini tamamlamalıyım? 
* Gereksinimlerimi karşılamama yardımcı olacak hangi karma kimlik teknolojisi ve yapılandırma seçenekleri mevcuttur? İşim için en iyi seçeneği seçebilmek için bu seçenekler arasındaki dengeler nelerdir?

## <a name="who-is-this-guide-intended-for"></a>Bu kılavuz kime yöneliktir?
 CIO, CITO, Baş Kimlik Mimarlar, Kurumsal Mimarlar ve BT Mimarlar orta veya büyük kuruluşlar için bir hibrid kimlik çözümü tasarımı ndan sorumludur.

## <a name="how-can-this-guide-help-you"></a>Bu kılavuz size nasıl yardımcı olabilir?
Bulut tabanlı kimlik yönetim sistemini mevcut şirket içi kimlik çözümünüzle tümleştirebilen karma kimlik çözümünüzü niçin tasarlayacağınızı anlamak için bu kılavuzu kullanabilirsiniz. 

Aşağıdaki grafik, BT Yöneticilerinin kullanıcıların Tek Oturum Açma (Tek Oturum Açma) kullanabilmeleri için şirket içinde bulunan mevcut Windows Server Active Directory çözümlerini Microsoft Azure Active Directory ile entegre etmeyi başarmasını sağlayan karma kimlik çözümlerini örnek olarak gösterir( SSO) bulutta ve şirket içinde bulunan uygulamalar arasında.

![Örnek](media/plan-hybrid-identity-design-considerations/hybridID-example.png)

Yukarıdaki örnek, son kullanıcı kimlik doğrulama işlemine tek bir deneyim sağlamak ve BT'nin bunları yönetmesini kolaylaştırmak için şirket içi özelliklerle tümleştirmek için bulut hizmetlerinden yararlanan karma kimlik çözümüne bir örnektir. Kaynak. Bu örnek yaygın bir senaryo olsa da, her kuruluşun karma kimlik tasarımı, farklı gereksinimler nedeniyle Şekil 1'de gösterilen örnekten farklı olabilir. 

Bu kılavuz, kuruluşunuzun benzersiz gereksinimlerini karşılayan karma kimlik çözümü tasarlamak için izleyebileceğiniz bir dizi adım ve görev sağlar. Aşağıdaki adımlar ve görevler boyunca kılavuz, kuruluşunuz için işlevsel ve hizmet kalitesi düzeyi gereksinimlerini karşılamak için kullanabileceğiniz ilgili teknolojileri ve özellik seçeneklerini sunar.

**Varsayımlar**: Windows Server, Active Directory Domain Services ve Azure Active Directory ile ilgili bazı deneyiminiz vardır. Bu belgede, bu çözümlerin iş ihtiyaçlarınızı kendi başlarına veya entegre bir çözümle nasıl karşılayabildiğini aradığınız varsayılır.

## <a name="design-considerations-overview"></a>Tasarım konularına genel bakış
Bu belge, gereksinimlerinizi en iyi karşılayan karma kimlik çözümü tasarlamak için izleyebileceğiniz adımlar ve görevler kümesi sağlar. Adımlar sıralı halde verilmiştir. Ancak, daha sonraki adımlarda öğrendiğiniz tasarım konuları, çakışan tasarım seçenekleri nedeniyle önceki adımlarda aldığınız kararları değiştirmenizi gerektirebilir. Belge boyunca olası tasarım çakışmaları konusunda sizi uyarmak için her girişimde bulunular. 

Yalnızca gerekli adımları belgedeki tüm hususları birleştirmek için gerekli olduğu kadar çok kez yineledikten sonra gereksinimlerinizi en iyi şekilde karşılayan tasarıma ulaşabilirsiniz. 

| Karma Kimlik Aşaması | Konu Listesi |
| --- | --- |
| Kimlik gereksinimlerini belirleme |[İş gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-business-needs.md)<br> [Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Karma kimlik benimseme stratejisi tanımlayın](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Güçlü kimlik çözümü ile veri güvenliğini artırma planı |[Veri koruma gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [İçerik yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Erişim denetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Hibrit kimlik yaşam döngüsünü planlama |[Karma kimlik yönetimi görevlerini belirleme](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Senkronizasyon Yönetimi](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Karma kimlik yönetimi benimseme stratejisini belirleme](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |

## <a name="next-steps"></a>Sonraki Adımlar
[Kimlik gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-business-needs.md)

