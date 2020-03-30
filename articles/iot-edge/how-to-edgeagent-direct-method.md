---
title: Yerleşik edgeAgent doğrudan yöntemler - Azure IoT Edge
description: IoT Edge aracısı çalışma zamanı modülünde yerleşik doğrudan yöntemleri kullanarak bir IoT Edge dağıtımını izleyin ve yönetin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/02/2020
ms.topic: conceptual
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 57b9d46918414cef9e8cbcffb941b98c98f985ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240354"
---
# <a name="communicate-with-edgeagent-using-built-in-direct-methods"></a>Yerleşik doğrudan yöntemleri kullanarak edgeAgent ile iletişim kurun

IoT Edge aracı modülünde yer alan doğrudan yöntemleri kullanarak IoT Edge dağıtımlarını izleyin ve yönetin. Aygıtta doğrudan yöntemler uygulanır ve buluttan çağrılabilir. IoT Edge aracısı, IoT Edge aygıtlarınızı uzaktan izlemenize ve yönetmenize yardımcı olan doğrudan yöntemler içerir.

Doğrudan yöntemler, bunları nasıl kullanacağınız ve bunları kendi modüllerinizde nasıl uygulayacağınız hakkında daha fazla bilgi için [Bkz.](../iot-hub/iot-hub-devguide-direct-methods.md)

Bu doğrudan yöntemlerin adları büyük/küçük harf duyarsız ele alınır.

## <a name="ping"></a>Ping

**Ping** yöntemi, IoT Edge'in bir aygıtta çalışıp çalışmadığını veya aygıtın IoT Hub'a açık bağlantısı olup olmadığını denetlemek için yararlıdır. IoT Edge aracısını pinglemek ve durumunu almak için bu doğrudan yöntemi kullanın. Başarılı bir ping boş bir yük ve **"durum" döndürür: 200**.

Örnek:

```azurecli
az iot hub invoke-module-method --method-name 'ping' -n <hub name> -d <device name> -m '$edgeAgent'
```

Azure portalında, yöntem adı `ping` ve boş bir JSON `{}`yüküyle yöntemi çağırın.

![Azure portalında doğrudan 'ping' yöntemini çağırma](./media/how-to-edgeagent-direct-method/ping-direct-method.png)

## <a name="restart-module"></a>Modülü yeniden başlatın

**Yeniden Başlatma Modülü** yöntemi, bir IoT Edge aygıtında çalışan modüllerin uzaktan yönetilmesini sağlar. Bir modül başarısız bir durum veya diğer sağlıksız davranışı bildiriyorsa, IoT Edge aracısını yeniden başlatmak için tetikleyebilirsiniz. Başarılı bir yeniden başlatma komutu boş bir yük ve **"durum" döndürür: 200**.

Yeniden Başlatma Modülü yöntemi IoT Edge sürüm 1.0.9 ve sonraki sürümlerde mevcuttur. 

EdgeAgent modülü nün kendisi de dahil olmak üzere, IoT Edge aygıtında çalışan herhangi bir modülde Yeniden Başlatma Modülü doğrudan yöntemini kullanabilirsiniz. Ancak, edgeAgent'ı kapatmak için bu doğrudan yöntemi kullanırsanız, modül yeniden başlatılırken bağlantı kesintiye geldiğinden başarı sonucu almazsınız.

Örnek:

```azurecli
az iot hub invoke-module-method --method-name 'RestartModule' -n <hub name> -d <device name> -m '$edgeAgent' --method-payload \
'
    {
        "schemaVersion": "1.0",
        "id": "<module name>"
    }
'
```

Azure portalında, yöntem adı `RestartModule` ve aşağıdaki JSON yüküyle yöntemi çağırın:

```json
{
    "schemaVersion": "1.0",
    "id": "<module name>"
}
```

![Azure portalında doğrudan 'Yeniden Başlatma Modülü' yöntemini çağırın](./media/how-to-edgeagent-direct-method/restartmodule-direct-method.png)

## <a name="experimental-methods"></a>Deneysel yöntemler

Yeni doğrudan yöntem seçenekleri, test etmek için deneysel özellikler olarak kullanılabilir:

* [UploadLogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md): Modül günlüklerini alın ve Azure Blob Depolama'ya yükleyin.
* [GetTaskStatus](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#gettaskstatus): Yükleme günlükleri isteğinin durumunu kontrol edin.
* [GetLogs](https://github.com/Azure/iotedge/blob/master/doc/built-in-logs-pull.md#getlogs): Doğrudan yöntemin yanıtında modül günlüklerini satır satıralın.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge aracısının ve IoT Edge hub modülü ikizlerinin özellikleri](module-edgeagent-edgehub.md)
