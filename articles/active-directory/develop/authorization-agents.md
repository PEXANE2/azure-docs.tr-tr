---
title: Yetkilendirme aracıları ve bunları etkinleştirme | Mavisi
description: Farklı yetkilendirme aracıları hakkında bilgi edinin Microsoft kimlik doğrulama kitaplığı (MSAL), Android uygulamanızın kullanmasına ve bunların nasıl etkinleştirileceğini sağlar.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/05/2019
ms.author: marsma
ms.reviewer: shoatman, brianmel, hahamil
ms.custom: aaddev
ms.openlocfilehash: 83a33fa3891e01c484f298f22d67467bc54a7618
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85551975"
---
# <a name="authorization-agents-android"></a>Yetkilendirme aracıları (Android)

Bu makalede, Microsoft kimlik doğrulama kitaplığı 'nın (MSAL) uygulamanızın kullanmasını ve bunların nasıl etkinleştirileceğini sağlayan farklı yetkilendirme aracıları açıklanmaktadır.

Yetkilendirme aracıları için belirli bir strateji seçilmesi isteğe bağlıdır ve uygulamaların özelleştirebileceği ek işlevleri temsil eder. Çoğu uygulama, MSAL varsayılanlarını kullanır (çeşitli Varsayılanları görmek için bkz. [ANDROID msal yapılandırma dosyasını anlayın](msal-configuration.md) ).

MSAL `WebView` , veya sistem tarayıcısını kullanarak yetkilendirmeyi destekler.  Aşağıdaki görüntüde, veya customtab `WebView` ile veya customtab ile sistem tarayıcısını kullanarak nasıl göründüğü gösterilmektedir:

![MSAL oturum açma örnekleri](./media/authorization-agents/sign-in-ui.jpg)

## <a name="single-sign-in-implications"></a>Çoklu oturum açma etkileri

Varsayılan olarak, MSAL ile tümleştirilmiş uygulamalar, yetkilendirme için sistem tarayıcısının özel sekmelerini kullanır. WebViews 'ın aksine özel sekmeler, Web veya özel sekmelerle tümleştirilmiş diğer yerel uygulamalarla daha az oturum açma olanağı sağlayan bir tanımlama bilgisi jar 'yi varsayılan sistem tarayıcısıyla paylaşır.

Uygulama `WebView` Microsoft Authenticator veya şirket portalı desteğini uygulamayla tümleştirmeden kullanıyorsa, kullanıcılar cihazda veya yerel uygulamalar ile Web uygulamaları arasında çoklu oturum açma (SSO) deneyimine sahip olmaz.

Uygulama, Microsoft Authenticator veya Şirket Portalı desteği ile MSAL kullanıyorsa, kullanıcının uygulamalardan biriyle etkin bir oturum açma işlemi varsa, kullanıcılar uygulamalar arasında SSO deneyimine sahip olabilir.

## <a name="webview"></a>WebView

Uygulama içi WebView 'u kullanmak için, MSAL 'e geçirilen uygulama yapılandırması JSON öğesine aşağıdaki satırı koyun:

```json
"authorization_user_agent" : "WEBVIEW"
```

Uygulama içi kullanıldığında `WebView` , Kullanıcı doğrudan uygulamada oturum açar. Belirteçler, uygulamanın korumalı alanı içinde tutulur ve uygulamanın tanımlama bilgisi jar dışında kullanılamaz. Sonuç olarak, uygulamalar Authenticator veya Şirket Portalı tümleştirmediği sürece kullanıcının uygulamalar arasında SSO deneyimi yoktur.

Ancak, `WebView` oturum açma kullanıcı arabirimi için görünüm ve kullanım özelliklerini özelleştirme yeteneği sağlar. Bu özelleştirmeyi nasıl yapılacağı hakkında daha fazla bilgi için bkz. [Android WebViews](https://developer.android.com/reference/android/webkit/WebView) .

## <a name="default-browser-plus-custom-tabs"></a>Varsayılan tarayıcı Plus özel sekmeler

Varsayılan olarak, MSAL tarayıcıyı ve [özel bir sekmeler](https://developer.chrome.com/multidevice/android/customtabs) stratejisini kullanır. Bu stratejiyi, `DEFAULT` AŞAĞıDAKI JSON yapılandırmasını özel yapılandırma dosyasında kullanarak, gelecekteki sürümlerde yapılan değişikliklerin önlenmesi için açıkça belirtebilirsiniz:

```json
"authorization_user_agent" : "BROWSER"
```

Cihazın tarayıcısı aracılığıyla bir SSO deneyimi sağlamak için bu yaklaşımı kullanın. MSAL, diğer yerel uygulamaların veya Web uygulamalarının, MSAL tarafından ayarlanan kalıcı oturum tanımlama bilgisini kullanarak cihazda SSO sağlamasına izin veren paylaşılan bir tanımlama bilgisi jar 'i kullanır.

## <a name="browser-selection-heuristic"></a>Tarayıcı seçimi buluşsal yöntemi

MSAL 'in çok sayıda Android telefonunda kullanılacak tarayıcı paketinin tam olarak belirtilmediği için, MSAL, en iyi cihazlar arası SSO sağlamaya çalışan bir tarayıcı seçimi buluşsal yöntemi uygular.

MSAL, hangi tarayıcıyı kullanacağınızı seçmek için cihaza yüklenmiş tarayıcıların tam listesini alır. Liste Paket Yöneticisi tarafından döndürülen sırada, Kullanıcı tercihlerini dolaylı olarak yansıtır. Örneğin, varsayılan tarayıcı, ayarlandığında, listedeki ilk girdidir. Listedeki _ilk_ tarayıcı, özel sekmeleri destekleyip desteklememesinden bağımsız olarak seçilir. Tarayıcı özel sekmeleri destekliyorsa, MSAL özel sekmesini başlatacaktır. özel sekmeler, uygulamaya yönelik bir görünüm ve `WebView` temel kullanıcı arabirimi özelleştirmesine izin verir. Daha fazla bilgi edinmek için bkz. [Android 'de özel sekmeler](https://developer.chrome.com/multidevice/android/customtabs) .

Cihazda hiç tarayıcı paketi yoksa, MSAL uygulamayı kullanır `WebView` .

Tarayıcı listesindeki tarayıcıların sırası, işletim sistemi tarafından belirlenir. En az tercih edilen en azından bir sıradır. Cihaz varsayılan ayarı değiştirilmemişse, SSO deneyimi sağlamak için her oturum açma için aynı tarayıcı başlatılmalıdır.

> [!NOTE]
> MSAL artık, başka bir tarayıcı varsayılan olarak ayarlandıysa Chrome 'ı her zaman tercih eder. Örneğin, hem Chrome hem de başka bir tarayıcı önceden yüklenmiş bir cihazda, MSAL kullanıcının varsayılan olarak ayarlandığı tarayıcıyı kullanır.

### <a name="tested-browsers"></a>Sınanan tarayıcılar

Aşağıdaki tarayıcılar yapılandırma dosyasında belirtilen öğesine doğru şekilde yönlendirildiklerini görmek üzere test edilmiştir `"redirect_uri"` :

| Cihaz | Yerleşik tarayıcı | Chrome | Opera  | Microsoft Edge | UC tarayıcısı | Firefox |
| -- |:-------------:| -----:|-----:|-----:|-----:|-----:|
| Nexus 4 (API 17) | aktar | aktar |uygulanamaz |uygulanamaz |uygulanamaz |uygulanamaz |
| Samsung S7 (API 25) | aktar | aktar | aktar | aktar | Neden |aktar |
| Huawei (API 26) |Pass * * | aktar | Neden | aktar | aktar |aktar |
| Vivo (API 26) |aktar|aktar|aktar|aktar|aktar|Neden|
| Piksel 2 (API 26) |aktar | aktar | aktar | aktar | Neden |aktar |
| Oppo | aktar | uygulanamaz * * * |uygulanamaz  |uygulanamaz |uygulanamaz | uygulanamaz|
| OnePlus (API 25) |aktar | aktar | aktar | aktar | Neden |aktar |
| Nexus (API 28) |aktar | aktar | aktar | aktar | Neden |aktar |
|MI | aktar | aktar | aktar | aktar | Neden |aktar |

* Samsung 'in yerleşik tarayıcısı Samsung Internet 'dir.  
* * Huawei 'ın yerleşik tarayıcı, Huawei tarayıcısıdır.  
Varsayılan tarayıcı, Oppo cihaz ayarı içinde değiştirilemez.
