---
author: baanders
description: Azure dijital TWINS için dosya ekleme-Cloud Shell ve IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 76480959d94b9e2a2e04857c85b7359105e73e4f
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612896"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Cloud Shell oturum ayarlama

Cloud Shell bir pencere açtıktan sonra, ilk yapmanız gereken oturum açma ve kabuk bağlamını bu oturum için aboneliğinize ayarlamış. 

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Bu aboneliği Azure Digital TWINS ile ilk kez kullandıysanız, Azure Digital TWINS ad alanına kaydolmak için bu komutu çalıştırın. (Emin değilseniz, geçmişte bir zaman gerçekleştirseniz bile bu işlemi yeniden çalıştırmak daha iyi olur.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Daha sonra, Azure CLı için Microsoft Azure IoT uzantısını eklemek üzere Cloud Shell Örneğinizde aşağıdaki komutu çalıştırın.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

> [!NOTE]
> Bu makalede, adlı Azure IoT uzantısının en yeni sürümü kullanılmıştır `azure-iot` . Eski sürüm çağrılır `azure-iot-cli-ext` . Aynı anda yalnızca bir sürümü yüklü olmalıdır. `az extension list`Şu anda yüklü olan uzantıları doğrulamak için komutunu kullanabilirsiniz.
> `az extension remove --name azure-cli-iot-ext`Uzantının eski sürümünü kaldırmak için kullanın.
> `az extension add --name azure-iot`Uzantının yeni sürümünü eklemek için kullanın. Yüklediğiniz uzantıları görmek için kullanın `az extension list` .

> [!TIP]
> `az dt -h`En üst düzey Azure dijital TWINS komutlarını görmek için komutunu kullanabilirsiniz.