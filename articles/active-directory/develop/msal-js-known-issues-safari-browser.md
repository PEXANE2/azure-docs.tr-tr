---
title: Bilinen Safari tarayıcısı sorunları (MSAL. js) | Mavisi
titleSuffix: Microsoft identity platform
description: Safari tarayıcısı ile JavaScript için Microsoft kimlik doğrulama kitaplığı (MSAL. js) kullanılırken sorunları öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76696121"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Safari tarayıcısı ile MSAL. js ile ilgili bilinen sorunlar 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Safari 12 ve ıOP 2,0 ' de sessiz belirteç yenilemesi

Apple iOS 12 ve MacOS 10,14 işletim sistemleri [Safari 12 tarayıcısının](https://developer.apple.com/safari/whats-new/)bir sürümünü içeriyordu. Güvenlik ve gizlilik amaçları doğrultusunda Safari 12, [akıllı Izleme önleme 2,0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/)' i içerir. Bu aslında tarayıcının ayarlanan üçüncü taraf tanımlama bilgilerini bırakmaya neden olur. IOP 2,0 ayrıca kimlik sağlayıcıları tarafından ayarlanan tanımlama bilgilerini üçüncü taraf tanımlama bilgileri olarak değerlendirir.

### <a name="impact-on-msaljs"></a>MSAL. js üzerindeki etki

MSAL. js, `acquireTokenSilent` çağrıların bir parçası olarak sessiz belirteç alma ve yenileme işlemleri gerçekleştirmek için gizli bir IFRAME kullanır. Sessiz Belirteç istekleri, Azure AD tarafından ayarlanan tanımlama bilgileriyle temsil edilen kimliği doğrulanmış kullanıcı oturumuna erişimi olan IFRAME 'yi kullanır. IOP 2,0, bu tanımlama bilgilerine erişimi engelliyor, MSAL. js belirteçleri sessizce alamıyor ve yenilemez ve bu durum `acquireTokenSilent` hatalara neden olur.

Bu noktada bu sorun için çözüm yoktur ve standartları topluluk ile seçenekleri değerlendiriyoruz.

### <a name="work-around"></a>Geçici çözüm

Varsayılan olarak, ıOP ayarı Safari tarayıcısı üzerinde etkinleştirilmiştir. **Tercihler** -> **gizliliği** ' ne giderek ve **siteler arası izlemeyi engelle** seçeneğinin işaretini kaldırarak bu ayarı devre dışı bırakabilirsiniz.

![Safari ayarı](./media/msal-js-known-issue-safari-browser/safari.png)

Kullanıcıdan oturum açmasını isteyen bir etkileşimli `acquireTokenSilent` alma belirteci çağrısıyla ilgili sorunları işlemeniz gerekecektir.
Yinelenen oturum açma işlemlerini önlemek için uygulayabileceğiniz bir yaklaşım, `acquireTokenSilent` hatayı idare etmek ve kullanıcıya etkileşimli çağrıya devam etmeden önce Safari 'deki IP ayarını devre dışı bırakma seçeneğini sağlamaktır. Ayar devre dışı bırakıldıktan sonra, sonraki sessiz belirteç yenilemeler başarılı olmalıdır.
