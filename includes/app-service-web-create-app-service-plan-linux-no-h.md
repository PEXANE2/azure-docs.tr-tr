---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 12/20/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: f3d558736751d3c50e3c007e3aebb369093ef856
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102244563"
---
Cloud Shell, komutuyla kaynak grubunda bir App Service planı oluşturun [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) .

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Aşağıdaki örnek, `myAppServicePlan` **ücretsiz** fiyatlandırma katmanında ( `--sku F1` ) ve bir Linux kapsayıcısında () adlı bir App Service planı oluşturur `--is-linux` .

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku F1 --is-linux
```

App Service planı oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  <JSON data removed for brevity.>
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
