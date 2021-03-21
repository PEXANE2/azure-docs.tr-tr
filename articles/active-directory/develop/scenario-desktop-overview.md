---
title: Web API 'Lerini çağıran bir masaüstü uygulaması oluşturun | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin (genel bakış)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: feedc0065419162237c4f4826fe275a6fd947e53
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98753731"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran masaüstü uygulaması

Web API 'Lerini çağıran bir masaüstü uygulaması oluşturmak için ihtiyacınız olan her şey hakkında bilgi edinin.

## <a name="get-started"></a>başlarken

Henüz yapmadıysanız, bir hızlı başlangıcı tamamlayarak ilk uygulamanızı oluşturun:

- [Hızlı Başlangıç: Bir belirteç alma ve bir Windows masaüstü uygulamasından Microsoft Graph API'si çağırma](./quickstart-v2-windows-desktop.md)
- [Hızlı başlangıç: UWP uygulamasından belirteç alma ve Microsoft Graph API çağırma](./quickstart-v2-uwp.md)
- [Hızlı başlangıç: bir macOS yerel uygulamasından belirteç alma ve Microsoft Graph API çağrısı](./quickstart-v2-ios.md)

## <a name="overview"></a>Genel Bakış

Bir masaüstü uygulaması yazar ve kullanıcılara oturum açmak ve Microsoft Graph, diğer Microsoft API 'Leri ya da kendi Web API 'niz gibi Web API 'Lerini çağırmak istiyorsunuz. Birkaç seçeneğiniz vardır:

- Etkileşimli belirteç alımı kullanabilirsiniz:

  - Masaüstü uygulamanız grafik denetimlerini destekliyorsa (örneğin, bir Windows. form uygulaması, bir WPF uygulaması veya macOS yerel uygulaması).
  - Ya da bir .NET Core uygulaması ise ve Azure Active Directory (Azure AD) ile ilgili kimlik doğrulama etkileşiminin sistem tarayıcısında gerçekleşme olduğunu kabul ediyorsanız.

- Windows barındırılan uygulamalar için, bir Windows etki alanına katılmış bilgisayarlarda çalışan uygulamalar veya Azure AD 'ye katılmış bir belirteci, tümleşik Windows kimlik doğrulaması kullanarak sessizce bir belirteç elde etmek için de mümkündür.
- Son olarak, ancak önerilmese de, genel istemci uygulamalarında bir Kullanıcı adı ve parola kullanabilirsiniz. DevOps gibi bazı senaryolarda hala gereklidir. Bunu kullanarak uygulamanıza kısıtlamalar uygular. Örneğin, [çok faktörlü kimlik doğrulaması](../authentication/concept-mfa-howitworks.md) (koşullu erişim) gerçekleştirmesi gereken bir Kullanıcı oturum açamaz. Ayrıca, uygulamanız çoklu oturum açma özelliğinden (SSO) faydalanır.

  Bu, modern kimlik doğrulaması ilkelerine de karşı geçerlidir ve yalnızca eski nedenlerle sağlanır.

  ![Masaüstü uygulaması](media/scenarios/desktop-app.svg)

- Büyük olasılıkla Linux veya Mac üzerinde çalışan bir .NET Core uygulaması olan taşınabilir bir komut satırı aracı yazarsanız ve bu kimlik doğrulamasını kabul ediyorsanız, etkileşimli kimlik doğrulaması kullanabilirsiniz. .NET Core bir [Web tarayıcısı](https://aka.ms/msal-net-uses-web-browser)sağlamıyor, bu nedenle sistem tarayıcısında kimlik doğrulaması gerçekleşmez. Aksi takdirde, bu durumda en iyi seçenek cihaz kodu akışını kullanmaktır. Bu akış, IoT uygulamaları gibi tarayıcı olmayan uygulamalar için de kullanılır.

  ![Tarayıcısız uygulama](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Özelliklerini

Masaüstü uygulamalarında çok sayıda karmaşıklık vardır. Bunlar, genellikle uygulamanızın etkileşimli kimlik doğrulaması kullanıp kullanmadığına bağlı olarak değişir.

## <a name="recommended-reading"></a>Önerilen okuma

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryonun [uygulama kaydı](scenario-desktop-app-registration.md)olan sonraki makaleye geçin.
