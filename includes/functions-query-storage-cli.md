---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4fe159660421113e0f0ac0586ae7e4a22d5bcda7
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77474126"
---
### <a name="query-the-storage-queue"></a>Depolama sırasını sorgula

Aşağıdaki örnekte [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) olduğu gibi, hesabınızdaki Depolama kuyruklarını görüntülemek için komutu kullanabilirsiniz:

```azurecli-interactive
az storage queue list --output tsv
```

Bu komutun çıktısı, `outqueue`işlev çalıştırıldığında oluşturulan sıra olan "" adlı bir sıra içerir.

Ardından, bu [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek) örnekteki gibi bu kuyruktaki iletileri görüntülemek için komutu kullanın:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Döndürülen dize, işlevi sınamak için gönderdiğiniz iletiyle aynı olmalıdır.

> [!NOTE]  
> Önceki örnekbase64 gelen döndürülen dize deşifre. Bunun nedeni, Sıra depolama bağlamalarının Azure Depolama'ya [base64 dizeleri](../articles/azure-functions/functions-bindings-storage-queue-trigger.md#encoding)olarak yazıp okumasidır.