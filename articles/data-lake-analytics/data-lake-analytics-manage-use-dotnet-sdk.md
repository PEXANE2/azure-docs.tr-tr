---
title: Azure .NET SDK kullanarak Azure Data Lake Analytics yönetme
description: Bu makalede, Azure .NET SDK 'nın Data Lake Analytics işleri, veri kaynaklarını ve & kullanıcıları yöneten uygulamalar yazmak için nasıl kullanılacağı açıklanır.
services: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: 811d172d-9873-4ce9-a6d5-c1a26b374c79
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 06/18/2017
ms.openlocfilehash: 12043ead1c7d59cc3955bb927fabb54278c18bd5
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87127698"
---
# <a name="manage-azure-data-lake-analytics-a-net-app"></a>.NET uygulaması ile Azure Data Lake Analytics’i yönetme

[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Azure .NET SDK kullanılarak yazılmış bir uygulamayı kullanarak Azure Data Lake Analytics hesaplarının, veri kaynaklarının, kullanıcıların ve işlerin nasıl yönetileceği açıklanmaktadır. 

## <a name="prerequisites"></a>Önkoşullar

* **Visual Studio 2015, Visual Studio 2013 Güncelleştirme 4 veya Visual C++ Yüklü Visual Studio 2012**.
* **.NET sürüm 2.5 veya üzeri için Microsoft Azure SDK**.  [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Gerekli NuGet paketleri**

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

|Paket|Sürüm|
|-------|-------|
|[Microsoft. Rest. ClientRuntime. Azure. Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)| 2.3.1|
|[Microsoft.Azure.Management.DataLake.Analytics](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Analytics)|3.0.0|
|[Microsoft.Azure.Management.DataLake.Store](https://www.nuget.org/packages/Microsoft.Azure.Management.DataLake.Store)|2.2.0|
|[Microsoft.Azure.Management.ResourceManager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|1.6.0-Önizleme|
|[Microsoft.Azure.Graph.RBAC](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)|3.4.0-Önizleme|

Aşağıdaki komutlarla NuGet komut satırı aracılığıyla bu paketleri yükleyebilirsiniz:

```powershell
Install-Package -Id Microsoft.Rest.ClientRuntime.Azure.Authentication  -Version 2.3.1
Install-Package -Id Microsoft.Azure.Management.DataLake.Analytics  -Version 3.0.0
Install-Package -Id Microsoft.Azure.Management.DataLake.Store  -Version 2.2.0
Install-Package -Id Microsoft.Azure.Management.ResourceManager  -Version 1.6.0-preview
Install-Package -Id Microsoft.Azure.Graph.RBAC -Version 3.4.0-preview
```

## <a name="common-variables"></a>Ortak değişkenler

```csharp
string subid = "<Subscription ID>"; // Subscription ID (a GUID)
string tenantid = "<Tenant ID>"; // AAD tenant ID or domain. For example, "contoso.onmicrosoft.com"
string rg == "<value>"; // Resource  group name
string clientid = "1950a258-227b-4e31-a9cf-717495945fc2"; // Sample client ID (this will work, but you should pick your own)
```

## <a name="authentication"></a>Kimlik doğrulaması

Azure Data Lake Analytics oturum açma için birden çok seçeneğiniz vardır. Aşağıdaki kod parçacığında bir açılır pencere ile etkileşimli kullanıcı kimlik doğrulaması ile bir kimlik doğrulaması örneği gösterilmektedir.

``` csharp
using System;
using System.IO;
using System.Threading;
using System.Security.Cryptography.X509Certificates;

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Analytics;
using Microsoft.Azure.Management.DataLake.Analytics.Models;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Azure.Graph.RBAC;

public static Program
{
   public static string TENANT = "microsoft.onmicrosoft.com";
   public static string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
   public static System.Uri ARM_TOKEN_AUDIENCE = new System.Uri( @"https://management.core.windows.net/");
   public static System.Uri ADL_TOKEN_AUDIENCE = new System.Uri( @"https://datalake.azure.net/" );
   public static System.Uri GRAPH_TOKEN_AUDIENCE = new System.Uri( @"https://graph.windows.net/" );

   static void Main(string[] args)
   {
      string MY_DOCUMENTS= System.Environment.GetFolderPath( System.Environment.SpecialFolder.MyDocuments);
      string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");

      var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
      var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
      var graphCreds = GetCreds_User_Popup(TENANT, GRAPH_TOKEN_AUDIENCE, CLIENTID, tokenCache);
   }
}
```

**GetCreds_User_Popup** için kaynak kodu ve kimlik doğrulama için diğer seçeneklere yönelik kod, [Data Lake Analytics .NET kimlik doğrulama seçeneklerinde](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options) kapsanır


## <a name="create-the-client-management-objects"></a>İstemci yönetimi nesneleri oluşturma

``` csharp
var resourceManagementClient = new ResourceManagementClient(armCreds) { SubscriptionId = subid };

var adlaAccountClient = new DataLakeAnalyticsAccountManagementClient(armCreds);
adlaAccountClient.SubscriptionId = subid;

var adlsAccountClient = new DataLakeStoreAccountManagementClient(armCreds);
adlsAccountClient.SubscriptionId = subid;

var adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(adlCreds);
var adlaJobClient = new DataLakeAnalyticsJobManagementClient(adlCreds);

var adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(adlCreds);

var  graphClient = new GraphRbacManagementClient(graphCreds);
graphClient.TenantID = domain;

```

## <a name="manage-accounts"></a>Hesapları yönetme

### <a name="create-an-azure-resource-group"></a>Azure Kaynak Grubu oluşturma

Henüz bir tane oluşturmadıysanız, Data Lake Analytics bileşenlerinizi oluşturmak için bir Azure Kaynak grubunuz olması gerekir. Kimlik doğrulama kimlik bilgileriniz, abonelik KIMLIĞINIZ ve bir konum gereklidir. Aşağıdaki kod, bir kaynak grubu oluşturmayı göstermektedir:

``` csharp
var resourceGroup = new ResourceGroup { Location = location };
resourceManagementClient.ResourceGroups.CreateOrUpdate(groupName, rg);
```

Daha fazla bilgi için bkz. Azure Kaynak grupları ve Data Lake Analytics.

### <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma

Ever ADLA hesabının bir ADLS hesabı olması gerekir. Henüz bir tane kullanmak istemiyorsanız, aşağıdaki kodla bir tane oluşturabilirsiniz:

``` csharp
var new_adls_params = new DataLakeStoreAccount(location: _location);
adlsAccountClient.Account.Create(rg, adls, new_adls_params);
```

### <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Aşağıdaki kod bir ADLS hesabı oluşturur

``` csharp
var new_adla_params = new DataLakeAnalyticsAccount()
{
   DefaultDataLakeStoreAccount = adls,
   Location = location
};

adlaClient.Account.Create(rg, adla, new_adla_params);
```

### <a name="list-data-lake-store-accounts"></a>Data Lake Store hesaplarını listeleme

``` csharp
var adlsAccounts = adlsAccountClient.Account.List().ToList();
foreach (var adls in adlsAccounts)
{
   Console.WriteLine($"ADLS: {0}", adls.Name);
}
```

### <a name="list-data-lake-analytics-accounts"></a>Data Lake Analytics hesaplarını listeleme

``` csharp
var adlaAccounts = adlaClient.Account.List().ToList();

for (var adla in AdlaAccounts)
{
   Console.WriteLine($"ADLA: {0}, adla.Name");
}
```

### <a name="checking-if-an-account-exists"></a>Bir hesabın var olup olmadığı denetleniyor

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
```

### <a name="get-information-about-an-account"></a>Hesap hakkında bilgi alın

``` csharp
bool exists = adlaClient.Account.Exists(rg, adla));
if (exists)
{
   var adla_accnt = adlaClient.Account.Get(rg, adla);
}
```

### <a name="delete-an-account"></a>Hesap silme

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
   adlaClient.Account.Delete(rg, adla);
}
```

### <a name="get-the-default-data-lake-store-account"></a>Varsayılan Data Lake Store hesabını al

Her Data Lake Analytics hesabının varsayılan bir Data Lake Store hesabı olması gerekir. Bir analiz hesabının varsayılan depolama hesabını öğrenmek için bu kodu kullanın.

``` csharp
if (adlaClient.Account.Exists(rg, adla))
{
  var adla_accnt = adlaClient.Account.Get(rg, adla);
  string def_adls_account = adla_accnt.DefaultDataLakeStoreAccount;
}
```

## <a name="manage-data-sources"></a>Veri kaynaklarını yönetme

Data Lake Analytics Şu anda aşağıdaki veri kaynaklarını desteklemektedir:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Depolama Hesabı](../storage/common/storage-introduction.md)

### <a name="link-to-an-azure-storage-account"></a>Azure depolama hesabına bağlantı

Azure depolama hesaplarına bağlantılar oluşturabilirsiniz.

``` csharp
string storage_key = "xxxxxxxxxxxxxxxxxxxx";
string storage_account = "mystorageaccount";
var addParams = new AddStorageAccountParameters(storage_key);            
adlaClient.StorageAccounts.Add(rg, adla, storage_account, addParams);
```

### <a name="list-azure-storage-data-sources"></a>Azure depolama veri kaynaklarını listeleme

``` csharp
var stg_accounts = adlaAccountClient.StorageAccounts.ListByAccount(rg, adla);

if (stg_accounts != null)
{
  foreach (var stg_account in stg_accounts)
  {
      Console.WriteLine($"Storage account: {0}", stg_account.Name);
  }
}
```

### <a name="list-data-lake-store-data-sources"></a>Data Lake Store veri kaynaklarını listeleme

``` csharp
var adls_accounts = adlsClient.Account.List();

if (adls_accounts != null)
{
  foreach (var adls_accnt in adls_accounts)
  {
      Console.WriteLine($"ADLS account: {0}", adls_accnt.Name);
  }
}
```

### <a name="upload-and-download-folders-and-files"></a>Klasörleri ve dosyaları karşıya yükleme ve indirme

Aşağıdaki yöntemleri kullanarak, tek tek dosyaları veya klasörleri Azure 'dan yerel bilgisayarınıza yüklemek ve indirmek için Data Lake Store dosya sistemi istemci yönetimi nesnesini kullanabilirsiniz:

- UploadFolder
- UploadFile
- DownloadFolder
- DownloadFile

Bu yöntemlerin ilk parametresi, Data Lake Store hesabının adıdır ve ardından kaynak yolu ve hedef yolu için parametreler izler.

Aşağıdaki örnek, Data Lake Store bir klasörün nasıl indirileceği gösterilmektedir.

``` csharp
adlsFileSystemClient.FileSystem.DownloadFolder(adls, sourcePath, destinationPath);
```

### <a name="create-a-file-in-a-data-lake-store-account"></a>Data Lake Store hesapta bir dosya oluşturun

``` csharp
using (var memstream = new MemoryStream())
{
   using (var sw = new StreamWriter(memstream, UTF8Encoding.UTF8))
   {
      sw.WriteLine("Hello World");
      sw.Flush();
      
      memstream.Position = 0;

      adlsFileSystemClient.FileSystem.Create(adls, "/Samples/Output/randombytes.csv", memstream);
   }
}
```

### <a name="verify-azure-storage-account-paths"></a>Azure depolama hesabı yollarını doğrulama

Aşağıdaki kod, bir Azure depolama hesabının (storageAccntName) Data Lake Analytics hesapta (analyticsAccountName) bulunup bulunmadığını ve Azure depolama hesabında bir kapsayıcı (containerName) olup olmadığını denetler.

``` csharp
string storage_account = "mystorageaccount";
string storage_container = "mycontainer";
bool accountExists = adlaClient.Account.StorageAccountExists(rg, adla, storage_account));
bool containerExists = adlaClient.Account.StorageContainerExists(rg, adla, storage_account, storage_container));
```

## <a name="manage-catalog-and-jobs"></a>Katalog ve işleri yönetme

Datalakeanal, Alogmanagementclient nesnesi her bir Azure Data Lake Analytics hesabı için sağlanan SQL veritabanının yönetilmesi için yöntemler sağlar. Datalakeanalsjobmanagementclient, veritabanı üzerinde U-SQL betikleri ile çalıştırılan işleri göndermek ve yönetmek için yöntemler sağlar.

### <a name="list-databases-and-schemas"></a>Veritabanları ve şemaları listeleme

Birçok konuda, en yaygın olarak veritabanları ve bunların şeması bulunur. Aşağıdaki kod bir veritabanları koleksiyonu edinir ve sonra her bir veritabanı için şemayı numaralandırır.

``` csharp
var databases = adlaCatalogClient.Catalog.ListDatabases(adla);
foreach (var db in databases)
{
  Console.WriteLine($"Database: {db.Name}");
  Console.WriteLine(" - Schemas:");
  var schemas = adlaCatalogClient.Catalog.ListSchemas(adla, db.Name);
  foreach (var schm in schemas)
  {
      Console.WriteLine($"\t{schm.Name}");
  }
}
```

### <a name="list-table-columns"></a>Tablo sütunlarını listeleme

Aşağıdaki kod, belirtilen tablodaki sütunları listelemek için bir Data Lake Analytics katalog yönetimi istemcisiyle veritabanına nasıl erişdeğiştirileceğini gösterir.

```csharp
var tbl = adlaCatalogClient.Catalog.GetTable(adla, "master", "dbo", "MyTableName");
IEnumerable<USqlTableColumn> columns = tbl.ColumnList;

foreach (USqlTableColumn utc in columns)
{
  Console.WriteLine($"\t{utc.Name}");
}
```

### <a name="submit-a-u-sql-job"></a>U-SQL işi gönderme

Aşağıdaki kod, bir işi göndermek için Data Lake Analytics Iş Yönetimi istemcisinin nasıl kullanılacağını gösterir.

``` csharp
string scriptPath = "/Samples/Scripts/SearchResults_Wikipedia_Script.txt";
Stream scriptStrm = adlsFileSystemClient.FileSystem.Open(_adlsAccountName, scriptPath);
string scriptTxt = string.Empty;
using (StreamReader sr = new StreamReader(scriptStrm))
{
    scriptTxt = sr.ReadToEnd();
}

var jobName = "SR_Wikipedia";
var jobId = Guid.NewGuid();
var properties = new USqlJobProperties(scriptTxt);
var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
var jobInfo = adlaJobClient.Job.Create(adla, jobId, parameters);
Console.WriteLine($"Job {jobName} submitted.");
```

### <a name="list-failed-jobs"></a>Başarısız işleri listeleme

Aşağıdaki kod, başarısız olan işlerle ilgili bilgileri listeler.

```csharp
var odq = new ODataQuery<JobInformation> { Filter = "result eq 'Failed'" };
var jobs = adlaJobClient.Job.List(adla, odq);
foreach (var j in jobs)
{
   Console.WriteLine($"{j.Name}\t{j.JobId}\t{j.Type}\t{j.StartTime}\t{j.EndTime}");
}
```

### <a name="list-pipelines"></a>İşlem hatlarını Listele

Aşağıdaki kod, hesaba gönderilen işlerin her bir ardışık düzenine ilişkin bilgileri listeler.

``` csharp
var pipelines = adlaJobClient.Pipeline.List(adla);
foreach (var p in pipelines)
{
   Console.WriteLine($"Pipeline: {p.Name}\t{p.PipelineId}\t{p.LastSubmitTime}");
}
```

### <a name="list-recurrences"></a>Tekrarları Listele

Aşağıdaki kod, hesaba gönderilen işlerin her yinelemesi hakkındaki bilgileri listeler.

``` csharp
var recurrences = adlaJobClient.Recurrence.List(adla);
foreach (var r in recurrences)
{
   Console.WriteLine($"Recurrence: {r.Name}\t{r.RecurrenceId}\t{r.LastSubmitTime}");
}
```

## <a name="common-graph-scenarios"></a>Ortak Graf senaryoları

### <a name="look-up-user-in-the-aad-directory"></a>Kullanıcıyı AAD dizininde ara

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
```

### <a name="get-the-objectid-of-a-user-in-the-aad-directory"></a>AAD dizinindeki bir kullanıcının ObjectID 'sini al

``` csharp
var userinfo = graphClient.Users.Get( "bill@contoso.com" );
Console.WriteLine( userinfo.ObjectId )
```

## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

Datalakeanalticsaccountmanagementclient nesnesi, bir Data Lake Analytics hesabının işlem ilkelerini yönetmek için yöntemler sağlar.

### <a name="list-compute-policies"></a>İşlem ilkelerini Listele

Aşağıdaki kod, bir Data Lake Analytics hesabı için işlem ilkelerinin bir listesini alır.

``` csharp
var policies = adlaAccountClient.ComputePolicies.ListByAccount(rg, adla);
foreach (var p in policies)
{
   Console.WriteLine($"Name: {p.Name}\tType: {p.ObjectType}\tMax AUs / job: {p.MaxDegreeOfParallelismPerJob}\tMin priority / job: {p.MinPriorityPerJob}");
}
```

### <a name="create-a-new-compute-policy"></a>Yeni bir işlem İlkesi Oluştur

Aşağıdaki kod, bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur, belirtilen kullanıcı için kullanılabilir maksimum AU sayısını 50 olarak ayarlar ve en düşük iş önceliği 250 ' dir.

``` csharp
var userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde";
var newPolicyParams = new ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250);
adlaAccountClient.ComputePolicies.CreateOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure portal kullanarak Azure Data Lake Analytics yönetme](data-lake-analytics-manage-use-portal.md)
* [Azure portal kullanarak Azure Data Lake Analytics işleri izleme ve sorunlarını giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)