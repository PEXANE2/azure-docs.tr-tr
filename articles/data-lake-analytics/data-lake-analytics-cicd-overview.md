---
title: Azure Data Lake Analytics için CI/CD işlem hattı ayarlama
description: Azure Data Lake Analytics için sürekli tümleştirmeyi ve sürekli dağıtımı ayarlamayı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 09/14/2018
ms.openlocfilehash: b035be727df2dfecb613da79681affd740c69bec
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "60333885"
---
# <a name="how-to-set-up-a-cicd-pipeline-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için CI/CD işlem hattı ayarlama  

Bu makalede, U-SQL işleri ve U-SQL veritabanları için bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı ayarlamayı öğreneceksiniz.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-cicd-for-u-sql-jobs"></a>U-SQL işleri için CI/CD kullanma

Visual Studio için Azure Data Lake Araçları, U-SQL betiklerini düzenlemenize yardımcı olan U-SQL proje türünü sağlar. U-SQL projenizi kullanarak U-SQL kodunuzu yönetmek için başka bir CI/CD senaryosu daha kolay olur.

## <a name="build-a-u-sql-project"></a>U-SQL projesi oluşturma

Bir U-SQL projesi, karşılık gelen parametreler geçirerek Microsoft Build Engine (MSBuild) ile oluşturulabilir. U-SQL projesi için bir yapı işlemi oluşturmak için bu makaledeki adımları izleyin.

### <a name="project-migration"></a>Proje geçişi

U-SQL projesi için bir yapı görevi ayarlamadan önce, U-SQL projesinin en son sürümüne sahip olduğunuzdan emin olun. Düzenleyicinizde U-SQL proje dosyasını açın ve şu içeri aktarma öğelerine sahip olduğunuzu doğrulayın:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Aksi takdirde, projeyi geçirmek için iki seçeneğiniz vardır:

- Seçenek 1: eski içeri aktarma öğesini bir önceki ile değiştirin.
- 2. seçenek: Visual Studio için Azure Data Lake Araçları eski projeyi açın. 2.3.3000.0 'den daha yeni bir sürüm kullanın. Eski proje şablonu otomatik olarak en yeni sürüme yükseltilir. 2.3.3000.0 'den daha yeni sürümlerle oluşturulan yeni projeler yeni şablonu kullanır.

### <a name="get-nuget"></a>NuGet 'i al

MSBuild, U-SQL projeleri için yerleşik destek sağlamaz. Bu desteği almak için, gerekli dil hizmetini ekleyen [Microsoft. Azure. DataLake. USQL. SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet paketine çözümünüz için bir başvuru eklemeniz gerekir.

NuGet paket başvurusunu eklemek için, Visual Studio Çözüm Gezgini çözüme sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. Ya da çözüm klasöründe adlı `packages.config` bir dosya ekleyebilir ve aşağıdaki içeriği bu klasöre koyabilirsiniz:

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL veritabanı başvurularını yönetme

U-SQL projesindeki u-SQL betikleri U-SQL veritabanı nesneleri için sorgu deyimlerine sahip olabilir. Bu durumda, U-SQL projesini oluşturmadan önce nesneler tanımını içeren karşılık gelen U-SQL veritabanı projesine başvurmanız gerekir. Bir U-SQL tablosunu sorgulayıp bir derlemeye başvuru yaptığınızda örnek bir örnektir. 

[U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
>DROP deyimleri kaza silme sorununa neden olabilir. DROP deyiminizi etkinleştirmek için MSBuild bağımsız değişkenlerini açıkça belirtmeniz gerekir. **Allowdropdeyimin** bırakma derlemesi ve bırakma tablosu değerli işlevi gibi verilerle ılgılı olmayan bırakma işlemi etkinleştirilir. **Allowdatadropdeyimin** bırakma tablosu ve bırakma şeması gibi veri Ile ilgili bırakma işlemi etkinleştirilir. Allowdatadropdeyimin kullanılmadan önce Allowdropdeyiminizi etkinleştirmeniz gerekir.
>

### <a name="build-a-u-sql-project-with-the-msbuild-command-line"></a>MSBuild komut satırı ile bir U-SQL projesi oluşturma

Önce projeyi geçirin ve NuGet paketini alın. Ardından, U-SQL projenizi derlemek için aşağıdaki ek bağımsız değişkenlerle standart MSBuild komut satırını çağırın: 

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

Bağımsız değişkenler tanımı ve değerleri aşağıdaki gibidir:

* **Ussqlsdkpath =\<U-SQL NuGet paketi> \build\runtime**. Bu parametre, U-SQL dil hizmeti için NuGet paketinin yükleme yolunu ifade eder.
* **Ussqltargettype = Merge veya SyntaxCheck**:
    * **Birleştirme**. Birleştirme modu arka plan kod dosyalarını derler. Örnekler **. cs**, **. Kopyala**ve **. r** dosyalarıdır. Bu, sonuç Kullanıcı tanımlı kod kitaplığını U-SQL betiğine göre satır içine alabilir. Örnek olarak DLL ikili, Python veya R kodudur.
    * **SyntaxCheck**. SyntaxCheck modu, ilk olarak arka plan kod dosyalarını U-SQL betiğine birleştirir. Ardından, kodunuzu doğrulamak için U-SQL betiğini derler.
* **Dataroot =\<dataroot yol>**. DataRoot yalnızca SyntaxCheck modu için gereklidir. SyntaxCheck modu ile betiği oluşturduğunda MSBuild, betikteki veritabanı nesnelerine başvuruları denetler. Oluşturmadan önce, derleme makinesinin DataRoot klasöründeki U-SQL veritabanından başvurulan nesneleri içeren, eşleşen bir yerel ortam ayarlayın. Ayrıca, bu veritabanı bağımlılıklarını [U-SQL veritabanı projesine başvurarak](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)yönetebilirsiniz. MSBuild, dosyaları değil yalnızca veritabanı nesnesi başvurularını denetler.
* **Enabledeployment = true** veya **false**. EnableDeployment, derleme işlemi sırasında başvurulan U-SQL veritabanlarını dağıtmaya izin verilip verilmediğini gösterir. Bir U-SQL veritabanı projesine başvuruda bulunmak ve U-SQL betiğinizdeki veritabanı nesnelerini kullanacaksanız, bu parametreyi **true**olarak ayarlayın.

### <a name="continuous-integration-through-azure-pipelines"></a>Azure Pipelines aracılığıyla sürekli tümleştirme

Komut satırına ek olarak, Azure Pipelines ' de U-SQL projeleri oluşturmak için Visual Studio Build veya MSBuild görevini de kullanabilirsiniz. Derleme işlem hattını ayarlamak için derleme ardışık düzenine iki görev eklediğinizden emin olun: bir NuGet geri yükleme görevi ve MSBuild görevi.

![U-SQL projesi için MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

1.  MSBuild 'in U-SQL dil hedeflerini bulabilmesi için, çözümü bulunan NuGet paketini içeren `Azure.DataLake.USQL.SDK`çözüme başvurulu bir NuGet geri yükleme görevi ekleyin. Doğrudan adım 2 ' de `$(Build.SourcesDirectory)/packages` MSBuild bağımsız değişkenleri örneğini kullanmak istiyorsanız, **Gelişmiş** > **hedef dizinini** olarak ayarlayın.

    ![U-SQL projesi için NuGet geri yükleme görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  MSBuild bağımsız değişkenlerini Visual Studio derleme araçları 'nda veya bir MSBuild görevinde aşağıdaki örnekte gösterildiği gibi ayarlayın. Ya da Azure Pipelines yapı ardışık düzeninde bu bağımsız değişkenler için değişkenler tanımlayabilirsiniz.

    ![U-SQL projesi için CI/CD MSBuild değişkenlerini tanımlama](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

### <a name="u-sql-project-build-output"></a>U-SQL proje derlemesi çıkışı

Bir derlemeyi çalıştırdıktan sonra, U-SQL projesindeki tüm betikler oluşturulur ve adlı `USQLProjectName.usqlpack`bir ZIP dosyasına çıktı. Projenizdeki klasör yapısı daraltılmış derleme çıkışında tutulur.

> [!NOTE]
>
> Her U-SQL betiği için arka plan kod dosyaları, betik oluşturma çıktısına bir satır içi deyim olarak birleştirilir.
>

## <a name="test-u-sql-scripts"></a>U-SQL betiklerini test etme

Azure Data Lake U-SQL betikleri ve C# UDO/UDAG/UDF için test projeleri sağlar:
* [U-SQL betikleri ve genişletilmiş C# kodu için test çalışmalarının nasıl ekleneceğini](data-lake-analytics-cicd-test.md#test-u-sql-scripts)öğrenin.
* [Azure pipelines ' de test çalışmalarını çalıştırmayı](data-lake-analytics-cicd-test.md#run-test-cases-in-azure-devops)öğrenin.

## <a name="deploy-a-u-sql-job"></a>U-SQL işi dağıtma

Derleme ve test süreci aracılığıyla kodu doğruladıktan sonra, Azure PowerShell bir görev aracılığıyla U-SQL işlerini doğrudan Azure Pipelines gönderebilirsiniz. Ayrıca, betiği Azure Data Lake Store veya Azure Blob depolama alanına dağıtabilir ve [Azure Data Factory aracılığıyla zamanlanmış işleri çalıştırabilirsiniz](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-azure-pipelines"></a>U-SQL işlerini Azure Pipelines aracılığıyla gönderme

U-SQL projesinin derleme çıktısı **Usguprojectname. ustopack**adlı bir zip dosyasıdır. ZIP dosyası, projedeki tüm U-SQL betiklerini içerir. U-SQL işlerini doğrudan Azure Pipelines göndermek için aşağıdaki örnek PowerShell betiği ile işlem hatlarında [Azure PowerShell görevini](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) kullanabilirsiniz.

```powershell
<#
    This script can be used to submit U-SQL Jobs with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and submit all scripts one-by-one.

    Note: the code behind file for each U-SQL script will be merged into the built U-SQL script in build output.
          
    Example :
        USQLJobSubmission.ps1 -ADLAAccountName "myadlaaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\" -DegreeOfParallelism 2
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLAAccountName, # ADLA account name to submit U-SQL jobs
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

## Get U-SQL scripts in U-SQL project build output(.usqlpack file)
Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue

    return $USQLFiles
}

## Submit U-SQL scripts to ADLA account one-by-one
Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."

    # Submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzDataLakeAnalyticsJob -Account $ADLAAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        
        LogJobInformation $jobToSubmit
        
        Write-Output "Waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzDataLakeAnalyticsJob -Account $ADLAAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{
    Write-Output ([string]::Format("ADLA account: {0}", $ADLAAccountName))
    Write-Output ([string]::Format("Root folde for usqlpack: {0}", $ArtifactsRoot))
    Write-Output ([string]::Format("AU count: {0}", $DegreeOfParallelism))

    Write-Output "Starting USQL script deployment..."
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>U-SQL işlerini Azure Data Factory aracılığıyla dağıtma

U-SQL işlerini doğrudan Azure Pipelines gönderebilirsiniz. Ya da Azure Data Lake Store veya Azure Blob depolama alanına oluşturulmuş betikleri karşıya yükleyebilir ve [Azure Data Factory aracılığıyla zamanlanmış işleri çalıştırabilirsiniz](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

U-SQL betiklerini bir Azure Data Lake Store hesabına yüklemek için aşağıdaki örnek PowerShell betiği ile Azure Pipelines [Azure PowerShell görevini](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) kullanın:

```powershell
<#
    This script can be used to upload U-SQL files to ADLS with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and upload all scripts to ADLS one-by-one.
          
    Example :
        FileUpload.ps1 -ADLSName "myadlsaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\"
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLSName, # ADLS account name to upload U-SQL scripts
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DestinationFolder = "USQLScriptSource" # Destination folder in ADLS
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles

    $files = @(get-childitem $usqlScripts -recurse)
    foreach($file in $files)
    {
        Write-Host "Uploading file: $($file.Name)"
        Import-AzDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/$(Join-Path $DestinationFolder $file)" -Force
    }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue
}

UploadResources
```

## <a name="cicd-for-a-u-sql-database"></a>U-SQL veritabanı için CI/CD

Visual Studio için Azure Data Lake Araçları, U-SQL veritabanlarını geliştirmenize, yönetmenize ve dağıtmanıza yardımcı olan U-SQL veritabanı proje şablonları sağlar. [U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md)hakkında daha fazla bilgi edinin.

## <a name="build-u-sql-database-project"></a>U-SQL veritabanı projesi oluştur

### <a name="get-the-nuget-package"></a>NuGet paketini al

MSBuild, U-SQL veritabanı projeleri için yerleşik destek sağlamaz. Bu özelliği almak için, gerekli dil hizmetini ekleyen [Microsoft. Azure. DataLake. USQL. SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet paketine çözümünüz için bir başvuru eklemeniz gerekir.

NuGet paket başvurusunu eklemek için Visual Studio Çözüm Gezgini çözüme sağ tıklayın. **NuGet Paketlerini Yönet**' i seçin. Ardından NuGet paketini arayın ve yükler. Ya da çözüm klasörüne **Packages. config** adlı bir dosya ekleyebilir ve aşağıdaki içeriği bu klasöre koyabilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-a-database-project-with-the-msbuild-command-line"></a>MSBuild komut satırı ile U-SQL a veritabanı projesi derleme

U-SQL veritabanı projenizi derlemek için standart MSBuild komut satırını çağırın ve U-SQL SDK NuGet paketi başvurusunu ek bir bağımsız değişken olarak geçirin. Aşağıdaki örneğe bakın: 

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Bağımsız değişkeni `USQLSDKPath=<U-SQL Nuget package>\build\runtime` , U-SQL dil hizmeti için NuGet paketinin install yolunu ifade eder.

### <a name="continuous-integration-with-azure-pipelines"></a>Azure Pipelines ile sürekli tümleştirme

Komut satırına ek olarak, Azure Pipelines ' de U-SQL veritabanı projelerini derlemek için Visual Studio Build veya MSBuild görevi kullanabilirsiniz. Bir yapı görevi ayarlamak için derleme ardışık düzenine iki görev eklediğinizden emin olun: bir NuGet geri yükleme görevi ve MSBuild görevi.

   ![U-SQL projesi için CI/CD MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 


1. MSBuild 'in U-SQL dil hedeflerini bulabileceği şekilde, Çözümle başvurulan NuGet paketini içeren `Azure.DataLake.USQL.SDK`bir NuGet geri yükleme görevi ekleyin. Doğrudan adım 2 ' de `$(Build.SourcesDirectory)/packages` MSBuild bağımsız değişkenleri örneğini kullanmak istiyorsanız, **Gelişmiş** > **hedef dizinini** olarak ayarlayın.

   ![U-SQL projesi için CI/CD NuGet görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2. MSBuild bağımsız değişkenlerini Visual Studio derleme araçları 'nda veya bir MSBuild görevinde aşağıdaki örnekte gösterildiği gibi ayarlayın. Ya da Azure Pipelines yapı ardışık düzeninde bu bağımsız değişkenler için değişkenler tanımlayabilirsiniz.

   ![U-SQL veritabanı projesi için CI/CD MSBuild değişkenlerini tanımlama](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

   ```
   /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
   ```
 
### <a name="u-sql-database-project-build-output"></a>U-SQL veritabanı proje derlemesi çıkışı

U-SQL veritabanı projesi için derleme çıktısı, son ek `.usqldbpack`ile adlandırılmış bir u-SQL veritabanı dağıtım paketidir. `.usqldbpack` Paket, bir DDL klasöründeki tek bir U-SQL betiğinin tüm DDL deyimlerini içeren bir zip dosyasıdır. Geçici bir klasördeki derleme için tüm **. dll 'leri** ve ek dosyaları içerir.

## <a name="test-table-valued-functions-and-stored-procedures"></a>Tablo değerli işlevleri ve saklı yordamları test edin

Tablo değerli işlevler ve saklı yordamlar için test çalışmalarını doğrudan eklemek Şu anda desteklenmemektedir. Geçici bir çözüm olarak, bu işlevleri çağıran ve bunlara yönelik test çalışmalarını yazan U-SQL betikleri olan bir U-SQL projesi oluşturabilirsiniz. Tablo değerli işlevler ve U-SQL veritabanı projesinde tanımlanan saklı yordamlar için test çalışmalarını ayarlamak üzere aşağıdaki adımları uygulayın:

1.  Test amaçları için bir U-SQL projesi oluşturun ve tablo değerli işlevleri ve saklı yordamları çağıran U-SQL betikleri yazın.
2.  U-SQL projesine bir veritabanı başvurusu ekleyin. Tablo değerli işlev ve saklı yordam tanımını almak için DDL ifadesini içeren veritabanı projesine başvurmanız gerekir. [Veritabanı başvuruları](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)hakkında daha fazla bilgi edinin.
3.  Tablo değerli işlevleri ve saklı yordamları çağıran U-SQL betikleri için test çalışmaları ekleyin. [U-SQL betikleri için test çalışmalarının nasıl ekleneceğini](data-lake-analytics-cicd-test.md#test-u-sql-scripts)öğrenin.

## <a name="deploy-u-sql-database-through-azure-pipelines"></a>U-SQL veritabanını Azure Pipelines aracılığıyla dağıtma

`PackageDeploymentTool.exe`U-SQL veritabanı dağıtım paketleri dağıtmaya yardımcı olan programlama ve komut satırı arabirimlerini sağlar **. ustabdbpack**. SDK, **Build/Runtime/PackageDeploymentTool. exe**dosyasında bulunan [U-SQL SDK NuGet paketine](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)dahildir. Kullanarak `PackageDeploymentTool.exe`u-SQL veritabanlarını hem Azure Data Lake Analytics hem de yerel hesaplara dağıtabilirsiniz.

> [!NOTE]
>
> U-SQL veritabanı dağıtımı için PowerShell komut satırı desteği ve Azure Pipelines sürüm görevi desteği şu anda beklemede.
>

Azure Pipelines bir veritabanı dağıtım görevi ayarlamak için aşağıdaki adımları uygulayın:

1. Bir derleme veya sürüm ardışık düzenine bir PowerShell betik görevi ekleyin ve aşağıdaki PowerShell betiğini yürütün. Bu görev, ve `PackageDeploymentTool.exe` `PackageDeploymentTool.exe`için Azure SDK bağımlılıklarını almaya yardımcı olur. **-Azuresdk** ve **-dbdeploymenttool** parametrelerini, bağımlılıklar ve dağıtım aracını belirli klasörlere yüklemek için ayarlayabilirsiniz. **-Azuresdk** yolunu adım 2 ' `PackageDeploymentTool.exe` de **-azuresdkpath** parametresi olarak geçirin. 

    ```powershell
    <#
        This script is used for getting dependencies and SDKs for U-SQL database deployment.
        PowerShell command line support for deploying U-SQL database package(.usqldbpack file) will come soon.
        
        Example :
            GetUSQLDBDeploymentSDK.ps1 -AzureSDK "AzureSDKFolderPath" -DBDeploymentTool "DBDeploymentToolFolderPath"
    #>

    param (
        [string]$AzureSDK = "AzureSDK", # Folder to cache Azure SDK dependencies
        [string]$DBDeploymentTool = "DBDeploymentTool", # Folder to cache U-SQL database deployment tool
        [string]$workingfolder = "" # Folder to execute these command lines
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.5.1-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.4.1-preview -outf Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip
    iwr https://www.nuget.org/api/v2/package/Newtonsoft.Json/6.0.8 -outf Newtonsoft.Json.6.0.8.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.DataLake.USQL.SDK/ -outf USQLSDK.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.4.1-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force
    Expand-Archive Newtonsoft.Json.6.0.8.zip -DestinationPath Newtonsoft.Json.6.0.8 -Force
    Expand-Archive USQLSDK.zip -DestinationPath USQLSDK -Force

    echo "Copy required DLLs to output folder..."

    mkdir $AzureSDK -Force
    mkdir $DBDeploymentTool -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.Azure.Management.DataLake.Store.2.4.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $AzureSDK
    copy Newtonsoft.Json.6.0.8\lib\net45\*.dll $AzureSDK
    copy USQLSDK\build\runtime\*.* $DBDeploymentTool
    ```

2. Bir derleme veya sürüm ardışık düzenine bir **komut satırı görevi** ekleyin ve çağırarak `PackageDeploymentTool.exe`betiği girin. `PackageDeploymentTool.exe`, tanımlı **$DBDeploymentTool** klasörünün altında bulunur. Örnek betik şu şekildedir: 

    * U-SQL veritabanını yerel olarak dağıtma:

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Bir U-SQL veritabanını bir Azure Data Lake Analytics hesabına dağıtmak için etkileşimli kimlik doğrulama modunu kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Bir U-SQL veritabanını Azure Data Lake Analytics hesabına dağıtmak için **secrete** kimlik doğrulamasını kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Bir U-SQL veritabanını bir Azure Data Lake Analytics hesabına dağıtmak için **SertifikaDosyası** kimlik doğrulamasını kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

### <a name="packagedeploymenttoolexe-parameter-descriptions"></a>PackageDeploymentTool. exe parametre açıklamaları

#### <a name="common-parameters"></a>Ortak Parametreler

| Parametre | Açıklama | Varsayılan Değer | Gerekli |
|---------|-----------|-------------|--------|
|Paket|Dağıtılacak U-SQL veritabanı dağıtım paketinin yolu.|null|true|
|Veritabanı|Dağıtılacak veya oluşturulacak veritabanı adı.|ana|yanlış|
|Günlük|Günlüğe kaydetme için dosyanın yolu. Varsayılan olarak standart çıkış (konsol).|null|yanlış|
|LogLevel|Günlük düzeyi: Verbose, normal, uyarı veya hata.|LogLevel. normal|yanlış|

#### <a name="parameter-for-local-deployment"></a>Yerel dağıtım parametresi

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|DataRoot|Yerel veri kök klasörünün yolu.|null|true|

#### <a name="parameters-for-azure-data-lake-analytics-deployment"></a>Azure Data Lake Analytics dağıtımı için parametreler

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|Hesap|Hesap adına göre dağıtılacak Azure Data Lake Analytics hesabını belirtir.|null|true|
|ResourceGroup|Azure Data Lake Analytics hesabının Azure Kaynak grubu adı.|null|true|
|SubscriptionId|Azure Data Lake Analytics hesabının Azure abonelik KIMLIĞI.|null|true|
|Kiracı|Kiracı adı Azure Active Directory (Azure AD) etki alanı adıdır. Azure portal abonelik yönetimi sayfasında bulun.|null|true|
|AzureSDKPath|Azure SDK 'da bağımlı derlemelerin aranacağı yol.|null|true|
|Etkileşimli|Kimlik doğrulaması için etkileşimli mod kullanılıp kullanılmayacağını belirtir.|yanlış|yanlış|
|ClientId|Etkileşimli olmayan kimlik doğrulaması için Azure AD uygulama KIMLIĞI gereklidir.|null|Etkileşimli olmayan kimlik doğrulaması için gereklidir.|
|Secrete|Etkileşimli olmayan kimlik doğrulaması için secrete veya parola. Yalnızca güvenilen ve güvenli bir ortamda kullanılmalıdır.|null|Etkileşimli olmayan kimlik doğrulaması için gereklidir ya da SecreteFile komutunu kullanın.|
|SecreteFile|Dosya, etkileşimli olmayan kimlik doğrulaması için secrete veya parolayı kaydeder. Yalnızca geçerli kullanıcı tarafından okunabilir durumda kalmasını sağlayın.|null|Etkileşimli olmayan kimlik doğrulaması için gereklidir veya daha önce secrete kullanın.|
|SertifikaDosyası|Dosya, etkileşimli olmayan kimlik doğrulaması için X. 509.440 sertifikasını kaydeder. Varsayılan değer istemci secrete kimlik doğrulamasını kullanmaktır.|null|yanlış|
| JobPrefix | Bir U-SQL DDL işinin veritabanı dağıtımı ön eki. | Deploy_ + DateTime. Now | yanlış |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md).
- [Yerel makinenizde U-SQL betiğini çalıştırın](data-lake-analytics-data-lake-tools-local-run.md).
- U-SQL veritabanı [geliştirmek için u-SQL veritabanı projesini kullanın](data-lake-analytics-data-lake-tools-develop-usql-database.md).
