---
title: Azure VM 'de Web Apps profili oluşturma-Application Insights Profiler
description: Application Insights Profiler kullanarak bir Azure VM 'de Web Apps profili oluşturma.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 11/08/2019
ms.reviewer: mbullwin
ms.openlocfilehash: f514dd7b54ac091535aeab43a8a7d2a645b50a09
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87315844"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Application Insights Profiler kullanarak bir Azure sanal makinesinde veya bir sanal makine ölçek kümesinde çalışan Web uygulamaları profili oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Ayrıca, bu hizmetlerde Azure Application Insights Profiler dağıtabilirsiniz:
* [Azure App Service](./profiler.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](?toc=%2fazure%2fazure-monitor%2ftoc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Bir sanal makine veya sanal makine ölçek kümesi üzerinde profil oluşturucu dağıtma
Bu makalede, Azure sanal makinenizde (VM) veya Azure sanal makine ölçek kümesinde çalışan Application Insights Profiler nasıl alınacağı gösterilmektedir. Profiler, VM 'Ler için Azure Tanılama Uzantısı ile yüklenir. Uzantıyı profil oluşturucu çalıştıracak şekilde yapılandırın ve uygulamanızda Application Insights SDK 'Yı derleyin.

1. Application Insights SDK 'sını [ASP.net uygulamanıza](./asp-net.md)ekleyin.

   İsteklerinizin profillerini görüntülemek için Application Insights istek Telemetriyi göndermeniz gerekir.

1. Azure Tanılama uzantısı 'nı sanal makinenize yükler. Tam Kaynak Yöneticisi şablon örnekleri için bkz.:  
   * [Sanal makine](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
   * [Sanal makine ölçek kümesi](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
     Anahtar bölümü WadCfg içindeki Applicationınsightsprofilersink ' dir. Ikey 'e veri göndermek için profil oluşturucuyu etkinleştirmek Azure Tanılama için bu bölüme başka bir havuz ekleyin.
    
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

1. Değiştirilen ortam dağıtım tanımını dağıtın.  

   Değişikliklerin uygulanması genellikle tam şablon dağıtımı veya PowerShell cmdlet 'leri ya da Visual Studio aracılığıyla bulut hizmeti tabanlı yayımlama içerir.  

   Aşağıdaki PowerShell komutları, yalnızca Azure Tanılama uzantısına dokunarak var olan sanal makinelere yönelik alternatif bir yaklaşımdır. Daha önce bahsedilen ProfilerSink öğesini Get-Azvmdiagnosticsextenma komutu tarafından döndürülen yapılandırmaya ekleyin. Ardından, güncelleştirilmiş yapılandırmayı set-Azvmdiagnosticsextenma komutuna geçirin.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Hedeflenen uygulama [IIS](https://www.microsoft.com/web/downloads/platform.aspx)aracılığıyla çalışıyorsa, `IIS Http Tracing` Windows özelliğini etkinleştirin.

   a. Ortama uzaktan erişim oluşturun ve ardından [Windows Özellikleri Ekle](/iis/configuration/system.webserver/tracing/) penceresini kullanın. Veya PowerShell (yönetici olarak) içinde aşağıdaki komutu çalıştırın:  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Uzaktan erişim için bir sorun varsa, aşağıdaki komutu çalıştırmak için [Azure CLI](/cli/azure/get-started-with-azure-cli) 'yi kullanabilirsiniz:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Uygulamanızı dağıtın.

## <a name="set-profiler-sink-using-azure-resource-explorer"></a>Profil Oluşturucu havuzunu Azure Kaynak Gezgini kullanarak ayarlama
Application Insights Profiler havuzunu portaldan ayarlamak için henüz bir yol yoktur. PowerShell 'i yukarıda açıklanan şekilde kullanmak yerine, havuzu ayarlamak için Azure Kaynak Gezgini kullanabilirsiniz. Ancak, VM 'yi yeniden dağıtırsanız havuzun kaybedildiğini aklınızda olursunuz. Bu ayarı korumak için VM dağıtımı yaparken kullandığınız yapılandırmayı güncelleştirmeniz gerekir.

1. Sanal makineniz için yüklü uzantıları görüntüleyerek Windows Azure Tanılama uzantısının yüklü olduğundan emin olun.  

    ![WAD uzantısının yüklenip yüklenmediğini denetle][wadextension]

2. VM 'niz için VM tanılama uzantısını bulun. Adresine gidin [https://resources.azure.com](https://resources.azure.com) . Kaynak grubunuzu, Microsoft. COMPUTE virtualMachines, sanal makine adı ve uzantıları ' nı genişletin.  

    ![Azure Kaynak Gezgini 'de WAD config 'e gidin][azureresourceexplorer]

3. WadCfg altındaki SinksConfig düğümüne Application Insights Profiler havuzunu ekleyin. Önceden bir SinksConfig bölümü yoksa, bir tane eklemeniz gerekebilir. Ayarlarınızda uygun Application Insights iKey değerini belirttiğinizden emin olun. Araştırıcılar modunu sağ üst köşeye okumak/yazmak ve mavi ' Düzenle ' düğmesine basmanız gerekir.

    ![Application Insights Profiler Havuzu Ekle][resourceexplorersinksconfig]

4. Yapılandırmayı düzenledikten sonra ' Put ' düğmesine basın. Put başarılı olursa, ekranın ortasında yeşil bir onay işareti görünür.

    ![Değişiklikleri uygulamak için PUT isteği gönder][resourceexplorerput]






## <a name="can-profiler-run-on-on-premises-servers"></a>Şirket içi sunucularda profil oluşturucu çalıştırılabilir mi?
Şirket içi sunucular için Application Insights Profiler desteklemeyi planlıyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulamanıza trafik oluşturun (örneğin, bir [Kullanılabilirlik testi](monitor-web-app-availability.md)başlatın). Ardından, izlemelerin Application Insights örneğine gönderilmesi için 10 ila 15 dakika bekleyin.
- Azure portal [Profil Oluşturucu izlemeleri](profiler-overview.md?toc=/azure/azure-monitor/toc.json) bölümüne bakın.
- Profil Oluşturucu sorunlarını gidermeye yönelik yardım için bkz. [Profil Oluşturucu sorun giderme](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

[azureresourceexplorer]: ./media/profiler-vm/azure-resource-explorer.png
[resourceexplorerput]: ./media/profiler-vm/resource-explorer-put.png
[resourceexplorersinksconfig]: ./media/profiler-vm/resource-explorer-sinks-config.png
[wadextension]: ./media/profiler-vm/wad-extension.png

