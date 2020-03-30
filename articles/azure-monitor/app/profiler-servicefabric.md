---
title: Application Insights ile profil canlı Azure Service Fabric uygulamaları
description: Service Fabric uygulaması için Profiler'ı etkinleştirin
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 664d6eb377185613a1a5670daf6747b482c79d9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671622"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Application Insights ile profil canlı Azure Hizmet Kumaş uygulamaları

Uygulama Öngörüleri Profil Oluşturucusu'yu bu hizmetlere de dağıtabilirsiniz:
* [Azure Uygulama Hizmeti](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="set-up-the-environment-deployment-definition"></a>Ortam dağıtım tanımını ayarlama

Uygulama Öngörüleri Profiloluştur, Azure Tanılama'ya dahildir. Hizmet Kumaşı kümeniz için Bir Azure Kaynak Yöneticisi şablonu kullanarak Azure Tanılama uzantısını yükleyebilirsiniz. Hizmet [Kumaş Kümesine Azure Tanılama'yı yükleyen bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)alın.

Ortamınızı ayarlamak için aşağıdaki işlemleri yapmayın:

1. Profiler .NET Framework ve .Net Core'u destekler. .NET Framework [kullanıyorsanız,.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya sonraki lerini kullandığınızdan emin olun. Dağıtılan işletim sistemi veya daha sonra `Windows Server 2012 R2` olduğunu doğrulamak için yeterlidir. Profiler .NET Core 2.1 ve yeni uygulamaları destekler.

1. Dağıtım şablonu dosyasında [Azure Tanılama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) uzantısını arayın.

1. Aşağıdaki `SinksConfig` bölümü bir alt öğe `WadCfg`olarak ekleyin. `ApplicationInsightsProfiler` Özellik değerini kendi Application Insights enstrümantasyon anahtarınızla değiştirin:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Dağıtım şablonunuza Tanılama uzantısı ekleme hakkında bilgi [için](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bkz.

1. Azure Kaynak Yöneticisi şablonunuzu kullanarak Hizmet Dokusu kümenizi dağıtın.  
  Ayarlarınız doğruysa, Azure Tanılama uzantısı yüklendiğinde Application Insights Profiler yüklenir ve etkinleştirilir. 

1. Service Fabric uygulamanıza Uygulama Öngörüleri ekleyin.  
  Profiler'ın istekleriniz için profil toplaması için uygulamanızın Uygulama Öngörüleri ile işlemleri izlemesi gerekir. Durum disu'suz API'ler [için profil oluşturma isteklerini izleme yönergelerine](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)başvurabilirsiniz. Diğer uygulama türlerinde özel işlemleri izleme hakkında daha fazla bilgi için [Uygulama Öngörüleri .NET SDK ile özel işlemleri izleyin.](custom-operations-tracking.md?toc=/azure/azure-monitor/toc.json)

1. Başvurunuzu yeniden dağıtın.


## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanıza trafik oluşturun (örneğin, [kullanılabilirlik testi](monitor-web-app-availability.md)başlatın). Ardından, izlemelerin Uygulama Öngörüleri örneğine gönderilmeye başlaması için 10 ila 15 dakika bekleyin.
* Azure portalındaki [Profiler izlemelerine](profiler-overview.md?toc=/azure/azure-monitor/toc.json) bakın.
* Sorun giderme Profiler sorunlarıyla ilgili yardım için [Profiler sorun giderme](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)konusuna bakın.
