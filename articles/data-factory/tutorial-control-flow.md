---
title: Azure Veri Fabrikası'nda Dallanma
description: Dallanma ve zincirleme etkinlikleriyle Azure Data Factory'de veri akışını denetleme hakkında bilgi edinin.
services: data-factory
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 9/27/2019
ms.openlocfilehash: 77fa8f72d4d4d929d15859fde71f112de1ddd14e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418737"
---
# <a name="branching-and-chaining-activities-in-a-data-factory-pipeline"></a>Data Factory işlem hattında dallanma ve zincirleme etkinlikleri

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu öğreticide, bazı denetim akışı özelliklerini sergileyen bir Veri Fabrikası ardışık hattı oluşturursunuz. Bu ardışık işlem, Azure Blob Depolama'daki bir kapsayıcıdan aynı depolama hesabındaki başka bir kapsayıcıya kopyalanır. Kopyalama etkinliği başarılı olursa, ardışık işlem başarılı kopyalama işleminin ayrıntılarını bir e-postayla gönderir. Bu bilgiler, yazılan veri miktarını içerebilir. Kopyalama etkinliği başarısız olursa, bir e-postada hata iletisi gibi kopyalama hatasının ayrıntılarını gönderir. Öğretici boyunca parametreleri nasıl geçireceğinizi göreceksiniz.

Bu grafik, senaryoya genel bir bakış sağlar:

![Genel Bakış](media/tutorial-control-flow/overview.png)

Bu öğretici, aşağıdaki görevleri nasıl yapacağınızı gösterir:

> [!div class="checklist"]
> * Veri fabrikası oluşturma
> * Azure Depolama bağlı hizmeti oluşturma
> * Azure blob veri kümesi oluşturma
> * Kopyalama etkinliği ve bir web etkinliği içeren işlem hattı oluşturma
> * Etkinliklerin çıktılarını sonraki etkinliklere gönderme
> * Parametre geçişi ve sistem değişkenlerini kullanma
> * Bir işlem hattı çalıştırması başlatma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme

Bu öğreticide .NET SDK kullanılır. Azure Veri Fabrikası ile etkileşim kurmak için diğer mekanizmaları kullanabilirsiniz. Veri Fabrikası hızlı başlangıçlar için, [5-Minute Quickstarts](/azure/data-factory/quickstart-create-data-factory-portal)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Depolama hesabı. Kaynak veri deposu olarak blob depolama kullanın. Azure depolama hesabınız yoksa, [bkz.](../storage/common/storage-account-create.md)
* Azure Depolama Gezgini. Bu aracı yüklemek için [Azure Depolama Gezgini'ne](https://storageexplorer.com/)bakın.
* Azure SQL Veritabanı çözümünü karşılaştırabilirsiniz. Veritabanını havuz veri deposu olarak kullanabilirsiniz. Azure SQL Veritabanınız [yoksa,](../sql-database/sql-database-get-started-portal.md)bkz.
* Visual Studio. Bu makale, Visual Studio 2019'u kullansın.
* Azure .NET SDK. [Azure .NET SDK'yı](https://azure.microsoft.com/downloads/)indirin ve kurun.

Veri Fabrikası'nın şu anda kullanılabildiği Azure bölgelerinin listesi için, [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/)bakın. Veri depoları ve hesaplamaları başka bölgelerde olabilir. Mağazalar, Azure Depolama ve Azure SQL Veritabanı'nı içerir. İşlemler, Veri Fabrikası'nın kullandığı HDInsight'ı içerir.

[Azure Etkin Dizin oluştur uygulamasında](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)açıklandığı gibi bir uygulama oluşturun. Aynı makalede talimatları izleyerek uygulamayı **Katılımcı** rolüne atayın. Bu öğreticinin uygulama **(istemci) kimliği** ve **Dizin (kiracı) kimliği**gibi sonraki bölümleri için çeşitli değerlere ihtiyacınız vardır.

### <a name="create-a-blob-table"></a>Blob tablosu oluşturma

1. Bir metin düzenleyicisi açın. Aşağıdaki metni kopyalayın ve *input.txt*olarak yerel olarak kaydedin.

   ```
   Ethel|Berg
   Tamika|Walsh
   ```

1. Azure Depolama Gezgini'ni açın. Depolama hesabınızı genişletin. Blob **Containers** sağ tıklatın ve **Blob Konteyner oluştur'u**seçin.
1. Yeni kapsayıcı *adfv2branch'ı* adlandırın ve *input.txt* dosyanızı kapsayıcıya eklemek için **Yükle'yi** seçin.

## <a name="create-visual-studio-project"></a>Visual Studio projesi oluşturma<a name="create-visual-studio-project"></a>

C# .NET konsol uygulaması oluşturun:

1. Visual Studio'yı başlatın ve **yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da**C# için Konsol **Uygulaması'nı (.NET Framework)** seçin ve **İleri'yi**seçin.
1. Proje *ADFv2BranchTutorial*adı .
1. **.NET sürüm 4.5.2** veya üzerini seçin ve sonra **Oluştur'u**seçin.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

1. **Araçları** > **nuget paket yöneticisi** > **paket yöneticisi konsolseçin.**
1. Paket **Yöneticisi Konsolunda,** paketleri yüklemek için aşağıdaki komutları çalıştırın. Ayrıntılar için [Microsoft.Azure.Management.DataFactory nuget paketine](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/) bakın.

   ```powershell
   Install-Package Microsoft.Azure.Management.DataFactory
   Install-Package Microsoft.Azure.Management.ResourceManager -IncludePrerelease
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

### <a name="create-a-data-factory-client"></a>Veri fabrikası istemcisi oluşturma

1. *açık Program.cs* ve aşağıdaki ifadeleri ekleyin:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using Microsoft.Rest;
   using Microsoft.Azure.Management.ResourceManager;
   using Microsoft.Azure.Management.DataFactory;
   using Microsoft.Azure.Management.DataFactory.Models;
   using Microsoft.IdentityModel.Clients.ActiveDirectory;
   ```

1. Bu statik değişkenleri `Program` sınıfa ekleyin. Yer tutucuları kendi değerlerinizle değiştirin.

   ```csharp
   // Set variables
   static string tenantID = "<tenant ID>";
   static string applicationId = "<application ID>";
   static string authenticationKey = "<Authentication key for your application>";
   static string subscriptionId = "<Azure subscription ID>";
   static string resourceGroup = "<Azure resource group name>";

   static string region = "East US";
   static string dataFactoryName = "<Data factory name>";

   // Specify the source Azure Blob information
   static string storageAccount = "<Azure Storage account name>";
   static string storageKey = "<Azure Storage account key>";
   // confirm that you have the input.txt file placed in th input folder of the adfv2branch container.
   static string inputBlobPath = "adfv2branch/input";
   static string inputBlobName = "input.txt";
   static string outputBlobPath = "adfv2branch/output";
   static string emailReceiver = "<specify email address of the receiver>";

   static string storageLinkedServiceName = "AzureStorageLinkedService";
   static string blobSourceDatasetName = "SourceStorageDataset";
   static string blobSinkDatasetName = "SinkStorageDataset";
   static string pipelineName = "Adfv2TutorialBranchCopy";

   static string copyBlobActivity = "CopyBlobtoBlob";
   static string sendFailEmailActivity = "SendFailEmailActivity";
   static string sendSuccessEmailActivity = "SendSuccessEmailActivity";
   ```

1. `Main` yöntemine aşağıdaki kodu ekleyin. Bu kod `DataFactoryManagementClient` sınıf bir örnek oluşturur. Daha sonra veri fabrikası, bağlantılı hizmet, veri kümeleri ve ardışık işlem oluşturmak için bu nesneyi kullanırsınız. Bu nesneyi, ardışık hatlar çalışır ayrıntılarını izlemek için de kullanabilirsiniz.

   ```csharp
   // Authenticate and create a data factory management client
   var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
   ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
   AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
   ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
   var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
   ```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Program.cs `CreateOrUpdateDataFactory` dosyanıza *Program.cs* bir yöntem ekleyin:

   ```csharp
   static Factory CreateOrUpdateDataFactory(DataFactoryManagementClient client)
   {
       Console.WriteLine("Creating data factory " + dataFactoryName + "...");
       Factory resource = new Factory
       {
           Location = region
       };
       Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));

       Factory response;
       {
           response = client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, resource);
       }

       while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
       {
           System.Threading.Thread.Sleep(1000);
       }
       return response;
   }
   ```

1. Veri fabrikası oluşturan `Main` yönteme aşağıdaki satırı ekleyin:

   ```csharp
   Factory df = CreateOrUpdateDataFactory(client);
   ```

## <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

1. Program.cs `StorageLinkedServiceDefinition` dosyanıza *Program.cs* bir yöntem ekleyin:

   ```csharp
   static LinkedServiceResource StorageLinkedServiceDefinition(DataFactoryManagementClient client)
   {
      Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");
      AzureStorageLinkedService storageLinkedService = new AzureStorageLinkedService
      {
          ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
      };
      Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
      LinkedServiceResource linkedService = new LinkedServiceResource(storageLinkedService, name:storageLinkedServiceName);
      return linkedService;
   }
   ```

1. Azure Depolama bağlantılı `Main` bir hizmet oluşturan yönteme aşağıdaki satırı ekleyin:

   ```csharp
   client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
   ```

Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için [Bkz. Bağlantılı hizmet özellikleri.](connector-azure-blob-storage.md#linked-service-properties)

## <a name="create-datasets"></a>Veri kümeleri oluşturma

Bu bölümde, biri kaynak, diğeri lavabo için olmak üzere iki veri kümesi oluşturursunuz.

### <a name="create-a-dataset-for-a-source-azure-blob"></a>Kaynak Azure Blob için veri kümesi oluşturma

*Azure blob veri kümesi*oluşturan bir yöntem ekleyin. Desteklenen özellikler ve ayrıntılar hakkında daha fazla bilgi için [Azure Blob veri kümesi özelliklerine](connector-azure-blob-storage.md#dataset-properties)bakın.

Program.cs `SourceBlobDatasetDefinition` dosyanıza *Program.cs* bir yöntem ekleyin:

```csharp
static DatasetResource SourceBlobDatasetDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating dataset " + blobSourceDatasetName + "...");
    AzureBlobDataset blobDataset = new AzureBlobDataset
    {
        FolderPath = new Expression { Value = "@pipeline().parameters.sourceBlobContainer" },
        FileName = inputBlobName,
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        }
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
    DatasetResource dataset = new DatasetResource(blobDataset, name:blobSourceDatasetName);
    return dataset;
}
```

Azure Blob’da kaynak verilerini temsil eden bir veri kümesi tanımlayın. Bu Blob veri kümesi, önceki adımda desteklenen Azure Depolama bağlantılı hizmetanlamına gelir. Blob veri kümesi, kopyalanması gereken blob'un konumunu açıklar: *FolderPath* ve *FileName.*

*FolderPath*için parametrelerin kullanımına dikkat edin. `sourceBlobContainer`parametrenin adıdır ve ifade, ardışık hatlar çalıştırında geçirilen değerlerle değiştirilir. Parametreleri tanımlamaya yönelik söz dizimi `@pipeline().parameters.<parameterName>`

### <a name="create-a-dataset-for-a-sink-azure-blob"></a>Lavabo Azure Blob için veri kümesi oluşturma

1. Program.cs `SourceBlobDatasetDefinition` dosyanıza *Program.cs* bir yöntem ekleyin:

   ```csharp
   static DatasetResource SinkBlobDatasetDefinition(DataFactoryManagementClient client)
   {
       Console.WriteLine("Creating dataset " + blobSinkDatasetName + "...");
       AzureBlobDataset blobDataset = new AzureBlobDataset
       {
           FolderPath = new Expression { Value = "@pipeline().parameters.sinkBlobContainer" },
           LinkedServiceName = new LinkedServiceReference
           {
               ReferenceName = storageLinkedServiceName
           }
       };
       Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
       DatasetResource dataset = new DatasetResource(blobDataset, name: blobSinkDatasetName);
       return dataset;
   }
   ```

1. Hem Azure Blob `Main` kaynağı hem de lavabo veri kümelerini oluşturan yönteme aşağıdaki kodu ekleyin.

   ```csharp
   client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));

   client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));
   ```

## <a name="create-a-c-class-emailrequest"></a>C# sınıfı oluşturma: EmailRequest

C# projenizde, adı bir `EmailRequest`sınıf oluşturun. Bu sınıf, e-posta gönderirken ardışık kuruluşun gövde isteğine hangi özellikleri gönderdiğini tanımlar. Bu öğreticide işlem hattı, işlem hattından e-postaya dört özellik gönderir:

* İleti. E-postanın gövdesi. Başarılı bir kopya için bu özellik, yazılan veri miktarını içerir. Başarısız bir kopya için bu özellik hatanın ayrıntılarını içerir.
* Veri fabrikası adı. Veri fabrikasının adı.
* Boru hattı adı. İşlem hattının adı.
* Alıcı. Parametre geçer. Bu özellik, e-postanın alıcısını belirtir.

```csharp
    class EmailRequest
    {
        [Newtonsoft.Json.JsonProperty(PropertyName = "message")]
        public string message;

        [Newtonsoft.Json.JsonProperty(PropertyName = "dataFactoryName")]
        public string dataFactoryName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "pipelineName")]
        public string pipelineName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "receiver")]
        public string receiver;

        public EmailRequest(string input, string df, string pipeline, string receiverName)
        {
            message = input;
            dataFactoryName = df;
            pipelineName = pipeline;
            receiver = receiverName;
        }
    }
```

## <a name="create-email-workflow-endpoints"></a>E-posta iş akışı uç noktaları oluşturma

E-posta göndermeyi tetiklemek için, [Logic Apps](../logic-apps/logic-apps-overview.md) kullanarak iş akışını tanımlayın. Mantık Uygulamaları iş akışı oluşturma yla ilgili ayrıntılar için, [Mantık Uygulaması oluşturma hakkında](../logic-apps/quickstart-create-first-logic-app-workflow.md)bilgi edinin.

### <a name="success-email-workflow"></a>Başarı e-postası iş akışı

Azure [portalında,](https://portal.azure.com) *CopySuccessEmail*adlı bir Mantık Uygulamaları iş akışı oluşturun. İş akışı tetikleyicisini `When an HTTP request is received`. İstek tetikleyiciniz için `Request Body JSON Schema` alanını aşağıdaki JSON ile doldurun:

```json
{
    "properties": {
        "dataFactoryName": {
            "type": "string"
        },
        "message": {
            "type": "string"
        },
        "pipelineName": {
            "type": "string"
        },
        "receiver": {
            "type": "string"
        }
    },
    "type": "object"
}
```

İş akışınız aşağıdaki örneğe benzer:

![Başarı e-postası iş akışı](media/tutorial-control-flow/success-email-workflow-trigger.png)

Bu JSON içeriği, `EmailRequest` önceki bölümde oluşturduğunuz sınıfla uyumlu.

Bir eylem `Office 365 Outlook – Send an email`ekleyin. **E-posta gönder** eylemi için, **Body** JSON şemasında geçirilen özellikleri kullanarak e-postayı nasıl biçimlendirmek istediğinizi özelleştirin. Bir örneği aşağıda verilmiştir:

![Mantık uygulaması tasarımcısı - e-posta eylem göndermek](media/tutorial-control-flow/customize-send-email-action.png)

İş akışını kurtardıktan sonra HTTP **POST URL** değerini tespeden kopyalayın ve kaydedin.

## <a name="fail-email-workflow"></a>Hata e-postası iş akışı

Klon **CopySuccessEmail** *copyfailemail*adlı başka bir Mantık Apps iş akışı olarak . İstek tetikleyicisinde `Request Body JSON schema` değeri aynıdır. Hata e-postasına uyarlamak için e-postanızın biçimini `Subject` olarak değiştirin. Örnek aşağıda verilmiştir:

![Mantık uygulaması tasarımcısı - başarısız e-posta iş akışı](media/tutorial-control-flow/fail-email-workflow.png)

İş akışını kurtardıktan sonra HTTP **POST URL** değerini tespeden kopyalayın ve kaydedin.

Şimdi aşağıdaki örnekler gibi iki iş akışı URL'niz olmalıdır:

```csharp
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000

//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Visual Studio'daki projenize geri dön. Şimdi, kopyalama etkinliği ve `DependsOn` özelliği olan bir ardışık hatlar oluşturan kodu ekleyeceğiz. Bu öğreticide, ardışık işlem, blob veri kümesini kaynak olarak alan bir etkinlik, bir kopyalama etkinliği ve lavabo olarak başka bir Blob veri kümesi içerir. Kopyalama etkinliği başarılı olursa veya başarısız olursa, farklı e-posta görevleri çağırır.

Bu işlem hattında aşağıdaki özellikleri kullanırsınız:

* Parametreler
* Web etkinliği
* Etkinlik bağımlılığı
* Bir etkinlikten çıktıyı başka bir faaliyete girdi olarak kullanma

1. Bu yöntemi projenize ekleyin. Aşağıdaki bölümler daha ayrıntılı olarak sağlar.

    ```csharp
    static PipelineResource PipelineDefinition(DataFactoryManagementClient client)
            {
                Console.WriteLine("Creating pipeline " + pipelineName + "...");
                PipelineResource resource = new PipelineResource
                {
                    Parameters = new Dictionary<string, ParameterSpecification>
                    {
                        { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                        { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                        { "receiver", new ParameterSpecification { Type = ParameterType.String } }

                    },
                    Activities = new List<Activity>
                    {
                        new CopyActivity
                        {
                            Name = copyBlobActivity,
                            Inputs = new List<DatasetReference>
                            {
                                new DatasetReference
                                {
                                    ReferenceName = blobSourceDatasetName
                                }
                            },
                            Outputs = new List<DatasetReference>
                            {
                                new DatasetReference
                                {
                                    ReferenceName = blobSinkDatasetName
                                }
                            },
                            Source = new BlobSource { },
                            Sink = new BlobSink { }
                        },
                        new WebActivity
                        {
                            Name = sendSuccessEmailActivity,
                            Method = WebActivityMethod.POST,
                            Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/00000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000000",
                            Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                            DependsOn = new List<ActivityDependency>
                            {
                                new ActivityDependency
                                {
                                    Activity = copyBlobActivity,
                                    DependencyConditions = new List<String> { "Succeeded" }
                                }
                            }
                        },
                        new WebActivity
                        {
                            Name = sendFailEmailActivity,
                            Method =WebActivityMethod.POST,
                            Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000",
                            Body = new EmailRequest("@{activity('CopyBlobtoBlob').error.message}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                            DependsOn = new List<ActivityDependency>
                            {
                                new ActivityDependency
                                {
                                    Activity = copyBlobActivity,
                                    DependencyConditions = new List<String> { "Failed" }
                                }
                            }
                        }
                    }
                };
                Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));
                return resource;
            }
    ```

1. Ardışık çizgiyi oluşturan `Main` yönteme aşağıdaki satırı ekleyin:

   ```csharp
   client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));
   ```

### <a name="parameters"></a>Parametreler

Boru hattı kodumuzun ilk bölümü parametreleri tanımlar.

* `sourceBlobContainer`. Kaynak blob veri kümesi bu parametreyi boru hattında tüketir.
* `sinkBlobContainer`. Lavabo blob veri kümesi bu parametreyi boru hattında tüketir.
* `receiver`. Alıcıya başarı veya hata e-postaları gönderen ardışık ardışık işlem bu parametreyi kullanır.

```csharp
Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "receiver", new ParameterSpecification { Type = ParameterType.String } }
    },
```

### <a name="web-activity"></a>Web etkinliği

Web etkinliği herhangi bir REST bitiş noktasına çağrı yapılmasına izin verir. Etkinlik hakkında daha fazla bilgi için [Azure Veri Fabrikası'ndaki Web etkinliği'ne](control-flow-web-activity.md)bakın. Bu ardışık işlem, Logic Apps e-posta iş akışını aramak için bir web etkinliği kullanır. İki web aktivitesi oluşturursunuz: biri `CopySuccessEmail` iş akışına çağıran, diğeri `CopyFailWorkFlow`ise .

```csharp
        new WebActivity
        {
            Name = sendCopyEmailActivity,
            Method = WebActivityMethod.POST,
            Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/12345",
            Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
            DependsOn = new List<ActivityDependency>
            {
                new ActivityDependency
                {
                    Activity = copyBlobActivity,
                    DependencyConditions = new List<String> { "Succeeded" }
                }
            }
        }
```

Özellik, `Url` Logic Apps iş akışlarınızdan **HTTP POST URL** uç noktalarını yapıştırın. Özellik, `Body` sınıfın bir örneğini `EmailRequest` geçirin. E-posta isteği aşağıdaki özellikleri içerir:

* İleti. `@{activity('CopyBlobtoBlob').output.dataWritten`Değeri geçer. Önceki kopyalama etkinliğinin özelliğine erişir `dataWritten`ve 'nin değerini geçer. Hata durumunda `@{activity('CopyBlobtoBlob').error.message` yerine hata çıktısını geçirir.
* Veri Fabrikası Adı. Bu sistem `@{pipeline().DataFactory}` değişkeninin değerini geçer, ilgili veri fabrikası adına erişmenizi sağlar. Sistem değişkenlerinin listesi için [bkz.](control-flow-system-variables.md)
* Boru Hattı Adı. `@{pipeline().Pipeline}`Değeri geçer. Bu sistem değişkeni, ilgili ardışık düzen adı erişmenizi sağlar.
* Alıcı. `"@pipeline().parameters.receiver"`Değeri geçer. Boru hattı parametrelerine erişir.

Bu kod, önceki kopyalama etkinliğine bağlı yeni bir Etkinlik Bağımlılığı oluşturur.

## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma

Bir ardışık sistem `Main` çalışmasını tetikleyen yönteme aşağıdaki kodu ekleyin.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};

CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="main-class"></a>Main sınıfı

Son `Main` yöntemin şu nasiç olmalı.

```csharp
// Authenticate and create a data factory management client
var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };

Factory df = CreateOrUpdateDataFactory(client);

client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));

client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));

Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};

CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

Bir işlem hattı çalıştırması tetiklemek için programınızı derleyip çalıştırın!

## <a name="monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. `Main` yöntemine aşağıdaki kodu ekleyin:

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

    Bu kod, verileri kopyalamayı bitirene kadar çalıştırmanın durumunu sürekli olarak denetler.

1. Kopyalama etkinliği çalıştırma `Main` ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) alan yönteme aşağıdaki kodu ekleyin:

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");

    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList();

    if (pipelineRun.Status == "Succeeded")
    {
        Console.WriteLine(activityRuns.First().Output);
        //SaveToJson(SafeJsonConvert.SerializeObject(activityRuns.First().Output, client.SerializationSettings), "ActivityRunResult.json", folderForJsons);
    }
    else
        Console.WriteLine(activityRuns.First().Error);

    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı derleyip başlatın, ardından işlem hattı yürütmesini doğrulayın.

Uygulama, veri fabrikası, bağlantılı hizmet, veri kümeleri, ardışık hatlar ve ardışık hatlar çalışması oluşturma ilerlemesini görüntüler. Daha sonra işlem hattı çalıştırma durumunu denetler. Okunan/yazılan veri boyutunu içeren kopyalama etkinliği ayrıntılarını görene kadar bekleyin. Daha sonra, blob'un değişkenlerde belirttiğiniz gibi *blob'un blobPath girişinden çıktıblobPath'e* kopyalandığını kontrol etmek için Azure Depolama Gezgini gibi araçları kullanın. *outputBlobPath*

Çıktınız aşağıdaki örneğe benzemelidir:

```json
Creating data factory DFTutorialTest...
{
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "type": "AzureStorage",
  "typeProperties": {
    "connectionString": "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***"
  }
}
Creating dataset SourceStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sourceBlobContainer"
    },
    "fileName": "input.txt"
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating dataset SinkStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sinkBlobContainer"
    }
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating pipeline Adfv2TutorialBranchCopy...
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
            "type": "BlobSink"
          }
        },
        "inputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SourceStorageDataset"
          }
        ],
        "outputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SinkStorageDataset"
          }
        ],
        "name": "CopyBlobtoBlob"
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').output.dataWritten}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendSuccessEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Succeeded"
            ]
          }
        ]
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').error.message}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendFailEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Failed"
            ]
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      },
      "receiver": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 00000000-0000-0000-0000-0000000000000
Checking pipeline run status...
Status: InProgress
Status: InProgress
Status: Succeeded
Checking copy activity run details...
{
  "dataRead": 20,
  "dataWritten": 20,
  "copyDuration": 4,
  "throughput": 0.01,
  "errors": [],
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
}
{}

Press any key to exit...
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki görevleri yaptınız:

> [!div class="checklist"]
> * Veri fabrikası oluşturma
> * Azure Depolama bağlı hizmeti oluşturma
> * Azure blob veri kümesi oluşturma
> * Kopyalama etkinliği ve bir web etkinliği içeren işlem hattı oluşturma
> * Etkinliklerin çıktılarını sonraki etkinliklere gönderme
> * Parametre geçişi ve sistem değişkenlerini kullanma
> * Bir işlem hattı çalıştırması başlatma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme

Azure Veri Fabrikası hakkında daha fazla bilgi için artık Kavramlar bölümüne devam edebilirsiniz.
> [!div class="nextstepaction"]
>[İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
