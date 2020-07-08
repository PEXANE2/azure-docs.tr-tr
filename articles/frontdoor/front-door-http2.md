---
title: Azure ön kapılı HTTP2 desteği | Microsoft Docs
description: Bu makale Azure ön kapılarında HTTP/2 desteği hakkında bilgi edinmenize yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 8a3ae8065553b34a72528cb0f2681e327dc90097
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80985193"
---
# <a name="http2-support-in-azure-front-door"></a>Azure ön kapıda HTTP/2 desteği

Şu anda HTTP/2 desteği tüm Azure ön kapı yapılandırmalarında etkindir. Müşterilerden başka bir eylem yapılması gerekmez.

Http/2, HTTP/1.1 için önemli bir düzeltme. Daha hızlı Web performansı, daha az yanıt süresi ve geliştirilmiş Kullanıcı deneyimi sağlar, ancak tanıdık HTTP yöntemlerini, durum kodlarını ve semantiğini koruyun. Http/2, HTTP ve HTTPS ile çalışmak üzere tasarlanmış olsa da, çoğu istemci Web tarayıcısı yalnızca Aktarım Katmanı Güvenliği (TLS) üzerinden HTTP/2 ' yi destekler.

> [!NOTE]
> HTTP/2 protokol desteği yalnızca istemcilerden ön kapıya yapılan istekler için kullanılabilir. Arka uç havuzunda ön kapıdan geri uca iletişim HTTP/1.1 üzerinden yapılır. 

### <a name="http2-benefits"></a>HTTP/2 avantajları

HTTP/2 avantajları şunları içerir:

*   **Çoğullama ve eşzamanlılık**

    HTTP 1,1 kullanarak birden çok kaynak isteğinin birden çok TCP bağlantısı olması ve her bağlantının onunla ilişkili performans yükü vardır. HTTP/2, tek bir TCP bağlantısında birden fazla kaynağın istenme izin verir.

*   **Üstbilgi sıkıştırması**

    Sunulan kaynakların HTTP üst bilgilerini sıkıştırarak, iletişimdeki zaman önemli ölçüde azaltılır.

*   **Akış bağımlılıkları**

    Akış bağımlılıkları, istemcinin önceliğe sahip olduğu sunucuya işaret ediyor olmasını sağlar.


## <a name="http2-browser-support"></a>HTTP/2 tarayıcı desteği

Tüm ana tarayıcılar geçerli sürümlerinde HTTP/2 desteği uyguladık. Desteklenmeyen tarayıcılar, otomatik olarak HTTP/1.1 'e geri dönüş.

|Tarayıcı|En Düşük Sürüm|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="next-steps"></a>Sonraki Adımlar

HTTP/2 hakkında daha fazla bilgi edinmek için aşağıdaki kaynakları ziyaret edin:

- [HTTP/2 belirtim giriş sayfası](https://http2.github.io/)
- [Resmi HTTP/2 SSS](https://http2.github.io/faq/)
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
