---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4fe159660421113e0f0ac0586ae7e4a22d5bcda7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "77474126"
---
### <a name="query-the-storage-queue"></a>Depolama kuyruğunu sorgulama

[`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list)Aşağıdaki örnekte olduğu gibi, hesabınızdaki depolama kuyruklarını görüntülemek için komutunu kullanabilirsiniz:

```azurecli-interactive
az storage queue list --output tsv
```

Bu komutun çıktısı, işlev çalıştırıldığında oluşturulan kuyruk olan adlı bir kuyruk içerir `outqueue` .

Sonra, bu [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) kuyruktaki iletileri görüntülemek için komutunu kullanın:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Döndürülen dize, işlevi test etmek için gönderdiğiniz iletiyle aynı olmalıdır.

> [!NOTE]  
> Önceki örnek, Base64 olan döndürülen dizenin kodunu çözer. Bunun nedeni, kuyruk depolama bağlamalarının Azure depolama 'ya [Base64 dizeleri](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)olarak yazma ve okuma.