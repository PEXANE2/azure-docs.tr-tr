---
title: PowerShell | Microsoft Dokümanlar
description: PowerShell'i kullanarak bulut hizmetleri için tanılamayı nasıl etkinleştirin
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 09/06/2016
ms.author: tagore
ms.openlocfilehash: 76cdffed813fd182980b36f848e0ae42f3226539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386553"
---
# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>PowerShell'i kullanarak Azure Bulut Hizmetleri'nde tanılamayı etkinleştirme
Azure Tanılama uzantısını kullanarak bir Bulut Hizmetinden uygulama günlükleri, performans sayaçları vb. gibi tanılama verilerini toplayabilirsiniz. Bu makalede, PowerShell'i kullanarak bulut hizmeti için Azure Tanılama uzantısınasıl etkinleştirilir açıklanmaktadır.  Bu makale için gereken ön koşullar için [Azure PowerShell'i nasıl yükleyip yapılandırılabildiğini](/powershell/azure/overview) öğrenin.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Bulut Hizmeti dağıtımının bir parçası olarak tanılama uzantısını etkinleştirme
Bu yaklaşım, tanılama uzantısı bulut hizmeti dağıtmanın bir parçası olarak etkinleştirilebilir senaryoların sürekli tümleştirme türü için geçerlidir. Yeni bir Bulut Hizmeti dağıtımı oluştururken, *Uzantı Yapılandırma* parametresini [Yeni-AzureDeployment](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-3.7.0) cmdlet'ine geçirerek tanılama uzantısını etkinleştirebilirsiniz. *ExtensionConfiguration* parametresi, [Yeni AzureServiceDiagnosticsExtensionConfig](/powershell/module/servicemanagement/azure/new-azureservicediagnosticsextensionconfig?view=azuresmps-3.7.0) cmdlet kullanılarak oluşturulabilen bir dizi tanılama yapılandırması alır.

Aşağıdaki örnek, her biri farklı bir tanılama yapılandırmasına sahip bir WebRole ve WorkerRole içeren bir bulut hizmeti için tanılamayı nasıl etkinleştirebileceğinizi gösterir.

```powershell
$service_name = "MyService"
$service_package = "CloudService.cspkg"
$service_config = "ServiceConfiguration.Cloud.cscfg"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)
```

Tanılama yapılandırma dosyasında depolama `StorageAccount` hesabı adı olan bir öğe `New-AzureServiceDiagnosticsExtensionConfig` belirtilirse, cmdlet bu depolama hesabını otomatik olarak kullanır. Bunun işe yaraması için depolama hesabının, dağıtılan Bulut Hizmeti yle aynı abonelikte olması gerekir.

Azure SDK 2.6'dan itibaren MSBuild yayımlama hedef çıktısı tarafından oluşturulan uzantı yapılandırma dosyaları, hizmet yapılandırma dosyasında (.cscfg) belirtilen tanılama yapılandırma dizesini temel alan depolama hesabı adını içerir. Aşağıdaki komut dosyası, uzantı yapılandırma dosyalarını yayımlama hedef çıktısından nasıl ayrıştırabileceğinizi ve bulut hizmetini dağıtırken her rol için tanılama uzantısını nasıl yapılandırabileceğinizi gösterir.

```powershell
$service_name = "MyService"
$service_package = "C:\build\output\CloudService.cspkg"
$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

#Find the Extensions path based on service configuration file
$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
$diagnosticsConfigurations = @()
foreach ($extPath in $diagnosticsExtensions)
{
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
    {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
    }
}
New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations
```

Visual Studio Online, tanılama uzantısı ile Bulut Hizmetlerinin otomatik dağıtımı için benzer bir yaklaşım kullanır. Tam bir örnek için [Publish-AzureCloudDeployment.ps1'e](https://github.com/Microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureCloudPowerShellDeploymentV1/Publish-AzureCloudDeployment.ps1) bakın.

Tanılama `StorageAccount` yapılandırmasında hiçbir belirtilmediyse, *StorageAccountName* parametresini cmdlet'e geçirmeniz gerekir. *StorageAccountName* parametresi belirtilirse, cmdlet her zaman tanılama yapılandırma dosyasında belirtilen değil, parametrede belirtilen depolama hesabını kullanır.

Tanılama depolama hesabı Bulut Hizmeti'nden farklı bir abonelikteyse, *StorageAccountName* ve *StorageAccountKey* parametrelerini açıkça cmdlet'e aktarmanız gerekir. Tanılama depolama hesabı aynı abonelikte yken *StorageAccountKey* parametresi gerekli değildir, çünkü cmdlet tanılama uzantısını etkinleştirirken anahtar değerini otomatik olarak sorgulayabilir ve ayarlayabilir. Ancak, tanılama depolama hesabı farklı bir abonelikteyse, cmdlet anahtarı otomatik olarak alamayabilir ve *StorageAccountKey* parametresi aracılığıyla anahtarı açıkça belirtmeniz gerekir.

```powershell
$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
```

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Mevcut bir Bulut Hizmetinde tanılama uzantısını etkinleştirme
Zaten çalışmakta olan bir Bulut Hizmetinde tanılama yapılandırmasını etkinleştirmek veya güncelleştirmek için [Set-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/set-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'i kullanabilirsiniz.

[!INCLUDE [cloud-services-wad-warning](../../includes/cloud-services-wad-warning.md)]

```powershell
$service_name = "MyService"
$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name
```

## <a name="get-current-diagnostics-extension-configuration"></a>Güncel tanılama uzantı yapılandırmasını alma
Bir bulut hizmeti için geçerli tanılama yapılandırmasını almak için [Get-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/get-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'ini kullanın.

```powershell
Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

## <a name="remove-diagnostics-extension"></a>Tanılama uzantısını kaldırma
Bir bulut hizmetindeki tanılamayı kapatmak için [Kaldır-AzureServiceDiagnosticsExtension](/powershell/module/servicemanagement/azure/remove-azureservicediagnosticsextension?view=azuresmps-3.7.0) cmdlet'ini kullanabilirsiniz.

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

*Rol* parametresi olmadan *Set-AzureServiceDiagnosticsExtension* veya *New-AzureServiceDiagnosticsExtensionConfig'i* kullanarak tanılama uzantısını etkinleştirdiyseniz, *Rolü* parametresi olmadan *Remove-AzureServiceDiagnosticsExtension'ı* kullanarak uzantıyı kaldırabilirsiniz. Uzantıyı etkinleştirirken *Rol* parametresi kullanıldıysa, uzantıyı kaldırırken de kullanılmalıdır.

Tanılama uzantısını her bir rolden kaldırmak için:

```powershell
Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```

## <a name="next-steps"></a>Sonraki Adımlar
* Sorunları gidermek için Azure tanılama ve diğer teknikleri kullanma hakkında ek kılavuz [için](cloud-services-dotnet-diagnostics.md)bkz.
* [Tanılama Yapılandırma Şeması](/azure/azure-monitor/platform/diagnostics-extension-schema-1dot3) tanılama uzantısı için çeşitli xml yapılandırmaları seçeneklerini açıklar.
* Sanal Makineler için tanılama uzantısını nasıl etkinleştireceklerini öğrenmek için Azure [Kaynak Yöneticisi Şablonu'nu kullanarak izleme ve tanılama içeren bir Windows Sanal makine oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md)



