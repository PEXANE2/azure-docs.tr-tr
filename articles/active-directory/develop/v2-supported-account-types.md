---
title: Desteklenen hesap türleri - Microsoft kimlik platformu | Azure
description: Uygulamalarda hedef kitleler ve desteklenen hesap türleri hakkında kavramsal dokümantasyon
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: de90c81f56b6017b2d53ecbfb2c400a4c9f05d81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262288"
---
# <a name="supported-account-types"></a>Desteklenen hesap türleri

Bu makalede, uygulamalarda hangi hesap türlerinin (bazen adlandırılmış hedef kitleler) desteklenir.

<!-- This section can be in an include for many of the scenarios (SPA, Web App signing-in users, protecting a Web API, Desktop (depending on the flows), Mobile -->

## <a name="supported-accounts-types-in-microsoft-identity-platform-applications"></a>Microsoft Identity platform uygulamalarında desteklenen hesap türleri

Microsoft Azure genel Bulutu'nda, çoğu uygulama türü herhangi bir hedef kitleye sahip kullanıcılarda oturum açabilir:

- Bir Business Hattı (LOB) uygulaması yazıyorsanız, kendi kuruluşunuzdaki kullanıcılararasında oturum açabilirsiniz. Böyle bir uygulama bazen **tek kiracı**olarak adlandırılır.
- ISV'yseniz, kullanıcılara imza atan bir uygulama yazabilirsiniz:

  - Herhangi bir organizasyonda. Böyle bir uygulama **çok kiracılı** web uygulaması olarak adlandırılır. Bazen, kullanıcıların iş veya okul hesaplarıyla kaydolduğunu okursunuz.
  - İşleriyle, okullarıyla veya kişisel Microsoft hesaplarıyla.
  - Yalnızca kişisel Microsoft hesabı yla.
    > [!NOTE]
    > Şu anda Microsoft kimlik platformu kişisel Microsoft hesaplarını yalnızca **iş veya okul veya Microsoft kişisel hesapları**için bir uygulama kaydederek destekler ve ardından, uygulama nın `https://login.microsoftonline.com/consumers`bir Azure REKLAM yetkilisi belirterek uygulama kodunda oturum açmayı kısıtlar, örneğin .

- Tüketicilere bir iş yazıyorsanız, Azure AD B2C'yi kullanarak kullanıcıları sosyal kimlikleriyle oturum açabilirsiniz.

## <a name="certain-authentication-flows-dont-support-all-the-account-types"></a>Belirli kimlik doğrulama akışları tüm hesap türlerini desteklemez

Bazı hesap türleri belirli kimlik doğrulama akışlarıyla kullanılamaz. Örneğin, masaüstü, UWP uygulamaları veya daemon uygulamalarında:

- Daemon uygulamaları yalnızca Azure Active Directory kuruluşlarında kullanılabilir. Microsoft'un kişisel hesaplarını işlemek için daemon uygulamalarını kullanmaya çalışmak mantıklı değildir (yönetici onayı asla verilmez).  
- Tümleşik Windows Kimlik Doğrulama akışını yalnızca iş veya okul hesaplarıyla (kuruluşunuzda veya herhangi bir kuruluşta) kullanabilirsiniz. Gerçekten de, Tümleşik Windows Kimlik Doğrulama etki alanı hesapları yla çalışır ve makinelerin etki alanına katılmasını veya Azure AD'nin katılmasını gerektirir. Bu akış kişisel Microsoft Hesapları için anlamlı değildir.
- [Kaynak Sahibi Parola Hibesi](./v2-oauth-ropc.md) (Kullanıcı Adı/Parola), kişisel Microsoft hesaplarıyla kullanılamaz. Gerçekten de, kişisel Microsoft hesapları, kullanıcının her oturumda kişisel kaynaklara erişmeye onay verdiğini gerektirir. Bu nedenle, bu davranış etkileşimli olmayan akışlarla uyumlu değildir.
- Aygıt kodu akışı henüz kişisel Microsoft hesaplarıyla çalışmaz.

## <a name="supported-account-types-in-national-clouds"></a>Ulusal bulutlarda desteklenen hesap türleri

 Uygulamalar, [ulusal bulutlardaki](authentication-national-cloud.md)kullanıcılarda da oturum açabilir. Ancak, Microsoft kişisel hesapları bu bulutlarda desteklenmez (bu bulutların tanımı gereği). Bu nedenle desteklenen hesap türleri, bu bulutlar için kuruluşunuz (tek kiracı) veya herhangi bir kuruluşa (çok kiracılı uygulamalar) indirgenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory'de Kiralama](./single-and-multi-tenant-apps.md) hakkında daha fazla bilgi edinin
- [Ulusal Bulutlar](./authentication-national-cloud.md) hakkında daha fazla bilgi edinin
