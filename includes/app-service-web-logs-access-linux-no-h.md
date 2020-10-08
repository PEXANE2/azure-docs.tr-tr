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
ms.openlocfilehash: df71f0804b62eb4b17ff8d2f652b076b5c64c959
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91822768"
---
Kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz.

İlk olarak, aşağıdaki komutu çalıştırarak kapsayıcı günlüğünü açın:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group <resource-group-name> --docker-container-logging filesystem
```

`<app-name>`Ve ' i `<resource-group-name>` Web uygulamanız için uygun adlarla değiştirin.

Kapsayıcı günlüğü açıldıktan sonra, günlük akışını görmek için şu komutu çalıştırın:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group <resource-group-name>
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

Günlük akışını istediğiniz zaman durdurmak için **CTRL** + **C**yazın.

Günlük dosyalarını konumundaki bir tarayıcıda da inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .
