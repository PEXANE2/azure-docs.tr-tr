---
title: Karma kimlik tasarımı-olay yanıtı gereksinimleri Azure | Microsoft Docs
description: Olası tehditleri belirlemek ve azaltmak üzere eylemler gerçekleştirmek için yararlanılabilir tarafından kullanılabilecek karma kimlik çözümü için izleme ve raporlama özelliklerini belirleme
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
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
ms.openlocfilehash: 52b5e37c29e4b3df3f171f683266b5d0a3e0c95d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67109287"
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için olay yanıtı gereksinimlerini belirleme
Büyük veya orta ölçekli kuruluşlar, olay düzeyine göre işlem yapmanıza yardımcı olmak için bir [güvenlik olayı yanıtı](https://technet.microsoft.com/library/cc700825.aspx) oluşturacaktır. Kimlik yönetimi sistemi, hedefte belirli bir eylemi gerçekleştirmeyen kişileri tanımlamaya yardımcı olmak üzere kullanılabilir olduğundan, olay yanıtı sürecinde önemli bir bileşendir. Karma kimlik çözümü, olası bir tehdidi belirlemek ve azaltmak üzere eylemler gerçekleştirmek için yararlanılabilir olabilecek izleme ve raporlama özellikleri sağlayabilmelidir. Tipik bir olay yanıt planında, planın bir parçası olarak aşağıdaki aşamalara sahip olursunuz:

1. İlk değerlendirme.
2. Olay iletişimi.
3. Hasar denetimi ve risk azaltma.
4. Ne kadar risk ve önem derecesine sahip olduğunu tanımlama.
5. Kanıt koruma.
6. Uygun taraflara bildirim.
7. Sistem kurtarma.
8. Belgelerle.
9. Hasar ve maliyet değerlendirmesi.
10. İşleme ve plan düzeltmesi.

Tehlikeye atımı ve önem derecesine sahip olduğunu belirlemek için, tehlikeye girmiş olan sistemleri, erişilen dosyaları ve bu dosyaların duyarlılığını belirlemek için gerekli olacaktır. Karma kimlik sisteminiz, bu değişiklikleri yapan kullanıcıyı tanımlamaya yardımcı olması için bu gereksinimleri yerine getirebilmelidir. 

## <a name="monitoring-and-reporting"></a>İzleme ve raporlama
Çoğu zaman, sistemin denetim ve raporlama özelliklerine yerleşik olması durumunda kimlik sistemi ilk değerlendirme aşamasında de yardımcı olabilir. İlk değerlendirme sırasında, BT yöneticisinin şüpheli bir etkinliği belirleyebilmesi veya sistemin önceden yapılandırılmış bir göreve göre otomatik olarak tetikleyebilmesi gerekir. Birçok etkinlik olası bir saldırıyı belirtebilir, ancak başka durumlarda, hatalı yapılandırılmış bir sistem, yetkisiz giriş algılama sisteminde bir dizi hatalı pozitif sonuç oluşmasına neden olabilir. 

Kimlik yönetimi sistemi, BT yöneticilerine bu şüpheli etkinlikleri tanımlamak ve raporlamak için yardımcı olmalıdır. Genellikle bu teknik gereksinimler, tüm sistemleri izleyerek ve olası tehditleri vurgulayabileceği bir raporlama yeteneği sunarak yerine getirilir. Önemli olay yanıtı gereksinimlerini göz önünde bulundurarak karma kimlik çözümünüzü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Şirketinizin bir güvenlik olayı yanıtı var mı?
  * Yanıt Evet ise, işlemin bir parçası olarak kullanılan geçerli kimlik yönetimi sistemidir mı?
* Şirketinizin farklı cihazlarda kullanıcılardan gelen şüpheli oturum açma girişimlerini belirlemesi gerekiyor mu?
* Şirketinizin olası güvenliği aşılmış kullanıcının kimlik bilgilerini algılaması gerekiyor mu?
* Şirketinizin kullanıcının erişimini ve eylemini denetlemesi gerekiyor mu?
* Bir kullanıcının parolasını sıfırladığında şirketiniz tarafından bilinsin mi?

## <a name="policy-enforcement"></a>İlke zorlama
Hasar denetimi ve risk azaltma aşamasında, bir saldırının gerçek ve potansiyel etkileri hızla azaltılmanız önemlidir. Bu noktada yapacağınız eylem, küçük ve birincil bir nokta arasındaki farkı yapabilir. Tam yanıt kuruluşunuza ve yüzünüzü sunan saldırının doğasına göre değişir. İlk değerlendirme bir hesabın güvenliğinin aşıldığını sonuçlandırdığında, bu hesabı engellemek için ilkeyi zorunlu kılabilirsiniz. Bu, kimlik yönetimi sisteminin yararlanılabilir olacağı tek bir örnektir. Sürekli bir olaya tepki vermek üzere ilkelerin nasıl zorlanacağını göz önünde bulundurarak, karma kimlik çözümünüzü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Şirketiniz, kullanıcıların ağa erişmesini engellemek için gereken ilkelere sahip mi?
  * Yanıt Evet ise, geçerli çözüm benimsemek istediğiniz karma kimlik yönetimi sistemiyle tümleştirilmesine mi?
* Şirketinizin Karantinadaki kullanıcılar için koşullu erişimi zorlaması gerekiyor mu? 

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md) , kullanılabilir seçenekler ve her seçeneğin olumlu/olumsuz yönleri üzerinden alınacaktır.  Bu sorulara yanıt vererek, iş gereksinimlerinize en uygun seçeneği seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

