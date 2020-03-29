---
title: Microsoft Kimlik Doğrulama Kitaplığına (MSAL) geçirin
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ve Azure AD Kimlik Doğrulama Kitaplığı (ADAL) arasındaki farklar ve MSAL'a nasıl geçirilir hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/27/2020
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 3af18eb09fd9906a0caaebda0b786795400467f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164938"
---
# <a name="migrate-applications-to-microsoft-authentication-library-msal"></a>Uygulamaları Microsoft Kimlik Doğrulama Kitaplığı'na (MSAL) geçirin

Hem Microsoft Kimlik Doğrulama Kitaplığı (MSAL) hem de Azure AD Kimlik Doğrulama Kitaplığı (ADAL), Azure AD varlıklarının kimliğini doğrulamak ve Azure AD'den belirteç istemek için kullanılır. Şimdiye kadar çoğu geliştirici, Azure AD Kimlik Doğrulama Kitaplığı 'nı (ADAL) kullanarak belirteçler isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için geliştiriciler platformu (v1.0) için Azure AD ile birlikte çalışmıştır. MSAL kullanma:

- Microsoft kimlik platformu bitiş noktasını kullandığından daha geniş bir Microsoft kimlik kümesini (Azure AD kimlikleri ve Microsoft hesapları ve Azure AD B2C aracılığıyla sosyal ve yerel hesaplar) doğrulayabilirsiniz.
- Kullanıcılarınız en iyi tek oturum açma deneyimini yaşar.
- Uygulamanız artımlı onay sağlayabilir ve Koşullu Erişimi desteklemek daha kolaydır.
- İnovasyondan yararlanırsınız.

**MSAL şimdi Microsoft kimlik platformu ile kullanmak için önerilen auth kütüphane .** ADAL'da yeni özellikler uygulanmayacaktır. Çalışmalar MSAL'ın iyileştirilmesi ne yöneliktir.

Aşağıdaki makaleler, MSAL ve ADAL kitaplıkları arasındaki farkları açıklar ve MSAL'a geçiş edeyim yardımcı olur:
- [MSAL.NET'e geçirme](msal-net-migration.md)
- [MSAL.js'ye geçirme](msal-compare-msal-js-and-adal-js.md)
- [MSAL.Android’e geçiş](migrate-android-adal-msal.md)
- [MSAL.iOS / macOS'a geçirin](migrate-objc-adal-msal.md)
- [MSAL Python’a geçiş](migrate-python-adal-msal.md)
- [Java için MSAL’ye geçiş](migrate-adal-msal-java.md)
- [Aracıları kullanarak Xamarin uygulamalarını MSAL.NET’e geçirme](msal-net-migration-ios-broker.md)
