---
title: Karma kimlik tasarımı - dizin eşitleme gereksinimleri Azure | Microsoft Dokümanlar
description: Kuruluş için çevrimiçi şirket ve bulut arasındaki tüm kullanıcıları eşitlemek için hangi gereksinimlerin gerekli olduğunu belirleyin.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 593eaa71-17eb-4c16-8c98-43cc62987e65
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21558c4eccf0cd1f4e9e1d630f0e89dbb6f01c51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60381170"
---
# <a name="determine-directory-synchronization-requirements"></a>Dizin eşitleme gereksinimlerini belirleme
Eşitleme, kullanıcılara şirket içi kimliklerine dayalı olarak bulutta bir kimlik sağlamakla ilgilidir. Kimlik doğrulama veya federe kimlik doğrulama için senkronize hesabı kullanıp kullanmayacakları, kullanıcıların bulutta bir kimliğe sahip olmaları gerekir.  Bu kimliğin düzenli olarak korunması ve güncellenmesi gerekir.  Güncelleştirmeler, başlık değişikliklerinden parola değişikliklerine kadar birçok biçim alabilir.  

Kuruluşları şirket içi kimlik çözümleme ve kullanıcı gereksinimlerini değerlendirerek başlayın. Bu değerlendirme, kullanıcı kimliklerinin bulutta nasıl oluşturulacağı ve korunacağı yla ilgili teknik gereksinimleri tanımlamak için önemlidir.  Kuruluşların çoğunluğu için Active Directory şirket içidir ve kullanıcıların senkronize edilerek eşitleedecekleri şirket içi dizini olacaktır, ancak bazı durumlarda durum böyle olmayacaktır.  

Aşağıdaki soruları yanıtladıklısa mutlaka:

* Bir AD orman, birden fazla veya hiç var mı?
  
  * Kaç Azure REKLAM dizininle eşitleceksiniz?
    
    1. Filtreleme mi kullanıyorsunuz?
    2. Birden fazla Azure AD Connect sunucunuz var mı?
* Şu anda şirket içinde bir senkronizasyon aracınız var mı?
  
  * Evet ise, kullanıcılarınız kimliklerin sanal bir dizin /entegrasyonu varsa, kullanıcılarınız mı?
* Senkronize etmek istediğiniz başka bir dizin (örneğin LDAP Dizini, İk veritabanı, vb.) var mı?
  * GalSync yapacak mısın?
  * Kuruluşunuzdaki UPN'lerin geçerli durumu nedir? 
  * Kullanıcıların karşı kimlik doğrulaması yaptığı farklı bir dizin var mı?
  * Şirketiniz Microsoft Exchange kullanıyor mu?
    * Melez bir değişim dağıtımı yapmayı mı planlıyorlar?

Artık eşitleme gereksinimleriniz hakkında bir fikriniz olduğuna göre, bu gereksinimleri karşılamak için hangi aracın doğru aracı olduğunu belirlemeniz gerekir.  Microsoft dizin tümleştirme ve eşitleme gerçekleştirmek için çeşitli araçlar sağlar.  Daha fazla bilgi için [Karma Kimlik dizin tümleştirme araçları karşılaştırma tablosuna](plan-hybrid-identity-design-considerations-tools-comparison.md) bakın. 

Artık senkronizasyon gereksinimlerinize ve şirketiniz için bunu gerçekleştirecek araca sahip olduğunuza göre, bu dizin hizmetlerini kullanan uygulamaları değerlendirmeniz gerekir. Bu değerlendirme, bu uygulamaları buluta entegre etmek için teknik gereksinimleri tanımlamak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Bu uygulamalar buluta taşınacak ve dizini kullanacak mı?
* Bu uygulamaların bunları başarıyla kullanabilmesi için buluta eşitedilmesi gereken özel özellikler var mı?
* Bulut auth yararlanmak için bu uygulamaların yeniden yazılması gerekir mi?
* Kullanıcılar bulut kimliğini kullanarak bu uygulamalara erişirken bu uygulamalar şirket içinde yaşamaya devam edecek mi?

Ayrıca güvenlik gereksinimleri ve kısıtlamalar dizin eşitleme belirlemeniz gerekir. Bu değerlendirme, kullanıcının kimliklerini bulutta oluşturmak ve korumak için gereken gereksinimlerin bir listesini almak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Eşitleme sunucusu nerede bulunacak?
* Etki alanı birleştirilir mi?
* Sunucu, DMZ gibi bir güvenlik duvarının arkasındaki kısıtlı bir ağda mı bulunacak?
  * Eşitlemayı desteklemek için gerekli güvenlik duvarı bağlantı noktalarını açabilecek misiniz?
* Senkronizasyon sunucusu için bir olağanüstü durum kurtarma planınız var mı?
* Senkronize etmek istediğiniz tüm ormanlar için doğru izinlere sahip bir hesabınız var mı?
  * Şirketiniz bu sorunun cevabını bilmiyorsa, [makaledeki](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) "Parola eşitleme izinleri" bölümünü inceleyin ve bu izinlere sahip bir hesabınız olup olmadığını veya bir hesap oluşturmanız gerekip gerekmediğini belirleyin.
* Mutli-forest eşitleme varsa eşitleme sunucusu her ormana ulaşmak mümkün?

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıt gereksinimlerini belirleyin,](plan-hybrid-identity-design-considerations-incident-response-requirements.md) kullanılabilir seçeneklerin üzerinden geçer. Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Çok faktörlü kimlik doğrulama gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

