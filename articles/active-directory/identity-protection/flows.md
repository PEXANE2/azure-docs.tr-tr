---
title: Azure AD Kimlik Koruması | oturum açma deneyimleri | Microsoft Docs
description: Kimlik koruması bir kullanıcıyı azaldığında veya seçtiğinde ya da bir ilke için Multi-Factor Authentication gerektiğinde kullanıcı deneyimine genel bir bakış sağlar.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e513027eed44ec7649f41f8786882aed8511bc6
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335501"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Azure AD Kimlik Koruması oturum açma deneyimleri

Azure Active Directory Kimlik Koruması, şunları yapabilirsiniz:

* Kullanıcıların Multi-Factor Authentication 'a kaydolmesini gerektir
* riskli oturum açma işlemlerini ve güvenliği aşılmış kullanıcıları işleme

Sistemin bu sorunlara verdiği yanıt, kullanıcının oturum açma deneyiminde bir etkiye sahiptir çünkü bir Kullanıcı adı sağlayarak doğrudan oturum açmak ve bir parola artık mümkün olmayacaktır. Bir kullanıcıyı güvenle iş 'e geri almak için ek adımlar gereklidir.

Bu makalede, bir kullanıcının gerçekleşebileceğini tüm durumlarda oturum açma deneyimine ilişkin bir genel bakış sunulmaktadır.

**Multi-Factor Authentication**

* Multi-Factor Authentication kaydı

**Risk altında oturum açma**

* Riskli oturum açma kurtarması
* Riskli oturum açma engellendi
* Riskli oturum açma sırasında Multi-Factor Authentication kaydı

**Risk altındaki Kullanıcı**

* Güvenliği aşılmış hesap kurtarma
* Güvenliği aşılmış hesap engellendi

## <a name="multi-factor-authentication-registration"></a>Multi-Factor Authentication kaydı
Hem güvenliği aşılmış hesap kurtarma akışı hem de riskli oturum açma akışı için en iyi kullanıcı deneyimi, kullanıcının kendi kendine kurtarılmasına yönelik bir yöntemdir. Kullanıcılar Multi-Factor Authentication için kaydedilmişse, güvenlik sorunlarını geçebilmeleri için kullanılabilecek kendi hesabıyla ilişkilendirilmiş bir telefon numarası zaten vardır. Hesap güvenliğinin aşılmasına karşı kurtarmak için yardım masası veya yönetici katılımı gerekmez. Bu nedenle, kullanıcılarınızın çok faktörlü kimlik doğrulaması için kaydolmasının kesinlikle yapılması önerilir. 

Yöneticiler, kullanıcıların ek güvenlik doğrulaması için hesaplarını ayarlamanızı gerektiren bir ilke ayarlayabilir. Bu ilke, kullanıcıların Multi-Factor Authentication kaydını 14 güne kadar atmasını sağlar. 14 günlük yetkisiz kullanım süresi yapılandırılamaz.

**Multi-Factor Authentication kaydında üç adım vardır:**

1. İlk adımda Kullanıcı, hesabı Multi-Factor Authentication için ayarlama gereksinimi hakkında bir bildirim alır. 
   
    ![Düzeltme](./media/flows/140.png "Düzeltme")
2. Multi-Factor Authentication 'ı ayarlamak için sistemin nasıl iletişim kurmak istediğinizi bilmesini istemeniz gerekir.
   
    ![Düzeltme](./media/flows/141.png "Düzeltme")
3. Sistem size bir zorluk gönderir ve yanıt almanız gerekir.
   
    ![Düzeltme](./media/flows/142.png "Düzeltme")

## <a name="risky-sign-in-recovery"></a>Riskli oturum açma kurtarması
Yönetici oturum açma riskleri için bir ilke yapılandırmışsa, etkilenen kullanıcılara oturum açmaya çalıştıklarında bildirim yapılır. 

**Riskli oturum açma akışında iki adım vardır:** 

1. Kullanıcı, yeni bir konumdan, cihazdan veya uygulamadan oturum açma gibi, oturum açma bilgileri hakkında olağan dışı bir şey olduğunu bilgilendirilir. 
   
    ![Düzeltme](./media/flows/120.png "Düzeltme")
2. Kullanıcının bir güvenlik sınamasını çözerek kimliğini kanıtlamaları gerekir. Kullanıcı Multi-Factor Authentication için kaydedilmişse, bir güvenlik kodunu telefon numarasına geri dönüş yapması gerekir. Bu yalnızca riskli oturum açma ve güvenliği aşılmış bir hesap olduğundan, kullanıcının bu akıştaki parolayı değiştirmesi gerekmez. 
   
    ![Düzeltme](./media/flows/121.png "Düzeltme")

## <a name="risky-sign-in-blocked"></a>Riskli oturum açma engellendi
Yöneticiler, risk düzeyine bağlı olarak oturum açtığında kullanıcıları engellemek için bir oturum açma risk ilkesi ayarlamayı da tercih edebilir. Engellenmemiş olması için, son kullanıcıların bir yönetici veya yardım masası ile iletişim kurabilmesi veya tanıdık bir konumdan veya cihazdan oturum açmayı deneyebilmeleri gerekir. Multi-Factor Authentication 'ı çözerek kendi kendine kurtarma Bu durumda bir seçenek değildir.

![Düzeltme](./media/flows/200.png "Düzeltme")

## <a name="compromised-account-recovery"></a>Güvenliği aşılmış hesap kurtarma
Bir Kullanıcı riski güvenlik ilkesi yapılandırıldığında, ilkede belirtilen Kullanıcı risk düzeyini karşılayan (ve bu nedenle güvenliği ihlal edilen) kullanıcılar, oturum açmadan önce Kullanıcı güvenliğinin aşılmasına karşı koruma akışından gelmelidir. 

**Kullanıcı güvenliğinin aşılmasına yönelik kurtarma akışı üç adımdan daha sahiptir:**

1. Kullanıcı, şüpheli etkinlik veya sızdırılan kimlik bilgileri nedeniyle hesap güvenliğinin risk altında olduğunu bilgilendirilir.
   
    ![Düzeltme](./media/flows/101.png "Düzeltme")
2. Kullanıcının bir güvenlik sınamasını çözerek kimliğini kanıtlamaları gerekir. Kullanıcı Multi-Factor Authentication için kaydedilmişse, kendi kendine kurtarılmaları tehlikeye girebilir. Bu kişilerin telefon numarasına bir güvenlik kodu geri dönüş yapması gerekecektir. 
   
   ![Düzeltme](./media/flows/110.png "Düzeltme")
3. Son olarak, başka birinin hesabına erişimi olduğundan, Kullanıcı parolasını değiştirmeye zorlanır. 
   Bu deneyimin ekran görüntüleri aşağıda verilmiştir.
   
   ![Düzeltme](./media/flows/111.png "Düzeltme")

## <a name="compromised-account-blocked"></a>Güvenliği aşılmış hesap engellendi
Kullanıcı risk güvenlik ilkesi engellemesi tarafından engellenmiş bir kullanıcıyı engellemek için Kullanıcı bir yönetici veya yardım masası ile bağlantı kurmanız gerekir. Multi-Factor Authentication 'ı çözerek kendi kendine kurtarma Bu durumda bir seçenek değildir.

![Düzeltme](./media/flows/104.png "Düzeltme")

## <a name="reset-password"></a>Parola sıfırla
Güvenliği aşılmış kullanıcıların oturum açması engelleniyorsa, yönetici bunlar için geçici bir parola oluşturabilir. Kullanıcıların bir sonraki oturum açma sırasında parolalarını değiştirmesi gerekir.

![Düzeltme](./media/flows/160.png "Düzeltme")

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Active Directory Kimlik Koruması](../active-directory-identityprotection.md) 
