---
title: "\"Windows hazırlanıyor\" sanal makine başlatması takılmış. Azure 'da bilgisayarınızı kapatmayın \" Microsoft Docs"
description: "\"Windows 'u alma\" bölümünde, VM başlatmasının takılmasına neden olan sorunu gidermeye yönelik adımları izleyin. Bilgisayarınızı kapatmayın.” iletisinde takılıyor"
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: 2f3c18ea1887ea5b05bb89f85371139ac83dfe49
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70080155"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>"Windows hazırlanıyor" sanal makine başlatması takılmış. Azure 'da bilgisayarınızı kapatmayın "

Bu makale, sanal makineniz (VM) "Windows 'u alma" üzerine takıldığında sorunu çözmenize yardımcı olur. Başlangıç sırasında bilgisayarınızı kapatmayın "aşaması.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="symptoms"></a>Belirtiler

Bir VM 'nin ekran görüntüsünü almak için **önyükleme tanılamayı** kullandığınızda, işletim sistemi tam olarak başlatılmaz. VM "Windows hazırlanıyor" iletisini görüntüler. Bilgisayarınızı kapatmayın.” iletisinde takılıyor

![Windows Server 2012 R2 için ileti örneği](./media/troubleshoot-vm-configure-update-boot/message1.png)

![İleti örneği](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Nedeni

Bu sorun genellikle sunucu, yapılandırma değiştirildikten sonra son yeniden başlatma işlemi gerçekleştirdiğinde oluşur. Yapılandırma değişikliği Windows güncelleştirmeleri veya sunucu rolleri/özelliğindeki değişiklikler tarafından başlatılabilir. Windows Update için, güncelleştirmelerin boyutu büyükse, işletim sisteminin değişiklikleri yeniden yapılandırmak için daha fazla zaman gerekir.

## <a name="back-up-the-os-disk"></a>İşletim sistemi diskini yedekleme

Sorunu gidermeyi denemeden önce, işletim sistemi diskini yedekleyin.

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Şifrelenmiş disk olan VM 'Ler için önce disklerin kilidini açmanız gerekir

VM 'nin şifrelenmiş bir VM olup olmadığını öğrenmek için bu adımları izleyin.

1. Azure portal VM 'nizi açın ve ardından disklere gidin.

2. Şifrelemenin etkin olup olmadığını görmek için **şifreleme** sütununa bakın.

İşletim sistemi diski şifrelendiyse, şifrelenen diskin kilidini açın. Diskin kilidini açmak için aşağıdaki adımları izleyin.

1. Etkilenen VM ile aynı kaynak grubunda, depolama hesabında ve konumda bulunan bir kurtarma VM 'si oluşturun.

2. Azure portal, etkilenen sanal makineyi silin ve diski koruyun.

3. PowerShell 'i yönetici olarak çalıştırın.

4. Gizli adı almak için aşağıdaki cmdlet 'i çalıştırın.

    ```Powershell
    Login-AzAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Gizli dizi adını aldıktan sonra PowerShell 'de aşağıdaki komutları çalıştırın.

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Base64 ile kodlanmış değeri bayta dönüştürün ve çıktıyı bir dosyaya yazın. 

    > [!Note]
    > USB kilidi açma seçeneğini kullanırsanız, BEK dosya adı özgün BEK GUID 'SI ile eşleşmelidir. Bu adımları izlemeden önce, C sürücünüzde "BEK" adlı bir klasör oluşturun.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. Bilgisayarınızda BEK dosyası oluşturulduktan sonra dosyayı kilitli işletim sistemi diskinin bağlı olduğu kurtarma VM 'sine kopyalayın. BEK dosya konumunu kullanarak aşağıdaki komutları çalıştırın.

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **Isteğe bağlı**: Bazı senaryolarda, bu komutu kullanarak diskin şifresinin çözülmesi gerekebilir.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Önceki komut, şifrelenecek diskin F harfinden fazla olduğunu varsayar.

8. Günlükleri toplamanız gerekiyorsa, şu yol **sürücüsüne gidin: \ Windows\System32\winevt\Logs**.

9. Sürücüyü kurtarma makinesinden ayırın.

### <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Anlık görüntü oluşturmak için [bir disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md)' nde adımları izleyin.

## <a name="collect-an-os-memory-dump"></a>Bir işletim sistemi bellek dökümü toplayın

VM 'nin yapılandırmada takıldığında bir işletim sistemi dökümü toplamak için [işletim sistemi dökümünü topla](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) bölümündeki adımları kullanın.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurun

Döküm dosyasını topladıktan sonra, kök nedenini çözümlemek için [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 'ne başvurun.


## <a name="rebuild-the-vm-by-using-powershell"></a>PowerShell kullanarak VM 'yi yeniden derleme

Bellek dökümü dosyasını topladıktan sonra, sanal makineyi yeniden derlemek için aşağıdaki adımları izleyin.

**Yönetilmeyen diskler için**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID “SubscriptionID” | Select-AzSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Yönetilen diskler için**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID "SubscriptionID" | Select-AzSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
