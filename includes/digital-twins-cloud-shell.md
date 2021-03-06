---
author: baanders
description: Azure dijital TWINS için dosya ekleme-Cloud Shell ve IoT uzantısını ayarlama
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: e61b6b9a09d759571029db4f01dd8f9d707ca518
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102244683"
---
Açık bir [Azure Cloud Shell](https://shell.azure.com) penceresinde Azure dijital TWINS ile çalışmaya başlamak için ilk yapmanız gereken oturum açma ve kabuk bağlamını bu oturum için aboneliğinize ayarlamış. Cloud Shell şu komutları çalıştırın:

```azurecli-interactive
az login
az account set --subscription "<your-Azure-subscription-ID>"
```
> [!TIP]
> Yukarıdaki komutta KIMLIK yerine abonelik adınızı de kullanabilirsiniz. 

Bu aboneliği Azure Digital TWINS ile ilk kez kullandıysanız, Azure Digital TWINS ad alanına kaydolmak için bu komutu çalıştırın. (Emin değilseniz, geçmişte bir zaman gerçekleştirseniz bile bu işlemi yeniden çalıştırmak daha iyi olur.)

```azurecli-interactive
az provider register --namespace 'Microsoft.DigitalTwins'
```

Daha sonra, Azure dijital TWINS ve diğer IoT hizmetleriyle etkileşime yönelik komutları etkinleştirmek için [**Azure CLI için Microsoft Azure IoT uzantısını**](/cli/azure/ext/azure-iot/iot) Cloud Shell ekleyeceksiniz. 

[!INCLUDE [digital-twins-cloud-shell-extensions.md](digital-twins-cloud-shell-extensions.md)]

Artık Cloud Shell Azure dijital TWINS ile çalışmaya hazırsınız.

`az dt -h`Kullanılabilir en üst düzey Azure dijital TWINS komutlarının bir listesini görmek için bunu dilediğiniz zaman çalıştırabilirsiniz.