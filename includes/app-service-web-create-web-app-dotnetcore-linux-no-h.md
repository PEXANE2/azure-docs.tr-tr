---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/22/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: f397a3df7280b9277b2b7205368ef5788ed321aa
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82206689"
---
App Service planında bir [Web uygulaması](../articles/app-service/containers/app-service-linux-intro.md) oluşturun. `myAppServicePlan` 

Cloud Shell [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest) komutunu kullanabilirsiniz. Aşağıdaki örnekte `<app-name>` kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). Çalışma zamanı, .NET Core `DOTNETCORE|LTS`3,1 olan olarak ayarlanır. Desteklenen tüm çalışma zamanlarını görmek için öğesini [`az webapp list-runtimes --linux`](/cli/azure/webapp?view=azure-cli-latest)çalıştırın. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|LTS" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|LTS" --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

<pre>
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
</pre>

Bir Linux kapsayıcısında, Git dağıtımı etkin boş bir web uygulaması oluşturdunuz.

> [!NOTE]
> Git uzak URL’si `deploymentLocalGitUrl` özelliği içinde `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git` biçiminde gösterilir. Bu URL’ye daha sonra ihtiyacınız olacağı için URL’yi kaydedin.
>

Şu anda .NET Core sürümünü düzgün şekilde yapılandırmak için aşağıdaki ek komutu çalıştırmanız gerekir (önceki adımdan biriyle değiştirin `<app-name>` ):

```azurecli-interactive
# Bash
az webapp config set --resource-group myResourceGroup --name <app-name> --linux-fx-version "DOTNETCORE|3.1"
# PowerShell
az --% webapp config set --resource-group myResourceGroup --name <app-name> --linux-fx-version "DOTNETCORE|3.1"
```
