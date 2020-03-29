---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 03/29/2019
ms.author: erhopf
ms.openlocfilehash: dc5e251fee00ee22edb2261c1abd8404714834ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78669235"
---
## <a name="authentication"></a>Kimlik doğrulaması

Her istek bir yetkilendirme üstbilgigerektirir. Bu tablo, her hizmet için hangi üstbilginin destekleniyi gösterir:

| Desteklenen yetkilendirme üstbilgi | Konuşmayı metne dönüştürme | Metin okuma |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | Evet | Hayır |
| Yetkilendirme: Taşıyıcı | Evet | Evet |

Üstbilgi `Ocp-Apim-Subscription-Key` kullanırken yalnızca abonelik anahtarınızı sağlamanız gerekir. Örnek:

```http
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

`Authorization: Bearer` Üstbilgi kullanırken, `issueToken` bitiş noktasına bir istekte bulunmanız gerekir. Bu istekte, abonelik anahtarınızı 10 dakika geçerli bir erişim jetonuyla değiştirirsiniz. Sonraki birkaç bölümde bir belirteç almayı ve bir belirteç kullanmayı öğreneceksiniz.

### <a name="how-to-get-an-access-token"></a>Erişim jetonu nasıl elde edilir?

Erişim jetonu almak için, abonelik anahtarını `issueToken` `Ocp-Apim-Subscription-Key` ve bitiş noktasına bir istekte bulunmanız gerekir.

Bitiş `issueToken` noktası şu biçime sahiptir:

```http
https://<REGION_IDENTIFIER>.api.cognitive.microsoft.com/sts/v1.0/issueToken
```

Aboneliğinizin bulunduğu bölgeye uyan tanımlayıcıyı bu tablodan değiştirin: `<REGION_IDENTIFIER>`

[!INCLUDE [](cognitive-services-speech-service-region-identifier.md)]

Erişim jeton isteğinizi oluşturmak için bu örnekleri kullanın.

#### <a name="http-sample"></a>HTTP örneği

Bu örnek, bir belirteç almak için basit bir HTTP isteğidir. Konuşma `YOUR_SUBSCRIPTION_KEY` Hizmeti abonelik anahtarınızla değiştirin. Aboneliğiniz Batı ABD bölgesinde değilse, üstbilginin yerine bölgenizin `Host` ana bilgisayar adı değiştirin.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Yanıtın gövdesi JSON Web Belirteci (JWT) biçiminde erişim belirteci içerir.

#### <a name="powershell-sample"></a>PowerShell örneği

Bu örnek, erişim jetonu almak için basit bir PowerShell komut dosyasıdır. Konuşma `YOUR_SUBSCRIPTION_KEY` Hizmeti abonelik anahtarınızla değiştirin. Aboneliğinizle eşleşen bölge için doğru bitiş noktasını kullandığınızdan emin olun. Bu örnek şu anda Batı ABD olarak ayarlanır.

```powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

#### <a name="curl-sample"></a>cURL örneği

cURL, Linux'ta (ve Linux için Windows Alt Sistemi'nde) kullanılabilen bir komut satırı aracıdır. Bu cURL komutu, erişim jetonunun nasıl alındığını gösterir. Konuşma `YOUR_SUBSCRIPTION_KEY` Hizmeti abonelik anahtarınızla değiştirin. Aboneliğinizle eşleşen bölge için doğru bitiş noktasını kullandığınızdan emin olun. Bu örnek şu anda Batı ABD olarak ayarlanır.

```console
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>C# örneği

Bu C# sınıfı, erişim jetonunun nasıl alındığını göstermektedir. Sınıfı anında yaptığınızda Konuşma Hizmeti abonelik anahtarınızı geçirin. Aboneliğiniz Batı ABD bölgesinde değilse, aboneliğiniz için `FetchTokenUri` bölgeyle eşleşecek şekilde değerini değiştirin.

```csharp
public class Authentication
{
    public static readonly string FetchTokenUri =
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

#### <a name="python-sample"></a>Python örneği

```python
# Request module must be installed.
# Run pip install requests if necessary.
import requests

subscription_key = 'REPLACE_WITH_YOUR_KEY'


def get_token(subscription_key):
    fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
    headers = {
        'Ocp-Apim-Subscription-Key': subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    access_token = str(response.text)
    print(access_token)
```

### <a name="how-to-use-an-access-token"></a>Erişim jetonu nasıl kullanılır?

Erişim belirteci `Authorization: Bearer <TOKEN>` üstbilgi olarak servise gönderilmelidir. Her erişim jetonu 10 dakika süreyle geçerlidir. Ancak, ağ trafiğini ve gecikme süresini en aza indirmek için istediğiniz zaman yeni bir belirteç alabilirsiniz, aynı belirteci dokuz dakika kullanmanızı öneririz.

Burada metin-to-speech REST API için örnek bir HTTP isteği' s:

```http
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.stt.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

// Message body here...
```
