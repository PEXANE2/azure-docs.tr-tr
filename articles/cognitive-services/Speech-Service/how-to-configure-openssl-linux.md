---
title: Linux için OpenSSL’yi yapılandırma
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
ms.openlocfilehash: ff8772f7c3c3213c010b0bdbd0d0aa8897404bac
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119988"
---
# <a name="configure-openssl-for-linux"></a>Linux için OpenSSL’yi yapılandırma

1\.9.0 öncesinde herhangi bir konuşma SDK sürümü kullanılırken, [OpenSSL](https://www.openssl.org) dinamik olarak konak sistemi sürümüne yapılandırılır. Konuşma SDK 'sının sonraki sürümlerinde OpenSSL (sürüm [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)), konuşma SDK 'sının temel kitaplığına statik olarak bağlanır.

Bağlantıyı sağlamak için, OpenSSL sertifikalarının sisteminizde yüklü olduğunu doğrulayın. Bir komut çalıştırın:
```bash
openssl version -d
```

Ubuntu/de, tabanlı sistemlerdeki çıkış şu şekilde olmalıdır:
```
OPENSSLDIR: "/usr/lib/ssl"
```

OPENSSLDIR altında `certs` alt dizin olup olmadığını denetleyin. Yukarıdaki örnekte `/usr/lib/ssl/certs`olacaktır.

* `/usr/lib/ssl/certs` varsa ve çok sayıda ayrı sertifika dosyası içeriyorsa (`.crt` veya `.pem` uzantılı), başka eylemlere gerek yoktur.

* OPENSSLDIR `/usr/lib/ssl` ' den başka bir şeydir ve/veya birden çok ayrı dosya yerine tek bir sertifika paketi dosyası varsa, sertifikaların nerede bulunabileceği göstermek için uygun bir SSL ortam değişkeni ayarlamanız gerekir.

## <a name="examples"></a>Örnekler

- OPENSSLDIR `/opt/ssl`. Birçok `.crt` veya `.pem` dosyası içeren `certs` alt dizini vardır.
Konuşma SDK 'sını kullanan bir programı çalıştırmadan önce `/opt/ssl/certs` göstermek için ortam değişkeni `SSL_CERT_DIR` ayarlayın. Örneğin:
```bash
SSL_CERT_DIR=/opt/ssl/certs ./helloworld
```

- OPENSSLDIR `/etc/pki/tls`. Bir sertifika paketi dosyası (örneğin `ca-bundle.pem` veya `ca-bundle.crt`) vardır.
Konuşma SDK 'sını kullanan bir programı çalıştırmadan önce `/etc/pki/tls/ca-bundle.pem` göstermek için ortam değişkeni `SSL_CERT_FILE` ayarlayın. Örneğin:
```bash
SSL_CERT_FILE=/etc/pki/tls/ca-bundle.pem ./helloworld
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK 'Sı hakkında](speech-sdk.md)
