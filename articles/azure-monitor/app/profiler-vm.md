---
title: Azure VM'deki profil web uygulamaları - Application Insights Profiler
description: Application Insights Profiler'ı kullanarak Azure VM'deki profil web uygulamaları.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 11/08/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 7c5dfe6ed08df01f78346c76fd5a35e7d64ab520
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671588"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Bir Azure sanal makinesinde çalışan profil web uygulamaları veya Application Insights Profiler kullanarak ayarlanan sanal makine ölçeği

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Uygulama Öngörüleri Profil Oluşturucusu'yu bu hizmetlere de dağıtabilirsiniz:
* [Azure Uygulama Hizmeti](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Profiler'ı sanal makinede veya sanal makine ölçeği kümesine dağıtma
Bu makalede, Azure sanal makinenizde (VM) veya Azure sanal makine ölçeği setinizde Uygulama Öngörüleri Profil oluşturucusu nasıl çalıştırılabilirsiniz. Profiler, VM'ler için Azure Tanılama uzantısı ile yüklenir. Uzantıyı Profiler'ı çalıştırmak için yapılandırın ve Uygulama Öngörüleri SDK'sını uygulamanız içine oluşturun.

1. Uygulama Öngörüleri SDK'yı [ASP.NET uygulamanıza](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net)ekleyin.

   İsteklerinizin profillerini görüntülemek için, Uygulama Öngörüleri'ne istek telemetrisi göndermeniz gerekir.

1. Azure Tanılama uzantısını VM'nizde yükleyin. Tam Kaynak Yöneticisi şablonu örnekleri için bkz:  
   * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
   * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
     Önemli kısmı WadCfg'deki ApplicationInsightsProfilerSink'tir. Azure Tanılama'nın Profiler'ın iKey'inize veri göndermesini sağlaması için bu bölüme bir lavabo daha ekleyin.
    
     ```json
     "SinksConfig": {
       "Sink": [
         {
           "name": "ApplicationInsightsSink",
           "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
         },
         {
           "name": "MyApplicationInsightsProfilerSink",
           "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
         }
       ]
     },
     ```

1. Değiştirilmiş ortam dağıtım tanımını dağıtın.  

   Değişikliklerin uygulanması genellikle powershell cmdlets veya Visual Studio aracılığıyla tam bir şablon dağıtımı veya bulut hizmeti tabanlı yayımlama içerir.  

   Aşağıdaki PowerShell komutları, yalnızca Azure Tanılama uzantısına dokunan varolan sanal makineler için alternatif bir yaklaşımdır. Get-AzVMDiagnosticsExtension komutu tarafından döndürülen config'e daha önce bahsedilen ProfilerSink'i ekleyin. Ardından güncelleştirilmiş config'i Set-AzVMDiagnosticsExtension komutuna geçirin.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Amaçlanan uygulama [IIS](https://www.microsoft.com/web/downloads/platform.aspx)üzerinden çalışıyorsa, Windows özelliğini etkinleştirin. `IIS Http Tracing`

   a. Ortama uzaktan erişim kurun ve ardından [Windows özellik ekle]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) penceresini kullanın. Veya PowerShell'de (yönetici olarak) aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim kurmak bir sorunsa, aşağıdaki komutu çalıştırmak için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanabilirsiniz:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Uygulamanızı dağıtın.

## <a name="set-profiler-sink-using-azure-resource-explorer"></a>Azure Kaynak Gezgini'ni kullanarak Profiler Lavabosu'nı ayarlama
Uygulama Öngörüleri Profil oluşturucusu portalından ayarlamanın henüz bir yolu yok. Yukarıda açıklandığı gibi powershell kullanmak yerine, lavaboyu ayarlamak için Azure Kaynak Gezgini'ni kullanabilirsiniz. Ancak, VM'yi tekrar dağıtırsanız lavabo kaybolur. Bu ayarı korumak için VM'yi dağıtırken kullandığınız config'i güncelleştirmeniz gerekir.

1. Sanal makineniz için yüklenen uzantıları görüntüleyerek Windows Azure Tanılama uzantısının yüklü olup olmadığını denetleyin.  

    ![WAD uzantısı yüklü olup olmadığını denetleyin][wadextension]

2. VM'niz için VM Diagnostics uzantısını bulun. [https://resources.azure.com](https://resources.azure.com)Git. Kaynak grubunuzu, Microsoft.Compute virtualMachines'i, sanal makine adını ve uzantılarını genişletin.  

    ![Azure Kaynak Gezgini'nde WAD config'ine gidin][azureresourceexplorer]

3. WadCfg altında SinksConfig düğüm uygulama insights Profiler lavabo ekleyin. Zaten bir SinksConfig bölümünüz yoksa, bir tane eklemeniz gerekebilir. Ayarlarınızda uygun Uygulama Öngörüleri iKey'i belirttiğinizden emin olun. Sağ üst köşede Okuma/Yazma ve mavi 'Edit' düğmesine basmak için kaşifler modunu değiştirmeniz gerekir.

    ![Uygulama Öngörüleri Profiloluşturucu Lavabo Ekle][resourceexplorersinksconfig]

4. Config'i düzenlemeyi bitirdiğinizde 'Put' tuşuna basın. Put başarılı olursa, ekranın ortasında yeşil bir denetim görüntülenir.

    ![Değişiklikleri uygulamak için put isteği gönderme][resourceexplorerput]






## <a name="can-profiler-run-on-on-premises-servers"></a>Profiler şirket içi sunucularda çalıştırılabilir mi?
Şirket içi sunucular için Application Insights Profiler'ı destekleme planımız yoktur.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanıza trafik oluşturun (örneğin, [kullanılabilirlik testi](monitor-web-app-availability.md)başlatın). Ardından, izlemelerin Uygulama Öngörüleri örneğine gönderilmeye başlaması için 10 ila 15 dakika bekleyin.
- Azure portalındaki [Profiler izlemelerine](profiler-overview.md?toc=/azure/azure-monitor/toc.json) bakın.
- Sorun giderme Profiler sorunlarıyla ilgili yardım için [Profiler sorun giderme](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)konusuna bakın.

[azureresourceexplorer]: ./media/profiler-vm/azure-resource-explorer.png
[resourceexplorerput]: ./media/profiler-vm/resource-explorer-put.png
[resourceexplorersinksconfig]: ./media/profiler-vm/resource-explorer-sinks-config.png
[wadextension]: ./media/profiler-vm/wad-extension.png
