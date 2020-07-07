---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 0dd6618bdee8e6810d414d4b04b16a1e0a9c90ed
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905639"
---
Kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz. İlk olarak, Cloud Shell aşağıdaki komutu çalıştırarak kapsayıcı günlüğünü açın:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Kapsayıcı günlüğü açıldıktan sonra, günlük akışını görmek için şu komutu çalıştırın:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Ayrıca, konumundaki tarayıcıdan günlük dosyalarını inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .

Günlük akışını istediğiniz zaman durdurmak için, yazın `Ctrl` + `C` .
