---
title: 'Azure AD Connect: geçişli kimlik doğrulaması-geçerli sınırlamalar | Microsoft Docs'
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulamanın geçerli sınırlamaları açıklanmaktadır
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması, Active Directory yüklemesi, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60347762"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory geçişli kimlik doğrulaması: geçerli sınırlamalar

>[!IMPORTANT]
>Azure Active Directory (Azure AD) geçişli kimlik doğrulaması, ücretsiz bir özelliktir ve Azure AD 'nin ücretli sürümlerinden birini kullanması gerekmez. Doğrudan kimlik doğrulaması, [Microsoft Azure Almanya bulutta](https://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure Kamu bulutta](https://azure.microsoft.com/features/gov/)değil, Azure AD 'nin yalnızca dünya genelindeki örneğinde mevcuttur.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolar desteklenir:

- Web tarayıcısı tabanlı uygulamalarda Kullanıcı oturum açma işlemleri.
- Exchange ActiveSync, EAS, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcilerinde Kullanıcı oturum açma işlemleri.
- Eski Office istemci uygulamalarına ve [modern kimlik doğrulamayı](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview)destekleyen Office uygulamalarına Kullanıcı oturum açma işlemleri: Office 2013 ve 2016 sürümleri.
- PowerShell sürüm 1,0 ve diğerleri gibi eski protokol uygulamalarına Kullanıcı oturum açma işlemleri.
- Windows 10 cihazları için Azure AD birleşimleri.
- Multi-Factor Authentication için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki _senaryolar desteklenmez:_

- [Sızdırılan kimlik bilgilerine](../reports-monitoring/concept-risk-events.md#leaked-credentials)sahip kullanıcıların algılanması.
- Azure AD Domain Services kiracının etkin olması için Parola karması eşitlemesi gerekiyor. Bu nedenle doğrudan kimlik doğrulaması kullanan kiracılar _yalnızca_ Azure AD Domain Services ihtiyacı olan senaryolar için çalışmaz.
- Geçişli kimlik doğrulaması [Azure AD Connect Health](whatis-hybrid-identity-health.md)ile tümleştirilmiştir.

> [!IMPORTANT]
> _Yalnızca_ desteklenmeyen senaryolar (Azure AD Connect Health tümleştirme hariç) için geçici bir çözüm olarak, Azure AD Connect sihirbazındaki [isteğe bağlı özellikler](how-to-connect-install-custom.md#optional-features) sayfasında parola karması eşitlemesini etkinleştirin.
> 
> [!NOTE]
> Parola karması eşitlemesini etkinleştirmek, şirket içi altyapınız kesintiye uğradıysanız kimlik doğrulama için yük devretme seçeneği sağlar. Geçiş kimlik doğrulamasından Parola karması eşitlemesine bu yük devretme işlemi otomatik değildir. Oturum açma yöntemini Azure AD Connect kullanarak el ile geçmeniz gerekir. Azure AD Connect çalıştıran sunucu devre dışı bırakılırsa doğrudan kimlik doğrulamasını kapatmak için Microsoft Desteği yardım almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı başlangıç](how-to-connect-pta-quick-start.md): Azure AD geçişli kimlik doğrulaması ile çalışmaya başlayın.
- [AD FS 'den geçişli kimlik doğrulamaya geçiş](https://aka.ms/ADFSTOPTADPDownload) -AD FS (veya diğer Federasyon teknolojileri) üzerinden doğrudan kimlik doğrulamaya geçiş yapmak için ayrıntılı kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızda akıllı kilitleme özelliğini nasıl yapılandıracağınızı öğrenin.
- [Teknik derinlemesine](how-to-connect-pta-how-it-works.md)bakış: geçişli kimlik doğrulama özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): doğrudan kimlik doğrulama özelliği hakkında sık sorulan soruların yanıtlarını bulun.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): doğrudan kimlik doğrulama özelliğiyle yaygın sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](how-to-connect-pta-security-deep-dive.md)bakış: doğrudan kimlik doğrulama özelliği hakkında ayrıntılı teknik bilgiler alın.
- [Azure AD sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik isteklerini dosya olarak yüklemek Için Azure Active Directory forumunu kullanın.
