---
title: Uygulama çoklu oturum açmayı yapılandırma
description: Azure AD ile geliştirdiğiniz ve kayıt yaptığınız özel bir uygulama için çoklu oturum açmayı yapılandırma.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.openlocfilehash: 465adbb71abaa45160399ecba2ebfb692a8307c2
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88120686"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Bir uygulama için çoklu oturum açmayı yapılandırma

OpenID Connect, SAML 2,0 veya WS-beslemelerine yönelik Azure AD aracılığıyla federasyona katılarak, uygulamanızda federe çoklu oturum açmayı (SSO) etkinleştirme işlemi otomatik olarak etkinleştirilir. Azure AD 'de zaten mevcut bir oturuma sahip olsa da son kullanıcılarınızın oturum açması gerekiyorsa, uygulamanız yanlış yapılandırılmış olabilir.

* ADAL/MSAL kullanıyorsanız, **Promptbehavior** ' ın **her zaman**yerine **Auto** olarak ayarlandığından emin olun.

* Bir mobil uygulama oluşturuyorsanız, aracılı veya aracılı olmayan SSO 'yu etkinleştirmek için ek yapılandırmalara ihtiyacınız olabilir.

Android için bkz. [Android 'de uygulamalar arası SSO 'Yu etkinleştirme](../azuread-dev/howto-v1-enable-sso-android.md).<br>

İOS için bkz. [iOS 'Ta uygulamalar arası SSO etkinleştirme](../azuread-dev/howto-v1-enable-sso-ios.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD SSO 'SU](../manage-apps/what-is-single-sign-on.md)<br>

[Android 'de uygulamalar arası SSO 'yu etkinleştirme](../azuread-dev/howto-v1-enable-sso-android.md)<br>

[İOS 'ta çapraz uygulama SSO 'SU etkinleştirme](../azuread-dev/howto-v1-enable-sso-ios.md)<br>

[Uygulamaları AzureAD ile tümleştirme](./quickstart-register-app.md)<br>

[Microsoft kimlik platformu uç noktasında izinler ve onay](./v2-permissions-and-consent.md)<br>

[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)