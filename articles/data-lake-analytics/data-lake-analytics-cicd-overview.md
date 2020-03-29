---
title: Azure Veri Gölü Analitiği için BIR CI/CD ardışık nokta nasıl ayarlanır?
description: Azure Veri Gölü Analitiği için sürekli tümleştirme ve sürekli dağıtım ayarlamayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60333885"
---
# <a name="how-to-set-up-a-cicd-pipeline-for-azure-data-lake-analytics"></a>Azure Veri Gölü Analitiği için BIR CI/CD ardışık nokta nasıl ayarlanır?  

Bu makalede, U-SQL işleri ve U-SQL veritabanları için sürekli bir tümleştirme ve dağıtım (CI/CD) ardışık birimi kurmayı öğrenirsiniz.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-cicd-for-u-sql-jobs"></a>U-SQL işleri için CI/CD'yi kullanma

Visual Studio için Azure Veri Gölü Araçları, U-SQL komut dosyalarını düzenlemenize yardımcı olan U-SQL proje türünü sağlar. U-SQL kodunuzu yönetmek için U-SQL projesini kullanmak, daha fazla CI/CD senaryosunu kolaylaştırır.

## <a name="build-a-u-sql-project"></a>U-SQL projesi oluşturma

Bir U-SQL projesi, ilgili parametreleri geçirerek Microsoft Build Engine (MSBuild) ile oluşturulabilir. U-SQL projesi için bir yapı işlemi ayarlamak için bu makaledeki adımları izleyin.

### <a name="project-migration"></a>Proje geçişi

Bir U-SQL projesi için bir yapı görevi ayarlamadan önce, U-SQL projesinin en son sürümüne sahip olduğundan emin olun. Düzenleyicinizde U-SQL proje dosyasını açın ve bu alma öğelerine sahip olduğunuzu doğrulayın:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Değilse, projeyi geçirmek için iki seçeneğiniz var:

- Seçenek 1: Eski alma öğesini bir öncekine değiştirin.
- Seçenek 2: Visual Studio için Azure Veri Gölü Araçları'nda eski projeyi açın. 2.3.3000.0'dan daha yeni bir sürüm kullanın. Eski proje şablonu otomatik olarak en yeni sürüme yükseltilir. 2.3.3000.0'dan daha yeni sürümlerle oluşturulan yeni projeler yeni şablonu kullanır.

### <a name="get-nuget"></a>NuGet alın

MSBuild, U-SQL projeleri için yerleşik destek sağlamaz. Bu desteği almak için, gerekli dil hizmetini ekleyen [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet paketine çözümünüz için bir başvuru eklemeniz gerekir.

NuGet paketi referansını eklemek için Visual Studio Solution Explorer'da çözüme sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin. Veya çözüm klasörüne `packages.config` çağrılan bir dosya ekleyebilir ve içine aşağıdaki içerikleri koyabilirsiniz:

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL veritabanı başvurularını yönetme

U-SQL projesindeki U-SQL komut dosyaları, U-SQL veritabanı nesneleri için sorgu deyimleri olabilir. Bu durumda, U-SQL projesini oluşturmadan önce nesnelerin tanımını içeren ilgili U-SQL veritabanı projesine başvurmanız gerekir. Bir U-SQL tablosunu sorgularken veya bir derlemeye başvuruyaptığınızda bir örnektir. 

[U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
>DROP deyimi kaza silme sorununa neden olabilir. DROP deyimini etkinleştirmek için MSBuild bağımsız değişkenlerini açıkça belirtmeniz gerekir. **AllowDropStatement,** damla derleme ve damla tablo değeri işlevi gibi veri ile ilgili olmayan DROP işlemini sağlar. **AllowDataDropStatement,** damla tablosu ve damla şeması gibi veri ile ilgili DROP işlemini sağlar. AllowDataDropStatement'ı kullanmadan önce İzin DamlaBildirimi'ni etkinleştirmeniz gerekir.
>

### <a name="build-a-u-sql-project-with-the-msbuild-command-line"></a>MSBuild komut satırı ile bir U-SQL projesi oluşturma

Önce projeyi geçirin ve NuGet paketini alın. Ardından, U-SQL projenizi oluşturmak için aşağıdaki ek bağımsız değişkenlerle standart MSBuild komut satırını arayın: 

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

Bağımsız değişkentanımı ve değerleri aşağıdaki gibidir:

* **USQLSDKPath=\<U-SQL Nuget paketi>\build\runtime**. Bu parametre, U-SQL dil hizmeti için NuGet paketinin yükleme yolunu ifade eder.
* **USQLTargetType=Birleştirme veya SözdizimiDenetimi**:
    * **Birleştirme**. Birleştirme modu, dosyaların arkasındaki kodları derler. Örnekler **.cs**, **.py**ve **.r** dosyalarıdır. U-SQL komut dosyasına, ortaya çıkan kullanıcı tanımlı kod kitaplığını satır içine satır içine. Örnekler dll ikili, Python veya R kodu.
    * **SözdizimiÇek**. SyntaxCheck modu önce kod arkasındaki dosyaları U-SQL komut dosyasında birleştirir. Ardından kodunuzu doğrulamak için U-SQL komut dosyasını derler.
* **DataRoot=\<DataRoot yolu>. ** DataRoot yalnızca Sözdizimi Denetimi modu için gereklidir. Sözdizimi Denetimi moduile komut dosyası oluşturduğunda, MSBuild komut dosyasındaki veritabanı nesnelerine yapılan başvuruları denetler. Oluşturmadan önce, yapı makinesinin DataRoot klasöründe U-SQL veritabanından başvurulan nesneleri içeren eşleşen bir yerel ortam ayarlayın. Ayrıca, [bir U-SQL veritabanı projesine başvurarak](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)bu veritabanı bağımlılıklarını yönetebilirsiniz. MSBuild dosyaları değil, yalnızca veritabanı nesne stobaşvurularını denetler.
* **EnableDeployment=doğru** veya **yanlış**. EnableDeployment, yapı işlemi sırasında başvurulan U-SQL veritabanlarını dağıtmasına izin verilip verilmediğini gösterir. Bir U-SQL veritabanı projesine başvurur ve U-SQL komut dosyanızdaki veritabanı nesnelerini tüketirseniz, bu parametreyi **doğru**olarak ayarlayın.

### <a name="continuous-integration-through-azure-pipelines"></a>Azure Pipelines ile sürekli tümleştirme

Komut satırına ek olarak, Azure Pipelines'da U-SQL projeleri oluşturmak için Visual Studio Build veya MSBuild görevini de kullanabilirsiniz. Yapı ardışık düzeni kurmak için yapı ardışık yapısına iki görev eklediğinizden emin olun: NuGet geri yükleme görevi ve MSBuild görevi.

![U-SQL projesi için MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

1.  MSBuild'in U-SQL dil hedeflerini bulabilmeleri için `Azure.DataLake.USQL.SDK`çözüme başvurulan NuGet paketini almak için Bir NuGet geri yükleme görevi ekleyin. MSBuild bağımsız değişkenörneğini doğrudan adım 2'de kullanmak `$(Build.SourcesDirectory)/packages` istiyorsanız **Gelişmiş** > **Hedef dizini** olarak ayarlayın.

    ![NuBir U-SQL projesi için geri yükleme görevi alın](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  Aşağıdaki örnekte gösterildiği gibi Visual Studio oluşturma araçlarında veya MSBuild görevinde MSBuild bağımsız değişkenlerini ayarlayın. Veya Azure Ardışık Hatları yapı ardışık yapısında bu bağımsız değişkenler için değişkenler tanımlayabilirsiniz.

    ![U-SQL projesi için CI/CD MSBuild değişkenlerini tanımlama](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

### <a name="u-sql-project-build-output"></a>U-SQL proje oluşturma çıktısı

Bir yapıyı çalıştırdıktan sonra, U-SQL projesindeki tüm komut dosyaları `USQLProjectName.usqlpack`oluşturulur ve bir zip dosyasına çıkar. Projenizdeki klasör yapısı sıkıştırılmış yapı çıktısında tutulur.

> [!NOTE]
>
> Her U-SQL komut dosyası için kod arkası dosyaları komut dosyası oluşturma çıktısına satır içi deyimi olarak birleştirilir.
>

## <a name="test-u-sql-scripts"></a>U-SQL betiklerini test etme

Azure Veri Gölü, U-SQL komut dosyaları ve C# UDO/UDAG/UDF için test projeleri sağlar:
* [U-SQL komut dosyaları ve genişletilmiş C# kodu için test örnekleri](data-lake-analytics-cicd-test.md#test-u-sql-scripts)nin nasıl ekleyeceğinizi öğrenin.
* [Azure Ardışık Durumlarda test denemelerini nasıl çalıştırılayacağınızı](data-lake-analytics-cicd-test.md#run-test-cases-in-azure-devops)öğrenin.

## <a name="deploy-a-u-sql-job"></a>U-SQL işini dağıtma

Yapı ve test işlemi yle kodu doğruladıktan sonra, Bir Azure PowerShell görevi aracılığıyla U-SQL işlerini doğrudan Azure Pipelines'dan gönderebilirsiniz. Ayrıca komut dosyasını Azure Veri Gölü Deposu'na veya Azure Blob depolamasına dağıtabilir ve [zamanlanan işleri Azure Veri Fabrikası üzerinden çalıştırabilirsiniz.](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics)

### <a name="submit-u-sql-jobs-through-azure-pipelines"></a>Azure Pipelines aracılığıyla U-SQL işlerini gönderme

U-SQL projesinin yapı çıktısı **USQLProjectName.usqlpack**adlı bir zip dosyasıdır. Zip dosyası projedeki tüm U-SQL komut dosyalarını içerir. U-SQL işlerini doğrudan Azure Pipelines'dan göndermek için aşağıdaki örnek PowerShell komut dosyasıyla Pipelines'daki [Azure PowerShell görevini](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) kullanabilirsiniz.

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

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>Azure Veri Fabrikası aracılığıyla U-SQL işlerini dağıtma

U-SQL işlerini doğrudan Azure Pipelines'dan gönderebilirsiniz. Veya yapılı komut dosyalarını Azure Veri Gölü Deposu'na veya Azure Blob depolama alanına yükleyebilir ve [zamanlanan işleri Azure Veri Fabrikası üzerinden çalıştırabilirsiniz.](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics)

U-SQL komut dosyalarını Azure Veri Gölü Deposu hesabına yüklemek için aşağıdaki örnek PowerShell komut dosyasıyla Azure Veri Hatları'ndaki [Azure PowerShell görevini](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) kullanın:

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

Visual Studio için Azure Veri Gölü Araçları, U-SQL veritabanlarını geliştirmenize, yönetmenize ve dağıtmanıza yardımcı olan U-SQL veritabanı proje şablonları sağlar. [U-SQL veritabanı projesi](data-lake-analytics-data-lake-tools-develop-usql-database.md)hakkında daha fazla bilgi edinin.

## <a name="build-u-sql-database-project"></a>U-SQL veritabanı projesi oluşturma

### <a name="get-the-nuget-package"></a>NuGet paketini alın

MSBuild, U-SQL veritabanı projeleri için yerleşik destek sağlamaz. Bu özelliği elde etmek için, gerekli dil hizmetini ekleyen [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet paketine çözümünüz için bir başvuru eklemeniz gerekir.

NuGet paketi referansını eklemek için Visual Studio Solution Explorer'da çözüme sağ tıklayın. **NuGet Paketlerini Yönet'i**seçin. Ardından NuGet paketini arayın ve yükleyin. Veya çözüm klasörüne **packages.config** adlı bir dosya ekleyebilir ve içine aşağıdaki içerikleri koyabilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-a-database-project-with-the-msbuild-command-line"></a>MSBuild komut satırı ile U-SQL veritabanı projesi oluşturma

U-SQL veritabanı projenizi oluşturmak için standart MSBuild komut satırını arayın ve Ek bir bağımsız değişken olarak U-SQL SDK NuGet paket başvuruhattını geçin. Aşağıdaki örneğe bakın: 

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Bağımsız `USQLSDKPath=<U-SQL Nuget package>\build\runtime` değişken, U-SQL dil hizmeti için NuGet paketinin yükleme yolunu ifade eder.

### <a name="continuous-integration-with-azure-pipelines"></a>Azure Pipelines ile sürekli tümleştirme

Komut satırına ek olarak, Azure Ardışık Hatları'nda U-SQL veritabanı projeleri oluşturmak için Visual Studio Build veya MSBuild görevini kullanabilirsiniz. Bir yapı görevi ayarlamak için yapı ardışık yapısına iki görev eklediğinizden emin olun: NuGet geri yükleme görevi ve MSBuild görevi.

   ![Bir U-SQL projesi için CI/CD MSBuild görevi](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 


1. MSBuild'in U-SQL dil hedeflerini bulabilmeleri için `Azure.DataLake.USQL.SDK`çözüme başvurulan NuGet paketini almak için Bir NuGet geri yükleme görevi ekleyin. MSBuild bağımsız değişkenörneğini doğrudan adım 2'de kullanmak `$(Build.SourcesDirectory)/packages` istiyorsanız **Gelişmiş** > **Hedef dizini** olarak ayarlayın.

   ![CI/CD NuU-SQL projesi için görev alın](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2. Aşağıdaki örnekte gösterildiği gibi Visual Studio oluşturma araçlarında veya MSBuild görevinde MSBuild bağımsız değişkenlerini ayarlayın. Veya Azure Ardışık Hatları yapı ardışık yapısında bu bağımsız değişkenler için değişkenler tanımlayabilirsiniz.

   ![U-SQL veritabanı projesi için CI/CD MSBuild değişkenlerini tanımlama](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

   ```
   /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
   ```
 
### <a name="u-sql-database-project-build-output"></a>U-SQL veritabanı proje oluşturma çıktısı

U-SQL veritabanı projesinin yapı çıktısı, sonek `.usqldbpack`ile birlikte bir U-SQL veritabanı dağıtım paketidir. Paket, `.usqldbpack` ddl klasöründeki tek bir U-SQL komut dosyasındaki tüm DDL ifadelerini içeren bir zip dosyasıdır. Geçici bir klasörde derleme için tüm **.dll'leri** ve ek dosyaları içerir.

## <a name="test-table-valued-functions-and-stored-procedures"></a>Test tablosu değerli fonksiyonlar ve depolanan yordamlar

Tablo değerindeki işlevler ve depolanan yordamlar için test çalışmaları ekleme şu anda desteklenmemektedir. Geçici çözüm olarak, bu işlevleri arayan ve onlar için test örnekleri yazan U-SQL komut dosyaları içeren bir U-SQL projesi oluşturabilirsiniz. U-SQL veritabanı projesinde tanımlanan tablo değerli işlevler ve saklı yordamlar için test çalışmaları ayarlamak için aşağıdaki adımları uygulayın:

1.  Test amacıyla bir U-SQL projesi oluşturun ve tablo değeri olan işlevleri ve depolanmış yordamları çağıran U-SQL komut dosyaları yazın.
2.  U-SQL projesine bir veritabanı başvurusu ekleyin. Tablo değerindeki işlevi ve depolanmış yordam tanımını almak için DDL deyimini içeren veritabanı projesine başvurmanız gerekir. [Veritabanı başvuruları](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)hakkında daha fazla bilgi edinin.
3.  Tablo değeri olan işlevleri ve depolanmış yordamları arayan U-SQL komut dosyaları için test örnekleri ekleyin. [U-SQL komut dosyaları için test örnekleri](data-lake-analytics-cicd-test.md#test-u-sql-scripts)nin nasıl ekleyeceğinizi öğrenin.

## <a name="deploy-u-sql-database-through-azure-pipelines"></a>Azure Pipelines aracılığıyla U-SQL veritabanını dağıtma

`PackageDeploymentTool.exe`U-SQL veritabanı dağıtım paketleri, **.usqldbpack**dağıtmaya yardımcı programlama ve komut satırı arabirimleri sağlar. SDK, **build/runtime/PackageDeploymentTool.exe**adresinde bulunan [U-SQL SDK NuGet paketine](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)dahildir. U-SQL `PackageDeploymentTool.exe`veritabanlarını hem Azure Veri Gölü Analitiği'ne hem de yerel hesaplara dağıtabilirsiniz.

> [!NOTE]
>
> PowerShell komut satırı desteği ve Azure Pipelines sürümünde U-SQL veritabanı dağıtımı için görev desteği şu anda beklemede.
>

Azure Ardışık Hatları'nda bir veritabanı dağıtım görevi ayarlamak için aşağıdaki adımları izleyin:

1. Yapı veya sürüm ardışık yapısına bir PowerShell Script görevi ekleyin ve aşağıdaki PowerShell komut dosyasını çalıştırın. Bu görev, Azure SDK `PackageDeploymentTool.exe` bağımlılıklarını `PackageDeploymentTool.exe`ve . Bağımlılıkları ve dağıtım aracını belirli klasörlere yüklemek için **-AzureSDK** ve **-DBDeploymentTool** parametrelerini ayarlayabilirsiniz. **-AzureSDK** yolunu adım `PackageDeploymentTool.exe` 2'de **-AzureSDKPath** parametresi olarak geçirin. 

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

2. Yapı veya sürüm ardışık yapısına **komut satırı görevi** ekleyin `PackageDeploymentTool.exe`ve arayarak komut dosyasını doldurun. `PackageDeploymentTool.exe`tanımlanan **$DBDeploymentTool** klasörün altında yer alır. Örnek komut dosyası aşağıdaki gibidir: 

    * Bir U-SQL veritabanını yerel olarak dağıtın:

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Bir U-SQL veritabanını Azure Veri Gölü Analizi hesabına dağıtmak için etkileşimli kimlik doğrulama modunu kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Bir U-SQL veritabanını Azure Veri Gölü Analizi hesabına dağıtmak için **gizli** kimlik doğrulaması kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Bir U-SQL veritabanını Azure Veri Gölü Analizi hesabına dağıtmak için **certFile** kimlik doğrulamasını kullanın:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

### <a name="packagedeploymenttoolexe-parameter-descriptions"></a>PackageDeploymentTool.exe parametre açıklamaları

#### <a name="common-parameters"></a>Ortak parametreler

| Parametre | Açıklama | Varsayılan Değer | Gerekli |
|---------|-----------|-------------|--------|
|Paket|Dağıtılacak U-SQL veritabanı dağıtım paketinin yolu.|null|true|
|Database|Dağıtılacak veya oluşturulacak veritabanı adı.|ana|yanlış|
|Logfile|Günlüğe kaydetme için dosyanın yolu. Varsayılan olarak standart çıkış (konsol).|null|yanlış|
|LogLevel|Günlük düzeyi: Verbose, Normal, Uyarı veya Hata.|LogLevel.Normal|yanlış|

#### <a name="parameter-for-local-deployment"></a>Yerel dağıtım için parametre

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|DataRoot|Yerel veri kökü klasörünün yolu.|null|true|

#### <a name="parameters-for-azure-data-lake-analytics-deployment"></a>Azure Veri Gölü Analizi dağıtımı için parametreler

|Parametre|Açıklama|Varsayılan Değer|Gerekli|
|---------|-----------|-------------|--------|
|Hesap|Hangi Azure Veri Gölü Analizi hesabının hesap adına dağıtılacayacağını belirtir.|null|true|
|ResourceGroup|Azure Veri Gölü Analizi hesabının Azure kaynak grubu adı.|null|true|
|SubscriptionId|Azure Veri Gölü Analizi hesabı için Azure abonelik kimliği.|null|true|
|Kiracı|Kiracı adı Azure Etkin Dizin (Azure AD) etki alanı adıdır. Azure portalındaki abonelik yönetimi sayfasında bulabilirsiniz.|null|true|
|AzureSDKPath|Azure SDK'da bağımlı derlemelerde arama yolu.|null|true|
|Etkileşimli|Kimlik doğrulama için etkileşimli modu kullanıp kullanmamak.|yanlış|yanlış|
|ClientId|Etkileşimli olmayan kimlik doğrulaması için gereken Azure AD uygulama kimliği.|null|Etkileşimli olmayan kimlik doğrulaması için gereklidir.|
|Salgıladıkları|Etkileşimli olmayan kimlik doğrulamasının salgılanması veya parolası. Yalnızca güvenilir ve güvenli bir ortamda kullanılmalıdır.|null|Etkileşimli olmayan kimlik doğrulaması için gerekli veya SecreteFile'ı başka bir şekilde kullanın.|
|SecreteFile|Dosya, etkileşimli olmayan kimlik doğrulama için salgıyı veya parolayı kaydeder. Yalnızca geçerli kullanıcı tarafından okunabilir tuttuğundan emin olun.|null|Etkileşimli olmayan kimlik doğrulaması için gerekli veya Secrete'ı başka bir şekilde kullanın.|
|Certfile|Dosya, etkileşimli olmayan kimlik doğrulama için X.509 sertifikasını kaydeder. Varsayılan istemci salgılanması kullanmaktır.|null|yanlış|
| İş Öneki | U-SQL DDL işinin veritabanı dağıtımı için öneki. | Deploy_ + DateTime.Now | yanlış |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gölü Analizi kodunuzu nasıl test emilir?](data-lake-analytics-cicd-test.md)
- [Yerel makinenizde U-SQL komut dosyası çalıştırın.](data-lake-analytics-data-lake-tools-local-run.md)
- [U-SQL veritabanını geliştirmek için U-SQL veritabanı projesini kullanın.](data-lake-analytics-data-lake-tools-develop-usql-database.md)
