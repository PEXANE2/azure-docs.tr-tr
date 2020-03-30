---
title: Blob depolamasına istek te şifreleme anahtarı sağlama
titleSuffix: Azure Storage
description: Azure Blob depolamasına karşı istekte bulunma isteği yapan istemciler, istek başına (önizleme) bir şifreleme anahtarı sağlama seçeneğine sahiptir. İstekteki şifreleme anahtarının eklenmesi, Blob depolama işlemleri için şifreleme ayarları üzerinde parçalı denetim sağlar.
services: storage
author: tamram
ms.service: storage
ms.date: 03/12/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: c8a5555c5c33255fdc5902a115e7e9103a4e936f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79410070"
---
# <a name="provide-an-encryption-key-on-a-request-to-blob-storage-preview"></a>Blob depolama alanına istekte şifreleme anahtarı sağlama (önizleme)

Azure Blob depolamasına karşı istekte bulunma isteği yapan istemciler, istek başına (önizleme) bir şifreleme anahtarı sağlama seçeneğine sahiptir. İstekteki şifreleme anahtarının eklenmesi, Blob depolama işlemleri için şifreleme ayarları üzerinde parçalı denetim sağlar. Müşteri tarafından sağlanan anahtarlar Azure Key Vault'ta veya başka bir anahtar mağazasında saklanabilir.

## <a name="encrypting-read-and-write-operations"></a>Okuma ve yazma işlemlerini şifreleme

Bir istemci uygulaması istek üzerine bir şifreleme anahtarı sağladığında, Azure Depolama, blob verilerini okurken ve yazarken şifreleme ve şifre çözmeyi saydam bir şekilde gerçekleştirir. Azure Depolama, blob içeriğinin yanında şifreleme anahtarının SHA-256 karmasını yazar. Karma, blob'a karşı sonraki tüm işlemlerin aynı şifreleme anahtarını kullandığını doğrulamak için kullanılır.

Azure Depolama, istemcinin istekle birlikte gönderdiği şifreleme anahtarını depolamaz veya yönetmez. Şifreleme veya şifre çözme işlemi tamamlanır tamamlanmaz anahtar güvenli bir şekilde atılır.

İstemci istek temüşteri tarafından sağlanan bir anahtar kullanarak bir blob oluşturduğunda veya güncellediğinde, sonraki okuma ve yazma isteklerini de bu blob için sağlamalıdır. Anahtar, müşteri tarafından sağlanan bir anahtarla şifrelenmiş bir blob isteğinde sağlanmazsa, istek hata kodu 409 (Çakışma) ile başarısız olur.

İstemci uygulaması istek üzerine bir şifreleme anahtarı gönderirse ve depolama hesabı da Microsoft tarafından yönetilen bir anahtar veya müşteri tarafından yönetilen bir anahtar kullanılarak şifrelenirse, Azure Depolama şifreleme ve şifre çözme isteğinde sağlanan anahtarı kullanır.

İsteğin bir parçası olarak şifreleme anahtarını göndermek için, istemcinin HTTPS'yi kullanarak Azure Depolama'ya güvenli bir bağlantı kurması gerekir.

Her blob anlık görüntüsünün kendi şifreleme anahtarı olabilir.

## <a name="request-headers-for-specifying-customer-provided-keys"></a>Müşteri tarafından sağlanan anahtarları belirtmek için üstbilgi isteme

REST aramaları için istemciler, Blob depolamasına yapılan bir istekle ilgili şifreleme anahtarı bilgilerini güvenli bir şekilde aktarmak için aşağıdaki üstbilgileri kullanabilir:

|İstek Başlığı | Açıklama |
|---------------|-------------|
|`x-ms-encryption-key` |İstekleri hem yazmak hem de okumak için gereklidir. Base64 kodlanmış AES-256 şifreleme anahtar değeri. |
|`x-ms-encryption-key-sha256`| İstekleri hem yazmak hem de okumak için gereklidir. Şifreleme anahtarının Base64 kodlanmış SHA256'sı. |
|`x-ms-encryption-algorithm` | Yazma istekleri için gereklidir, okuma istekleri için isteğe bağlıdır. Verilen anahtarı kullanarak verileri şifrelerken kullanılacak algoritmayı belirtir. AES256 olmalı. |

İstekte şifreleme anahtarları nın belirtilmesi isteğe bağlıdır. Ancak, bir yazma işlemi için yukarıda listelenen üstbilgilerden birini belirtirseniz, bunların hepsini belirtmeniz gerekir.

## <a name="blob-storage-operations-supporting-customer-provided-keys"></a>Müşteri tarafından sağlanan anahtarları destekleyen Blob depolama işlemleri

Aşağıdaki Blob depolama işlemleri, bir istek üzerine müşteri tarafından sağlanan şifreleme anahtarlarının gönderilmesini destekler:

- [İkili Büyük Nesne Koyma](/rest/api/storageservices/put-blob)
- [Blok Listesi Koy](/rest/api/storageservices/put-block-list)
- [Blok Koy](/rest/api/storageservices/put-block)
- [URL'den Blok Koy](/rest/api/storageservices/put-block-from-url)
- [Sayfa Koy](/rest/api/storageservices/put-page)
- [URL'den Sayfa Koy](/rest/api/storageservices/put-page-from-url)
- [Ek Blok](/rest/api/storageservices/append-block)
- [Blob Özelliklerini Ayarla](/rest/api/storageservices/set-blob-properties)
- [Blob Meta verilerini ayarlama](/rest/api/storageservices/set-blob-metadata)
- [Blob alın](/rest/api/storageservices/get-blob)
- [BLOB özelliklerini Al](/rest/api/storageservices/get-blob-properties)
- [Blob Meta verilerini alın](/rest/api/storageservices/get-blob-metadata)
- [İkili Büyük Nesne Anlık Görüntüsünü Alma](/rest/api/storageservices/snapshot-blob)

## <a name="rotate-customer-provided-keys"></a>Müşteri tarafından sağlanan anahtarları döndürme

Bir blob şifrelemek için kullanılan bir şifreleme anahtarı döndürmek için, blob indirin ve sonra yeni şifreleme anahtarı ile yeniden yükleyin.

> [!IMPORTANT]
> Azure portalı, istekte sağlanan bir anahtarla şifrelenmiş bir kapsayıcı veya blob'dan okumak veya yazmak için kullanılamaz.
>
> Azure Key Vault gibi güvenli bir anahtar mağazasında Blob depolamasına istekte sağladığınız şifreleme anahtarını koruduğunuzdan emin olun. Şifreleme anahtarı olmadan bir kapsayıcı veya blob üzerinde bir yazma işlemi denerseniz, işlem başarısız olur ve nesneye erişimi kaybedersiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [.NET ile Blob depolama sına istek te müşteri tarafından sağlanan bir anahtar belirtin](../blobs/storage-blob-customer-provided-key.md)
- [Veriler için Azure Depolama şifrelemesi](storage-service-encryption.md)
