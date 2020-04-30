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
ms.openlocfilehash: 42960c25c4124203b64646fdc5cbca833b246e21
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81683157"
---
# <a name="configure-openssl-for-linux"></a>Linux için OpenSSL’yi yapılandırma

1.9.0 öncesinde herhangi bir konuşma SDK sürümü kullanılırken, [OpenSSL](https://www.openssl.org) dinamik olarak konak sistemi sürümüne yapılandırılır. Konuşma SDK 'sının sonraki sürümlerinde OpenSSL (sürüm [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)), konuşma SDK 'sının temel kitaplığına statik olarak bağlanır.

Bağlantıyı sağlamak için, OpenSSL sertifikalarının sisteminizde yüklü olduğunu doğrulayın. Bir komut çalıştırın:
```bash
openssl version -d
```

Ubuntu/de, tabanlı sistemlerdeki çıkış şu şekilde olmalıdır:
```
OPENSSLDIR: "/usr/lib/ssl"
```

OPENSSLDIR altında `certs` alt dizin olup olmadığını denetleyin. Yukarıdaki örnekte, olacaktır `/usr/lib/ssl/certs`.

* Varsa `/usr/lib/ssl/certs` ve çok sayıda ayrı sertifika dosyası ( `.crt` veya `.pem` uzantısı) içeriyorsa, başka eylemlere gerek yoktur.

* OPENSSLDIR, ve/' den `/usr/lib/ssl` başka bir şeydir veya birden çok ayrı dosya yerine tek bir sertifika paketi dosyası varsa, sertifikaların nerede bulunabileceği göstermek için uygun bir SSL ortam değişkeni ayarlamanız gerekir.

## <a name="examples"></a>Örnekler

- OPENSSLDIR `/opt/ssl`. Çok `certs` `.crt` veya `.pem` dosyaları olan alt dizin vardır.
Ortam değişkenini `SSL_CERT_DIR` , konuşma SDK 'sını `/opt/ssl/certs` kullanan bir programı çalıştırmadan önce işaret etmek üzere ayarlayın. Örneğin:
```bash
export SSL_CERT_DIR=/opt/ssl/certs
```

- OPENSSLDIR, `/etc/pki/tls` (RHEL/CentOS tabanlı sistemlerde olduğu gibi). Örneğin `ca-bundle.crt`, `certs` bir sertifika paketi dosyası olan alt dizin bulunur.
Konuşma SDK 'sını `SSL_CERT_FILE` kullanan bir programı çalıştırmadan önce bu dosyaya işaret etmek için ortam değişkenini ayarlayın. Örneğin:
```bash
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt
```
> [!NOTE]
> Ayrıca, bazı Linux dağıtımlarının tanımlı bir TMP veya TMPDIR ortam değişkeni olmadığından da buna dikkat edin. Bu, konuşma SDK 'sının, sertifika Iptal listesini (CRL) her seferinde karşıdan indirmesini, ancak süreleri dolana kadar yeniden kullanmak üzere CRL 'yi diske almasını sağlar. İlk bağlantı performansını geliştirmek için, [TMPDIR adlı bir ortam değişkeni oluşturabilir ve bunu seçtiğiniz geçici dizininizin yoluna ayarlayabilirsiniz.](https://help.ubuntu.com/community/EnvironmentVariables)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK'sı hakkında](speech-sdk.md)
