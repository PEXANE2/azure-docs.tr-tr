---
title: Microsoft kimlik doğrulaması kitaplığı 'na (MSAL) geçiş
titleSuffix: Microsoft identity platform
description: Microsoft kimlik doğrulama kitaplığı (MSAL) ve Azure AD kimlik doğrulama kitaplığı (ADAL) arasındaki farklar ve MSAL 'e geçiş yapma hakkında bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78164938"
---
# <a name="migrate-applications-to-microsoft-authentication-library-msal"></a>Uygulamaları Microsoft kimlik doğrulama kitaplığı 'na geçirme (MSAL)

Hem Microsoft kimlik doğrulama kitaplığı (MSAL) hem de Azure AD kimlik doğrulama kitaplığı (ADAL), Azure AD varlıklarının kimliğini doğrulamak için kullanılır ve Azure AD 'nin belirteçleri ister. Bu aşamada, çoğu geliştirici Azure AD kimlik doğrulama kitaplığı (ADAL) kullanarak belirteçleri isteyerek Azure AD kimliklerinin (iş ve okul hesapları) kimliğini doğrulamak için Azure AD 'de geliştiriciler platformu (v 1.0) ile çalıştık. MSAL kullanma:

- Microsoft Identity platform uç noktasını kullandığından, daha geniş bir Microsoft kimliği kümesinin (Azure AD kimlikleri ve Microsoft hesapları ve sosyal ve yerel hesaplar Azure AD B2C aracılığıyla) kimlik doğrulaması yapabilirsiniz.
- Kullanıcılarınız en iyi çoklu oturum açma deneyimini alacak.
- Uygulamanız artımlı onayı etkinleştirebilir ve Koşullu erişimin desteklenmesi daha kolay olabilir.
- Yeniliğin avantajlarından yararlanabilirsiniz.

**Msal artık Microsoft Identity platformu ile kullanmak için önerilen kimlik doğrulama kitaplığıdır**. ADAL üzerinde yeni özellik uygulanmaz. Çabalar MSAL geliştirmeye odaklanılmıştır.

Aşağıdaki makalelerde, MSAL ve ADAL kitaplıkları arasındaki farklar açıklanır ve MSAL 'e geçiş yapmanıza yardımcı olur:
- [MSAL.NET'e geçirme](msal-net-migration.md)
- [MSAL.js'ye geçirme](msal-compare-msal-js-and-adal-js.md)
- [MSAL.Android’e geçiş](migrate-android-adal-msal.md)
- [MSAL.iOS / macOS’e Geçiş](migrate-objc-adal-msal.md)
- [MSAL Python’a geçiş](migrate-python-adal-msal.md)
- [Java için MSAL’ye geçiş](migrate-adal-msal-java.md)
- [Aracıları kullanarak Xamarin uygulamalarını MSAL.NET’e geçirme](msal-net-migration-ios-broker.md)
