---
title: Azure Stack Edge Pro cihazınızdan GPU VM 'lerinin genel bakış ve dağıtımı
description: Şablonlar kullanılarak Azure Stack Edge Pro cihazında GPU sanal makinelerinin (VM 'Ler) nasıl oluşturulacağını ve yönetileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/21/2020
ms.author: alkohli
ms.openlocfilehash: 7534052412c2bee0f31e352fc577d376c11215c3
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98804926"
---
# <a name="gpu-vms-for-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınız için GPU VM 'Leri

Bu makalede Azure Stack Edge Pro cihazınızda GPU sanal makinelerine (VM 'Ler) genel bir bakış sunulmaktadır. Makalesinde, bir GPU sanal makinesi oluşturma ve ardından uygun NVIDIA sürücülerini yüklemek için GPU sürücü uzantısının nasıl yükleneceği açıklanır. GPU sanal makinesini oluşturmak ve GPU sürücü uzantısını yüklemek için Azure Resource Manager şablonlarını kullanın. 

Bu makale Azure Stack Edge Pro GPU ve Azure Stack Edge Pro R cihazları için geçerlidir.

## <a name="about-gpu-vms"></a>GPU VM 'Leri hakkında

Azure Stack Edge Pro cihazlarınız, NVIDIA Tesla T4 GPU 'SU olan 1 veya 2 ' dir. GPU hızlandırmalı VM iş yüklerini bu cihazlarda dağıtmak için GPU için iyileştirilmiş VM boyutlarını kullanın. Örneğin, NC T4 v3 serisi, T4 GPU 'lara sahip çıkarım iş yüklerini dağıtmak için kullanılmalıdır. 

Daha fazla bilgi için bkz. [NC T4 v3 serisi VM 'ler](../virtual-machines/nct4-v3-series.md).

## <a name="supported-os-and-gpu-drivers"></a>Desteklenen işletim sistemi ve GPU sürücüleri 

Azure N serisi VM 'lerin GPU yeteneklerini avantajlarından yararlanmak için NVIDIA GPU sürücüleri yüklenmelidir. 

NVIDIA GPU sürücü uzantısı uygun NVIDIA CUDA veya KıLAVUZ sürücülerini yüklüyor. Azure Resource Manager şablonlarını kullanarak uzantıyı yükleyebilir veya yönetebilirsiniz.

### <a name="supported-os-for-gpu-extension-for-windows"></a>Windows için GPU uzantısı için desteklenen işletim sistemi

Bu uzantı, aşağıdaki işletim sistemlerini (OSs) destekler. Diğer sürümler çalışabilir, ancak Azure Stack Edge Pro cihazlarında çalışan GPU VM 'lerinde şirket içinde test edilmemiştir.

| Dağıtım | Sürüm |
|---|---|
| Windows Server 2019 | Çekirdek |
| Windows Server 2016 | Çekirdek |

### <a name="supported-os-for-gpu-extension-for-linux"></a>Linux için GPU uzantısı için desteklenen işletim sistemi

Bu uzantı, belirli işletim sistemi sürümü için sürücü desteğine bağlı olarak aşağıdaki işletim sistemi destekleri ' nı destekler. Diğer sürümler çalışabilir, ancak Azure Stack Edge Pro cihazlarında çalışan GPU VM 'lerinde şirket içinde test edilmemiştir.


| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 18,04 LTS |
| Red Hat Enterprise Linux | 7.4 |


## <a name="gpu-vms-and-kubernetes"></a>GPU VM 'Leri ve Kubernetes

Cihazınızda GPU VM 'Leri dağıtmadan önce, cihazda Kubernetes yapılandırılmışsa aşağıdaki noktaları gözden geçirin.

#### <a name="for-1-gpu-device"></a>1-GPU cihaz için: 

- **Cihazınızda Kubernetes yapılandırması tarafından izlenen BIR GPU VM 'Si oluşturun**: Bu SENARYODA, GPU VM oluşturma ve Kubernetes yapılandırmasının her ikisi de başarılı olur. Kubernetes 'in bu durumda GPU 'ya erişimi olmayacaktır.

- **Cihazınızda Kubernetes 'ı yapılandırın**: Bu senaryoda, Kubernetes GPU 'yu cihazınızda talep eder ve kullanılabilir GPU kaynağı olmadığından VM oluşturma işlemi başarısız olur.

#### <a name="for-2-gpu-device"></a>2 GPU cihaz için

- **Cihazınızda Kubernetes yapılandırması tarafından izlenen BIR GPU VM 'Si oluşturun**: Bu senaryoda, oluşturduğunuz GPU sanal makinesi CIHAZıNıZDA bir GPU talep eder ve Kubernetes yapılandırması da başarılı olur ve kalan bir GPU talep eder. 

- **Cihazınızda Kubernetes yapılandırması ile birlikte ıkı GPU VM oluşturun**: Bu senaryoda, Iki GPU sanal makinesi cihazda iki GPU talep eder ve Kubernetes, hiçbir GPU olmadan başarıyla yapılandırılır. 

- **Cihazınızda Kubernetes 'ı yapılandırın**: Bu senaryoda, Kubernetes hem cihazınızdaki GPU 'ları hem de sanal makine oluşturma, kullanılabilir GPU kaynakları olmadığından başarısız olur.

Cihazınızda çalışan GPU VM 'niz varsa ve Kubernetes de yapılandırılmışsa, VM her zaman serbest bırakılır (Stop-AzureRmVM veya-AzureRmVM 'yi kullanarak bir VM 'yi durdurup veya kaldırdığınızda), Kubernetes kümesinin cihazdaki tüm GPU 'ları talep etmesine yardımcı olur. Böyle bir örnekte, cihazınızda dağıtılan GPU VM 'lerini yeniden başlatmanız veya GPU VM 'Leri oluşturmanız mümkün olmayacaktır.


## <a name="create-gpu-vms"></a>GPU VM 'Leri oluşturma

Cihazınızda GPU VM 'Leri dağıtımında şu adımları izleyin:

1. Cihazınızın Kubernetes 'in de çalışıp çalışmadığını belirler. Cihaz Kubernetes 'i çalıştıracaktır, önce GPU VM 'yi oluşturmanız ve ardından Kubernetes 'i yapılandırmanız gerekir. İlk olarak Kubernetes yapılandırıldıysa, tüm kullanılabilir GPU kaynaklarını talep eder ve GPU VM oluşturma işlemi başarısız olur.

1. [VM şablonlarını ve parametreleri dosyalarını](https://aka.ms/ase-vm-templates) Istemci makinenize indirin. Bu dosyayı, çalışma dizini olarak kullanacağınız bir dizine ayıklayın.

1. GPU VM 'Leri oluşturmak için aşağıdaki farklar dışında [şablonları kullanarak Azure Stack Edge Pro 'daki sanal makine dağıtma](azure-stack-edge-gpu-deploy-virtual-machine-templates.md) içindeki tüm adımları izleyin: 

    1. İşlem ağını yapılandırırken, işlem için Internet 'e bağlı olan bağlantı noktasını etkinleştirin. Bu, GPU sanal makinelerinize yönelik GPU uzantıları için gereken GPU sürücülerini indirmelerini sağlar.

        Bağlantı noktası 2 ' nin internet 'e bağlandığı ve işlem ağını etkinleştirmek için kullanıldığı bir örnek aşağıda verilmiştir. Önceki adımda Kubernetes 'in gerekli olmadığını belirlediyseniz, Kubernetes düğüm IP 'sini ve dış hizmet IP atamasını atlayabilirsiniz.

        ![İnternet 'e bağlı bağlantı noktasında işlem ayarlarını etkinleştir](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/enable-compute-network-1.png)

            
    1. Şablonları kullanarak bir VM oluşturun. GPU VM boyutlarını belirtirken, ' de NCasT4-v3-Series ' i `CreateVM.parameters.json` GPU VM 'leri için desteklenmiş gibi kullandığınızdan emin olun. Daha fazla bilgi için bkz. [GPU sanal makineleri Için desteklenen VM boyutları](azure-stack-edge-gpu-virtual-machine-sizes.md#ncast4_v3-series-preview).

        ```json
            "vmSize": {
          "value": "Standard_NC4as_T4_v3"
        },
        ```

    1. GPU VM başarıyla oluşturulduktan sonra, bu VM 'yi, Azure portal Azure Stack Edge kaynağındaki sanal makineler listesinde görüntüleyebilirsiniz.

        ![Azure portal içindeki sanal makineler listesindeki GPU VM 'si](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/list-virtual-machine-1.png)

1. VM 'yi seçin ve ayrıntılara gidin. VM 'ye ayrılan IP 'yi kopyalayın.

    ![Azure portal 'de GPU VM 'sine ayrılan IP](media/azure-stack-edge-gpu-deploy-gpu-virtual-machine/get-ip-gpu-virtual-machine-1.png)

1. VM oluşturulduktan sonra uzantı şablonunu kullanarak GPU uzantısı dağıtın. Linux VM 'Ler için bkz. [Linux IÇIN GPU uzantısı](#gpu-extension-for-linux) ve Windows VM 'leri için bkz. [Windows Için GPU uzantısını yüklemeyi](#gpu-extension-for-windows).

1. GPU uzantısının yüklenmesini doğrulamak için GPU VM 'ye bağlanın:
    1. Windows VM kullanıyorsanız, [WINDOWS VM 'ye bağlanma](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-windows-vm)bölümündeki adımları izleyin. [Yüklemeyi doğrulayın](#verify-windows-driver-installation).
    1. Linux VM kullanıyorsanız, [LINUX VM 'ye bağlanma](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-linux-vm)bölümündeki adımları izleyin. [Yüklemeyi doğrulayın](#verify-linux-driver-installation).

1. Gerekirse, işlem ağını, ihtiyacınız olan her şey için geri geçirebilirsiniz. 


> [!NOTE]
> Cihaz yazılımı sürümünüzü 2012 ' den daha sonra güncelleştirirken GPU VM 'lerini el ile durdurmanız gerekir.


## <a name="install-gpu-extension"></a>GPU uzantısını yükler

SANAL makinenizin işletim sistemine bağlı olarak, Windows için veya Linux için GPU uzantısı 'nı yükleyebilirsiniz.

> [!NOTE]
> GPU uzantısını yüklemeden önce, cihazınızdaki işlem ağı için etkinleştirilen bağlantı noktasının Internet 'e bağlı ve erişime sahip olduğundan emin olun. GPU sürücüleri Internet erişimi aracılığıyla indirilir.

### <a name="gpu-extension-for-windows"></a>Windows için GPU uzantısı

Mevcut bir VM için NVIDIA GPU sürücülerini dağıtmak üzere, `addGPUExtWindowsVM.parameters.json` Parametreler dosyasını düzenleyin ve şablonu dağıtın `addGPUextensiontoVM.json` .

#### <a name="edit-parameters-file"></a>Parametre dosyasını Düzenle

Dosya `addGPUExtWindowsVM.parameters.json` aşağıdaki parametreleri alır:

```json
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: windowsGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverWindows" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DriverURL" : "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
    "DriverCertificateUrl" : "https://go.microsoft.com/fwlink/?linkid=871664",
    "DriverType":"CUDA"
    }
    }
    }
```

Bu makalede kullanılan örnek bir parametre dosyası aşağıda verilmiştir:

```powershell
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\CreateVM\CreateVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\CreateVM\CreateVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment2"

DeploymentName          : deployment2
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:02:56 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          adminUsername    String                     Administrator
                          password         String                     Password1
                          imageName        String                     myasewindowsimg
                          vmSize           String                     Standard_NC4as_T4_v3
                          vnetName         String                     ASEVNET
                          vnetRG           String                     aserg
                          subnetName       String                     ASEVNETsubNet
                          nicName          String                     nic6
                          ipConfigName     String                     ipconfig6
                          privateIPAddress  String

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```
#### <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtın `addGPUextensiontoVM.json` . Bu şablon, uzantıyı var olan bir sanal makineye dağıtır. Şu komutu çalıştırın:

```powershell
$templateFile = "<Path to addGPUextensiontoVM.json>" 
$templateParameterFile = "<Path to addGPUExtWindowsVM.parameters.json>" 
$RGName = "<Name of your resource group>"
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
```
> [!NOTE]
> Uzantı dağıtımı uzun süredir çalışan bir iş ve tamamlana yaklaşık 10 dakika sürer.

Örnek çıktı aşağıdaki gibidir:

```powershell
PS C:\WINDOWS\system32> "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json
PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtWindowsVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "myasegpuvm1"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "deployment3"

DeploymentName          : deployment3
ResourceGroupName       : myasegpuvm1
ProvisioningState       : Succeeded
Timestamp               : 12/16/2020 12:18:50 AM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM2
                          extensionName    String                     windowsgpuext
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverWindows
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
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
Örnek çıktı aşağıdaki gibidir:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName myasegpuvm1 -VMName VM2 -Name windowsgpuext

ResourceGroupName       : myasegpuvm1
VMName                  : VM2
Name                    : windowsgpuext
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverWindows
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/myasegpuvm1/providers/Microsoft.Compute/virtualMachines/VM2/extensions/windowsgpuext
PublicSettings          : {
                            "DriverURL": "http://us.download.nvidia.com/tesla/442.50/442.50-tesla-desktop-winserver-2019-2016-international.exe",
                            "DriverCertificateUrl": "https://go.microsoft.com/fwlink/?linkid=871664",
                            "DriverType": "CUDA"
                          }
ProtectedSettings       :
ProvisioningState       : Creating
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : True
ForceUpdateTag          :

PS C:\WINDOWS\system32>
```

Uzantı yürütme çıkışı aşağıdaki dosyaya kaydedilir. `C:\Packages\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverWindows\1.3.0.0\Status`Yükleme durumunu izlemek için bu dosyaya başvurun. 


Başarılı bir yüklemesi, ve ile olarak `message` belirtilir `Enable Extension` `status` `success` .

```powershell
"status":  {
                       "formattedMessage":  {
                                                "message":  "Enable Extension",
                                                "lang":  "en"
                                            },
                       "name":  "NvidiaGpuDriverWindows",
                       "status":  "success",
```

#### <a name="verify-windows-driver-installation"></a>Windows sürücü yüklemeyi doğrulama

VM 'de oturum açın ve sürücüyle birlikte yüklenen NVIDIA-SMI komut satırı yardımcı programını çalıştırın. , `nvidia-smi.exe` Konumunda bulunur  `C:\Program Files\NVIDIA Corporation\NVSMI\nvidia-smi.exe` . Dosyayı görmüyorsanız, sürücü yüklemesi hala arka planda çalışıyor olabilir. 10 dakika bekleyip yeniden denetleyin.

Sürücü yüklüyse, aşağıdaki örneğe benzer bir çıktı görürsünüz: 

```powershell
PS C:\Users\Administrator> cd "C:\Program Files\NVIDIA Corporation\NVSMI"
PS C:\Program Files\NVIDIA Corporation\NVSMI> ls

    Directory: C:\Program Files\NVIDIA Corporation\NVSMI

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/26/2020  12:00 PM         849640 MCU.exe
-a----        2/26/2020  12:00 PM         443104 nvdebugdump.exe
-a----        2/25/2020   2:06 AM          81823 nvidia-smi.1.pdf
-a----        2/26/2020  12:01 PM         566880 nvidia-smi.exe
-a----        2/26/2020  12:01 PM         991344 nvml.dll

PS C:\Program Files\NVIDIA Corporation\NVSMI> .\nvidia-smi.exe
Wed Dec 16 00:35:51 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 442.50       Driver Version: 442.50       CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name            TCC/WDDM | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla T4            TCC  | 0000503C:00:00.0 Off |                    0 |
| N/A   35C    P8    11W /  70W |      8MiB / 15205MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
PS C:\Program Files\NVIDIA Corporation\NVSMI>
```

Daha fazla bilgi için bkz. [Windows Için NVıDıA GPU sürücü uzantısı](../virtual-machines/extensions/hpccompute-gpu-windows.md).

### <a name="gpu-extension-for-linux"></a>Linux için GPU uzantısı

Mevcut bir VM için NVIDIA GPU sürücülerini dağıtmak üzere, parametreler dosyasını düzenleyin ve şablonu dağıtın `addGPUextensiontoVM.json` . Aşağıdaki bölümlerde anlatıldığı gibi Ubuntu ve Red Hat Enterprise Linux (RHEL) için belirli parametre dosyaları vardır.

#### <a name="edit-parameters-file"></a>Parametre dosyasını Düzenle

Ubuntu kullanılıyorsa, `addGPUExtLinuxVM.parameters.json` dosya aşağıdaki parametreleri alır:

```powershell
"parameters": { 
    "vmName": {
    "value": "<name of the VM>" 
    },
    "extensionName": {
    "value": "<name for the extension. Example: linuxGpu>" 
    },
    "publisher": {
    "value": "Microsoft.HpcCompute" 
    },
    "type": {
    "value": "NvidiaGpuDriverLinux" 
    },
    "typeHandlerVersion": {
    "value": "1.3" 
    },
    "settings": {
    "value": {
    "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
    "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
    "CUDA_ver": "10.0.130",
    "InstallCUDA": "true"
    }
    }
    }
```
Red Hat Enterprise Linux (RHEL) kullanılıyorsa, `addGPUExtensionRHELVM.parameters.json` dosya aşağıdaki parametreleri alır:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "<name of the VM>" 
        },
        "extensionName": {
            "value": "<name for the extension. Example: linuxGpu>" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
                    "isCustomInstall":true,
                    "DRIVER_URL":"https://go.microsoft.com/fwlink/?linkid=874273",
                    "CUDA_ver":"10.0.130",
                    "PUBKEY_URL":"http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                    "DKMS_URL":"https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm",
                    "LIS_URL":"https://aka.ms/lis",
                    "LIS_RHEL_ver":"3.10.0-1062.9.1.el7"
            }
        }
    }
}
```


Bu makalede kullanılan örnek Ubuntu parametre dosyası aşağıda verilmiştir:

```powershell
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "value": "VM1" 
        },
        "extensionName": {
            "value": "gpuLinux" 
        },
        "publisher": {
            "value": "Microsoft.HpcCompute" 
        },
        "type": {
            "value": "NvidiaGpuDriverLinux" 
        },
        "typeHandlerVersion": {
            "value": "1.3" 
        },
        "settings": {
            "value": {
            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
            "CUDA_ver": "10.0.130",
            "InstallCUDA": "true"
            }
        }
    }
}
```


#### <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtın `addGPUextensiontoVM.json` . Bu şablon, uzantıyı var olan bir sanal makineye dağıtır. Şu komutu çalıştırın:

```powershell
$templateFile = "Path to addGPUextensiontoVM.json" 
$templateParameterFile = "Path to addGPUExtLinuxVM.parameters.json" 
$RGName = "<Name of your resource group>" 
New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "<Name for your deployment>"
``` 

> [!NOTE]
> Uzantı dağıtımı uzun süredir çalışan bir iş ve tamamlana yaklaşık 10 dakika sürer.

Örnek çıktı aşağıdaki gibidir:

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> $templateFile = "C:\12-09-2020\ExtensionTemplates\addGPUextensiontoVM.json"
PS C:\WINDOWS\system32> $templateParameterFile = "C:\12-09-2020\ExtensionTemplates\addGPUExtLinuxVM.parameters.json"
PS C:\WINDOWS\system32> $RGName = "rg2"
PS C:\WINDOWS\system32> New-AzureRmResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile $templateFile -TemplateParameterFile $templateParameterFile -Name "delpoyment7"

DeploymentName          : delpoyment7
ResourceGroupName       : rg2
ProvisioningState       : Succeeded
Timestamp               : 12/10/2020 10:43:23 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          vmName           String                     VM1
                          extensionName    String                     gpuLinux
                          publisher        String                     Microsoft.HpcCompute
                          type             String                     NvidiaGpuDriverLinux
                          typeHandlerVersion  String                     1.3
                          settings         Object                     {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL":
                          "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
                          }

Outputs                 :
DeploymentDebugLogLevel :
PS C:\WINDOWS\system32>
```

#### <a name="track-deployment-status"></a>Dağıtım durumunu izleme    
    
Şablon dağıtımı uzun süredir çalışan bir iş. Belirli bir VM için uzantıların dağıtım durumunu denetlemek için başka bir PowerShell oturumu açın (yönetici olarak çalıştır). Şu komutu çalıştırın: 

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName <VM Name> -Name <Extension Name>
```
Örnek çıktı aşağıdaki gibidir: 

```powershell
Copyright (C) Microsoft Corporation. All rights reserved.
Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\WINDOWS\system32> Get-AzureRmVMExtension -ResourceGroupName rg2 -VMName VM1 -Name gpulinux

ResourceGroupName       : rg2
VMName                  : VM1
Name                    : gpuLinux
Location                : dbelocal
Etag                    : null
Publisher               : Microsoft.HpcCompute
ExtensionType           : NvidiaGpuDriverLinux
TypeHandlerVersion      : 1.3
Id                      : /subscriptions/947b3cfd-7a1b-4a90-7cc5-e52caf221332/resourceGroups/rg2/providers/Microsoft.Compute/virtualMachines/VM1/extensions/gpuLinux
PublicSettings          : {
                            "DRIVER_URL": "https://go.microsoft.com/fwlink/?linkid=874271",
                            "PUBKEY_URL": "http://download.microsoft.com/download/F/F/A/FFAC979D-AD9C-4684-A6CE-C92BB9372A3B/7fa2af80.pub",
                            "CUDA_ver": "10.0.130",
                            "InstallCUDA": "true"
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

Uzantı yürütme çıkışı şu dosyaya kaydedilir: `/var/log/azure/nvidia-vmext-status` .

#### <a name="verify-linux-driver-installation"></a>Linux sürücü yüklemesini doğrulama

Sürücü yüklemeyi doğrulamak için aşağıdaki adımları izleyin:

1. GPU VM 'ye bağlanın. [LINUX VM 'ye bağlanma](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#connect-to-linux-vm)bölümündeki yönergeleri izleyin. 

    Örnek çıktı aşağıdaki gibidir:

    ```powershell
    PS C:\WINDOWS\system32> ssh -l Administrator 10.57.50.60
    Administrator@10.57.50.60's password:
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
      System information as of Thu Dec 10 22:57:01 UTC 2020
    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
    
    249 packages can be updated.
    140 updates are security updates.
    
    Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 5.0.0-1031-azure x86_64)    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage    
      System information as of Thu Dec 10 22:57:01 UTC 2020    
      System load:  0.0                Processes:           133
      Usage of /:   24.8% of 28.90GB   Users logged in:     0
      Memory usage: 2%                 IP address for eth0: 10.57.50.60
      Swap usage:   0%
        
    249 packages can be updated.
    140 updates are security updates.
    
    New release '20.04.1 LTS' available.
    Run 'do-release-upgrade' to upgrade to it.
        
    *** System restart required ***
    Last login: Thu Dec 10 21:49:29 2020 from 10.90.24.23
    To run a command as administrator (user "root"), use "sudo <command>".
    See "man sudo_root" for details.
    
    Administrator@VM1:~$
    ```
2. Sürücüsüyle birlikte yüklenen NVIDIA-SMI komut satırı yardımcı programını çalıştırın. Sürücü başarıyla yüklendiyse, yardımcı programı çalıştırabileceksiniz ve aşağıdaki çıktıyı görebilirsiniz:

    ```powershell
    Administrator@VM1:~$ nvidia-smi
    Thu Dec 10 22:58:46 2020
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            Off  | 0000941F:00:00.0 Off |                    0 |
    | N/A   48C    P0    27W /  70W |      0MiB / 15109MiB |      5%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    Administrator@VM1:~$
    ```

Daha fazla bilgi için bkz. [Linux Için NVıDıA GPU sürücü uzantısı](../virtual-machines/extensions/hpccompute-gpu-linux.md).

## <a name="remove-gpu-extension"></a>GPU uzantısını kaldır

GPU uzantısını kaldırmak için aşağıdaki komutu kullanın:

`Remove-AzureRmVMExtension -ResourceGroupName <Resource group name> -VMName <VM name> -Name <Extension name>`

Örnek çıktı aşağıdaki gibidir:

```powershell
PS C:\azure-stack-edge-deploy-vms> Remove-AzureRmVMExtension -ResourceGroupName rgl -VMName WindowsVM -Name windowsgpuext
Virtual machine extension removal operation
This cmdlet will remove the specified virtual machine extension. Do you want to continue? [Y] Yes [N] No [S] Suspend [?] Help (default is "Y"): y
Requestld IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------    
          True                OK         OK
```




## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager cmdlet 'leri](/powershell/module/azurerm.resources/?view=azurermps-6.13.0&preserve-view=true)