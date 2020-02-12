---
title: Web API 'Lerini çağıran bir mobil uygulama oluşturun | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Leri çağıran bir mobil uygulama oluşturmayı öğrenin (genel bakış)
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
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 6675d67299091325fcc3e12572a906716bf5b88d
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132429"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran mobil uygulama

Web API 'Lerini çağıran bir mobil uygulama oluşturmayı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

İlk mobil uygulamanızı oluşturun ve hızlı başlangıç yapın.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir Android uygulamasından belirteç alma ve Microsoft Graph API çağırma](./quickstart-v2-android.md)
>
> [Hızlı başlangıç: bir iOS uygulamasından belirteç alma ve Microsoft Graph API çağırma](./quickstart-v2-ios.md)
>
> [Hızlı başlangıç: bir Xamarin iOS ve Android uygulamasından belirteç alma ve Microsoft Graph API çağrısı](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Genel Bakış

Kişiselleştirilmiş, sorunsuz bir kullanıcı deneyimi, mobil uygulamalar için gereklidir.  Microsoft Identity platform, mobil geliştiricilerin iOS ve Android kullanıcıları için bu deneyimi oluşturmalarına olanak sağlar. Uygulamanız Azure Active Directory (Azure AD) kullanıcıları, kişisel Microsoft hesabı kullanıcıları ve Azure AD B2C kullanıcıları için oturum açabilir. Ayrıca, bir Web API 'sini adına çağırmak için belirteçleri de alabilir. Bu akışları uygulamak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanacağız. MSAL, sektör standardı [OAuth 2.0 yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md)uygular.

![Daemon uygulamaları](./media/scenarios/mobile-app.svg)

Mobil uygulamalar için dikkat edilmesi gerekenler:

- **Kullanıcı deneyimi anahtar**: oturum açmadan önce kullanıcıların uygulamanızın değerini görmesine izin verin. Yalnızca gerekli izinleri iste.
- **Tüm Kullanıcı yapılandırmalarının desteklenmesi**: birçok mobil iş kullanıcısının koşullu erişim ilkelerine ve cihaz uyumluluk ilkelerine uyması gerekir. Bu anahtar senaryolarını desteklediğinizden emin olun.
- **Çoklu oturum açma (SSO) uygulama**: msal ve Microsoft Identity platformunu kullanarak, cihazın tarayıcısı veya Microsoft Authenticator (ve Android üzerinde Intune şirket portalı) aracılığıyla çoklu oturum açmayı etkinleştirebilirsiniz.

## <a name="specifics"></a>Özelliklerini

Microsoft Identity platformunda bir mobil uygulama oluştururken aşağıdaki noktalara dikkat edin:

- Platforma bağlı olarak, kullanıcılar ilk kez oturum açtığında bazı Kullanıcı etkileşimi gerekebilir. Örneğin iOS, Microsoft Authenticator (ve Android üzerinde Intune Şirket Portalı) ile ilk kez SSO kullandıklarında, uygulamaların kullanıcı etkileşimini göstermesini gerektirir.
- İOS ve Android 'de MSAL, kullanıcıların oturum açması için bir dış tarayıcı kullanabilir. Dış tarayıcı, uygulamanızın üst kısmında görünebilir. Bunun yerine uygulama içi Web görünümlerini kullanmak için yapılandırmayı özelleştirebilirsiniz.
- Bir mobil uygulamada hiç gizli anahtar kullanmayın. Bu uygulamalarda, gizli dizileri tüm kullanıcılar tarafından erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-mobile-app-registration.md)
