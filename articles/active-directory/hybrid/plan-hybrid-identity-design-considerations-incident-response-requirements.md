---
title: Karma kimlik tasarımı - olay yanıt gereksinimleri Azure | Microsoft Dokümanlar
description: Olası tehditleri belirlemek ve azaltmak için eylemlerde bulunmak üzere BT tarafından yararlanılabilen karma kimlik çözümü için izleme ve raporlama yeteneklerini belirlemek
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109287"
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için olay yanıt gereksinimlerini belirleme
Büyük veya orta ölçekli kuruluşlar büyük olasılıkla BT'nin olay düzeyine göre harekete geçtirene yardımcı olmak için bir [güvenlik olayı yanıtına](https://technet.microsoft.com/library/cc700825.aspx) sahip olacaktır. Kimlik yönetim sistemi, hedefe karşı belirli bir eylemi kimin gerçekleştirdiğini belirlemeye yardımcı olmak için kullanılabildiği için olay yanıtlama sürecinde önemli bir bileşendir. Karma kimlik çözümü, olası bir tehdidi belirlemek ve azaltmak için eylemlerde bulunmak için BT tarafından yararlanılabilen izleme ve raporlama yetenekleri sağlayabilmelidir. Tipik bir olay yanıt planında planın bir parçası olarak aşağıdaki aşamalara sahip olursunuz:

1. İlk değerlendirme.
2. Olay iletişimi.
3. Hasar kontrolü ve risk azaltma.
4. Ne kadar uzlaşma cılız ve şiddet olduğunu niçin tanımlama.
5. Kanıt ların korunması.
6. İlgili taraflara bildirim.
7. Sistem kurtarma.
8. Belge.
9. Hasar ve maliyet değerlendirmesi.
10. Süreç ve plan revizyonu.

Ne kadar uzlaşmacı ve önem alanı olduğunun tanımlanması sırasında, gizliliği ihlal edilen sistemleri, erişilen dosyaları belirlemek ve bu dosyaların duyarlılığını belirlemek gerekecektir. Karma kimlik sisteminiz, bu değişiklikleri yapan kullanıcıyı tanımlamanıza yardımcı olmak için bu gereksinimleri yerine getirebilmelidir. 

## <a name="monitoring-and-reporting"></a>İzleme ve raporlama
Çoğu zaman kimlik sistemi, sistem denetim ve raporlama yetenekleri yerleşik ise, ilk değerlendirme aşamasında da yardımcı olabilir. İlk değerlendirme sırasında BT Yöneticisi şüpheli bir etkinliği tanımlayabilmeli veya sistem önceden yapılandırılmış bir göreve dayanarak bu etkinliği otomatik olarak tetikleyebilmeli. Birçok etkinlik olası bir saldırıyı gösterebilir, ancak diğer durumlarda, kötü yapılandırılmış bir sistem bir saldırı algılama sisteminde bir dizi yanlış pozitife yol açabilir. 

Kimlik yönetim sistemi, BT yöneticilerinin bu şüpheli etkinlikleri tanımlamasına ve bildirmesine yardımcı olmalıdır. Genellikle bu teknik gereksinimler, tüm sistemlerin izlenmesi ve olası tehditleri vurgulayabilecek bir raporlama yeteneğine sahip olarak yerine getirilebilir. Olay yanıtı gereksinimlerini göz önünde bulundurarak karma kimlik çözümünüzü tasarlamanıza yardımcı olmak için aşağıdaki soruları kullanın:

* Şirketinizin bir güvenlik olayı tepkisi var mı?
  * Evet ise, geçerli kimlik yönetim sistemi sürecin bir parçası olarak kullanılır mı?
* Şirketinizin farklı cihazlardaki kullanıcıların şüpheli oturum açma girişimlerini tanımlaması gerekiyor mu?
* Şirketinizin tehlikeye atabilecek olası kullanıcı kimlik bilgilerini algılaması gerekiyor mu?
* Şirketinizin kullanıcıerişimini ve eylemini denetlemesi gerekiyor mu?
* Şirketinizin bir kullanıcının parolasını ne zaman sıfırladığını bilmesi gerekiyor mu?

## <a name="policy-enforcement"></a>İlke zorlama
Hasar kontrolü ve risk azaltma-fazı sırasında, bir saldırının gerçek ve potansiyel etkilerini hızlı bir şekilde azaltmak önemlidir. Bu noktada yapacağınız bu eylem, küçük ve büyük bir eylem arasındaki farkı yaratabilir. Tam yanıt, kuruluşunuza ve karşılaştığınız saldırının niteliğine bağlıdır. İlk değerlendirmede bir hesabın gizliliğinin ihlal edildiği sonucuna varsa, bu hesabı engellemek için ilke uygulamanız gerekir. Bu, kimlik yönetim sisteminin yararlanılabildiği bir örnek. Devam eden bir olaya tepki vermek için politikaların nasıl uygulanacağını göz önünde bulundurarak karma kimlik çözümünüzü tasarlamanıza yardımcı olmak için aşağıdaki soruları kullanın:

* Şirketinizin, gerektiğinde kullanıcıların ağa erişimini engelleyen politikaları var mı?
  * Evet ise, mevcut çözüm benimseyeceğiniz karma kimlik yönetim sistemiyle bütünleşir mi?
* Şirketinizin karantinada olan kullanıcılar için Koşullu Erişim'i uygulaması gerekiyor mu? 

> [!NOTE]
> Her yanıtı not almayı unutmayın ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın,](plan-hybrid-identity-design-considerations-data-protection-strategy.md) mevcut seçeneklerin ve her seçeneğin avantajlarının/dezavantajlarının üzerinden geçecektir.  Bu soruları yanıtlayarak, işletme ihtiyaçlarınıza en uygun seçeneği seçecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma stratejisini tanımlama](plan-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

