---
title: 'Azure AD Connect: Geçiş Kimlik Doğrulama - Nasıl çalışır | Microsoft Dokümanlar'
description: Bu makalede, Azure Etkin Dizin Geçişi Kimlik Doğrulaması'nın nasıl çalıştığı açıklanmaktadır
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, yükleme Active Directory, Azure AD, SSO, Tek Oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59cd52dbdf6c13900cde592aeb52d8bf9abf850f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60347791"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory Pass-through Authentication: Teknik derin dalış
Bu makale, Azure Etkin dizininin (Azure AD) Geçiş Kimlik Doğrulaması'nın nasıl çalıştığına genel bir bakıştır. Derin teknik ve güvenlik bilgileri için [Güvenlik derin dalış](how-to-connect-pta-security-deep-dive.md) makalesine bakın.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Azure Active Directory Pass-through Authentication nasıl çalışır?

>[!NOTE]
>Geçiş Kimlik Doğrulaması'nın çalışması için ön koşul olarak, kullanıcıların Azure AD Connect'i kullanarak şirket içi Active Directory'den Azure AD'ye dahil edilmesi gerekir. Geçiş Kimlik Doğrulaması yalnızca bulut kullanıcıları için geçerli değildir.

Bir kullanıcı Azure AD tarafından güvenli bir uygulamada oturum açmaya çalıştığında ve kiracıda Geçiş Kimlik Doğrulaması etkinleştirilirse aşağıdaki adımlar oluşur:

1. Kullanıcı bir uygulamaya erişmeye çalışır, örneğin, [Outlook Web App](https://outlook.office365.com/owa/).
2. Kullanıcı zaten oturum açmamışsa, kullanıcı Azure AD **Kullanıcı Oturum Açma** sayfasına yönlendirilir.
3. Kullanıcı kullanıcı adını Azure AD oturum açma sayfasına girer ve **sonra Sonraki** düğmesini seçer.
4. Kullanıcı parolasını Azure AD oturum açma sayfasına girer ve oturum **açma** düğmesini seçer.
5. Azure AD, oturum açma isteğini alırken, kullanıcı adını ve parolayı (Kimlik Doğrulama Aracılarının ortak anahtarını kullanarak şifrelenir) sıraya yerleştirir.
6. Şirket içi Kimlik Doğrulama Aracısı, kullanıcı adını ve şifrelenmiş parolayı kuyruktan alır. Aracının kuyruktaki istekler için sık sık anket yapmadığını, ancak önceden kurulmuş kalıcı bir bağlantı üzerinden istekleri aldığını unutmayın.
7. Aracı, özel anahtarını kullanarak parolanın şifresini çözer.
8. Aracı, Active Directory Federation Services'in (AD FS) kullandığına benzer bir mekanizma olan standart Windows API'lerini kullanarak Kullanıcı adını ve parolayı Active Directory'ye karşı doğrular. Kullanıcı adı, genellikle `userPrincipalName`şirket içi varsayılan kullanıcı adı veya Azure AD Connect'te yapılandırılan `Alternate ID`başka bir öznitelik (adıyla bilinir) olabilir.
9. Şirket içi Active Directory etki alanı denetleyicisi (DC) isteği değerlendirir ve uygun yanıtı (başarı, hata, parola süresi dolmuş veya kilitlenmiş kullanıcı) aracıya döndürür.
10. Kimlik Doğrulama Aracısı da bu yanıtı Azure AD'ye geri döndürür.
11. Azure AD yanıtı değerlendirir ve kullanıcıya uygun şekilde yanıt verir. Örneğin, Azure AD kullanıcıyı hemen imzalar veya Azure Çok Faktörlü Kimlik Doğrulaması isteklerini ister.
12. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamaya erişebilir.

Aşağıdaki diyagram, ilgili tüm bileşenleri ve adımları gösterir:

![Doğrudan Kimlik Doğrulama](./media/how-to-connect-pta-how-it-works/pta2.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Hangi senaryoların desteklenip hangilerinin desteklenmediğini öğrenin.
- [Hızlı Başlangıç](how-to-connect-pta-quick-start.md): Azure AD Pass-through Kimlik Doğrulama'da çalışmaya başlayın.
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://aka.ms/adfstoPTADP) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızdaki Akıllı Kilitleme özelliğini yapılandırın.
- [Sık Sorulan Sorular](how-to-connect-pta-faq.md): Sık sorulan soruların yanıtlarını bulun.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md): Geçiş Kimlik Doğrulama özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Security Deep Dive](how-to-connect-pta-security-deep-dive.md): Pass-through Kimlik Doğrulama özelliği hakkında derin teknik bilgiler alın.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosyalamak için Azure Active Directory Forum'u kullanın.

