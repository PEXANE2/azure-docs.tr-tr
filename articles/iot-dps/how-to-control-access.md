---
title: IoT Cihaz Sağlama Hizmeti'nde güvenlik bitiş noktaları | Microsoft Dokümanlar
description: Kavramlar - arka uç uygulamaları için IoT Cihaz Sağlama Hizmeti'ne (DPS) erişimin nasıl kontrol edilebildiğini. Güvenlik belirteçleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: wesmc
ms.openlocfilehash: 2a7e0932d226b1533c039b8529c2c11de06cf525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79285155"
---
# <a name="control-access-to-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Aygıt Sağlama Hizmetine erişimi denetleme

Bu makalede, IoT Aygıt Sağlama hizmetinizi güvence altına alma seçenekleri açıklanmaktadır. Sağlama hizmeti, her bitiş noktasına erişim sağlamak için *izinleri* kullanır. İzinler, işlevselliği temel alan bir hizmet örneğine erişimi sınırlar.

Bu makalede açıklanır:

* Sağlama hizmetinize erişmek için bir arka uç uygulamasına verebileceğiniz farklı izinler.
* Kimlik doğrulama işlemi ve izinleri doğrulamak için kullandığı belirteçler.

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Sağlama hizmeti bitiş noktalarından herhangi biri sine erişmek için uygun izinlere sahip olmalısınız. Örneğin, bir arka uç uygulaması, hizmete gönderdiği her iletiyle birlikte güvenlik kimlik bilgilerini içeren bir belirteç içermelidir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinler

[İzinleri](#device-provisioning-service-permissions) aşağıdaki yollarla verebilirsiniz:

* **Paylaşılan erişim yetkilendirme ilkeleri.** Paylaşılan erişim ilkeleri, herhangi bir [izin](#device-provisioning-service-permissions)bileşimi verebilir. [Azure portalında][lnk-management-portal]veya [Aygıt Sağlama Hizmeti REST API'lerini][lnk-resource-provider-apis]kullanarak programlı bir şekilde ilkeleri tanımlayabilirsiniz. Yeni oluşturulan bir sağlama hizmeti aşağıdaki varsayılan ilkeyi vardır:

* **provisioningserviceowner**: Tüm izinleri ile politika.

> [!NOTE]
> Ayrıntılı bilgi için [izinlere](#device-provisioning-service-permissions) bakın.

## <a name="authentication"></a>Kimlik doğrulaması

Azure IoT Hub Aygıt Sağlama Hizmeti, paylaşılan erişim ilkelerine karşı bir belirteç doğrulayarak uç noktalara erişim sağlar. Simetrik anahtarlar gibi güvenlik kimlik bilgileri hiçbir zaman tel üzerinden gönderilmez.

> [!NOTE]
> Azure [Kaynak Yöneticisi'ndeki][lnk-azure-resource-manager]tüm sağlayıcılar gibi Aygıt Sağlama Hizmeti kaynak sağlayıcısı da Azure aboneliğiniz aracılığıyla güvence altına alınmıştır.

Güvenlik belirteçlerinin nasıl oluşturup kullanılacağı hakkında daha fazla bilgi için bir sonraki bölüme bakın.

HTTP desteklenen tek protokoldür ve **Yetkilendirme** isteği üstbilgisine geçerli bir belirteç ekleyerek kimlik doğrulaması uygular.

#### <a name="example"></a>Örnek
```csharp
SharedAccessSignature sr = 
   mydps.azure-devices-provisioning.net&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501&skn=provisioningserviceowner`\
```

> [!NOTE]
> [Azure IoT Aygıt Sağlama Hizmeti SDK'ları,][lnk-sdks] hizmete bağlanırken otomatik olarak belirteçler oluşturur.

## <a name="security-tokens"></a>Güvenlik belirteçleri

Aygıt Sağlama Hizmeti, kabloya anahtar göndermemek için hizmetlerin doğruluğunu doğrulamak için güvenlik belirteçleri kullanır. Ayrıca, güvenlik belirteçleri zaman geçerliliği ve kapsamı sınırlıdır. [Azure IoT Aygıt Sağlama Hizmeti SDK'ları,][lnk-sdks] özel bir yapılandırma gerektirmeden otomatik olarak belirteçler oluşturur. Bazı senaryolar, güvenlik belirteçlerini doğrudan oluşturmanızı ve kullanmanızı gerektirir. Bu tür senaryolar, HTTP yüzeyinin doğrudan kullanımını içerir.

### <a name="security-token-structure"></a>Güvenlik belirteç yapısı

IoT Aygıt Sağlama Hizmeti'ndeki belirli işlevlere hizmetleriçin zamana bağlı erişim sağlamak için güvenlik belirteçlerini kullanırsınız. Sağlama hizmetine bağlanmak için yetki almak için, hizmetlerin paylaşılan bir erişim veya simetrik anahtarla imzalanmış güvenlik belirteçleri göndermesi gerekir.

Paylaşılan erişim anahtarıyla imzalanmış bir belirteç, paylaşılan erişim ilkesi izinleriyle ilişkili tüm işlevlere erişim sağlar. 

Güvenlik belirteci aşağıdaki biçimi vardır:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imza} |Formun Bir HMAC-SHA256 imza `{URL-encoded-resourceURI} + "\n" + expiry`dizesi: . **Önemli**: Anahtar base64'ten deşifre edilir ve HMAC-SHA256 hesaplamasını gerçekleştirmek için anahtar olarak kullanılır.|
| {son kullanma tarihi} |UTF8 dizeleri 1 Ocak 1970 tarihinde 00:00:00 UTC beri saniye sayısı için. |
| {URL kodlanmış-resourceURI} | Küçük harf kaynağı URI'nin küçük harfurl kodlaması. Uri öneki (segmente göre) bu belirteç ile erişilebilen uç noktaların, IoT Aygıt Sağlama Hizmeti (protokol yok) ana adı ile başlayan. Örneğin, `mydps.azure-devices-provisioning.net`. |
| {policyName} |Bu belirteç için başvuran paylaşılan erişim ilkesinin adı. |

**Önek üzerine not**: URI öneki karaktere göre değil, segmente göre hesaplanır. Örneğin `/a/b` bir `/a/b/c` önek tir, `/a/bc`ancak için değil.

Aşağıdaki Düğüm.js snippet girişleri `resourceUri, signingKey, policyName, expiresInMins`belirteci bilgisayara **SasToken adlı** bir işlev gösterir. Sonraki bölümlerde, farklı belirteç kullanım örnekleri için farklı girdilerin nasıl başharflere yararlanılakullanılacağı ayrıntılı olarak açıklanmıştır.

```javascript
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires + "&skn="+ policyName;
    return token;
};
```

Karşılaştırma olarak, bir güvenlik belirteci oluşturmak için eşdeğer Python kodu:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl)),
        'skn' : policy_name
    }

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Belirteç zaman geçerliliği IoT Cihaz Sağlama Hizmeti makinelerinde doğrulanır olduğundan, belirteci oluşturan makinenin saatindeki sürüklenme en az olmalıdır.

### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenlerinden gelen güvenlik belirteçlerini kullanma

Hizmet bileşenleri, yalnızca daha önce açıklandığı gibi uygun izinleri veren paylaşılan erişim ilkelerini kullanarak güvenlik belirteçleri oluşturabilir.

Uç noktalarda açığa çıkan hizmet işlevleri şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{your-service}.azure-devices-provisioning.net/enrollments` |Aygıt Sağlama Hizmeti ile cihaz kayıt işlemleri sağlar. |
| `{your-service}.azure-devices-provisioning.net/enrollmentGroups` |Aygıt kayıt gruplarını yönetmek için işlemler sağlar. |
| `{your-service}.azure-devices-provisioning.net/registrations/{id}` |Cihaz kayıtlarının durumunu almak ve yönetmek için işlemler sağlar. |


Örnek olarak, **enrollmentread** adı verilen önceden oluşturulmuş paylaşılan erişim ilkesi kullanılarak oluşturulan bir hizmet, aşağıdaki parametrelere sahip bir belirteç oluşturur:

* kaynak URI: `{mydps}.azure-devices-provisioning.net`,
* imzalama anahtarı: `enrollmentread` ilke nin anahtarlarından biri,
* politika adı: `enrollmentread`,
* herhangi bir son kullanma süresi.backn

![Portalda Cihaz Sağlama hizmeti örneğiniz için paylaşılan bir erişim ilkesi oluşturun][img-add-shared-access-policy]

```javascript
var endpoint ="mydps.azure-devices-provisioning.net";
var policyName = 'enrollmentread'; 
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm kayıt kayıtlarını okumak için erişim sağlayacak sonuç:

`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=enrollmentread`

## <a name="reference-topics"></a>Referans konuları:

Aşağıdaki başvuru konuları, IoT Aygıt Sağlama Hizmetinize erişimi denetleme hakkında daha fazla bilgi sağlar.

### <a name="device-provisioning-service-permissions"></a>Cihaz Sağlama Hizmeti izinleri

Aşağıdaki tabloda, IoT Aygıt Sağlama Hizmetinize erişimi denetlemek için kullanabileceğiniz izinler listelenir.

| İzin | Notlar |
| --- | --- |
| **Serviceconfig** |Hizmet yapılandırmalarını değiştirmeye erişim sağlar. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **KayıtOkuma** |Verilenler, aygıt kayıtlarına ve kayıt gruplarına erişim okunur. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **KayıtYazma** |Aygıt kayıtlarıve kayıt gruplarına erişim sağlar. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **Kayıt DurumuOku** |Aygıt kayıt durumuna okuma erişimi sağlar. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **Kayıt DurumuYazma**  |Aygıt kayıt durumuna erişimi silme izni verir. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |

<!-- links and images -->

[img-add-shared-access-policy]: ./media/how-to-control-access/how-to-add-shared-access-policy.PNG
[lnk-sdks]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-management-portal]: https://portal.azure.com
[lnk-azure-resource-manager]: ../azure-resource-manager/management/overview.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iot-dps/
