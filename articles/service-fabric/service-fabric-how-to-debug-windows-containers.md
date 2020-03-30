---
title: Service Fabric ve VS ile Windows kapsayıcı hatalarını ayıklama
description: Visual Studio 2019'u kullanarak Azure Hizmet Dokusu'ndaki Windows kapsayıcılarını nasıl hata ayıklamakonusunda öğreneceksiniz.
ms.topic: article
ms.date: 02/14/2019
ms.author: mikhegn
ms.openlocfilehash: 2a00a352d09562ffe46dc8e6e63a5d4963ac3a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127620"
---
# <a name="how-to-debug-windows-containers-in-azure-service-fabric-using-visual-studio-2019"></a>Nasıl yapılır: Visual Studio 2019'u kullanarak Azure Hizmet Dokusundaki Windows kaplarını hata ayıklama

Visual Studio 2019 ile .NET uygulamalarını servis kumaşı hizmeti olarak kaplarda hata ayıklayabilirsiniz. Bu makalede, ortamınızı nasıl yapılandırdığınızı ve yerel bir Hizmet Kumaşı kümesinde çalışan bir kapsayıcıda bir .NET uygulamasını nasıl hata ayıkladığınızı gösterir.

## <a name="prerequisites"></a>Ön koşullar

* Windows 10'da, Windows [kapsayıcılarını çalıştırmak için Windows 10 yapılandırmak için](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10) bu hızlı başlatış izleyin
* Windows Server 2016'da, Windows kapsayıcılarını çalıştırmak için [Windows 2016'yı Yapılandırmak için](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) bu hızlı başlangıcı izleyin
* [Windows'da geliştirme ortamınızı hazırla'yı](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started) izleyerek yerel Service Fabric ortamınızı ayarlama

## <a name="configure-your-developer-environment-to-debug-containers"></a>Geliştirici ortamınızı hata ayıklama kapsayıcıları için yapılandırma

1. Bir sonraki adıma geçmeden önce Pencere docker hizmetinin çalıştığını unutmayın.

1. Kapsayıcılar arasındaki DNS çözümünü desteklemek için, makine adını kullanarak yerel geliştirme kümenizi ayarlamanız gerekir. Hizmetleri ters proxy üzerinden ele almak istiyorsanız, bu adımlar da gereklidir.
   1. PowerShell'i yönetici olarak aç
   2. Genellikle SDK Küme kurulum klasörüne `C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup`gidin.
   3. Komut dosyasını çalıştırma`DevClusterSetup.ps1`

      ``` PowerShell
        C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1
      ```

      > [!NOTE]
      > Tek düğümlü `-CreateOneNodeCluster` küme kurmak için kullanabilirsiniz. Varsayılan, yerel bir beş düğümlü küme oluşturur.
      >

      Hizmet Kumaşındaki DNS Hizmeti hakkında daha fazla bilgi edinmek için [Azure Hizmet Kumaşında DNS Hizmeti'ne](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice)bakın. Bir kapsayıcıda çalışan hizmetlerden Service Fabric ters proxy kullanma hakkında daha fazla bilgi için, [kapsayıcılarda çalışan hizmetler için Ters proxy özel işleme](service-fabric-reverseproxy.md#special-handling-for-services-running-in-containers)bakın.

### <a name="known-limitations-when-debugging-containers-in-service-fabric"></a>Servis Kumaşında konteynerlerin hata ayıklanmasında bilinen sınırlamalar

Aşağıda Hizmet Kumaş ve olası çözünürlüklerde hata ayıklama kapları ile bilinen sınırlamalar listesi:

* ClusterFQDNorIP için localhost kullanmak kapsayıcılarda DNS çözünürlüğünü desteklemez.
    * Çözünürlük: Yerel kümeyi makine adını kullanarak ayarlama (yukarıya bakın)
* Windows10'u Sanal Makine'de çalıştırmak, DNS yanıtını kapsayıcıya geri almaz.
    * Çözünürlük: Virtual Machines NIC'de IPv4 için UDP checksum boşaltmasını devre dışı
    * Windows10'u çalıştırmak makinedeki ağ performansını düşürür.
    * https://github.com/Azure/service-fabric-issues/issues/1061
* Uygulama Docker Compose kullanılarak dağıtıldıysa, DNS hizmet adını kullanarak aynı uygulamadaki hizmetleri çözme Windows10'da çalışmaz
    * Çözüm: Hizmet bitiş noktalarını çözmek için servicename.applicationname'yi kullanın
    * https://github.com/Azure/service-fabric-issues/issues/1062
* ClusterFQDNorIP için IP adresi kullanıyorsanız, ana bilgisayarda birincil IP'yi değiştirmek DNS işlevini bozar.
    * Çözünürlük: Ana bilgisayardaki yeni birincil IP'yi kullanarak kümeyi yeniden oluşturun veya makine adını kullanın. Bu kırılma tasarım gereğidir.
* Kümenin oluşturulduğu FQDN ağda çözülemezse, DNS başarısız olur.
    * Çözünürlük: Ana bilgisayarbirincil IP kullanarak yerel küme yeniden oluşturun. Bu hata tasarım gereğidir.
* Bir konteynerhatasını yaparken, docker günlükleri Yalnızca Service Fabric Explorer da dahil olmak üzere Service Fabric API'ler aracılığıyla değil, Visual Studio çıkış penceresinde kullanılabilir

## <a name="debug-a-net-application-running-in-docker-containers-on-service-fabric"></a>Servis Kumaşı üzerinde docker kaplarında çalışan bir .NET uygulamasını hata ayıklama

1. Visual Studio'yu yönetici olarak çalıştırın.

1. Varolan bir .NET uygulamasını açın veya yeni bir uygulama oluşturun.

1. Projeye sağ tıklayın ve **Ekle -> Konteyner Orchestrator Desteği -> Servis Kumaşı'nı** seçin

1. Uygulamayı hata ayıklamaya başlamak için **F5** tuşuna basın.

    Visual Studio,.NET ve .NET Core için konsol ve ASP.NET proje türlerini destekler.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric ve konteynerlerin özellikleri hakkında daha fazla bilgi edinmek için [Service Fabric konteynerlere genel bakış](service-fabric-containers-overview.md)bölümüne bakın.
