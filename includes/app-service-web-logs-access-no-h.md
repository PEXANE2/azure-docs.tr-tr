---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 0dd6618bdee8e6810d414d4b04b16a1e0a9c90ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188847"
---
Kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz. İlk olarak, Bulut Kabuğu'nda aşağıdaki komutu çalıştırarak konteyner günlüğe kaydetmeyi açın:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Kapsayıcı günlüğe kaydetme açık olduktan sonra, günlük akışını görmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Ayrıca tarayıcıdan günlük dosyalarını da `https://<app-name>.scm.azurewebsites.net/api/logs/docker`inceleyebilirsiniz.

Günlük akışını herhangi bir zamanda `Ctrl` + `C`durdurmak için .
