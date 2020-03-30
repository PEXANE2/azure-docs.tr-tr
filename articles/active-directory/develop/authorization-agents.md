---
title: Yetkilendirme aracıları ve bunları nasıl etkinleştirmek | Azure
description: Microsoft Kimlik Doğrulama Kitaplığı'nın (MSAL) Android uygulamanızın kullanmasına izin verdiği farklı yetkilendirme aracıları ve bunları nasıl etkinleştireceğimiz hakkında bilgi edinin.
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
ms.openlocfilehash: 4f1b3fc5b60069cfa47d437e4341ded141204418
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77085323"
---
# <a name="authorization-agents-android"></a>Yetkilendirme aracıları (Android)

Bu makalede, Microsoft Kimlik Doğrulama Kitaplığı'nın (MSAL) uygulamanızın kullanmasına izin verdiği farklı yetkilendirme aracıları ve bunları nasıl etkinleştireceğimiz açıklanmaktadır.

Yetkilendirme aracıları için belirli bir strateji seçmek isteğe bağlıdır ve uygulamaların özelleştirebileceği ek işlevleri temsil eder. Çoğu uygulama MSAL varsayılanlarını kullanır (bkz. çeşitli varsayılanları görmek için [Android MSAL yapılandırma dosyasını anlayın).](msal-configuration.md)

`WebView`MSAL, bir veya sistem tarayıcısı kullanarak yetkilendirmeyi destekler.  Aşağıdaki resim, CustomTabs `WebView`ile veya CustomTabs olmadan sistem tarayıcısını kullanarak nasıl göründüğünü gösterir:

![MSAL giriş örnekleri](./media/authorization-agents/sign-in-ui.jpg)

## <a name="single-sign-in-implications"></a>Tek oturum açma etkileri

Varsayılan olarak, MSAL ile tümleşik uygulamalar yetkilendirmek için sistem tarayıcısının Özel Sekmelerini kullanır. Web Görünümleri'nin aksine, Özel Sekmeler varsayılan sistem tarayıcısıyla bir çerez kavanozu paylaşır ve web veya Özel Sekmelerle tümleştirilmiş diğer yerel uygulamalarla daha az oturum açma sağlar.

Uygulama, Microsoft `WebView` Authenticator veya Şirket Portalı desteğini uygulamalarına entegre etmeden bir strateji kullanıyorsa, kullanıcılar cihaz da veya yerel uygulamalar ve web uygulamaları arasında Tek Oturum Açma (SSO) deneyimine sahip olmaz.

Uygulama, Microsoft Authenticator veya Şirket Portalı desteğiyle MSAL kullanıyorsa, kullanıcı uygulamalardan biriyle etkin oturum açarsa, kullanıcılar uygulamalar arasında Bir SSO deneyimi yaşayabilir.

## <a name="webview"></a>WebView

Uygulama içi WebView'ı kullanmak için, MSAL'a geçirilen uygulama yapılandırması JSON'a aşağıdaki satırı koyun:

```json
"authorization_user_agent" : "WEBVIEW"
```

Uygulamayı `WebView`kullanırken, kullanıcı doğrudan uygulamaya imza atar. Jetonlar uygulamanın kum havuzunda tutulur ve uygulamanın çerez kavanozunun dışında kullanılamaz. Sonuç olarak, uygulamalar Kimlik Doğrulayıcı veya Şirket Portalı ile tümleşmedikçe kullanıcı uygulamalar arasında Bir SSO deneyimine sahip olamaz.

Ancak, `WebView` görünüm özelleştirmek için yeteneği sağlar ve oturum açma UI için hissediyorum. Bu özelleştirmenin nasıl yapılacağını öğrenmek için [Android Web Görünümleri'ne](https://developer.android.com/reference/android/webkit/WebView) bakın.

## <a name="default-browser-plus-custom-tabs"></a>Varsayılan tarayıcı artı özel sekmeler

Varsayılan olarak, MSAL tarayıcıyı ve [özel sekme stratejisini](https://developer.chrome.com/multidevice/android/customtabs) kullanır. Özel yapılandırma dosyasında aşağıdaki JSON yapılandırmasını `DEFAULT` kullanarak gelecekteki sürümlerde değişiklikleri önlemek için bu stratejiyi açıkça belirtebilirsiniz:

```json
"authorization_user_agent" : "BROWSER"
```

Aygıtın tarayıcısı üzerinden Bir SSO deneyimi sağlamak için bu yaklaşımı kullanın. MSAL, diğer yerel uygulamaların veya web uygulamalarının MSAL tarafından ayarlanan kalıcı oturum çerezini kullanarak aygıtta SSO'ya ulaşmasını sağlayan paylaşılan bir çerez kavanozu kullanır.

## <a name="browser-selection-heuristic"></a>Tarayıcı seçimi sezgisel

MSAL'ın android telefonların geniş bir dizide kullanılacak tam tarayıcı paketini belirtebilmesi mümkün olmadığından, MSAL en iyi cihaz arası SSO'yu sağlamaya çalışan bir tarayıcı seçimi sezgiseluygular.

MSAL, hangi tarayıcının kullanılacağını seçmek için cihaza yüklenen tarayıcıların tam listesini alır. Liste, dolaylı olarak kullanıcının tercihlerini yansıtan paket yöneticisi tarafından döndürülen sıradadır. Örneğin, varsayılan tarayıcı, eğer ayarlanırsa, listedeki ilk giriştir. Listedeki _ilk_ tarayıcı, özel sekmeleri destekleyip desteklemediğine bakılmaksızın seçilir. Tarayıcı Özel Sekmeleri destekliyorsa, MSAL Özel Sekmeleri başlatacak. `WebView` Daha fazla bilgi edinmek için [Android'deki Özel Sekmeler'e](https://developer.chrome.com/multidevice/android/customtabs) bakın.

Cihazda tarayıcı paketi yoksa, MSAL uygulamayı `WebView`kullanır.

Tarayıcı listesindeki tarayıcıların sırası işletim sistemi tarafından belirlenir. En çok tercih edilenden en azına doğru sırayla. Aygıt varsayılan ayarı değiştirilmezse, SSO deneyimi sağlamak için her oturum açma için aynı tarayıcı nın başlatılması gerekir.

> [!NOTE]
> Başka bir tarayıcı varsayılan olarak ayarlanmışsa MSAL artık her zaman Chrome'u tercih etmez. Örneğin, hem Chrome'u hem de başka bir tarayıcıyı önceden yüklenmiş olan bir aygıtta, MSAL kullanıcının varsayılan olarak belirlediği tarayıcıyı kullanır.

### <a name="tested-browsers"></a>Test Edilmiş Tarayıcılar

Aşağıdaki tarayıcılar, yapılandırma dosyasında `"redirect_uri"` belirtilenlere doğru şekilde yönlendirilip yönlendirilmediklerini görmek için sınanmıştır:

| | Dahili Tarayıcı | Chrome | Opera  | Microsoft Edge | UC Tarayıcı | Firefox |
| -- |:-------------:| -----:|-----:|-----:|-----:|-----:|
| Nexus 4 (API 17) | Geçirmek | Geçirmek |geçerli değildir |geçerli değildir |geçerli değildir |geçerli değildir |
| Samsung S7 (API 25) | geçiş* | Geçirmek | Geçirmek | Geçirmek | Başarısız |Geçirmek |
| Huawei (API 26) |geçiş** | Geçirmek | Başarısız | Geçirmek | Geçirmek |Geçirmek |
| Vivo (API 26) |Geçirmek|Geçirmek|Geçirmek|Geçirmek|Geçirmek|Başarısız|
| Piksel 2 (API 26) |Geçirmek | Geçirmek | Geçirmek | Geçirmek | Başarısız |Geçirmek |
| Oppo | Geçirmek | geçerli değildir*** |geçerli değildir  |geçerli değildir |geçerli değildir | geçerli değildir|
| OnePlus (API 25) |Geçirmek | Geçirmek | Geçirmek | Geçirmek | Başarısız |Geçirmek |
| Nexus (API 28) |Geçirmek | Geçirmek | Geçirmek | Geçirmek | Başarısız |Geçirmek |
|MI | Geçirmek | Geçirmek | Geçirmek | Geçirmek | Başarısız |Geçirmek |

*Samsung'un yerleşik tarayıcısı Samsung Internet'tir.  
**Huawei'nin yerleşik tarayıcısı Huawei Browser'dır.  
Varsayılan tarayıcı Oppo aygıt ayarı içinde değiştirilemez.
