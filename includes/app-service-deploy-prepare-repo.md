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
ms.openlocfilehash: f783245b17a880a67b62aa24a3feabb9b113053b
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102122264"
---
## <a name="prepare-your-repository"></a>Deponuzu hazırlayın

Azure App Service yapı sunucusundan otomatik derlemeler almak için, depo köklerinizin projenizde doğru dosyalara sahip olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*. sln_, _*. csproj_ veya _default. aspx_ |
| ASP.NET Core | _*. sln_ veya _*. csproj_ |
| PHP | _index. php_ |
| Ruby (yalnızca Linux) | _Gemfile_ |
| Node.js | Bir başlangıç betiğine _server.js_, _app.js_ veya _package.js_ |
| Python | _\* . kopyala_, _requirements.txt_ veya _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default. asp_, _index.htm_, _index.html_ veya _iisstart.htm_ |
| Web İşleri | _\<job_name>/Run.\<extension>_ _uygulama \_ verileri/işleri/_ sürekli Web işleri için sürekli veya tetiklenen Web işleri için _uygulama \_ verileri/işleri_ /işleri. Daha fazla bilgi için bkz. [kudu WebJobs belgeleri](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| İşlevler | Bkz. [Azure işlevleri Için sürekli dağıtım](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Dağıtımınızı özelleştirmek için depo köküne bir *. Deployment* dosyası ekleyin. Daha fazla bilgi için bkz. dağıtımları ve [özel dağıtım betiğini](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) [Özelleştirme](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) .

> [!NOTE]
> Visual Studio 'da geliştirirseniz, [Visual Studio 'nun sizin için bir depo oluşturmasına](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio&preserve-view=true)izin verin. Proje git kullanılarak dağıtılmaya hemen hazırlanın.
>

