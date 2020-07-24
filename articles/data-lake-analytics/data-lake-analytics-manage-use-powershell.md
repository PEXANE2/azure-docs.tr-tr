---
title: Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
description: Bu makalede, Data Lake Analytics hesaplarını, veri kaynaklarını, kullanıcıları & işlerini yönetmek için Azure PowerShell nasıl kullanılacağı açıklanır.
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: how-to
ms.date: 06/29/2018
ms.openlocfilehash: 70a251db6c08f353f9c50512c41551e7a909a059
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125658"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure PowerShell kullanarak Azure Data Lake Analytics hesaplarının, veri kaynaklarının, kullanıcıların ve işlerin nasıl yönetileceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell 'i Data Lake Analytics kullanmak için aşağıdaki bilgi parçalarını toplayın: 

* **ABONELIK kimliği**: Data Lake Analytics hesabınızı içeren Azure aboneliğinin kimliği.
* **Kaynak grubu**: Data Lake Analytics hesabınızı içeren Azure Kaynak grubunun adı.
* **Data Lake Analytics hesap adı**: Data Lake Analytics hesabınızın adı.
* **Varsayılan Data Lake Store hesap adı**: her Data Lake Analytics hesabının varsayılan bir Data Lake Store hesabı vardır.
* **Konum**: Data Lake Analytics hesabınızın konumu (örneğin, "Doğu ABD 2" veya desteklenen diğer konumlar).

Bu öğreticideki PowerShell kod parçacıkları, bu bilgileri depolamak için bu değişkenleri kullanır

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Azure'da oturum açma

### <a name="log-in-using-interactive-user-authentication"></a>Etkileşimli kullanıcı kimlik doğrulaması kullanarak oturum açın

Abonelik KIMLIĞI veya abonelik adı ile oturum açın

```powershell
# Using subscription id
Connect-AzAccount -SubscriptionId $subId

# Using subscription name
Connect-AzAccount -SubscriptionName $subname 
```

## <a name="saving-authentication-context"></a>Kimlik doğrulama bağlamı kaydediliyor

`Connect-AzAccount`Cmdlet, her zaman kimlik bilgilerini ister. Aşağıdaki cmdlet 'leri kullanarak sorulmaktan kaçınabilirsiniz:

```powershell
# Save login session information
Save-AzAccounts -Path D:\profile.json  

# Load login session information
Select-AzAccounts -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Hizmet sorumlusu kimliği (SPI) kullanarak oturum açma

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

Her Data Lake Analytics hesabı, günlükleri depolamak için kullandığı varsayılan bir Data Lake Store hesabı gerektirir. Var olan bir hesabı yeniden kullanabilir veya hesap oluşturabilirsiniz. 

```powershell
# Create a data lake store if needed, or you can re-use an existing one
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-account-information"></a>Hesap bilgilerini al

Hesap hakkındaki ayrıntıları alın.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Bir hesabın var olup olmadığını denetle

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme
Azure Data Lake Analytics Şu anda aşağıdaki veri kaynaklarını desteklemektedir:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama](../storage/common/storage-introduction.md)

Her Data Lake Analytics hesabının varsayılan bir Data Lake Store hesabı vardır. Varsayılan Data Lake Store hesabı, iş meta verilerini ve iş denetim günlüklerini depolamak için kullanılır. 

### <a name="find-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabını bulun

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Varsayılan Data Lake Store hesabını, özelliği tarafından veri kaynakları listesini filtreleyerek bulabilirsiniz `IsDefault` :

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

### <a name="list-data-sources"></a>Veri kaynaklarını listeleme

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Account $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL işlerini gönderme

### <a name="submit-a-string-as-a-u-sql-job"></a>Bir U-SQL işi olarak bir dize gönderme

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

### <a name="submit-a-file-as-a-u-sql-job"></a>Bir dosyayı U-SQL işi olarak gönderme

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>İşleri listeleme

Çıktı o anda çalışan işleri ve yakın zamanda tamamlanan işleri içerir.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>İlk N işi listeleyin

Varsayılan olarak, iş listesi gönderme zamanında sıralanır. Yani en son gönderilen işler önce görüntülenir. Varsayılan olarak ADLA hesabı, işleri 180 gün süreyle anımsar, ancak get-AdlJob cmdlet 'i varsayılan olarak yalnızca ilk 500 ' i döndürür. Belirli sayıda işi listelemek için top parametresini kullanın.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>İşleri iş durumuna göre Listele

Parametresini kullanarak `-State` . Şu değerlerden herhangi birini birleştirebilirsiniz:

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

### <a name="list-jobs-by-job-result"></a>İşleri iş sonucuna göre Listele

`-Result`Sonlandırılan işlerin başarıyla tamamlanıp tamamlanmadığını belirlemek için parametresini kullanın. Şu değerlere sahiptir:

* İptal Edildi
* Başarısız
* Yok
* Başarılı

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>İş gönderişine göre işleri listeleme

`-Submitter`Parametresi, kimin bir işi verdiğini belirlemenize yardımcı olur.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>İşleri gönderim zamanına göre Listele

`-SubmittedAfter`Bir zaman aralığına filtrelemede faydalıdır.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>İş durumunu al

Belirli bir işin durumunu alın.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Bir işi iptal etme

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>İşin bitmesini bekle

`Get-AdlAnalyticsJob`İş bitene kadar yinelemek yerine, `Wait-AdlJob` işin bitmesini beklemek için cmdlet 'ini kullanabilirsiniz.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>İş geçmişi çözümleniyor

Data Lake Analytics 'te çalıştırılan işlerin geçmişini çözümlemek için Azure PowerShell kullanmak güçlü bir tekniktir. Kullanım ve maliyetle ilgili Öngörüler elde etmek için bunu kullanabilirsiniz. [Iş geçmişi Analizi örnek deposu](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis) ' na bakarak daha fazla bilgi edinebilirsiniz  

## <a name="list-job-pipelines-and-recurrences"></a>İş işlem hatlarını ve tekrarları listeleme

`Get-AdlJobPipeline`Daha önce gönderilen iş hattı bilgilerini görmek için cmdlet 'ini kullanın.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

`Get-AdlJobRecurrence`Daha önce gönderilen işlerin yinelenme bilgilerini görmek için cmdlet 'ini kullanın.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

### <a name="list-existing-compute-policies"></a>Mevcut işlem ilkelerini listeleyin

`Get-AdlAnalyticsComputePolicy`Cmdlet 'i bir Data Lake Analytics hesabının işlem ilkeleri hakkında bilgi alır.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>İşlem ilkesi oluşturma

`New-AdlAnalyticsComputePolicy`Cmdlet 'i bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur. Bu örnek, belirtilen kullanıcının kullanabileceği maksimum au 'yı 50 olarak, en düşük iş önceliğini 250 olarak ayarlar.

```powershell
$userObjectId = (Get-AzAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Dosyaları yönetme

### <a name="check-for-the-existence-of-a-file"></a>Bir dosyanın varlığını denetleyin.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Karşıya yükleme ve indirme

Karşıya bir dosya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Tüm klasörü yinelemeli olarak karşıya yükleyin.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Bir dosya indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Tüm klasörü yinelemeli olarak indirin.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Karşıya yükleme veya indirme işlemi kesintiye uğrarsa, bayrağı ile cmdlet 'i yeniden çalıştırarak işlemi sürdürmeyi deneyebilirsiniz ``-Resume`` .

## <a name="manage-the-u-sql-catalog"></a>U-SQL kataloğunu yönetme

U-SQL kataloğu, U-SQL betikleri tarafından paylaşılabilmesi için verileri ve kodu yapılandırmak üzere kullanılır. Katalog, Azure Data Lake veri ile mümkün olan en yüksek performansı sunar. Daha fazla bilgi için bkz. [U-SQL kataloğunu kullanma](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>U-SQL kataloğundaki öğeleri Listeleme

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Tüm derlemeleri bir U-SQL kataloğu ile listeleme

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

### <a name="get-details-about-a-catalog-item"></a>Katalog öğesiyle ilgili ayrıntıları alın

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Kimlik bilgilerini katalogda depolayın

Bir U-SQL veritabanı içinde, Azure 'da barındırılan bir veritabanı için bir kimlik bilgisi nesnesi oluşturun. Şu anda, U-SQL kimlik bilgileri PowerShell aracılığıyla oluşturabileceğiniz tek tür Katalog öğesidir.

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

### <a name="list-firewall-rules"></a>Güvenlik duvarı kurallarını Listele

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

### <a name="modify-a-firewall-rule"></a>Bir güvenlik duvarı kuralını değiştirme

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="remove-a-firewall-rule"></a>Bir güvenlik duvarı kuralını kaldır

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="allow-azure-ip-addresses"></a>Azure IP adreslerine izin ver

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Azure ile çalışma

### <a name="get-error-details"></a>Hata ayrıntılarını al

```powershell
Resolve-AzError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Windows makinenizde yönetici olarak çalıştırıp çalıştırmediğinizi doğrulayın

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Tenantıd bulma

Bir abonelik adından:

```powershell
function Get-TenantIdFromSubscriptionName( [string] $subname )
{
    $sub = (Get-AzSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubscriptionName "ADLTrainingMS"
```

Bir abonelik KIMLIĞINDEN:

```powershell
function Get-TenantIdFromSubscriptionId( [string] $subid )
{
    $sub = (Get-AzSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubscriptionId $subid
```

"Contoso.com" gibi bir etki alanı adresinden

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Tüm aboneliklerinizi ve kiracı kimliklerini listeleyin

```powershell
$subs = Get-AzSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Şablon kullanarak Data Lake Analytics hesabı oluşturma

Aşağıdaki örneği kullanarak bir Azure Kaynak grubu şablonu da kullanabilirsiniz: [şablon kullanarak Data Lake Analytics hesabı oluşturma](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portal](data-lake-analytics-get-started-portal.md)  |  [Azure PowerShell](data-lake-analytics-get-started-powershell.md)  |  [Azure CLI](data-lake-analytics-get-started-cli.md) Azure PowerShell Azure Portal kullanarak Data Lake Analytics kullanmaya başlama
* Azure Data Lake Analytics [Azure Portal](data-lake-analytics-manage-use-portal.md)  |  [Azure PowerShell](data-lake-analytics-manage-use-powershell.md)  |  [CLI](data-lake-analytics-manage-use-cli.md) kullanarak yönetme 
