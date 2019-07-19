---
title: Web API 'Lerini çağıran mobil uygulama-uygulamanın kod yapılandırması | Microsoft Identity platformu
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir mobil uygulama oluşturmayı öğrenin
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f43ae9da51f68c9765a36d27c993d1c9935d61fa
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326118"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Web API 'Lerini çağıran mobil uygulama-uygulama kaydı

Bu makale, bir mobil uygulama oluşturmaya yönelik uygulama kayıt yönergelerini içerir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Mobil uygulamalarda desteklenen hesap türleri etkinleştirmek istediğiniz deneyime ve uygulamanızın hedeflediği kullanıcılara bağlıdır.

## <a name="platform-configuration-and-redirect-uris"></a>Platform yapılandırma ve yeniden yönlendirme URI 'Leri  

Bir mobil uygulama oluştururken, en kritik kayıt adımı yeniden yönlendirme URI 'sidir. Bu, [kimlik doğrulama dikey penceresindeki platform yapılandırması](https://aka.ms/MobileAppReg)aracılığıyla ayarlanabilir.

Bu deneyim, uygulamanızın Microsoft Authenticator (ve Android 'de Intune Şirket Portalı) aracılığıyla çoklu oturum açma (SSO) ve cihaz yönetim ilkelerini destekleme olanağı sağlar.

Yeniden yönlendirme URI 'sini el ile yapılandırmayı tercih ediyorsanız, bunu uygulama bildirimi aracılığıyla yapabilirsiniz. Önerilen biçim aşağıda verilmiştir:

- ***iOS***:`msauth.<BUNDLE_ID>://auth`
- ***Android***:`msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Android imza karması, anahtar aracı komutu aracılığıyla sürüm veya hata ayıklama anahtarları kullanılarak oluşturulabilir.

## <a name="api-permissions"></a>API izinleri

Mobil uygulamalar, oturum açmış kullanıcı adına API 'Leri çağırır. Uygulamanızın, kapsam olarak da adlandırılan, temsilci izinleri istemesi gerekir. İstenen deneyime bağlı olarak, bu Azure portal statik olarak veya çalışma zamanında dinamik olarak yapılabilir. Statik olarak kayıt izinleri, yöneticilerin uygulamanızı kolayca onaylamasını sağlar ve önerilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Belirteç alma](scenario-mobile-acquire-token.md)
