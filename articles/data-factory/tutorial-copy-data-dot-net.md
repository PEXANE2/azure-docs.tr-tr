---
title: Azure Blob depolamadan Azure SQL veritabanı 'na veri kopyalama
description: Bu öğretici, Azure Blob Depolama alanından Azure SQL Veritabanına veri kopyalamaya ilişkin adım adım yönergeler sağlar.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 11/08/2019
ms.author: jingwang
ms.openlocfilehash: 7f3fdf1b723158db873bc2635de34d878c464201
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75439432"
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob’dan Azure SQL Veritabanına veri kopyalama

Bu öğreticide, Azure Blob Depolama alanından Azure SQL Veritabanına veri kopyalayan bir Data Factory işlem hattı oluşturacaksınız. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).

Bu öğreticide aşağıdaki adımları izleyebilirsiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure Depolama ve Azure SQL Veritabanı bağlı hizmeti oluşturma.
> * Azure Blob ve Azure SQL Veritabanı veri kümeleri oluşturma.
> * Kopyalama etkinliği içeren bir işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Bu öğreticide .NET SDK kullanılır. Azure Data Factory ile etkileşim kurmak için başka mekanizmalar kullanabilirsiniz; **hızlı**başlangıçlarda örneklere bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* *Azure Depolama hesabı*. Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, bkz. [genel amaçlı depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).
* *Azure SQL Veritabanı*. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. Azure SQL veritabanınız yoksa bkz. [Azure SQL veritabanı oluşturma](../sql-database/sql-database-single-database-get-started.md).
* *Visual Studio*. Bu makaledeki izlenecek yol, Visual Studio 2019 kullanır.
* *[.Net Için Azure SDK](/dotnet/azure/dotnet-tools)* .
* *Azure Active Directory Uygulama*. Azure Active Directory uygulamanız yoksa, [nasıl yapılır: Azure AD uygulaması oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md)konusunun [Azure Active Directory uygulama oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) bölümüne bakın. Sonraki adımlarda kullanılmak üzere aşağıdaki değerleri kopyalayın: **uygulama (istemci) kimliği**, **kimlik doğrulama anahtarı**ve **Dizin (kiracı) kimliği**. Aynı makaledeki yönergeleri izleyerek uygulamayı **katkıda bulunan** rolüne atayın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Şimdi, kaynak blog ve havuz SQL tablosu oluşturarak Azure Blob 'U ve Azure SQL veritabanınızı öğreticiye hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

İlk olarak, bir kapsayıcı oluşturup buna bir giriş metin dosyası yükleyerek bir kaynak blobu oluşturun:

1. Not Defteri'ni açın. Aşağıdaki metni kopyalayın ve *ınputemp. txt*adlı bir dosyaya yerel olarak kaydedin.

    ```inputEmp.txt
    John|Doe
    Jane|Doe
    ```

2. *Adfv2tutorial* kapsayıcısını oluşturmak ve *ınputemp. txt* dosyasını kapsayıcıya yüklemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi bir araç kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

Ardından, bir havuz SQL tablosu oluşturun:

1. Azure SQL Veritabanınızda *dbo.emp* tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Azure hizmetlerinin SQL sunucusuna erişmesine izin ver. Data Factory hizmetinin Azure SQL sunucunuza veri yazabilmesi için Azure SQL sunucunuzdaki Azure hizmetlerine erişime izin verildiğinden emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    1. SQL Server 'nizi yönetmek için [Azure Portal](https://portal.azure.com) gidin. **SQL Server 'lar**için arama yapın ve seçin.

    2. Sunucunuzu seçin.
    
    3. SQL Server menüsünün **güvenlik** başlığı altında Güvenlik **duvarları ve sanal ağlar**' ı seçin.

    4. **Güvenlik duvarı ve sanal ağlar** sayfasında, **Azure hizmetlerinin ve kaynaklarının bu sunucuya erişmesine Izin ver**altında **Açık**' ı seçin.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio 'yu kullanarak bir C# .NET konsol uygulaması oluşturun.

1. Visual Studio'yu açın.
2. **Başlangıç** penceresinde **Yeni proje oluştur**' u seçin.
3. **Yeni proje oluştur** penceresinde, proje türleri listesinden C# **konsol uygulaması (.NET Framework)** sürümünü seçin. Sonra **İleri**’yi seçin.
4. **Yeni projeyi yapılandırın** penceresinde, *ADFv2Tutorial*için bir **Proje adı** girin. **Konum**için, projenin kaydedileceği dizine gidin ve/veya oluşturun. Ardından **Oluştur**’u seçin. Yeni proje, Visual Studio IDE 'de görüntülenir.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

Ardından, NuGet Paket Yöneticisi 'ni kullanarak gerekli kitaplık paketlerini yükler.

1. Menü çubuğunda **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin.
2. **Paket Yöneticisi konsolu** bölmesinde, paketleri yüklemek için aşağıdaki komutları çalıştırın. NuGet paketi Azure Data Factory hakkında daha fazla bilgi için bkz. [Microsoft. Azure. Management. DataFactory](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/).

    ```package manager console
    Install-Package Microsoft.Azure.Management.DataFactory
    Install-Package Microsoft.Azure.Management.ResourceManager -PreRelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-data-factory-client"></a>Veri fabrikası istemcisi oluşturma

Data Factory istemcisi oluşturmak için bu adımları izleyin.

1. *Program.cs*' i açın, ardından ad alanlarına başvurular eklemek için aşağıdaki kodla mevcut `using` deyimlerinin üzerine yazın.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Aşağıdaki kodu, değişkenleri ayarlayan `Main` yöntemine ekleyin. 14 yer tutucuları kendi değerlerinizle değiştirin.

    Data Factory Şu anda kullanılabildiği Azure bölgelerinin listesini görmek için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). **Ürünler** açılan listesinde **, > ** **Analytics** > **Data Factory**' yı seçin. Sonra **bölgeler** açılan listesinde ilgilendiğiniz bölgeleri seçin. Seçtiğiniz bölgeler için Data Factory ürünlerin kullanılabilirlik durumuyla birlikte bir kılavuz görüntülenir.

    > [!NOTE]
    > Azure depolama ve Azure SQL veritabanı gibi veri depoları ve Data Factory kullanımları, Data Factory için seçtiğiniz sayıdan başka bölgelerde olabilir.

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID to create the factory>";
    string resourceGroup = "<your resource group to create the factory>";

    string region = "<location to create the data factory in, such as East US>";
    string dataFactoryName = "<name of data factory to create (must be globally unique)>";

    // Specify the source Azure Blob information
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    string inputBlobPath = "adfv2tutorial/";
    string inputBlobName = "inputEmp.txt";

    // Specify the sink Azure SQL Database information
    string azureSqlConnString = 
        "Server=tcp:<your server name>.database.windows.net,1433;" +
        "Database=<your database name>;" +
        "User ID=<your username>@<your server name>;" +
        "Password=<your password>;" +
        "Trusted_Connection=False;Encrypt=True;Connection Timeout=30";
    string azureSqlTableName = "dbo.emp";

    string storageLinkedServiceName = "AzureStorageLinkedService";
    string sqlDbLinkedServiceName = "AzureSqlDbLinkedService";
    string blobDatasetName = "BlobDataset";
    string sqlDatasetName = "SqlDataset";
    string pipelineName = "Adfv2TutorialBlobToSqlCopy";
    ```

3. Aşağıdaki kodu `DataFactoryManagementClient` sınıfının bir örneğini oluşturan `Main` yöntemine ekleyin. Veri fabrikası, bağlı hizmet, veri kümeleri ve işlem hattı oluşturmak için bu nesneyi kullanırsınız. Bu nesneyi ayrıca işlem hattı ayrıntılarını izlemek için kullanabilirsiniz.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync(
        "https://management.azure.com/", cc
    ).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Aşağıdaki kodu bir *Veri Fabrikası*oluşturan `Main` yöntemine ekleyin.

```csharp
// Create a data factory
Console.WriteLine("Creating a data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};

client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings)
);

while (
    client.Factories.Get(
        resourceGroup, dataFactoryName
    ).ProvisioningState == "PendingCreation"
)
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu öğreticide, sırasıyla kaynak ve havuz için iki bağlı hizmet oluşturursunuz.

### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

Aşağıdaki kodu, *Azure depolama bağlı hizmeti*oluşturan `Main` yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için bkz. [Azure Blob bağlı hizmet özellikleri](connector-azure-blob-storage.md#linked-service-properties).

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString(
            "DefaultEndpointsProtocol=https;AccountName=" + storageAccount +
            ";AccountKey=" + storageKey
        )
    }
);

client.LinkedServices.CreateOrUpdate(
    resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings)
);
```

### <a name="create-an-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti oluşturma

Aşağıdaki kodu bir *Azure SQL veritabanı bağlı hizmeti*oluşturan `Main` yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlı hizmeti özellikleri](connector-azure-sql-database.md#linked-service-properties).

```csharp
// Create an Azure SQL Database linked service
Console.WriteLine("Creating linked service " + sqlDbLinkedServiceName + "...");

LinkedServiceResource sqlDbLinkedService = new LinkedServiceResource(
    new AzureSqlDatabaseLinkedService
    {
        ConnectionString = new SecureString(azureSqlConnString)
    }
);

client.LinkedServices.CreateOrUpdate(
    resourceGroup, dataFactoryName, sqlDbLinkedServiceName, sqlDbLinkedService
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(sqlDbLinkedService, client.SerializationSettings)
);
```

## <a name="create-datasets"></a>Veri kümeleri oluşturun

Bu bölümde, kaynak için diğeri havuz için olmak üzere iki veri kümesi oluşturursunuz. 

### <a name="create-a-dataset-for-source-azure-blob"></a>Kaynak Azure Blob için veri kümesi oluşturma

Aşağıdaki kodu bir *Azure blob veri kümesi*oluşturan `Main` yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için bkz. [Azure blob veri kümesi özellikleri](connector-azure-blob-storage.md#dataset-properties).

Azure Blob’da kaynak verilerini temsil eden bir veri kümesi tanımlayın. Bu Blob veri kümesi, önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetini ifade eder:

- Kopyalanacak Blobun konumu: `FolderPath` ve `FileName`
- İçeriğin nasıl ayrıştıralınacağını belirten blob biçimi: `TextFormat` ve, sütun sınırlayıcısı gibi ayarları
- Bu örnekte havuz SQL tablosuna eşlenen sütun adları ve veri türleri de dahil olmak üzere veri yapısı

```csharp
// Create an Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference { 
            ReferenceName = storageLinkedServiceName 
        },
        FolderPath = inputBlobPath,
        FileName = inputBlobName,
        Format = new TextFormat { ColumnDelimiter = "|" },
        Structure = new List<DatasetDataElement>
        {
            new DatasetDataElement { Name = "FirstName", Type = "String" },
            new DatasetDataElement { Name = "LastName", Type = "String" }
        }
    }
);

client.Datasets.CreateOrUpdate(
    resourceGroup, dataFactoryName, blobDatasetName, blobDataset
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings)
);
```

### <a name="create-a-dataset-for-sink-azure-sql-database"></a>Havuz Azure SQL Veritabanı için veri kümesi oluşturma

Aşağıdaki kodu bir *Azure SQL veritabanı veri kümesi*oluşturan `Main` yöntemine ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı veri kümesi özellikleri](connector-azure-sql-database.md#dataset-properties).

Azure SQL Veritabanı’nda havuz verilerini temsil eden bir veri kümesi tanımlayın. Bu veri kümesi, önceki adımda oluşturduğunuz Azure SQL veritabanı bağlı hizmetini ifade eder. Ayrıca, kopyalanan verileri tutan SQL tablosunu belirtir. 

```csharp
// Create an Azure SQL Database dataset
Console.WriteLine("Creating dataset " + sqlDatasetName + "...");
DatasetResource sqlDataset = new DatasetResource(
    new AzureSqlTableDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = sqlDbLinkedServiceName
        },
        TableName = azureSqlTableName
    }
);

client.Datasets.CreateOrUpdate(
    resourceGroup, dataFactoryName, sqlDatasetName, sqlDataset
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(sqlDataset, client.SerializationSettings)
);
```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Aşağıdaki kodu *bir kopyalama etkinliği ile işlem hattı*oluşturan `Main` yöntemine ekleyin. Bu öğreticide, bu işlem hattı bir etkinlik içerir: blob veri kümesini kaynak olarak ve SQL veri kümesi havuz olarak alan `CopyActivity`. Kopyalama etkinliği ayrıntıları hakkında daha fazla bilgi için bkz. [Azure Data Factory etkinliği kopyalama](copy-activity-overview.md).

```csharp
// Create a pipeline with copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToSQL",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference() { ReferenceName = blobDatasetName }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference { ReferenceName = sqlDatasetName }
            },
            Source = new BlobSource { },
            Sink = new SqlSink { }
        }
    }
};

client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings)
);
```

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Aşağıdaki kodu *bir işlem hattı çalıştırmasını tetikleyen*`Main` yöntemine ekleyin.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(
    resourceGroup, dataFactoryName, pipelineName
).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

Şimdi işlem hattı çalıştırma durumlarını denetlemek ve kopyalama etkinliği çalıştırma hakkındaki ayrıntıları almak için kodu ekleyin.

1. Veri kopyalamayı tamamlayana kadar işlem hattı çalıştırmasının durumlarını sürekli olarak denetlemek için `Main` yöntemine aşağıdaki kodu ekleyin.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(
            resourceGroup, dataFactoryName, runResponse.RunId
        );
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. , Okunan veya yazılan verilerin boyutu gibi kopyalama etkinliği çalıştırma ayrıntılarını alan `Main` yöntemine aşağıdaki kodu ekleyin.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");

    RunFilterParameters filterParams = new RunFilterParameters(
        DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)
    );

    ActivityRunsQueryResponse queryResponse = client.ActivityRuns.QueryByPipelineRun(
        resourceGroup, dataFactoryName, runResponse.RunId, filterParams
    );
 
    if (pipelineRun.Status == "Succeeded")
    {
        Console.WriteLine(queryResponse.Value.First().Output);
    }
    else
        Console.WriteLine(queryResponse.Value.First().Error);
    
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Kodu çalıştırma

**Build** > **Build Solution**öğesini seçerek uygulamayı derleyin. Ardından **hata ayıkla** > hata **ayıklamayı Başlat**' ı seçerek uygulamayı başlatın ve işlem hattı yürütmesini doğrulayın.

Konsol; veri fabrikası, bağlı hizmet, veri kümeleri, işlem hattı ve işlem hattı çalıştırmasının ilerleme durumunu yazdırır. Daha sonra işlem hattı çalıştırma durumunu denetler. Kopyalama etkinliğinin çalışma ayrıntılarını görene kadar bekleyin ve okunan veri okuma/yazma boyutudur. Daha sonra, SQL Server Management Studio (SSMS) veya Visual Studio gibi araçları kullanarak hedef Azure SQL veritabanınıza bağlanabilir ve belirttiğiniz hedef tablonun kopyalanmış verileri içerip içermediğini kontrol edebilirsiniz.

### <a name="sample-output"></a>Örnek çıktı

```json
Creating a data factory AdfV2Tutorial...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "type": "SecureString",
        "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>"
      }
    }
  }
}
Creating linked service AzureSqlDbLinkedService...
{
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": {
        "type": "SecureString",
        "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
      }
    }
  }
}
Creating dataset BlobDataset...
{
  "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": "adfv2tutorial/",
      "fileName": "inputEmp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "|"
      }
    },
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "linkedServiceName": {
      "type": "LinkedServiceReference",
      "referenceName": "AzureStorageLinkedService"
    }
  }
}
Creating dataset SqlDataset...
{
  "properties": {
    "type": "AzureSqlTable",
    "typeProperties": {
      "tableName": "dbo.emp"
    },
    "linkedServiceName": {
      "type": "LinkedServiceReference",
      "referenceName": "AzureSqlDbLinkedService"
    }
  }
}
Creating pipeline Adfv2TutorialBlobToSqlCopy...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "inputs": [
          {
            "type": "DatasetReference",
            "referenceName": "BlobDataset"
          }
        ],
        "outputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SqlDataset"
          }
        ],
        "name": "CopyFromBlobToSQL"
      }
    ]
  }
}
Creating pipeline run...
Pipeline run ID: 1cd03653-88a0-4c90-aabc-ae12d843e252
Checking pipeline run status...
Status: InProgress
Status: InProgress
Status: Succeeded
Checking copy activity run details...
{
  "dataRead": 18,
  "dataWritten": 28,
  "rowsCopied": 2,
  "copyDuration": 2,
  "throughput": 0.01,
  "errors": [],
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
  "usedDataIntegrationUnits": 2,
  "billedDuration": 2
}

Press any key to exit...
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure Depolama ve Azure SQL Veritabanı bağlı hizmeti oluşturma.
> * Azure Blob ve Azure SQL Veritabanı veri kümeleri oluşturma.
> * Kopyalama etkinliği içeren bir işlem hattı oluşturun.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Buluttan şirket içine veri kopyalama](tutorial-hybrid-copy-powershell.md)
