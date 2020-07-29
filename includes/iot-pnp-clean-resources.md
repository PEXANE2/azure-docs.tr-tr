---
author: dominicbetts
ms.author: dominicbetts
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: 6cd983b6421fc821853aa22d4dc2c297f672d292
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87336913"
---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek IoT Tak ve Kullan makaleleriyle devam etmeyi planlıyorsanız, bu makalede kullandığınız kaynakları tutabilir ve yeniden kullanabilirsiniz. Aksi takdirde, ek ücretlerden kaçınmak için bu makalede oluşturduğunuz kaynakları silebilirsiniz.

Aşağıdaki Azure CLı komutuyla tüm kaynak grubunu silerek hub ve kayıtlı cihazı aynı anda silebilirsiniz. Bu kaynaklar, tutmak istediğiniz diğer kaynaklarla bir kaynak grubunu paylaştığını bu komutu kullanmayın.

```azurecli-interactive
az group delete --name <YourResourceGroupName>
```

Yalnızca IoT Hub 'ını silmek için Azure CLı kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name <YourIoTHubName>
```

Yalnızca IoT Hub 'ınıza kaydettiğiniz cihaz kimliğini silmek için Azure CLı kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub device-identity delete --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Ayrıca, kopyalanan örnek dosyaları geliştirme makinenizden da kaldırmak isteyebilirsiniz.
