---
author: baanders
description: temel bir Azure dijital TWINS örneği ve uygulama kaydı Temizleme için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 45d6a806e94ceca9574b0ed5f73c2b87ef438438
ms.sourcegitcommit: c6b9a46404120ae44c9f3468df14403bcd6686c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88891513"
---
Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name <your-resource-group>
```

Ardından, aşağıdaki komutla istemci uygulamanız için oluşturduğunuz Azure Active Directory Uygulama kaydını silin:

```azurecli-interactive
az ad app delete --id <your-application-ID>
```