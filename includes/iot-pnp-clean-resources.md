---
author: baanders
ms.author: baanders
ms.service: iot-pnp
ms.topic: include
ms.date: 11/15/2019
ms.openlocfilehash: d88d166e001e71cabdabc3a3d344adc4da19aa8c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75453681"
---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Ek IoT Tak ve Çalıştır makaleleriyle devam etmeyi planlıyorsanız, bu hızlı başlatmada kullandığınız kaynakları saklayabilir ve yeniden kullanabilirsiniz. Aksi takdirde, ek ücretlerden kaçınmak için bu hızlı başlatmada oluşturduğunuz kaynakları silebilirsiniz.

Azure CLI için aşağıdaki komutla tüm kaynak grubunu silerek hem hub'ı hem de kayıtlı aygıtı aynı anda silebilirsiniz. (Ancak, bu kaynaklar farklı amaçlariçin sahip olduğunuz diğer kaynaklarla bir kaynak grubunu paylaşıyorsa bunu kullanmayın.)

```azurecli-interactive
az group delete --name <YourResourceGroupName>
```
Sadece IoT merkezini silmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub delete --name <YourIoTHubName>
```

IoT hub'ınıza kaydettiğiniz aygıt kimliğini silmek için Azure CLI kullanarak aşağıdaki komutu çalıştırın:

```azurecli-interactive
az iot hub device-identity delete --hub-name <YourIoTHubName> --device-id <YourDeviceID>
```

Klonlanan örnek dosyaları geliştirme makinenizden de kaldırmak isteyebilirsiniz.