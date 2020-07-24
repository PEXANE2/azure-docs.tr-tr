---
title: PowerShell betiği-silinmiş bir dosya paylaşımının silmeyi geri al
description: Yanlışlıkla silinen bir dosya paylaşımının silmeyi geri almak için Azure PowerShell betiğini nasıl kullanacağınızı öğrenin.
ms.topic: sample
ms.date: 02/02/2020
ms.openlocfilehash: d3a3ad3c2904b98f7a3350d3672b51f3a1e9415e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87007641"
---
# <a name="powershell-script-to-undelete-an-accidentally-deleted-file-share"></a>Yanlışlıkla silinen dosya paylaşımının silmeyi geri almak için PowerShell betiği

Bu betik, yanlışlıkla sildiyseniz bir dosya paylaşımının silmeyi geri almanıza yardımcı olur. Dosya paylaşımları için geçici silme güvenliği özelliği, 14 günlük saklama süresi içinde bir dosya paylaşımının silinmesini ve tüm dosya paylaşım içeriklerinizin, anlık görüntülerinin ve kurtarma noktalarının kurtarılmasına olanak sağlar. Geçici silme hakkında daha fazla bilgi edinmek için bu [bağlantıyı](../soft-delete-azure-file-share.md)ziyaret edin.

## <a name="sample-script"></a>Örnek betik

```powershell
#Import-Module Az.Storage -MinimumVersion 1.7.0 -Scope Local
Param(
        [Parameter(Mandatory=$True)][System.String] $ResourceGroupName,
        [Parameter(Mandatory=$True)][System.String] $StorageAccountName,
        [Parameter(Mandatory=$True)][System.String] $FileShareName,
        [Parameter(Mandatory=$True)][System.String] $SubscriptionId,
        [Parameter(Mandatory=$False)][System.Boolean] $ListOption,
        [Parameter(Mandatory=$False)][System.String] $DeletedShareVersion
    )

Function Restore-DeletedFileShare
{
    Param(
        [Parameter(Mandatory=$True)][Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext] $Context,
        [Parameter(Mandatory=$True)][System.String] $FileShareName,
        [Parameter(Mandatory=$False)][System.String] $DeletedShareVersion
        )

    if ([string]::IsNullOrWhiteSpace($FileShareName))
    {
        Write-Error "Please specify the required input parameter: FileShareName" -ErrorAction Stop
    }

    $FileShareName = $FileShareName.ToLowerInvariant()

    Write-Verbose "Restoring a file share with the name: $FileShareName" -Verbose


    Write-Information -MessageData "Started: Creating SASToken to List File Shares" -InformationAction Continue

    $listToken = New-AzStorageAccountSASToken -Context $Context -Service File -ResourceType Service -Permission "l" -Protocol HttpsOrHttp -StartTime (Get-Date).AddHours(-1) -ExpiryTime (Get-Date).AddHours(1)

    Write-Information -MessageData "Completed: Creating SASToken to List File Shares" -InformationAction Continue

    Write-Information -MessageData "Started: Listing File Shares to find the deleted file share" -InformationAction Continue

    $listSharesUrl = [string]::Concat($Context.FileEndPoint, "?include=metadata,deleted&comp=list&api-version=2019-10-10&", $listToken.Substring(1))

    $listSharesResponse = Invoke-WebRequest $listSharesUrl -Method "GET" -Verbose

    if ($listSharesResponse.StatusCode -ne 200)
    {
        Write-Error "Request to list file shares failed." -ErrorAction Stop
    }

    Write-Verbose $listSharesResponse.RawContent -Verbose

    $listSharesResponseContent = $listSharesResponse.Content.Substring(3)

    Write-Information -MessageData "Completed: Listing File Shares to find the deleted file share" -InformationAction Continue

    Write-Information -MessageData "Started: Search for a deleted file share with the specified name" -InformationAction Continue

    $deletedFileShares = Select-Xml -Content $listSharesResponseContent -XPath "/EnumerationResults/Shares/Share[Deleted=""true"" and Name=""$FileShareName""]"

    $matchedCount = 0
    $deletedShareVersions = New-Object System.Collections.Generic.List[string]

    foreach($share in $deletedFileShares)
    {
        if($matchedCount -eq 0)
        {
          Write-Verbose $share.Node.InnerXml -Verbose

          Write-Information -MessageData "Completed: Search for a deleted file share with the specified name And Found versions" -InformationAction Continue
        }

        $shareVer = $share.Node.Item("Version").InnerText
        $shareDelTime = $share.Node.Item("Properties").Item("DeletedTime").InnerText
        $retDays = $share.Node.Item("Properties").Item("RemainingRetentionDays").InnerText

        $deletedShareVersions.Add($share.Node.Item("Version").InnerText)

        Write-Information -MessageData "DeletedVersion: $shareVer, DeletedTime: $shareDelTime, RemainingRetentionDays: $retDays"  -InformationAction Continue

        $matchedCount++
    }

    if($ListOption -eq $True)
    {
       return;
    }

    if ($matchedCount -eq 0)
    {
        Write-Error "Deleted file share with the specified name was not found." -ErrorAction Stop
    }
    elseif($matchedCount -eq 1 -and ([string]::IsNullOrWhiteSpace($DeletedShareVersion) -or $deletedShareVersions.Contains($DeletedShareVersion)))
    {
      $DeletedShareVersion = $deletedShareVersions
    }
    elseif ($matchedCount -gt 1)
    {
      if ([string]::IsNullOrWhiteSpace($DeletedShareVersion) -or !$deletedShareVersions.Contains($DeletedShareVersion))
      {
        Write-Error "More than one share with the specified name was found. Please specify a valid DeletedShareVersion parameter from above possible values." -ErrorAction Stop
      }
    }

    Write-Information -MessageData "Completed: Search for a deleted file share with the specified name And Found version: $DeletedShareVersion" -InformationAction Continue

    Write-Information -MessageData "Started: Creating SASToken to Restore File Share" -InformationAction Continue

    $restoreToken = New-AzStorageAccountSASToken -Context $Context -Service File -ResourceType Container -Permission "w" -Protocol HttpsOrHttp -StartTime (Get-Date).AddHours(-1) -ExpiryTime (Get-Date).AddHours(1)

    Write-Information -MessageData "Completed: Creating SASToken to Restore File Share" -InformationAction Continue

    Write-Information -MessageData "Started: Restore File Share" -InformationAction Continue

 $restoreShareUrl = [string]::Concat($Context.FileEndPoint, $FileShareName, "?restype=share&comp=undelete&api-version=2019-10-10&", $restoreToken.Substring(1))

    $restoreHeaders = @{"x-ms-deleted-share-name" = $FileShareName; "x-ms-deleted-share-version" = $DeletedShareVersion}

    $restoreResponse = Invoke-WebRequest $restoreShareUrl -Headers $restoreHeaders -Method "PUT" -Verbose

    if ($restoreResponse.StatusCode -ne 201)
    {
        Write-Error "Request to restore a file share failed." -ErrorAction Stop
    }

    Write-Verbose $restoreResponse.RawContent -Verbose

    Write-Information -MessageData "Completed: Restore File Share" -InformationAction Continue
}

Connect-AzAccount
Select-AzSubscription -Subscription $SubscriptionId
$sa = Get-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName


Restore-DeletedFileShare $sa.Context $FileShareName $DeletedShareVersion
```

## <a name="how-to-use-the-script-in-different-scenarios"></a>Betiği farklı senaryolarda kullanma

### <a name="prerequisites"></a>Önkoşullar

1. Betiği çalıştırmadan önce [Bu bağlantıdan](/powershell/azure/install-az-ps) en son Azure PowerShell az modül yükler.
2. Aşağıdaki ayrıntıları, komut dosyasının farklı parametreleri için değer olarak geçirmeniz gereken şekilde yararlı tutun:

    * **-SubscriptionID** -dosya paylaşımının bulunduğu aboneliğin kimliği.
    * **-Resourcegroupname** -dosya paylaşımının barındırıldığı depolama hesabının kaynak grubu.
    * **-StorageAccountName** -dosya paylaşımının barındırıldığı depolama hesabının adı.
    * **-Filesharename** -silinmesi için dosya paylaşımının adı

### <a name="execution-steps"></a>Yürütme adımları

1. Yukarıdaki betiği, seçtiğiniz bir adla makinenize kaydedin. Bu örnekte, *Undelete.ps1* olarak kaydediyoruz
2. Komut dosyasını gereksinimlerinize uyan senaryoya göre çalıştırın.

#### <a name="scenario-1"></a>1\. Senaryo

Silmeyi denediğiniz dosya paylaşımıyla aynı ada sahip birden fazla silinmiş sürüm yok.

Aşağıdaki örnek, depolama hesabı *afsshare*'da bulunan *Share1* dosya paylaşımının kaydını kaldırır.

```powershell
   .\UnDelete.ps1 -ResourceGroupName afsshare -StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1
```

Çıktıda ileti gösterilmesi gerekir`Completed:Restore File Share`

#### <a name="scenario-2"></a>2\. Senaryo

Silmeyi denediğiniz dosya adı ile aynı ada sahip birden fazla silinmiş sürüm var.

Aşağıdaki örnek dosya paylaşımının bir sürümünü kaldırır *Share1*

##### <a name="step-1"></a>1\. Adım

Dosya paylaşımının adını sağlayarak betiği aşağıdaki gibi yürütün.

```PowerShell
   .\UnDelete.ps1 -ResourceGroupName afsshare -StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1
```

```Output
Completed: Search for a deleted file share with the specified name and Found versions
DeletedVersion: 01D5D7F77ACC7864, DeletedTime: Fri, 31 Jan 2020 05:30:33 GMT, RemainingRetentionDays: 14
DeletedVersion: 01D5D7F7A76CAF42, DeletedTime: Fri, 31 Jan 2020 05:31:25 GMT, RemainingRetentionDays: 14
Restore-DeletedFileShare : More than one share with the specified name was found. Please specify a valid DeletedShareVersion parameter from above possible values.
```

##### <a name="step-2"></a>2. Adım

Geri almak istediğiniz 1. adım çıktısından sürümü seçin ve **-deletedshareversion** parametresi için bir değer olarak geçirin.

Aşağıdaki örnek, *Share1* dosya paylaşımının *01D5d7f77acc7864* sürümünü kaldırır.

```powershell
   .\UnDelete.ps1 -ResourceGroupName afsshare-StorageAccountName afsshare -SubscriptionId f75d8d8b-6735-4697-82e1-1a7a3ff0d5d4 -FileShareName share1 -DeletedShareVersion 01D5D7F77ACC7864
```
