---
title: Azure Event Grid güvenlik ve kimlik doğrulaması
description: Bu makalede, Event Grid kaynaklarınıza (Web kancası, abonelikler, özel konular) erişimin kimliklerinin nasıl doğrulanmasıyla ilgili farklı yollar açıklanmaktadır
services: event-grid
author: femila
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: femila
ms.openlocfilehash: 8335d5a41dc2f322623c163e08f8a4a2c1be8360
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84559005"
---
# <a name="authenticating-access-to-azure-event-grid-resources"></a>Azure Event Grid kaynaklarına erişim izni doğrulanıyor
Bu makalede aşağıdaki senaryolar hakkında bilgi verilmektedir:  

- Paylaşılan erişim Imzasını (SAS) veya anahtarı kullanarak Azure Event Grid konularına olay yayınlayan istemcilerin kimliğini doğrulayın. 
- Azure Active Directory (Azure AD) veya paylaşılan bir gizli dizi kullanarak Event Grid olayları almak için kullanılan Web kancası uç noktasının güvenliğini sağlayın.

## <a name="authenticate-publishing-clients-using-sas-or-key"></a>SAS veya anahtar kullanarak yayımlama istemcilerinin kimliğini doğrulama
Özel konular, paylaşılan erişim Imzası (SAS) veya anahtar kimlik doğrulaması kullanır. SAS önerilir, ancak anahtar kimlik doğrulaması basit programlama sağlar ve birçok mevcut Web kancası yayımcısıyla uyumludur.

Kimlik doğrulama değerini HTTP üstbilgisine dahil edersiniz. SAS için, üst bilgi değeri için **AEG-SAS-Token** ' ı kullanın. Anahtar kimlik doğrulaması için, üst bilgi değeri için **AEG-SAS-Key** kullanın.

### <a name="key-authentication"></a>Anahtar kimlik doğrulaması

Anahtar kimlik doğrulaması en basit kimlik doğrulama biçimidir. İleti üstbilgisinde şu biçimi kullanın: `aeg-sas-key: <your key>` .

Örneğin, şunu içeren bir anahtar geçirirsiniz:

```
aeg-sas-key: XXXXXXXX53249XX8XXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

Ayrıca, `aeg-sas-key` bir sorgu parametresi olarak belirtebilirsiniz. 

```
https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01&&aeg-sas-key=XXXXXXXX53249XX8XXXXX0GXXX/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>SAS belirteçleri

Event Grid için SAS belirteçleri, kaynak, sona erme saati ve imza içerir. SAS belirtecinin biçimi: `r={resource}&e={expiration}&s={signature}` .

Kaynak, olayları gönderdiğiniz olay Kılavuzu konusunun yoludur. Örneğin, geçerli bir kaynak yolu: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01` . Desteklenen tüm API sürümlerini görmek için bkz. [Microsoft. EventGrid kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions). 

İmza bir anahtardan oluşturulur.

Örneğin, geçerli bir **AEG-SAS-Token** değeri:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

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

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Event Grid hizmeti tarafından diske yazılan tüm olaylar veya veriler, bekleyen bir şekilde şifrelendiğinden emin olmak için Microsoft tarafından yönetilen bir anahtarla şifrelenir. Ayrıca, olayların veya verilerin saklanacağı en uzun süre, [Event Grid yeniden deneme ilkesiyle](delivery-and-retry.md)ilgili olarak 24 saat olur. Event Grid, 24 saat sonra tüm olayları veya verileri otomatik olarak siler veya olayın yaşam süresi (hangisi daha az) olur.

## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>Web kancası uç noktalarına olay teslimi kimlik doğrulaması
Aşağıdaki bölümlerde, Web kancası uç noktalarına olay tesliminin nasıl doğrulanabilmesi anlatılmaktadır. Kullandığınız yöntemden bağımsız olarak bir doğrulama el sıkışma mekanizması kullanmanız gerekir. Ayrıntılar için bkz. [Web kancası olay teslimi](webhook-event-delivery.md) . 

### <a name="using-azure-active-directory-azure-ad"></a>Azure Active Directory kullanma (Azure AD)
Azure AD 'yi kullanarak Event Grid olayları almak için kullanılan Web kancası uç noktasının güvenliğini sağlayabilirsiniz. Bir Azure AD uygulaması oluşturmanız, uygulamanızda yetkilendirme Event Grid bir rol ve hizmet sorumlusu oluşturmanız ve olay aboneliğini Azure AD uygulamasını kullanacak şekilde yapılandırmanız gerekir. [Event Grid Azure Active Directory yapılandırmayı](secure-webhook-delivery.md)öğrenin.

### <a name="using-client-secret-as-a-query-parameter"></a>Sorgu parametresi olarak istemci gizli anahtarını kullanma
Ayrıca, olay aboneliği oluşturmanın bir parçası olarak belirtilen Web kancası hedef URL 'sine sorgu parametreleri ekleyerek Web kancası uç noktanızı da güvenli hale getirebilirsiniz. Sorgu parametrelerinden birini, [erişim belirteci](https://en.wikipedia.org/wiki/Access_token) veya paylaşılan gizlilik gibi bir istemci gizli anahtarı olacak şekilde ayarlayın. Event Grid hizmet, Web kancasına her olay teslim isteğindeki tüm sorgu parametrelerini içerir. Web kancası hizmeti gizli dizi alabilir ve doğrulayabilir. İstemci parolası güncelleştirilirse olay aboneliğinin da güncelleştirilmesi gerekir. Bu gizli anahtar sırasında teslimat hatalarından kaçınmak için, Web kancasının, olay aboneliğini yeni gizli anahtar ile güncelleştirmeden önce sınırlı bir süre için hem eski hem de yeni gizli dizileri kabul etmesini sağlayın. 

Sorgu parametreleri, istemci gizli dizileri içerebildiği için, bunlar daha fazla dikkatli işlenir. Bunlar şifreli olarak depolanır ve hizmet işleçlerine erişemez. Hizmet günlüklerinin/izlemelerinin bir parçası olarak günlüğe kaydedilmez. Olay aboneliği özellikleri alınırken, hedef sorgu parametreleri varsayılan olarak döndürülmez. Örneğin: [--Include-Full-Endpoint-URL](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) parametresi Azure [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)'da kullanılır.

Web kancalarına olay sunma hakkında daha fazla bilgi için bkz. [Web kancası olay teslimi](webhook-event-delivery.md)

> [!IMPORTANT]
Azure Event Grid yalnızca **https** Web kancası uç noktalarını destekler. 

## <a name="next-steps"></a>Sonraki adımlar

- Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
