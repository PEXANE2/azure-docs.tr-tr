---
title: Linux için OpenSSL 'yi yapılandırma
titleSuffix: Azure Cognitive Services
description: Linux için OpenSSL 'yi nasıl yapılandıracağınızı öğrenin.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: cadf31dede8ee81323076013d00b9431f597bda6
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156497"
---
# <a name="configure-openssl-for-linux"></a>Linux için OpenSSL 'yi yapılandırma

1\.9.0 öncesinde herhangi bir konuşma SDK sürümü kullanılırken, [OpenSSL](https://www.openssl.org) dinamik olarak konak sistemi sürümüne yapılandırılır. Konuşma SDK 'sının sonraki sürümlerinde OpenSSL (sürüm [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)), konuşma SDK 'sının temel kitaplığına statik olarak bağlanır.

## <a name="troubleshoot-connectivity"></a>Bağlantı sorunlarını giderme

Konuşma SDK 'sının 1.9.0 sürümünü kullanırken bağlantı hatalarıyla karşılaşırsanız, `ssl/certs` dizininin Linux dosya sisteminde bulunan `/usr/lib` dizininde bulunduğundan emin olun. `ssl/certs` Dizin *yoksa*, aşağıdaki komutu kullanarak OpenSSL 'nin sisteminizde yüklü olduğunu kontrol edin:

```bash
which openssl
```

Ardından, OpenSSL `certs` dizinini bulun ve bu dizinin içeriğini `/usr/lib/ssl/certs` dizinine kopyalayın. Sonra, bağlantı sorunlarının çözümlenip çözümlenmediğini görmek için yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK 'Sı hakkında](speech-sdk.md)