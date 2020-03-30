---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 10/24/2019
ms.openlocfilehash: a3340eb9e53afa83c35109bad7d22f81413dd644
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80234139"
---
## <a name="prepare-an-iot-hub"></a>Bir IoT hub'ı hazırlama

Ayrıca, bu hızlı başlangıcı tamamlamak için Azure aboneliğinizde bir Azure IoT hub'ına ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. IoT hub'ıniz yoksa, [oluşturmak için bu yönergeleri](../articles/iot-hub/iot-hub-create-using-cli.md)izleyin.

Azure CLI'yi yerel olarak kullanıyorsanız, önce Azure aboneliğinizde oturum `az login`açın. Bu komutları Azure Bulut Kabuğu'nda çalıştırıyorsanız, otomatik olarak oturum açmış olursunuz.

Azure CLI'yi yerel olarak kullanıyorsanız, `az` sürüm **2.0.73** veya daha sonra olmalıdır; Azure Bulut Kabuğu en son sürümü kullanır. Makinenize `az --version` yüklenen sürümü denetlemek için komutu kullanın.

Azure CLI için Microsoft Azure IoT Uzantısı'nı örneğinize eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az extension add --name azure-iot
```

IoT hub'ınızda aygıt kimliğini oluşturmak için aşağıdaki komutu çalıştırın. **YourIoTHubName** ve **YourDeviceID** yer tutucularını kendi _IoT Hub adınız_ ve seçtiğiniz bir _cihaz kimliğiyle_ değiştirin.

```azurecli-interactive
az iot hub device-identity create --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Yeni kaydettiğiniz aygıt için _aygıt bağlantı dizesini_ almak için aşağıdaki komutu çalıştırın (daha sonra kullanım için not:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```
