---
title: JavaScript tek sayfalık uygulama senaryosu - Microsoft kimlik platformu | Azure
description: Microsoft kimlik platformlarını kullanarak tek sayfalık bir uygulamayı (senaryoya genel bakış) nasıl oluşturabilirsiniz öğrenin.
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
ms.openlocfilehash: 3ead0ea58c6860519f027eb6a7450df37396bd89
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885200"
---
# <a name="scenario-single-page-application"></a>Senaryo: Tek sayfalık uygulama

Tek sayfalık bir uygulama (SPA) oluşturmak için ihtiyacınız olan her şeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

JavaScript SPA hızlı başlatınızı izleyerek ilk uygulamanızı oluşturabilirsiniz:

> [!div class="nextstepaction"]
> [Quickstart: Tek sayfauygulaması](./quickstart-v2-javascript.md)

## <a name="overview"></a>Genel Bakış

Birçok modern web uygulamaları istemci tarafı tek sayfauygulamaları olarak oluşturulur. Geliştiriciler JavaScript veya Açısal, Vue.js ve React.js gibi bir SPA çerçevesi kullanarak yazmak. Bu uygulamalar bir web tarayıcısı üzerinde çalışır ve geleneksel sunucu tarafındaki web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir. 

Microsoft kimlik platformu, tek sayfalı uygulamaların kullanıcıları oturum açmasına ve [OAuth 2.0 örtük akışını](./v2-oauth2-implicit-grant-flow.md)kullanarak arka uç hizmetlerine veya web API'lerine erişebilmesini sağlar. Örtük akış, uygulamanın kimlik bilgilerini kimlik doğrulaması kullanıcıyı temsil etmesini ve korumalı API'leri aramak için gereken belirteçlere erişmesine olanak tanır.

![Tek sayfauygulamaları](./media/scenarios/spa-app.svg)

Bu kimlik doğrulama akışı, Elektron ve React-Native gibi platformlar arası JavaScript çerçevelerini kullanan uygulama senaryolarını içermez. Onlar yerli platformlar ile etkileşim için daha fazla yetenek gerektirir.

## <a name="specifics"></a>Özellikleri

Uygulamanız için bu senaryoyu etkinleştirmek için şunları yapmanız gerekir:

* Azure Active Directory (Azure AD) ile uygulama kaydı. Bu kayıt, örtük akışı etkinleştirme ve belirteçleri döndürülen bir yeniden yönlendirme URI ayarlama içerir.
* Uygulama kimliği gibi kayıtlı uygulama özellikleriyle uygulama yapılandırması.
* Oturum açma ve belirteçleri elde etmek için kimlik doğrulama akışını yapmak için Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanma.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-spa-app-registration.md)
