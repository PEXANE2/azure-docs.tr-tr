---
title: Azure IoT Hub güvenliğini anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıt uygulamaları ve arka uç uygulamaları için IoT Hub'a erişimi nasıl kontrol edebilirsiniz. Güvenlik belirteçleri ve X.509 sertifikaları desteği hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: d37320ad6e8caf3300756466eabe8de29a339c4a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258295"
---
# <a name="control-access-to-iot-hub"></a>IoT Hub’a erişimi denetleme

Bu makalede, IoT hub'ınızı güvence altına alma seçenekleri açıklanmaktadır. IoT Hub, her IoT hub bitiş noktasına erişim sağlamak için *izinleri* kullanır. İzinler işlevselliği temel alan bir IoT hub'ına erişimi sınırlar.

Bu makalede tanıtıyor:

* IoT hub'ınıza erişmek için bir aygıta veya arka uç uygulamasına verebileceğiniz farklı izinler.
* Kimlik doğrulama işlemi ve izinleri doğrulamak için kullandığı belirteçler.
* Belirli kaynaklara erişimi sınırlamak için kimlik bilgilerini kapsama nasıl.
* X.509 sertifikaları için IoT Hub desteği.
* Varolan aygıt kimlik kayıtları veya kimlik doğrulama düzenlerini kullanan özel aygıt kimlik doğrulama mekanizmaları.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub uç noktalarından herhangi biri sine erişmek için uygun izinlere sahip olmalısınız. Örneğin, bir aygıt, IoT Hub'a gönderdiği her iletiyle birlikte güvenlik kimlik bilgilerini içeren bir belirteç içermelidir.

## <a name="access-control-and-permissions"></a>Erişim denetimi ve izinler

[İzinleri](#iot-hub-permissions) aşağıdaki yollarla verebilirsiniz:

* **IoT hub düzeyinde paylaşılan erişim ilkeleri.** Paylaşılan erişim ilkeleri, herhangi bir [izin](#iot-hub-permissions)bileşimi verebilir. [IoT Hub Kaynak REST API'lerini](/rest/api/iothub/iothubresource)kullanarak veya [az iot hub ilkesi](/cli/azure/iot/hub/policy?view=azure-cli-latest) CLI'yi kullanarak Azure [portalındaki](https://portal.azure.com)ilkeleri programlı olarak tanımlayabilirsiniz. Yeni oluşturulan Bir IoT hub'ı aşağıdaki varsayılan ilkelere sahiptir:
  
  | Paylaşılan Erişim İlkesi | İzinler |
  | -------------------- | ----------- |
  | iothubowner | Tüm izinler |
  | hizmet | **ServiceConnect** izinleri |
  | cihaz | **DeviceConnect** izinleri |
  | kayıt DefteriRead | **Kayıt DefteriOkuma** izinleri |
  | kayıt defteriReadWrite | **Kayıt DefteriOkuma** ve **Kayıt İzinleri Yazma** |

* **Aygıt Başına Güvenlik Kimlik Bilgileri.** Her IoT Hub'ı bir [kimlik kaydı](iot-hub-devguide-identity-registry.md) içerir Bu kimlik kayıt defterindeki her aygıt için, **DeviceConnect** izinlerini ilgili aygıt uç noktalarına göre alan güvenlik kimlik bilgilerini yapılandırabilirsiniz.

Örneğin, tipik bir IoT çözümünde:

* Aygıt yönetimi bileşeni *kayıt defteriReadWrite* ilkesini kullanır.
* Olay işlemci bileşeni *hizmet* ilkesini kullanır.
* Çalışma zamanı aygıt iş mantığı bileşeni *hizmet* ilkesini kullanır.
* Tek tek aygıtlar, IoT hub'ın kimlik kayıt defterinde depolanan kimlik bilgilerini kullanarak bağlanır.

> [!NOTE]
> Ayrıntılı bilgi için [izinlere](#iot-hub-permissions) bakın.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure IoT Hub, bir belirteci paylaşılan erişim ilkeleri ve kimlik kayıt defteri güvenlik kimlik bilgileri ile doğrulayarak uç noktalara erişim izni verir.

Simetrik anahtarlar gibi güvenlik kimlik bilgileri hiçbir zaman tel üzerinden gönderilmez.

> [!NOTE]
> Azure IoT Hub kaynak sağlayıcısı, [Azure Kaynak Yöneticisi'ndeki](../azure-resource-manager/management/overview.md)tüm sağlayıcılar gibi Azure aboneliğiniz aracılığıyla güvence altına alınır.

Güvenlik belirteçlerinin nasıl oluşturup kullanılacağı hakkında daha fazla bilgi için [IoT Hub güvenlik belirteçleri'ne](iot-hub-devguide-security.md#security-tokens)bakın.

### <a name="protocol-specifics"></a>Protokol özellikleri

MQTT, AMQP ve HTTPS gibi desteklenen her protokol belirteçleri farklı şekillerde taşır.

MQTT kullanırken, CONNECT paketinde ClientId `{iothubhostname}/{deviceId}` olarak kullanıcı adı alanında aygıt kimliği ve Parola alanında bir SAS belirteci vardır. `{iothubhostname}`IoT hub'ının tam CName'si olmalıdır (örneğin, contoso.azure-devices.net).

[AMQP](https://www.amqp.org/)kullanırken, IoT Hub [SASL PLAIN](https://tools.ietf.org/html/rfc4616) ve [AMQP Alacak tabanlı-Güvenlik](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc)destekler.

AMQP talep tabanlı güvenlik kullanıyorsanız, standart bu belirteçleri nasıl ileteceklerini belirtir.

SASL PLAIN için **kullanıcı adı:**

* `{policyName}@sas.root.{iothubName}`IoT hub düzeyi belirteçleri kullanıyorsanız.
* `{deviceId}@sas.{iothubname}`aygıt kapsamlı belirteçleri kullanıyorsanız.

Her iki durumda da, parola [alanı, IoT Hub güvenlik belirteçlerinde](iot-hub-devguide-security.md#security-tokens)açıklandığı gibi belirteci içerir.

HTTPS, **Yetkilendirme** isteği üstbilgisine geçerli bir belirteç ekleyerek kimlik doğrulaması uygular.

#### <a name="example"></a>Örnek

Kullanıcı adı (DeviceId büyük/küçük harf duyarlıdır):`iothubname.azure-devices.net/DeviceId`

Parola (CLI uzantı komutu [az iot hub oluşturma-sas-belirteci](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token)veya [Visual Studio Code için Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile bir SAS belirteci oluşturabilirsiniz):

`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> [Azure IoT SDK'ları](iot-hub-devguide-sdks.md) hizmete bağlanırken otomatik olarak belirteçler oluşturur. Bazı durumlarda, Azure IoT SDK'ları tüm protokolleri veya tüm kimlik doğrulama yöntemlerini desteklemez.

### <a name="special-considerations-for-sasl-plain"></a>SASL PLAIN için özel hususlar

AMQP ile SASL PLAIN kullanırken, bir IoT hub'ına bağlanan bir istemci her TCP bağlantısı için tek bir belirteç kullanabilir. Belirteç süresi dolduğunda, TCP bağlantısı hizmetbağlantısını keser ve yeniden bağlantıyı tetikler. Bu davranış, bir arka uç uygulaması için sorunlu olmasa da, aşağıdaki nedenlerden dolayı bir aygıt uygulaması için zararlıdır:

* Ağ geçitleri genellikle birçok aygıt adına bağlanır. SASL PLAIN kullanırken, bir IoT hub'ına bağlanan her aygıt için ayrı bir TCP bağlantısı oluşturmaları gerekir. Bu senaryo, güç ve ağ kaynakları tüketimini önemli ölçüde artırır ve her aygıt bağlantısının gecikmesini artırır.

* Kaynak kısıtlamalı aygıtlar, her belirteç sona erdikten sonra yeniden bağlanmak için kaynakların artan kullanımından olumsuz etkilenir.

## <a name="scope-iot-hub-level-credentials"></a>Kapsam IoT hub düzeyi kimlik bilgileri

Sınırlı bir kaynak URI ile belirteçleri oluşturarak IoT hub düzeyinde güvenlik ilkelerini kapsam dışı edebilirsiniz. Örneğin, aygıttan aygıttan buluta ileti göndermenin bitiş noktası **/devices/{deviceId}/messages/events**'tir. Ayrıca, **resourceURI/devices/{deviceId}** olan bir belirteç imzalamak için **DeviceConnect** izinleri ile IoT hub düzeyinde paylaşılan bir erişim ilkesi de kullanabilirsiniz. Bu yaklaşım, yalnızca **aygıt aygıtId**adına ileti göndermek için kullanılabilir bir belirteç oluşturur.

Bu mekanizma Olay [Hub'ları yayımcı ilkesine](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab)benzer ve özel kimlik doğrulama yöntemleri uygulamanızı sağlar.

## <a name="security-tokens"></a>Güvenlik belirteçleri

IoT Hub, kabloya anahtar göndermemek için aygıtların ve hizmetlerin kimliğini doğrulamak için güvenlik belirteçleri kullanır. Ayrıca, güvenlik belirteçleri zaman geçerliliği ve kapsamı sınırlıdır. [Azure IoT SDK'lar,](iot-hub-devguide-sdks.md) özel yapılandırma gerektirmeden otomatik olarak belirteçler oluşturur. Bazı senaryolar, güvenlik belirteçlerini doğrudan oluşturmanızı ve kullanmanızı gerektirir. Bu tür senaryolar şunlardır:

* MQTT, AMQP veya HTTPS yüzeylerinin doğrudan kullanımı.

* [Özel aygıt kimlik doğrulamasında](iot-hub-devguide-security.md#custom-device-and-module-authentication)açıklandığı gibi belirteç hizmet deseni uygulaması.

IoT Hub ayrıca [x.509 sertifikalarını](iot-hub-devguide-security.md#supported-x509-certificates)kullanarak aygıtların IoT Hub ile kimlik doğrulamasını sağlar.

### <a name="security-token-structure"></a>Güvenlik belirteç yapısı

IoT Hub'daki belirli işlevlere aygıtlara ve hizmetlere zaman alametli erişim sağlamak için güvenlik belirteçlerini kullanırsınız. IoT Hub'a bağlanmak için yetki almak için aygıtların ve hizmetlerin paylaşılan erişim veya simetrik anahtarla imzalanmış güvenlik belirteçleri göndermesi gerekir. Bu anahtarlar kimlik defterinde bir aygıt kimliğiyle depolanır.

Paylaşılan erişim anahtarıyla imzalanmış bir belirteç, paylaşılan erişim ilkesi izinleriyle ilişkili tüm işlevlere erişim sağlar. Aygıt kimliğinin simetrik anahtarıyla imzalanmış bir belirteç yalnızca **deviceconnect'e** ilişkili aygıt kimliği için izin verir.

Güvenlik belirteci aşağıdaki biçimi vardır:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Beklenen değerler şunlardır:

| Değer | Açıklama |
| --- | --- |
| {imza} |Formun Bir HMAC-SHA256 imza `{URL-encoded-resourceURI} + "\n" + expiry`dizesi: . **Önemli**: Anahtar base64'ten deşifre edilir ve HMAC-SHA256 hesaplamasını gerçekleştirmek için anahtar olarak kullanılır. |
| {resourceURI} |Uri öneki (segmente göre) bu belirteç ile erişilebilen uç noktaları, IoT hub (hiçbir protokol) ana adı ile başlayan. Örneğin, `myHub.azure-devices.net/devices/device1` |
| {son kullanma tarihi} |UTF8 dizeleri 1 Ocak 1970 tarihinde 00:00:00 UTC beri saniye sayısı için. |
| {URL kodlanmış-resourceURI} |Küçük harf li kaynak URI'nin küçük harf URL kodlaması |
| {policyName} |Bu belirteç için başvuran paylaşılan erişim ilkesinin adı. Belirteç aygıt kayıt defteri kimlik bilgilerine atıfta bulunuyorsa yok. |

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
    var hmac = crypto.createHmac('sha256', Buffer.from(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Karşılaştırma olarak, bir güvenlik belirteci oluşturmak için eşdeğer Python kodu:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import parse
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((parse.quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key.encode('utf-8'), sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + parse.urlencode(rawtoken)
```

Aşağıda önkoşullar için kurulum talimatları vereme yönergeleri yer almaktadır.

[!INCLUDE [Iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]


C#'da bir güvenlik belirteci oluşturmak için işlevsellik:

```csharp
using System;
using System.Globalization;
using System.Net;
using System.Net.Http;
using System.Security.Cryptography;
using System.Text;

public static string generateSasToken(string resourceUri, string key, string policyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = WebUtility.UrlEncode(resourceUri) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}", WebUtility.UrlEncode(resourceUri), WebUtility.UrlEncode(signature), expiry);

    if (!String.IsNullOrEmpty(policyName))
    {
        token += "&skn=" + policyName;
    }

    return token;
}

```


> [!NOTE]
> Belirteç zaman geçerliliği IoT Hub makinelerinde doğrulanır olduğundan, belirteci oluşturan makinenin saatindeki sürüklenme en az olmalıdır.

### <a name="use-sas-tokens-in-a-device-app"></a>Aygıt uygulamasında SAS belirteçlerini kullanma

Güvenlik belirteçleri ile IoT Hub ile **DeviceConnect** izinleri almak için iki yol vardır: [kimlik kayıt defterinden simetrik](#use-a-symmetric-key-in-the-identity-registry)bir aygıt anahtarı kullanın veya paylaşılan bir [erişim anahtarı](#use-a-shared-access-policy)kullanın.

Cihazlardan erişilebilen tüm işlevlerin önek ile `/devices/{deviceId}`uç noktalarda tasarım tarafından ortaya çıkarılanın caydığını unutmayın.

> [!IMPORTANT]
> IoT Hub'ın belirli bir aygıtı doğrulamasının tek yolu aygıt kimliği simetrik anahtarını kullanmaktır. Aygıt işlevselliğine erişmek için paylaşılan bir erişim ilkesinin kullanıldığı durumlarda, çözüm güvenlik belirteci veren bileşeni güvenilir bir alt bileşen olarak dikkate almalıdır.

Cihaza bakan uç noktalar (protokolden bağımsız olarak):

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Aygıttan buluta iletiler gönderin. |
| `{iot hub host name}/devices/{deviceId}/messages/devicebound` |Buluttan cihaza iletiler alın. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Kimlik defterinde simetrik anahtar kullanma

Bir belirteç oluşturmak için aygıt kimliğinin simetrik anahtarını`skn`kullanırken, belirteçteki ilke Adı ( ) öğesi atlanır.

Örneğin, tüm aygıt işlevlerine erişmek için oluşturulan bir belirteç aşağıdaki parametrelere sahip olmalıdır:

* kaynak URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: `{device id}` kimlik için herhangi bir simetrik anahtar,
* hiçbir ilke adı,
* herhangi bir son kullanma süresi.

Önceki Düğüm.js işlevini kullanan bir örnek:

```javascript
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Aygıt1 için tüm işlevlere erişim sağlayan sonuç şu şekilde olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> CLI uzantı komutu [az iot hub oluşturma-sas-token](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token)veya [Visual Studio Code için Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)ile bir SAS belirteci oluşturmak mümkündür.

### <a name="use-a-shared-access-policy"></a>Paylaşılan erişim ilkesini kullanma

Paylaşılan bir erişim ilkesinden bir belirteç `skn` oluşturduğunuzda, alanı ilkenin adına ayarlayın. Bu ilke **DeviceConnect'e** izin vermelidir.

Aygıt işlevselliğine erişmek için paylaşılan erişim ilkelerini kullanmak için iki ana senaryo şunlardır:

* [bulut protokolü ağ geçitleri](iot-hub-devguide-endpoints.md),
* özel kimlik doğrulama düzenlerini uygulamak için kullanılan [belirteç hizmetleri.](iot-hub-devguide-security.md#custom-device-and-module-authentication)

Paylaşılan erişim ilkesi herhangi bir aygıt olarak bağlanmak için erişim izni verebilir, güvenlik belirteçleri oluştururken doğru kaynak URI kullanmak önemlidir. Bu ayar, uri kaynağını kullanarak belirteci belirli bir aygıta kapsaması gereken belirteç hizmetleri için özellikle önemlidir. Bu nokta, tüm aygıtlar için trafiğe zaten aracılık ettikleri için protokol ağ geçitleri için daha az alakalıdır.

Örnek olarak, **aygıt** adı verilen önceden oluşturulmuş paylaşılan erişim ilkesini kullanan bir belirteç hizmeti, aşağıdaki parametrelere sahip bir belirteç oluşturur:

* kaynak URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* imzalama anahtarı: `device` ilke nin anahtarlarından biri,
* politika adı: `device`,
* herhangi bir son kullanma süresi.

Önceki Düğüm.js işlevini kullanan bir örnek:

```javascript
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Aygıt1 için tüm işlevlere erişim sağlayan sonuç şu şekilde olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Bir protokol ağ geçidi, kaynak URI'yi yalnızca .'e `myhub.azure-devices.net/devices`ayarlayarak tüm aygıtlar için aynı belirteci kullanabilir.

### <a name="use-security-tokens-from-service-components"></a>Hizmet bileşenlerinden gelen güvenlik belirteçlerini kullanma

Hizmet bileşenleri, yalnızca daha önce açıklandığı gibi uygun izinleri veren paylaşılan erişim ilkelerini kullanarak güvenlik belirteçleri oluşturabilir.

Uç noktalarda açığa çıkan hizmet işlevleri şunlardır:

| Uç Nokta | İşlev |
| --- | --- |
| `{iot hub host name}/devices` |Aygıt kimliklerini oluşturun, güncelleyin, alın ve silin. |
| `{iot hub host name}/messages/events` |Aygıttan buluta iletiler alın. |
| `{iot hub host name}/servicebound/feedback` |Buluttan cihaza iletiler için geri bildirim alın. |
| `{iot hub host name}/devicebound` |Buluttan cihaza iletiler gönderin. |

Örnek olarak, **registryRead** adlı önceden oluşturulmuş paylaşılan erişim ilkesini kullanan bir hizmet aşağıdaki parametrelere sahip bir belirteç oluşturur:

* kaynak URI: `{IoT hub name}.azure-devices.net/devices`,
* imzalama anahtarı: `registryRead` ilke nin anahtarlarından biri,
* politika adı: `registryRead`,
* herhangi bir son kullanma süresi.

```javascript
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'registryRead';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Tüm cihaz kimliklerinin okunmasına erişim sağlayacak sonuç şu şekilde olacaktır:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Desteklenen X.509 sertifikaları

Azure IoT Hub'ına bir sertifika parmak izi veya sertifika yetkilisi (CA) yükleyerek IoT Hub'lı bir aygıtın kimliğini doğrulamak için herhangi bir X.509 sertifikasık kullanabilirsiniz. Sertifika parmak izlerini kullanarak kimlik doğrulama, sunulan parmak izinin yapılandırılan parmak iziyle eşleştiğini doğrular. Sertifika yetkilisini kullanarak kimlik doğrulaması sertifika zincirini doğrular. Her iki durumda da TLS el sıkışması aygıtın geçerli bir sertifikaya ve özel anahtara sahip olmasını gerektirir. Ayrıntılar için TLS belirtimine bakın: [RFC 5246 - Transport Layer Security (TLS) Protokol Sürüm 1.2](https://tools.ietf.org/html/rfc5246/).

Desteklenen sertifikalar şunlardır:

* **Varolan bir X.509 sertifikası.** Aygıtın zaten onunla ilişkili bir X.509 sertifikası olabilir. Aygıt, IoT Hub ile kimlik doğrulaması yapmak için bu sertifikayı kullanabilir. Parmak izi veya CA kimlik doğrulaması ile çalışır. 

* **CA imzalı X.509 sertifikası.** Bir aygıtı tanımlamak ve IoT Hub ile doğrulamak için, Sertifika Yetkilisi (CA) tarafından oluşturulan ve imzalanan x.509 sertifikasını kullanabilirsiniz. Parmak izi veya CA kimlik doğrulaması ile çalışır.

* **Kendi kendine oluşturulan ve kendi imzalanmış X-509 sertifikası.** Aygıt üreticisi veya şirket içi dağıtımcısı bu sertifikaları oluşturabilir ve ilgili özel anahtarı (ve sertifikayı) aygıtta depolayabilir. Bu amaçla [OpenSSL](https://www.openssl.org/) ve [Windows SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) yardımcı programı gibi araçları kullanabilirsiniz. Yalnızca parmak izi kimlik doğrulamasıyla çalışır. 

Aygıt, kimlik doğrulaması için X.509 sertifikası veya güvenlik belirteci kullanabilir, ancak her ikisini birden kullanmaz.

Sertifika yetkilisini kullanarak kimlik doğrulama hakkında daha fazla bilgi için [X.509 CA Sertifikalarını kullanarak Aygıt Kimlik Doğrulaması'na](iot-hub-x509ca-overview.md)bakın.

### <a name="register-an-x509-certificate-for-a-device"></a>Aygıt için X.509 sertifikası kaydetme

[C# için Azure IoT Service SDK](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/service) (sürüm 1.0.8+) kimlik doğrulaması için X.509 sertifikası kullanan bir aygıtı kaydetmeyi destekler. Aygıtların içe aktarıl/dışa aktarılmı gibi diğer API'ler de X.509 sertifikalarını destekler.

Ayrıca cli uzantı komutu [az iot hub aygıt kimliği](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) cihazlar için X.509 sertifikaları yapılandırmak için kullanabilirsiniz.

### <a name="c-support"></a>C\# Destek

**RegistryManager** sınıfı, bir aygıtı kaydetmek için programlı bir yol sağlar. Özellikle, **AddDeviceAsync** ve **UpdateDeviceAsync** yöntemleri, IoT Hub kimlik kayıt defterine bir aygıtı kaydetmenizi ve güncelleştirmenizi sağlar. Bu iki yöntem, bir **Aygıt** örneğini giriş olarak alır. **Aygıt** sınıfı, birincil ve ikincil X.509 sertifikası parmak izlerini belirtmenize olanak tanıyan bir **Kimlik Doğrulama** özelliği içerir. Parmak izi X.509 sertifikasının SHA256 karmasını temsil eder (ikili DER kodlaması kullanılarak depolanır). Birincil parmak izi veya ikincil parmak izi veya her ikisini birden belirtme seçeneğiniz var. Sertifika devrisenaryolarını işlemek için birincil ve ikincil parmak izleri desteklenir.

X.509\# sertifikası parmak izi kullanarak bir cihazı kaydetmek için örnek bir C kodu parçacığı aşağıda veda edebilirsiniz:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "B4172AB44C28F3B9E117648C6F7294978A00CDCBA34A46A1B8588B3F7D82C4F1"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Çalışma zamanı işlemleri sırasında X.509 sertifikası kullanma

[.NET için Azure IoT aygıtı SDK](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device) (sürüm 1.0.11+) X.509 sertifikalarının kullanımını destekler.

### <a name="c-support"></a>C\# Destek

Sınıf **DeviceAuthenticationWithX509Certificate,** X.509 sertifikası kullanarak **DeviceClient** örneklerinin oluşturulmasını destekler. X.509 sertifikası, özel anahtarı içeren PFX (PKCS #12 olarak da adlandırılır) biçiminde olmalıdır.

Burada bir örnek kod snippet:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-and-module-authentication"></a>Özel cihaz ve modül kimlik doğrulaması

IoT Hub kimlik [kayıt defterini,](iot-hub-devguide-identity-registry.md) [belirteçleri](iot-hub-devguide-security.md#security-tokens)kullanarak aygıt/modül güvenlik kimlik bilgilerini yapılandırmak ve denetime erişmek için kullanabilirsiniz. Bir IoT çözümzaten özel bir kimlik kayıt defteri ve / veya kimlik doğrulama düzeni varsa, IoT Hub ile bu altyapıyı tümleştirmek için bir *belirteç hizmeti* oluşturmayı düşünün. Bu şekilde, çözümünüzde diğer IoT özelliklerini kullanabilirsiniz.

Belirteç hizmeti özel bir bulut hizmetidir. *Aygıt kapsamı* veya modül *kapsamlı* belirteçler oluşturmak için DeviceConnect veya **ModuleConnect** izinleriyle ioT Hub paylaşılan erişim *ilkesini* kullanır. **ModuleConnect** Bu belirteçler, bir aygıtın ve modülün IoT hub'ınıza bağlanmasını sağlar.

![Belirteç hizmet deseninin adımları](./media/iot-hub-devguide-security/tokenservice.png)

Belirteç hizmet deseninin ana adımları şunlardır:

1. IoT hub'ınız için **DeviceConnect** veya **ModuleConnect** izinleri ile IoT Hub paylaşılan erişim ilkesi oluşturun. Bu ilkeyi [Azure portalında](https://portal.azure.com) veya programlı olarak oluşturabilirsiniz. Belirteç hizmeti oluşturduğu belirteçleri imzalamak için bu ilkeyi kullanır.

2. Bir aygıtın/modülün IoT hub'ınıza erişmesi gerektiğinde, belirteç hizmetinizden imzalı bir belirteç ister. Cihaz, belirteç hizmetinin belirteci oluşturmak için kullandığı aygıt/modül kimliğini belirlemek için özel kimlik kayıt defteri/kimlik doğrulama şemanLa kimlik doğrulaması yapabilir.

3. Belirteç hizmeti bir belirteç döndürür. Belirteç, aygıtın `/devices/{deviceId}` `/devices/{deviceId}/module/{moduleId}` kimlik `resourceURI`doğrulaması `deviceId` olarak veya modülün `moduleId` kimlik doğrulaması olarak kullanılarak veya olarak oluşturulur. Belirteç hizmeti, belirteci oluşturmak için paylaşılan erişim ilkesini kullanır.

4. Cihaz/modül belirteci doğrudan IoT hub'ı ile kullanır.

> [!NOTE]
> Belirteç hizmetinizde bir belirteç oluşturmak için .NET sınıfı [SharedAccessSignatureBuilder](https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx) veya Java sınıfı [IotHubServiceSasToken'ı](/java/api/com.microsoft.azure.sdk.iot.service.auth.iothubservicesastoken) kullanabilirsiniz.

Belirteç hizmeti belirteç sona ermesini istediğiniz gibi ayarlayabilir. Belirteç süresi dolduğunda, IoT hub'ı aygıt/modül bağlantısını keser. Ardından, aygıt/modül belirteç hizmetinden yeni bir belirteç talep etmelidir. Kısa bir son kullanma süresi hem cihaz/modül hem de belirteç hizmeti üzerindeki yükü artırır.

Bir aygıtın/modülün hub'ınıza bağlanması için, bağlanmak için anahtar değil, bir belirteç kullanıyor olsa bile, onu Yine de IoT Hub kimlik kayıt defterine eklemeniz gerekir. Bu nedenle, kimlik kaydında aygıt/modül kimliklerini etkinleştirerek veya devre dışı bırakarak cihaz başına/modül başına erişim denetimini kullanmaya devam [edebilirsiniz.](iot-hub-devguide-identity-registry.md) Bu yaklaşım, uzun son kullanma süreleri ile belirteçleri kullanmanın risklerini azaltır.

### <a name="comparison-with-a-custom-gateway"></a>Özel bir ağ geçidiyle karşılaştırma

Belirteç hizmet deseni, IoT Hub ile özel kimlik kayıt defteri/kimlik doğrulama düzeni uygulamanın önerilen yoludur. IoT Hub çözüm trafiğinin çoğunu işlemeye devam ettiği için bu desen önerilir. Ancak, özel kimlik doğrulama düzeni protokolle bu kadar iç içeyse, tüm trafiği işlemek için özel bir *ağ geçidi* gerekebilir. Böyle bir senaryoya örnek olarak [Aktarım Katmanı Güvenliği (TLS) ve önceden paylaşılan anahtarlar (PSKs)](https://tools.ietf.org/html/rfc4279)kullanılmasıdır. Daha fazla bilgi için [protokol ağ geçidi](iot-hub-protocol-gateway.md) makalesine bakın.

## <a name="reference-topics"></a>Referans konuları:

Aşağıdaki başvuru konuları, IoT hub'ınıza erişimi denetleme hakkında daha fazla bilgi sağlar.

## <a name="iot-hub-permissions"></a>IoT Hub izinleri

Aşağıdaki tabloda, IoT hub'ınıza erişimi denetlemek için kullanabileceğiniz izinler listelenebiyidir.

| İzin | Notlar |
| --- | --- |
| **Kayıt DefteriRead** |Verilen ler kimlik kaydına erişim okunur. Daha fazla bilgi için [kimlik kaydına](iot-hub-devguide-identity-registry.md)bakın. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **Kayıt DefteriReadWrite** |Kimlik kaydına erişim izni verir. Daha fazla bilgi için [kimlik kaydına](iot-hub-devguide-identity-registry.md)bakın. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **ServiceConnect** |Bulut hizmetine bakan iletişim ve izleme uç noktalarına erişim sağlar. <br/>Aygıttan buluta iletiler alma, buluttan aygıta ileti gönderme ve ilgili teslim bildirimlerini alma izni verir. <br/>Dosya yüklemeleri için teslimat bildirimlerini alma izni verir. <br/>Etiketleri ve istenen özellikleri güncelleştirmek, bildirilen özellikleri almak ve sorguları çalıştırmak için ikizlere erişim izni verir. <br/>Bu izin arka uç bulut hizmetleri tarafından kullanılır. |
| **DeviceConnect** |Aygıta bakan uç noktalara erişim sağlar. <br/>Aygıttan buluta ileti gönderme ve buluttan aygıta ileti alma izni verir. <br/>Bir aygıttan dosya yükleme yapma izni verir. <br/>Cihaz alabilme izni ni alır, istenen özellik bildirimlerini ve aygıt ikiz bildirilen özelliklerini günceller. <br/>Dosya yüklemeleri gerçekleştirme izni verir. <br/>Bu izin aygıtlar tarafından kullanılır. |

## <a name="additional-reference-material"></a>Ek referans materyali

IoT Hub geliştirici kılavuzundaki diğer başvuru konuları şunlardır:

* [IoT Hub uç noktaları,](iot-hub-devguide-endpoints.md) her IoT hub'ın çalışma zamanı ve yönetim işlemleri için ortaya çıkardığı çeşitli uç noktaları açıklar.

* [Azaltma ve kotalar,](iot-hub-devguide-quotas-throttling.md) IoT Hub hizmetiiçin geçerli olan kotaları ve azaltma davranışlarını açıklar.

* [Azure IoT aygıt ve hizmet SDK'ları,](iot-hub-devguide-sdks.md) IoT Hub ile etkileşimde bulunan hem aygıt hem de hizmet uygulamaları geliştirdiğinizde kullanabileceğiniz çeşitli dil SDK'larını listeler.

* [IoT Hub sorgu dili, IoT](iot-hub-devguide-query-language.md) Hub'dan aygıtınız ve işleriniz hakkında bilgi almak için kullanabileceğiniz sorgu dilini açıklar.

* [IoT Hub MQTT desteği,](iot-hub-mqtt-support.md) MQTT protokolü için IoT Hub desteği hakkında daha fazla bilgi sağlar.

* [RFC 5246 - Transport Layer Security (TLS) Protokol Üte 1.2](https://tools.ietf.org/html/rfc5246/) TLS kimlik doğrulaması hakkında daha fazla bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Hub'a erişimi nasıl denetlediğinizi öğrendiğiniz için, aşağıdaki IoT Hub geliştirici kılavuzu konuları yla ilgilenebilirsiniz:

* [Durumu ve yapılandırmaları eşitlemek için aygıt ikizlerini kullanma](iot-hub-devguide-device-twins.md)
* [Aygıtta doğrudan bir yöntem çağırma](iot-hub-devguide-direct-methods.md)
* [Birden fazla cihazda işleri zamanlama](iot-hub-devguide-jobs.md)

Bu makalede açıklanan bazı kavramları denemek istiyorsanız, aşağıdaki IoT Hub öğreticilerine bakın:

* [Azure IoT Hub ile başlayın](quickstart-send-telemetry-node.md)
* [IoT Hub ile buluttan cihaza ileti gönderme](iot-hub-csharp-csharp-c2d.md)
* [IoT Hub aygıttan buluta iletileri işleme](tutorial-routing.md)
