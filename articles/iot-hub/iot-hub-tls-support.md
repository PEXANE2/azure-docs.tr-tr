---
title: Azure IoT Hub TLS desteği
description: IoT Hub ile iletişim kuran cihazlar ve hizmetler için güvenli TLS bağlantıları kullanmanın en iyi yöntemleri
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: jlian
ms.openlocfilehash: 08ecb766a1a9bd7ff75bf97647be811577212eb5
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90006049"
---
# <a name="tls-support-in-iot-hub"></a>IoT Hub 'de TLS desteği

IoT Hub IoT cihazlarından ve hizmetlerinden gelen bağlantıları güvenli hale getirmek için Aktarım Katmanı Güvenliği 'ni (TLS) kullanır. TLS protokolünün üç sürümü şu anda desteklenmektedir. 1,0, 1,1 ve 1,2 sürümleri.

TLS 1,0 ve 1,1 eski olarak değerlendirilir ve kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [IoT Hub IÇIN TLS 1,0 ve 1,1 kullanımdan](iot-hub-tls-deprecating-1-0-and-1-1.md)kaldırılıyor. IoT Hub bağlanırken tercih edilen TLS sürümü olarak TLS 1,2 kullanmanız önemle önerilir.

## <a name="tls-12-enforcement-available-in-select-regions"></a>TLS 1,2 zorlaması seçim bölgelerinde kullanılabilir

Ek güvenlik için, IoT Hub 'larınızı *yalnızca* TLS sürüm 1,2 kullanan istemci bağlantılarına izin verecek ve [şifre paketlerinin](#cipher-suites)kullanımını zorunlu kılacak şekilde yapılandırın. Bu özellik yalnızca şu bölgelerde desteklenir:

* Doğu ABD
* Orta Güney ABD
* Batı ABD 2
* US Gov Arizona
* US Gov Virginia

Bu amaçla, desteklenen bölgelerin herhangi birinde yeni bir IoT Hub sağlayın ve `minTlsVersion` özelliğini `1.2` Azure Resource Manager şablonunuzun IoT Hub kaynak belirtiminde olarak ayarlayın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/IotHubs",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
            }
        }
    ]
}
```

Bu yapılandırmayı kullanan oluşturulan IoT Hub kaynak, 1,0 ve 1,1 TLS sürümlerini kullanarak bağlanmayı deneyen cihaz ve hizmet istemcilerini reddeder. Benzer şekilde, `ClientHello` ileti [Önerilen şifrelemelerin](#cipher-suites)hiçbirini LISTELEMEZ ise TLS el sıkışması reddedilir.

> [!NOTE]
> `minTlsVersion`Özelliği salt okunurdur ve IoT Hub kaynağınız oluşturulduktan sonra değiştirilemez. Bu nedenle, *Tüm* IoT cihazlarınızın ve hizmetlerinizin TLS 1,2 ile uyumlu olduğunu ve [Önerilen şifrelemeleri](#cipher-suites) önceden test etmek ve doğrulamak önemlidir.
> 
> `minTlsVersion`Yük devretme sonrasında, IoT Hub özelliği, coğrafi olarak eşleştirilmiş bölgenin yük devretmesinde etkili olmaya devam edecektir.

## <a name="cipher-suites"></a>Şifre paketleri

Yalnızca TLS 1,2 kabul edecek şekilde yapılandırılmış IoT Hub 'Ları, aşağıdaki önerilen şifre paketlerinin kullanımını da zorunlu tutar:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

TLS 1,2 zorlaması için yapılandırılmayan IoT Hub 'Ları için, TLS 1,2, aşağıdaki şifre paketleriyle birlikte hala geçerlidir:

* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_CBC_SHA256`
* `TLS_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

İstemci, sırasında kullanmak üzere daha yüksek şifre paketlerinin bir listesini önerebilir `ClientHello` . Ancak, bunlardan bazıları IoT Hub (örneğin,) tarafından desteklenmiyor olabilir `ECDHE-ECDSA-AES256-GCM-SHA384` . Bu durumda IoT Hub, istemcinin tercihini izlemeye çalışacaktır, ancak sonunda şifre paketini görüşmelidir `ServerHello` .

## <a name="use-tls-12-in-your-iot-hub-sdks"></a>IoT Hub SDK 'larınız için TLS 1,2 kullanın

IoT Hub istemci SDK 'lerinde TLS 1,2 ve izin verilen şifrelemeleri yapılandırmak için aşağıdaki bağlantıları kullanın.

| Dil | TLS 1,2 destekleyen sürümler | Belgeler |
|----------|------------------------------------|---------------|
| C        | Etiket 2019-12-11 veya üzeri            | [Bağlantısının](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Sürüm 2.0.0 veya daha yeni             | [Bağlantısının](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Sürüm 1.21.4 veya daha yeni            | [Bağlantısının](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Sürüm 1.19.0 veya daha yeni            | [Bağlantısının](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Sürüm 1.12.2 veya daha yeni            | [Bağlantısının](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-in-your-iot-edge-setup"></a>IoT Edge kurulumunda TLS 1,2 kullanma

IoT Edge cihazlar, IoT Hub iletişim kurarken TLS 1,2 kullanacak şekilde yapılandırılabilir. Bu amaçla [IoT Edge belge sayfasını](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md)kullanın.

## <a name="device-authentication"></a>Cihaz kimlik doğrulaması

Başarılı bir TLS el sıkışması sonrasında, IoT Hub simetrik anahtar veya X. 509.440 sertifikası kullanarak bir cihazın kimliğini doğrulayabilirler. Sertifika tabanlı kimlik doğrulaması için, bu, ECC dahil olmak üzere herhangi bir X. 509.952 sertifikası olabilir. IoT Hub sertifikayı, sağladığınız parmak izine veya sertifika yetkilisine (CA) göre doğrular. IoT Hub, henüz X. 509.440 tabanlı karşılıklı kimlik doğrulamasını desteklemez (mTLS). Daha fazla bilgi için bkz. [desteklenen X. 509.440 sertifikaları](iot-hub-devguide-security.md#supported-x509-certificates).
