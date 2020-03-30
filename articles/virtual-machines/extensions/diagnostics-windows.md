---
title: Windows VM'de tanılamayı etkinleştirmek için Azure PowerShell'i kullanın
services: virtual-machines-windows
documentationcenter: ''
description: Windows çalıştıran sanal bir makinede Azure Tanılama'yı etkinleştirmek için PowerShell'i nasıl kullanacağınızı öğrenin
author: sbtron
manager: gwallace
editor: ''
ms.assetid: 2e6d88f2-1980-4a24-827e-a81616a0d247
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 12/15/2015
ms.author: saurabh
ms.openlocfilehash: 55afeb52323ead7db8be7e8fd1dabc880328e888
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921547"
---
# <a name="use-powershell-to-enable-azure-diagnostics-in-a-virtual-machine-running-windows"></a>Windows çalıştıran sanal bir makinede Azure Tanılama'yı etkinleştirmek için PowerShell'i kullanın

Azure Tanılama, Azure'da dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlayan bir özelliktir. Windows çalıştıran bir Azure sanal makineden (VM) uygulama günlükleri veya performans sayaçları gibi tanılama verilerini toplamak için tanılama uzantısını kullanabilirsiniz. 

 

## <a name="enable-the-diagnostics-extension-if-you-use-the-resource-manager-deployment-model"></a>Kaynak Yöneticisi dağıtım modelini kullanıyorsanız tanılama uzantısını etkinleştirin
Uzantı yapılandırmasını Kaynak Yöneticisi şablonuna ekleyerek Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla bir Windows VM oluştururken tanılama uzantısını etkinleştirebilirsiniz. Bkz. [Azure Kaynak Yöneticisi şablonu kullanarak izleme ve tanılama içeren bir Windows sanal makinesi oluşturun.](diagnostics-template.md)

Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan varolan bir VM'deki tanılama uzantısını etkinleştirmek için, aşağıda gösterildiği gibi [Set-AzVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiagnosticsextension) PowerShell cmdlet'i kullanabilirsiniz.

    $vm_resourcegroup = "myvmresourcegroup"
    $vm_name = "myvm"
    $diagnosticsconfig_path = "DiagnosticsPubConfig.xml"

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path


*$diagnosticsconfig_path,* aşağıdaki [örnekte](#sample-diagnostics-configuration) açıklandığı gibi XML'deki tanılama yapılandırmasını içeren dosyaya giden yoldur.  

Tanılama yapılandırma dosyasında depolama hesabı adı olan bir **StorageAccount** öğesi belirtilirse, *Set-AzVMDiagnosticsExtension* komut dosyası tanılama uzantısını otomatik olarak ayarlayıp tanılama uzantısını bu depolama hesabına gönderir. Bunun işe yaraması için depolama hesabının VM ile aynı abonelikte olması gerekir.

Tanılama yapılandırmasında **StorageAccount** belirtilmediyse, *StorageAccountName* parametresini cmdlet'e geçirmeniz gerekir. *StorageAccountName* parametresi belirtilirse, cmdlet her zaman tanılama yapılandırma dosyasında belirtilen değil, parametrede belirtilen depolama hesabını kullanır.

Tanılama depolama hesabı VM'den farklı bir abonelikteyse, *StorageAccountName* ve *StorageAccountKey* parametrelerini açıkça cmdlet'e aktarmanız gerekir. Tanılama depolama hesabı aynı abonelikte yken *StorageAccountKey* parametresi gerekli değildir, çünkü cmdlet tanılama uzantısını etkinleştirirken anahtar değerini otomatik olarak sorgulayabilir ve ayarlayabilir. Ancak, tanılama depolama hesabı farklı bir abonelikteyse, cmdlet anahtarı otomatik olarak alamayabilir ve *StorageAccountKey* parametresi aracılığıyla anahtarı açıkça belirtmeniz gerekir.  

    Set-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key

Tanılama uzantısı VM'de etkinleştirildikten sonra [Get-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiagnosticsextension) cmdlet'i kullanarak geçerli ayarları alabilirsiniz.

    Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name

Cmdlet, tanılama yapılandırmasını içeren *PublicSettings'i*döndürür. WadCfg ve xmlCfg olmak üzere desteklenen iki tür yapılandırma vardır. WadCfg JSON yapılandırmave xmlCfg Base64 kodlanmış biçiminde XML yapılandırma. XML okumak için, bunu çözmek gerekir.

    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName $vm_resourcegroup -VMName $vm_name).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

[Remove-AzVmDiagnosticsExtension](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdiagnosticsextension) cmdlet, tanıuzantısını VM'den çıkarmak için kullanılabilir.  

## <a name="enable-the-diagnostics-extension-if-you-use-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanıyorsanız tanılama uzantısını etkinleştirin

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modeli aracılığıyla oluşturduğunuz bir VM'de tanılama uzantısını etkinleştirmek için [Set-AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet'i kullanabilirsiniz. Aşağıdaki örnek, tanılama uzantısı etkin leştirilmiş klasik dağıtım modeli aracılığıyla yeni bir VM'nin nasıl oluşturulturolduğunu gösterir.

    $VM = New-AzureVMConfig -Name $VM -InstanceSize Small -ImageName $VMImage
    $VM = Add-AzureProvisioningConfig -VM $VM -AdminUsername $Username -Password $Password -Windows
    $VM = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    New-AzVM -Location $Location -ServiceName $Service_Name -VM $VM

Klasik dağıtım modeli aracılığıyla oluşturulan varolan bir VM'deki tanılama uzantısını etkinleştirmek için önce VM yapılandırmasını almak için [Get-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azurevm) cmdlet'ini kullanın. Ardından, [Set-AzureVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) cmdlet'i kullanarak tanılama uzantısını içerecek şekilde VM yapılandırmasını güncelleştirin. Son olarak, Güncelleştirilmiş yapılandırmayı [Update-AzureVM](https://docs.microsoft.com/powershell/module/servicemanagement/azure/update-azurevm)kullanarak VM'ye uygulayın.

    $VM = Get-AzureVM -ServiceName $Service_Name -Name $VM_Name
    $VM_Update = Set-AzureVMDiagnosticsExtension  -DiagnosticsConfigurationPath $Config_Path -VM $VM -StorageContext $Storage_Context
    Update-AzureVM -ServiceName $Service_Name -Name $VM_Name -VM $VM_Update.VM

## <a name="sample-diagnostics-configuration"></a>Örnek tanılama yapılandırması
Aşağıdaki XML, yukarıdaki komut dosyalarıyla birlikte tanılama ortak yapılandırması için kullanılabilir. Bu örnek yapılandırma, windows olay günlüklerinde uygulama, güvenlik ve sistem kanallarından gelen hatalar ve tanılama altyapısı günlüklerinden gelen hatalarla birlikte çeşitli performans sayaçlarını tanılama depolama hesabına aktaracaktır.

Yapılandırmanın aşağıdakileri içerecek şekilde güncelleştirilmesi gerekir:

* **Ölçümler** öğesinin *kaynak kimliği* özniteliğinin VM için kaynak kimliğiyle güncelleştirilmesi gerekir.
  
  * Kaynak kimliği aşağıdaki desen kullanılarak oluşturulabilir: "/subscriptions/{*VM*}/resourceGroups/{*VM*}/providers/Microsoft.Compute/virtualMachines/{*VM Adı*}" ile abonelik için abonelik kimliği.
  * Örneğin, VM'nin çalıştığı abonelik için abonelik kimliği **11111111-1111-1111-1111111111111111111111ise,** kaynak grubunun kaynak grubu adı **MyResourceGroup'sa**ve VM adı **MyWindowsVM** *ise, resourceID* değeri olacaktır:
    
      ```xml
      <Metrics resourceId="/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyWindowsVM" >
      ```
  * Performans sayaçları ve ölçümler yapılandırmasına göre ölçümlerin nasıl oluşturulduğu hakkında daha fazla bilgi [için, depolama alanında Azure Tanılama ölçümleri tablosuna](diagnostics-template.md#wadmetrics-tables-in-storage)bakın.
* **StorageAccount** öğesinin tanılama depolama hesabının adı ile güncelleştirilmesi gerekir.
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
          <DiagnosticMonitorConfiguration overallQuotaInMB="4096">
            <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error"/>
            <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU utilization" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Privileged Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU privileged time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% User Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="CPU user time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Processor Information(_Total)\Processor Frequency" sampleRate="PT15S" unit="Count">
            <annotation displayName="CPU frequency" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\System\Processes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Processes" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Thread Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Threads" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Process(_Total)\Handle Count" sampleRate="PT15S" unit="Count">
            <annotation displayName="Handles" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\% Committed Bytes In Use" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Memory usage" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory available" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory committed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Commit Limit" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory commit limit" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Paged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Pool Nonpaged Bytes" sampleRate="PT15S" unit="Bytes">
            <annotation displayName="Memory non-paged pool" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Read Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active read time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\% Disk Write Time" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk active write time" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Transfers/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Reads/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk read operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Writes/sec" sampleRate="PT15S" unit="CountPerSecond">
            <annotation displayName="Disk write operations" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Read Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk read speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Disk Write Bytes/sec" sampleRate="PT15S" unit="BytesPerSecond">
            <annotation displayName="Disk write speed" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Read Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average read queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\PhysicalDisk(_Total)\Avg. Disk Write Queue Length" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk average write queue length" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\% Free Space" sampleRate="PT15S" unit="Percent">
            <annotation displayName="Disk free space (percentage)" locale="en-us"/>
          </PerformanceCounterConfiguration>
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(_Total)\Free Megabytes" sampleRate="PT15S" unit="Count">
            <annotation displayName="Disk free space (MB)" locale="en-us"/>
          </PerformanceCounterConfiguration>
        </PerformanceCounters>
        <Metrics resourceId="(Update with resource ID for the VM)" >
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*[System[(Level = 1 or Level = 2)]]"/>
          <DataSource name="Security!*[System[(Level = 1 or Level = 2)]"/>
          <DataSource name="System!*[System[(Level = 1 or Level = 2)]]"/>
        </WindowsEventLog>
          </DiagnosticMonitorConfiguration>
        </WadCfg>
        <StorageAccount>(Update with diagnostics storage account name)</StorageAccount>
    </PublicConfig>
    ```

## <a name="next-steps"></a>Sonraki adımlar
* Sorunları gidermek için Azure Tanılama özelliğini ve diğer teknikleri kullanma hakkında ek kılavuz [için](../../cloud-services/cloud-services-dotnet-diagnostics.md)bkz.
* [Tanılama yapılandırmaları şema](https://msdn.microsoft.com/library/azure/mt634524.aspx) tanılama uzantısı için çeşitli XML yapılandırmaları seçenekleri açıklar.

