---
title: Azure CDN kuralları altyapısı için Verizon'a özel HTTP başlıkları | Microsoft Dokümanlar
description: Bu makalede, Azure CDN kuralları altyapısıyla Verizon'a özgü HTTP üstbilginin nasıl kullanılacağı açıklanmaktadır.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: magattus
ms.openlocfilehash: a5881bea578f2791f8dc0d6e760fd15c6f47e435
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593256"
---
# <a name="verizon-specific-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN kuralları altyapısı için Verizon'a özgü HTTP başlıkları

**Verizon ürünlerinden Azure CDN Premium** için, başlangıç sunucusuna bir HTTP isteği gönderildiğinde, durum noktası (POP) sunucusu, istemci isteğine bir veya daha fazla ayrılmış üstbilgi (veya proxy özel üstbilgi) ekleyebilir. Bu üstbilgi, alınan standart iletme üstbilgilerine ek olarak verilir. Standart istek üstbilgileri hakkında bilgi için [İstek alanlarına](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields)bakın.

Bu ayrılmış üstbilgilerden birinin kaynak sunucuya Azure CDN (İçerik Dağıtım Ağı) POP isteğine eklenmesini önlemek istiyorsanız, kurallar altyapısındaki [Proxy Özel Üstbilgi özelliğine](cdn-verizon-premium-rules-engine-reference-features.md#proxy-special-headers) sahip bir kural oluşturmanız gerekir. Bu kuralda, üstbilgi alanında üstbilgi alanında varsayılan listesinden kaldırmak istediğiniz üstbilgi hariç. [Hata Ayıklama Önbellek Yanıtı Üstbilgi özelliğini](cdn-verizon-premium-rules-engine-reference-features.md#debug-cache-response-headers)etkinleştirdiyseniz, `X-EC-Debug` gerekli üstbilgi eklemeyi unutmayın. 

Örneğin, `Via` üstbilgi kaldırmak için, kuralın üstbilgi alanı aşağıdaki üstbilgi listesini içermelidir: *X-Forwarded-For, X-Forwarded-Proto, X-Host, X-Midgress, X-Gateway-List, X-EC-Name, Host*. 

![Proxy Özel Başlıklar kuralı](./media/cdn-http-headers/cdn-proxy-special-header-rule.png)

Aşağıdaki tabloda, verizon CDN POP tarafından istek içinde eklenebilir üstbilgi açıklanır:

İstek üst bilgisi | Açıklama | Örnek
---------------|-------------|--------
[Via](#via-request-header) | İsteği bir başlangıç sunucusuna yakınlayan POP sunucusunu tanımlar. | HTTP/1.1 ECS (dca/1A2B)
X-Forwarded-For | İsteklinin IP adresini gösterir.| 10.10.10.10
X-İlli-Proto | İsteğin protokolünü gösterir. | http
X-Host | İsteğin ana bilgisayar adını gösterir. | cdn.mydomain.com
X-Midgress | İsteğin ek bir CDN sunucusu aracılığıyla yakınlanıp yakınlanmadığını gösterir. Örneğin, pop sunucusundan orijinli kalkan sunucusuna veya POP sunucusundan ADN'ye ağ geçidi sunucusu. <br />Bu üstbilgi, yalnızca orta daki trafik gerçekleştiğinde isteğe eklenir. Bu durumda, üstbilgi isteği ek bir CDN sunucusu aracılığıyla yakınolduğunu belirtmek için 1 olarak ayarlanır.| 1
[Konak](#host-request-header) | İstenen içeriğin bulunabileceği ana bilgisayarı ve bağlantı noktasını tanımlar. | marketing.mydomain.com:80
[X-Ağ Geçidi Listesi](#x-gateway-list-request-header) | ADN: Müşteri kaynağına atanan ADN Ağ Geçidi sunucularının başarısız listesini tanımlar. <br />Kaynak kalkanı: Müşteri menşeine atanan kaynak kalkanı sunucularının kümesini gösterir. | `icn1,hhp1,hnd1`
X-EC_&lt;adı&gt;_ | *X-EC* ile başlayan istek üstaylıkları (örneğin, X-EC-Tag, [X-EC-Hata Ayıklama)](cdn-http-debug-headers.md)CDN tarafından kullanılmak üzere ayrılmıştır.| waf-üretim

## <a name="via-request-header"></a>İstek üstbilgisi aracılığıyla
İstek üstbilgisinin `Via` bir POP sunucusunu tanımladığı biçim aşağıdaki sözdizimi tarafından belirtilir:

`Via: Protocol from Platform (POP/ID)` 

Sözdiziminde kullanılan terimler aşağıdaki gibi tanımlanır:
- Protokol: İsteğin proxy'si için kullanılan protokolün sürümünü (örneğin, HTTP/1.1) gösterir. 

- Platform: İçeriğin istendiği platformu gösterir. Aşağıdaki kodlar bu alan için geçerlidir: 

    Kod | Platform
    -----|---------
    ECAcc | HTTP Büyük
    Ecs   | HTTP Küçük
    Eçg   | Uygulama dağıtım ağı (ADN)

- POP: İsteği işleyen [POP'u](cdn-pop-abbreviations.md) gösterir. 

- ID: Yalnızca dahili kullanım için.

### <a name="example-via-request-header"></a>Örnek İstek üstbilgisi üzerinden

`Via: HTTP/1.1 ECD (dca/1A2B)`

## <a name="host-request-header"></a>Ana bilgisayar istek üstbilgisi
Pop sunucuları, aşağıdaki `Host` koşulların her ikisi de doğru olduğunda üstbilginin üzerine yazar:
- İstenen içeriğin kaynağı bir müşteri kökenli sunucudur.
- İlgili müşteri menşeliNIN HTTP Ana Bilgisayar Üstbilgi seçeneği boş değildir.

İstek `Host` üstbilgisi, HTTP Ana Bilgisayar Üstbilgisi seçeneğinde tanımlanan değeri yansıtacak şekilde üzerine yazılır.
Müşteri menşeliNIN HTTP Ana Bilgisayar Üstbilgi seçeneği `Host` boş olarak ayarlanmışsa, istek sahibi tarafından gönderilen istek üstbilgisi müşterinin başlangıç sunucusuna iletilir.

## <a name="x-gateway-list-request-header"></a>X-Ağ Geçidi Listesi istek üstbilgisi
Bir POP sunucusu, aşağıdaki koşullardan biri karşılandığında 'X-Gateway-List istek üstbilgisini ekler/üzerine yazar:
- İstek ADN platformuna işaret ediyor.
- İstek, Origin Shield özelliği tarafından korunan bir müşteri kökenli sunucuya iletilir.

