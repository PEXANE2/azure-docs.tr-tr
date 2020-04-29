---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: d88d166e001e71cabdabc3a3d344adc4da19aa8c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75453681"
---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek IoT Tak ve Kullan makaleleriyle devam etmeyi planlıyorsanız, bu hızlı başlangıçta kullandığınız kaynakları tutabilir ve yeniden kullanabilirsiniz. Aksi takdirde, ek ücretlerden kaçınmak için bu hızlı başlangıçta oluşturduğunuz kaynakları silebilirsiniz.

Tüm kaynak grubunu Azure CLı için aşağıdaki komutla silerek hem hub hem de kayıtlı cihazı bir kerede silebilirsiniz. (Ancak, bu kaynaklar farklı amaçlar için sahip olduğunuz diğer kaynaklarla bir kaynak grubu paylaştığını bu şekilde kullanmayın.)

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