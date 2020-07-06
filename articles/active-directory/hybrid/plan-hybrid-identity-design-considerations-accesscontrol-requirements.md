---
title: Karma kimlik tasarımı erişim denetimi gereksinimleri Azure | Microsoft Docs
description: ', Kimlik ve Karma ortamda kullanıcıların kaynakları için erişim gereksinimlerini tanımlayan ve kimlik ve erişim gereksinimlerini ele alır.'
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60295152"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için erişim denetimi gereksinimlerini belirleme
Bir kuruluş, hibrit kimlik çözümünü tasarlarken, bu fırsatı kullanıcılara hazır hale getirmek için plantıkları kaynakların erişim gereksinimlerini gözden geçirmek için de kullanabilirler. Veri erişimi, her dört ve daha fazla kimlik:

* Yönetim
* Kimlik Doğrulaması
* Yetkilendirme
* Denetim

Aşağıdaki bölümlerde, daha fazla ayrıntı, yönetim ve denetim, karma kimlik yaşam döngüsünün bir parçası olan kimlik doğrulama ve yetkilendirme ele alınacaktır. Bu yetenekler hakkında daha fazla bilgi için [karma kimlik yönetimi görevlerini](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) okuyun.

> [!NOTE]
> Bu anların her biri hakkında daha fazla bilgi için [karma BT 'Nin yaş çağına göre kimlik-kimlik yönetimi 'Nin dört](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) bir listesini okuyun.
> 
> 

## <a name="authentication-and-authorization"></a>Kimlik doğrulaması ve yetkilendirme
Kimlik doğrulama ve yetkilendirme için farklı senaryolar vardır. Bu senaryolar, şirketin benimseeceği karma kimlik çözümü tarafından yerine getirilmesi gereken belirli gereksinimlere sahip olacaktır. Işletmeler arası (B2B) iletişimiyle ilgili senaryolar, kuruluş tarafından kullanılan kimlik doğrulama ve yetkilendirme yönteminin iş ortaklarıyla iletişim kurabildiğinden emin olmaları gerekeceğinden, BT yöneticileri için ek bir zorluk ekleyebilir. Kimlik doğrulama ve yetkilendirme gereksinimleri için tasarlama işlemi sırasında aşağıdaki soruların yanıtlanmasıyla emin olun:

* Kuruluşunuz yalnızca kendi kimlik yönetimi sistemlerinde bulunan kullanıcıların kimliğini doğrular ve yetkilendirsin mi?
  * B2B senaryolarına yönelik herhangi bir plan var mı?
  * Yanıt Evet ise, her iki işletmeye de bağlanmak için hangi protokollerin (SAML, OAuth, Kerberos veya sertifika) kullanılacağını zaten biliyoruz musunuz?
* Benimsemek istediğiniz karma kimlik çözümü bu protokolleri destekliyor mu?

Dikkate alınması gereken diğer önemli nokta, kullanıcılar ve iş ortakları tarafından kullanılacak kimlik doğrulama deposunun bulunacağı ve kullanılacak yönetim modelinin bulunduğu yerdir. Aşağıdaki iki temel seçeneği göz önünde bulundurun:

* Merkezi: Bu modelde, kullanıcının kimlik bilgileri, ilkeleri ve yönetimi şirket içinde veya bulutta merkezi hale getirilmiş olabilir.
* Karma: Bu modelde, kullanıcının kimlik bilgileri, ilkeleri ve Yönetimi Merkezi şirket içinde ve bulutta yinelenmiş olacaktır.

Kuruluşunuzun benimseeceği model, iş gereksinimlerine göre farklılık gösterir. kimlik yönetimi sisteminin nerede olduğunu ve kullanılacak yönetim modunu belirlemek için aşağıdaki soruları cevaplamak istersiniz:

* Kuruluşunuz Şu anda şirket içi bir kimlik yönetimine sahip mi?
  * Yanıt Evet ise, korumayı planlıyor musunuz?
  * Kuruluşunuzun izlemesi gereken, kimlik yönetimi sisteminin nerede olması gerektiğini belirleyen bir düzenleme veya uyumluluk gereksinimi var mı?
* Kuruluşunuz şirket içinde veya bulutta bulunan uygulamalar için çoklu oturum açma kullanıyor mu?
  * Yanıt Evet ise, karma kimlik modelinin benimsenmesi bu işlemi etkiler mi?

## <a name="access-control"></a>Erişim Denetimi
Kimlik doğrulama ve yetkilendirme, kullanıcının doğrulaması aracılığıyla şirket verilerine erişimi etkinleştirmek için temel öğelerdir, ancak bu kullanıcıların sahip olacağı erişim düzeyini ve erişim yöneticilerinin yönettikleri kaynakların üzerinde sahip olacağı erişim düzeyini denetlemek de önemlidir. Karma kimlik çözümünüz, kaynaklara, temsilciye ve rol tabanlı erişim denetimine ayrıntılı erişim sağlayabilmelidir. Erişim denetimiyle ilgili olarak aşağıdaki sorunun yanıtlandığına emin olun:

* Şirketiniz, kimlik sisteminizi yönetmek için yükseltilmiş ayrıcalığa sahip birden fazla kullanıcıya sahip mi?
  * Yanıt Evet ise, her kullanıcının aynı erişim düzeyine ihtiyacı var mı?
* Şirketinizin belirli kaynakları yönetmek için kullanıcılara erişim yetkisi verme gereksinimi var mı?
  * Yanıt Evet ise ne sıklıkla olur?
* Şirketinizin şirket içi ve bulut kaynakları arasında erişim denetimi yeteneklerini tümleştirmeleri gerekir mi?
* Şirketinizin kaynaklara erişimi bazı koşullara göre sınırlandırmaları gerekiyor mu?
* Şirketinizin bazı kaynaklara özel denetim erişimi olması gereken herhangi bir uygulaması mı var?
  * Yanıt Evet ise (Şirket içi veya bulutta) Bu uygulamalar nerede bulunur?
  * Yanıt Evet ise (Şirket içi veya bulutta) Bu hedef kaynaklar nerede bulunur?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md) , kullanılabilir seçenekler ve her seçeneğin olumlu/olumsuz yönleri üzerinden alınacaktır.  Bu soruları yanıtlayarak, iş gereksinimlerinize en uygun seçeneği seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

