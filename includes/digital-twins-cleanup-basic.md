---
author: baanders
description: Azure dijital TWINS örneği temizlemek için ekleme dosyası
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: 9a02c4f5c5699b4a6308bfaa519fa9eb776414d6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102244740"
---
* **Bu öğreticide oluşturduğunuz kaynakların hiçbirine ihtiyacınız** yoksa, [az Group Delete](/cli/azure/group#az-group-delete) komutuyla bu makaledeki Azure dijital TWINS örneğini ve diğer tüm kaynakları silebilirsiniz. Bu, kaynak grubunun yanı sıra kaynak grubundaki tüm Azure kaynaklarını da siler.
    
    > [!IMPORTANT]
    > Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun.
    
    [Azure Cloud Shell](https://shell.azure.com)açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```