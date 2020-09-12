---
title: JavaScript tek sayfalı uygulama senaryosu-Microsoft Identity platform | Mavisi
description: Microsoft Identity platformunu kullanarak tek sayfalı uygulama (senaryoya genel bakış) oluşturmayı öğrenin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 60e4ca80faa2c8787a13d87ab06cad9243299e50
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89291968"
---
# <a name="scenario-single-page-application"></a>Senaryo: tek sayfalı uygulama

Tek sayfalı uygulama (SPA) oluşturmak için ihtiyacınız olan tüm uygulamaları öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

JavaScript SPA hızlı başlangıcı ' nı izleyerek ilk uygulamanızı oluşturabilirsiniz:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: tek sayfalı uygulama](./quickstart-v2-javascript.md)

## <a name="overview"></a>Genel Bakış

Birçok modern web uygulaması, istemci tarafı tek sayfalı uygulamalar olarak oluşturulmuştur. Geliştiriciler bunları JavaScript veya angular, Vue ve tepki verme gibi bir SPA çerçevesi kullanarak yazar. Bu uygulamalar bir Web tarayıcısında çalışır ve geleneksel sunucu tarafı Web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir. 

Microsoft Identity platformu, tek sayfalı uygulamaların kullanıcılara oturum açmasını ve arka uç hizmetlerine veya Web API 'Lerine erişim belirteçleri almasını sağlamak için **iki** seçenek sunar:

- [OAuth 2,0 yetkilendirme kodu akışı (PKCE ile)](./v2-oauth2-auth-code-flow.md). Yetkilendirme kodu akışı, uygulamanın **kimlik** belirteçleri için bir yetkilendirme kodu gönderip, korunan API 'leri aramak için gereken kimliği doğrulanmış kullanıcı ve **erişim** belirteçlerini temsil etmesine olanak tanır. Buna ek olarak, kullanıcılar adına bu kullanıcılarla etkileşime gerek kalmadan kaynaklara uzun süreli erişim sağlayan **yenileme** belirteçlerini döndürür. Bu, **Önerilen** yaklaşımdır.

![Tek sayfalı uygulamalar-kimlik doğrulama](./media/scenarios/spa-app-auth.svg)

- [OAuth 2,0 örtük akış](./v2-oauth2-implicit-grant-flow.md). Örtük verme akışı, uygulamanın **kimlik** ve **erişim** belirteçleri almasına izin verir. Yetkilendirme kodu akışından farklı olarak, örtük verme akışı bir **yenileme belirteci**döndürmez.

![Tek sayfalı uygulamalar-örtük](./media/scenarios/spa-app.svg)

Bu kimlik doğrulama akışı, elektron ve tepki verme gibi platformlar arası JavaScript çerçeveleri kullanan uygulama senaryoları içermez. Yerel platformlarla etkileşim için daha fazla yetenek gerektirir.

## <a name="specifics"></a>Özelliklerini

Uygulamanız için bu senaryoyu etkinleştirmek üzere şunları yapmanız gerekir:

* Azure Active Directory (Azure AD) ile uygulama kaydı. Kayıt adımları örtük verme akışı ve yetkilendirme kodu akışı arasında farklılık gösterir.
* Uygulama KIMLIĞI gibi kayıtlı uygulama özellikleriyle uygulama yapılandırması.
* Oturum açmak ve belirteçleri almak üzere kimlik doğrulama akışını yapmak için JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı 'nı kullanma.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-spa-app-registration.md)
