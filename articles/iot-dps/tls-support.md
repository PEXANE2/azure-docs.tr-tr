---
title: Azure IoT cihaz sağlama hizmeti (DPS) TLS desteği
description: IoT cihaz sağlama hizmeti (DPS) ile iletişim kuran cihazlar ve hizmetler için güvenli TLS bağlantıları kullanmanın en iyi yöntemleri
services: iot-dps
author: wesmc7777
ms.service: iot-dps
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: wesmc
ms.openlocfilehash: 285832d80d37c8553ffc8e37c6f6eab5d7f6d943
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82984856"
---
# <a name="tls-support-in-azure-iot-hub-device-provisioning-service-dps"></a>Azure IoT Hub cihaz sağlama hizmeti 'nde (DPS) TLS desteği

DPS, IoT cihazlarındaki bağlantıları güvenli hale getirmek için Aktarım Katmanı Güvenliği 'ni (TLS) kullanır. TLS protokolünün üç sürümü şu anda desteklenmektedir. 1,0, 1,1 ve 1,2 sürümleri.

TLS 1,0 ve 1,1 eski olarak değerlendirilir ve kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [IoT Hub IÇIN TLS 1,0 ve 1,1 kullanımdan](../iot-hub/iot-hub-tls-deprecating-1-0-and-1-1.md)kaldırılıyor. DPS 'e bağlanırken tercih edilen TLS sürümü olarak TLS 1,2 kullanmanız önemle önerilir.

## <a name="restrict-connections-to-tls-12"></a>TLS 1,2 ile bağlantıları kısıtlama

Ek güvenlik için, DPS örneklerinizi *yalnızca* TLS sürüm 1,2 kullanan cihaz istemci bağlantılarına izin verecek şekilde yapılandırmanız ve [Önerilen şifrelemelerin](#recommended-ciphers)kullanımını zorunlu hale getirilmesi önerilir.

Bunu yapmak için, [desteklenen bölgelerin](#supported-regions) herhangi birinde yenı bir DPS kaynağı sağlayın ve `minTlsVersion` özelliği Azure Resource Manager şablonunuzun DPS kaynak belirtiminde `1.2` olarak ayarlayın. Aşağıdaki örnek şablon JSON, yeni bir `minTlsVersion` DPS örneği için özelliğini belirtir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/ProvisioningServices",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-DPS-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "S1",
                "capacity": 1
            },
        }     
    ]
}
```

Aşağıdaki Azure CLı komutunu kullanarak şablonu dağıtabilirsiniz. 

```azurecli
az deployment group create -g <your resource group name> --template-file template.json
```

Kaynak Yöneticisi şablonlarıyla DPS kaynakları oluşturma hakkında daha fazla bilgi için, bkz. [bir Azure Resource Manager ŞABLONUYLA DPS ayarlama](quick-setup-auto-provision-rm.md).

Bu yapılandırma kullanılarak oluşturulan DPS kaynağı, 1,0 ve 1,1 TLS sürümlerini kullanarak bağlanmayı deneyen cihazları reddeder. Benzer şekilde, cihaz istemcisinin HELLO iletisi [Önerilen şifrelemelerin](#recommended-ciphers)hiçbirini LISTELEMEZ ise TLS el sıkışması reddedilir.

> [!NOTE]
> `minTlsVersion` Özelliği salt OKUNURDUR ve DPS kaynağınız oluşturulduktan sonra değiştirilemez. Bu nedenle, *Tüm* IoT CIHAZLARıNıZıN TLS 1,2 ile uyumlu olduğunu ve [Önerilen şifrelemeleri](#recommended-ciphers) önceden test etmeniz ve doğrulamanız önemlidir.

## <a name="supported-regions"></a>Desteklenen bölgeler

TLS 1,2 kullanımını gerektiren IoT DPS örnekleri aşağıdaki bölgelerde oluşturulabilir:

* US Gov Arizona
* US Gov Virginia

> [!NOTE]
> Yük devretme sonrasında, DPS `minTlsVersion` özelliğinin özelliği, coğrafi olarak eşleştirilmiş bölgenin yük devretmesinde etkili olmaya devam edecektir.

## <a name="recommended-ciphers"></a>Önerilen şifrelemeler

Yalnızca TLS 1,2 kabul edecek şekilde yapılandırılan DPS örnekleri, aşağıdaki önerilen şifrelemelerin kullanımını da zorunlu tutar:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

## <a name="use-tls-12-in-the-iot-sdks"></a>IoT SDK 'lerinde TLS 1,2 kullanma

Azure IoT istemci SDK 'lerinde TLS 1,2 ve izin verilen şifrelemeleri yapılandırmak için aşağıdaki bağlantıları kullanın.

| Dil | TLS 1,2 destekleyen sürümler | Belgeler |
|----------|------------------------------------|---------------|
| C        | Etiket 2019-12-11 veya üzeri            | [Bağlantı](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Sürüm 2.0.0 veya daha yeni             | [Bağlantı](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Sürüm 1.21.4 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Sürüm 1.19.0 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Sürüm 1.12.2 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-with-iot-edge"></a>IoT Edge TLS 1,2 kullanma

IoT Edge cihazlar, IoT Hub ve DPS ile iletişim kurarken TLS 1,2 kullanacak şekilde yapılandırılabilir. Bu amaçla [IoT Edge belge sayfasını](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md)kullanın.