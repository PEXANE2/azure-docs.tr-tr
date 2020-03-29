---
title: Azure VM sorunlarını gidermek için uzak araçları kullanın | Microsoft Dokümanlar
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: b86b1a2d8a49554cc3df99e0a32a2c0ccaacb560
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920017"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Azure VM sorunlarını gidermek için uzak araçları kullanın

Bir Azure sanal makinesindeki (VM) sorunları giderdiğinizde, Uzak Masaüstü Protokolü 'nü (RDP) kullanmak yerine bu makalede tartışılan uzak araçları kullanarak VM'ye bağlanabilirsiniz.

## <a name="serial-console"></a>Seri konsol

Uzak [Azure](serial-console-windows.md) VM'de komutları çalıştırmak için Azure Sanal Makineler için bir seri konsol kullanın.

## <a name="remote-cmd"></a>Uzak CMD

[PsExec'i](https://docs.microsoft.com/sysinternals/downloads/psexec)indirin. Aşağıdaki komutu çalıştırarak VM'ye bağlanın:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!NOTE]
>* Komut, aynı sanal ağda bulunan bir bilgisayarda çalıştırılmalıdır.
>* DGerek veya HostName, \<bilgisayar> değiştirmek için kullanılabilir.
>* -s parametresi, Sistem Hesabı (yönetici izni) kullanılarak komutun çağrılmasını sağlar.
>* PsExec TCP portları 135 ve 445 kullanır. Sonuç olarak, iki bağlantı noktasının güvenlik duvarında açık olması gerekir.

## <a name="run-command"></a>Çalıştır komutu

VM'de komut dosyalarını çalıştırmak için run komutu özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi için, [çalıştır komutuyla Windows VM'nizde PowerShell komut dosyalarını çalıştır'a](../windows/run-command.md)bakın.

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

Hedef VM'de özel bir komut dosyası çalıştırmak için Özel Komut Dosyası Uzantısı özelliğini kullanabilirsiniz. Bu özelliği kullanmak için aşağıdaki koşulların karşılanması gerekir:

* VM bağlantısı vardır.
* Azure Sanal Makine Aracısı yüklüdür ve VM'de beklendiği gibi çalışır.
* Uzantı daha önce VM yüklü değildi.
 
  Uzantı, komut dosyasını yalnızca ilk kez kullanıldığında enjekte eder. Bu özelliği daha sonra kullanırsanız, uzantı zaten kullanıldığını algılar ve yeni komut dosyasını yüklemez.

Komut dosyanızı bir depolama hesabına yükleyin ve kendi kapsayıcısını oluşturun. Ardından, Azure PowerShell'de aşağıdaki komut dosyasını VM bağlantısı olan bir bilgisayarda çalıştırın.

### <a name="for-classic-deployment-model-vms"></a>Klasik dağıtım modeli VM'ler için

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


```powershell
#Set up the basic variables.
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage.
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group or cloud service where the VM is hosted. For example, for "demo305.cloudapp.net" the cloud service is going to be demo305.

#Set up the Azure PowerShell module, and ensure the access to the subscription.
Import-Module Azure
Add-AzureAccount  #Ensure login with the account associated with the subscription ID.
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Set up the access to the storage account, and upload the script.
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM.
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-azure-resource-manager-vms"></a>Azure Kaynak Yöneticisi VM'leri için

 

```powershell
#Set up the basic variables.
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage.
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Set up the Azure PowerShell module, and ensure the access to the subscription.
Login-AzAccount #Ensure login with the account associated with the subscription ID.
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription

#Set up the access to the storage account, and upload the script.
$storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM.
Set-AzVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>Uzaktan PowerShell

>[!NOTE]
>Bu seçeneği kullanabilmeniz için TCP Bağlantı Noktası 5986 (HTTPS) açık olmalıdır.
>
>Azure Kaynak Yöneticisi VM'leri için ağ güvenlik grubunda (NSG) bağlantı noktası 5986'yı açmanız gerekir. Daha fazla bilgi için Güvenlik gruplarına bakın. 
>
>RDFE VM'leri için, özel bağlantı noktası (5986) ve ortak bağlantı noktası olan bir bitiş noktanız olmalıdır. O zaman NSG'deki halka açık limanı da açmanız gerekiyor.

### <a name="set-up-the-client-computer"></a>İstemci bilgisayarını ayarlama

VM'ye uzaktan bağlanmak için PowerShell'i kullanmak için öncelikle bağlantıya izin verecek şekilde istemci bilgisayarı kurmanız gerekir. Bunu yapmak için, uygun şekilde aşağıdaki komutu çalıştırarak PowerShell güvenilen ana bilgisayarlar listesine VM'yi ekleyin.

Güvenilen ana bilgisayarlar listesine bir VM eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Güvenilen ana bilgisayarlar listesine birden çok VM eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Tüm bilgisayarları güvenilen ana bilgisayarlar listesine eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>VM'de RemotePS'yi etkinleştir

Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için, aşağıdaki komut dosyasını çalıştırmak için Özel Komut Dosyası Uzantısı'nı kullanın:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

Azure Kaynak Yöneticisi VM'leri için, EnableRemotePS komut dosyasını çalıştırmak için portaldaki çalıştır komutlarını kullanın:

![Çalıştır komutu](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

İstemci bilgisayar konumuna göre aşağıdaki komutu çalıştırın:

* Sanal ağ veya dağıtım dışında

  * Klasik dağıtım modeli kullanılarak oluşturulan bir VM için aşağıdaki komutu çalıştırın:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

  * Azure Kaynak Yöneticisi VM için önce ortak IP adresine bir DNS adı ekleyin. Ayrıntılı adımlar için bkz. Windows [VM için Azure portalında tam nitelikli bir etki alanı adı oluşturun.](../windows/portal-create-fqdn.md) Ardından şu komutu çalıştırın:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* Sanal ağ veya dağıtım içinde aşağıdaki komutu çalıştırın:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!NOTE] 
>SkipCaCheck bayrağını ayarlamak, oturumu başlattığınızda VM'ye sertifika alma gereksinimini atlar.

VM'de komut dosyasını uzaktan çalıştırmak için Invoke Komutu cmdlet'ini de kullanabilirsiniz.

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Uzak Kayıt Defteri

>[!NOTE]
>Bu seçeneği kullanabilmek için TCP bağlantı noktası 135 veya 445'in açık olması gerekir.
>
>Azure Kaynak Yöneticisi VM'leri için NSG'de 5986 bağlantı noktasını açmanız gerekir. Daha fazla bilgi için Güvenlik gruplarına bakın. 
>
>RDFE VM'leri için, özel bağlantı noktası 5986 ve ortak bağlantı noktası olan bir bitiş noktanız olmalıdır. Ayrıca NSG'deki halka açık limanı da açmalısın.

1. Aynı sanal ağdaki başka bir VM'den kayıt defteri düzenleyicisini (regedit.exe) açın.

2. **Dosya** > **Bağlan Ağ Kayıt Defteri'ni**seçin.

   ![Kayıt defteri düzenleyicisi](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Hedef **VM'yi ana bilgisayar adına** veya **dinamik IP'ye** (tercih edilen) göre, **kutuyu seçmek için nesne adını girin'e** girerek bulun.

   ![Kutuyu seçmek için nesne adını girin](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Hedef VM için kimlik bilgilerini girin.

5. Gerekli kayıt defteri değişikliklerini yapın.

## <a name="remote-services-console"></a>Uzak hizmetler konsolu

>[!NOTE]
>Bu seçeneği kullanabilmek için TCP bağlantı noktaları 135 veya 445'in açık olması gerekir.
>
>Azure Kaynak Yöneticisi VM'leri için NSG'de 5986 bağlantı noktasını açmanız gerekir. Daha fazla bilgi için Güvenlik gruplarına bakın. 
>
>RDFE VM'leri için, özel bağlantı noktası 5986 ve ortak bağlantı noktası olan bir bitiş noktanız olmalıdır. Ayrıca NSG'deki halka açık limanı da açmalısın.

1. Aynı sanal ağdaki başka bir VM'den **Services.msc'nin**bir örneğini açın.

2. Sağ tıkla **Hizmetler (Yerel)**.

3. **Başka bir bilgisayara bağlan'ı**seçin.

   ![Uzaktan hizmet](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Hedef VM'nin dinamik IP'sini girin.

   ![Giriş dinamik IP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Hizmetlerde gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar

- Enter-PSSession cmdlet hakkında daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/hh849707.aspx)
- Klasik dağıtım modelini kullanarak Windows için Özel Komut Dosyası Uzantısı hakkında daha fazla bilgi için [Windows için Özel Komut Dosyası Uzantısı'na](../extensions/custom-script-classic.md)bakın.
- PsExec, [PSTools Suite'in](https://download.sysinternals.com/files/PSTools.zip)bir parçasıdır.
- PSTools Suite hakkında daha fazla bilgi için [PSTools'a](https://docs.microsoft.com/sysinternals/downloads/pstools)bakın.


