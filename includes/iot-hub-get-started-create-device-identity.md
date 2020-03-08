---
title: include dosyası
description: include dosyası
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 68260bf8aafbbe5afd46ec7dfb763eb88ee2123e
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78893223"
---
Bu bölümde, bu makale için bir cihaz kimliği oluşturmak üzere Azure CLı 'yi kullanırsınız. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

1. [Azure Cloud Shell](https://shell.azure.com/)'i açın.

1. Azure Cloud Shell ' de, Azure CLı için Microsoft Azure IoT uzantısını yüklemek üzere aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

2. `myDeviceId` adlı yeni bir cihaz kimliği oluşturun ve şu komutlarla cihaz bağlantı dizesini alın:

    ```azurecli-interactive
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Sonuçtan cihaz bağlantı dizesini bir yere göz önünde koyun. Bu cihaz bağlantı dizesi cihaz uygulaması tarafından IoT Hub bir cihaz olarak bağlanmak üzere kullanılır.

<!-- images and links -->
