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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78893223"
---
Bu bölümde, bu makale için bir aygıt kimliği oluşturmak için Azure CLI'yi kullanırsınız. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

1. [Azure Cloud Shell](https://shell.azure.com/)'i açın.

1. Azure Bulut Kabuğu'nda, Azure CLI için Microsoft Azure IoT Uzantısını yüklemek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

2. Adı verilen `myDeviceId` yeni bir aygıt kimliği oluşturun ve aygıt bağlantı dizesini bu komutlarla alın:

    ```azurecli-interactive
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Sonuçtan aygıt bağlantı dizesini not alın. Bu aygıt bağlantı dizesi, aygıt uygulaması tarafından IoT Hub'ınıza aygıt olarak bağlanmak için kullanılır.

<!-- images and links -->
