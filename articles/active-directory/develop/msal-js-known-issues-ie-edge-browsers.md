---
title: Microsoft Edge (MSAL.js) & Internet Explorer sorunları | Azure
titleSuffix: Microsoft identity platform
description: Internet Explorer ve Microsoft Edge tarayıcılarıyla JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanırken sorunları öğrenin.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 5ae2dee68ec0da8e8a00d4f01583461462bc196c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696104"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-msaljs"></a>Internet Explorer ve Microsoft Edge tarayıcılarında bilinen sorunlar (MSAL.js)

## <a name="issues-due-to-security-zones"></a>Güvenlik bölgeleri nedeniyle sorunlar
IE ve Microsoft Edge'de kimlik doğrulamayla ilgili birden çok sorun bildirmiştik *(Microsoft Edge tarayıcı sürümünün 40.15063.0.0*olarak güncellenmelerinden bu yana). Bunları takip ediyoruz ve Microsoft Edge ekibini bilgilendirdik. Microsoft Edge bir çözüm üzerinde çalışırken, sık sık oluşan sorunların ve uygulanabilecek olası geçici çözümlerin açıklaması aşağıda verilmiştir.

### <a name="cause"></a>Nedeni
Bu sorunların çoğu için nedeni aşağıdaki gibidir. Oturum depolama ve yerel depolama Microsoft Edge tarayıcısında güvenlik bölgeleri tarafından bölümlenir. Microsoft Edge'in bu özel sürümünde, uygulama bölgelere yönlendirildiğinde, oturum depolama alanı ve yerel depolama temizlenir. Özellikle, oturum depolama normal tarayıcı navigasyon temizlenir ve hem oturum ve yerel depolama tarayıcının InPrivate modunda temizlenir. MSAL.js oturum depolamasında belirli durumu kaydeder ve kimlik doğrulama akışları sırasında bu durumu denetlemeye dayanır. Oturum depolama temizlendiğinde, bu durum kaybolur ve bu nedenle kırık deneyimler sonuçlanır.

### <a name="issues"></a>Sorunlar

- **Kimlik doğrulama sırasında sonsuz yönlendirme döngüleri ve sayfa yeniden yükler.** Kullanıcılar Microsoft Edge'deki uygulamada oturum açtıklarında, AAD oturum açma sayfasından geri yönlendirilir ler ve tekrarlanan sayfa yeniden yüklemeleri ile sonuçlanan sonsuz bir yönlendirme döngüsüne takılıp kalırlar. Bu genellikle oturum depolama `invalid_state` bir hata eşlik ediyor.

- **Sonsuz edinme belirteç döngüleri ve AADSTS50058 hatası.** Microsoft Edge'de çalışan bir uygulama bir kaynak için belirteç elde etmeye çalıştığında, uygulama, ağınızdaAAD'den gelen aşağıdaki hatayla birlikte edinme belirteci çağrısının sonsuz bir döngüsüne takılabilir:

    `Error :login_required; Error description:AADSTS50058: A silent sign-in request was sent but no user is signed in. The cookies used to represent the user's session were not sent in the request to Azure AD. This can happen if the user is using Internet Explorer or Edge, and the web app sending the silent sign-in request is in different IE security zone than the Azure AD endpoint (login.microsoftonline.com)`

- **Pop-up penceresi kapanmaz veya kimlik doğrulaması için Popup üzerinden giriş kullanırken takılıp kalır.** Microsoft Edge veya IE(InPrivate)'daki açılır pencereden kimlik bilgileri girerken ve oturum açtıktan sonra, gezintiye güvenlik bölgeleri arasında birden çok etki alanı dahilse, açılır pencere kapanmaz, çünkü MSAL.js tutamacı nı kaybeder açılır pencere.  

### <a name="update-fix-available-in-msaljs-023"></a>Güncelleme: Düzeltme MSAL.js 0.2.3 mevcuttur
Kimlik doğrulama yönlendirme döngü sorunları için [düzeltmeler MSAL.js 0.2.3](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)serbest bırakıldı. Bu düzeltmeden yararlanmak için MSAL.js config'deki bayrağı `storeAuthStateInCookie` etkinleştirin. Varsayılan olarak bu bayrak false olarak ayarlanır.

`storeAuthStateInCookie` Bayrak etkinleştirildiğinde, MSAL.js auth akışlarının doğrulanması için gerekli istek durumunu depolamak için tarayıcı çerezlerini kullanır.

> [!NOTE]
> Bu düzeltme henüz msal-açısal ve msal-açılar sarmalayıcılar için mevcut değildir. Bu düzeltme, Pop-up pencereleriyle ilgili sorunu gidermez.

Aşağıdaki geçici işleri kullanın.

#### <a name="other-workarounds"></a>Diğer geçici geçici işler
Sorununuzun yalnızca Microsoft Edge tarayıcısının belirli sürümünde oluştuğunu ve bu geçici çözümlerini benimsemeden önce diğer tarayıcılarda çalıştığını test edin.  
1. Bu sorunları aşmak için ilk adım olarak, uygulama etki alanı nın ve kimlik doğrulama akışının yeniden yönlendirilmesinde yer alan diğer sitelerin tarayıcının güvenlik ayarlarında güvenilir siteler olarak eklenmelerini ve böylece aynı güvenlik bölgesine ait olmalarını sağlayın.
Bunu yapmak için şu adımları uygulayın:
    - **Internet Explorer'ı** açın ve sağ üst köşedeki **ayarlara** (vites simgesi) tıklayın
    - **Internet Seçeneklerini** Seçin
    - **Güvenlik** sekmesini seçin
    - Güvenilir **Siteler** seçeneğinin altında, **siteler** düğmesine tıklayın ve açılan iletişim kutusuna URL'leri ekleyin.

2. Daha önce de belirtildiği gibi, normal navigasyon sırasında yalnızca oturum depolama temizlenir, bunun yerine yerel depolama kullanmak için MSAL.js yapılandırabilirsiniz. Bu, MSAL'ı başlatma sırasında `cacheLocation` config parametresi olarak ayarlanabilir.

Hem oturum hem de yerel depolama temizlendiğinden, bu durum InPrivate tarama sorununu çözmez.

## <a name="issues-due-to-popup-blockers"></a>Açılır pencere engelleyicileri nedeniyle sorunlar

Açılır pencerelerin IE veya Microsoft Edge'de engellendiğinde, örneğin çok faktörlü kimlik doğrulama sırasında ikinci bir açılır pencere oluştuğunda durumlar vardır. Bir kez veya her zaman açılır pencere için izin vermek için tarayıcıda bir uyarı alırsınız. İzin vermeyi seçerseniz, tarayıcı açılır pencereyi otomatik olarak `null` açar ve bunun için bir tutamacı döndürür. Sonuç olarak, kitaplık pencere için bir tutamak yok ve açılır pencereyi kapatmak için bir yol yoktur. Açılır pencereyi otomatik olarak açmadığından, açılır pencerelere izin vermenizi istediğinde aynı sorun Chrome'da da olmaz.

Geçici **çözüm**olarak, geliştiricilerin bu sorunu önlemek için uygulamalarını kullanmaya başlamadan önce IE ve Microsoft Edge'deki açılır pencerelere izin vermeleri gerekir.

## <a name="next-steps"></a>Sonraki adımlar
[Internet Explorer'da MSAL.js kullanma](msal-js-use-ie-browser.md)hakkında daha fazla bilgi edinin.
