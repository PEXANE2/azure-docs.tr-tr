---
title: IoT Tak ve Kullan önizleme deposuna erişmek için güvenlik belirteci oluşturma | Microsoft Docs
description: Programlama yoluyla IoT Tak ve Kullan önizleme modeli deposuna eriştiğinizde kullanılacak bir paylaşılan erişim imza belirteci oluşturun.
author: Philmea
ms.author: philmea
ms.date: 08/06/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: f6c4f5b9784eeff9d03b6e93953674736fb78c6c
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976035"
---
# <a name="generate-sas-token"></a>SAS belirteci oluştur

Bu nasıl yapılır Kılavuzu, IoT Tak ve Kullan önizleme modeli deposu API 'Leri ile kullanmak üzere programlı bir şekilde paylaşılan erişim imzası (SAS) belirteci oluşturmayı gösterir.

## <a name="python"></a>Python

Aşağıdaki kod parçacığında, Python kullanarak bir SAS belirteci oluşturma gösterilmektedir:

```python
from base64 import b64decode, b64encode
from hashlib import sha256
from hmac import digest
from time import time
from urllib.parse import quote_plus, urlencode

def calculate_sas_token(hostname, repo_id, key_name, key, expiry_in_second):
    expire = int(time() + expiry_in_second)
    sign_value = "{0}\n{1}\n{2}".format(repo_id, quote_plus(hostname), expire)
    sig = b64encode(digest(b64decode(key), sign_value.encode("utf-8"), sha256))
    token = "SharedAccessSignature " + urlencode({
        "sr": hostname, 
        "sig": sig,
        "se": str(expire),
        "skn": key_name,
        "rid": repo_id
        })
    return token
```

## <a name="c"></a>C\#

Aşağıdaki kod parçacığında, C\#kullanarak bir SAS belirteci oluşturma gösterilmektedir:

```csharp
public static string generateSasToken(string hostName, string repoId, string key, string keyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = repoId + "\n" + WebUtility.UrlEncode(hostName) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(
        CultureInfo.InvariantCulture,
        "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}&rid={4}",
        WebUtility.UrlEncode(hostName),
        WebUtility.UrlEncode(signature),
        expiry,
        keyName,
        repoId);

    return token;
}
```

## <a name="use-the-sas-token"></a>SAS belirtecini kullanma

Bir SAS belirteci oluşturduktan sonra, bir HTTP POST isteği oluşturmak için kullanabilirsiniz. Örnek:

```text
POST https:///models/{modelId}?repositoryId={repositoryId}&api-version=2019-07-01-preview
```

İstemciye bir SAS belirteci verirseniz, istemci kaynağın birincil anahtarına sahip değildir ve karmayı elde etmek için ters çeviremez. Bir SAS belirteci, istemcinin erişebileceği ve ne kadar süreyle erişebileceklerini denetlemenizi sağlar. İlkede birincil anahtarı değiştirdiğinizde, bundan oluşturulan tüm SAS belirteçleri geçersiz kılınır.

## <a name="next-steps"></a>Sonraki adımlar

IoT Tak ve Kullan önizleme modeli depolarında erişim için kullanılacak güvenlik belirteçleri oluşturmayı öğrendiğinize göre, [ıot Tak ve kullan Preview modelleme geliştirici kılavuzunda](concepts-developer-guide.md)daha fazla bilgi edinilmesi önerilir.
