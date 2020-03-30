---
title: Aygıt ve Azure Aygıt Sağlama Hizmeti arasında yük aktarma
description: Bu belge, aygıt ve Aygıt Sağlama Hizmeti (DPS) arasında bir yükün nasıl aktarılabildiğini açıklar
author: menchi
ms.author: menchi
ms.date: 02/11/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: de6bb3fe0879a69467283e93a04a355876a02cba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246696"
---
# <a name="how-to-transfer-a-payload-between-device-and-dps"></a>Aygıt ve DPS arasında yük aktarma
Bazen DPS'nin aygıtlardan doğru IoT Hub'ına düzgün bir şekilde sağlanması için daha fazla veriye ihtiyacı vardır ve bu verilerin aygıt tarafından sağlanması gerekir. Tam tersi, DPS istemci yan mantık kolaylaştırmak için aygıta veri döndürebilir. 

## <a name="when-to-use-it"></a>Kullanılması gereken durumlar
Bu özellik [özel ayırma](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies)için bir geliştirme olarak kullanılabilir. Örneğin, cihazlarınızı insan müdahalesi olmadan aygıt modeline göre ayırmak istiyorsunuz. Bu durumda, özel [ayırma](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies)kullanırsınız. Aygıtı, [kayıt aygıtı çağrısının](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice)bir parçası olarak model bilgilerini bildirecek şekilde yapılandırabilirsiniz. DPS, aygıtın yükünü özel ayırma webhook'una aktaracaktır. Ayrıca işleviniz, bu aygıtın cihaz modeli bilgilerini aldığında hangi IoT Hub'ına gideceğine karar verebilir. Benzer şekilde, webhook aygıta bazı verileri döndürmek isterse, verileri webhook yanıtında bir dize olarak geri aktaracaktır.  

## <a name="device-sends-data-payload-to-dps"></a>Aygıt DPS'ye veri yükü gönderir
Cihazınız DPS'ye bir [kayıt cihazı çağrısı](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) gönderirken, kayıt çağrısı vücuttaki diğer alanları almak için geliştirilebilir. Vücut aşağıdaki gibi görünüyor: 
   ```
   { 
       “registrationId”: “mydevice”, 
       “tpm”:                
       { 
           “endorsementKey”: “stuff”, 
           “storageRootKey”: “things” 
       }, 
       “payload”: “your additional data goes here. It can be nested JSON.” 
    } 
   ```

## <a name="dps-returns-data-to-the-device"></a>DPS verileri aygıta döndürür
Özel ayırma ilkesi webhook aygıta bazı verileri döndürmek isterse, verileri webhook yanıtında bir dize olarak geri aktaracaktır. Değişiklik aşağıdaki yük bölümündedir. 
   ```
   { 
       "iotHubHostName": "sample-iot-hub-1.azure-devices.net", 
       "initialTwin": { 
           "tags": { 
               "tag1": true 
               }, 
               "properties": { 
                   "desired": { 
                       "tag2": true 
                    } 
                } 
            }, 
        "payload": "whatever is returned by the webhook" 
    } 
   ```

## <a name="sdk-support"></a>SDK desteği
Bu özellik C, C#, JAVA ve Node.js [istemci SDK'larında](https://docs.microsoft.com/azure/iot-dps/)kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* Azure IoT Hub ve Azure IoT Hub Aygıt Sağlama Hizmeti için [Azure IoT SDK'yı]( https://github.com/Azure/azure-iot-sdks) kullanarak geliştirme
