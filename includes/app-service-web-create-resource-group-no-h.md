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
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188852"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell, komutuyla bir kaynak grubu oluşturun [`az group create`](/cli/azure/group?view=azure-cli-latest) . Aşağıdaki örnek *Batı Avrupa* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur. **Ücretsiz** katmanda App Service için desteklenen tüm konumları görüntülemek için [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest) komutunu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. 

Komut tamamlandığında, bir JSON çıkışı size kaynak grubu özelliklerini gösterir.