---
title: İOS için MSAL & macOS farkları | Mavisi
titleSuffix: Microsoft identity platform
description: İOS ve macOS arasındaki Microsoft kimlik doğrulama kitaplığı (MSAL) kullanım farklarını açıklar.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: cdc06ee2aceae4bf8c689d4ef6db0609b8d71c8f
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76834221"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>iOS ve macOS için Microsoft Kimlik Doğrulaması Kitaplığı farkları

Bu makalede iOS ve macOS için Microsoft kimlik doğrulama kitaplığı (MSAL) arasındaki işlevsellikten oluşan farklar açıklanmaktadır.

> [!NOTE]
> Mac 'te, MSAL yalnızca macOS uygulamalarını destekler.

## <a name="general-differences"></a>Genel farklılıklar

MacOS için MSAL, iOS için kullanılabilen işlevlerin bir alt kümesidir.

MacOS için MSAL şunları desteklemez:

- `ASWebAuthenticationSession`, `SFAuthenticationSession``SFSafariViewController`gibi farklı tarayıcı türleri.
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
