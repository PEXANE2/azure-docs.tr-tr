---
title: Yerleşik edgeAgent doğrudan yöntemleri-Azure IoT Edge
description: IoT Edge Aracısı çalışma zamanı modülündeki yerleşik doğrudan yöntemleri kullanarak IoT Edge dağıtımını izleme ve yönetme
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/02/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 57b9d46918414cef9e8cbcffb941b98c98f985ff
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80240354"
---
# <a name="communicate-with-edgeagent-using-built-in-direct-methods"></a>Yerleşik doğrudan yöntemler kullanarak edgeAgent ile iletişim kurma

IoT Edge aracı modülüne dahil olan doğrudan yöntemleri kullanarak IoT Edge dağıtımlarını izleyin ve yönetin. Doğrudan yöntemler cihaza uygulanır ve sonra buluttan çağrılabilir. IoT Edge Aracısı, IoT Edge cihazlarınızı uzaktan izlemenize ve yönetmenize yardımcı olan doğrudan yöntemler içerir.

Doğrudan yöntemler hakkında daha fazla bilgi için, bunları kullanma ve kendi modüllerinize uygulama hakkında daha fazla bilgi için, bkz. [IoT Hub doğrudan yöntemleri anlama ve çağırma](../iot-hub/iot-hub-devguide-direct-methods.md).

Bu doğrudan yöntemlerin adları, büyük/küçük harfe duyarsız işlenir.

## <a name="ping"></a>Ping

**Ping** yöntemi, bir cihazda IoT Edge çalışıp çalışmadığını veya cihazın IoT Hub açık bağlantısı olup olmadığını denetlemek için faydalıdır. IoT Edge aracısına ping getirmek ve durumunu almak için bu doğrudan yöntemi kullanın. Başarılı bir ping boş bir yük ve **"durum" döndürür: 200**.

Örneğin:

```azurecli
az iot hub invoke-module-method --method-name 'ping' -n <hub name> -d <device name> -m '$edgeAgent'
```

Azure portal Yöntem adı `ping` ve boş bir JSON yükünün `{}`bulunduğu yöntemi çağırın.

![Azure portal ' ping ' doğrudan metodunu çağır](./media/how-to-edgeagent-direct-method/ping-direct-method.png)

## <a name="restart-module"></a>Modülü yeniden Başlat

**RestartModule** yöntemi, IoT Edge cihazda çalışan modüllerin uzaktan yönetilmesine olanak tanır. Bir modül hatalı durumu veya sağlıksız bir davranışı raporladığında, IoT Edge aracısını yeniden başlatmak için tetikleyebilirsiniz. Başarılı yeniden başlatma komutu boş bir yük ve **"durum" döndürür: 200**.

RestartModule yöntemi IoT Edge sürüm 1.0.9 ve üzeri sürümlerde kullanılabilir. 

RestartModule Direct yöntemini, edgeAgent modülünün kendisi de dahil olmak üzere IoT Edge cihazında çalışan herhangi bir modülde kullanabilirsiniz. Ancak, edgeAgent 'ı kapatmak için bu doğrudan yöntemini kullanırsanız, modül yeniden başlatılırken bağlantının kesintiye uğramasından dolayı başarılı bir sonuç almazsınız.

Örneğin:

```azurecli
az iot hub invoke-module-method --method-name 'RestartModule' -n <hub name> -d <device name> -m '$edgeAgent' --method-payload \
'
    {
        "schemaVersion": "1.0",
        "id": "<module name>"
    }
'
```

Azure portal Yöntem adı `RestartModule` ve aşağıdaki JSON yüküyle yöntemi çağırın:

```json
{
    "schemaVersion": "1.0",
    "id": "<module name>"
}
```

![Azure portal ' RestartModule ' doğrudan metodunu çağır](./media/how-to-edgeagent-direct-method/restartmodule-direct-method.png)

## <a name="experimental-methods"></a>Deneysel Yöntemler

Yeni doğrudan Yöntem seçenekleri, test edilecek Deneysel özellikler olarak kullanılabilir:

* [Uploadlogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md): modül günlüklerini alın ve Azure Blob depolamaya yükleyin.
* [Gettaskstatus](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#gettaskstatus): karşıya yükleme günlüğü isteğinin durumunu denetleyin.
* [Getlogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#getlogs): doğrudan yöntemin yanıtında modül günlüklerini satır içinde alın.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge Aracısı ve IoT Edge hub modülünün özellikleri TWINS](module-edgeagent-edgehub.md)
