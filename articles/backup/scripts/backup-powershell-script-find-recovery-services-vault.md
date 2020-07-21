---
title: PowerShell betiği-depolama hesabı için kasa bulma
description: Depolama hesabınızın kaydedildiği kurtarma hizmetleri kasasını bulmak için bir Azure PowerShell betiği kullanmayı öğrenin.
ms.topic: sample
ms.date: 1/28/2020
ms.openlocfilehash: b343b2f93ed439188c5c0238bf108064f6e132c1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513260"
---
# <a name="powershell-script-to-find-the-recovery-services-vault-where-a-storage-account-is-registered"></a>Bir depolama hesabının kaydedildiği kurtarma hizmetleri kasasını bulmak için PowerShell betiği

Bu betik, depolama hesabınızın kaydedildiği kurtarma hizmetleri kasasını bulmanıza yardımcı olur.

## <a name="sample-script"></a>Örnek betik

```powershell
Param(
        [Parameter(Mandatory=$True)][System.String] $ResourceGroupName,
        [Parameter(Mandatory=$True)][System.String] $StorageAccountName,
        [Parameter(Mandatory=$True)][System.String] $SubscriptionId
    )

Connect-AzAccount
Select-AzSubscription -Subscription $SubscriptionId
$vaults = Get-AzRecoveryServicesVault
$found = $false
foreach($vault in $vaults)
{
  Write-Verbose "Checking vault: $($vault.Id)" -Verbose
  
  $containers = Get-AzRecoveryServicesBackupContainer -ContainerType AzureStorage -FriendlyName $StorageAccountName -ResourceGroupName $ResourceGroupName -VaultId $vault.Id -Status Registered
  
  if($containers -ne $null)
  {
    $found = $True
    Write-Information "Found Storage account $StorageAccountName registered in vault: $($vault.Id)" -InformationAction Continue
    break;
  }
}

if(!$found)
{
     Write-Information "Storage account: $StorageAccountName is not registered in any vault of this subscription" -InformationAction Continue
}
```

## <a name="how-to-execute-the-script"></a>Betiği yürütme

1. Yukarıdaki betiği, seçtiğiniz bir adla makinenize kaydedin. Bu örnekte, onu *FindRegisteredStorageAccount.ps1*olarak kaydettik.
2. Aşağıdaki parametreleri sağlayarak betiği yürütün:

    * **-Resourcegroupname** -depolama hesabının kaynak grubu
    * **-StorageAccountName** -depolama hesabı adı
    * **-SubscriptionID** -depolama hesabının bulunduğu aboneliğin kimliği.

Aşağıdaki örnek, *afsaccount* depolama hesabının kaydedildiği kurtarma hizmetleri kasasını bulmayı dener:

```powershell
.\FindRegisteredStorageAccount.ps1 -ResourceGroupName AzureFiles -StorageAccountName afsaccount -SubscriptionId ef4ad5a7-c2c0-4304-af80-af49f49af3d1
```

## <a name="output"></a>Çıktı

Çıktı, depolama hesabının kaydedildiği kurtarma hizmetleri kasasının tüm yolunu görüntüler. Örnek çıktı aşağıdaki gibidir:

```output
Found Storage account afsaccount registered in vault: /subscriptions/ ef4ad5a7-c2c0-4304-af80-af49f49af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault123
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Portal Azure dosya paylaşımlarını nasıl yedekleyeceğinizi](../backup-afs.md) öğrenin
