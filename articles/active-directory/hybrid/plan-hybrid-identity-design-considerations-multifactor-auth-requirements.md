---
title: Karma kimlik tasarımı-Multi-Factor Authentication gereksinimleri Azure | Microsoft Docs
description: Koşullu erişim denetimi ile, Azure AD, kullanıcının kimliğini doğrularken ve uygulamaya erişim izni vermeden önce seçtiğiniz belirli koşulları doğrular.
documentationcenter: ''
services: active-directory
author: billmath
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
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
ms.openlocfilehash: 7d8ddf372e234bab242e4b28ba53dce7dd68cc89
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976052"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için Multi-Factor Authentication gereksinimlerini belirleme
Bu Mobility dünyasında, bulutta ve herhangi bir cihazdan veri ve uygulamalara erişen kullanıcılar, bu bilgilerin güvenliğini sağlamak, Paramount hale geldi.  Her gün güvenlik ihlali hakkında yeni bir başlık bulunur.  Bu tür ihlaller, çok faktörlü kimlik doğrulamasıyla karşı garanti yoktur, ancak bu ihlaller önlemeye yardımcı olmak için ek bir güvenlik katmanı sağlar.
Multi-Factor Authentication için kuruluş gereksinimlerini değerlendirerek başlatın. Diğer bir deyişle, kuruluş güvenli hale getirmeye çalışıyor.  Bu değerlendirme, kuruluşların kullanıcılarını çok faktörlü kimlik doğrulaması için ayarlamaya ve etkinleştirmeye yönelik teknik gereksinimlerin tanımlanması açısından önemlidir.

Aşağıdakileri cevaplayın:

* Şirketiniz Microsoft uygulamalarını güvenli hale getirmeye çalışıyor mu? 
* Bu uygulamalar nasıl yayımlanır?
* Şirketiniz, çalışanların şirket içi uygulamalara erişmesine izin vermek için uzaktan erişim sağlıyor mu?

Yanıt Evet ise, ne tür uzaktan erişim? Ayrıca, bu uygulamalara erişen kullanıcıların nerede olacağını da değerlendirmelisiniz. Bu değerlendirme, uygun Multi-Factor Authentication stratejisini tanımlamaya yönelik başka bir önemli adımdır. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Kullanıcılar nerede bulunur?
* Her yerde konumlandırılabilir mi?
* Şirketiniz kullanıcının konumuna göre kısıtlamalar oluşturmak istiyor mu?

Bu gereksinimleri anladıktan sonra, kullanıcının Multi-Factor Authentication gereksinimlerini de değerlendirmek önemlidir. Bu değerlendirme, çok faktörlü kimlik doğrulamasının alınması için gereksinimleri tanımlayacağından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Kullanıcılar Multi-Factor Authentication 'ı öğrensin mi?
* Ek kimlik doğrulaması sağlamak için bazı kullanımlar gerekli olacaktır mi?  
  * Yanıtınız evet ise, her zaman, dış ağlardan geldiği veya belirli uygulamalara erişme veya başka koşullar altında mi?
* Kullanıcılar Multi-Factor Authentication 'ı ayarlama ve uygulama hakkında eğitim ister misiniz?
* Şirketiniz, kullanıcıları için Multi-Factor Authentication 'ı etkinleştirmek istediği önemli senaryolar nelerdir?

Önceki soruları yanıtladıktan sonra, şirket içinde çok faktörlü kimlik doğrulamasının zaten uygulanıp uygulanmadığı anlayabileceksiniz. Bu değerlendirme, kuruluşların kullanıcılarını çok faktörlü kimlik doğrulaması için ayarlamaya ve etkinleştirmeye yönelik teknik gereksinimlerin tanımlanması açısından önemlidir. Aşağıdaki soruları cevaplamadığınızdan emin olun:

* Şirketinizin, MFA ile ayrıcalıklı hesapları koruması gerekiyor mu?
* Şirketinizin uyumluluk nedenleriyle belirli bir uygulama için MFA 'yı etkinleştirmesi gerekiyor mu?
* Şirketinizin bu uygulamanın veya yalnızca Yöneticiler için uygun olan tüm kullanıcılar için MFA 'yı etkinleştirmesi gerekiyor mu?
* MFA 'nın her zaman etkin mi yoksa yalnızca kullanıcılar şirket ağınızın dışında oturum açtıklarında mı ihtiyacınız var?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik benimseme stratejisi tanımlama](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

