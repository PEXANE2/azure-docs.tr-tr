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
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67836743"
---
## <a name="prepare-your-repository"></a>Deponuzu hazırlayın

Azure App Service kudu yapı sunucusundan otomatik derlemeler almak için, depo köklerinizin projenizde doğru dosyalara sahip olduğundan emin olun.

| Çalışma Zamanı | Kök dizin dosyaları |
|-|-|
| ASP.NET (yalnızca Windows) | _*. sln_, _*. csproj_veya _default. aspx_ |
| ASP.NET Core | _*. sln_ veya _*. csproj_ |
| PHP | _index. php_ |
| Ruby (yalnızca Linux) | _Gemfile_ |
| Node.js | Start betiği ile _Server. js_, _app. js_veya _Package. JSON_ |
| Python | . Kopyala, _requirements. txt_veya _Runtime. txt_ _ \*_ |
| HTML | _default. htm_, _default. html_, _default. asp_, _index. htm_, _index. html_veya _iisstart. htm_ |
| WebJobs | _\<job_name>/Run. uzantı \<_ , _uygulama\_verileri/işleri/_ sürekli Web işleri için sürekli veya tetiklenen Web işleri için _tetiklenen\_uygulama verileri/işler_ altında>. Daha fazla bilgi için bkz. [kudu WebJobs belgeleri](https://github.com/projectkudu/kudu/wiki/WebJobs). |
| İşlevler | Bkz. [Azure işlevleri Için sürekli dağıtım](../articles/azure-functions/functions-continuous-deployment.md#requirements-for-continuous-deployment). |

Dağıtımınızı özelleştirmek için depo köküne bir *. Deployment* dosyası ekleyin. Daha fazla bilgi için bkz. dağıtımları ve [özel dağıtım betiğini](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) [Özelleştirme](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) .

> [!NOTE]
> Visual Studio 'da geliştirirseniz, [Visual Studio 'nun sizin için bir depo oluşturmasına](/azure/devops/repos/git/creatingrepo?view=vsts&tabs=visual-studio)izin verin. Proje git kullanılarak dağıtılmaya hemen hazırlanın.
>

