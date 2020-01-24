---
title: Uygulama çoklu oturum açmayı yapılandırma | Microsoft Docs
description: Azure AD ile geliştirdiğiniz ve kayıt yaptığınız özel bir uygulama için çoklu oturum açmayı yapılandırma.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ms.openlocfilehash: bb77f376e22428e9259ff3efc84cf6f1cb3491fe
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702650"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Bir uygulama için çoklu oturum açmayı yapılandırma

OpenID Connect, SAML 2,0 veya WS-beslemelerine yönelik Azure AD aracılığıyla federasyona katılarak, uygulamanızda federe çoklu oturum açmayı (SSO) etkinleştirme işlemi otomatik olarak etkinleştirilir. Azure AD 'de zaten mevcut bir oturuma sahip olsa da son kullanıcılarınızın oturum açması gerekiyorsa, uygulamanız yanlış yapılandırılmış olabilir.

* ADAL/MSAL kullanıyorsanız, **Promptbehavior** ' ın **her zaman**yerine **Auto** olarak ayarlandığından emin olun.

* Bir mobil uygulama oluşturuyorsanız, aracılı veya aracılı olmayan SSO 'yu etkinleştirmek için ek yapılandırmalara ihtiyacınız olabilir.

Android için bkz. [Android 'de uygulamalar arası SSO 'Yu etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

İOS için bkz. [iOS 'Ta uygulamalar arası SSO etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD SSO 'SU](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Android 'de uygulamalar arası SSO 'yu etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[İOS 'ta çapraz uygulama SSO 'SU etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Uygulamaları AzureAD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[AzureAD v 2.0 yakınsanmış uygulamalar için onay ve Izinler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
