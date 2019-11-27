---
title: Azure VM sorunlarını gidermek için uzak araçları kullanma | Microsoft Docs
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
ms.openlocfilehash: 3f028431fcd4b338d2e610ce1828a02b753c4d32
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483695"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Azure VM sorunlarını gidermek için uzak araçları kullanma

Bir Azure sanal makinesinde (VM) sorunları giderirken, Uzak Masaüstü Protokolü (RDP) kullanmak yerine bu makalede ele alınan uzak araçları kullanarak VM 'ye bağlanabilirsiniz.

## <a name="serial-console"></a>Seri konsol

[Azure sanal makineleri için](serial-console-windows.md) , uzak Azure VM 'de komutları çalıştırmak üzere bir seri konsol kullanın.

## <a name="remote-cmd"></a>Uzak CMD

[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)'yi indirin. Aşağıdaki komutu çalıştırarak VM 'ye bağlanın:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!NOTE]
>* Komutun aynı sanal ağdaki bir bilgisayarda çalıştırılması gerekir.
>* DIP veya HostName, \<bilgisayar > değiştirmek için kullanılabilir.
>* -S parametresi, komutun sistem hesabı (yönetici izni) kullanılarak çağrıldığından emin olur.
>* PsExec 135 ve 445 TCP bağlantı noktalarını kullanır. Sonuç olarak, iki bağlantı noktasının güvenlik duvarında açık olması gerekir.

## <a name="run-command"></a>Komutu Çalıştır

VM 'de betikleri çalıştırmak için komutu Çalıştır özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi için, bkz. [Run komutuyla WINDOWS sanal makinenizde PowerShell betikleri çalıştırma](../windows/run-command.md).

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

Özel Betik uzantısı özelliğini kullanarak hedef VM 'de özel bir komut dosyası çalıştırabilirsiniz. Bu özelliği kullanmak için aşağıdaki koşulların karşılanması gerekir:

* VM 'nin bağlantısı vardır.
* Azure sanal makine Aracısı yüklü ve sanal makinede beklendiği şekilde çalışıyor.
* Uzantı daha önce VM 'de yüklü değildi.
 
  Uzantı, betiği yalnızca ilk kullanıldığı zaman çıkartır. Bu özelliği daha sonra kullanırsanız, uzantı zaten kullanıldığını algılar ve yeni betiği karşıya yüklememez.

Komut dosyanızı bir depolama hesabına yükleyin ve kendi kapsayıcısını oluşturun. Ardından, VM bağlantısı olan bir bilgisayarda Azure PowerShell aşağıdaki betiği çalıştırın.

### <a name="for-classic-deployment-model-vms"></a>Klasik dağıtım modeli VM 'Leri için

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

### <a name="for-azure-resource-manager-vms"></a>Azure Resource Manager VM 'Ler için

 

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

## <a name="remote-powershell"></a>Uzak PowerShell

>[!NOTE]
>Bu seçeneği kullanabilmeniz için TCP bağlantı noktası 5986 (HTTPS) açık olmalıdır.
>
>Azure Resource Manager VM 'Ler için, ağ güvenlik grubu (NSG) üzerinde 5986 numaralı bağlantı noktasını açmanız gerekir. Daha fazla bilgi için bkz. güvenlik grupları. 
>
>RDFE VM 'Leri için özel bağlantı noktası (5986) ve genel bağlantı noktası olan bir uç noktaya sahip olmanız gerekir. Daha sonra, NSG 'de genel kullanıma yönelik bağlantı noktasını da açmanız gerekir.

### <a name="set-up-the-client-computer"></a>İstemci bilgisayarı ayarlama

VM 'yi uzaktan bağlamak üzere PowerShell 'i kullanmak için önce istemci bilgisayarı bağlantıya izin verecek şekilde ayarlamanız gerekir. Bunu yapmak için, aşağıdaki komutu uygun şekilde çalıştırarak VM 'yi PowerShell güvenilir ana bilgisayarlar listesine ekleyin.

Güvenilen konaklar listesine bir VM eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Güvenilir konaklar listesine birden çok VM eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Tüm bilgisayarları güvenilir konaklar listesine eklemek için:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>VM 'de RemotePS 'yi etkinleştirme

Klasik dağıtım modeli kullanılarak oluşturulan VM 'Ler için, aşağıdaki betiği çalıştırmak üzere özel Betik uzantısı ' nı kullanın:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

Azure Resource Manager VM 'Ler için, EnableRemotePS betiğini çalıştırmak için portaldan Çalıştır komutlarını kullanın:

![Komutu Çalıştır](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>VM’ye bağlanma

İstemci bilgisayar konumuna bağlı olarak aşağıdaki komutu çalıştırın:

* Sanal ağ veya dağıtım dışında

  * Klasik dağıtım modeli kullanılarak oluşturulan bir VM için aşağıdaki komutu çalıştırın:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

  * Azure Resource Manager VM için, önce genel IP adresine bir DNS adı ekleyin. Ayrıntılı adımlar için bkz. [WINDOWS VM için Azure Portal tam etki alanı adı oluşturma](../windows/portal-create-fqdn.md). Ardından şu komutu çalıştırın:

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
>SkipCaCheck bayrağını ayarlamak, oturumu başlattığınızda VM 'ye bir sertifikayı aktarma gereksinimini atlar.

Ayrıca, VM 'de bir betiği uzaktan çalıştırmak için Invoke-Command cmdlet 'ini de kullanabilirsiniz.

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Uzak kayıt defteri

>[!NOTE]
>Bu seçeneği kullanabilmek için TCP bağlantı noktası 135 veya 445 açık olmalıdır.
>
>Azure Resource Manager VM 'Ler için NSG 'de 5986 numaralı bağlantı noktasını açmanız gerekir. Daha fazla bilgi için bkz. güvenlik grupları. 
>
>RDFE VM 'Leri için, özel bağlantı noktası 5986 ve genel bağlantı noktası olan bir uç noktaya sahip olmanız gerekir. Ayrıca, NSG 'de genel kullanıma yönelik bağlantı noktasını açmanız gerekir.

1. Aynı sanal ağdaki başka bir VM 'den, kayıt defteri Düzenleyicisi 'ni (Regedit. exe) açın.

2. **Dosya** > **ağ kayıt defterine bağla**' yı seçin.

   ![Kayıt Defteri Düzenleyicisi](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Hedef VM 'yi **konak adına** veya **dinamik IP** 'ye (tercih edilen) ekleyerek **Seçilecek nesne adını girin** kutusunu girerek bulun.

   ![Seçilecek nesne adını girin kutusu](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Hedef VM 'nin kimlik bilgilerini girin.

5. Tüm gerekli kayıt defteri değişikliklerini yapın.

## <a name="remote-services-console"></a>Uzak hizmetler konsolu

>[!NOTE]
>Bu seçeneği kullanabilmek için 135 veya 445 TCP bağlantı noktaları açık olmalıdır.
>
>Azure Resource Manager VM 'Ler için NSG 'de 5986 numaralı bağlantı noktasını açmanız gerekir. Daha fazla bilgi için bkz. güvenlik grupları. 
>
>RDFE VM 'Leri için, özel bağlantı noktası 5986 ve genel bağlantı noktası olan bir uç noktaya sahip olmanız gerekir. Ayrıca, NSG 'de genel kullanıma yönelik bağlantı noktasını açmanız gerekir.

1. Aynı sanal ağdaki başka bir VM 'den **Services. msc**' nin bir örneğini açın.

2. **Hizmetler (yerel)** öğesine sağ tıklayın.

3. **Başka bir bilgisayara bağlan**' ı seçin.

   ![Uzak hizmet](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Hedef VM 'nin dinamik IP 'sini girin.

   ![Giriş dinamik IP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. Hizmetlerde gerekli değişiklikleri yapın.

## <a name="next-steps"></a>Sonraki adımlar

- ENTER-PSSession cmdlet 'i hakkında daha fazla bilgi için bkz. [-PSSession](https://technet.microsoft.com/library/hh849707.aspx).
- Klasik dağıtım modelini kullanarak Windows için özel Betik uzantısı hakkında daha fazla bilgi için bkz. [Windows Için özel Betik uzantısı](../extensions/custom-script-classic.md).
- PsExec, [Padstools paketinin](https://download.sysinternals.com/files/PSTools.zip)bir parçasıdır.
- Padstools Suite hakkında daha fazla bilgi için bkz. [padstools](https://docs.microsoft.com/sysinternals/downloads/pstools).


