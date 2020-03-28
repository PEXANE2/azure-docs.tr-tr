---
title: Azure Blob Depolama'dan Azure SQL Veritabanına veri kopyalama
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
ms.openlocfilehash: 93c4f71c762cff3e3f5a01f0e2595f3498f9d38d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75977323"
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob’dan Azure SQL Veritabanına veri kopyalama

Bu öğreticide, Azure Blob Depolama alanından Azure SQL Veritabanına veri kopyalayan bir Data Factory işlem hattı oluşturacaksınız. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.

Bu öğreticide aşağıdaki adımları atarsınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure Depolama ve Azure SQL Veritabanı bağlı hizmeti oluşturma.
> * Azure Blob ve Azure SQL Veritabanı veri kümeleri oluşturma.
> * Kopyalama etkinliği içeren bir işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Bu öğreticide .NET SDK kullanılır. Azure Veri Fabrikası ile etkileşim kurmak için diğer mekanizmaları kullanabilirsiniz; **Quickstarts**altındaki örneklere bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* *Azure Depolama hesabı.* Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, [bkz.](../storage/common/storage-account-create.md)
* *Azure SQL Veritabanı*. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. Azure SQL Veritabanınız [yoksa,](../sql-database/sql-database-single-database-get-started.md)bkz.
* *Görsel Stüdyo*. Bu makaledeki walkthrough Visual Studio 2019 kullanır.
* *[.NET için Azure SDK](/dotnet/azure/dotnet-tools)*.
* *Azure Active Directory uygulaması*. Bir Azure Etkin Dizin uygulamanız yoksa, [Azure AD uygulaması oluşturmak için portalı kullanma: Nasıl](../active-directory/develop/howto-create-service-principal-portal.md)Oluşturulacak'ın Bir Azure Etkin [Dizin Uygulaması Oluştur](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) bölümüne bakın. Sonraki adımlarda kullanılmak üzere aşağıdaki değerleri kopyalayın: **Uygulama (istemci) Kimliği,** **kimlik doğrulama anahtarı**ve **Dizin (kiracı) Kimliği**. Aynı makaledeki yönergeleri izleyerek uygulamayı **Katılımcı** rolüne atayın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Şimdi, bir kaynak blog ve lavabo SQL tablosu oluşturarak Azure Blob ve Azure SQL Veritabanınızı öğreticiye hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

İlk olarak, bir kapsayıcı oluşturarak ve bir giriş metin dosyası yükleyerek bir kaynak blob oluşturun:

1. Not Defteri'ni açın. Aşağıdaki metni kopyalayın ve yerel olarak *inputEmp.txt*adlı bir dosyaya kaydedin.

    ```inputEmp.txt
    John|Doe
    Jane|Doe
    ```

2. *Adfv2tutorial* kapsayıcısını oluşturmak ve *girişEmp.txt* dosyasını kapsayıcıya yüklemek için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi bir araç kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

Ardından, bir lavabo SQL tablosu oluşturun:

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

2. Azure hizmetlerinin SQL sunucusuna erişmesine izin ver. Veri Fabrikası hizmetinin Azure SQL sunucunuza veri yazabilmesi için Azure SQL sunucunuzdaki Azure hizmetlerine erişime izin verdiğinizden emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    1. SQL sunucunuzu yönetmek için [Azure portalına](https://portal.azure.com) gidin. **SQL sunucularını**arayın ve seçin.

    2. Sunucunuzu seçin.

    3. SQL sunucu menüsünün **Güvenlik** başlığı altında **Güvenlik Duvarları ve sanal ağları**seçin.

    4. Güvenlik **Duvarı ve sanal ağlar** sayfasında, **Azure hizmetlerine ve kaynaklarına bu sunucuya erişmeye izin**ver **altında, A.B.'yi**seçin.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'yu kullanarak C# .NET konsol uygulaması oluşturun.

1. Visual Studio'yu açın.
2. **Başlat** penceresinde yeni **bir proje oluştur'u**seçin.
3. Yeni **bir proje oluşturma** penceresinde, proje türleri listesinden **Konsol Uygulamasının (.NET Framework)** C# sürümünü seçin. Ardından **İleri'yi**seçin.
4. Yeni **proje pencerenizi Yapılandır'a** *ADFv2Tutorial'in* **bir Proje adını** girin. **Konum**için, projeyi kaydetmek için dizine göz atın ve/veya oluşturun. Ardından **Oluştur**’u seçin. Yeni proje Visual Studio IDE görünür.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

Ardından, NuGet paket yöneticisini kullanarak gerekli kitaplık paketlerini yükleyin.

1. Menü çubuğunda **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**seçin.
2. Paket **Yöneticisi Konsol** bölmesinde, paketleri yüklemek için aşağıdaki komutları çalıştırın. Azure Veri Fabrikası NuGet paketi hakkında daha fazla bilgi için [Microsoft.Azure.Management.DataFactory](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/)adresine bakın.

    ```package manager console
    Install-Package Microsoft.Azure.Management.DataFactory
    Install-Package Microsoft.Azure.Management.ResourceManager -PreRelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-data-factory-client"></a>Veri fabrikası istemcisi oluşturma

Bir veri fabrikası istemcisi oluşturmak için aşağıdaki adımları izleyin.

1. *Program.cs*açın, ardından ad `using` alanlarına başvuru eklemek için aşağıdaki kodla varolan deyimlerin üzerine yazın.

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

2. Değişkenleri ayarlayan `Main` yönteme aşağıdaki kodu ekleyin. 14 yer tutucuyu kendi değerlerinizle değiştirin.

    Veri Fabrikası'nın şu anda kullanılabildiği Azure bölgelerinin listesini görmek için [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/)bakın. **Ürünler** açılır listesi altında,**Analytics** > Veri **Fabrikası'na Gözat'ı** > seçin.**Data Factory** Ardından **Bölgeler** açılır listesinde, ilginizi çeken bölgeleri seçin. Seçtiğiniz bölgeler için Veri Fabrikası ürünlerinin kullanılabilirlik durumuna sahip bir ızgara görüntülenir.

    > [!NOTE]
    > Azure Depolama ve Azure SQL Veritabanı gibi veri depoları ve Veri Fabrikası'nın kullandığı HDInsight gibi hesaplamalar, Veri Fabrikası için seçtiğinizden başka bölgelerde olabilir.

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

3. Sınıfın bir örneğini `Main` `DataFactoryManagementClient` oluşturan yönteme aşağıdaki kodu ekleyin. Veri fabrikası, bağlı hizmet, veri kümeleri ve işlem hattı oluşturmak için bu nesneyi kullanırsınız. Bu nesneyi ayrıca işlem hattı ayrıntılarını izlemek için kullanabilirsiniz.

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

*Veri fabrikası*oluşturan `Main` yönteme aşağıdaki kodu ekleyin.

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

Bu öğreticide, kaynak ve lavabo için sırasıyla iki bağlantılı hizmet oluşturursunuz.

### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

Azure Depolama bağlantılı `Main` bir *hizmet*oluşturan yönteme aşağıdaki kodu ekleyin. Desteklenen özellikler ve ayrıntılar hakkında bilgi için [Azure Blob bağlantılı hizmet özelliklerine](connector-azure-blob-storage.md#linked-service-properties)bakın.

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

Azure SQL Veritabanı `Main` bağlantılı bir *hizmet*oluşturan yönteme aşağıdaki kodu ekleyin. Desteklenen özellikler ve ayrıntılar hakkında bilgi için [Azure SQL Veritabanı bağlantılı hizmet özellikleri'ne](connector-azure-sql-database.md#linked-service-properties)bakın.

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

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bu bölümde, biri kaynak, diğeri lavabo için olmak üzere iki veri kümesi oluşturursunuz.

### <a name="create-a-dataset-for-source-azure-blob"></a>Kaynak Azure Blob için veri kümesi oluşturma

*Azure blob veri kümesi*oluşturan `Main` yönteme aşağıdaki kodu ekleyin. Desteklenen özellikler ve ayrıntılar hakkında bilgi için [Azure Blob veri kümesi özelliklerine](connector-azure-blob-storage.md#dataset-properties)bakın.

Azure Blob’da kaynak verilerini temsil eden bir veri kümesi tanımlayın. Bu Blob veri kümesi, önceki adımda oluşturduğunuz Azure Depolama bağlı hizmetini ifade eder:

- Kopyalamak için blob konumu: `FolderPath` ve`FileName`
- İçeriği niçin ayrışdırılabildiğini belirten `TextFormat` blob biçimi: ve sütun sınırlayıcı gibi ayarları
- Bu örnekte lavabo SQL tablosuna eşleyen sütun adları ve veri türleri de dahil olmak üzere veri yapısı

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

Azure SQL Veritabanı `Main` veri *kümesi*oluşturan yönteme aşağıdaki kodu ekleyin. Desteklenen özellikler ve ayrıntılar hakkında bilgi için [Azure SQL Veritabanı veri kümesi özelliklerine](connector-azure-sql-database.md#dataset-properties)bakın.

Azure SQL Veritabanı’nda havuz verilerini temsil eden bir veri kümesi tanımlayın. Bu veri kümesi, önceki adımda oluşturduğunuz Azure SQL Veritabanı bağlantılı hizmetanlamına gelir. Ayrıca, kopyalanan verileri tutan SQL tablosunu belirtir.

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

Kopyalama etkinliği olan `Main` bir *ardışık hatlar*oluşturan yönteme aşağıdaki kodu ekleyin. Bu öğreticide, bu ardışık `CopyActivity`işlem bir etkinlik içerir: Blob veri kümesini kaynak olarak ve SQL veri kümesini lavabo olarak alır. Kopyalama etkinliği ayrıntıları hakkında bilgi için Azure [Veri Fabrikası'ndaki Kopyalama etkinliği'ne](copy-activity-overview.md)bakın.

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

Bir `Main` *ardışık sistem çalışmasını tetikleyen*yönteme aşağıdaki kodu ekleyin.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(
    resourceGroup, dataFactoryName, pipelineName
).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

Şimdi, ardışık hatlar denetim durumlarını denetlemek ve kopyalama etkinliği çalışması hakkında ayrıntılı bilgi almak için kodu ekleyin.

1. Verileri kopyalamayı bitirene kadar ardışık sistem çalışmasının durumlarını sürekli olarak denetlemek için `Main` yönteme aşağıdaki kodu ekleyin.

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

2. Okundu veya yazılan `Main` verilerin boyutu gibi kopyalama etkinliği çalıştırayrıntılarını alan yönteme aşağıdaki kodu ekleyin.

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

**Yapı** > **Çözüm'üni**seçerek uygulamayı oluşturun. Ardından **Hata** > **Ayıklama Başlat Hata Ayıklama'yı**seçerek uygulamayı başlatın ve ardışık işlem yürütmeyi doğrulayın.

Konsol; veri fabrikası, bağlı hizmet, veri kümeleri, işlem hattı ve işlem hattı çalıştırmasının ilerleme durumunu yazdırır. Daha sonra işlem hattı çalıştırma durumunu denetler. Kopya etkinliği ayrıntılarının veri okuma/yazılı boyutuyla çalıştırıldığını görene kadar bekleyin. Ardından, SQL Server Management Studio (SSMS) veya Visual Studio gibi araçları kullanarak, hedef Azure SQL Veritabanı'na bağlanabilir ve belirttiğiniz hedef tablonun kopyalanan verileri bulunup içermediğini kontrol edebilirsiniz.

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
> * Kopyalama etkinliği içeren bir ardışık hatlar oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Şirket içinden buluta veri kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Buluttan şirket içine veri kopyalama](tutorial-hybrid-copy-powershell.md)
