---
title: Web API 'Leri çağıran bir masaüstü uygulaması oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'Leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin (genel bakış)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 09cc43dec2eff48754f5a6e693badd6bb1907cce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80883010"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran masaüstü uygulaması

Web API 'Lerini çağıran bir masaüstü uygulaması oluşturmak için ihtiyacınız olan her şey hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="get-started"></a>başlarken

Henüz yapmadıysanız, .NET Masaüstü hızlı başlangıcı, Evrensel Windows Platformu (UWP) hızlı başlangıcı veya macOS yerel uygulama hızlı başlangıcı ' nı izleyerek ilk uygulamanızı oluşturun:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir belirteç alma ve bir Windows masaüstü uygulamasından Microsoft Graph API'si çağırma](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Hızlı başlangıç: UWP uygulamasından belirteç alma ve Microsoft Graph API çağırma](./quickstart-v2-uwp.md)

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir macOS yerel uygulamasından belirteç alma ve Microsoft Graph API çağrısı](./quickstart-v2-ios.md)

## <a name="overview"></a>Genel Bakış

Bir masaüstü uygulaması yazar ve kullanıcılara oturum açmak ve Microsoft Graph, diğer Microsoft API 'Leri ya da kendi Web API 'niz gibi Web API 'Lerini çağırmak istiyorsunuz. Çeşitli olanaklar vardır:

- Etkileşimli belirteç alımı kullanabilirsiniz:

  - Masaüstü uygulamanız grafik denetimlerini destekliyorsa (örneğin, bir Windows. form uygulaması, bir WPF uygulaması veya macOS yerel uygulaması).
  - Ya da bir .NET Core uygulaması ise ve Azure Active Directory (Azure AD) ile ilgili kimlik doğrulama etkileşiminin sistem tarayıcısında gerçekleşme olduğunu kabul ediyorsanız.

- Windows barındırılan uygulamalar için, bir Windows etki alanına katılmış bilgisayarlarda çalışan uygulamalar veya Azure AD 'ye katılmış bir belirteci, tümleşik Windows kimlik doğrulaması kullanarak sessizce bir belirteç elde etmek için de mümkündür.
- Son olarak, ancak önerilmese de, genel istemci uygulamalarında bir Kullanıcı adı ve parola kullanabilirsiniz. DevOps gibi bazı senaryolarda hala gereklidir. Bunu kullanarak uygulamanıza kısıtlamalar uygular. Örneğin, çok faktörlü kimlik doğrulaması (koşullu erişim) gerçekleştirmesi gereken bir Kullanıcı oturum açamaz. Ayrıca, uygulamanız çoklu oturum açma özelliğinden (SSO) faydalanır.

  Bu, modern kimlik doğrulaması ilkelerine de karşı geçerlidir ve yalnızca eski nedenlerle sağlanır.

  ![Masaüstü uygulaması](media/scenarios/desktop-app.svg)

- Büyük olasılıkla Linux veya Mac üzerinde çalışan bir .NET Core uygulaması olan taşınabilir bir komut satırı aracı yazarsanız ve bu kimlik doğrulamasını kabul ediyorsanız, etkileşimli kimlik doğrulaması kullanabilirsiniz. .NET Core bir [Web tarayıcısı](https://aka.ms/msal-net-uses-web-browser)sağlamıyor, bu nedenle sistem tarayıcısında kimlik doğrulaması gerçekleşmez. Aksi takdirde, bu durumda en iyi seçenek cihaz kodu akışını kullanmaktır. Bu akış, IoT uygulamaları gibi tarayıcı olmayan uygulamalar için de kullanılır.

  ![Tarayıcısız uygulama](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Özelliklerini

Masaüstü uygulamalarında çok sayıda karmaşıklık vardır. Bunlar, genellikle uygulamanızın etkileşimli kimlik doğrulaması kullanıp kullanmadığına bağlı olarak değişir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması: uygulama kaydı](scenario-desktop-app-registration.md)
