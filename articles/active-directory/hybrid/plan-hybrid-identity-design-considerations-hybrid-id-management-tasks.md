---
title: Karma kimlik tasarımı - yönetim görevleri Azure | Microsoft Dokümanlar
description: Koşullu Erişim denetimi ile Azure Etkin Dizin, kullanıcının kimliğini doğrularken ve uygulamaya erişime izin vermeden önce seçtiğiniz belirli koşulları denetler. Bu koşullar yerine getirildiğinde, kullanıcı nın kimliği doğrulanır ve uygulamaya erişime izin verilir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109377"
---
# <a name="plan-for-hybrid-identity-lifecycle"></a>Hibrit Kimlik Yaşam Döngüsü Planı
Kimlik, kurumsal mobilitenin ve uygulamaya erişim stratejinizin temellerinden biridir. İster mobil cihazınızda ister SaaS uygulamanızda oturum açın, kimliğiniz her şeye erişmeniz için anahtardır. En üst düzeyde, kimlik yönetimi çözümü, kaynakları sağlama işlemini otomatikleştirmeyi ve merkezileştirmeyi içeren kimlik depolarınız arasında birleştirici ve eşitleme yi kapsar. Kimlik çözümü şirket içi ve bulut genelinde merkezi bir kimlik olmalı ve aynı zamanda merkezi kimlik doğrulamayı sürdürmek ve dış kullanıcılar ve işletmelerle güvenli bir şekilde paylaşmak ve işbirliği yapmak için bir tür kimlik federasyonu kullanmalıdır. Kaynaklar işletim sistemlerinden uygulamalardan bir kuruluştaki veya kuruluşa bağlı kişilere kadar değişir. Kuruluş yapısı, sağlama politikaları ve prosedürlerine uyacak şekilde değiştirilebilir.

Ayrıca, kullanıcılarınızı üretken tutmak için self servis deneyimleri sunarak güçlendirecek bir kimlik çözümüne sahip olmak da önemlidir. Kimlik çözümünüz, kullanıcılara erişmeleri gereken tüm kaynaklarda tek oturum açma olanağı sağlıyorsa daha sağlamdır. Tüm düzeylerdeki yöneticiler, kullanıcı kimlik bilgilerini yönetmek için standart yordamlar kullanabilir. Bazı yönetim düzeyleri, tedarik yönetimi çözümünün genişliğine bağlı olarak azaltılabilir veya ortadan kaldırılabilir. Ayrıca, yönetim yeteneklerini çeşitli kuruluşlar arasında el ile veya otomatik olarak güvenli bir şekilde dağıtabilirsiniz. Örneğin, bir etki alanı yöneticisi yalnızca bu etki alanında ki kişilere ve kaynaklara hizmet verebilir. Bu kullanıcı yönetim ve sağlama görevleri yapabilir, ancak iş akışları oluşturma gibi yapılandırma görevleri yapmaya yetkili değildir.

## <a name="determine-hybrid-identity-management-tasks"></a>Karma Kimlik Yönetimi Görevlerini Belirleme
Kuruluşunuzda yönetim görevlerini dağıtmak, yönetimin doğruluğunu ve etkinliğini artırır ve bir kuruluşun iş yükü dengesini geliştirir. Aşağıda sağlam bir kimlik yönetim sistemi tanımlayan pivotlar vepivotlar yer alıyor.

 ![kimlik yönetimi hususları](./media/plan-hybrid-identity-design-considerations/Identity_management_considerations.png)

Karma kimlik yönetimi görevlerini tanımlamak için, karma kimliği benimseyecek olan kuruluşun bazı temel özelliklerini anlamanız gerekir. Kimlik kaynakları için kullanılan mevcut depoları anlamak önemlidir. Bu temel öğeleri bilerek, temel gereksinimleri olacak ve size Kimlik çözümü için daha iyi bir tasarım kararı götürecek daha ayrıntılı sorular sormak gerekir dayalı.  

Bu gereksinimleri tanımlarken, en azından aşağıdaki soruların yanıtlanmasını sağlayın

* Sağlama seçenekleri: 
  
  * Karma kimlik çözümü sağlam bir hesap erişim yönetimi ve sağlama sistemini destekliyor mu?
  * Kullanıcılar, gruplar ve parolalar nasıl yönetilecek?
  * Kimlik yaşam döngüsü yönetimi yanıt veriyor mu? 
    * Parola güncellemeleri hesabının askıya alınması ne kadar sürer?
* Lisans yönetimi: 
  
  * Karma kimlik çözümü lisans yönetimini işliyor mu?
    * Evet ise, hangi yetenekler kullanılabilir?
  * Çözüm grup tabanlı lisans yönetimini işliyor mu? 
  
    * Evet ise, buna bir güvenlik grubu atamak mümkün mü? 
    * Evet ise, bulut yönetmeni grubun tüm üyelerine otomatik olarak lisans atar mı? 
    * Bir kullanıcı daha sonra gruba eklenirse veya gruptan kaldırılırsa, bir lisans uygun şekilde otomatik olarak atanacak veya kaldırılırsa ne olur? 
* Diğer üçüncü taraf kimlik sağlayıcılarıyla tümleştirme:
  * Bu karma çözüm, tek oturum açma uygulamak için üçüncü taraf kimlik sağlayıcılarıyla entegre edilebilir mi?
  * Tüm farklı kimlik sağlayıcılarını uyumlu bir kimlik sisteminde birletmek mümkün mü?
  * Evet ise, nasıl ve hangileri ve hangi yetenekleri mevcuttur?

## <a name="synchronization-management"></a>Senkronizasyon Yönetimi
Bir kimlik yöneticisinin amaçlarından biri, tüm kimlik sağlayıcılarını getirebilmek ve senkronize tutmak. Yetkili bir ana kimlik sağlayıcısına göre verileri eşitlenmiş tutarsınız. Karma kimlik senaryosunda, senkronize bir yönetim modeliyle, şirket içi sunucudaki tüm kullanıcı ve aygıt kimliklerini yönetir ve hesapları ve isteğe bağlı olarak bulutparolalarını eşitlersiniz. Kullanıcı, bulutta olduğu gibi şirket içine aynı parolayı girer ve oturum açma sırasında parola kimlik çözümü tarafından doğrulanır. Bu model bir dizin eşitleme aracı kullanır.

![dizin](./media/plan-hybrid-identity-design-considerations/Directory_synchronization.png) eşitlemi Uygun tasarım için karma kimlik çözümünüzün senkronizasyonu aşağıdaki soruların yanıtlanmasını sağlar:
*    Karma kimlik çözümü için kullanılabilir eşitleme çözümleri nelerdir?
*    Mevcut yeteneklere ilişkin tek işaret nedir?
*    B2B ve B2C arasındaki kimlik federasyonu seçenekleri nelerdir?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik yönetimi benimseme stratejisini belirleme](plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

