---
title: Microsoft Identity platform ile uygulama oturum açma akışı | Mavisi
titleSuffix: Microsoft identity platform
description: Web, masaüstü ve mobil uygulamaların Microsoft Identity platformunda (v 2.0) oturum açma akışı hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/18/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: af5b27dc85a276c731a61135ab59ab81f5aaf3c2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83772208"
---
# <a name="app-sign-in-flow-with-microsoft-identity-platform"></a>Microsoft Identity platform ile uygulama oturum açma akışı

Bu konuda, Microsoft Identity platform kullanılarak Web, masaüstü ve mobil uygulamalar için temel oturum açma akışı ele alınmaktadır. Microsoft Identity platform tarafından desteklenen oturum açma senaryoları hakkında bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) .

## <a name="web-app-sign-in-flow"></a>Web uygulaması oturum açma akışı

Bir kullanıcı tarayıcıda bir Web uygulamasına gittiğinde aşağıdakiler olur:

* Web uygulaması, kullanıcının kimlik doğrulamasının yapılıp yapılmayacağını belirler.
* Kullanıcının kimliği doğrulanmadıysa, Web uygulaması Kullanıcı oturumu açmak için Azure AD 'ye temsilciler atar. Bu oturum açma, kuruluşun ilkesi ile uyumlu olacaktır. Bu, kullanıcıdan [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) (bazen iki öğeli kimlik doğrulama veya 2FA olarak adlandırılır) veya hiç parola (örneğin, Windows Hello) kullanarak kimlik bilgilerini girmesini isteyen bir sorun olabilir.
* Kullanıcıdan, istemci uygulamasına gereken erişimi onaylaması istenir. Bu nedenle, Microsoft Identity platform kullanıcının tarafından onaylanan erişimi temsil eden belirteçleri sunabilmesi için istemci uygulamalarının Azure AD 'ye kaydedilmesi gerekir.

Kullanıcının kimliği başarıyla doğrulandı:

* Microsoft Identity platform, Web uygulamasına bir belirteç gönderir.
* Kullanıcının tarayıcı tanımlama bilgisi jar içindeki kimliğini içeren Azure AD 'nin etki alanı ile ilişkili bir tanımlama bilgisi kaydedilir. Bir uygulama Microsoft Identity platform yetkilendirme uç noktasına gitmek için tarayıcıyı bir sonraki sefer kullandığında, tarayıcı tanımlama bilgisini Kullanıcı yeniden oturum açmak zorunda kalmayacak şekilde gösterir. Bu, SSO 'nun elde edilmesi için de bir yoldur. Tanımlama bilgisi Azure AD tarafından üretilir ve yalnızca Azure AD tarafından anlaşılabilirler.
* Web uygulaması, belirteci doğrular. Doğrulama başarılı olursa, Web uygulaması korumalı sayfayı görüntüler ve tarayıcının tanımlama bilgisi jar 'e bir oturum tanımlama bilgisi kaydeder. Kullanıcı başka bir sayfaya gittiğinde, Web uygulaması, oturum tanımlama bilgisine göre kullanıcının kimliğinin doğrulandığını bilir.

Aşağıdaki sıra diyagramı bu etkileşimi özetler:

![Web uygulaması kimlik doğrulama işlemi](media/authentication-scenarios/web-app-how-it-appears-to-be.png)

### <a name="how-a-web-app-determines-if-the-user-is-authenticated"></a>Web uygulaması, kullanıcının kimliğinin doğrulanmadığını nasıl belirler

Web uygulaması geliştiricileri, kimlik doğrulaması gerektirdiğini veya yalnızca belirli sayfaların olduğunu belirtebilir. Örneğin, ASP.NET/ASP.NET Core 'da, `[Authorize]` Denetleyici eylemlerine özniteliği eklenerek yapılır.

Bu öznitelik, ASP.NET Kullanıcı kimliğini içeren bir oturum tanımlama bilgisinin varlığını denetlamasına neden olur. Bir tanımlama bilgisi yoksa, ASP.NET kimlik doğrulamasını belirtilen kimlik sağlayıcısına yönlendirir. Kimlik sağlayıcısı Azure AD ise, Web uygulaması, `https://login.microsoftonline.com` oturum açma iletişim kutusunu gösteren kimlik doğrulamasını yeniden yönlendirir.

### <a name="how-a-web-app-delegates-sign-in-to-microsoft-identity-platform-and-obtains-a-token"></a>Web uygulamasının Microsoft Identity platformunda oturum açması ve bir belirteç alacağı

Tarayıcı aracılığıyla Kullanıcı kimlik doğrulaması gerçekleşir. OpenID Protokolü standart HTTP protokol iletilerini kullanır.

* Web uygulaması, Microsoft Identity platform 'ı kullanmak için tarayıcıya bir HTTP 302 (Redirect) gönderir.
* Kullanıcının kimliği doğrulandığında, Microsoft Identity platform tarayıcı aracılığıyla yeniden yönlendirme kullanarak belirteci Web uygulamasına gönderir.
* Yeniden yönlendirme, Web uygulaması tarafından yeniden yönlendirme URI 'SI biçiminde sağlanır. Bu yeniden yönlendirme URI 'SI, Azure AD uygulama nesnesi ile kaydedilir. Uygulama çeşitli URL 'Lerde dağıtılabilmesi için birkaç yeniden yönlendirme URI 'si olabilir. Bu nedenle, Web uygulamasının kullanılacak yeniden yönlendirme URI 'sini belirtmesi de gerekecektir.
* Azure AD, Web uygulaması tarafından gönderilen yeniden yönlendirme URI 'sinin, uygulamanın kayıtlı yeniden yönlendirme URI 'lerinden biri olduğunu doğrular.

## <a name="desktop-and-mobile-app-sign-in-flow"></a>Masaüstü ve mobil uygulama oturum açma akışı

Yukarıda açıklanan akış, küçük farklılıklar ile masaüstü ve mobil uygulamalar için geçerlidir.

Masaüstü ve mobil uygulamalar, kimlik doğrulaması için katıştırılmış bir Web denetimi veya bir sistem tarayıcısı kullanabilir. Aşağıdaki diyagramda, bir masaüstü veya mobil uygulamanın, erişim belirteçleri almak ve Web API 'Lerini çağırmak için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) nasıl kullandığı gösterilmektedir.

![Masaüstü uygulaması nasıl görünür](media/authentication-scenarios/desktop-app-how-it-appears-to-be.png)

MSAL belirteçleri almak için bir tarayıcı kullanır. Web uygulamalarında olduğu gibi, kimlik doğrulaması da Microsoft Identity platformu 'na atanmış olur.

Azure AD, Web Apps için olduğu gibi tarayıcıda aynı kimlik tanımlama bilgisini kaydettiği için, yerel veya mobil uygulama sistem tarayıcısını kullanıyorsa, karşılık gelen Web uygulamasıyla SSO 'yu hemen alır.

Varsayılan olarak, MSAL sistem tarayıcısını kullanır. Bu özel durum, daha tümleşik bir kullanıcı deneyimi sağlamak için gömülü bir denetimin kullanıldığı masaüstü uygulamalarının .NET Framework.

## <a name="next-steps"></a>Sonraki adımlar

Kimlik doğrulama ve yetkilendirme temellerini kapsayan diğer konular için:

* Microsoft Identity platform 'da kimlik doğrulaması ve yetkilendirmenin temel kavramları hakkında bilgi edinmek için bkz [. kimlik doğrulaması ve yetkilendirme karşılaştırması](authentication-vs-authorization.md) .
* Kimlik doğrulama ve yetkilendirme için erişim belirteçlerinin, yenileme belirteçlerinin ve KIMLIK belirteçlerinin nasıl kullanıldığını öğrenmek için [güvenlik belirteçlerine](security-tokens.md) bakın.
* Uygulamanızı Microsoft Identity platform ile tümleştirilebilen şekilde kaydetme süreci hakkında bilgi edinmek için bkz. [uygulama modeli](application-model.md) .

Uygulama oturum açma akışı hakkında daha fazla bilgi edinmek için:

* Microsoft Identity platform tarafından desteklenen kullanıcıların kimliğini doğrulamak için diğer senaryolar hakkında daha fazla bilgi edinmek için bkz. [kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md) .
* Microsoft hesapları, Azure AD hesapları ve Azure AD B2C kullanıcıları tek ve kolaylaştırılmış bir programlama modelinde çalışan uygulamalar geliştirmenize yardımcı olan Microsoft kitaplıkları hakkında bilgi edinmek için bkz. [msal kitaplıkları](msal-overview.md) .
