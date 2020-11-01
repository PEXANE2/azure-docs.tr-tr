---
title: Mevcut bir .NET uygulamasını Service Fabric Mesh için kapsayıcılı hale getirme
description: Tam .NET Framework kullanan ASP.NET ve konsol projelerine Service Fabric kafes kapsayıcı düzenleme desteği ekleyin.
author: georgewallace
ms.author: gwallace
ms.date: 11/08/2018
ms.topic: conceptual
ms.openlocfilehash: 0f236292fff0d0e806e6eec32e1e058cbf67545c
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93144486"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Mevcut bir .NET uygulamasını Service Fabric Mesh için kapsayıcılı hale getirme

Bu makalede, var olan bir .NET uygulamasına Service Fabric kafes kapsayıcısı düzenleme desteğinin nasıl ekleneceği gösterilmektedir.

Visual Studio 2017 ' de, tam .NET Framework kullanan ASP.NET ve konsol projelerine kapsayıcı desteği ekleyebilirsiniz.

> [!NOTE]
> .NET **Core** projeleri Şu anda desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md)emin olun. Bu, Service Fabric çalışma zamanı, SDK, Docker, Visual Studio 2017 ' i yüklemeyi ve yerel bir küme oluşturmayı içerir.

## <a name="open-an-existing-net-app"></a>Mevcut bir .NET uygulamasını açın

Kapsayıcı düzenleme desteği eklemek istediğiniz uygulamayı açın.

Bir örnek denemek isterseniz [eShop](https://github.com/MikkelHegn/ContainersSFLab) kod örneğini kullanabilirsiniz. Bu makalenin geri kalanı, bu projeyi kullandığımızda, bu adımları kendi projenize uygulayabileceğiniz varsayırsınız.

**EShop** projesinin bir kopyasını alın:

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

Yüklendikten sonra Visual Studio 2017 Open **Containerssflab\eshoplegacywebformssolution\eshoplegacywebforms.sln** içinde.

## <a name="add-container-support"></a>Kapsayıcı desteği ekle
 
Service Fabric kafes araçlarını kullanarak mevcut bir ASP.NET veya konsol projesine kapsayıcı düzenleme desteğini aşağıdaki gibi ekleyin:

Visual Studio Çözüm Gezgini 'nde, proje adına (örneğin, **eshoplegacywebforms** ) sağ tıklayın ve ardından **Add**  >  **kapsayıcı Orchestrator desteği** Ekle ' yi seçin.
**Kapsayıcı Orchestrator desteği ekle** iletişim kutusu görünür.

![Visual Studio kapsayıcı Orchestrator iletişim kutusu Ekle](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Açılan listeden **Service Fabric kafes** ' i seçin ve ardından **Tamam** ' a tıklayın.


>[!NOTE]
> 2 Kasım 2020 ' den itibaren geçerli olan Docker Hub 'a yönelik anonim ve kimliği doğrulanmış istekler, Docker Ücretsiz plan hesaplarından [uygulanır](https://docs.docker.com/docker-hub/download-rate-limit/) ve IP adresi tarafından zorlanır. Daha ayrıntılı bilgi için bkz. [Docker Hub Ile kimlik doğrulama](https://docs.microsoft.com/azure/container-registry/buffer-gate-public-content#authenticate-with-docker-hub).
>
> Oran sınırlı olmamak için, `FROM microsoft/aspnet:4.7.2-windowsservercore-1803 AS base` Dockerfile dosyanızdaki varsayılan değer ile değiştirildiğinden emin olun `FROM mcr.microsoft.com/dotnet/framework/aspnet:4.7.2-windowsservercore-1803 AS base`

Araç daha sonra Docker 'ın yüklü olduğunu doğrular, projenize bir Dockerfile ekler ve projeniz için bir Docker görüntüsü çeker.  
Çözümünüze bir Service Fabric kafes uygulama projesi eklenir. Ağ yayımlama profillerinizi ve yapılandırma dosyalarını içerir. Projenin adı, projenin adı ile aynıdır (örneğin, **Eshoplegacywebformsapplication** gibi). 

Yeni kafes projesinde, dikkat etmeniz gereken iki klasör görürsünüz:
- Ağ gibi ek kafes kaynaklarını tanımlayan YAML dosyalarını içeren **uygulama kaynakları** .
- Uygulamanızın dağıtıldığında nasıl çalışacağını açıklayan bir Service. YAML dosyası içeren **hizmet kaynakları** .

Uygulamanıza kapsayıcı düzenleme desteği eklendikten sonra, Yerel Service Fabric ağ kümenizde .NET uygulamanızda hata ayıklamak için **F5** 'e basabilirsiniz. Service Fabric bir ağ kümesinde çalışan eShop ASP.NET uygulaması aşağıda verilmiştir: 

![eShop uygulaması](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

Artık uygulamayı Azure Service Fabric ağı 'nda yayımlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. Service Fabric kafeste uygulama yayımlama: [öğretici-Service Fabric bir kafes uygulaması dağıtma](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)