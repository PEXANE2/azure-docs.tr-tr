---
title: Uygulama Öngörüleri ile canlı Azure Bulut Hizmetleri profili | Microsoft Dokümanlar
description: Azure Bulut Hizmetleri için Uygulama Öngörüleri Profiloluştur'u etkinleştirin.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 3fbeb1120e97a884135cd4622a49ef97fd43e58e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671673"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Application Insights ile canlı Azure Bulut Hizmetleri profili

Uygulama Öngörüleri Profil Oluşturucusu'yu bu hizmetlere de dağıtabilirsiniz:
* [Azure Uygulama Hizmeti](profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Hizmet Kumaşı uygulamaları](profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Azure Sanal Makineler](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Uygulama Öngörüleri Profiloluşturc, Azure Tanılama uzantısı ile yüklenir. Profiler'ı yüklemek ve Profilleri Application Insights kaynağınıza göndermek için Azure Diagnostics'i yapılandırmanız gerekir.

## <a name="enable-profiler-for-azure-cloud-services"></a>Azure Bulut Hizmetleri için Profil Oluştur'u etkinleştirme
1. [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) veya daha yeni kullandığınızdan emin olun. Os ailesi 4 kullanıyorsanız, .NET Framework 4.6.1 veya daha yeni bir [başlangıç görevi](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-install-dotnet)yle yüklemeniz gerekir. OS Family 5 varsayılan olarak .NET Framework'ün uyumlu bir sürümünü içerir. 

1. [Azure Bulut Hizmetlerine Uygulama Öngörüleri SDK](../../azure-monitor/app/cloudservices.md?toc=/azure/azure-monitor/toc.json)ekleyin.

    **Bulut Hizmetleri için WAD'da bulunan profil oluşturucudaki hata düzeltildi.** Bulut Hizmetleri için WAD'ın (1.12.2.0) en son sürümü, App Insights SDK'nın tüm son sürümleriyle çalışır. Bulut Hizmeti ana bilgisayarları WAD'ı otomatik olarak yükseltir, ancak hemen yükseltmeyapmaz. Yükseltmeyi zorlamak için, hizmetinizi yeniden dağıtabilir veya düğümü yeniden başlatabilirsiniz.

1. Uygulama Öngörüleri ile istekleri izleyin:

    * ASP.NET web rolleri için Application Insights istekleri otomatik olarak izleyebilir.

    * Alt roller [için, istekleri izlemek için kod ekleyin.](profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)

1. Profiler'ı etkinleştirmek için Azure Tanılama uzantısını yapılandırın:

    a. Uygulama rolünüz için [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) *diagnostics.wadcfgx* dosyasını burada gösterildiği gibi bulun:  

      ![Tanılama config dosyasının konumu](./media/profiler-cloudservice/cloudservice-solutionexplorer.png)  

      Dosyayı bulamıyorsanız, Azure [Bulut Hizmetleri ve Sanal Makineler için tanılama](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)ayarla'ya bakın.

    b. Aşağıdaki `SinksConfig` bölümü bir alt öğe `WadCfg`olarak ekleyin:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    > [!NOTE]
    > *Diagnostics.wadcfgx* dosyası da applicationinsights türünden başka bir lavabo içeriyorsa, aşağıdaki enstrümantasyon tuşlarının üçü de eşleşmelidir:  
    > * Uygulamanız tarafından kullanılan anahtar. 
    > * ApplicationInsights tarafından kullanılan anahtar batar. 
    > * ApplicationInsightsProfiler lavabosu tarafından kullanılan anahtar. 
    >
    > `ApplicationInsights` ServiceConfiguration'da lavabo tarafından kullanılan gerçek enstrümantasyon anahtar değerini *bulabilirsiniz.\* cscfg* dosyaları. 
    > Visual Studio 15.5 Azure SDK sürümünden sonra, yalnızca uygulama tarafından kullanılan enstrümantasyon tuşları ve ApplicationInsightsProfiler lavabosu birbiriyle eşleşmeli.

1. Yeni Tanılama yapılandırmasıyla hizmetinizi dağıtın ve Application Insights Profiler hizmetinizde çalışacak şekilde yapılandırılır.
 
## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanıza trafik oluşturun (örneğin, [kullanılabilirlik testi](monitor-web-app-availability.md)başlatın). Ardından, izlemelerin Uygulama Öngörüleri örneğine gönderilmeye başlaması için 10 ila 15 dakika bekleyin.
* Azure portalındaki [Profiler izlemelerine](profiler-overview.md?toc=/azure/azure-monitor/toc.json) bakın.
* Profiler sorunlarını gidermek için [Profiler sorun giderme konusuna](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)bakın.
