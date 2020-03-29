---
title: Azure CDN'de HTTP/2 desteği | Microsoft Dokümanlar
description: HTTP/2 ve CDN desteği hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: lichard
manager: erikre
editor: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: ril
ms.openlocfilehash: 703623e3f7f314d87417458f3f9a218dfdf45427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67849965"
---
# <a name="http2-support-in-azure-cdn"></a>Azure CDN'de HTTP/2 Desteği

HTTP/2 HTTP/1.1\'e yapılan büyük bir revizyondur. Tanıdık HTTP yöntemlerini, durum kodlarını ve anlambilimini korurken daha hızlı web performansı, daha az yanıt süresi ve gelişmiş kullanıcı deneyimi sağlar. HTTP/2 HTTP ve HTTPS ile çalışmak üzere tasarlanmış olsa da, birçok istemci web tarayıcısı sadece TLS üzerinden HTTP/2'yi destekler.

### <a name="http2-benefits"></a>HTTP/2 Avantajlar

HTTP/2'nin yararları şunlardır:

*   **Çoklama ve eşzamanlılık**

    HTTP 1.1'i kullanarak, birden çok kaynak isteğinde bulunmak için birden çok TCP bağlantısı gerektirir ve her bağlantıyla ilişkili performans ek yükü vardır. HTTP/2, tek bir TCP bağlantısında birden çok kaynağın istenmesine izin verir.

*   **Üstbilgi sıkıştırma**

    Hizmet verilen kaynaklar için HTTP üstbilgilerini sıkıştırarak, kabloüzerindeki süre önemli ölçüde azalır.

*   **Akış bağımlılıkları**

    Akış bağımlılıkları istemcinin sunucuya hangi kaynakların önceliği olduğunu göstermesine olanak sağlar.


## <a name="http2-browser-support"></a>HTTP/2 Tarayıcı Desteği

Tüm büyük tarayıcılar mevcut sürümlerinde HTTP/2 desteği uyguladık. Desteklenmeyen tarayıcılar otomatik olarak HTTP/1.1'e geri döner.

|Tarayıcı|En Düşük Sürüm|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="enabling-http2-support-in-azure-cdn"></a>Azure CDN'de HTTP/2 Desteğini Etkinleştirme

Şu anda, HTTP/2 desteği tüm Azure CDN profilleri için etkindir. Müşterilerden başka bir işlem gerekmez.

## <a name="next-steps"></a>Sonraki Adımlar

HTTP/2'nin faydalarını iş başında görmek [için, Akamai'den bu demoya](https://http2.akamai.com/demo)bakın.

HTTP/2 hakkında daha fazla bilgi edinmek için aşağıdaki kaynakları ziyaret edin:

*   [HTTP/2 belirtim ana sayfası](https://http2.github.io/)
*   [Resmi HTTP/2 SSS](https://http2.github.io/faq/)
*   [Akamai HTTP/2 bilgi](https://http2.akamai.com/)

Azure CDN'nin kullanılabilir özellikleri hakkında daha fazla bilgi edinmek için [Azure CDN Genel Bakış'a](https://azure.microsoft.com/documentation/articles/cdn-overview/)bakın.