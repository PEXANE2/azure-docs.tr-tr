---
title: Azure Site Kurtarma'da Mobilite hizmetinin otomatik olarak güncellemesi
description: Azure Site Kurtarma'yı kullanarak Azure VM'lerini çoğaltırken Mobilite hizmetinin otomatik olarak güncelliğine genel bakış.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/24/2019
ms.author: rajanaki
ms.openlocfilehash: 3a9b0717368fa67f5a7dd477e018a68e048b6740
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451400"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Azure'dan Azure'a çoğaltmada Mobilite hizmetinin otomatik olarak güncellemesi

Azure Site Kurtarma, sorunları gidermek ve varolan özellikleri geliştirmek veya yenilerini eklemek için aylık bir sürüm sürümü kullanır. Hizmette güncel kalmak için, her ay yama dağıtımı için plan yapmak gerekir. Her yükseltmeyle ilişkili ek yükü önlemek için, bunun yerine Site Kurtarma'nın bileşen güncelleştirmelerini yönetmesine izin verebilirsiniz.

[Azure'dan Azure'a olağanüstü durum kurtarma mimarisinde](azure-to-azure-architecture.md)de belirtildiği gibi, Etkinleştirme hizmeti çoğaltmanın etkin olduğu tüm Azure sanal makinelerine (VM) yüklenirken, VM'leri bir Azure bölgesinden diğerine çoğaltır. Otomatik güncelleştirmeleri kullandığınızda, her yeni sürüm Mobilite hizmet uzantısını güncelleştirir.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-automatic-updates-work"></a>Otomatik güncelleştirmeler nasıl çalışır?

Güncelleştirmeleri yönetmek için Site Kurtarma'yı kullandığınızda, kasayla aynı abonelikte oluşturulan bir otomasyon hesabı üzerinden genel bir runbook (Azure hizmetleri tarafından kullanılır) dağıtılır. Her kasada bir otomasyon hesabı vardır. Runbook, etkin otomatik güncelleştirmeler için kasadaki her VM'yi denetler ve daha yeni bir sürüm varsa Mobilite hizmet uzantısını yükseltir.

Varsayılan runbook zamanlaması, çoğaltılan VM'nin coğrafi saat diliminde günlük 12:00'de yinelenir. Ayrıca, otomasyon hesabı üzerinden runbook zamanlamasını da değiştirebilirsiniz.

> [!NOTE]
> Güncelleştirme Rollup 35'ten başlayarak, güncelleştirmeler için kullanmak üzere varolan bir otomasyon hesabı seçebilirsiniz. Bu güncelleştirmeden önce, Site Kurtarma varsayılan olarak bu hesabı oluşturdu. Bu seçeneği yalnızca Bir VM için çoğaltmayı etkinleştirdiğinizde seçebileceğinizi unutmayın. Çoğaltma VM için kullanılamaz. Seçtiğiniz ayar, aynı kasada korunan tüm Azure VM'leri için geçerli olacaktır.
 
> Otomatik güncelleştirmeleri açmak Azure VM'lerinizin yeniden başlatılmasını gerektirmez veya devam eden çoğaltmayı etkiler.

> Otomasyon hesabındaki iş faturalandırması, bir ay içinde kullanılan iş çalışma zamanı dakikalarının sayısına bağlıdır. Varsayılan olarak, 500 dakika bir otomasyon hesabı için ücretsiz birimler olarak dahildir. İş yürütme yaklaşık bir dakika her gün birkaç saniye sürer ve ücretsiz birimler olarak kaplıdır.

| Ücretsiz birimler dahil (her ay) | Fiyat |
|---|---|
| İş çalışma süresi 500 dakika | ≥0,14/dakika

## <a name="enable-automatic-updates"></a>Otomatik güncelleştirmeleri etkinleştirme

Site Kurtarma'nın güncelleştirmeleri aşağıdaki şekillerde yönetmesine izin verebilirsiniz.

### <a name="manage-as-part-of-the-enable-replication-step"></a>Etkinleştirme çoğaltma adımının bir parçası olarak yönetme

[VM görünümünden](azure-to-azure-quickstart.md) veya [kurtarma hizmetleri kasasından](azure-to-azure-how-to-enable-replication.md)başlayarak bir VM için çoğaltmayı etkinleştirdiğinizde, Site Kurtarma uzantısı güncelleştirmelerini yönetmesine veya el ile yönetmesine izin verebilirsiniz.

![Uzantı ayarları](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Kasanın içindeki uzantı güncelleştirme ayarlarını değiştirme

1. Kasanın içinde,**Site Kurtarma Altyapısını** **Yönet'e** > gidin.
2. **Azure Sanal Makineler** > **Uzantısı Güncelleştirme Ayarları**için, geçiş yapmak için **Site Kurtarma'ya İzin Ver'i** açın. El ile yönetmek için kapatın. 
3. **Kaydet'i**seçin.

![Uzantı güncelleştirme ayarları](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important]
> **Site Kurtarma'yı yönetmeye izin ver'i**seçtiğinizde, ayar ilgili kasadaki tüm VM'lere uygulanır.


> [!Note]
> Her iki seçenek de güncelleştirmeleri yönetmek için kullanılan otomasyon hesabını size iletilir. Bu özelliği ilk kez bir kasada kullanıyorsanız, varsayılan olarak yeni bir otomasyon hesabı oluşturulur. Alternatif olarak, ayarı özelleştirebilir ve varolan bir otomasyon hesabı seçebilirsiniz. Sonraki tüm etkinleştirme çoğaltmaları aynı tonoz da daha önce oluşturulan bir kullanın. Şu anda açılır yalnızca kasayla aynı Kaynak Grubunda bulunan Otomasyon hesaplarını listeleyecektir.  

> [!IMPORTANT]
> Aşağıdaki komut dosyasının özel bir otomasyon hesabı için bir otomasyon hesabı bağlamında çalıştırılması gerekir, aşağıdaki komut dosyasını kullanın:

```azurepowershell
param(
    [Parameter(Mandatory=$true)]
    [String] $VaultResourceId,

    [Parameter(Mandatory=$true)]
    [ValidateSet("Enabled",'Disabled')]
    [Alias("Enabled or Disabled")]
    [String] $AutoUpdateAction,

    [Parameter(Mandatory=$false)]
    [String] $AutomationAccountArmId
)

$SiteRecoveryRunbookName = "Modify-AutoUpdateForVaultForPatner"
$TaskId = [guid]::NewGuid().ToString()
$SubscriptionId = "00000000-0000-0000-0000-000000000000"
$AsrApiVersion = "2018-01-10"
$RunAsConnectionName = "AzureRunAsConnection"
$ArmEndPoint = "https://management.azure.com"
$AadAuthority = "https://login.windows.net/"
$AadAudience = "https://management.core.windows.net/"
$AzureEnvironment = "AzureCloud"
$Timeout = "160"

function Throw-TerminatingErrorMessage
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    throw ("Message: {0}, TaskId: {1}.") -f $Message, $TaskId
}

function Write-Tracing
{
    Param
    (
        [Parameter(Mandatory=$true)]      
        [ValidateSet("Informational", "Warning", "ErrorLevel", "Succeeded", IgnoreCase = $true)]
        [String]
        $Level,

        [Parameter(Mandatory=$true)]
        [String]
        $Message,

        [Switch]
        $DisplayMessageToUser
    )

    Write-Output $Message

}

function Write-InformationTracing
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    Write-Tracing -Message $Message -Level Informational -DisplayMessageToUser
}

function ValidateInput()
{
    try
    {
        if(!$VaultResourceId.StartsWith("/subscriptions", [System.StringComparison]::OrdinalIgnoreCase))
        {
            $ErrorMessage = "The vault resource id should start with /subscriptions."
            throw $ErrorMessage
        }

        $Tokens = $VaultResourceId.SubString(1).Split("/")
        if(!($Tokens.Count % 2 -eq 0))
        {
            $ErrorMessage = ("Odd Number of tokens: {0}." -f $Tokens.Count)
            throw $ErrorMessage
        }

        if(!($Tokens.Count/2 -eq 4))
        {
            $ErrorMessage = ("Invalid number of resource in vault ARM id expected:4, actual:{0}." -f ($Tokens.Count/2))
            throw $ErrorMessage
        }

        if($AutoUpdateAction -ieq "Enabled" -and [string]::IsNullOrEmpty($AutomationAccountArmId))
        {
            $ErrorMessage = ("The automation account ARM id should not be null or empty when AutoUpdateAction is enabled.")
            throw $ErrorMessage
        }
    }
    catch
    {
        $ErrorMessage = ("ValidateInput failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Initialize-SubscriptionId()
{
    try
    {
        $Tokens = $VaultResourceId.SubString(1).Split("/")

        $Count = 0
        $ArmResources = @{}
        while($Count -lt $Tokens.Count)
        {
            $ArmResources[$Tokens[$Count]] = $Tokens[$Count+1]
            $Count = $Count + 2
        }
        
        return $ArmResources["subscriptions"]
    }
    catch
    {
        Write-Tracing -Level ErrorLevel -Message ("Initialize-SubscriptionId: failed with [Exception: {0}]." -f $_.Exception) -DisplayMessageToUser
        throw
    }
}

function Invoke-InternalRestMethod($Uri, $Headers, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-RestMethod -Uri $Uri -Headers $Headers    
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Invoke-InternalWebRequest($Uri, $Headers, $Method, $Body, $ContentType, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-WebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -UseBasicParsing
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Get-Header([ref]$Header, $AadAudience, $AadAuthority, $RunAsConnectionName){
    try 
    {
        $RunAsConnection = Get-AutomationConnection -Name $RunAsConnectionName
        $TenantId = $RunAsConnection.TenantId
        $ApplicationId = $RunAsConnection.ApplicationId
        $CertificateThumbprint = $RunAsConnection.CertificateThumbprint
        $Path = "cert:\CurrentUser\My\{0}" -f $CertificateThumbprint
        $Secret = Get-ChildItem -Path $Path
        $ClientCredential = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.ClientAssertionCertificate(
                $ApplicationId,
                $Secret)

        # Trim the forward slash from the AadAuthority if it exist.
        $AadAuthority = $AadAuthority.TrimEnd("/")
        $AuthContext = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(
            "{0}/{1}" -f $AadAuthority, $TenantId )
        $AuthenticationResult = $authContext.AcquireToken($AadAudience, $Clientcredential)
        $Header.Value['Content-Type'] = 'application\json'
        $Header.Value['Authorization'] = $AuthenticationResult.CreateAuthorizationHeader()
        $Header.Value["x-ms-client-request-id"] = $TaskId + "/" + (New-Guid).ToString() + "-" + (Get-Date).ToString("u")
    }
    catch
    {
        $ErrorMessage = ("Get-BearerToken: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Get-ProtectionContainerToBeModified([ref] $ContainerMappingList)
{
    try 
    {
        Write-InformationTracing ("Get protection container mappings : {0}." -f $VaultResourceId)
        $ContainerMappingListUrl = $ArmEndPoint + $VaultResourceId + "/replicationProtectionContainerMappings" + "?api-version=" + $AsrApiVersion
        
        Write-InformationTracing ("Getting the bearer token and the header.")
        Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
        
        $Result = @()
        Invoke-InternalRestMethod -Uri $ContainerMappingListUrl -Headers $header -Result ([ref]$Result)
        $ContainerMappings = $Result[0]

        Write-InformationTracing ("Total retrieved container mappings: {0}." -f $ContainerMappings.Value.Count)
        foreach($Mapping in $ContainerMappings.Value)
        {
            if(($Mapping.properties.providerSpecificDetails -eq $null) -or ($Mapping.properties.providerSpecificDetails.instanceType -ine "A2A"))
            {
                Write-InformationTracing ("Mapping properties: {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the provider does not match." -f ($Mapping.Id))
                continue;
            }

            if($Mapping.Properties.State -ine "Paired")
            {
                Write-InformationTracing ("Ignoring container mapping: {0} as the state is not paired." -f ($Mapping.Id))
                continue;
            }

            Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties.providerSpecificDetails))
            $MappingAutoUpdateStatus = $Mapping.properties.providerSpecificDetails.agentAutoUpdateStatus
            $MappingAutomationAccountArmId = $Mapping.properties.providerSpecificDetails.automationAccountArmId
            $MappingHealthErrorCount = $Mapping.properties.HealthErrorDetails.Count

            if($AutoUpdateAction -ieq "Enabled" -and
                ($MappingAutoUpdateStatus -ieq "Enabled") -and
                ($MappingAutomationAccountArmId -ieq $AutomationAccountArmId) -and
                ($MappingHealthErrorCount -eq 0))
            {
                Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the auto update is already enabled and is healthy." -f ($Mapping.Id))
                continue;
            }

            ($ContainerMappingList.Value).Add($Mapping.id)
        }
    }
    catch
    {
        $ErrorMessage = ("Get-ProtectionContainerToBeModified: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

$OperationStartTime = Get-Date
$ContainerMappingList = New-Object System.Collections.Generic.List[System.String]
$JobsInProgressList = @()
$JobsCompletedSuccessList = @()
$JobsCompletedFailedList = @()
$JobsFailedToStart = 0
$JobsTimedOut = 0
$Header = @{}

$AzureRMProfile = Get-Module -ListAvailable -Name AzureRM.Profile | Select Name, Version, Path
$AzureRmProfileModulePath = Split-Path -Parent $AzureRMProfile.Path
Add-Type -Path (Join-Path $AzureRmProfileModulePath "Microsoft.IdentityModel.Clients.ActiveDirectory.dll")

$Inputs = ("Tracing inputs VaultResourceId: {0}, Timeout: {1}, AutoUpdateAction: {2}, AutomationAccountArmId: {3}." -f $VaultResourceId, $Timeout, $AutoUpdateAction, $AutomationAccountArmId)
Write-Tracing -Message $Inputs -Level Informational -DisplayMessageToUser
$CloudConfig = ("Tracing cloud configuration ArmEndPoint: {0}, AadAuthority: {1}, AadAudience: {2}." -f $ArmEndPoint, $AadAuthority, $AadAudience)
Write-Tracing -Message $CloudConfig -Level Informational -DisplayMessageToUser
$AutomationConfig = ("Tracing automation configuration RunAsConnectionName: {0}." -f $RunAsConnectionName)
Write-Tracing -Message $AutomationConfig -Level Informational -DisplayMessageToUser

ValidateInput
$SubscriptionId = Initialize-SubscriptionId
Get-ProtectionContainerToBeModified ([ref]$ContainerMappingList)

$Input = @{
  "properties"= @{
    "providerSpecificInput"= @{
        "instanceType" = "A2A"
        "agentAutoUpdateStatus" = $AutoUpdateAction
        "automationAccountArmId" = $AutomationAccountArmId
    }
  }
}
$InputJson = $Input |  ConvertTo-Json

if ($ContainerMappingList.Count -eq 0)
{
    Write-Tracing -Level Succeeded -Message ("Exiting as there are no container mappings to be modified.") -DisplayMessageToUser
    exit
}

Write-InformationTracing ("Container mappings to be updated has been retrieved with count: {0}." -f $ContainerMappingList.Count)

try
{
    Write-InformationTracing ("Start the modify container mapping jobs.")
    ForEach($Mapping in $ContainerMappingList)
    {
    try {
            $UpdateUrl = $ArmEndPoint + $Mapping + "?api-version=" + $AsrApiVersion
            Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
            
            $Result = @()
            Invoke-InternalWebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -Result ([ref]$Result)
            $Result = $Result[0]

            $JobAsyncUrl = $Result.Headers['Azure-AsyncOperation']
            Write-InformationTracing ("The modify container mapping job invoked with async url: {0}." -f $JobAsyncUrl)
            $JobsInProgressList += $JobAsyncUrl;

            # Rate controlling the set calls to maximum 60 calls per minute.
            # ASR throttling for set calls is 200 in 1 minute.
            Start-Sleep -Milliseconds 1000
        }
        catch{
            Write-InformationTracing ("The modify container mappings job creation failed for: {0}." -f $Ru)
            Write-InformationTracing $_
            $JobsFailedToStart++
        }
    }

    Write-InformationTracing ("Total modify container mappings has been initiated: {0}." -f $JobsInProgressList.Count)
}
catch
{
    $ErrorMessage = ("Modify container mapping jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

try
{
    while($JobsInProgressList.Count -ne 0)
    {
        Sleep -Seconds 30
        $JobsInProgressListInternal = @()
        ForEach($JobAsyncUrl in $JobsInProgressList)
        {
            try
            {
                Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
                $Result = Invoke-RestMethod -Uri $JobAsyncUrl -Headers $header
                $JobState = $Result.Status
                if($JobState -ieq "InProgress")
                {
                    $JobsInProgressListInternal += $JobAsyncUrl
                }
                elseif($JobState -ieq "Succeeded" -or `
                    $JobState -ieq "PartiallySucceeded" -or `
                    $JobState -ieq "CompletedWithInformation")
                {
                    Write-InformationTracing ("Jobs succeeded with state: {0}." -f $JobState)
                    $JobsCompletedSuccessList += $JobAsyncUrl
                }
                else
                {
                    Write-InformationTracing ("Jobs failed with state: {0}." -f $JobState)
                    $JobsCompletedFailedList += $JobAsyncUrl
                }
            }
            catch
            {
                Write-InformationTracing ("The get job failed with: {0}. Ignoring the exception and retrying the next job." -f $_.Exception)

                # The job on which the tracking failed, will be considered in progress and tried again later.
                $JobsInProgressListInternal += $JobAsyncUrl
            }

            # Rate controlling the get calls to maximum 120 calls each minute.
            # ASR throttling for get calls is 10000 in 60 minutes.
            Start-Sleep -Milliseconds 500
        }

        Write-InformationTracing ("Jobs remaining {0}." -f $JobsInProgressListInternal.Count)

        $CurrentTime = Get-Date
        if($CurrentTime -gt $OperationStartTime.AddMinutes($Timeout))
        {
            Write-InformationTracing ("Tracing modify cloud pairing jobs has timed out.")
            $JobsTimedOut = $JobsInProgressListInternal.Count
            $JobsInProgressListInternal = @()
        }

        $JobsInProgressList = $JobsInProgressListInternal
    }
}
catch
{
    $ErrorMessage = ("Tracking modify cloud pairing jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage  -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage 
}

Write-InformationTracing ("Tracking modify cloud pairing jobs completed.")
Write-InformationTracing ("Modify cloud pairing jobs success: {0}." -f $JobsCompletedSuccessList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed: {0}." -f $JobsCompletedFailedList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed to start: {0}." -f $JobsFailedToStart)
Write-InformationTracing ("Modify cloud pairing jobs timedout: {0}." -f $JobsTimedOut)

if($JobsTimedOut -gt  0)
{
    $ErrorMessage = "One or more modify cloud pairing jobs has timedout."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)   
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}
elseif($JobsCompletedSuccessList.Count -ne $ContainerMappingList.Count)
{
    $ErrorMessage = "One or more modify cloud pairing jobs failed."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

Write-Tracing -Level Succeeded -Message ("Modify cloud pairing completed.") -DisplayMessageToUser
```

### <a name="manage-updates-manually"></a>Güncelleştirmeleri el ile yönetme

1. VM'lerinizde yüklü olan Mobilite hizmeti için yeni güncelleştirmeler varsa, aşağıdaki bildirimi görürsünüz: "Yeni Site Kurtarma çoğaltma aracısı güncelleştirmesi kullanılabilir. Yüklemek için tıklayın"

     ![Çoğaltılan öğeler penceresi](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)
2. VM seçim sayfasını açmak için bildirimi seçin.
3. Yükseltmek istediğiniz VM'leri seçin ve ardından **Tamam'ı**seçin. Seçilen her VM için Mobiliteyi Güncelleştir hizmeti başlatılacaktır.

     ![Çoğaltılan öğeler VM listesi](./media/vmware-azure-install-mobility-service/update-okpng.png)


## <a name="common-issues-and-troubleshooting"></a>Sık karşılaşılan sorunlar ve sorun giderme

Otomatik güncelleştirmelerle ilgili bir sorun varsa, kasa panosunda **Yapılandırma sorunları** altında bir hata bildirimi görürsünüz.

Otomatik güncelleştirmeleri etkinleştiremediyseniz, aşağıdaki yaygın hatalara ve önerilen eylemlere bakın:

- **Hata**: Bir Azure Run As hesabı (hizmet sorumlusu) oluşturmak ve Katılımcı rolünü hizmet sorumlusuna vermek için izininiz yoktur.

   **Önerilen eylem**: Oturum açan hesabın Katılımcı olarak atandığından emin olun ve yeniden deneyin. İzin atama hakkında daha fazla bilgi için [kaynaklara erişebilen bir Azure REKLAM uygulaması ve hizmet ilkesi oluşturmak için portalı kullan'daki](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) gerekli izinler bölümüne bakın.
 
   Otomatik güncelleştirmeleri etkinleştirdikten sonra çoğu sorunu gidermek için **Onarım'ı**seçin. Onarım düğmesi kullanılamıyorsa, uzantı güncelleştirme ayarları bölmesinde görüntülenen hata iletisine bakın.

   ![Uzantı güncelleştirme ayarlarında Site Kurtarma hizmeti onarım düğmesi](./media/azure-to-azure-autoupdate/repair.png)

- **Hata**: Çalıştır Hesabının kurtarma hizmetleri kaynağına erişim izni yoktur.

    **Önerilen eylem**: Çalıştır hesabını sil ve sonra [yeniden oluştur.](https://docs.microsoft.com/azure/automation/automation-create-runas-account) Veya, Otomasyon Hesabı Olarak Çalıştır'ın Azure Etkin Dizin uygulamasının kurtarma hizmetleri kaynağına erişebilmesini unutmayın.

- **Hata**: Run As Hesabı bulunamadı. Bunlardan biri silinmiş veya oluşturulmadı - Azure Active Directory Application, Service Principal, Role, Automation Certificate varlık, Otomasyon Bağlantısı varlığı - veya Thumbprint Sertifika ve Bağlantı arasında aynı değildir. 

    **Önerilen eylem**: Çalıştır hesabını sil ve sonra [yeniden oluştur.](https://docs.microsoft.com/azure/automation/automation-create-runas-account)

-  **Hata**: Otomasyon hesabı tarafından kullanılan Sertifika Olarak Azure Çalıştır'ın süresi dolmak üzeredir. 

    Run As hesabı için oluşturulan kendi imzalı sertifikanın süresi, oluşturulma tarihinden itibaren bir yıl sonra sona erer. Sertifikayı süresi dolmadan önce herhangi bir zamanda yenileyebilirsiniz. E-posta bildirimleri için kaydolduysanız, sizin tarafınızdan bir eylem gerektiğinde de e-posta alırsınız. Bu hata, son kullanma tarihinden iki ay önce gösterilir ve sertifikanın süresi dolmuşsa kritik bir hataya dönüşecektir. Sertifikanın süresi dolduktan sonra, siz aynı sertifikayı yenileyene kadar otomatik güncelleştirme işlevsel olmayacaktır.

   **Önerilen eylem**: Bu sorunu çözmek için 'Onarım'a ve ardından 'Sertifika Yenile'ye tıklayın.
    
   ![yenileme-sertifika](media/azure-to-azure-autoupdate/automation-account-renew-runas-certificate.PNG)

> [!NOTE]
> Sertifikayı yeniledikten sonra, geçerli durumun güncellenmesi için lütfen sayfayı yenileyin.
