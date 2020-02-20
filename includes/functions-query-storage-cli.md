---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4fe159660421113e0f0ac0586ae7e4a22d5bcda7
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77474126"
---
### <a name="query-the-storage-queue"></a>Depolama kuyruğunu sorgulama

Aşağıdaki örnekte olduğu gibi, hesabınızdaki depolama kuyruklarını görüntülemek için [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) komutunu kullanabilirsiniz:

```azurecli-interactive
az storage queue list --output tsv
```

Bu komutun çıktısı, işlev çalıştırıldığında oluşturulan kuyruk olan `outqueue`adlı bir sıra içerir.

Daha sonra, bu kuyruktaki iletileri görüntülemek için [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) komutunu kullanın, örneğin:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Döndürülen dize, işlevi test etmek için gönderdiğiniz iletiyle aynı olmalıdır.

> [!NOTE]  
> Önceki örnek, Base64 olan döndürülen dizenin kodunu çözer. Bunun nedeni, kuyruk depolama bağlamalarının Azure depolama 'ya [Base64 dizeleri](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)olarak yazma ve okuma.