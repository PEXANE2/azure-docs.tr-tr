---
title: IoT Tak ve Çalıştır Önizleme deposuna erişmek için güvenlik belirteci oluşturma | Microsoft Dokümanlar
description: Bir IoT Tak ve Çalıştır Önizleme modeli deposuna programlı olarak erişdiğinizde kullanmak üzere paylaşılan bir erişim imza belirteci oluşturun.
author: Philmea
ms.author: philmea
ms.date: 12/27/2019
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: f008627317588467d731ccc03aec7738f58e46e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159209"
---
# <a name="generate-sas-token"></a>SAS belirteci oluşturma

Bu nasıl yapılır kılavuzu, IoT Tak ve Çalıştır Önizleme modeli deposu API'leri ile kullanılacak paylaşılan erişim imzası (SAS) belirteci nasıl programlanabilir şekilde oluşturacağınızı gösterir.

## <a name="python"></a>Python

Aşağıdaki parçacık Python kullanarak bir SAS belirteci oluşturmak için nasıl gösterir:

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

Aşağıdaki parçacık, C\#kullanarak nasıl bir SAS belirteci oluşturacağınızı gösterir:

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

## <a name="use-the-sas-token"></a>SAS belirteci kullanma

Bir SAS belirteci oluşturduktan sonra, bir HTTP POST isteği nde bulunmak için kullanabilirsiniz. Örnek:

```text
POST https:///models/{modelId}?repositoryId={repositoryId}&api-version=2019-07-01-preview
```

Bir istemciye bir SAS belirteci verirseniz, istemci kaynağın birincil anahtarına sahip değildir ve elde etmek için karmayı tersine çeviremez. SAS belirteci, istemcinin nelere erişebileceği ve ne kadar süreyle erişebileceği üzerinde denetim sağlar. İlkede birincil anahtarı değiştirdiğinizde, ondan oluşturulan SAS belirteçleri geçersiz kılınmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi model IoT Tak ve Çalıştır Önizleme modeli depolarına erişmek için kullanmak için güvenlik belirteçleri oluşturma hakkında öğrendim, önerilen bir sonraki adım [IoT Tak ve Çalıştır Önizleme modelleme geliştirici kılavuzunda](concepts-developer-guide.md)daha fazla bilgi edinmektir.
