---
title: Azure CDN Rules altyapısı için Verizon özgü HTTP üstbilgileri | Microsoft Docs
description: Bu makalede, Azure CDN Rules Engine ile Verizon özgü HTTP üstbilgilerinin nasıl kullanılacağı açıklanır.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: allensu
ms.openlocfilehash: d2208f6769c8051b38bdafb92d62ec03cb2d668c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81253569"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN Rules altyapısı için Verizon 'e özgü HTTP üstbilgileri

**Verizon ürünlerinden Azure CDN Premium** için, kaynak sunucuya bir http isteği gönderildiğinde, varlık noktası (pop) sunucusu, pop 'a istemci isteğine bir veya daha fazla ayrılmış üst bilgi (veya proxy özel üst bilgileri) ekleyebilir. Bu üst bilgiler, alınan standart iletme üst bilgilerine ek niteliğindedir. Standart istek üstbilgileri hakkında daha fazla bilgi için bkz. [istek alanları](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Bu ayrılmış üstbilgilerin, kaynak sunucuya Azure CDN (Content Delivery Network) POP isteğine eklenmesini engellemek istiyorsanız, kurallar altyapısında [Ara sunucu özel üstbilgileri özelliğiyle](cdn-verizon-premium-rules-engine-reference-features.md#proxy-special-headers) bir kural oluşturmanız gerekir. Bu kuralda, kaldırmak istediğiniz üstbilgiyi üstbilgiler alanındaki üstbilgiler listesinden dışlayın. [Hata ayıklama önbelleği yanıt üst bilgileri özelliğini](cdn-verizon-premium-rules-engine-reference-features.md#debug-cache-response-headers)etkinleştirdiyseniz, gerekli `X-EC-Debug` üst bilgileri eklediğinizden emin olun. 

Örneğin, `Via` üstbilgiyi kaldırmak için kuralın üstbilgiler alanı aşağıdaki üst bilgi listesini Içermelidir: *x-iletilmiş-for, x-iletilmiş-Proto, x-Host, x-midgress, x-Gateway-List, x-EC-Name, Host*. 

![Proxy özel üstbilgileri kuralı](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

Aşağıdaki tabloda, istekte Verizon CDN POP tarafından eklenebilen üstbilgiler açıklanmaktadır:

İstek üst bilgisi | Açıklama | Örnek
---------------|-------------|--------
[Yazıcısıyla](#via-request-header) | İsteği bir kaynak sunucuya kullanan POP sunucusunu tanımlar. | HTTP/1.1 ECS (DCA/1A2B)
X-Iletilmiş-Için | İsteyenin IP adresini gösterir.| 10.10.10.10
X-Iletilen-proto | İsteğin protokolünü gösterir. | http
X-ana bilgisayar | İsteğin ana bilgisayar adını gösterir. | cdn.mydomain.com
X-Midgress | İsteğin ek bir CDN sunucusu aracılığıyla proxy kullanıp kullanmadığını belirtir. Örneğin, bir başlangıç sunucusundan kaynağa kalkan sunucusu veya bir AÇıLAN sunucudan ADN ağ geçidi sunucusu. <br />Bu üst bilgi yalnızca orta boyutlu trafik gerçekleştiğinde isteğe eklenir. Bu durumda, isteğin ek bir CDN sunucusu aracılığıyla proxy olduğunu göstermek için üst bilgi 1 olarak ayarlanır.| 1
[Konak](#host-request-header) | İstenen içeriğin bulunabileceği Konağı ve bağlantı noktasını tanımlar. | marketing.mydomain.com:80
[X-Gateway-List](#x-gateway-list-request-header) | ADN: bir müşteri kaynağına atanan ADN Ağ Geçidi sunucularının yük devretme listesini tanımlar. <br />Kaynak Kalkanı: bir müşteri kaynağına atanan kaynak kalkan sunucularının kümesini gösterir. | `icn1,hhp1,hnd1`
X-EC-_&lt;adı&gt;_ | *X-EC* (örneğin, x-EC-Tag, [x-EC-Debug](cdn-http-debug-headers.md)) Ile başlayan istek üstbilgileri CDN tarafından kullanılmak üzere ayrılmıştır.| WAF-üretim

## <a name="via-request-header"></a>İstek üst bilgisi aracılığıyla
`Via` İstek ÜSTBILGISININ bir pop sunucusunu tanımladığı biçim aşağıdaki sözdizimi tarafından belirtilir:

`Via: Protocol from Platform (POP/ID)` 

Sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:
- Protokol: isteği proxy için kullanılan protokol sürümünü (örneğin, HTTP/1.1) gösterir. 

- Platform: içeriğin istendiği platformu gösterir. Bu alan için aşağıdaki kodlar geçerlidir: 

    Kod | Platform
    -----|---------
    ECAcc | HTTP büyük
    ECS   | HTTP küçük
    ECD   | Uygulama teslim ağı (ADN)

- POP: isteği işlenen [pop 'U](cdn-pop-abbreviations.md) gösterir. 

- ID: yalnızca iç kullanım Içindir.

### <a name="example-via-request-header"></a>İstek üst bilgisi aracılığıyla örnek

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Ana bilgisayar istek üst bilgisi
Aşağıdaki koşullardan her ikisi de doğru `Host` olduğunda pop sunucuları üst bilginin üzerine yazar:
- İstenen içerik kaynağı bir müşteri kaynak sunucusudur.
- Karşılık gelen müşteri kaynağının HTTP ana bilgisayar üst bilgisi seçeneği boş değil.

HTTP `Host` ana bilgisayar üstbilgisi seçeneğinde tanımlanan değeri yansıtacak şekilde istek üst bilgisinin üzerine yazılacak.
Müşteri başlangıcının HTTP ana bilgisayar üst bilgisi seçeneği boş olarak ayarlandıysa, `Host` istek sahibi tarafından gönderilen istek üst bilgisi müşterinin kaynak sunucusuna iletilir.

## <a name="x-gateway-list-request-header"></a>X-Gateway-List istek üst bilgisi
Aşağıdaki koşullardan biri karşılandığında bir POP sunucusu, ' X-Gateway-List istek üst bilgisini ekler/üzerine yazar:
- İstek, ADN platformunu işaret ediyor.
- İstek, kaynak kalkan özelliği tarafından korunan bir müşteri kaynak sunucusuna iletilir.

