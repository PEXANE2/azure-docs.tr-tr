---
title: Windows geliştirme ortamı ayarlama
description: Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra Windows üzerinde uygulama derlemek için hazır hale gelirsiniz.
author: peterpogorski
ms.topic: conceptual
ms.date: 03/02/2020
ms.custom: sfrev
ms.openlocfilehash: f08c6b0675475b4e15ce6db3a9dbe0e2863b9ddb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252760"
---
# <a name="prepare-your-development-environment-on-windows"></a>Windows üzerinde geliştirme ortamınızı hazırlama

> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>

Windows geliştirme makinenizde [Azure Service Fabric uygulamaları][1] derlemek ve çalıştırmak için Service Fabric çalışma zamanını, SDK'yı ve araçları yükleyin. Ayrıca, SDK'da yer alan [Windows PowerShell komut dosyalarının yürütülmesini etkinleştirmeniz](#enable-powershell-script-execution) gerekir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="supported-operating-system-versions"></a>Desteklenen işletim sistemi sürümleri

Geliştirme için şu işletim sistemi sürümleri desteklenir:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 desteği:
> - Windows 7 varsayılan olarak yalnızca Windows PowerShell 2.0 içerir. Service Fabric PowerShell cmdlet’leri PowerShell 3.0 veya üzerini gerektirir. Windows [PowerShell 5.1'i][powershell5-download] Microsoft Download Center'dan indirebilirsiniz.
> - Windows 7'de Service Fabric Ters Proxy kullanılamaz.

## <a name="install-the-sdk-and-tools"></a>SDK'yı ve araçları yükleme

Web Platform Installer (WebPI) SDK ve araçları yüklemek için önerilen yoldur. WebPI kullanarak çalışma zamanı hataları alırsanız, belirli bir Hizmet Kumaşı sürümü için sürüm notlarında yükleyicilere doğrudan bağlantılar da bulabilirsiniz. Sürüm notları [Service Fabric takım blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/)çeşitli sürüm duyurular bulunabilir.

> [!NOTE]
> Yerel Hizmet Kumaş geliştirme küme yükseltmeleri desteklenmez.

### <a name="to-use-visual-studio-2017-or-2019"></a>Visual Studio 2017 veya 2019'u kullanmak için

Service Fabric Tools, Visual Studio 2017 ve 2019'daki Azure Geliştirme iş yükünün bir parçasıdır. Bu iş yükünü Visual Studio yüklemenizin bir parçası olarak etkinleştirin.
Ayrıca Web Platformu Yükleyicisini kullanarak Microsoft Azure Service Fabric SDK'sını da yüklemeniz gerekir.

* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Visual Studio 2015'i kullanmak için (Visual Studio 2015 Güncelleştirme 2 veya üzeri gerekir)

Visual Studio 2015 için Service Fabric araçları Web Platformu Yükleyicisi kullanılarak SDK ile birlikte yüklenir:

* [Microsoft Azure Hizmet Kumaşı SDK ve Araçlarını Yükleme][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Yalnızca SDK'yı yükleme

Yalnızca SDK'yı yüklemeniz gerekiyorsa bu paketi yükleyebilirsiniz:

* [Microsoft Azure Service Fabric SDK'sını yükleyin][core-sdk]

Geçerli sürümler şunlardır:

* Servis Kumaş ı SDK ve Aletler 4.0.470
* Servis Kumaş çalışma süresi 7.0.470

Desteklenen sürümlerin listesi için [Service Fabric sürümlerine](service-fabric-versions.md) bakın

> [!NOTE]
> Uygulama veya Küme yükseltmeleri için tek makine kümeleri (OneBox) desteklenmez; Bir Küme yükseltmesi gerçekleştirmeniz gerekiyorsa veya Uygulama yükseltmesi yaparken herhangi bir sorun varsa OneBox kümesini silin ve yeniden oluşturun. 

## <a name="enable-powershell-script-execution"></a>PowerShell betik yürütmesini etkinleştirme

Service Fabric, yerel geliştirme merkezi oluşturmak ve Visual Studio'dan uygulamaları dağıtmak için Windows PowerShell betiklerini kullanır. Varsayılan olarak, Windows bu betiklerin çalışmasını engeller. Betikleri etkinleştirmek için PowerShell yürütme ilkenizi değiştirmeniz gerekir. PowerShell'i yönetici olarak açın ve şu komutu girin:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="install-docker-optional"></a>Docker'ı yükleyin (isteğe bağlı)

[Service Fabric,](service-fabric-containers-overview.md) mikro hizmetleri bir makine kümesine dağıtmak için kullanılan bir konteyner orkestratördür. Windows kapsayıcı uygulamalarını yerel geliştirme kümenizde çalıştırmak için önce Windows için Docker'ı yüklemeniz gerekir. [Windows için Docker CE alın (kararlı)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Docker’ı yükleyip başlattıktan sonra tepsi simgesine sağ tıklayıp **Windows kapsayıcılarına geç** öğesini seçin. Bu adım, Windows temelinde Docker görüntülerini çalıştırmak için gereklidir.

## <a name="next-steps"></a>Sonraki adımlar

Artık geliştirme ortamınızı ayarlamayı tamamladığınıza göre, uygulama derlemeye ve çalıştırmaya başlayın.

* [Uygulamaları nasıl oluşturup dağıtacak ve yöneteceklerini öğrenin](service-fabric-tutorial-create-dotnet-app.md)
* [Programlama modelleri hakkında bilgi edinin: Reliable Services ve Reliable Actors](service-fabric-choose-framework.md)
* [GitHub'da Service Fabric kod örneklerine bakın](https://aka.ms/servicefabricsamples)
* [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[1]: https://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric kampanya sayfası"
[2]: https://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VS 2015 WebPI bağlantısı"
[full-bundle-dev15]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI bağlantısı"
[core-sdk]:https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI bağlantısı"
[powershell5-download]:https://www.microsoft.com/download/details.aspx?id=54616
