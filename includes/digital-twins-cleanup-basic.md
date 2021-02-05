---
author: baanders
description: Azure dijital TWINS örneği temizlemek için ekleme dosyası
ms.service: digital-twins
ms.topic: include
ms.date: 2/4/2021
ms.author: baanders
ms.openlocfilehash: c3c1b814b357a2e4b724590261657e485852f99c
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575700"
---
* **Bu öğreticide oluşturduğunuz kaynakların hiçbirine ihtiyacınız** yoksa, [az Group Delete](/cli/azure/group?preserve-view=true&view=azure-cli-latest#az-group-delete) komutuyla bu makaledeki Azure dijital TWINS örneğini ve diğer tüm kaynakları silebilirsiniz. Bu, kaynak grubunun yanı sıra kaynak grubundaki tüm Azure kaynaklarını da siler.
    
    > [!IMPORTANT]
    > Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun.
    
    [Azure Cloud Shell](https://shell.azure.com)açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.
    
    ```azurecli-interactive
    az group delete --name <your-resource-group>
    ```