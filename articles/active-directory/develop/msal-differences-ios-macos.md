---
title: İOS için MSAL & macOS farkları | Mavisi
titleSuffix: Microsoft identity platform
description: İOS ve macOS arasındaki Microsoft kimlik doğrulama kitaplığı (MSAL) kullanım farklarını açıklar.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 62b79ee7398286b8e6c8ed8612bd001595e1f6ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77084971"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>iOS ve macOS için Microsoft Kimlik Doğrulaması Kitaplığı farkları

Bu makalede iOS ve macOS için Microsoft kimlik doğrulama kitaplığı (MSAL) arasındaki işlevsellikten oluşan farklar açıklanmaktadır.

> [!NOTE]
> Mac 'te, MSAL yalnızca macOS uygulamalarını destekler.

## <a name="general-differences"></a>Genel farklılıklar

MacOS için MSAL, iOS için kullanılabilen işlevlerin bir alt kümesidir.

MacOS için MSAL şunları desteklemez:

- `ASWebAuthenticationSession`, `SFAuthenticationSession`,, `SFSafariViewController`gibi farklı tarayıcı türleri.
- Microsoft Authenticator uygulama aracılığıyla aracılı kimlik doğrulaması, macOS için desteklenmez.

Aynı yayımcıdan uygulamalar arasında Anahtarlık paylaşımı, macOS 10,14 ve daha önceki sürümlerde daha sınırlıdır. Anahtarlığı paylaşması gereken uygulamaların yollarını belirtmek için [erişim denetimi listelerini](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) kullanın. Kullanıcı, ek Anahtarlık istemler görebilir.

MacOS 10.15 + ' da, MSAL 'ın davranışı iOS ve macOS arasında aynıdır. MSAL Anahtarlık paylaşımı için [Anahtarlık erişim grupları](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) kullanır. 

### <a name="conditional-access-authentication-differences"></a>Koşullu erişim kimlik doğrulama farklılıkları

Koşullu erişim senaryolarında, iOS için MSAL kullandığınızda daha az Kullanıcı istemi olacaktır. Bunun nedeni, Microsoft Authenticator iOS 'un bazı durumlarda kullanıcıya isteme gereksinimini geçersiz hale getirir.

### <a name="project-setup-differences"></a>Proje kurulumu farklılıkları

**macOS**

- Projeyi macOS 'ta ayarlarken, uygulamanızın geçerli bir geliştirme veya üretim sertifikasıyla imzalandığından emin olun. MSAL, imzasız modda hala çalışır, ancak önbellek kalıcılığına göre farklı davranacaktır. Uygulamanın yalnızca hata ayıklama amacıyla imzasız çalıştırması gerekir. Uygulamayı imzasız olarak dağıtırsanız:
1. 10,14 ve önceki sürümlerde MSAL, uygulamayı her yeniden başlattıklarında kullanıcıdan bir anahtarlık parolası ister.
2. 10.15 + tarihinde, MSAL her belirteç alımı için kullanıcıdan kimlik bilgilerini ister. 

- macOS uygulamalarının AppDelegate çağrısını uygulaması gerekmez.

**iOS**

- Projenizi kimlik doğrulama Aracısı akışını destekleyecek şekilde ayarlamak için ek adımlar vardır. Adımlarda öğreticide çağrılır.
- iOS projelerinin, Info. plist dosyasında özel şemaları kaydetmesi gerekir. Bu, macOS 'ta gerekli değildir.
