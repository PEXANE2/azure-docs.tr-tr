---
title: Azure Bildirim Hub'ları kayıtlarını toplu olarak dışa aktarma ve alma | Microsoft Dokümanlar
description: Bildirim Hub'ları toplu desteğini, bir bildirim hub'ında çok sayıda işlem gerçekleştirmek veya tüm kayıtları dışa aktarmak için nasıl kullanacağınızı öğrenin.
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 03/18/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/18/2019
ms.openlocfilehash: 8eb03a42f38c0cc7fe82eda6a81d1c8c1213ec74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71212391"
---
# <a name="export-and-import-azure-notification-hubs-registrations-in-bulk"></a>Azure Bildirim Hub'ları kayıtlarını toplu olarak dışa aktarma ve alma
Bir bildirim hub'ında çok sayıda kayıt oluşturmak veya değiştirmek için gereken senaryolar vardır. Bu senaryolardan bazıları, toplu iş hesaplamalarını izleyen veya Bildirim Hub'larını kullanmak için varolan bir itme uygulamasını geçirmek için etiket güncelleştirmeleridir.

Bu makalede, bir bildirim hub'ında çok sayıda işlem nasıl gerçekleştirildirilen veya tüm kayıtları toplu olarak dışa aktarmak için nasıl açıklanmaktadır.

## <a name="high-level-flow"></a>Üst düzey akış
Toplu iş desteği, milyonlarca kaydı içeren uzun süreli işleri desteklemek üzere tasarlanmıştır. Bu ölçek elde etmek için toplu destek, iş ayrıntılarını ve çıktıyı depolamak için Azure Depolama'yı kullanır. Toplu güncelleştirme işlemleri için, kullanıcının, içeriği kayıt güncelleştirme işlemlerinin listesi olan blob kapsayıcısında bir dosya oluşturması gerekir. İşe başlarken, kullanıcı giriş blob'una bir URL ve çıktı dizinine (ayrıca blob kapsayıcısında) bir URL sağlar. İş başladıktan sonra, kullanıcı işin başında sağlanan bir URL konumunu sorgulayarak durumu denetleyebilir. Belirli bir iş yalnızca belirli bir tür (oluşturur, güncelleştirmeler veya siler) işlemleri gerçekleştirebilir. Dışa aktarma işlemleri benzer şekilde gerçekleştirilir.

## <a name="import"></a>İçeri Aktarma

### <a name="set-up"></a>Ayarla
Bu bölüm, aşağıdaki varlıklara sahip olduğunuzu varsayar:

- Sağlanan bir bildirim merkezi.
- Azure Depolama blob kapsayıcısı.
- [Azure Depolama NuGet paketine](https://www.nuget.org/packages/windowsazure.storage/) ve Bildirim Hub'larına yapılan başvurular [NuGet paketine](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

### <a name="create-input-file-and-store-it-in-a-blob"></a>Giriş dosyası oluşturma ve bir blob'da saklama
Giriş dosyası, XML'de serihale getirilen, satır başına bir kayıt listesi içerir. Azure SDK'yı kullanarak, aşağıdaki kod örneği, kayıtların nasıl serileştirilebildiğini ve blob kapsayıcısına nasıl yükleyirileni gösterir.

```csharp
private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
{
    StringBuilder builder = new StringBuilder();
    foreach (var registrationDescription in descriptions)
    {
        builder.AppendLine(RegistrationDescription.Serialize());
    }

    var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
    using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
    {
        inputBlob.UploadFromStream(stream);
    }
}
```

> [!IMPORTANT]
> Önceki kod bellekteki kayıtları serileştirir ve tüm akışı bir blob'a yükler. Birkaç megabayttan daha fazla dosya yüklediyseniz, bu adımların nasıl gerçekleştirilebildiğini anlatan Azure blob kılavuzuna bakın; örneğin, [blok lekeler](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).

### <a name="create-url-tokens"></a>URL belirteçleri oluşturma
Giriş dosyanız yüklendikten sonra, hem giriş dosyası hem de çıktı dizini için bildirim merkezinize sağlamak için URL'ler oluşturun. Giriş ve çıkış için iki farklı blob kapsayıcısı kullanabilirsiniz.

```csharp
static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
    };

    string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + sasContainerToken);
}

static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
    {
        SharedAccessExpiryTime = DateTime.UtcNow.AddDays(1),
        Permissions = SharedAccessBlobPermissions.Read
    };
    string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);
    return new Uri(container.Uri + "/" + filePath + sasToken);
}
```

### <a name="submit-the-job"></a>İşi gönderme
İki giriş ve çıktı URL'si ile artık toplu iş başlatabilirsiniz.

```csharp
NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
var createTask = client.SubmitNotificationHubJobAsync(
new NotificationHubJob {
        JobType = NotificationHubJobType.ImportCreateRegistrations,
        OutputContainerUri = outputContainerSasUri,
        ImportFileUri = inputFileSasUri
    }
);
createTask.Wait();

var job = createTask.Result;
long i = 10;
while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
{
    var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
    getJobTask.Wait();
    job = getJobTask.Result;
    Thread.Sleep(1000);
    i--;
}
```

Giriş ve çıkış URL'lerine ek olarak, bu `NotificationHubJob` örnek, aşağıdaki `JobType` türlerden biri olabilecek bir nesne içeren bir nesne oluşturur:

- `ImportCreateRegistrations`
- `ImportUpdateRegistrations`
- `ImportDeleteRegistrations`

Arama tamamlandıktan sonra, iş bildirim merkezi tarafından devam edilir ve [GetNotificationHubJobAsync](/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.getnotificationhubjobasync?view=azure-dotnet)için arama ile durumunu kontrol edebilirsiniz.

İş tamamlandığında, çıktı dizininizdeki aşağıdaki dosyalara bakarak sonuçları inceleyebilirsiniz:

- `/<hub>/<jobid>/Failed.txt`
- `/<hub>/<jobid>/Output.txt`

Bu dosyalar, toplu iş toplu iş başarılı ve başarısız işlemlerin listesini içerir. Dosya biçimi, `.cvs`her satırda özgün giriş dosyasının satır numarasına ve işlemin çıktısının (genellikle oluşturulan veya güncelleştirilen kayıt açıklaması) bulunduğu biçimdir.

### <a name="full-sample-code"></a>Tam örnek kodu
Aşağıdaki örnek kod kayıtları bir bildirim merkezine içeri girer.

```csharp
using Microsoft.Azure.NotificationHubs;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Xml;

namespace ConsoleApplication1
{
    class Program
    {
        private static string CONNECTION_STRING = "---";
        private static string HUB_NAME = "---";
        private static string INPUT_FILE_NAME = "CreateFile.txt";
        private static string STORAGE_ACCOUNT = "---";
        private static string STORAGE_PASSWORD = "---";
        private static StorageUri STORAGE_ENDPOINT = new StorageUri(new Uri("---"));

        static void Main(string[] args)
        {
            var descriptions = new[]
            {
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMkUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMjUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMhUxREQFBlVTTkMwMQ"),
                new MpnsRegistrationDescription(@"http://dm2.notify.live.net/throttledthirdparty/01.00/12G9Ed13dLb5RbCii5fWzpFpAgAAAAADAQAAAAQUZm52OkJCMjg1QTg1QkZDMdUxREQFBlVTTkMwMQ"),
            };

            //write to blob store to create an input file
            var blobClient = new CloudBlobClient(STORAGE_ENDPOINT, new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(STORAGE_ACCOUNT, STORAGE_PASSWORD));
            var container = blobClient.GetContainerReference("testjobs");
            container.CreateIfNotExists();

            SerializeToBlob(container, descriptions);

            // TODO then create Sas
            var outputContainerSasUri = GetOutputDirectoryUrl(container);
            var inputFileSasUri = GetInputFileUrl(container, INPUT_FILE_NAME);


            //Lets import this file
            NotificationHubClient client = NotificationHubClient.CreateClientFromConnectionString(CONNECTION_STRING, HUB_NAME);
            var createTask = client.SubmitNotificationHubJobAsync(
                new NotificationHubJob {
                    JobType = NotificationHubJobType.ImportCreateRegistrations,
                    OutputContainerUri = outputContainerSasUri,
                    ImportFileUri = inputFileSasUri
                }
            );
            createTask.Wait();

            var job = createTask.Result;
            long i = 10;
            while (i > 0 && job.Status != NotificationHubJobStatus.Completed)
            {
                var getJobTask = client.GetNotificationHubJobAsync(job.JobId);
                getJobTask.Wait();
                job = getJobTask.Result;
                Thread.Sleep(1000);
                i--;
            }
        }

        private static void SerializeToBlob(CloudBlobContainer container, RegistrationDescription[] descriptions)
        {
            StringBuilder builder = new StringBuilder();
            foreach (var registrationDescription in descriptions)
            {
                builder.AppendLine(RegistrationDescription.Serialize());
            }

            var inputBlob = container.GetBlockBlobReference(INPUT_FILE_NAME);
            using (MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(builder.ToString())))
            {
                inputBlob.UploadFromStream(stream);
            }
        }

        static Uri GetOutputDirectoryUrl(CloudBlobContainer container)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + sasContainerToken);
        }

        static Uri GetInputFileUrl(CloudBlobContainer container, string filePath)
        {
            //Set the expiry time and permissions for the container.
            //In this case no start time is specified, so the shared access signature becomes valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
            {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(4),
                Permissions = SharedAccessBlobPermissions.Read
            };

            //Generate the shared access signature on the container, setting the constraints directly on the signature.
            string sasToken = container.GetBlockBlobReference(filePath).GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            return new Uri(container.Uri + "/" + filePath + sasToken);
        }

        static string GetJobPath(string namespaceName, string notificationHubPath, string jobId)
        {
            return string.Format(CultureInfo.InvariantCulture, @"{0}//{1}/{2}/", namespaceName, notificationHubPath, jobId);
        }
    }
}
```

## <a name="export"></a>Dışarı Aktarma
Kayıt dışa aktarma, aşağıdaki farklılıklarla, içe aktarma benzer:

- Yalnızca çıktı URL'si gerekir.
- Dışa Aktarma Kayıtları türünden bir BildirimHubJob oluşturursunuz.

### <a name="sample-code-snippet"></a>Örnek kod snippet
Java'da kayıt aktarmak için örnek kod parçacığı aşağıda veda edebilirsiniz:

```java
// submit an export job
NotificationHubJob job = new NotificationHubJob();
job.setJobType(NotificationHubJobType.ExportRegistrations);
job.setOutputContainerUri("container uri with SAS signature");
job = hub.submitNotificationHubJob(job);

// wait until the job is done
while(true){
    Thread.sleep(1000);
    job = hub.getNotificationHubJob(job.getJobId());
    if(job.getJobStatus() == NotificationHubJobStatus.Completed)
        break;
}

```

## <a name="next-steps"></a>Sonraki adımlar
Kayıtlar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Kayıt yönetimi](notification-hubs-push-notification-registration-management.md)
- [Kayıtlar için etiketler](notification-hubs-tags-segment-push-message.md)
- [Şablon kayıtları](notification-hubs-templates-cross-platform-push-messages.md)
