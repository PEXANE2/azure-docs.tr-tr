---
author: baanders
description: temel bir Azure dijital TWINS örneğini temizlemek için ekleme dosyası
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 2e095a7b286a8860535d6b58fa93098735b30c87
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92372335"
---
Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name <your-resource-group>
```