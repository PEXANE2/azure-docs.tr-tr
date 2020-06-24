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
ms.openlocfilehash: e6c4b07d01a4992e22107cb7d524646f439c37c6
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905876"
---
Uygulama kodunuzun içinden oluşturulan konsol günlüklerine erişmek için, App Service [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırarak tanılama günlüğünü açın:

```azurecli-interactive
az webapp log config --resource-group <resource-group-name> --name <app-name> --application-logging true --level Verbose
```

İçin olası değerler `--level` şunlardır: `Error` , `Warning` , `Info` ve `Verbose` . Sonraki her düzey, önceki düzeyi içerir. Örneğin: `Error` yalnızca hata iletilerini içerir ve `Verbose` tüm iletileri içerir.

Tanılama günlüğü açıldıktan sonra günlük akışını görmek için şu komutu çalıştırın:

```azurecli-interactive
az webapp log tail --resource-group <resource-group-name> --name <app-name>
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Ayrıca, konumundaki tarayıcıdan günlük dosyalarını inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .

Günlük akışını istediğiniz zaman durdurmak için, yazın `Ctrl` + `C` .
