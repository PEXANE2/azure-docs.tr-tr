---
title: Linux için OpenSSL’yi yapılandırma
titleSuffix: Azure Cognitive Services
description: Linux için OpenSSL'yi nasıl yapılandırılacağınızı öğrenin.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
ms.openlocfilehash: 42960c25c4124203b64646fdc5cbca833b246e21
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683157"
---
# <a name="configure-openssl-for-linux"></a>Linux için OpenSSL’yi yapılandırma

1.9.0'dan önce herhangi bir Speech SDK sürümü kullanırken [OpenSSL](https://www.openssl.org) dinamik olarak ana bilgisayar sistemi sürümüne göre yapılandırılır. Konuşma SDK sonraki sürümlerinde, OpenSSL (sürüm [1.1.1b)](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)statik Konuşma SDK çekirdek kitaplığı ile bağlantılıdır.

Bağlantı sağlamak için OpenSSL sertifikalarının sisteminize yüklendiğini doğrulayın. Bir komut çalıştırın:
```bash
openssl version -d
```

Ubuntu/Debian tabanlı sistemlerde çıkış olmalıdır:
```
OPENSSLDIR: "/usr/lib/ssl"
```

OPENSSLDIR `certs` altında alt dizin olup olmadığını kontrol edin. Yukarıdaki örnekte, bu `/usr/lib/ssl/certs`olacaktır .

* Varsa `/usr/lib/ssl/certs` ve birçok bireysel sertifika dosyası `.crt` (veya uzantılı) `.pem` içeriyorsa, başka işlemlere gerek yoktur.

* OPENSSLDIR birden çok `/usr/lib/ssl` dosya yerine tek bir sertifika paketi dosyası varsa ve/veya birden çok ayrı dosya yerine tek bir sertifika paketi dosyası varsa, sertifikaların nerede bulunabileceğini belirtmek için uygun bir SSL ortamı değişkeni ayarlamanız gerekir.

## <a name="examples"></a>Örnekler

- OPENSSLDIR `/opt/ssl`olduğunu . Birçok `certs` `.crt` veya `.pem` dosyaları ile alt dizin vardır.
Konuşma SDK kullanan `/opt/ssl/certs` bir program çalıştırmadan önce noktaya ortam değişkeni `SSL_CERT_DIR` ayarlayın. Örneğin:
```bash
export SSL_CERT_DIR=/opt/ssl/certs
```

- OPENSSLDIR `/etc/pki/tls` (RHEL/CentOS tabanlı sistemlerde olduğu gibi). Örneğin, `certs` `ca-bundle.crt`sertifika paketi dosyası olan alt dizin vardır.
Konuşma SDK kullanan bir program çalıştırmadan önce ortam değişkenini `SSL_CERT_FILE` bu dosyayı işaret etmek için ayarlayın. Örneğin:
```bash
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt
```
> [!NOTE]
> Linux'un bazı dağılımlarında TMP veya TMPDIR ortam değişkeni tanımlı olmadığını da belirtmekte yarar vardır. Bu, Konuşma SDK'sının CRL'yi süresi dolana kadar yeniden kullanmak üzere diske önbelleğe almak yerine her seferinde Sertifika İptal Listesi'ni (CRL) indirmesine neden olur. İlk bağlantı performansını artırmak için [TMPDIR adında bir ortam değişkeni oluşturabilir ve seçtiğiniz geçici dizinin yoluna ayarlayabilirsiniz.](https://help.ubuntu.com/community/EnvironmentVariables)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK'sı hakkında](speech-sdk.md)
