---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: c70e2c166bee14ac58ee88bd18baf0cc61702767
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "67188845"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell, komutuyla bir kaynak grubu oluşturun [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) . Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Temel** katmanda Linux üzerinde App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.