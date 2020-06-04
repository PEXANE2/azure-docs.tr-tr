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
ms.openlocfilehash: e20f6ce9540d357b61ae2cfdf0e8f96d127dc6c0
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84343226"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN Rules altyapısı için Verizon 'e özgü HTTP üstbilgileri

**Verizon ürünlerinden Azure CDN Premium** için, kaynak sunucuya bir http isteği gönderildiğinde, varlık noktası (pop) sunucusu, pop 'a istemci isteğine bir veya daha fazla ayrılmış üst bilgi (veya proxy özel üst bilgileri) ekleyebilir. Bu üst bilgiler, alınan standart iletme üst bilgilerine ek niteliğindedir. Standart istek üstbilgileri hakkında daha fazla bilgi için bkz. [istek alanları](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields).

Bu ayrılmış üstbilgilerin, kaynak sunucuya Azure CDN (Content Delivery Network) POP isteğine eklenmesini engellemek istiyorsanız, kurallar altyapısında [Ara sunucu özel üstbilgileri özelliğiyle](https://docs.vdms.com/cdn/Content/HRE/F/Proxy-Special-Headers.htm) bir kural oluşturmanız gerekir. Bu kuralda, kaldırmak istediğiniz üstbilgiyi üstbilgiler alanındaki üstbilgiler listesinden dışlayın. [Hata ayıklama önbelleği yanıt üst bilgileri özelliğini](https://docs.vdms.com/cdn/Content/HRE/F/Debug-Cache-Response-Headers.htm)etkinleştirdiyseniz, gerekli üst bilgileri eklediğinizden emin olun `X-EC-Debug` . 

Örneğin, üstbilgiyi kaldırmak için `Via` kuralın üstbilgiler alanı aşağıdaki üst bilgi listesini içermelidir: *x-Iletilmiş-for, x-Iletilmiş-Proto, x-Host, x-Midgress, x-Gateway-List, x-EC-Name, Host*. 

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
X-EC-_ &lt; adı &gt; _ | *X-EC* (örneğin, x-EC-Tag, [x-EC-Debug](cdn-http-debug-headers.md)) Ile başlayan istek üstbilgileri CDN tarafından kullanılmak üzere ayrılmıştır.| WAF-üretim

## <a name="via-request-header"></a>İstek üst bilgisi aracılığıyla
`Via`İstek üstbilgisinin BIR pop sunucusunu tanımladığı biçim aşağıdaki sözdizimi tarafından belirtilir:

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
`Host`Aşağıdaki koşullardan her ikisi de doğru olduğunda pop sunucuları üst bilginin üzerine yazar:
- İstenen içerik kaynağı bir müşteri kaynak sunucusudur.
- Karşılık gelen müşteri kaynağının HTTP ana bilgisayar üst bilgisi seçeneği boş değil.

`Host`Http ana bilgisayar üstbilgisi seçeneğinde tanımlanan değeri yansıtacak şekilde istek üst bilgisinin üzerine yazılacak.
Müşteri başlangıcının HTTP ana bilgisayar üst bilgisi seçeneği boş olarak ayarlandıysa, `Host` istek sahibi tarafından gönderilen istek üst bilgisi müşterinin kaynak sunucusuna iletilir.

## <a name="x-gateway-list-request-header"></a>X-Gateway-List istek üst bilgisi
Aşağıdaki koşullardan biri karşılandığında bir POP sunucusu, ' X-Gateway-List istek üst bilgisini ekler/üzerine yazar:
- İstek, ADN platformunu işaret ediyor.
- İstek, kaynak kalkan özelliği tarafından korunan bir müşteri kaynak sunucusuna iletilir.

