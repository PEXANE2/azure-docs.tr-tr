---
title: Medya Hizmetleri iş bildirimlerini .NET ile izlemek için Azure Kuyruk depolamasını kullanın | Microsoft Dokümanlar
description: Medya Hizmetleri iş bildirimlerini izlemek için Azure Kuyruk depolama alanını nasıl kullanacağınızı öğrenin. Kod örneği C# ile yazılır ve .NET için Medya Hizmetleri SDK'sını kullanır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: f535d0b5-f86c-465f-81c6-177f4f490987
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 2a7f15eb7e90ba4dec9bc614a45d2de46c07bdfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64868096"
---
# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Medya Hizmetleri iş bildirimlerini .NET ile izlemek için Azure Kuyruk depolamasını kullanma 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Kodlama işlerini çalıştırdığınızda, genellikle iş ilerlemesini izlemek için bir yol gerekir. Medya Hizmetlerini [Azure Sıra depolama alanına](../../storage/storage-dotnet-how-to-use-queues.md)bildirim sunacak şekilde yapılandırabilirsiniz. Sıra depolamadan bildirimler alarak iş ilerlemesini izleyebilirsiniz. 

Sıra depolamasına teslim edilen iletilere dünyanın her yerinden erişilebilir. Sıra depolama ileti mimarisi güvenilir ve yüksek ölçeklenebilir. İletiler için Yoklama Sırası depolama diğer yöntemler kullanılarak üzerinde önerilir.

Medya Hizmetleri bildirimlerini dinlemek için yaygın bir senaryo, kodlama işi tamamlandıktan sonra bazı ek görevleri gerçekleştirmesi gereken bir içerik yönetim sistemi geliştiriyorsanız (örneğin, bir iş akışındaki bir sonraki adımı tetiklemek veya yayımlamak için içeriği).

Bu makalede, Sıra depolama dan bildirim iletileri almak için nasıl gösterir.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Sıra depolama alanını kullanan Medya Hizmetleri uygulamalarını geliştirirken aşağıdakileri göz önünde bulundurun:

* Sıra depolama, ilk ilk çıkış (FIFO) sipariş edilen teslimat garantisi sağlamaz. Daha fazla bilgi için bkz: [Azure Kuyrukları ve Azure Hizmet Veri Sırası Karşılaştırıldığında ve Zıt.](https://msdn.microsoft.com/library/azure/hh767287.aspx)
* Sıra depolama bir itme hizmeti değildir. Sırayı yoklamalısın.
* İstediğiniz sayıda sıra nız olabilir. Daha fazla bilgi için [Sıra Hizmeti REST API'ye](https://docs.microsoft.com/rest/api/storageservices/Queue-Service-REST-API)bakın.
* Sıra depolamanın dikkat edilmesi gereken bazı sınırlamalar ve ayrıntılar vardır. Bunlar Azure [Kuyrukları ve Azure Hizmet Veri Biş Kuyrukları Karşılaştırıldığında ve Zıt](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted).

## <a name="net-code-example"></a>.NET kodu örneği

Bu bölümdeki kod örneği aşağıdakileri yapar:

1. Bildirim iletisi biçimiyle eşleyen **EncodingJobMessage** sınıfını tanımlar. Kod, kuyruktan alınan iletileri **EncodingJobMessage** türündeki nesnelere dönüştürür.
2. Medya Hizmetleri ve Depolama hesap bilgilerini app.config dosyasından yükler. Kod örneği, **CloudMediaContext** ve **CloudQueue** nesnelerini oluşturmak için bu bilgileri kullanır.
3. Kodlama işiyle ilgili bildirim iletileri alan sırayı oluşturur.
4. Kuyruğa eşlenen bildirim bitiş noktasını oluşturur.
5. Bildirim bitiş noktasını işe bağlar ve kodlama işini gönderir. Bir işe birden çok bildirim bitiş noktası eklenmiş olabilir.
6. **BildirimJobState.FinalStatesOnly** **AddNew** yöntemine geçer. (Bu örnekte, yalnızca iş işlemenin son durumlarıyla ilgileniyoruz.)

        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
7. **NotificationJobState.All'ı**geçerseniz, aşağıdaki durum değişikliği bildirimlerinin tümünü alırsınız: sıraya, zamanlanmış, işlenir ve tamamlanır. Ancak, daha önce belirtildiği gibi, Sıra depolama sipariş teslim garanti etmez. İleti leri sipariş etmek için **Timestamp** özelliğini kullanın (aşağıdaki örnekte **EncodingJobMessage** türünde tanımlanır). Yinelenen iletiler mümkündür. Yinelenenleri denetlemek için **ETag özelliğini** **(EncodingJobMessage** türünde tanımlanan) kullanın. Bazı durum değişikliği bildirimleri atlanır da mümkündür.
8. Her 10 saniyede bir sırayı işaretleyerek işin tamamlanma durumuna gelmesini bekler. İletileri işlendikten sonra siler.
9. Sırayı ve bildirim bitiş noktasını siler.

> [!NOTE]
> Bir işin durumunu izlemenin önerilen yolu, aşağıdaki örnekte gösterildiği gibi bildirim iletilerini dinlemektir:
>
> Alternatif olarak, **IJob.State** özelliğini kullanarak bir işin durumunu kontrol edebilirsiniz.  Bir işin tamamlanmasıyla ilgili bir bildirim iletisi, **IJob'daki** durum **Tamamlanmaya**ayarlı olmadan önce gelebilir. **IJob.State** özelliği hafif bir gecikme ile doğru durumu yansıtır.
>
>

### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 
2. Yeni bir klasör oluşturun (klasör yerel sürücünüzde herhangi bir yerde olabilir) ve kodlamak ve akış veya aşamalı olarak indirmek istediğiniz bir .mp4 dosyasını kopyalayın. Bu örnekte, "C:\Media" yolu kullanılır.
3. **System.Runtime.Serialization** kitaplığına bir başvuru ekleyin.

### <a name="code"></a>Kod

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using System.Collections.Generic;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;
using System.Runtime.Serialization.Json;

namespace JobNotification
{
    public class EncodingJobMessage
    {
        // MessageVersion is used for version control.
        public String MessageVersion { get; set; }

        // Type of the event. Valid values are
        // JobStateChange and NotificationEndpointRegistration.
        public String EventType { get; set; }

        // ETag is used to help the customer detect if
        // the message is a duplicate of another message previously sent.
        public String ETag { get; set; }

        // Time of occurrence of the event.
        public String TimeStamp { get; set; }

        // Collection of values specific to the event.

        // For the JobStateChange event the values are:
        //     JobId - Id of the Job that triggered the notification.
        //     NewState- The new state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
        //     OldState- The old state of the Job. Valid values are:
        //          Scheduled, Processing, Canceling, Cancelled, Error, Finished

        // For the NotificationEndpointRegistration event the values are:
        //     NotificationEndpointId- Id of the NotificationEndpoint
        //          that triggered the notification.
        //     State- The state of the Endpoint.
        //          Valid values are: Registered and Unregistered.

        public IDictionary<string, object> Properties { get; set; }
    }

    class Program
    {

        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly string _StorageConnectionString = 
            ConfigurationManager.AppSettings["StorageConnectionString"];

        private static CloudMediaContext _context = null;
        private static CloudQueue _queue = null;
        private static INotificationEndPoint _notificationEndPoint = null;

        private static readonly string _singleInputMp4Path =
            Path.GetFullPath(@"C:\Media\BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            string endPointAddress = Guid.NewGuid().ToString();

            // Create the context.
            AzureAdTokenCredentials tokenCredentials = 
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
            
            // Create the queue that will be receiving the notification messages.
            _queue = CreateQueue(_StorageConnectionString, endPointAddress);

            // Create the notification point that is mapped to the queue.
            _notificationEndPoint =
                    _context.NotificationEndPoints.Create(
                    Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);


            if (_notificationEndPoint != null)
            {
                IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                WaitForJobToReachedFinishedState(job.Id);
            }

            // Clean up.
            _queue.Delete();
            _notificationEndPoint.Delete();
        }


        static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

            // Create the queue client
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

            // Retrieve a reference to a queue
            CloudQueue queue = queueClient.GetQueueReference(endPointAddress);

            // Create the queue if it doesn't already exist
            queue.CreateIfNotExists();

            return queue;
        }


        public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");

            //Create an encrypted asset and upload the mp4.
            IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted,
                inputMediaFilePath);

            // Get a media processor reference, and pass to it the name of the
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the conversion details, using a configuration file.
            ITask task = job.Tasks.AddNew("My encoding Task",
                processor,
                "Adaptive Streaming",
                Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);

            // Add a notification point to the job. You can add multiple notification points.  
            job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly,
                _notificationEndPoint);

            job.Submit();

            return job;
        }

        public static void WaitForJobToReachedFinishedState(string jobId)
        {
            int expectedState = (int)JobState.Finished;
            int timeOutInSeconds = 600;

            bool jobReachedExpectedState = false;
            DateTime startTime = DateTime.Now;
            int jobState = -1;

            while (!jobReachedExpectedState)
            {
                // Specify how often you want to get messages from the queue.
                Thread.Sleep(TimeSpan.FromSeconds(10));

                foreach (var message in _queue.GetMessages(10))
                {
                    using (Stream stream = new MemoryStream(message.AsBytes))
                    {
                        DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                        settings.UseSimpleDictionaryFormat = true;
                        DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                        EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);

                        Console.WriteLine();

                        // Display the message information.
                        Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                        Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                        Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                        Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                        foreach (var property in encodingJobMsg.Properties)
                        {
                            Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                        }

                        // We are only interested in messages
                        // where EventType is "JobStateChange".
                        if (encodingJobMsg.EventType == "JobStateChange")
                        {
                            string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                            if (JobId == jobId)
                            {
                                string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                string newJobStateStr = (String)encodingJobMsg.Properties.
                                                            Where(j => j.Key == "NewState").FirstOrDefault().Value;

                                JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);

                                if (newJobState == (JobState)expectedState)
                                {
                                    Console.WriteLine("job with Id: {0} reached expected state: {1}",
                                        jobId, newJobState);
                                    jobReachedExpectedState = true;
                                    break;
                                }
                            }
                        }
                    }
                    // Delete the message after we've read it.
                    _queue.DeleteMessage(message);
                }

                // Wait until timeout
                TimeSpan timeDiff = DateTime.Now - startTime;
                bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                if (timedOut)
                {
                    Console.WriteLine(@"Timeout for checking job notification messages,
                                        latest found state ='{0}', wait time = {1} secs",
                        jobState,
                        timeDiff.TotalSeconds);

                    throw new TimeoutException();
                }
            }
        }

        static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(),
                assetCreationOptions);

            var fileName = Path.GetFileName(singleFilePath);

            var assetFile = asset.AssetFiles.Create(fileName);

            Console.WriteLine("Created assetFile {0}", assetFile.Name);
            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading of {0}", assetFile.Name);

            return asset;
        }

        static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

Önceki örnekte aşağıdaki çıktı üretilir: Değerleriniz değişir.

    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4

    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35

    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected
    State: Finished


## <a name="next-step"></a>Sonraki adım
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
