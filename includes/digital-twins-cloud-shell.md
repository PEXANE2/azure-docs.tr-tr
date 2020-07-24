---
author: baanders
description: Azure dijital TWINS için dosya ekleme-Cloud Shell ve IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: b7c91d648c06970d53799c6ff505919dea17b3c0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87032246"
---
Açık bir [Azure Cloud Shell](https://shell.azure.com) penceresinde Azure dijital TWINS ile çalışmaya başlamak için ilk yapmanız gereken oturum açma ve kabuk bağlamını bu oturum için aboneliğinize ayarlamış. Cloud Shell şu komutları çalıştırın:

```azurecli
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> Yukarıdaki komutta KIMLIK yerine abonelik adınızı de kullanabilirsiniz. 

Bu aboneliği Azure Digital TWINS ile ilk kez kullandıysanız, Azure Digital TWINS ad alanına kaydolmak için bu komutu çalıştırın. (Emin değilseniz, geçmişte bir zaman gerçekleştirseniz bile bu işlemi yeniden çalıştırmak daha iyi olur.)

```azurecli
az provider register --namespace 'Microsoft.DigitalTwins'
```

Daha sonra, Azure dijital TWINS ve diğer IoT hizmetleriyle etkileşime yönelik komutları etkinleştirmek için [**Azure CLI için Microsoft Azure IoT uzantısını**](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot?view=azure-cli-latest) Cloud Shell ekleyeceksiniz. 

İlk olarak, zaten yüklemiş olduğunuz tüm uzantıların bir listesini görmek için bu komutu çalıştırın.

```azurecli-interactive
az extension list
```

Çıktı, şu anda sahip olduğunuz tüm uzantıların bir dizisidir. `"name"`Uzantıların adlarını görmek için her liste girişi alanını bulun.

Uzantı kurulumu için aşağıdaki komutlardan hangisinin çalıştırılacağını öğrenmek için çıktıyı kullanın (birden fazla çalıştırma olabilirsiniz).
* Liste şunu içeriyorsa `azure-iot` : uzantıya zaten sahipsiniz. En son güncelleştirmeye sahip olduğunuzdan emin olmak için bu komutu çalıştırın ve kullanılabilir başka güncelleştirme yok:

   ```azurecli-interactive
   az extension update --name azure-iot
   ```

* Liste **şunu içermiyorsa** `azure-iot` : uzantıyı yüklemeniz gerekir. Şu komutu çalıştırın:

    ```azurecli-interactive
    az extension add --name azure-iot
    ```

* Liste içeriyorsa `azure-iot-cli-ext` : Bu, uzantının eski sürümüdür. Tek seferde uzantının yalnızca bir sürümü yüklenmelidir, bu nedenle eski uzantıyı kaldırmanız gerekir. Şu komutu çalıştırın:

   ```azurecli-interactive
   az extension remove --name azure-cli-iot-ext
   ```

Artık Cloud Shell Azure dijital TWINS ile çalışmaya hazırsınız.

`az dt -h`Kullanılabilir en üst düzey Azure dijital TWINS komutlarının bir listesini görmek için bunu dilediğiniz zaman çalıştırabilirsiniz.