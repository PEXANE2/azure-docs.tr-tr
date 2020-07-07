---
author: baanders
description: Azure dijital TWINS için dosya ekleme-Cloud Shell ve IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 8be070826de0334483f4150925c05cb4dfb73f2c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85805583"
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

Daha sonra, Azure dijital TWINS ve diğer IoT hizmetleriyle etkileşime yönelik komutları etkinleştirmek için [**Azure CLI için Microsoft Azure IoT uzantısını**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) Cloud Shell ekleyeceksiniz. 

İlk olarak, zaten yüklemiş olduğunuz tüm uzantıların bir listesini görmek için bu komutu çalıştırın.

```azurecli-interactive
az extension list
```

Çıktıda, `"name"` uzantıların adlarını görmek için her bir liste girişi alanını bulun.

Uzantı kurulumu için aşağıdaki komutlardan hangisinin çalıştırılacağını öğrenmek için çıktıyı kullanın (birden fazla çalıştırma olabilirsiniz).
* Liste şunu içeriyorsa `azure-iot` : uzantıya zaten sahipsiniz. En son güncelleştirmeye sahip olduğunuzdan emin olmak için bu komutu çalıştırın:

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

* Liste **şunu içermiyorsa** `azure-iot` : uzantıyı yüklemeniz gerekir. Şu komutu kullanın:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

* Liste içeriyorsa `azure-iot-cli-ext` : Bu, uzantının eski sürümüdür. Tek seferde uzantının yalnızca bir sürümü yüklenmelidir, bu nedenle eski uzantıyı kaldırmanız gerekir. Şu komutu kullanın:

   ```azurecli-interactive
   az extension remove --name azure-cli-iot-ext
   ```

Artık Cloud Shell Azure dijital TWINS ile çalışmaya hazırsınız.

`az dt -h`Kullanılabilir en üst düzey Azure dijital TWINS komutlarının bir listesini görmek için bunu dilediğiniz zaman çalıştırabilirsiniz.