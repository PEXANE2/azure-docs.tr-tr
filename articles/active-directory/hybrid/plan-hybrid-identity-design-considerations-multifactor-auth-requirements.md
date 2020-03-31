---
title: Karma kimlik tasarımı - çok faktörlü kimlik doğrulama gereksinimleri Azure | Microsoft Dokümanlar
description: Koşullu Erişim denetimi ile Azure Etkin Dizin, kullanıcının kimliğini doğrularken ve uygulamaya erişime izin vermeden önce seçtiğiniz belirli koşulları denetler. Bu koşullar yerine getirildiğinde, kullanıcı nın kimliği doğrulanır ve uygulamaya erişime izin verilir.
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
ms.openlocfilehash: 4743195fc79d43571ec79a13b8518edc7e81379b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109303"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için çok faktörlü kimlik doğrulama gereksinimlerini belirleme
Bu mobilite dünyasında, kullanıcıların buluttaki verilere ve uygulamalara erişmesiyle ve herhangi bir cihazdan, bu bilgilerin güvenliğini sağlamak çok önemli hale gelmiştir.  Her gün bir güvenlik ihlali hakkında yeni bir başlık var.  Bu tür ihlallere karşı bir garanti olmamasına rağmen, çok faktörlü kimlik doğrulama, bu ihlalleri önlemeye yardımcı olacak ek bir güvenlik katmanı sağlar.
Çok faktörlü kimlik doğrulama için kuruluş gereksinimlerini değerlendirerek başlayın. Yani, ne organizasyon güvenli çalışıyor.  Bu değerlendirme, çok faktörlü kimlik doğrulaması için kuruluş kullanıcılarının kurulması ve etkinleştirilmesi için teknik gereksinimleri tanımlamak için önemlidir.

Aşağıdakileri yanıtlayabilmek için şunlara yanıt verin:

* Şirketiniz Microsoft uygulamalarını korumaya mı çalışıyor? 
* Bu uygulamalar nasıl yayınlanır?
* Şirketiniz, çalışanların şirket içi uygulamalara erişmesine izin vermek için uzaktan erişim sağlıyor mu?

Evet ise, ne tür bir uzaktan erişim? Ayrıca, bu uygulamalara erişen kullanıcıların nerede bulunacağını da değerlendirmeniz gerekir. Bu değerlendirme, uygun çok faktörlü kimlik doğrulama stratejisini tanımlamak için önemli bir adımdır. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Kullanıcılar nerede bulunacak?
* Herhangi bir yerde bulunabilirler mi?
* Şirketiniz kullanıcının konumuna göre kısıtlamalar oluşturmak istiyor mu?

Bu gereksinimleri anladıktan sonra, kullanıcının çok faktörlü kimlik doğrulama gereksinimlerini de değerlendirmek önemlidir. Bu değerlendirme önemlidir, çünkü çok faktörlü kimlik doğrulaması için gereksinimleri tanımlar. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Kullanıcılar çok faktörlü kimlik doğrulamayı biliyor mu?
* Ek kimlik doğrulaması sağlamak için bazı kullanımlar gerekli olacak mı?  
  * Evet ise, her zaman, harici ağlardan gelirken veya belirli uygulamalara erişirken veya başka koşullar altında?
* Kullanıcılar çok faktörlü kimlik doğrulamayı nasıl kurup uygulayacakları konusunda eğitime ihtiyaç duyar lar mı?
* Şirketinizin kullanıcıları için çok faktörlü kimlik doğrulaması etkinleştirmek istediği önemli senaryolar nelerdir?

Önceki soruları yanıtladıktan sonra, şirket içinde zaten uygulanan çok faktörlü kimlik doğrulaması olup olmadığını anlayabilirsiniz. Bu değerlendirme, çok faktörlü kimlik doğrulaması için kuruluş kullanıcılarının kurulması ve etkinleştirilmesi için teknik gereksinimleri tanımlamak için önemlidir. Aşağıdaki soruları yanıtladıklısa mutlaka:

* Şirketinizin MFA ile ayrıcalıklı hesapları koruması gerekiyor mu?
* Şirketinizin uyumluluk nedenleriyle belirli bir uygulama için MFA'yı etkinleştirmesi gerekiyor mu?
* Şirketinizin bu uygulamanın tüm uygun kullanıcıları veya yalnızca yöneticiler için MFA'yı etkinleştirmesi gerekiyor mu?
* MFA'nın her zaman etkin leştirilmiş olması mı gerekiyor yoksa yalnızca kullanıcılar şirket ağınızın dışında oturum açtıklarında mı?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik benimseme stratejisi tanımlayın](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

