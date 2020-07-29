---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 03/17/2020
ms.custom: references_regions
ms.openlocfilehash: adc621f932462422202d9f16fd539f5ecc7c3d8e
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87336914"
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
