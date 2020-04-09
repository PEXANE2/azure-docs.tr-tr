---
title: Uygulamayı tek oturum açma yapılandırma
description: Geliştirdiğiniz ve Azure AD'ye kaydolduğunuz özel bir uygulama için tek oturum açma yapılandırma sı.
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
ms.openlocfilehash: 630be1676d73410f1b0d376b163a8599299ddbe2
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883160"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>Bir uygulama için tek oturum açma nasıl yapılandırılabilen

OpenID Connect, SAML 2.0 veya WS-Fed için Azure AD aracılığıyla federe tek oturum açma (SSO) etkinleştirme otomatik olarak etkinleştirilir. Azure AD'de zaten varolan bir oturuma sahip olmasına rağmen son kullanıcılarınız oturum vermek zorunda ysa, uygulamanız yanlış yapılandırılmış olabilir.

* ADAL/MSAL kullanıyorsanız, **Her Zaman** **yerine** Otomatik Olarak **PromptBehavior** ayarlı olduğundan emin olun.

* Bir mobil uygulama oluşturuyorsanız, aracılı veya aracısız SSO'ları etkinleştirmek için ek yapılandırmalara ihtiyacınız olabilir.

Android için Bkz. [Android'de Çapraz Uygulama SSO'su etkinleştirme.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

iOS için bkz: [iOS'ta Çapraz Uygulama SSO'su etkinleştirme.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Android'de Çapraz Uygulama SSO'su etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[iOS'ta Çapraz Uygulama SSO'su etkinleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Uygulamaları AzureAD'a Entegre Etme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[AzureAD v2.0 yakınsanan Uygulamalar için Onay ve İzin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
