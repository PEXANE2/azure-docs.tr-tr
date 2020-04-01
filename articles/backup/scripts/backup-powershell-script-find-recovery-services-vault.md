---
title: PowerShell Script - Depolama Hesabı için Vault bulmak
description: Depolama hesabınızın kayıtlı olduğu kurtarma hizmetleri kasasını bulmak için Azure PowerShell komut dosyasını nasıl kullanacağınızı öğrenin.
ms.topic: sample
ms.date: 1/28/2020
ms.openlocfilehash: 786420ec8cef6516f7261c71b40641693efece07
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76775364"
---
# <a name="powershell-script-to-find-the-recovery-services-vault-where-a-storage-account-is-registered"></a>Depolama Hesabının kayıtlı olduğu Kurtarma Hizmetleri Kasası'nı bulmak için Powershell Script

Bu komut dosyası, depolama hesabınızın kayıtlı olduğu kurtarma hizmetleri kasasını bulmanıza yardımcı olur.

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

## <a name="how-to-execute-the-script"></a>Komut dosyası nasıl yürütülür?

1. Yukarıdaki komut dosyasını seçtiğiniz bir adla makinenize kaydedin. Bu örnekte, *FindRegisteredStorageAccount.ps1*olarak kaydettik.
2. Aşağıdaki parametreleri sağlayarak komut dosyası yürütmek:

    * **-ResourceGroupName** - Depolama hesabının Kaynak Grubu
    * **-StorageAccountName** - Depolama Hesap Adı
    * **-SubscriptionID** - Depolama hesabının bulunduğu abonelik kimliği.

Aşağıdaki örnek, *afsaccount* depolama hesabının kayıtlı olduğu kurtarma hizmetleri kasasını bulmaya çalışır:

```powershell
.\FindRegisteredStorageAccount.ps1 -ResourceGroupName AzureFiles -StorageAccountName afsaccount -SubscriptionId ef4ad5a7-c2c0-4304-af80-af49f49af3d1
```

## <a name="output"></a>Çıktı

Çıktı, depolama hesabının kayıtlı olduğu kurtarma hizmetleri kasasının tam yolunu görüntüler. Örnek çıktı aşağıdaki gibidir:

```output
Found Storage account afsaccount registered in vault: /subscriptions/ ef4ad5a7-c2c0-4304-af80-af49f49af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault123
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalından Azure Dosya Paylaşımlarını Nasıl Yedekleyebilirsiniz](https://docs.microsoft.com/azure/backup/backup-afs) öğrenin
