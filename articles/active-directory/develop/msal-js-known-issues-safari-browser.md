---
title: Tarayıcılarda bilinen sorunlar (JavaScript için Microsoft kimlik doğrulama kitaplığı)
titleSuffix: Microsoft identity platform
description: Safari tarayıcısı ile JavaScript için Microsoft kimlik doğrulama kitaplığı (MSAL. js) kullanılırken sorunları öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51d800ea2fbbc733a6213d7bc4f61f955612aba0
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803077"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Safari tarayıcısı ile MSAL. js ile ilgili bilinen sorunlar 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Safari 12 ve ıOP 2,0 ' de sessiz belirteç yenilemesi

Apple iOS 12 ve MacOS 10,14 işletim sistemleri [Safari 12 tarayıcısının](https://developer.apple.com/safari/whats-new/)bir sürümünü içeriyordu. Güvenlik ve gizlilik amaçları doğrultusunda Safari 12, [akıllı Izleme önleme 2,0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/)' i içerir. Bu aslında tarayıcının ayarlanan üçüncü taraf tanımlama bilgilerini bırakmaya neden olur. IOP 2,0 ayrıca kimlik sağlayıcıları tarafından ayarlanan tanımlama bilgilerini üçüncü taraf tanımlama bilgileri olarak değerlendirir.

### <a name="impact-on-msaljs"></a>MSAL. js üzerindeki etki

MSAL. js, `acquireTokenSilent` çağrılarının bir parçası olarak sessiz belirteç alma ve yenileme işlemleri gerçekleştirmek için gizli bir IFRAME kullanır. Sessiz Belirteç istekleri, Azure AD tarafından ayarlanan tanımlama bilgileriyle temsil edilen kimliği doğrulanmış kullanıcı oturumuna erişimi olan IFRAME 'yi kullanır. IOP 2,0, bu tanımlama bilgilerine erişimi engelliyor, MSAL. js belirteçleri sessizce alamıyor ve yenilemez ve bu durum `acquireTokenSilent` hatalara neden olur.

Bu noktada bu sorun için çözüm yoktur ve standartları topluluk ile seçenekleri değerlendiriyoruz.

### <a name="work-around"></a>Geçici çözüm

Varsayılan olarak, ıOP ayarı Safari tarayıcısı üzerinde etkinleştirilmiştir. **Tercihler** -> **gizliliği** ' ne giderek ve **siteler arası izlemeyi engelle** seçeneğinin işaretini kaldırarak bu ayarı devre dışı bırakabilirsiniz.

![Safari ayarı](./media/msal-js-known-issue-safari-browser/safari.png)

`acquireTokenSilent` hatasını, kullanıcıdan oturum açmasını isteyen etkileşimli bir alma belirteci çağrısıyla işlemeniz gerekir.
Yinelenen oturum açma işlemlerini önlemek için uygulayabileceğiniz bir yaklaşım, `acquireTokenSilent` başarısızlığını idare etmek ve kullanıcıya etkileşimli çağrıya devam etmeden önce Safari 'deki ıOP ayarını devre dışı bırakma seçeneği sağlamamaktır. Ayar devre dışı bırakıldıktan sonra, sonraki sessiz belirteç yenilemeler başarılı olmalıdır.
