---
title: Karma kimlik tasarımı erişim denetimi gereksinimleri Azure | Microsoft Dokümanlar
description: Kimlik sütunlarını kapsar ve karma ortamdaki kullanıcılar için kaynakların erişim gereksinimlerini tanımlar.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84b786a1701892823554a83fa2015ac88d6eff4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60295152"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için erişim denetimi gereksinimlerini belirleme
Bir kuruluş karma kimlik çözümlerini tasarlarken, kullanıcılar için kullanılabilir hale getirmek için planladıkları kaynakların erişim gereksinimlerini gözden geçirmek için de bu fırsatı kullanabilir. Veri erişimi, kimliğin dört sütunundan da geçer:

* Yönetim
* Kimlik doğrulaması
* Yetkilendirme
* Denetim

İzleyen bölümler, daha ayrıntılı olarak kimlik doğrulama ve yetkilendirmeyi, yönetim ve denetimi kapsayacak şekilde karma kimlik yaşam döngüsünün bir parçasıdır. Bu özellikler hakkında daha fazla bilgi için [karma kimlik yönetimi görevlerini belirleyin'i](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) okuyun.

> [!NOTE]
> Bu sütunların her biri hakkında daha fazla bilgi için [Hibrit BT Çağında Kimlik Yönetimi Dört Pillars](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) okuyun.
> 
> 

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Kimlik doğrulama ve yetkilendirme için farklı senaryolar vardır, bu senaryolar şirketin benimseyeceği karma kimlik çözümü tarafından yerine getirilmesi gereken belirli gereksinimlere sahip olacaktır. İş'ten İşletmeye (B2B) iletişimini içeren senaryolar, kuruluş tarafından kullanılan kimlik doğrulama ve yetkilendirme yönteminin iş ortaklarıyla iletişim kurabilmesinden emin olmaları gerektiğinden BT Yöneticileri için ekstra bir zorluk ekleyebilir. Kimlik doğrulama ve yetkilendirme gereksinimleri için tasarım işlemi sırasında, aşağıdaki soruların yanıtlanmasını sağlayın:

* Kuruluşunuz yalnızca kimlik yönetim sisteminde bulunan kullanıcıları doğrulayacak ve yetkilendirecek mi?
  * B2B senaryoları için herhangi bir plan var mı?
  * Evet ise, her iki işletmeyi de bağlamak için hangi protokollerin (SAML, OAuth, Kerberos veya Sertifikalar) kullanılacağını biliyor musunuz?
* Benimseyeceğiniz melez kimlik çözümü bu protokolleri destekliyor mu?

Göz önünde bulundurulması gereken bir diğer önemli nokta, kullanıcılar ve ortaklar tarafından kullanılacak kimlik doğrulama deposunun nerede bulunacağı ve kullanılacak yönetim modelidir. Aşağıdaki iki temel seçeneği göz önünde bulundurun:

* Merkezileştirilmiş: Bu modelde, kullanıcının kimlik bilgileri, ilkeleri ve yönetimi şirket içinde veya bulutta merkezileştirilebilir.
* Karma: Bu modelde, kullanıcının kimlik bilgileri, ilkeleri ve yönetimi şirket içinde merkezileştirilecek ve bulutta çoğaltılacaktır.

Kuruluşunuzun benimseyeceği model iş gereksinimlerine göre değişir, kimlik yönetim sisteminin nerede olacağını ve kullanılacak yönetim modunu belirlemek için aşağıdaki soruları yanıtlamak istersiniz:

* Kuruluşunuzun şu anda şirket içinde bir kimlik yönetimi var mı?
  * Eğer evetse, saklamayı mı planlıyorlar?
  * Kuruluşunuzun izlemesi gereken ve kimlik yönetim sisteminin nerede ikamet etmesi gerektiğini belirleyen herhangi bir düzenleme veya uyumluluk gereksinimi var mı?
* Kuruluşunuz şirket içinde veya bulutta bulunan uygulamalar için tek oturum açma kullanıyor mu?
  * Evet ise, karma kimlik modelinin benimsenmesi bu süreci etkiler mi?

## <a name="access-control"></a>Erişim Denetimi
Kimlik doğrulama ve yetkilendirme, kullanıcının doğrulaması yoluyla kurumsal verilere erişimi etkinleştirmek için temel unsurlar olmakla birlikte, bu kullanıcıların sahip olacağı erişim düzeyini ve yöneticilerin kaynaklar üzerinde sahip olacağı erişim düzeyini denetlemek de önemlidir. onlar yönetiyor. Karma kimlik çözümünüzün kaynaklara, temsilciliğe ve rol tabanı erişim denetimine parçalı erişim sağlayabilmelidir. Erişim denetimiyle ilgili aşağıdaki sorunun yanıtlandığından emin olun:

* Şirketinizin kimlik sisteminizi yönetmek için yüksek ayrıcalığa sahip birden fazla kullanıcısı var mı?
  * Evet ise, her kullanıcıaynı erişim düzeyine mi ihtiyaç duyar?
* Şirketinizin belirli kaynakları yönetmek için kullanıcılara erişimi devretmesi gerekir mi?
  * Evet ise, bu ne sıklıkta olur?
* Şirketinizin erişim denetimi özelliklerini şirket içi ve bulut kaynakları arasında entegre etmesi gerekir mi?
* Şirketinizin kaynaklara erişimi bazı koşullara göre sınırlaması gerekir mi?
* Şirketinizin bazı kaynaklara özel denetim erişimi gerektiren herhangi bir uygulaması var mı?
  * Evet ise, bu uygulamalar nerede bulunur (şirket içinde veya bulutta)?
  * Evet ise, bu hedef kaynaklar nerede bulunur (şirket içinde veya bulutta)?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri Koruma Stratejisini Tanımla,](plan-hybrid-identity-design-considerations-data-protection-strategy.md) mevcut seçeneklerin ve her seçeneğin avantajlarının/dezavantajlarının üzerinden geçecektir.  Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

