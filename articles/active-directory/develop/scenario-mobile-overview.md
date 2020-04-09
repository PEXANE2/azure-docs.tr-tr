---
title: Web API'lerini çağıran bir mobil uygulama oluşturun | Azure
titleSuffix: Microsoft identity platform | Azure
description: Web API'lerini çağıran bir mobil uygulama oluşturmayı öğrenin (genel bakış)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 1f90f7f23fbdf10b91d8dfc7cd00cca83cd32fbc
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882582"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Senaryo: Web API'lerini çağıran mobil uygulama

Web API'lerini çağıran bir mobil uygulama nın nasıl oluşturulabildiğini öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

İlk mobil uygulamanızı oluşturun ve hızlı bir başlangıç deneyin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Bir belirteç edinin ve bir Android uygulamasından Microsoft Graph API'yi arayın](./quickstart-v2-android.md)
>
> [Hızlı başlatma: Bir iOS uygulamasından bir belirteç edinin ve Microsoft Graph API'yi arayın](./quickstart-v2-ios.md)
>
> [Hızlı başlatma: Bir xamarin iOS ve Android uygulamasından bir belirteç edinin ve Microsoft Graph API'yi arayın](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Genel Bakış

Mobil uygulamalar için kişiselleştirilmiş, sorunsuz bir kullanıcı deneyimi esastır.  Microsoft kimlik platformu, mobil geliştiricilerin iOS ve Android kullanıcıları için bu deneyimi oluşturmasına olanak tanır. Uygulamanız Azure Active Directory (Azure AD) kullanıcıları, kişisel Microsoft hesap kullanıcıları ve Azure AD B2C kullanıcılarında oturum açabilir. Ayrıca, onlar adına bir web API aramak için belirteçleri edinebilirsiniz. Bu akışları uygulamak için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanırız. MSAL endüstri standardı [OAuth2.0 yetki kodu akışını](v2-oauth2-auth-code-flow.md)uygular.

![Daemon uygulamaları](./media/scenarios/mobile-app.svg)

Mobil uygulamalar için dikkat edilecek noktalar:

- **Kullanıcı deneyimi önemlidir**: Oturum açma talebinde bulunan dan önce kullanıcıların uygulamanızın değerini görmesine izin verin. Yalnızca gerekli izinleri isteyin.
- **Tüm kullanıcı yapılandırmalarını destekleyin:** Birçok mobil işletme kullanıcısı koşullu erişim ilkelerine ve cihaz uyumluluğu ilkelerine uymalıdır. Bu önemli senaryoları desteklediğinden emin olun.
- **Tek oturum açma (SSO) uygula**: MSAL ve Microsoft kimlik platformlarını kullanarak, cihazın tarayıcısı veya Microsoft Authenticator (ve Android'deki Intune Company Portal) aracılığıyla tek oturum açmayı etkinleştirebilirsiniz.
- **Paylaşılan cihaz modunu uygulayın**: Uygulamanızın hastaneler, üretim, perakende ve finans gibi paylaşılan cihaz senaryolarında kullanılmasını etkinleştirin. [Paylaşılan aygıt modunu destekleme hakkında daha fazla bilgi edinin.](msal-shared-devices.md)

## <a name="specifics"></a>Özellikleri

Microsoft kimlik platformunda bir mobil uygulama oluştururken aşağıdaki hususları göz önünde bulundurun:

- Platforma bağlı olarak, kullanıcıların ilk oturum açaması için bazı kullanıcı etkileşimi gerekebilir. Örneğin, iOS, uygulamaların SSO'yu ilk kez Microsoft Authenticator (ve Android'deki Intune Company Portal) aracılığıyla kullandıklarında kullanıcı etkileşimini göstermesini gerektirir.
- iOS ve Android'de, MSAL kullanıcıları oturum açmak için harici bir tarayıcı kullanabilir. Uygulamanızın üstünde harici tarayıcı görünebilir.
- Mobil uygulamada asla bir sır kullanmayın. Bu uygulamalarda, sırlar tüm kullanıcılar tarafından erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-mobile-app-registration.md)
