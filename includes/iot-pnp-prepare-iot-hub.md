---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 03/17/2020
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 3ac72360db5fb3d075b4caa3208f372f22441c7b
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92756003"
---
## <a name="prepare-an-iot-hub"></a>IoT Hub 'ı hazırlama

Bu makaledeki adımları tamamlayabilmeniz için Azure aboneliğinizde bir Azure IoT Hub 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Azure CLı 'yi yerel olarak kullanıyorsanız, önce kullanarak Azure aboneliğinizde oturum açın `az login` . Bu komutları Azure Cloud Shell çalıştırıyorsanız, otomatik olarak oturum açtınız.

Azure CLı 'yi yerel olarak kullanıyorsanız, `az` Sürüm **2.8.0** veya üzeri olmalıdır; Azure Cloud Shell en son sürümü kullanır. `az --version`Makinenizde yüklü sürümü denetlemek için komutunu kullanın.

Azure CLı için Microsoft Azure IoT uzantısını örneğinize eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az extension add --name azure-iot
```

Kullanabileceğiniz bir IoT Hub 'ınız yoksa, aboneliğinizdeki bir kaynak grubu ve ücretsiz bir IoT Hub 'ı oluşturmak için aşağıdaki komutları çalıştırın. `<YourIoTHubName>`İstediğiniz bir hub adıyla değiştirin:

```azurecli-interactive
az group create --name my-pnp-resourcegroup \
    --location centralus
az iot hub create --name <YourIoTHubName> \
    --resource-group my-pnp-resourcegroup --sku F1
```

> [!NOTE]
> IoT Tak ve Kullan şu anda Orta ABD, Kuzey Avrupa ve Doğu Japonya bölgelerinde oluşturulan IoT Hub 'larda sunulmaktadır. IoT Tak ve Kullan desteği temel katmanlı IoT Hub 'larına dahil değildir.

IoT Hub 'ınızda cihaz kimliğini oluşturmak için aşağıdaki komutu çalıştırın. `<YourIoTHubName>`Ve `<YourDeviceID>` yer tutucuları kendi _IoT Hub adı_ ve seçtiğiniz bir _cihaz kimliği_ ile değiştirin.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```
