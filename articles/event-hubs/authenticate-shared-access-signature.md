---
title: Paylaşılan erişim imzalarıyla Azure Etkinlik Hub'larına erişimi kimlik doğrulaması
description: Bu makalede, paylaşılan erişim imzalarını kullanarak Olay Hub'ları kaynaklarına erişimin nasıl doğruluğunu doğruladığınız gösterilmektedir.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: spelluru
ms.openlocfilehash: 82773ab6decfe15ee1a9d839a1f10a158ae72c42
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521312"
---
# <a name="authenticate-access-to-event-hubs-resources-using-shared-access-signatures-sas"></a>Paylaşılan erişim imzalarını (SAS) kullanarak Olay Hubkaynaklarına erişimi doğrulama
Paylaşılan erişim imzası (SAS), paylaşılan erişim imzasına sahip istemcilere sağladığınız erişim türü üzerinde ayrıntılı denetim sağlar. SAS'ta ayarlayabildiğiniz denetimlerden bazıları şunlardır: 

- Başlangıç saati ve son kullanma tarihi de dahil olmak üzere SAS'ın geçerli olduğu aralık.
- SAS tarafından verilen izinler. Örneğin, Olay Hub'ları ad alanı için bir SAS dinleme izni verebilir, ancak gönderme izni vermez.
- Yalnızca geçerli kimlik bilgileri sunan istemciler bir olay merkezine veri gönderebilir.
- İstemci başka bir istemcinin kimliğine bürünemez.
- Bir rouge istemcisi bir olay hub'ına veri göndermesi engellenebilir.

Bu makalede, SAS kullanarak Olay Hub kaynaklarına erişim kimlik doğrulaması kapsar. SAS kullanarak Olay Hub kaynaklarına erişim **yetkisi hakkında** bilgi edinmek için [bu makaleye](authorize-access-shared-access-signature.md)bakın. 

> [!NOTE]
> Microsoft, azure AD kimlik bilgilerini, daha kolay tehlikeye girebilecek paylaşılan erişim imzalarını kullanmak yerine, mümkün olduğunda güvenlik en iyi uygulaması olarak kullanmanızı önerir. Azure AD, Etkinlik Hub'ları kaynaklarınıza ayrıntılı erişim sağlamak için paylaşılan erişim imzalarını (SAS) kullanmaya devam edebilirken, SAS belirteçlerini yönetmeye veya gizliliği ihlal edilmiş bir SAS'ı iptal etme endişesi duymadan benzer özellikler sunar.
> 
> Azure Etkinlik Hub'larında Azure AD tümleştirmesi hakkında daha fazla bilgi için Azure [AD'yi kullanarak Etkinlik Hub'larına erişimi yetkilendirme konusuna](authorize-access-azure-active-directory.md)bakın. 


## <a name="configuring-for-sas-authentication"></a>SAS kimlik doğrulaması için yapılandırma
EventHubs paylaşılan erişim yetkilendirme kuralını bir Olay Hub'ları ad alanında veya bir varlıkta (olay hub örneği veya kafka topic'te olay hub'ında) yapılandırabilirsiniz. Bir tüketici grubunda paylaşılan erişim yetkilendirme kuralını yapılandırmak şu anda desteklenmez, ancak tüketici grubuna erişimi güvence altına almak için ad alanı veya varlık üzerinde yapılandırılan kuralları kullanabilirsiniz. 

Aşağıdaki resimde, yetkilendirme kurallarının örnek varlıklarüzerinde nasıl uygulandığı gösterilmektedir. 

![Yetkilendirme kuralını yapılandırma](./media/authenticate-shared-access-signature/configure-sas-authorization-rule.png)

Bu örnekte, örnek Olay Hub'ları ad alanı (ExampleNamespace) iki varlık vardır: eh1 ve topic1. Yetkilendirme kuralları hem varlık düzeyinde hem de ad alanı düzeyinde tanımlanır.  

ManageRuleNS, sendRuleNS ve listenRuleNS yetkilendirme kuralları hem olay merkezi örneği eh1 hem de konu t1 için geçerlidir. listenRule-eh ve sendRule-eh yetkilendirme kuralları yalnızca olay merkezi örneği eh1 için geçerlidir ve sendRuleT yetkilendirme kuralı yalnızca konu başlığı1 için geçerlidir. 

sendRuleNS yetkilendirme kuralını kullanırken, istemci uygulamaları hem eh1 hem de topic1'e gönderebilir. sendRuleT yetkilendirme kuralı kullanıldığında, yalnızca topic1'e parçalı erişimi zorlar ve bu nedenle bu kuralı kullanarak erişim için istemci uygulamaları artık eh1'e gönderemez, ancak yalnızca topic1'e.

## <a name="generate-a-shared-access-signature-token"></a>Paylaşılan Erişim İmza belirteci oluşturma 
Yetkilendirme kuralı adı ve imza anahtarlarından birine erişimi olan herhangi bir istemci bir SAS belirteci oluşturabilir. Belirteç aşağıdaki biçimde bir dize oluşturularak oluşturulur:

- `se`– Belirteç son kullanma anında. 1 Ocak 1970 tarihinde utc 00:00:00 (UNIX dönemi) zaman belirteç sona erdiğinde niçin zaman gelen saniye yansıtan
- `skn`– Yetkilendirme kuralının adı, yani SAS anahtar adıdır.
- `sr`– Erişilen kaynağın URI'si.
- `sig`– İmza.

İmza dizesi, KAYNAK URI (önceki bölümde açıklandığı gibi kapsam) üzerinden hesaplanan SHA-256 karma ve CRLF ile ayrılmış belirteç son kullanma anının dize gösterimidir.

Karma hesaplama aşağıdaki sözde koda benzer ve 256-bit/32-byte karma değeri döndürür. 

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Belirteç, alıcının karmayı aynı parametrelerle yeniden hesaplayabilmek için karma olmayan değerleri içerir ve verenin geçerli bir imzalama anahtarına sahip olduğunu doğrular.

Kaynak URI, erişimin talep edildiği Servis Veri Günü kaynağının tam URI'sidir. Örneğin, http://<namespace><entityPath> .servicebus.windows.net/ `sb://<namespace>.servicebus.windows.net/<entityPath>;` veya `http://contoso.servicebus.windows.net/eventhubs/eh1`yani.

URI yüzde kodlanmış olmalı.

İmzalama için kullanılan paylaşılan erişim yetkilendirme kuralı, bu URI tarafından belirtilen varlık veya hiyerarşik ebeveynlerinden biri tarafından yapılandırılmalıdır. Örneğin, `http://contoso.servicebus.windows.net/eventhubs/eh1` ya `http://contoso.servicebus.windows.net` da önceki örnekte.

SAS belirteci, imza dizesinde <resourceURI> kullanılan tüm kaynaklar için geçerlidir.

> [!NOTE]
> Paylaşılan erişim ilkesini kullanarak Olay Hub'ları için bir erişim jetonu oluşturursunuz. Daha fazla bilgi için [Paylaşılan erişim yetkilendirme ilkesine](authorize-access-shared-access-signature.md#shared-access-authorization-policies)bakın.

### <a name="generating-a-signaturetoken-from-a-policy"></a>İlkeden imza (belirteç) oluşturma 
Aşağıdaki bölümde paylaşılan erişim imza ilkeleri kullanılarak bir SAS belirteci oluşturma gösterir,

#### <a name="nodejs"></a>NodeJS

```javascript
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
```

#### <a name="java"></a>Java

```java
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```
#### <a name="php"></a>PHP

```php
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
    $targetUri = strtolower(rawurlencode(strtolower($uri))); 
    $expires = time();  
    $expiresInMins = 60; 
    $week = 60*60*24*7;
    $expires = $expires + $week; 
    $toSign = $targetUri . "\n" . $expires; 
    $signature = rawurlencode(base64_encode(hash_hmac('sha256',             
     $toSign, $sasKeyValue, TRUE))); 
    
    $token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
    return $token; 
}
```

#### <a name="c"></a>C#

```csharp
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="authenticating-event-hubs-publishers-with-sas"></a>SAS ile Etkinlik Hub'ları yayıncılarının kimlik doğrulaması 
Olay yayımcısı, bir olay hub'ı için sanal bir bitiş noktası tanımlar. Yayımcı yalnızca bir olay merkezine ileti göndermek ve ileti almak için kullanılabilir.

Genellikle, bir olay hub istemci başına bir yayımcı kullanır. Bir olay hub'ının yayımcılarından herhangi insine gönderilen tüm iletiler bu olay hub'ında sıraya alınır. Yayımcılar ince taneli erişim denetimi sağlar.

Her Olay Hub istemcisi istemciye yüklenen benzersiz bir belirteç atanır. Belirteçler, her benzersiz belirteç farklı benzersiz yayımcıya erişim sağlayacak şekilde üretilir. Belirteci olan bir istemci yalnızca bir yayımcıya gönderebilir ve başka bir yayımcıya gönderemez. Birden çok istemci aynı belirteci paylaşıyorsa, her biri yayımcıyı paylaşır.

Tüm belirteçler SAS tuşları ile atanır. Genellikle, tüm belirteçleri aynı anahtarla imzalanır. İstemciler, müşterilerin belirteçür üretmesini engelleyen anahtarın farkında değildir. İstemciler, süresi dolana kadar aynı belirteçlerle çalışır.

Örneğin, yalnızca Olay Hub'larına gönderme/yayımlama kapsamına giren yetkilendirme kurallarını tanımlamak için bir gönderme yetkilendirme kuralı tanımlamanız gerekir. Bu, ad alanı düzeyinde yapılabilir veya belirli bir varlığa (olay hub'ları örneği veya bir konu) daha ayrıntılı kapsam verebilir. Bu tür parçalı erişimle kapsama sahip bir istemci veya uygulama, Olay Hub'ları yayımcısı olarak adlandırılır. Bunu yapmak için şu adımları uygulayın:

1. Yayımlamak istediğiniz varlık üzerinde **gönderme** kapsamını atamak için bir SAS anahtarı oluşturun. Daha fazla bilgi için [Paylaşılan erişim yetkilendirme ilkelerine](authorize-access-shared-access-signature.md#shared-access-authorization-policies)bakın.
2. Adım1'de oluşturulan anahtarı kullanarak belirli bir yayımcı için son kullanma süresine sahip bir SAS belirteci oluşturun.

    ```csharp
    var sasToken = SharedAccessSignatureTokenProvider.GetPublisherSharedAccessSignature(
                new Uri("Service-Bus-URI"),
                "eventub-name",
                "publisher-name",
                "sas-key-name",
                "sas-key",
                TimeSpan.FromMinutes(30));
    ```
3. Belirteci yayımcı istemcisine sağlayın, bu da yalnızca belirteç erişim sağlayan varlık ve yayımcıya gönderebilirsiniz.

    Belirteç süresi dolduktan sonra, istemci varlığa gönderme/yayımlama erişimini kaybeder. 


> [!NOTE]
> Önerilmese de, aygıtları bir olay hub'ına veya ad alanına erişim sağlayan belirteçlerle donatmak mümkündür. Bu belirteci tutan herhangi bir aygıt, iletileri doğrudan bu olay merkezine gönderebilir. Ayrıca, aygıt bu olay hub'ına gönderilmesinden kara listeye alınamaz.
> 
> Her zaman belirli ve tanecikli kapsamlar vermek önerilir.

> [!IMPORTANT]
> Belirteçler oluşturulduktan sonra, her istemci kendi benzersiz belirteci ile birlikte verilir.
>
> İstemci verileri bir olay hub'ına gönderdiğinde, isteğini belirteçle etiketler. Bir saldırganın jetonu gizlice dinlemesini ve çalmasını önlemek için istemci ve olay merkezi arasındaki iletişimin şifreli bir kanal üzerinden gerçekleşmesi gerekir.
> 
> Bir belirteç bir saldırgan tarafından çalınırsa, saldırgan belirteci çalınan istemcinin kimliğine bürünebilir. Bir yayımcıyı kara listeye almak, istemciyi farklı bir yayımcı kullanan yeni bir belirteç alana kadar kullanılamaz hale getirir.


## <a name="authenticating-event-hubs-consumers-with-sas"></a>SAS ile Etkinlik Hub'larının doğrulanması 
Olay Hub'ları üreticileri tarafından oluşturulan verilerden tüketilen arka uç uygulamaları doğrulamak için, Olay Hub'ları belirteç kimlik doğrulaması, istemcilerinin **yönetim** haklarına veya Olay Hub'ına ad alanı veya olay hub örneği veya konusuna atanan **dinleme** ayrıcalıklarına sahip olmasını gerektirir. Veriler, tüketici grupları kullanılarak Olay Hub'larından tüketilir. SAS ilkesi size parçalı kapsam sağlarken, bu kapsam tüketici düzeyinde değil, yalnızca varlık düzeyinde tanımlanır. Ad alanı düzeyinde veya olay merkezi örneği veya konu düzeyinde tanımlanan ayrıcalıkların o varlığın tüketici gruplarına uygulanacağı anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [SAS kullanarak yetkilendirme](authenticate-shared-access-signature.md)
- [Rol temel erişim denetimini (RBAC) kullanarak yetkilendirme](authenticate-shared-access-signature.md)
- [Etkinlik Hub'ları hakkında daha fazla bilgi edinin](event-hubs-about.md)

Aşağıdaki ilgili makalelere bakın:

- [Azure Etkin Dizin'i kullanarak bir uygulamadan Azure Etkinlik Hub'larına yönelik istekleri kimlik doğrulaması](authenticate-application.md)
- [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](authenticate-managed-identity.md)
- [Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md)
- [Paylaşılan Erişim İmzalarını kullanarak Etkinlik Hubkaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md)