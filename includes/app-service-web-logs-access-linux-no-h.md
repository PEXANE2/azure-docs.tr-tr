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
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
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
> Ayrıca, tarayıcıdan `https://<app-name>.scm.azurewebsites.net/api/logs/docker` adresine giderek günlük dosyalarını inceleyebilirsiniz.

Günlük akışını dilediğiniz zaman durdurmak için `Ctrl`+`C` yazın.
