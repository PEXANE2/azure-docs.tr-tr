---
title: Service Fabric Mesh için windows geliştirme ortamı ayarlama
description: Service Fabric Mesh uygulaması geliştirebilmek ve bunu Azure Service Fabric Mesh'e dağıtabilmek için Windows geliştirme ortamınızı ayarlayın.
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.openlocfilehash: a674047722d4deca02d8f4d38a0826e479065037
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259207"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Service Fabric Mesh uygulamalarını derlemek için Windows geliştirme ortamınızı ayarlayın

Windows geliştirme makinenizde Azure Service Fabric Mesh uygulamaları oluşturmak ve çalıştırmak için şunları yapmanız gerekir:

* Docker
* Visual Studio 2017 veya sonrası
* Servis Kumaş Örgü çalışma süresi
* Servis Kumaş Örgü SDK ve araçları.

Ve Windows'un aşağıdaki sürümlerinden biri:

* Windows 10 (Kurumsal, Profesyonel veya Eğitim) sürümleri 1709 (Fall Creators güncellemesi) veya 1803 (Windows 10 Nisan 2018 güncelleştirmesi)
* Windows Server sürüm 1709
* Windows Server sürüm 1803

Aşağıdaki yönergeler, çalıştırdığınız Windows sürümüne göre her şeyi yüklemenize yardımcı olur.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 veya daha sonraki servis kumaş örgü uygulamaları dağıtmak için gereklidir. [Sürüm 15.6.0'ı][download-visual-studio] veya üstünü yükleyin ve aşağıdaki iş yüklerini etkinleştirin:

* ASP.NET ve web geliştirme
* Azure Geliştirme

## <a name="install-docker"></a>Docker'ı yükleme

Docker zaten yüklüyse, en son sürüme sahip olduğundan emin olun. Docker, yeni bir sürüm çıktığında sizi isteyebilir, ancak en son sürüme sahip olduğunuzdan emin olmak için el ile kontrol edin.

#### <a name="install-docker-on-windows-10"></a>Docker'ı Windows 10'a yükleme

Service Fabric Mesh tarafından kullanılan kapsayıcılı Service Fabric uygulamalarını desteklemek için en yeni [Docker Community Edition for Windows][download-docker] sürümünü indirin ve yükleyin.

Yükleme sırasında, istendiğinde **Linux kapsayıcıları yerine Windows kapsayıcılarını kullan**'ı seçin.

Makinenizde Hyper-V etkin değilse, Docker'ın yükleyicisi bunu etkinleştirme teklifinde bulunacaktır. Bu seçenek sunulursa **Tamam**'a tıklayın.

#### <a name="install-docker-on-windows-server-2016"></a>Windows Server 2016'da Docker'ı yükleyin

Hyper-V rolleri etkin değilse PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırarak Hyper-V'yi etkinleştirdikten sonra bilgisayarınızı yeniden başlatın. Daha fazla bilgi için bkz. [Windows Server için Docker Enterprise Edition][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Bilgisayarınızı yeniden başlatın.

Docker'ı yüklemek için PowerShell'i yönetici olarak açın ve aşağıdaki komutu çalıştırın:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK ve araçlar

Aşağıdaki sırayla Service Fabric Mesh çalışma zamanını, SDK'yı ve araçları yükleyin.

1. Web Platformu Yükleyicisi'ni kullanarak [Service Fabric Mesh SDK'sını][download-sdkmesh] yükleyin. Bu işlem Microsoft Azure Service Fabric SDK’sını ve çalışma zamanını da yükler.
2. Visual Studio Market'ten [Visual Studio Service Fabric Mesh Araçları (önizleme) uzantısını][download-tools] yükleyin.

## <a name="build-a-cluster"></a>Küme oluşturma

> [!IMPORTANT]
> Küme oluşturabilmeniz için Docker'in çalışıyor olması **gerekir**.
> Terminal penceresi açarak ve hata oluşup oluşmadığını görmek için `docker ps` komutunu çalıştırarak Docker'ı çalışmasını test edin. Yanıt bir hata göstermiyorsa, Docker çalışıyor ve siz de küme oluşturmaya hazırsınız demektir.

> [!Note]
> Windows Fall Creators güncelleştirme (sürüm 1709) makinesinde geliştiriyorsanız, yalnızca Windows sürüm 1709 docker görüntülerini kullanabilirsiniz.
> Windows 10 Nisan 2018 güncelleştirmesi (sürüm 1803) makinesinde geliştiriyorsanız, Windows sürüm 1709 veya 1803 docker görüntülerini kullanabilirsiniz.

Visual Studio kullanıyorsanız, bu bölümü atlayabilirsiniz, çünkü visual studio'nuz yoksa sizin için yerel bir küme oluşturur.

Aynı anda tek bir Service Fabric uygulaması oluştururken ve çalıştırırken en iyi hata ayıklama performansı için tek düğümlü yerel geliştirme kümesi oluşturun. Aynı anda birden çok uygulama çalıştırıyorsanız, beş düğümlü yerel geliştirme kümesi oluşturun. Bir Service Fabric Mesh projesini dağıttığınızda veya hata ayıkladiğinizde küme çalışıyor olmalıdır.

Çalışma süresini yükledikten sonra, SDK'lar, Visual Studio araçları, Docker ve Docker'ın çalışmasını sağlar, bir geliştirme kümesi oluşturun.

1. PowerShell pencerenizi kapatın.
2. Yönetici olarak yeni, yükseltilmiş bir PowerShell penceresi açın. Yüklediğiniz en son yüklenen Service Fabric modüllerinin açılması için bu adım gereklidir.
3. Aşağıdaki PowerShell komutunu çalıştırarak geliştirme kümesini oluşturun:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Yerel küme yöneticisi aracını başlatmak için, aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Hizmet küme yöneticisi aracı çalışmaya başladıktan sonra (sistem tepsinizde görünür), sağ tıklatın ve **Yerel Küme başlat'ı**tıklatın.

![Şekil 1 - Yerel kümeyi başlatın](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Artık Service Fabric Mesh uygulamaları oluşturmaya hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

[Azure Service Fabric uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md) öğreticisini okuyun.

[Sık sorulan soruların ve bilinen sorunların](service-fabric-mesh-faq.md)yanıtlarını bulun.

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/