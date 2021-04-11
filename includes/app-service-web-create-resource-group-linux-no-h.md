---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: d1f78034c2142bfb0fc787683b7efed22ae2698c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "102244711"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell, komutuyla bir kaynak grubu oluşturun [`az group create`](/cli/azure/group#az-group-create) . Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Temel** katmanda Linux üzerinde App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice#az-appservice-list-locations) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.