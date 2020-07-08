---
title: Özel konular veya etki alanları Event Grid için istemci yayımlama olaylarını doğrulama
description: Bu makalede, istemci yayımlama olaylarını Event Grid özel konuları doğrulamak için farklı yollar açıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/25/2020
ms.author: spelluru
ms.openlocfilehash: 7805c9b9096cb52fcef3dad8eb0bdd9509cbce47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85414902"
---
# <a name="authenticate-publishing-clients-azure-event-grid"></a>Yayımlama istemcilerinin kimliğini doğrulama (Azure Event Grid)
Bu makalede, **erişim anahtarı** veya **paylaşılan ERIŞIM imzası (SAS)** belirteci kullanarak Azure Event Grid konular veya etki alanları için olayları yayınlayan istemcilerin kimlik doğrulaması hakkında bilgi sağlanır. SAS belirtecini kullanmanızı öneririz, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok mevcut Web kancası yayımcısıyla uyumludur.  

## <a name="authenticate-using-an-access-key"></a>Erişim anahtarı kullanarak kimlik doğrulama
Erişim anahtarı kimlik doğrulaması en basit kimlik doğrulama biçimidir. Erişim anahtarını bir HTTP üstbilgisi veya URL sorgu parametresi olarak geçirebilirsiniz. 

### <a name="access-key-in-a-http-header"></a>HTTP üstbilgisindeki erişim anahtarı
Erişim anahtarını HTTP üstbilgisi için bir değer olarak geçirin: `aeg-sas-key` .

```
aeg-sas-key: XXXXXXXXXXXXXXXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="access-key-as-a-query-parameter"></a>Sorgu parametresi olarak erişim anahtarı
Ayrıca, `aeg-sas-key` bir sorgu parametresi olarak belirtebilirsiniz. 

```
https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01&&aeg-sas-key=XXXXXXXX53249XX8XXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

Bir konu veya etki alanı için erişim anahtarları alma hakkında yönergeler için bkz. [erişim anahtarlarını alma](get-access-keys.md).

## <a name="authenticate-using-a-sas-token"></a>SAS belirteci kullanarak kimlik doğrulama
Bir Event Grid kaynağı için SAS belirteçleri, kaynak, sona erme saati ve bir imzayı içerir. SAS belirtecinin biçimi: `r={resource}&e={expiration}&s={signature}` .

Kaynak, olayları gönderdiğiniz olay Kılavuzu konusunun yoludur. Örneğin, geçerli bir kaynak yolu: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01` . Desteklenen tüm API sürümlerini görmek için bkz. [Microsoft. EventGrid kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions). 

İlk olarak, program aracılığıyla bir SAS belirteci oluşturun ve ardından `aeg-sas-token` `Authorization SharedAccessSignature` Event Grid kimlik doğrulaması için üstbilgiyi veya üstbilgiyi kullanın. 

### <a name="generate-sas-token-programmatically"></a>Program aracılığıyla SAS belirteci oluştur
Aşağıdaki örnek, Event Grid ile kullanmak için bir SAS belirteci oluşturur:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

### <a name="using-aeg-sas-token-header"></a>AEG-SAS-belirteç üstbilgisini kullanma
Üst bilgi için SAS belirtecini bir değer olarak geçirme örneği aşağıda verilmiştir `aeg-sas-toke` . 

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=XXXXXXXXXXXXX%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

### <a name="using-authorization-header"></a>Yetkilendirme üst bilgisini kullanma
Üst bilgi için SAS belirtecini bir değer olarak geçirme örneği aşağıda verilmiştir `Authorization` . 

```http
Authorization: SharedAccessSignature r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=XXXXXXXXXXXXX%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

## <a name="next-steps"></a>Sonraki adımlar
Olayları iletmek için olay işleyicileriyle kimlik doğrulama hakkında bilgi edinmek için bkz. [olay teslimi kimlik doğrulaması](security-authentication.md) . 