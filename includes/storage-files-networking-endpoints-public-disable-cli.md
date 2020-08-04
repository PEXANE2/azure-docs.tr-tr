---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 195caa6f8d7c00c741741fbf80a77bd7fe5579b0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465045"
---
Aşağıdaki CLı komutu, depolama hesabının genel uç noktasındaki tüm trafiği reddeder. Bu komutun `-bypass` parametresi olarak ayarlandığını unutmayın `AzureServices` . Bu, Azure Dosya Eşitleme gibi güvenilen ilk taraf hizmetlerin, genel uç nokta aracılığıyla depolama hesabına erişmesi için izin verir.

```bash
# This assumes $storageAccountResourceGroupName and $storageAccountName 
# are still defined from the beginning of this guide.
az storage account update \
    --resource-group $storageAccountResourceGroupName \
    --name $storageAccountName \
    --bypass "AzureServices" \
    --default-action "Deny" \
    --output none
```