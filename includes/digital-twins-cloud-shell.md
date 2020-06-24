---
author: baanders
description: Azure dijital TWINS için dosya ekleme-Cloud Shell ve IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4aa016294f0ef3bd26f7f3ef6fa374e9367b672d
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85296976"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

### <a name="set-up-cloud-shell-session"></a>Cloud Shell oturum ayarlama

Cloud Shell bir pencere açtıktan sonra, ilk yapmanız gereken oturum açma ve kabuk bağlamını bu oturum için aboneliğinize ayarlamış. Cloud Shell şu komutları çalıştırın:

```azurecli
az login
az account set --subscription <your-Azure-subscription-ID>
```

Bu aboneliği Azure Digital TWINS ile ilk kez kullandıysanız, Azure Digital TWINS ad alanına kaydolmak için bu komutu çalıştırın. (Emin değilseniz, geçmişte bir zaman gerçekleştirseniz bile bu işlemi yeniden çalıştırmak daha iyi olur.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Daha sonra, Azure dijital TWINS ve diğer IoT hizmetleriyle etkileşime yönelik komutları etkinleştirmek için [**Azure CLI için Microsoft Azure IoT uzantısını**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) Cloud Shell ekleyeceksiniz. Uzantıyı eklemek için bu komutu kullanın:

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

Uzantıyı geçmişte yüklediyseniz, çıkış "Azure-IoT" uzantısı zaten yüklü olabilir. " Bu durumda, en son güncelleştirmeye sahip olduğunuzdan emin olmak için aşağıdakileri çalıştırın: 

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

Artık Cloud Shell Azure dijital TWINS ile çalışmaya hazırsınız.

`az dt -h`Kullanılabilir en üst düzey Azure dijital TWINS komutlarının bir listesini görmek için bunu dilediğiniz zaman çalıştırabilirsiniz.