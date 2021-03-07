---
title: Azure Stack Edge Pro cihazınızda VM 'Ler için özel Betik uzantıları kullanımı
description: Şablonlar kullanılarak Azure Stack Edge Pro cihazında çalışan sanal makinelere (VM) özel betik uzantılarının nasıl yükleneceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 2d2e7d403ab3e9cc7e8e17de53b6e821ec24caa1
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102438021"
---
# <a name="deploy-custom-script-extension-on-vms-running-on-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınızda çalışan VM 'lerde özel Betik uzantısı dağıtma

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Özel Betik uzantısı, Azure Stack Edge Pro cihazlarınızda çalışan sanal makinelerde betikleri veya komutları indirir ve çalıştırır. Bu makalede, Azure Resource Manager şablonu kullanarak özel betik uzantısının nasıl yükleneceği ve çalıştırılacağı açıklanır. 

Bu makale Azure Stack Edge Pro GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazları için geçerlidir.

## <a name="about-custom-script-extension"></a>Özel Betik uzantısı hakkında

Özel Betik uzantısı, dağıtım sonrası yapılandırma, yazılım yükleme veya başka bir yapılandırma/yönetim görevi için kullanışlıdır. Azure depolama 'dan veya başka bir erişilebilir internet konumundan betikler indirebilir veya uzantı çalışma zamanına betikler veya komutlar verebilirsiniz.

Özel Betik uzantısı Azure Resource Manager şablonlarıyla tümleştirilir. Azure CLı, PowerShell veya Azure sanal makinelerini REST API kullanarak da çalıştırabilirsiniz.

## <a name="os-for-custom-script-extension"></a>Özel Betik uzantısı için işletim sistemi

#### <a name="supported-os-for-custom-script-extension-on-windows"></a>Windows 'da özel Betik uzantısı için desteklenen işletim sistemi

Windows için özel Betik uzantısı aşağıdaki OSs 'de çalışacaktır. Diğer sürümler çalışabilir, ancak Azure Stack Edge Pro cihazlarında çalışan VM 'lerde şirket içinde sınanmamıştır.

| Dağıtım | Sürüm |
|---|---|
| Windows Server 2019 | Çekirdek |
| Windows Server 2016 | Çekirdek |

#### <a name="supported-os-for-custom-script-extension-on-linux"></a>Linux üzerinde özel Betik uzantısı için desteklenen işletim sistemi

Linux için özel Betik uzantısı aşağıdaki OSs 'de çalışacaktır. Diğer sürümler çalışabilir, ancak Azure Stack Edge Pro cihazlarında çalışan VM 'lerde şirket içinde sınanmamıştır.

| Dağıtım | Sürüm |
|---|---|
| Linux: Ubuntu | 18,04 LTS |
| Linux: Red Hat Enterprise Linux | 7,4, 7,5, 7,7 |

<!--### Script location

Instead of the scripts, in this article, we pass a command to execute via the Custom Script Extension. 

### Internet Connectivity

To download a script externally such as from GitHub or Azure Storage, make sure that the port on which you enable compute network, is connected to the internet. 

If your script is on a local server, then you may still need additional firewall and Network Security Group ports need to be opened.

> [!NOTE]
> Before you install the Custom Script extension, make sure that the port enabled for compute network on your device is connected to Internet and has access. -->

## <a name="prerequisites"></a>Önkoşullar

1. [VM şablonlarını ve parametreleri dosyalarını](https://aka.ms/ase-vm-templates) Istemci makinenize indirin. İndirmeyi, çalışma dizini olarak kullanacağınız bir dizine ayıklayın.

1. Cihazınızda oluşturulmuş ve dağıtılmış bir VM 'niz olmalıdır. VM 'Ler oluşturmak için [şablonları kullanarak Azure Stack Edge Pro 'unuzda VM dağıtma](azure-stack-edge-gpu-deploy-virtual-machine-templates.md)' daki tüm adımları izleyin.

    GitHub veya Azure Storage 'dan dışarıdan, işlem ağını yapılandırırken olduğu gibi bir betiği indirmeniz gerekiyorsa, işlem için Internet 'e bağlı bağlantı noktasını etkinleştirin. Bu sayede betiği indirebilirsiniz.

    Aşağıdaki örnekte, bağlantı noktası 2 Internet 'e bağlandı ve işlem ağını etkinleştirmek için kullanıldı. Önceki adımda Kubernetes 'in gerekli olmadığını belirlediyseniz, Kubernetes düğüm IP 'sini ve dış hizmet IP atamasını atlayabilirsiniz.

    ![İnternet 'e bağlı bağlantı noktasında işlem ayarlarını etkinleştir](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/enable-compute-network-1.png)

## <a name="install-custom-script-extension"></a>Özel Betik uzantısı 'nı yükler

SANAL makinenizin işletim sistemine bağlı olarak, Windows için veya Linux için özel Betik uzantısı 'nı yükleyebilirsiniz.
 

### <a name="custom-script-extension-for-windows"></a>Windows için Özel Betik Uzantısı

Cihazınızda çalışan bir VM için Windows için özel Betik uzantısı dağıtmak üzere, `addCSExtWindowsVM.parameters.json` Parametreler dosyasını düzenleyin ve şablonu dağıtın `addCSextensiontoVM.json` .

#### <a name="edit-parameters-file"></a>Parametre dosyasını Düzenle

Dosya `addCSExtWindowsVM.parameters.json` aşağıdaki parametreleri alır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<Name of VM>" 
        },
        "extensionName": {
            "value": "<Name of extension>" 
        },
        "publisher": {
            "value": "Microsoft.Compute" 
        },
        "type": {
            "value": "CustomScriptExtension" 
        },
        "typeHandlerVersion": {
            "value": "1.10" 
        },
        "settings": {
            "value": {
                "commandToExecute" : "<Command to execute>"
            }
        }
    }
}
```
SANAL makinenizin adını, uzantısı için adınızı ve yürütmek istediğiniz komutu belirtin.

Bu makalede kullanılan örnek parametre dosyası aşağıda verilmiştir.

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "VM5" 
        },
        "extensionName": {
            "value": "CustomScriptExtension" 
        },
        "publisher": {
            "value": "Microsoft.Compute" 
        },
        "type": {
            "value": "CustomScriptExtension" 
        },
        "typeHandlerVersion": {
            "value": "1.10" 
        },
        "settings": {
            "value": {
                "commandToExecute" : "md C:\\Users\\Public\\Documents\\test"
            }
        }
    }
}
```
#### <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtın `addCSextensiontoVM.json` . Bu şablon, uzantıyı var olan bir sanal makineye dağıtır. Şu komutu çalıştırın:

```powershell
$templateFile = "<Path to addCSExtensiontoVM.json file>"
$templateParameterFile = "<Path to addCSExtWindowsVM.parameters.json file>"
$RGName = "<Resource group name>"
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Deployment name>"
```
> [!NOTE]
> Uzantı dağıtımı uzun süredir çalışan bir iş ve tamamlana yaklaşık 10 dakika sürer.

Örnek bir çıktı aşağıda verilmiştir:

```powershell
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addCSExtensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addCSExtWindowsVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment7"

DeploymentName          : deployment7
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/17/2020 10:07:44 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM5
                          extensionName    String                     CustomScriptExtension
                          publisher        String                     Microsoft.Compute
                          type             String                     CustomScriptExtension
                          typeHandlerVersion  String                     1.10
                          settings         Object                     {
                            "commandToExecute": "md C:\\Users\\Public\\Documents\\test"
                          }

Outputs                 :
DeploymentDebugLogLevel :

PS C:\WINDOWS\system32>
```
#### <a name="track-deployment"></a>Dağıtımı izle

Belirli bir VM için uzantıların dağıtım durumunu denetlemek için şu komutu çalıştırın: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName <Name of resource group> -VMName <Name of VM> -Name <Name of the extension>
```
Örnek bir çıktı aşağıda verilmiştir:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM5 -Name CustomScriptExtension

ResourceGroupName       : myasegpuvm1
VMName                  : VM5
Name                    : CustomScriptExtension
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.Compute
ExtensionType           : CustomScriptExtension
TypeHandlerVersion      : 1.10
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/myasegpuvm1/providers/Microsoft.Compute/virtualMachines/VM5/extensions/CustomScriptExtension
PublicSettings          : {
                            "commandToExecute": "md C:\\Users\\Public\\Documents\\test"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

> [!NOTE]
> Dağıtım tamamlandığında, `ProvisioningState` üzerinde yapılan değişiklikler `Succeeded` .

Uzantı çıkışı, hedef sanal makinede bulunan aşağıdaki klasör altında bulunan dosyalara kaydedilir. 

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Belirtilen dosyalar hedef sanal makinede aşağıdaki klasöre indirilir.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Burada <n> , uzantının yürütmeleri arasında değişebilir bir ondalık tamsayıdır. 1. * değeri, uzantının gerçek, geçerli `typeHandlerVersion` değeriyle eşleşir. Örneğin, bu örnekteki gerçek dizin idi `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.10.9\Downloads\0` . 


Bu örnekte, özel uzantı için yürütülecek komut: `md C:\\Users\\Public\\Documents\\test` . Uzantı başarıyla yüklendiğinde, dizinde belirtilen yoldaki dizinin VM 'de oluşturulduğunu doğrulayabilirsiniz. 


### <a name="custom-script-extension-for-linux"></a>Linux için özel Betik uzantısı

Cihazınızda çalışan bir VM için Windows için özel Betik uzantısı dağıtmak üzere, `addCSExtLinuxVM.parameters.json` Parametreler dosyasını düzenleyin ve şablonu dağıtın `addCSExtensiontoVM.json` .

#### <a name="edit-parameters-file"></a>Parametre dosyasını Düzenle

Dosya `addCSExtLinuxVM.parameters.json` aşağıdaki parametreleri alır:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<Name of your VM>" 
        },
        "extensionName": {
            "value": "<Name of your extension>" 
        },
        "publisher": {
            "value": "Microsoft.Azure.Extensions" 
        },
        "type": {
            "value": "CustomScript" 
        },
        "typeHandlerVersion": {
            "value": "2.0" 
        },
        "settings": {
            "value": {
                "commandToExecute" : "<Command to execute>"
            }
        }
    }
}
```
SANAL makinenizin adını, uzantısı için adınızı ve yürütmek istediğiniz komutu belirtin.

Bu makalede kullanılan örnek bir parametre dosyası aşağıda verilmiştir:

```powershell
$templateFile = "<Path to addCSExtensionToVM.json file>"
$templateParameterFile = "<Path to addCSExtLinuxVM.parameters.json file>"
$RGName = "<Resource group name>"
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Deployment name>"
``` 

> [!NOTE]
> Uzantı dağıtımı uzun süredir çalışan bir iş ve tamamlana yaklaşık 10 dakika sürer.

Örnek bir çıktı aşağıda verilmiştir:

```powershell
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addCSExtensionToVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addCSExtLinuxVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment99"

DeploymentName          : deployment99
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/18/2020 1:55:23 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM6
                          extensionName    String                     LinuxCustomScriptExtension
                          publisher        String                     Microsoft.Azure.Extensions
                          type             String                     CustomScript
                          typeHandlerVersion  String                     2.0
                          settings         Object                     {
                            "commandToExecute": "sudo echo 'some text' >> /home/Administrator/file2.txt"
                          }

Outputs                 :
DeploymentDebugLogLevel :

PS C:\WINDOWS\system32>
```

`commandToExecute`Dizinde bir dosya oluşturmak için ayarlanmıştır `file2.txt` `/home/Administrator` ve dosyanın içeriği `some text` . Bu durumda, dosyanın belirtilen yolda oluşturulduğunu doğrulayabilirsiniz.

```powershell
Administrator@VM6:~$ dir
file2.txt
Administrator@VM6:~$ cat file2.txt
some text
Administrator@VM6:
```

#### <a name="track-deployment-status"></a>Dağıtım durumunu izleme    
    
Şablon dağıtımı uzun süredir çalışan bir iş. Belirli bir VM için uzantıların dağıtım durumunu denetlemek için başka bir PowerShell oturumu açın (yönetici olarak çalıştır). Şu komutu çalıştırın: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName <VM Name> -Name <Extension Name>
```
Örnek bir çıktı aşağıda verilmiştir: 

```powershell
PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM5 -Name CustomScriptExtension

ResourceGroupName       : myasegpuvm1
VMName                  : VM5
Name                    : CustomScriptExtension
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.Compute
ExtensionType           : CustomScriptExtension
TypeHandlerVersion      : 1.10
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/myasegpuvm1/providers/Microsoft.Compute/virtualMachines/VM5/extensions/CustomScriptExtension
PublicSettings          : {
                            "commandToExecute": "md C:\\Users\\Public\\Documents\\test"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

> [!NOTE]
> Dağıtım tamamlandığında, `ProvisioningState` üzerinde yapılan değişiklikler `Succeeded` .

Uzantı yürütme çıkışı şu dosyaya kaydedilir: `/var/lib/waagent/custom-script/download/0/` .


## <a name="remove-custom-script-extension"></a>Özel Betik uzantısını kaldır

Özel Betik uzantısını kaldırmak için aşağıdaki komutu kullanın:

`Remove-AzureRmVMExtension -ResourceGroupName <Resource group name> -VMName <VM name> -Name <Extension name>`

Örnek bir çıktı aşağıda verilmiştir:

```powershell
PS C:\WINDOWS\system32> Remove-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM6 -Name LinuxCustomScriptExtension
Virtual machine extension removal operation
This cmdlet will remove the specified virtual machine extension. Do you want to continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Yes
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK
```


## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager cmdlet 'leri](/powershell/module/azurerm.resources/?view=azurermps-6.13.0&preserve-view=true)
