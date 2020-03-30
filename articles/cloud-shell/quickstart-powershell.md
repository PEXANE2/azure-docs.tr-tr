---
title: Azure Bulut Kabuğu Quickstart - PowerShell
description: Azure Cloud Shell ile PowerShell'i tarayıcınızda nasıl kullanacağınızı öğrenin.
author: maertendmsft
ms.author: damaerte
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 10/18/2018
ms.openlocfilehash: 72261989b7cee9d2251eb18b36431ec807b0e874
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273013"
---
# <a name="quickstart-for-powershell-in-azure-cloud-shell"></a>Azure Bulut BulutU'da PowerShell için hızlı başlatma

Bu belge, [Azure portalında](https://portal.azure.com/)Cloud Shell'de PowerShell'in nasıl kullanılacağını ayrıntılarıyla anlatır.

> [!NOTE]
> [Azure Cloud Cloud Cloud Quickstart'ta](quickstart.md) bir Bash da mevcuttur.

## <a name="start-cloud-shell"></a>Cloud Shell'i Başlatma

1. Azure portalının üst teki gezinme çubuğundan **Bulut Kabuğu** düğmesine tıklayın

   ![](media/quickstart-powershell/shell-icon.png)

2. Açılan yerden PowerShell ortamını seçin ve Azure sürücüsünde olursunuz`(Azure:)`

   ![](media/quickstart-powershell/environment-ps.png)

## <a name="run-powershell-commands"></a>PowerShell komutlarını çalıştırın

Bulut Shell'de aşağıdakiler gibi düzenli PowerShell komutları çalıştırın:

```azurepowershell-interactive
PS Azure:\> Get-Date

# Expected Output
Friday, July 27, 2018 7:08:48 AM

PS Azure:\> Get-AzVM -Status

# Expected Output
ResourceGroupName       Name       Location                VmSize   OsType     ProvisioningState  PowerState
-----------------       ----       --------                ------   ------     -----------------  ----------
MyResourceGroup2        Demo        westus         Standard_DS1_v2  Windows    Succeeded           running
MyResourceGroup         MyVM1       eastus            Standard_DS1  Windows    Succeeded           running
MyResourceGroup         MyVM2       eastus   Standard_DS2_v2_Promo  Windows    Succeeded           deallocated
```

## <a name="navigate-azure-resources"></a>Azure kaynaklarında gezinme

 1. Sürücüdeki `Azure` tüm aboneliklerinizi listele

    ```azurepowershell-interactive
    PS Azure:\> dir
    ```

 2. `cd`tercih ettiğiniz aboneye

    ```azurepowershell-interactive
    PS Azure:\> cd MySubscriptionName
    PS Azure:\MySubscriptionName>
    ```

 3. Geçerli abonelik altında tüm Azure kaynaklarınızı görüntüleyin

    Azure `dir` kaynaklarınızın birden çok kez görüntülerini listelemek için yazın.

    ```azurepowershell-interactive
    PS Azure:\MySubscriptionName> dir

        Directory: azure:\MySubscriptionName

    Mode Name
    ---- ----
    +    AllResources
    +    ResourceGroups
    +    StorageAccounts
    +    VirtualMachines
    +    WebApps
    ```

### <a name="allresources-view"></a>AllResources görünümü

Azure `dir` `AllResources` kaynaklarınızı görüntülemek için dizin altında yazın.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir AllResources
```

### <a name="explore-resource-groups"></a>Kaynak gruplarını keşfedin

 `ResourceGroups` Dizin ve sanal makineleri bulabilirsiniz belirli bir kaynak grubu içinde gidebilirsiniz.

```azurepowershell-interactive
PS Azure:\MySubscriptionName> cd ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines

PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines> dir


    Directory: Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup1\Microsoft.Compute\virtualMachines


VMName    Location   ProvisioningState VMSize          OS            SKU             OSVersion AdminUserName  NetworkInterfaceName
------    --------   ----------------- ------          --            ---             --------- -------------  --------------------
TestVm1   westus     Succeeded         Standard_DS2_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo371
TestVm2   westus     Succeeded         Standard_DS1_v2 WindowsServer 2016-Datacenter Latest    AdminUser      demo271
```

> [!NOTE]
> İkinci kez yazarken `dir`Cloud Shell'in öğeleri çok daha hızlı görüntülediğini fark edebilirsiniz.
> Bunun nedeni, alt öğelerin daha iyi bir kullanıcı deneyimi için bellekte önbelleğe alınmış olmasıdır.
Ancak, her zaman `dir -Force` taze veri almak için kullanabilirsiniz.

### <a name="navigate-storage-resources"></a>Depolama kaynaklarında gezinme

Dizine `StorageAccounts` girerek, tüm depolama kaynaklarınızda kolayca gezinebilirsiniz

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> dir

    Directory: Azure:\MySubscriptionNameStorageAccounts\MyStorageAccountName\Files

Name          ConnectionString
----          ----------------
MyFileShare1  \\MyStorageAccountName.file.core.windows.net\MyFileShare1;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare2  \\MyStorageAccountName.file.core.windows.net\MyFileShare2;AccountName=MyStorageAccountName AccountKey=<key>
MyFileShare3  \\MyStorageAccountName.file.core.windows.net\MyFileShare3;AccountName=MyStorageAccountName AccountKey=<key>
```

Bağlantı dizesi ile Azure Dosyaları paylaşımını monte etmek için aşağıdaki komutu kullanabilirsiniz.

```azurepowershell-interactive
net use <DesiredDriveLetter>: \\<MyStorageAccountName>.file.core.windows.net\<MyFileShareName> <AccountKey> /user:Azure\<MyStorageAccountName>
```

Ayrıntılar için Azure Dosyalarını Dağı'na bakın [ve Windows'daki paylaşıma erişin.][azmount]

Azure Dosyaları paylaşımı altındaki dizinlerde de aşağıdaki gibi gezinebilirsiniz:

```azurepowershell-interactive
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files> cd .\MyFileShare1\
PS Azure:\MySubscriptionName\StorageAccounts\MyStorageAccountName\Files\MyFileShare1> dir

Mode  Name
----  ----
+     TestFolder
.     hello.ps1
```

### <a name="interact-with-virtual-machines"></a>Sanal makinelerle etkileşim

Tüm sanal makinelerinizi geçerli abonelik altında `VirtualMachines` dizin üzerinden bulabilirsiniz.

```azurepowershell-interactive
PS Azure:\MySubscriptionName\VirtualMachines> dir

    Directory: Azure:\MySubscriptionName\VirtualMachines


Name       ResourceGroupName  Location  VmSize          OsType              NIC ProvisioningState  PowerState
----       -----------------  --------  ------          ------              --- -----------------  ----------
TestVm1    MyResourceGroup1   westus    Standard_DS2_v2 Windows       my2008r213         Succeeded     stopped
TestVm2    MyResourceGroup1   westus    Standard_DS1_v2 Windows          jpstest         Succeeded deallocated
TestVm10   MyResourceGroup2   eastus    Standard_DS1_v2 Windows           mytest         Succeeded     running
```

#### <a name="invoke-powershell-script-across-remote-vms"></a>Uzak VM'ler arasında PowerShell komut dosyasını çağırma

 > [!WARNING]
 > Lütfen [Azure VM'lerinin Sorun Giderme uzaktan yönetimine](troubleshooting.md#troubleshooting-remote-management-of-azure-vms)bakın.

  Bir VM, MyVM1 varsayarak, `Invoke-AzVMCommand` uzak makinede bir PowerShell komut dosyası bloğu çağırmak için kullanalım.

  ```azurepowershell-interactive
  Enable-AzVMPSRemoting -Name MyVM1 -ResourceGroupname MyResourceGroup
  Invoke-AzVMCommand -Name MyVM1 -ResourceGroupName MyResourceGroup -Scriptblock {Get-ComputerInfo} -Credential (Get-Credential)
  ```

  Ayrıca ilk VirtualMachines dizinine gidin ve `Invoke-AzVMCommand` aşağıdaki gibi çalıştırın.

  ```azurepowershell-interactive
  PS Azure:\> cd MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines
  PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Invoke-AzVMCommand -Scriptblock {Get-ComputerInfo} -Credential (Get-Credential)

  # You will see output similar to the following:

  PSComputerName                                          : 65.52.28.207
  RunspaceId                                              : 2c2b60da-f9b9-4f42-a282-93316cb06fe1
  WindowsBuildLabEx                                       : 14393.1066.amd64fre.rs1_release_sec.170327-1835
  WindowsCurrentVersion                                   : 6.3
  WindowsEditionId                                        : ServerDatacenter
  WindowsInstallationType                                 : Server
  WindowsInstallDateFromRegistry                          : 5/18/2017 11:26:08 PM
  WindowsProductId                                        : 00376-40000-00000-AA947
  WindowsProductName                                      : Windows Server 2016 Datacenter
  WindowsRegisteredOrganization                           :
   ...
  ```

#### <a name="interactively-log-on-to-a-remote-vm"></a>Etkileşimli olarak uzak bir VM'de oturum açın

Azure'da `Enter-AzVM` çalışan bir VM'de etkileşimli olarak oturum açmak için kullanabilirsiniz.

  ```azurepowershell-interactive
  PS Azure:\> Enter-AzVM -Name MyVM1 -ResourceGroupName MyResourceGroup -Credential (Get-Credential)
  ```

Ayrıca önce dizine `VirtualMachines` gidebilir ve `Enter-AzVM` aşağıdaki gibi çalıştırabilirsiniz

  ```azurepowershell-interactive
 PS Azure:\MySubscriptionName\ResourceGroups\MyResourceGroup\Microsoft.Compute\virtualMachines> Get-Item MyVM1 | Enter-AzVM -Credential (Get-Credential)
 ```

### <a name="discover-webapps"></a>WebApps'ı Keşfedin

Dizine `WebApps` girerek, web uygulamaları kaynaklarınızda kolayca gezinebilirsiniz

```azurepowershell-interactive
PS Azure:\MySubscriptionName> dir .\WebApps\

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Stopped  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US

# You can use Azure cmdlets to Start/Stop your web apps
PS Azure:\MySubscriptionName\WebApps> Start-AzWebApp -Name mywebapp1 -ResourceGroupName MyResourceGroup1

Name           State    ResourceGroup        EnabledHostNames                   Location
----           -----    -------------        ----------------                   --------
mywebapp1      Running  MyResourceGroup1     {mywebapp1.azurewebsites.net ...   West US

# Refresh the current state with -Force
PS Azure:\MySubscriptionName\WebApps> dir -Force

    Directory: Azure:\MySubscriptionName\WebApps

Name            State    ResourceGroup      EnabledHostNames                  Location
----            -----    -------------      ----------------                  --------
mywebapp1       Running  MyResourceGroup1   {mywebapp1.azurewebsites.net...   West US
mywebapp2       Running  MyResourceGroup2   {mywebapp2.azurewebsites.net...   West Europe
mywebapp3       Running  MyResourceGroup3   {mywebapp3.azurewebsites.net...   South Central US
```

## <a name="ssh"></a>SSH

SSH kullanarak sunuculara veya VM'lere kimlik doğrulamak için Bulut Bulut'ta ortak `authorized_keys` özel anahtar çiftini `/home/user/.ssh/authorized_keys`oluşturun ve ortak anahtarı uzak makinede yayımlayın.

> [!NOTE]
> Cloud `ssh-keygen` `$env:USERPROFILE\.ssh` Shell'de bunları kullanarak SSH özel ortak anahtarları oluşturabilir ve bunları yayımlayabilirsiniz.

### <a name="using-ssh"></a>SSH kullanma

Azure PowerShell cmdlets kullanarak yeni bir VM yapılandırması oluşturmak için [buradaki](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-powershell) yönergeleri izleyin.
Dağıtımı başlatmak `New-AzVM` için çağırmadan önce, VM yapılandırmasına SSH ortak anahtarı ekleyin.
Yeni oluşturulan `~\.ssh\authorized_keys` VM, konumda ortak anahtarı içerecek ve böylece VM'ye kimlik bilgisi içermeyen SSH oturumu na olanak sağlayacaktır.

```azurepowershell-interactive
# Create VM config object - $vmConfig using instructions on linked page above

# Generate SSH keys in Cloud Shell
ssh-keygen -t rsa -b 2048 -f $HOME\.ssh\id_rsa 

# Ensure VM config is updated with SSH keys
$sshPublicKey = Get-Content "$HOME\.ssh\id_rsa.pub"
Add-AzVMSshPublicKey -VM $vmConfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

# Create a virtual machine
New-AzVM -ResourceGroupName <yourResourceGroup> -Location <vmLocation> -VM $vmConfig

# SSH to the VM
ssh azureuser@MyVM.Domain.Com
```

## <a name="list-available-commands"></a>Kullanılabilir komutları listele

Sürücü `Azure` altında, `Get-AzCommand` içeriğe özel Azure komutlarını almak için yazın.

Alternatif olarak, kullanılabilir `Get-Command *az* -Module Az.*` Azure komutlarını bulmak için her zaman kullanabilirsiniz.

## <a name="install-custom-modules"></a>Özel modüller yükleme

`Install-Module` [PowerShell Galerisi'nden][gallery]modülleri yüklemek için çalıştırabilirsiniz.

## <a name="get-help"></a>Yardım Al

Azure `Get-Help` Cloud Shell'de PowerShell hakkında bilgi almak için yazın.

```azurepowershell-interactive
Get-Help
```

Belirli bir komut için, `Get-Help` yine de bir cmdlet takip yapabilirsiniz.

```azurepowershell-interactive
Get-Help Get-AzVM
```

## <a name="use-azure-files-to-store-your-data"></a>Verilerinizi depolamak için Azure Dosyaları'nı kullanma

Bir komut dosyası oluşturabilir, kabuk oturumları arasında kullanmak için bir komut dosyası oluşturabilir, ', ve `helloworld.ps1`size `clouddrive` kaydedebilirsiniz.

```azurepowershell-interactive
cd $HOME\clouddrive
# Create a new file in clouddrive directory
New-Item helloworld.ps1
# Open the new file for editing
code .\helloworld.ps1
# Add the content, such as 'Hello World!'
.\helloworld.ps1
Hello World!
```

Cloud Shell'de PowerShell'i bir `helloworld.ps1` daha ki `$HOME\clouddrive` sefere kullandığınızda, dosya Azure Dosyaları nızı paylaşan dizinin altında bulunur.

## <a name="use-custom-profile"></a>Özel profil kullanın

PowerShell profil(ler) - `profile.ps1` (veya) `Microsoft.PowerShell_profile.ps1`oluşturarak PowerShell ortamınızı özelleştirebilirsiniz.
Cloud Shell `$profile.CurrentUserAllHosts` oturumundaki her PowerShell'e yüklenebilmek için (veya), `$profile.CurrentUserAllHosts`altında kaydedin.

Profil oluşturma hakkında [bkz.][profile]

## <a name="use-git"></a>Git'i kullan

Bulut Kabuğu'nda bir Git repo'yu klonlamak için kişisel bir [erişim jetonu][githubtoken] oluşturmanız ve kullanıcı adı olarak kullanmanız gerekir. Belirtecinizi aldıktan sonra, depoyu aşağıdaki gibi kopyala:

```azurepowershell-interactive
  git clone https://<your-access-token>@github.com/username/repo.git
```

## <a name="exit-the-shell"></a>Kabuktan çıkış yapma

Oturumu `exit` sonlandırmak için yazın.

[bashqs]:quickstart.md
[gallery]:https://www.powershellgallery.com/
[customex]:https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript
[profile]: https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_profiles
[azmount]: https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows
[githubtoken]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
