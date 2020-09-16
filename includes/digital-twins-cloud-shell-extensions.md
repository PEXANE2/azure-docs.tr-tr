---
author: baanders
description: Azure dijital TWINS için içerme dosyası-en son IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 7/31/2020
ms.author: baanders
ms.openlocfilehash: 7e43ab6d419d6a08973864838c66aef0941d74f5
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90606806"
---
İlk olarak, zaten yüklemiş olduğunuz tüm uzantıların bir listesini görmek için bu komutu çalıştırın.

```azurecli
az extension list
```

Çıktı, şu anda sahip olduğunuz tüm uzantıların bir dizisidir. `"name"`Uzantıların adlarını görmek için her liste girişi alanını bulun.

Uzantı kurulumu için aşağıdaki komutlardan hangisinin çalıştırılacağını öğrenmek için çıktıyı kullanın (birden fazla çalıştırma olabilirsiniz).
* Liste şunu içeriyorsa `azure-iot` : uzantıya zaten sahipsiniz. En son güncelleştirmeye sahip olduğunuzdan emin olmak için bu komutu çalıştırın ve kullanılabilir başka güncelleştirme yok:

   ```azurecli
   az extension update --name azure-iot
   ```

* Liste **şunu içermiyorsa** `azure-iot` : uzantıyı yüklemeniz gerekir. Şu komutu çalıştırın:

    ```azurecli
    az extension add --name azure-iot
    ```

* Liste içeriyorsa `azure-iot-cli-ext` : Bu, uzantının eski sürümüdür. Tek seferde uzantının yalnızca bir sürümü yüklenmelidir, bu nedenle eski uzantıyı kaldırmanız gerekir. Şu komutu çalıştırın:

   ```azurecli
   az extension remove --name azure-cli-iot-ext
   ```