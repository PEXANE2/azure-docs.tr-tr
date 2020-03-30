---
title: iOS & macOS farklılıkları için MSAL | Azure
titleSuffix: Microsoft identity platform
description: iOS ve macOS arasındaki Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kullanım farklarını açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084971"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>iOS ve macOS için Microsoft Kimlik Doğrulaması Kitaplığı farkları

Bu makalede, iOS ve macOS için Microsoft Kimlik Doğrulama Kitaplığı (MSAL) arasındaki işlevsellik farklılıkları açıklanmaktadır.

> [!NOTE]
> Mac'te MSAL yalnızca macOS uygulamalarını destekler.

## <a name="general-differences"></a>Genel farklılıklar

macOS için MSAL, iOS için kullanılabilen işlevselliğin bir alt kümesidir.

macOS için MSAL şunları desteklemez:

- gibi farklı tarayıcı `ASWebAuthenticationSession` `SFAuthenticationSession`türleri `SFSafariViewController`, , .
- macOS için Microsoft Authenticator uygulaması aracılığıyla aracılı kimlik doğrulama sı desteklenmez.

Aynı yayımcının uygulamaları arasında anahtarlık paylaşımı macOS 10.14 ve daha önceki uygulamalarda daha sınırlıdır. Anahtar zinciri paylaşması gereken uygulamaların yollarını belirtmek için [erişim denetim listelerini](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) kullanın. Kullanıcı ek anahtarzinciri istemleri görebilir.

macOS 10.15+'da, MSAL'ın davranışı iOS ve macOS arasında aynıdır. MSAL anahtarlık paylaşımı için [anahtarlık erişim gruplarını](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) kullanır. 

### <a name="conditional-access-authentication-differences"></a>Koşullu erişim kimlik doğrulama farklılıkları

Koşullu Erişim senaryoları için, iOS için MSAL kullandığınızda daha az kullanıcı istemleri olacaktır. Bunun nedeni, iOS'un bazı durumlarda kullanıcıdan istenme gereksinimini yok eden broker uygulamasını (Microsoft Authenticator) kullanmasıdır.

### <a name="project-setup-differences"></a>Proje kurulum farklılıkları

**Macos**

- Projenizi macOS'ta ayarladığınızda, uygulamanızın geçerli bir geliştirme veya üretim sertifikasıyla imzalandığından emin olun. MSAL hala imzasız modda çalışır, ancak önbellek kalıcılığı açısından farklı davranacaktır. Uygulama yalnızca hata ayıklama amacıyla imzasız çalıştırılmalıdır. Uygulamayı imzasız dağıtırsanız, aşağıdakileri yapacaktır:
1. 10.14 ve daha önce, MSAL uygulamayı her yeniden başlattıklarında kullanıcıdan bir anahtarlık şifresi ister.
2. 10.15+'ta, MSAL kullanıcıdan her belirteç edinimi için kimlik bilgilerini ister. 

- macOS uygulamalarının AppDelegate çağrısını uygulaması gerekmez.

**iOS**

- Kimlik doğrulama aracısı akışını desteklemek için projenizi ayarlamak için ek adımlar vardır. Adımlar öğreticide çağrılır.
- iOS projelerinin info.plist'e özel şemaları kaydetmesi gerekir. MacOS'ta bu gerekli değildir.
