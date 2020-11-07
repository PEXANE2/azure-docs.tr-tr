---
author: baanders
description: temel bir Azure dijital TWINS örneğini temizlemek için ekleme dosyası
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: ab179c5f0c56a7d6fd2ebd41e13d27a832512b54
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358089"
---
Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu komut, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun.

Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name <your-resource-group>
```