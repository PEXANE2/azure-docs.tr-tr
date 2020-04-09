---
title: Tek sayfalı uygulamaları kaydedin - Microsoft kimlik platformu | Azure
description: Tek sayfalık bir uygulama oluşturmayı öğrenin (uygulama kaydı)
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 6f690a8b3436a45d434ccad2bbaa7d2a1b0b76aa
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882157"
---
# <a name="single-page-application-app-registration"></a>Tek sayfalık uygulama: Uygulama kaydı

Bu sayfa, tek sayfalık bir uygulamanın (SPA) uygulama kaydı özelliklerini açıklar.

Microsoft kimlik [platformuna yeni bir uygulama kaydetmek](quickstart-register-app.md)için adımları izleyin ve uygulamanız için desteklenen hesapları seçin. SPA senaryosu, kuruluşunuzdaki veya herhangi bir kuruluştaki ve kişisel Microsoft hesaplarındaki hesaplarla kimlik doğrulamayı destekleyebilir.

Ardından, tek sayfalı uygulamalar için geçerli olan uygulama kaydının belirli yönlerini öğrenin.

## <a name="register-a-redirect-uri"></a>Yeniden yönlendirme URI'yi kaydedin

Örtük akış, bir web tarayıcısında çalışan tek sayfalık uygulamaya yönlendirmede belirteçleri gönderir. Bu nedenle, başvurunuzun jetonları alabileceği bir yeniden yönlendirme URI'si kaydetmeniz önemlidir. Yeniden yönlendirme URI'nin uygulamanız için URI ile tam olarak eşleştiğinden emin olun.

Azure [portalında](https://go.microsoft.com/fwlink/?linkid=2083908)kayıtlı uygulamanıza gidin. Uygulamanın **Kimlik Doğrulama** sayfasında **Web** platformunu seçin. **Uygulamanız** için yeniden yönlendirme URI değerini yeniden yönlendirme URI alanında girin.

## <a name="enable-the-implicit-flow"></a>Örtük akışı etkinleştirme

Aynı **Kimlik Doğrulama** sayfasında, **Gelişmiş ayarlar**altında, **örtülü hibeyi**de etkinleştirmelisiniz. Uygulamanız yalnızca kullanıcılarda oturum açıyor ve kimlik belirteçleri alıyorsa, **kimlik belirteçleri** onay kutusunu seçmek yeterlidir.

Uygulamanızın API'leri aramak için erişim belirteçleri alması gerekiyorsa, **Access belirteçleri** onay kutusunu da seçtiğinizden emin olun. Daha fazla bilgi için [kimlik belirteçleri](./id-tokens.md) ve [Erişim belirteçleri'ne](./access-tokens.md)bakın.

## <a name="api-permissions"></a>API izinleri

Tek sayfalı uygulamalar oturum açmış kullanıcı adına API'leri arayabilir. Temsilci lik izni istemeleri gerekir. Ayrıntılar için web [API'lerine erişmek için izin ekle'ye](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-spa-app-configuration.md)
