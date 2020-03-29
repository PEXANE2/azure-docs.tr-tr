---
title: Bilinen Safari tarayıcı sorunları (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Safari tarayıcısıyla JavaScript için Microsoft Kimlik Doğrulama Kitaplığını (MSAL.js) kullanırken sorunları öğrenin.
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
ms.openlocfilehash: edb995e31c2872c1541e29fee09dd66aafc8f9e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696121"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>MSAL.js ile Safari tarayıcıda bilinen sorunlar 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Safari 12 ve ITP 2.0'da sessiz belirteç yenileme

Apple iOS 12 ve MacOS 10.14 işletim sistemleri [Safari 12 tarayıcısının](https://developer.apple.com/safari/whats-new/)bir sürümü dahil . Güvenlik ve gizlilik amacıyla Safari 12, [Akıllı İzleme Önleme 2.0'ı](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/)içerir. Bu, tarayıcının ayarlanan üçüncü taraf tanımlama bilgilerini bırakmasına neden olur. ITP 2.0, kimlik sağlayıcıları tarafından ayarlanan tanımlama bilgilerini de üçüncü taraf tanımlama bilgileri olarak ele adatır.

### <a name="impact-on-msaljs"></a>MSAL.js üzerindeki etkisi

`acquireTokenSilent` MSAL.js, çağrıların bir parçası olarak sessiz belirteç edinimi ve yenileme gerçekleştirmek için gizli bir Iframe kullanır. Sessiz belirteç istekleri, Azure AD tarafından ayarlanan tanımlama bilgileriyle temsil edilen kimlik doğrulaması kullanıcı oturumuna erişene sahip Olan Iframe'e dayanır. ITP 2.0'ın bu tanımlama bilgilerine erişimi engellemesiyle, MSAL.js sessizce jetonları satın almak ve yenilemek için başarısız olur ve bu `acquireTokenSilent` da hatalara neden olur.

Bu noktada bu soruna bir çözüm bulunmamaktadır ve standartları topluluğuyla seçenekleri değerlendiriyoruz.

### <a name="work-around"></a>Geçici çözüm

Varsayılan olarak SAFARI tarayıcıda ITP ayarı etkinleştirilir. **Tercihler** -> **Gizliliği'ne** geçerek ve **site ler arası izlemeyi önleme** seçeneğinin denetimini kalarak bu ayarı devre dışı kullanabilirsiniz.

![safari ayarı](./media/msal-js-known-issue-safari-browser/safari.png)

Kullanıcının `acquireTokenSilent` oturum açmasını gerektiren etkileşimli bir edinme belirteci çağrısıyla hataları işlemeniz gerekir.
Tekrarlanan oturum açmaişlemlerini önlemek için, uygulayabileceğiniz bir `acquireTokenSilent` yaklaşım, hatayı işlemek ve kullanıcıya etkileşimli aramaya devam etmeden önce Safari'deki ITP ayarını devre dışı bırakılması seçeneği sunmaktır. Ayar devre dışı bırakıldıktan sonra, sonraki sessiz belirteç yenilemeleri başarılı olmalıdır.
