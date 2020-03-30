---
title: include dosyası
description: include dosyası
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 11/27/2019
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 456d550659c04b2272c048fcd64fe73b1a11522a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74566379"
---
Yazma Hızlandırıcı, M Serisi Sanal Makineler (VM'ler) için yalnızca Azure Yönetilen Diskler ile Premium Depolama'da bir disk özelliğidir. Adından da belirtildiği gibi, işlevselliğin amacı Azure Premium Depolama'ya karşı yazmaların G/Ç gecikmesini artırmaktır. Yazma Hızlandırıcı, günlük dosyası güncelleştirmelerinin modern veritabanları için son derece performanslı bir şekilde diske kalıcı olması gerektiğinde idealdir.

Yazma Hızlandırıcıgenel olarak Genel Bulut'taki M serisi VM'ler için kullanılabilir.

## <a name="planning-for-using-write-accelerator"></a>Yazma Hızlandırıcısı kullanma planlaması

Yazma Hızlandırıcı, bir DBMS'nin işlem günlüğünü veya redo günlüklerini içeren birimler için kullanılmalıdır. Özellik günlük disklerine karşı kullanılmak üzere optimize edildiğinden, Bir DBMS'nin veri hacimleri için Yazma Hızlandırıcısı kullanılması önerilmez.

Yazma Hızlandırıcısı yalnızca [Azure yönetilen disklerle](https://azure.microsoft.com/services/managed-disks/)birlikte çalışır.

> [!IMPORTANT]
> VM işletim sistemi diski için Yazma Hızlandırıcı'yı etkinleştirmek VM'yi yeniden başlatacaktır.
>
> Windows disk veya birim yöneticileri, Windows Depolama Alanları, Windows Ölçeklendirme dosya sunucusu (SOFS), Linux LVM veya MDADM ile birden fazla diskten yapıyapılamayan bir birim yapısına Yazma Hızlandırıcısını etkinleştirmek için Azure diskine erişen iş yükü kapatılması gerekiyor. Azure diskini kullanan veritabanı uygulamaları KAPATILMALıYıM.
>
> Birden çok Azure Premium Depolama diski dışında oluşturulmuş ve Windows disk veya ses düzeyi yöneticileri, Windows Depolama Alanları, Windows Ölçeklendirme dosya sunucusu (SOFS), Linux LVM veya MDADM kullanılarak şeritlenmiş varolan bir birim için Yazma Hızlandırıcısını etkinleştirmek veya devre dışı etmek istiyorsanız, tümü birim oluşturan diskler ayrı adımlarda Yazma Hızlandırıcısı için etkin veya devre dışı bırakılmalı. **Böyle bir yapılandırmada Yazma Hızlandırıcısını etkinleştirmeden veya devre dışı bırakmadan önce Azure VM'yi kapatın.**

SAP ile ilgili VM yapılandırmaları için IŞLETIM sistemi diskleri için Yazma Hızlandırıcısını etkinleştirmek gerekli olmamalıdır.

### <a name="restrictions-when-using-write-accelerator"></a>Yazma Hızlandırıcısı kullanımına yönelik kısıtlamalar

Azure diski/VHD için Yazma Hızlandırıcısı kullanırken aşağıdaki kısıtlamalar geçerlidir:

- Premium disk önbelleğe alma 'Yok' veya 'Yalnızca Oku' olarak ayarlanmalıdır. Diğer tüm önbelleğe alma modları desteklenmez.
- Anlık görüntü şu anda Hızlandırıcı Yaz etkin diskler için desteklenmez. Yedekleme sırasında Azure Yedekleme hizmeti, VM'ye bağlı Yazma Hızlandırıcı özellikli diskleri otomatik olarak hariç tutar.
- Yalnızca daha küçük G/Ç boyutları (<=512 KiB) hızlandırılmış yolu alıyor. Verilerin toplu yüklendiği veya depolamaya devam edilmeden önce farklı DBMS'nin işlem günlüğü arabelleklerinin daha büyük bir dereceye kadar doldurulduğu iş yükü durumlarında, diske yazılan G/Ç'nin hızlandırılmış yolu izlememe olasılığı yüksektir.

Azure Premium Depolama VHD'lerinin VM başına Yazma Hızlandırıcısı tarafından desteklenebilecek sınırları vardır. Geçerli sınırlar şunlardır:

| VM SKU | Yazma Hızlandırıcı disksayısı | VM başına Hızlandırıcı Disk IOPS yazma |
| --- | --- | --- |
| M416ms_v2, M416s_v2| 16 | 20000 |
| M208ms_v2, M208s_v2| 8 | 10000 |
| M128ms, M128s | 16 | 20000 |
| M64ms, M64ls, M64s | 8 | 10000 |
| M32ms, M32ls, M32ts, M32s | 4 | 5000 |
| M16ms, M16'lar | 2 | 2500 |
| M8ms, M8'ler | 1 | 1250 |

IOPS sınırları VM başınadır ve disk başına *değildir.* Tüm Yazma Hızlandırıcı diskleri VM başına aynı IOPS sınırını paylaşır.

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Yazma Hızlandırıcısını belirli bir diskte etkinleştirme

Sonraki birkaç bölümde, Azure Premium Depolama VHD'lerinde Yazma Hızlandırıcısı'nın nasıl etkinleştirilebileceği açıklanacaktır.

### <a name="prerequisites"></a>Ön koşullar

Bu noktada Yazma Hızlandırıcısı kullanımı için aşağıdaki önkoşullar geçerlidir:

- Azure Yazma Hızlandırıcısına karşı uygulamak istediğiniz disklerin Premium Depolama'da [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) olması gerekir.
- M serisi VM kullanıyor olmalısınız

## <a name="enabling-azure-write-accelerator-using-azure-powershell"></a>Azure Yazma Hızlandırıcısını Azure PowerShell ile etkinleştirme

Sürüm 5.5.0'daki Azure Power Shell modülü, belirli Azure Premium Depolama diskleri için Yazma Hızlandırıcısını etkinleştirmek veya devre dışı ksayamamak için ilgili cmdlets'teki değişiklikleri içerir.
Yazma Hızlandırıcı tarafından desteklenen diskleri etkinleştirmek veya dağıtmak için aşağıdaki Power Shell komutları değiştirildi ve Yazma Hızlandırıcıiçin bir parametre kabul etmek üzere genişletildi.

Yeni bir anahtar parametresi, **-WriteAccelerator** aşağıdaki cmdlets eklenmiştir:

- [Set-Azvmosdisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk?view=azurermps-6.0.0)
- [AzVMDataDisk Ekle](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMDataDisk?view=azurermps-6.0.0)
- [Set-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/Set-AzVMDataDisk?view=azurermps-6.0.0)
- [Ekle-AzVmsSDataDisk](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVmssDataDisk?view=azurermps-6.0.0)

Parametre vermemeözelliği false ayarlar ve Yazma Hızlandırıcı tarafından destek yok diskler dağıtacak.

Yeni bir anahtar parametresi, **-OsDiskWriteAccelerator** aşağıdaki cmdlets eklendi:

- [Set-AzVmssStorageProfile](https://docs.microsoft.com/powershell/module/az.compute/Set-AzVmssStorageProfile?view=azurermps-6.0.0)

Parametre belirtilmedikçe, özelliği varsayılan olarak false olarak ayarlar, Yazma Hızlandırıcı'dan yararlanmayan diskleri döndürür.

Yeni isteğe bağlı Boolean (nullable olmayan) parametresi, **-OsDiskWriteAccelerator** aşağıdaki cmdlets eklendi:

- [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/Update-AzVM?view=azurermps-6.0.0)
- [Güncelleme-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/Update-AzVmss?view=azurermps-6.0.0)

Disklerle Azure Yazma Hızlandırıcısı desteğini denetlemek için $true veya $false belirtin.

Komut örnekleri aşağıdaki gibi görünebilir:

```powershell
New-AzVMConfig | Set-AzVMOsDisk | Add-AzVMDataDisk -Name "datadisk1" | Add-AzVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzVM

Get-AzVM | Update-AzVM -OsDiskWriteAccelerator $true

New-AzVmssConfig | Set-AzVmssStorageProfile -OsDiskWriteAccelerator | Add-AzVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzVmss

Get-AzVmss | Update-AzVmss -OsDiskWriteAccelerator:$false
```

Aşağıdaki bölümlerde gösterildiği gibi iki ana senaryo komut dosyası olabilir.

### <a name="adding-a-new-disk-supported-by-write-accelerator-using-powershell"></a>PowerShell kullanarak Yazma Hızlandırıcı tarafından desteklenen yeni bir disk ekleme

VM'nize yeni bir disk eklemek için bu komut dosyasını kullanabilirsiniz. Bu komut dosyasıyla oluşturulan disk, Yazma Hızlandırıcı'yı kullanır.

Diskin boyutunu ve LunID'yi belirli dağıtımınıza uygun değerlerle değiştirin. `myVM` `myWAVMs` `log001`

```powershell
# Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "log001"
#LUN Id
$lunid=8
#size
$size=1023
#Pulls the VM info for later
$vm=Get-AzVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzVM -ResourceGroupName $rgname -VM $vm
```

### <a name="enabling-write-accelerator-on-an-existing-azure-disk-using-powershell"></a>PowerShell'i kullanarak varolan bir Azure diskinde Yazma Hızlandırıcısını etkinleştirme

Varolan bir diskte Hızlandırıcı Yaz'ı etkinleştirmek için bu komut dosyasını kullanabilirsiniz. Değiştirin `myWAVMs`ve `test-log001` belirli dağıtımınıza uygun değerlerle değiştirin. `myVM` Komut dosyası, **$newstatus** değerinin '$true' olarak ayarlandığı varolan bir diske Yazma Hızlandırıcısı ekler. '$false' değerini kullanmak, belirli bir diskteki Yazma Hızlandırıcısını devre dışı kalacaktır.

```powershell
#Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzVM -ResourceGroupName $rgname -VM $vm
```

> [!Note]
> Yukarıdaki komut dosyasının yürütülmesi belirtilen diski ayıracak, Diske karşı Hızlandırıcı Yaz'ı etkinleştirecek ve diski yeniden takacaktır

## <a name="enabling-write-accelerator-using-the-azure-portal"></a>Yazma Hızlandırıcısını Azure portal ile etkinleştirme

Disk önbelleğe alma ayarlarınızı belirttiğiniz portal aracılığıyla Yazma Hızlandırıcı'yı etkinleştirebilirsiniz:

![Azure portalında Hızlandırıcı yazın](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)

## <a name="enabling-write-accelerator-using-the-azure-cli"></a>Yazma Hızlandırıcısını Azure CLI ile etkinleştirme

Yazma Hızlandırıcısını etkinleştirmek için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) kullanabilirsiniz.

Varolan bir diskte Yazma Hızlandırıcısını etkinleştirmek için [az vm güncelleştirmesini](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-update)kullanın , diskName, VMName ve ResourceGroup'u kendi değerlerinizle değiştirirseniz aşağıdaki örnekleri kullanabilirsiniz:`az vm update -g group1 -n vm1 -write-accelerator 1=true`

Yazma Hızlandırıcı etkin leştirilmiş bir disk eklemek için [az vm disk ekle,](https://docs.microsoft.com/cli/azure/vm/disk?view=azure-cli-latest#az-vm-disk-attach)kendi değerlerinizi yerine geçerseniz aşağıdaki örneği kullanabilirsiniz:`az vm disk attach -g group1 -vm-name vm1 -disk d1 --enable-write-accelerator`

Yazma Hızlandırıcısını devre dışı kullanabilirsiniz, [az vm güncelleştirmesini](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-update)kullanarak özellikleri false olarak ayarı::`az vm update -g group1 -n vm1 -write-accelerator 0=false 1=false`

## <a name="enabling-write-accelerator-using-rest-apis"></a>Geri Alma API'lerini kullanarak Yazma Hızlandırıcısını Etkinleştirme

Azure Dinlenme API'si aracılığıyla dağıtmak için Azure armclient'ını yüklemeniz gerekir.

### <a name="install-armclient"></a>Armclient'ı yükleyin

Armclient çalıştırmak için, Chocolatey üzerinden yüklemeniz gerekir. Cmd.exe veya powershell ile yükleyebilirsiniz. Bu komutlar için yükseltilmiş hakları kullanın ("Yönetici Olarak Çalıştır").

cmd.exe kullanarak aşağıdaki komutu çalıştırın:`@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"`

Power Shell'i kullanarak aşağıdaki komutu çalıştırın:`Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

Şimdi cmd.exe veya PowerShell'de aşağıdaki komutu kullanarak armclient'ı yükleyebilirsiniz`choco install armclient`

### <a name="getting-your-current-vm-configuration"></a>Geçerli VM yapılandırmanızı alma

Disk yapılandırmanızın özniteliklerini değiştirmek için öncelikle geçerli yapılandırmayı bir JSON dosyasında almanız gerekir. Aşağıdaki komutu çalıştırarak geçerli yapılandırmayı alabilirsiniz:`armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>`

JSON dosyasının olması gereken dosya adı da dahil olmak üzere ,'<<   >>' içindeki terimleri verilerinizle değiştirin.

Çıktı aşağıdaki gibi görünebilir:

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Ardından, JSON dosyasını güncelleştirin ve 'log1' adı verilen diskte Hızlandırıcı Yaz'ı etkinleştirin. Bu, diskin önbellek girişinden sonra Bu özniteliği JSON dosyasına ekleyerek gerçekleştirilebilir.

```JSON
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "writeAcceleratorEnabled": true,
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Ardından varolan dağıtımı bu komutla güncelleştirin:`armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>`

Çıktı aşağıdaki gibi görünmelidir. Yazma Hızlandırıcı'nın bir disk için etkinleştirdiğini görebilirsiniz.

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "writeAcceleratorEnabled": true,
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"
```

Bu değişikliği yaptıktan sonra, sürücü Yazma Hızlandırıcı tarafından desteklenmelidir.
