---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/12/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: add0d392f39ab476c6d75f704d5b2e2e0faaa77c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67836743"
---
## <a name="prepare-your-repository"></a>Deponuzu hazırlayın

Azure App Service Kudu build sunucusundan otomatik yapılar almak için, depo root'unuzun projenizde doğru dosyalara sahip olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*.sln_, _*.csproj_, veya _default.aspx_ |
| ASP.NET Core | _*.sln_ veya _*.csproj_ |
| PHP | _Index.php_ |
| Ruby (sadece Linux) | _Gemfile_ |
| Node.js | _server.js_, _app.js_veya _package.json_ bir başlangıç komut dosyası ile |
| Python | .py , _requirements.txt_, veya _runtime.txt_ _ \*_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, veya _iisstart.htm_ |
| WebJobs | _\<job_name>/çalıştır. \<uzantısı,_ sürekli Web İşleri için _Uygulama\_Verileri/işler/sürekli_ veya Tetiklenen Web İşleri için _\_App Data/jobs/tetiklenen/tetiklenen uygulama verileri/sürekli_ altında>. Daha fazla bilgi için [Kudu WebJobs belgelerine](https://github.com/projectkudu/kudu/wiki/WebJobs)bakın. |
| İşlevler | [Azure İşlevler için Sürekli dağıtıma](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment)bakın. |

Dağıtımınızı özelleştirmek için depo köküne bir *.deployment* dosyası ekleyin. Daha fazla bilgi için dağıtımları ve [Özel dağıtım komut dosyalarını özelleştir'e](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script)bakın. [Customize deployments](https://github.com/projectkudu/kudu/wiki/Customizing-deployments)

> [!NOTE]
> Visual Studio'da gelişirseniz, [Visual Studio'nun sizin için bir depo oluşturmasına](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio)izin verin. Proje Git kullanılarak hemen dağıtılacak hazırdır.
>

