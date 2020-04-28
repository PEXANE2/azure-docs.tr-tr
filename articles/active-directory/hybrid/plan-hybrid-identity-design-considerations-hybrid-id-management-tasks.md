---
title: Karma kimlik tasarımı-yönetim görevleri Azure | Microsoft Docs
description: Koşullu erişim denetimi ile, kullanıcının kimliğini doğrularken ve uygulamaya erişim izni vermeden önce seçtiğiniz belirli koşulları denetler Azure Active Directory. Bu koşullar karşılandığında, kullanıcının kimliği doğrulanır ve uygulamaya erişim izni verilir.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 65f80aea-0426-4072-83e1-faf5b76df034
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a829d39ff21a1abeafd3b4362747894d196d9d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67109377"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Karma kimlik yaşam döngüsünü planlayın
Kimlik, kurumsal mobilite ve uygulama erişim stratejinizin temel ilerinden biridir. Mobil cihazınızda veya SaaS uygulamanızda oturum açıp etmeksizin, kimliğiniz her şeye erişim kazanmanız gereken anahtardır. En yüksek düzeyde, bir kimlik yönetimi çözümü, kaynak sağlama işlemini otomatik hale getirme ve oluşturma sürecini merkezileştirme dahil olmak üzere kimlik havuzlarınız arasında ifet ve eşitlemeyi kapsar. Kimlik çözümü, şirket içinde ve bulutta merkezi bir kimlik olmalıdır ve ayrıca merkezi kimlik doğrulamasını sürdürmek ve dış kullanıcılar ve işletmeler ile güvenli bir şekilde paylaşmak ve işbirliği yapmak için bir dizi Kimlik Federasyonu kullanır. Kaynaklar, işletim sistemlerinden ve uygulamalardan gelen kişilere veya bir kuruluşa bağlı olarak değişir. Kuruluş yapısı, sağlama ilkelerine ve yordamlarına uyum sağlayacak şekilde değiştirilebilir.

Ayrıca, kullanıcılarınızı üretken tutmaya yönelik self servis deneyimleri sunarak kullanıcılarınıza güç sağlamak için gereken bir kimlik çözümü olması da önemlidir. Kimlik çözümünüz, erişmesi gereken tüm kaynaklardaki kullanıcılar için çoklu oturum açma imkanı sağladığından daha sağlam hale gelir. Tüm düzeylerde Yöneticiler, Kullanıcı kimlik bilgilerini yönetmek için standartlaştırılmış yordamlar kullanabilir. Bazı yönetim düzeyleri, sağlama yönetimi çözümünün enine ' ına bağlı olarak azaltılabilir veya ortadan kaldırılabilir. Ayrıca, çeşitli kuruluşlar arasında el ile veya otomatik olarak yönetim özellikleri dağıtabilirsiniz. Örneğin, bir etki alanı Yöneticisi yalnızca ilgili etki alanındaki kişileri ve kaynakları kullanabilir. Bu kullanıcı yönetim ve sağlama görevleri gerçekleştirebilir, ancak iş akışları oluşturma gibi yapılandırma görevlerini yapmak için yetkilendirilmemiş.

## <a name="determine-hybrid-identity-management-tasks"></a>Karma kimlik yönetimi görevlerini belirleme
Kuruluşunuzda yönetim görevlerinin dağıtılması, yönetimin doğruluğunu ve verimliliğini artırır ve bir kuruluşun iş yükünün bakiyesini geliştirir. Güçlü bir kimlik yönetimi sistemini tanımlayan özetler aşağıda verilmiştir.

 ![kimlik yönetimi konuları](./media/plan-hybrid-identity-design-considerations/Identity_management_considerations.png)

Karma kimlik yönetimi görevlerini tanımlamak için, kuruluşun karma kimliği benimseme bazı önemli özelliklerini anlamanız gerekir. Kimlik kaynakları için kullanılan geçerli depoların anlaşılması önemlidir. Bu temel öğeleri bilerek temel gereksinimleri elde edersiniz ve size, kimlik çözümünüz için daha iyi bir tasarım kararı veren daha ayrıntılı sorular sormanız gerekecektir.  

Bu gereksinimleri tanımlarken, en azından aşağıdaki sorulara yanıt verildiğinden emin olun

* Sağlama seçenekleri: 
  
  * Karma kimlik çözümü sağlam bir hesap erişim yönetimi ve sağlama sistemini destekliyor mu?
  * Kullanıcılar, gruplar ve parolalar nasıl yönetilir?
  * Kimlik yaşam döngüsü yönetimi yanıt veriyor mu? 
    * Parola güncelleştirmeleri hesabını askıya alma ne kadar sürer?
* Lisans Yönetimi: 
  
  * Karma kimlik çözümü lisans yönetimini işliyor mu?
    * Yanıt Evet ise, hangi özellikler mevcuttur?
  * Çözüm grup tabanlı lisans yönetimini mi işleyecek? 
  
    * Yanıt Evet ise, buna bir güvenlik grubu atamak mümkün midir? 
    * Yanıt Evet ise, bulut dizini grubun tüm üyelerine otomatik olarak lisans atayacaktır mi? 
    * Bir Kullanıcı daha sonra gruba eklendiyse veya gruptan kaldırılırsa ne olur? bir lisans otomatik olarak atanır veya uygun şekilde kaldırılır. 
* Diğer üçüncü taraf kimlik sağlayıcılarıyla tümleştirme:
  * Bu karma çözüm, çoklu oturum açma uygulamak için üçüncü taraf kimlik sağlayıcılarıyla tümleştirilebilir mi?
  * Tüm farklı kimlik sağlayıcılarını birlikte bulunan bir kimlik sistemine birleştirmek mümkün mü?
  * Yanıtınız evet ise nasıl ve ne olacak ve hangi özellikler mevcuttur?

## <a name="synchronization-management"></a>Eşitleme yönetimi
Tüm kimlik sağlayıcılarını getirebilmek ve bunları eşitlenmiş halde tutmak için bir Identity Manager hedeflerinden biri. Verileri bir yetkili ana kimlik sağlayıcısına göre eşitlenmiş halde tutabilirsiniz. Bir karma kimlik senaryosunda, eşitlenmiş bir yönetim modeliyle tüm Kullanıcı ve cihaz kimliklerini şirket içi sunucuda yönetebilir ve hesapları ve isteğe bağlı olarak parolaları buluta eşitleyebilirsiniz. Kullanıcı, bulutta olduğu gibi şirket içinde aynı parolayı girer ve oturum açma sırasında parola kimlik çözümü tarafından doğrulanır. Bu model bir dizin eşitleme aracı kullanır.

![uygun tasarıma](./media/plan-hybrid-identity-design-considerations/Directory_synchronization.png) dizin eşitlemesi karma kimlik çözümünüzün eşitlenmesi aşağıdaki soruların yanıtlanmasına dikkat edin:
*    Karma kimlik çözümü için kullanılabilen eşitleme çözümleri nelerdir?
*    Çoklu oturum açma özellikleri kullanılabilir mi?
*    B2B ve B2C arasında kimlik Federasyonu seçenekleri nelerdir?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimi benimseme stratejisini belirleme](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

