---
author: baanders
description: temel bir Azure dijital TWINS örneğini temizlemek için ekleme dosyası
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 4c03ef942896dda63f678018cdd257024cfbb6d4
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95557492"
---
Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](/cli/azure/group?preserve-view=true&view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu komut, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun.

Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name <your-resource-group>
```