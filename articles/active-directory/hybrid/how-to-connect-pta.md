---
title: 'Azure AD Connect: Geçiş Kimlik Doğrulaması | Microsoft Dokümanlar'
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması ve kullanıcıların parolalarını şirket içi Active Directory'ye karşı doğrulayarak Azure AD oturum açmalarına nasıl izin verdiği açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication nedir, Active Directory'yi, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenleri yükleme
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d4b52a3025bfb15e2679709353cebf28254a75c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77185511"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Azure Active Directory Geçişli Kimlik Doğrulaması ile kullanıcı oturumu açma

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Azure Active Directory Pass-through Authentication nedir?

Azure Active Directory (Azure AD) Geçiş Kimlik Doğrulaması, kullanıcılarınızın aynı parolaları kullanarak hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmalarına olanak tanır. Bu özellik hatırlamaları gereken parola sayısını azaltarak kullanıcılarınıza daha iyi bir deneyim sağlar ve oturum açma bilgileri muhtemelen daha az unutulacağından BT yardım masası maliyetlerinizi de düşürür. Kullanıcılar Azure AD kullanarak oturum açtıklarında, bu özellik kullanıcıların parolalarını doğrudan şirket içi Active Directory'nize karşı doğrular.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Bu özellik, kuruluşlara bulut kimlik doğrulamasının aynı yararını sağlayan [Azure AD Parola Karma Eşitleme'ye](how-to-connect-password-hash-synchronization.md)alternatiftir. Ancak, şirket içi Etkin Dizin güvenlik lerini ve parola ilkelerini uygulamak isteyen bazı kuruluşlar, bunun yerine Geçiş Kimlik Doğrulama'yı kullanmayı seçebilir. Çeşitli Azure AD oturum açma yöntemlerinin karşılaştırılması ve kuruluşunuz için doğru oturum açma yönteminin nasıl seçilecek için [bu kılavuzu](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn) gözden geçirin.

![Azure AD Geçiş Kimlik Doğrulaması](./media/how-to-connect-pta/pta1.png)

Geçiş Kimlik Doğrulama'yı Kesintisiz [Tek Oturum Açma](how-to-connect-sso.md) özelliğiyle birleştirebilirsiniz. Bu şekilde, kullanıcılarınız kurumsal ağınızdaki kurumsal makinelerdeki uygulamalara erişirken, oturum açabilmek için parolalarını yazmaları gerekmez.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Azure AD Geçiş Kimlik Doğrulaması kullanmanın temel avantajları

- *Harika kullanıcı deneyimi*
  - Kullanıcılar hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açabilmek için aynı parolaları kullanır.
  - Kullanıcılar, parolayla ilgili sorunları çözme yardım masasıyla konuşmaya daha az zaman harcar.
  - Kullanıcılar bulutta [self servis parola yönetimi](../authentication/active-directory-passwords-overview.md) görevlerini tamamlayabilir.
- *Dağıtmak & yönetimi kolay*
  - Karmaşık şirket içi dağıtımlara veya ağ yapılandırmasına gerek yoktur.
  - Şirket içinde kurulacak hafif bir ajana ihtiyaç vardır.
  - Yönetim yükü yok. Aracı otomatik olarak iyileştirmeler ve hata düzeltmeleri alır.
- *Güvenlik*
  - Şirket içi parolalar hiçbir biçimde bulutta depolanır.
  - Çok Faktörlü Kimlik Doğrulama (MFA) dahil olmak üzere [Azure AD Koşullu Erişim ilkeleriyle](../active-directory-conditional-access-azure-portal.md)sorunsuz çalışarak, [eski kimlik doğrulamasını engelleyerek](../conditional-access/concept-conditional-access-conditions.md) ve [kaba kuvvet parola saldırılarını filtreleyerek](../authentication/howto-password-smart-lockout.md)kullanıcı hesaplarınızı korur.
  - Aracı yalnızca ağınızın içinden giden bağlantılar yapar. Bu nedenle, aracıyı DMZ olarak da bilinen bir çevre ağına yükleme zorunluluğu yoktur.
  - Bir aracı ile Azure AD arasındaki iletişim, sertifika tabanlı kimlik doğrulaması kullanılarak güvenli hale gelir. Bu sertifikalar Azure AD tarafından birkaç ayda bir otomatik olarak yenilenir.
- *Yüksek oranda kullanılabilir*
  - Oturum açma isteklerinin yüksek kullanılabilirliğini sağlamak için birden çok şirket içi sunucuya ek aracılar yüklenebilir.

## <a name="feature-highlights"></a>Özellik vurguları

- Tüm web tarayıcısı tabanlı uygulamalarda ve [modern kimlik doğrulaması](https://aka.ms/modernauthga)kullanan Microsoft Office istemci uygulamalarında kullanıcı oturum açmayı destekler.
- Oturum açma kullanıcı adları şirket içi varsayılan kullanıcı`userPrincipalName`adı ( ) veya Azure AD Connect'te yapılandırılan başka bir öznitelik (olarak `Alternate ID`bilinir) olabilir.
- Bu özellik, kullanıcılarınızın güvenliğini sağlamaya yardımcı olmak için Çok Faktörlü Kimlik Doğrulama (MFA) gibi [Koşullu Erişim](../active-directory-conditional-access-azure-portal.md) özellikleriyle sorunsuz çalışır.
- Yaygın olarak kullanılan parolaları yasaklayarak şirket içi Active Directory'ye parola yazma ve parola koruması da dahil olmak üzere bulut tabanlı [self servis parola yönetimiyle](../authentication/active-directory-passwords-overview.md)tümleştirilir.
- AD ormanlarınız arasında orman güvenleri varsa ve ad sonek yönlendirmesi doğru şekilde yapılandırılırsa, çok ormanlı ortamlar desteklenir.
- Ücretsiz bir özelliktir ve kullanmak için Azure AD'nin ücretli sürümlerine ihtiyacınız yoktur.
- [Azure AD Connect](whatis-hybrid-identity.md)üzerinden etkinleştirilebilir.
- Parola doğrulama isteklerini dinleyen ve yanıtlayan hafif bir şirket içi aracı kullanır.
- Birden çok aracının yüklenmesi, oturum açma isteklerinin yüksek kullanılabilirliğini sağlar.
- Şirket içi hesaplarınızı buluttaki kaba kuvvet parola saldırılarına karşı [korur.](../authentication/howto-password-smart-lockout.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart](how-to-connect-pta-quick-start.md) - Azure AD Pass-through Kimlik Doğrulaması'nı çalıştırın.
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md) - Kullanıcı hesaplarını korumak için kiracınızda Akıllı Kilitleme özelliğini yapılandırın.
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md) - Hangi senaryoların desteklenmediğini ve hangilerinin desteklenmediğini öğrenin.
- [Teknik Derin Dalış](how-to-connect-pta-how-it-works.md) - Bu özelliğin nasıl çalıştığını anlayın.
- [Sık Sorulan Sorular](how-to-connect-pta-faq.md) - Sık sorulan soruların yanıtları.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md) - Özellik ile ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Security Deep Dive](how-to-connect-pta-security-deep-dive.md) - Özellik hakkında ek derin teknik bilgiler.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md) - Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - Yeni özellik istekleri dosyalama için.
