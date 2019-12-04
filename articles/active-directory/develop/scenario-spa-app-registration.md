---
title: Tek sayfalı uygulama (uygulama kaydı)-Microsoft Identity platform
description: Tek sayfalı uygulama oluşturmayı öğrenin (uygulama kaydı)
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ef23b08beeee4cd7e82a5fc33a01b4988ddffb6
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74764704"
---
# <a name="single-page-application-app-registration"></a>Tek sayfalı uygulama: uygulama kaydı

Bu sayfada, tek sayfalı uygulama (SPA) için uygulama kaydı özellikleri açıklanmaktadır.

[Microsoft Identity platformu ile yeni bir uygulama kaydetme](quickstart-register-app.md)ve uygulamanız için desteklenen hesapları seçme adımlarını izleyin. SPA senaryosu, kuruluşunuzdaki hesaplarla veya herhangi bir kuruluş ve kişisel Microsoft hesabında kimlik doğrulamayı destekleyebilir.

Ardından, tek sayfalı uygulamalar için uygulanan uygulama kaydı 'nın belirli yönlerini öğrenin.

## <a name="register-a-redirect-uri"></a>Yeniden yönlendirme URI 'sini kaydetme

Örtük akış, belirteçleri bir Web tarayıcısında çalışan tek sayfalı uygulamaya yeniden yönlendirmeye gönderir. Bu nedenle, uygulamanızın belirteçleri alabileceği bir yeniden yönlendirme URI 'SI kaydedilmesi önemlidir. Yeniden yönlendirme URI 'sinin uygulamanızın URI 'siyle tam olarak eşleştiğinden emin olun.

[Azure Portal](https://go.microsoft.com/fwlink/?linkid=2083908)kayıtlı uygulamanıza gidin. Uygulamanın **kimlik doğrulaması** sayfasında **Web** platformu ' nu seçin. **Yeniden yönlendirme URI 'si** alanına uygulamanız için yeniden yönlendirme URI 'si değerini girin.

## <a name="enable-the-implicit-flow"></a>Örtük akışı etkinleştir

Aynı **kimlik doğrulama** sayfasında, **Gelişmiş ayarlar**' ın altında, **örtük vermeyi**de etkinleştirmeniz gerekir. Uygulamanız yalnızca kullanıcılar için oturum açıp KIMLIK belirteçleri alıyorsanız, **Kimlik belirteçleri** onay kutusunu seçmeniz yeterlidir.

Uygulamanızın API 'Leri çağırmak için erişim belirteçleri alması gerekiyorsa, **erişim belirteçleri** onay kutusunu da seçtiğinizden emin olun. Daha fazla bilgi için bkz. [Kimlik belirteçleri](./id-tokens.md) ve [erişim belirteçleri](./access-tokens.md).

## <a name="api-permissions"></a>API izinleri

Tek sayfalı uygulamalar, oturum açmış kullanıcı adına API 'Leri çağırabilir. Temsilci izinleri istemeleri gerekir. Ayrıntılar için bkz. [Web API 'lerine erişim Izinleri ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-spa-app-configuration.md)
