---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 09c4420647043fccc408631fec75854667923721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74085284"
---
Microsoft Azure bulut hizmetiyle ilgili sorunları tanılama, sorunlar ortaya çıkarken hizmetin günlük dosyalarını sanal makinelerde toplamayı gerektirir. AzureLogCollector uzantısını isteğe bağlı olarak, bir veya daha fazla Bulut Hizmeti VM'sinden (hem web rollerinden hem de çalışan rollerinden) tek seferlik günlük toplama işlemleri gerçekleştirmek ve toplanan dosyaları bir Azure depolama hesabına aktarmak için kullanabilirsiniz – bunların tümü herhangi bir oturum açmadan VM'ler.

> [!NOTE]
> Günlüğe kaydedilen bilgilerin çoğunun açıklamaları,https://blogs.msdn.microsoft.com/kwill/2013/08/09/windows-azure-paas-compute-diagnostics-data/
> 
> 

Toplanacak dosya türlerine bağlı olarak iki toplama modu vardır.

* **Azure Konuk Aracısı Yalnızca (GA) Günlükleri.** Bu koleksiyon modu, Azure konuk aracıları ve diğer Azure bileşenleriyle ilgili tüm günlükleri içerir.
* **Tüm Günlükler (Tam)**. Bu toplama modu GA modunda artı tüm dosyaları toplar:
  
  * sistem ve uygulama olay günlükleri
  * HTTP hata günlükleri
  * IIS Günlükleri
  * Kurulum günlükleri
  * diğer sistem günlükleri

Her iki toplama modunda da, ek veri toplama klasörleri aşağıdaki yapının bir koleksiyonu kullanılarak belirtilebilir:

* **Adı**: Toplanan dosyalarla birlikte zip dosyasının içindeki alt klasörün adı olarak kullanılan koleksiyonun adıdır.
* **Konum**: Toplanacak dosyaların bulunduğu sanal makinedeki klasöre giden yol.
* **SearchPattern**: Toplanacak dosyaların adlarının deseni. Varsayılan "\*"
* **Özyineleme**: toplanacak dosyalar belirtilen yerin altında özyinelemeli olarak bulunursa.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](./updated-for-az.md)]

* Oluşturulan zip dosyalarını kaydetmek için uzantı için bir depolama hesabına sahip.
* Azure PowerShell. Yükleme yönergeleri için [Azure PowerShell]'i yükleyin'](/powershell/azure/install-az-ps)e bakın.

## <a name="add-the-extension"></a>Uzantıyı ekleme
AzureLogCollector uzantısını eklemek için [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) cmdlets veya [Service Management REST API'lerini](https://msdn.microsoft.com/library/ee460799.aspx) kullanabilirsiniz.

Bulut Hizmetleri için, mevcut Azure Powershell cmdlet, **Set-AzureServiceExtension**, Bulut Hizmeti rol örneklerinde uzantını etkinleştirmek için kullanılabilir. Bu uzantı bu cmdlet aracılığıyla her etkinleştirildiğinde, günlük koleksiyonu seçili rollerin seçili rol örneklerinde tetiklenir.

Sanal Makineler için, mevcut Azure Powershell cmdlet, **Set-AzureVMExtension**, Sanal Makineler de uzantısı etkinleştirmek için kullanılabilir. Bu uzantı cmdlets aracılığıyla etkinleştirildiğinde, günlük toplama her örnekte tetiklenir.

Dahili olarak, bu uzantı JSON tabanlı PublicConfiguration ve PrivateConfiguration kullanır. Aşağıda, genel ve özel yapılandırma için örnek bir JSON'un düzeni ve resuletmek tir.

### <a name="publicconfiguration"></a>Genel Yapılandırma

```json
{
    "Instances":  "*",
    "Mode":  "Full",
    "SasUri":  "SasUri to your storage account with sp=wl",
    "AdditionalData":
    [
      {
              "Name":  "StorageData",
              "Location":  "%roleroot%storage",
              "SearchPattern":  "*.*",
              "Recursive":  "true"
      },
      {
            "Name":  "CustomDataFolder2",
            "Location":  "c:\customFolder",
            "SearchPattern":  "*.log",
            "Recursive":  "false"
      },
    ]
}
```

### <a name="privateconfiguration"></a>PrivateConfiguration

```json
{

}
```

> [!NOTE]
> Bu uzantı **privateConfiguration**gerekmez. **Yalnızca -PrivateConfiguration** bağımsız değişkeni için boş bir yapı sağlayabilirsiniz.
> 
> 

AzureLogCollector'u, toplanan dosyaları çalıştırmak ve belirtilen Azure hesabına göndermek için her VM'deki koleksiyonları tetikleyen, seçili rollerin bir veya daha fazla örneğine AzureLogCollector'u eklemek için aşağıdaki iki adımdan birini izleyebilirsiniz.

## <a name="adding-as-a-service-extension"></a>Hizmet Uzantısı Olarak Ekleme
1. Azure PowerShell'i aboneliğinize bağlamak için yönergeleri izleyin.
2. Eklemek istediğiniz hizmet adını, yuvasını, rolleri ve rol örneklerini belirtin ve AzureLogCollector uzantısını etkinleştirin.

   ```powershell
   #Specify your cloud service name
   $ServiceName = 'extensiontest2'

   #Specify the slot. 'Production' or 'Staging'
   $slot = 'Production'

   #Specified the roles on which the extension will be installed and enabled
   $roles = @("WorkerRole1","WebRole1")

   #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
   $instances = @("*")

   #Specify the collection mode, "Full" or "GA"
   $mode = "GA"
   ```

3. Dosyaların toplanacak ek veri klasörünü belirtin (bu adım isteğe bağlıdır).

   ```powershell
   #add one location
   $a1 = New-Object PSObject

   $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
   $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
   $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
   $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

   $AdditionalDataList+= $a1
   #more locations can be added....
   ```

   > [!NOTE]
   > Sabit bir sürücü `%roleroot%` kullanmadığından rol kökü sürücüsünün belirtinimi belirtebilirsiniz.
   > 
   > 
4. Toplanan dosyaların yükleneceği Azure depolama hesabı adını ve anahtarını sağlayın.

   ```powershell
   $StorageAccountName = 'YourStorageAccountName'
   $StorageAccountKey  = 'YourStorageAccountKey'
   ```

5. Bulut Hizmeti için AzureLogCollector uzantısını etkinleştirmek için SetAzureServiceLogCollector.ps1'i (makalenin sonuna dahil) aşağıdaki gibi arayın. Yürütme tamamlandıktan sonra, yüklenen dosyayı`https://YourStorageAccountName.blob.core.windows.net/vmlogs`

   ```powershell
   .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList
   ```

Aşağıdaki komut dosyasına geçirilen parametrelerin tanımıdır. (Bu da aşağıda kopyalanır.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances,

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: Bulut hizmet adınız.
* **Roller**: "WebRole1" veya "WorkerRole1" gibi rollerlistesi.
* **Örnekler**: Virgülle ayrılmış rol örneklerinin adlarının listesi -- tüm rol örnekleri için joker karakter dizesini ("*") kullanın.
* **Yuva**: Slot adı. "Üretim" veya "Evreleme".
* **Mod**: Toplama modu. "Tam" veya "GA".
* **StorageAccountName**: Toplanan verileri depolamak için Azure depolama hesabının adı.
* **StorageAccountKey**: Azure depolama hesabı anahtarının adı.
* **EkDataLocationList**: Aşağıdaki yapının listesi:

  ```powershell
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="adding-as-a-vm-extension"></a>VM Uzantısı olarak ekleme
Azure PowerShell'i aboneliğinize bağlamak için yönergeleri izleyin.

1. Hizmet adını, VM'yi ve toplama modunu belirtin.

   ```powershell
   #Specify your cloud service name
   $ServiceName = 'YourCloudServiceName'

   #Specify the VM name
   $VMName = "'YourVMName'"

   #Specify the collection mode, "Full" or "GA"
   $mode = "GA"

   Specify the additional data folder for which files will be collected (this step is optional).

   #add one location
   $a1 = New-Object PSObject

   $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
   $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
   $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
   $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"

   $AdditionalDataList+= $a1
        #more locations can be added....
   ```
  
2. Toplanan dosyaların yükleneceği Azure depolama hesabı adını ve anahtarını sağlayın.

   ```powershell
   $StorageAccountName = 'YourStorageAccountName'
   $StorageAccountKey  = 'YourStorageAccountKey'
   ```

3. Bulut Hizmeti için AzureLogCollector uzantısını etkinleştirmek için SetAzureVMLogCollector.ps1'i (makalenin sonuna dahil) aşağıdaki gibi arayın. Yürütme tamamlandıktan sonra, yüklenen dosyayı`https://YourStorageAccountName.blob.core.windows.net/vmlogs`

Aşağıdaki komut dosyasına geçirilen parametrelerin tanımıdır. (Bu da aşağıda kopyalanır.)

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string] $VMName ,

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)
```

* **ServiceName**: Bulut hizmet adınız.
* **VMName**: VM'nin adı.
* **Mod**: Toplama modu. "Tam" veya "GA".
* **StorageAccountName**: Toplanan verileri depolamak için Azure depolama hesabının adı.
* **StorageAccountKey**: Azure depolama hesabı anahtarının adı.
* **EkDataLocationList**: Aşağıdaki yapının listesi:

  ```
  {
    String Name,
    String Location,
    String SearchPattern,
    Bool   Recursive
  }
  ```

## <a name="extention-powershell-script-files"></a>Extention PowerShell Script dosyaları
### <a name="setazureservicelogcollectorps1"></a>SetAzureServiceLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
  [Parameter(Mandatory=$true)]
  [string]   $ServiceName,

  [Parameter(Mandatory=$false)]
  [string[]] $Roles ,

  [Parameter(Mandatory=$false)]
  [string[]] $Instances = '*',

  [Parameter(Mandatory=$false)]
  [string]   $Slot = 'Production',

  [Parameter(Mandatory=$false)]
  [string]   $Mode = 'Full',

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountName,

  [Parameter(Mandatory=$false)]
  [string]   $StorageAccountKey,

  [Parameter(Mandatory=$false)]
  [PSObject[]] $AdditionDataLocationList = $null
)

$publicConfig = New-Object PSObject

if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be separated by ,
{
  $instanceText = $Instances[0]
  for ($i = 1;$i -lt $Instances.Count;$i++)
  {
    $instanceText = $instanceText+ "," + $Instances[$i]
  }
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
}
else  #For all instances if not specified.  The value should be a space or *
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
}

if ($Mode -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
  "Container ($ContainerName) doesn't exist. Creating it now.."
  New-AzStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json
"publicConfig is:  $publicConfigJSON"

#we just provide an empty privateConfig object
$privateconfig = "{
}"

if ($Roles -ne $null)
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}
else
{
  Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
}

#
#This is an optional step: generate a sasUri to the container so it can be shared with other people if needed.
#
$SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
$SasUri = $SasUri + "&restype=container&comp=list"
Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"
```

### <a name="setazurevmlogcollectorps1"></a>SetAzureVMLogCollector.ps1

```powershell
[CmdletBinding(SupportsShouldProcess = $true)]

param (
              [Parameter(Mandatory=$true)]
              [string]   $ServiceName,

              [Parameter(Mandatory=$false)]
              [string] $VMName ,

              [Parameter(Mandatory=$false)]
              [string]   $Mode = 'Full',

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountName,

              [Parameter(Mandatory=$false)]
              [string]   $StorageAccountKey,

              [Parameter(Mandatory=$false)]
              [PSObject[]] $AdditionDataLocationList = $null
        )

$publicConfig = New-Object PSObject
$publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

if ($Mode -ne $null )
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
}
else
{
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
}

#
#we need to get the Sasuri from StorageAccount and containers
#
$context = New-AzStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

$ContainerName = "azurelogcollectordata"
$existingContainer = Get-AzStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
if ($existingContainer -eq $null)
{
    "Container ($ContainerName) doesn't exist. Creating it now.."
    New-AzStorageContainer -Context $context -Name $ContainerName -Permission off
}

$ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
$SasUri = New-AzStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
$publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

#
#Add AdditionalData to collect data from additional folders
#
if ($AdditionDataLocationList -ne $null )
{
  $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
}

#
# Convert it to JSON format
#
$publicConfigJSON = $publicConfig | ConvertTo-Json

Write-Output "PublicConfiguration is: \r\n$publicConfigJSON"

#
#we just provide an empty privateConfig object
#
$privateconfig = "{
}"

if ($VMName -ne $null )
{
      $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
      $VM.VM.OSVirtualHardDisk.OS

      if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
      {
            Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

            #
            #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
            #the presence of SubstatusList field.
            #
            $Completed = $false
            while ($Completed -ne $true)
            {
                    $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                    $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                    if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                    {
                        Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                          $Completed = $true
                    }
                    elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                    {
                          $Completed = $false
                          Write-Output "Waiting for operation to complete..."
                    }
                    else
                    {
                          $Completed = $true
                          Write-Output "Operation completed."

                    $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                          $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                  #
                        # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                          #
                          $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                          $ReadSasUri = New-AzStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                        Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                          #
                          #This is an optional step:  Remove the extension after we are done
                          #
                          Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                    }
                    Start-Sleep -s 5
            }
      }
      else
      {
          Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
      }

}
else
{
  Write-Output "VM name is not specified, the extension cannot be enabled"
}
```

## <a name="next-steps"></a>Sonraki Adımlar
Artık günlüklerinizi basit bir konumdan inceleyebilir veya kopyalayabilirsiniz.

