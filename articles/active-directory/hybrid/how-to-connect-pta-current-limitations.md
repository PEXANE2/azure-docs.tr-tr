---
title: 'Azure AD Connect: Geçiş Kimlik Doğrulaması - Geçerli sınırlamalar | Microsoft Dokümanlar'
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması'nın geçerli sınırlamaları açıklanmaktadır
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
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97dc67d46b08bf5765c59806b45edd82f38720cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60347762"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Etkin Dizin Geçiş Kimlik Doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması ücretsiz bir özelliktir ve kullanmak için Azure AD'nin ücretli sürümlerine ihtiyacınız yoktur. Geçiş Kimlik Doğrulaması, [Microsoft Azure Almanya bulutu](https://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure Devlet bulutu](https://azure.microsoft.com/features/gov/)nda değil, yalnızca dünya çapındaki Azure AD örneğinde kullanılabilir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolar desteklenir:

- Web tarayıcısı tabanlı uygulamalarda kullanıcı oturum açma.
- Exchange ActiveSync, EAS, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcilerine kullanıcı oturum açma.
- Eski Office istemci uygulamalarında ve [modern kimlik doğrulamasını](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview)destekleyen Office uygulamalarında kullanıcı oturum açma: Office 2013 ve 2016 sürümleri.
- PowerShell sürüm 1.0 ve diğerleri gibi eski protokol uygulamalarında kullanıcı oturum açma.
- Azure AD, Windows 10 aygıtları için birleşiyor.
- Çok Faktörlü Kimlik Doğrulama için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _desteklenmez:_

- [Sızdırılmış kimlik bilgilerine](../reports-monitoring/concept-risk-events.md#leaked-credentials)sahip kullanıcıların algılanması.
- Azure AD Etki Alanı Hizmetleri'nin kiracıda etkinleştirilebilmesi için Parola Hash Eşitlemesi gerekir. Bu nedenle, Geçiş Kimlik Doğrulaması kullanan kiracılar _yalnızca_ Azure AD Etki Alanı Hizmetleri gerektiren senaryolar için çalışmaz.
- Geçiş Kimlik [Doğrulaması, Azure AD Connect Health](whatis-hybrid-identity-health.md)ile tümleşik değildir.

> [!IMPORTANT]
> Yalnızca desteklenmeyen _senaryolar_ için geçici çözüm olarak (Azure AD Connect Health tümleştirmesi hariç), Azure AD Connect sihirbazındaki [İsteğe Bağlı özellikler](how-to-connect-install-custom.md#optional-features) sayfasında Parola Hash Senkronizasyonunu etkinleştirin.
> 
> [!NOTE]
> Parola Karma Eşitlemesini etkinleştirmek, şirket içi altyapınız kesintiye uğrarsa kimlik doğrulamayı başarısız bulma nızı sağlar. Geçiş Kimlik Doğrulama'dan Parola Hash Senkronizasyonuna geçiş otomatik değildir. Azure AD Connect'i kullanarak oturum açma yöntemini el ile değiştirmeniz gerekir. Azure AD Connect çalıştıran sunucu çökerse, Geçiş Kimlik Doğrulaması'nı kapatmak için Microsoft Desteği'nden yardım gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı başlangıç](how-to-connect-pta-quick-start.md): Azure AD Pass-through Authentication ile çalışmaya başlayın.
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://aka.ms/ADFSTOPTADPDownload) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızdaki Akıllı Kilitleme özelliğini nasıl yapılandırılabildiğini öğrenin.
- [Teknik derin dalış](how-to-connect-pta-how-it-works.md): Geçiş Kimlik Doğrulama özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Geçiş Kimlik Doğrulama özelliği hakkında sık sorulan soruların yanıtlarını bulun.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md): Geçiş Kimlik Doğrulama özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Güvenlik derin dalış](how-to-connect-pta-security-deep-dive.md): Pass-through Kimlik Doğrulama özelliği hakkında derin teknik bilgi alın.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosyalamak için Azure Active Directory Forum'u kullanın.
