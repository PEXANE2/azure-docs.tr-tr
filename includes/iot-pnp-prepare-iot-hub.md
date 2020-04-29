---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: a3340eb9e53afa83c35109bad7d22f81413dd644
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80234139"
---
## <a name="prepare-an-iot-hub"></a>IoT Hub 'ı hazırlama

Ayrıca, bu hızlı başlangıcı tamamlayabilmeniz için Azure aboneliğinizde bir Azure IoT Hub 'ınız olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. Bir IoT Hub 'ınız yoksa, [oluşturmak için bu yönergeleri](../articles/iot-hub/iot-hub-create-using-cli.md)izleyin.

Azure CLı 'yi yerel olarak kullanıyorsanız, önce kullanarak `az login`Azure aboneliğinizde oturum açın. Bu komutları Azure Cloud Shell çalıştırıyorsanız, otomatik olarak oturum açtınız.

Azure CLı 'yi yerel olarak kullanıyorsanız, `az` sürüm **2.0.73** veya üzeri olmalıdır; Azure Cloud Shell en son sürümü kullanır. Makinenizde yüklü `az --version` sürümü denetlemek için komutunu kullanın.

Azure CLı için Microsoft Azure IoT uzantısını örneğinize eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az extension add --name azure-iot
```

IoT Hub 'ınızda cihaz kimliğini oluşturmak için aşağıdaki komutu çalıştırın. **Youriothubname** ve **yourdeviceıd** yertutucuları kendi _IoT Hub adı_ ve seçtiğiniz bir _cihaz kimliği_ ile değiştirin.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Yeni kaydettiğiniz cihazın _Cihaz bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanmak üzere):

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```
