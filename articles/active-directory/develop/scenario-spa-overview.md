---
title: JavaScript tek sayfalı uygulama senaryosu-Microsoft Identity platform | Mavisi
description: Microsoft Identity platformunu kullanarak tek sayfalı uygulama (senaryoya genel bakış) oluşturmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: b430778bed811656b5c8aadc75ba3cf35917f737
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701884"
---
# <a name="scenario-single-page-application"></a>Senaryo: tek sayfalı uygulama

Tek sayfalı uygulama (SPA) oluşturmak için ihtiyacınız olan tüm uygulamaları öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlangıç

JavaScript SPA hızlı başlangıcı ' nı izleyerek ilk uygulamanızı oluşturabilirsiniz:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: tek sayfalı uygulama](./quickstart-v2-javascript.md)

## <a name="overview"></a>Genel Bakış

Birçok modern web uygulaması, istemci tarafı tek sayfalı uygulamalar olarak oluşturulmuştur. Geliştiriciler onları JavaScript veya angular, Vue. js ve yanıt verme. js gibi bir SPA çerçevesi kullanarak yazar. Bu uygulamalar bir Web tarayıcısında çalışır ve geleneksel sunucu tarafı Web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir. 

Microsoft Identity platformu, tek sayfalı uygulamaların kullanıcılara oturum açmasını ve [OAuth 2,0 örtük akışını](./v2-oauth2-implicit-grant-flow.md)kullanarak arka uç hizmetlerine veya Web API 'lerine erişim belirteçleri almasını sağlar. Örtük akış, uygulamanın kimliği doğrulanmış kullanıcıyı temsil etmesi için kimlik belirteçleri almasına izin verir ve ayrıca korumalı API 'Leri çağırmak için gereken belirteçleri de erişin.

![Tek sayfalı uygulamalar](./media/scenarios/spa-app.svg)

Bu kimlik doğrulama akışı, elektron ve tepki verme gibi platformlar arası JavaScript çerçeveleri kullanan uygulama senaryoları içermez. Yerel platformlarla etkileşim için daha fazla yetenek gerektirir.

## <a name="specifics"></a>Özelliklerini

Uygulamanız için bu senaryoyu etkinleştirmek üzere şunları yapmanız gerekir:

* Azure Active Directory (Azure AD) ile uygulama kaydı. Bu kayıt, örtük akışı etkinleştirmeyi ve belirteçlerin döndürüldüğü yeniden yönlendirme URI 'sini ayarlamayı içerir.
* Uygulama KIMLIĞI gibi kayıtlı uygulama özellikleriyle uygulama yapılandırması.
* Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanarak oturum açma ve belirteçleri alma için kimlik doğrulama akışını yapın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-spa-app-registration.md)
