---
title: Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
description: Bu makalede, Veri Gölü Analytics hesaplarını, veri kaynaklarını, kullanıcıları & işleri yönetmek için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/29/2018
ms.openlocfilehash: 4273828c9c2bdb75fcbc1de45da55c5a03dd615f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66156414"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure PowerShell'i kullanarak Azure Veri Gölü Analizi hesaplarının, veri kaynaklarının, kullanıcıların ve işlerin nasıl yönetilenbir şekilde yönetilen anlatılmaktadır.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i Data Lake Analytics ile kullanmak için aşağıdaki bilgileri toplayın: 

* **Abonelik Kimliği**: Veri Gölü Analizi hesabınızı içeren Azure aboneliğinin kimliği.
* **Kaynak grubu**: Veri Gölü Analizi hesabınızı içeren Azure kaynak grubunun adı.
* **Data Lake Analytics hesap adı**: Data Lake Analytics hesabınızın adı.
* **Varsayılan Veri Gölü Deposu hesap adı**: Her Veri Gölü Analizi hesabının varsayılan bir Data Lake Store hesabı vardır.
* **Konum**: "Doğu ABD 2" veya desteklenen diğer konumlar gibi Data Lake Analytics hesabınızın konumu.

Bu öğreticideki PowerShell kod parçacıkları, bu bilgileri depolamak için bu değişkenleri kullanır

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma

### <a name="log-in-using-interactive-user-authentication"></a>Etkileşimli kullanıcı kimlik doğrulamasını kullanarak oturum açma

Abonelik kimliği kullanarak veya abonelik adına göre giriş yapın

```powershell
# Using subscription id
Connect-AzAccount -SubscriptionId $subId

# Using subscription name
Connect-AzAccount -SubscriptionName $subname 
```

## <a name="saving-authentication-context"></a>Kimlik doğrulama bağlamında kaydetme

Cmdlet `Connect-AzAccount` her zaman kimlik bilgilerini ister. Aşağıdaki cmdlets kullanarak istenir kaçınabilirsiniz:

```powershell
# Save login session information
Save-AzAccounts -Path D:\profile.json  

# Load login session information
Select-AzAccounts -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Hizmet Temel Kimliği (SPI) kullanarak giriş yapın

```powershell
$tenantid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  
$spi_appname = "appname" 
$spi_appid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
$spi_secret = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

$pscredential = New-Object System.Management.Automation.PSCredential ($spi_appid, (ConvertTo-SecureString $spi_secret -AsPlainText -Force))
Login-AzAccount -ServicePrincipal -TenantId $tenantid -Credential $pscredential -Subscription $subid
```

## <a name="manage-accounts"></a>Hesapları yönetme


### <a name="list-accounts"></a>Hesapları listele

```powershell
# List Data Lake Analytics accounts within the current subscription.
Get-AdlAnalyticsAccount

# List Data Lake Analytics accounts within a specific resource group.
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

### <a name="create-an-account"></a>Hesap oluşturma

Her Data Lake Analytics hesabı, günlükleri depolamak için kullandığı varsayılan bir Data Lake Store hesabı gerektirir. Varolan bir hesabı yeniden kullanabilir veya hesap oluşturabilirsiniz. 

```powershell
# Create a data lake store if needed, or you can re-use an existing one
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-account-information"></a>Hesap bilgilerini alma

Bir hesap hakkında ayrıntılı bilgi alın.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Bir hesap olup olmadığını denetleme

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme
Azure Veri Gölü Analizi şu anda aşağıdaki veri kaynaklarını destekler:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Her Data Lake Analytics hesabının varsayılan bir Data Lake Store hesabı vardır. Varsayılan Veri Gölü Deposu hesabı, iş meta verilerini ve iş denetim günlüklerini depolamak için kullanılır. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Veri Gölü Deposu hesabını bulma

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Özelliğe göre `IsDefault` veri kaynakları listesini filtreleyerek varsayılan Veri Gölü Deposu hesabını bulabilirsiniz:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Veri kaynağı ekleme

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Veri kaynaklarını listele

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Account $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL işlerini gönderme

### <a name="submit-a-string-as-a-u-sql-job"></a>Bir dizeyi U-SQL işi olarak gönderme

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```

### <a name="submit-a-file-as-a-u-sql-job"></a>Dosyayı U-SQL işi olarak gönderme

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>İşleri listele

Çıktı o anda çalışan işleri ve yakın zamanda tamamlanan işleri içerir.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>En iyi N işleri listele

Varsayılan olarak, iş listesi teslim süresine göre sıralanır. Bu yüzden en son gönderilen işler ilk görünür. Varsayılan olarak, ADLA hesabı işleri 180 gün boyunca hatırlar, ancak Get-AdlJob cmdlet varsayılan olarak yalnızca ilk 500'e döndürür. Belirli sayıda işi listelemek için -Üst parametreyi kullanın.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>İşleri iş durumuna göre listelama

Parametreyi `-State` kullanarak. Bu değerlerden herhangi birini birleştirebilirsiniz:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

### <a name="list-jobs-by-job-result"></a>İşleri iş sonucuna göre listele

Tamamlanan `-Result` işlerin başarıyla tamamlanıp tamamlanmadığını algılamak için parametreyi kullanın. Bu değerlere sahiptir:

* İptal Edildi
* Başarısız
* None
* Başarılı oldu

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>İş gönderene göre işleri listele

Parametre, `-Submitter` bir işi kimin gönderdiğini belirlemenize yardımcı olur.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>İşlerini teslim süresine göre listeletme

Bir `-SubmittedAfter` zaman aralığına filtreleme yararlıdır.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>İş durumunu alma

Belirli bir işin durumunu alın.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Bir işi iptal etme

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>Bir işin bitmesini bekleyin

Bir iş `Get-AdlAnalyticsJob` bitene kadar yinelemek yerine, `Wait-AdlJob` işin bitmesini beklemek için cmdlet kullanabilirsiniz.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>İş geçmişini analiz etme

Veri Gölü analitiğinde çalışan işlerin geçmişini analiz etmek için Azure PowerShell'i kullanmak güçlü bir tekniktir. Kullanım ve maliyet hakkında bilgi edinmek için kullanabilirsiniz. İş Geçmişi Analizi örnek [repo'ya](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis) bakarak daha fazla bilgi edinebilirsiniz  

## <a name="list-job-pipelines-and-recurrences"></a>İş ardışık lıkları ve yinelemeleri listele

Daha `Get-AdlJobPipeline` önce gönderilen işlerden alınan ardışık hat lar bilgisini görmek için cmdlet'i kullanın.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Daha `Get-AdlJobRecurrence` önce gönderilen işlerin yineleme bilgilerini görmek için cmdlet'i kullanın.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>Bilgi işlem ilkelerini yönetme

### <a name="list-existing-compute-policies"></a>Varolan bilgi işlem ilkelerini listele

Cmdlet, `Get-AdlAnalyticsComputePolicy` Bir Data Lake Analytics hesabının bilgi işlem ilkeleri yle ilgili bilgileri alır.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>İşlem ilkesi oluşturma

Cmdlet, `New-AdlAnalyticsComputePolicy` Veri Gölü Analytics hesabı için yeni bir bilgi işlem ilkesi oluşturur. Bu örnek, belirtilen kullanıcı için kullanılabilen maksimum A'yi 50'ye, minimum iş önceliğini ise 250 olarak ayarlar.

```powershell
$userObjectId = (Get-AzAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Dosyaları yönetme

### <a name="check-for-the-existence-of-a-file"></a>Bir dosyanın varlığını denetleyin.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Yükleme ve indirme

Bir dosya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Tüm klasörü özyinelemeli olarak yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Bir dosya indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Tüm klasörü özyinelemeyle indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Yükleme veya indirme işlemi kesilirse, cmdlet'i bayrakla yeniden çalıştırarak işlemi sürdürmeyi ``-Resume`` deneyebilirsiniz.

## <a name="manage-the-u-sql-catalog"></a>U-SQL kataloğunu yönetme

U-SQL kataloğu, verileri ve kodu, U-SQL komut dosyaları tarafından paylaşılabilmeleri için yapılandırmak için kullanılır. Katalog, Azure Veri Gölü'ndeki verilerle mümkün olan en yüksek performansı sağlar. Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>Öğeleri U-SQL kataloğunda listele

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Tüm derlemeleri U-SQL kataloğunda listele

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Katalog öğesi hakkında ayrıntılı bilgi alın

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Kimlik bilgilerini katalogda depolama

U-SQL veritabanında, Azure'da barındırılan bir veritabanı için bir kimlik bilgisi nesnesi oluşturun. Şu anda, U-SQL kimlik bilgileri PowerShell aracılığıyla oluşturabileceğiniz tek katalog öğesi türüdür.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

## <a name="manage-firewall-rules"></a>Güvenlik duvarı kurallarını yönetme

### <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını listele

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

### <a name="add-a-firewall-rule"></a>Güvenlik duvarı kuralı ekleme

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="modify-a-firewall-rule"></a>Güvenlik duvarı kuralını değiştirme

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="remove-a-firewall-rule"></a>Güvenlik duvarı kuralını kaldırma

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="allow-azure-ip-addresses"></a>Azure IP adreslerine izin verme

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Azure ile çalışma

### <a name="get-error-details"></a>Hata ayrıntıları alma

```powershell
Resolve-AzError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Windows makinenizde Yönetici olarak çalışıyorsanız doğrulama

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Kiracı Kimliği bul

Abonelik adından:

```powershell
function Get-TenantIdFromSubscriptionName( [string] $subname )
{
    $sub = (Get-AzSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubscriptionName "ADLTrainingMS"
```

Abonelik kimliğinden:

```powershell
function Get-TenantIdFromSubscriptionId( [string] $subid )
{
    $sub = (Get-AzSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubscriptionId $subid
```

"contoso.com" gibi bir etki alanı adresinden

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Tüm aboneliklerinizi ve kiracı bilgilerinizi listele

```powershell
$subs = Get-AzSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Şablon kullanarak Veri Gölü Analizi hesabı oluşturma

Aşağıdaki örneği kullanarak bir Azure Kaynak Grubu [şablonu](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template) da kullanabilirsiniz: Şablon kullanarak Veri Gölü Analizi hesabı oluşturun

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portalını](data-lake-analytics-get-started-portal.md) | kullanarak Data Lake Analytics ile başlayın[Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [Azure CLI](data-lake-analytics-get-started-cli.md)
* [Azure portalı](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI'yi](data-lake-analytics-manage-use-cli.md) kullanarak Azure Veri Gölü Analizi'ni yönetme 
