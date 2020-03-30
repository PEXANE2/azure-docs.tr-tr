---
title: B2B işbirliği kullanıcıları için Koşullu Erişim - Azure AD
description: Azure Active Directory B2B işbirliği, kurumsal uygulamalarınıza seçici erişim için çok faktörlü kimlik doğrulamayı (MFA) destekler
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0b6ceba4c3c9202e2024b5c163c0e98bb6cbf55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74273009"
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için Koşullu Erişim

## <a name="multi-factor-authentication-for-b2b-users"></a>B2B kullanıcıları için çok faktörlü kimlik doğrulama
Azure AD B2B işbirliği yle kuruluşlar, B2B kullanıcıları için çok faktörlü kimlik doğrulama (MFA) ilkelerini uygulayabilir. Bu ilkeler, tam zamanlı çalışanlar ve kuruluş üyeleri için etkinleştirildikleri şekilde kiracı, uygulama veya bireysel kullanıcı düzeyinde uygulanabilir. Kaynak kuruluşunda MFA ilkeleri uygulanır.

Örnek:
1. A şirketindeki yönetici veya bilgi çalışanı, B şirketindeki kullanıcıyı A şirketindeki bir uygulama *Foo'ya* davet eder.
2. A şirketindeki Uygulama *Foo'su,* erişimde MFA gerektirecek şekilde yapılandırılmıştır.
3. B şirketindeki kullanıcı, A kiracışirketindeki *Foo* uygulamasına erişmeye çalıştığında, bir MFA yarışmasını tamamlamaları istenir.
4. Kullanıcı, MFA'sını A şirketiyle kurabilir ve MFA seçeneğini seçebilir.
5. Bu senaryo herhangi bir kimlik için çalışır (örneğin, B Şirketindeki kullanıcılar sosyal kimliği kullanarak kimlik doğrulaması yaparsa Azure AD veya MSA)
6. Şirket A, MFA'yı destekleyen yeterli Premium Azure REKLAM lisanslarına sahip olmalıdır. B şirketinin kullanıcısı bu lisansı A şirketinden tüketir.

Davet karşılığı, ortak ãrgüm MFA yeteneklerine sahip olsa bile, ortak kuruluştan kullanıcılar için MFA' dan her zaman sorumlu olur.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için MFA kurulumu
B2B işbirliği kullanıcıları için MFA'yı ayarlamanın ne kadar kolay olduğunu keşfetmek için aşağıdaki videoda nasıl olduğunu görün:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>Teklif kullanımı için B2B kullanıcıları MFA deneyimi
Kullanım deneyimini görmek için aşağıdaki animasyona göz atın:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>B2B işbirliği kullanıcıları için MFA sıfırlama
Şu anda, yönetici B2B işbirliği kullanıcılarının yalnızca aşağıdaki PowerShell cmdlets kullanarak yeniden prova gerektirebilir:

1. Azure AD'ye Bağlanma

   ```
   $cred = Get-Credential
   Connect-MsolService -Credential $cred
   ```
2. Tüm kullanıcıları prova yöntemleriyle elde edin

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```
   Örnek aşağıda verilmiştir:

   ```
   Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
   ```

3. Belirli bir kullanıcının B2B işbirliği kullanıcısının prova yöntemlerini yeniden ayarlamasını gerektirmesi için MFA yöntemini sıfırla. Örnek:

   ```
   Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
   ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a>Neden kaynak kiramda MFA gerçekleştiriyoruz?

Geçerli sürümde, MFA her zaman kaynak kira, öngörülebilirlik nedenlerle. Örneğin, bir Contoso kullanıcısının (Sally) Fabrikam'a davet edildiği ve Fabrikam'ın B2B kullanıcıları için MFA'yı etkinleştirdiğini varsayalım.

Contoso'nun App1 için MFA politikası etkinleştirilmişse, ancak App2 için etkinleştirilirse, belirteçteki Contoso MFA iddiasına bakarsak, aşağıdaki sorunu görebiliriz:

* 1. Gün: Bir kullanıcıcontoso'da MFA'ya sahiptir ve App1'e erişiyorsa, Fabrikam'da ek MFA istemi gösterilmez.

* 2. Gün: Kullanıcı Contoso'da App 2'ye erişmiştir, bu nedenle Fabrikam'a erişirken, orada MFA'ya kaydolmalıdır.

Bu işlem kafa karıştırıcı olabilir ve oturum açma tamamlamalarının düşmesine neden olabilir.

Ayrıca, Contoso MFA yeteneğine sahip olsa bile, Fabrikam'ın Contoso MFA politikasına güveneceği durum her zaman değildir.

Son olarak, kaynak kiracı MFA da MSAs ve sosyal iAnd'ler ve MFA kurmak yok ortak orglar için çalışır.

Bu nedenle, B2B kullanıcıları için MFA için tavsiye her zaman davet eden kiracı MFA gerektirir. Bu gereksinim bazı durumlarda MFA'nın iki katına çıkabilir, ancak davet eden kiracıya her erişildiğinde, son kullanıcı deneyimi tahmin edilebilir: Sally, Davet eden kiracıyla MFA'ya kaydolmalıdır.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>B2B kullanıcıları için cihaz tabanlı, konum tabanlı ve risk tabanlı Koşullu Erişim

Contoso, şirket verileri için aygıt tabanlı Koşullu Erişim ilkelerini etkinleştirdiğinde, Contoso tarafından yönetilmeyen ve Contoso aygıt ilkeleriyle uyumlu olmayan aygıtlardan erişim engellenir.

B2B kullanıcının aygıtı Contoso tarafından yönetilmiyorsa, b2B kullanıcılarının ortak kuruluşlardan erişimi, bu ilkeler hangi bağlamda uygulanırsa uygulansın engellenir. Ancak Contoso, aygıt tabanlı Koşullu Erişim ilkesinden hariç tutmak için belirli iş ortağı kullanıcıları içeren dışlama listeleri oluşturabilir.

#### <a name="mobile-application-management-policies-for-b2b"></a>B2B için mobil uygulama yönetimi politikaları

Davet eden kuruluşun B2B kullanıcısının ana kuruluşunda görünürlüğü olmadığından, Koşullu Erişim uygulaması koruma ilkeleri B2B kullanıcılarına uygulanamaz.

#### <a name="location-based-conditional-access-for-b2b"></a>B2B için Konum Tabanlı Koşullu Erişim

Davet eden kuruluş iş ortağı kuruluşlarını tanımlayan güvenilir bir IP adresi aralığı oluşturabiliyorsa, Konum tabanlı Koşullu Erişim ilkeleri B2B kullanıcıları için zorlanabilir.

#### <a name="risk-based-conditional-access-for-b2b"></a>B2B için Risk Tabanlı Koşullu Erişim

Risk değerlendirmesi B2B kullanıcısının ana kuruluşunda yapıldığından, şu anda risk tabanlı oturum açma ilkeleri B2B kullanıcılarına uygulanamaz.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
