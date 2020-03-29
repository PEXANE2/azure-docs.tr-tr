---
title: Web API'leri çağıran bir masaüstü uygulaması oluşturma - Microsoft kimlik platformu | Azure
description: Web API'leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin (genel bakış)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 196102769c1f5f68df316918a63079b09baad32d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76702156"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Senaryo: Web API'lerini çağıran masaüstü uygulaması

Web API'leri çağıran bir masaüstü uygulaması oluşturmak için ihtiyacınız olan her şeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="get-started"></a>Kullanmaya başlayın

Henüz yapmadıysanız, .NET masaüstü hızlı başlatma, Evrensel Windows Platformu (UWP) hızlı başlatma veya macOS yerel uygulaması hızlı başlatım aşağıdaki ilk uygulama oluşturun:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir belirteç alma ve bir Windows masaüstü uygulamasından Microsoft Graph API'si çağırma](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Hızlı başlatma: Bir belirteç edinin ve bir UWP uygulamasından Microsoft Graph API'yi arayın](./quickstart-v2-uwp.md)

> [!div class="nextstepaction"]
> [Hızlı başlatma: Bir belirteç edinin ve macOS yerel uygulamasından Microsoft Graph API'yi arayın](./quickstart-v2-ios.md)

## <a name="overview"></a>Genel Bakış

Bir masaüstü uygulaması yazarsınız ve kullanıcıları uygulamanızda oturum haline getirmek ve Microsoft Graph, diğer Microsoft API'leri veya kendi web API'niz gibi web API'lerini aramak istiyorsunuz. Çeşitli olanaklara sahipsiniz:

- Etkileşimli belirteç edinimi kullanabilirsiniz:

  - Masaüstü uygulamanız grafik denetimleri destekliyorsa( örneğin, windows.form uygulaması, WPF uygulaması veya macOS yerel uygulamasıysa).
  - Veya bir .NET Core uygulamasıysa ve Azure Active Directory (Azure AD) ile kimlik doğrulama etkileşiminin sistem tarayıcısında gerçekleşmesini kabul ediyorsanız.

- Windows barındırılan uygulamalar da, Windows etki alanına veya Azure AD'ye katılan bilgisayarlarda çalışan uygulamaların, Tümleşik Windows Kimlik Doğrulaması'nı kullanarak sessizce bir belirteci elde etmesi de mümkündür.
- Son olarak, ve önerilmese de, ortak istemci uygulamalarında bir kullanıcı adı ve parola kullanabilirsiniz. Hala DevOps gibi bazı senaryolarda ihtiyaç vardır. Bunu kullanmak, uygulamanız üzerinde kısıtlamalar uygular. Örneğin, çok faktörlü kimlik doğrulaması (koşullu erişim) gerçekleştirmesi gereken bir kullanıcı oturum açamaz. Ayrıca, başvurunuz tek oturum açma (SSO) yararlanmaz.

  Aynı zamanda modern kimlik doğrulama ilkelerine aykırı dır ve sadece eski nedenlerle sağlanır.

  ![Masaüstü uygulaması](media/scenarios/desktop-app.svg)

- Taşınabilir bir komut satırı aracı, muhtemelen Linux veya Mac üzerinde çalışan bir .NET Core uygulaması yazarsanız ve kimlik doğrulamanın sistem tarayıcısına devredilemeyeceğini kabul ederseniz, etkileşimli kimlik doğrulamayı kullanabilirsiniz. .NET Core bir [web tarayıcısı](https://aka.ms/msal-net-uses-web-browser)sağlamaz, bu nedenle kimlik doğrulama sistem tarayıcısında gerçekleşir. Aksi takdirde, bu durumda en iyi seçenek aygıt kodu akışını kullanmaktır. Bu akış, Tarayıcısı olmayan uygulamalar için de kullanılır, örneğin IoT uygulamaları.

  ![Tarayıcısız uygulama](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Özellikleri

Masaüstü uygulamalarının bir takım özellikleri vardır. Bunlar çoğunlukla uygulamanızın etkileşimli kimlik doğrulaması kullanıp kullanmadığına bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması: Uygulama kaydı](scenario-desktop-app-registration.md)
