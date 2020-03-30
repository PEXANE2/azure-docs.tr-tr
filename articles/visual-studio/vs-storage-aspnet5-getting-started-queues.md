---
title: Visual Studio (ASP.NET Core) kullanarak sıra depolama ya da sıra depolama ile başlayın
description: Visual Studio'daki ASP.NET Core projesinde Azure sıra depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 11/14/2017
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5cdf6f2644788674df91b533c9444fc88ab30b09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72300033"
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Sıra depolama ve Visual Studio bağlantılı hizmetler (ASP.NET Core) ile başlayın

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

Bu makalede, Visual Studio **Connected Services** özelliğini kullanarak bir ASP.NET Core projesinde bir Azure depolama hesabı oluşturduktan veya başvurulan sonra Visual Studio'da Azure Kuyruk depolama alanını kullanmaya nasıl başlanırsınız. **Bağlı Hizmetler** işlemi, projenizdeki Azure depolama alanına erişmek için uygun NuGet paketlerini yükler ve depolama hesabının bağlantı dizesini proje yapılandırma dosyalarınıza ekler. (Azure Depolama hakkında genel bilgi için [Depolama belgelerine](https://azure.microsoft.com/documentation/services/storage/) bakın.)

Azure sıra depolama, HTTP veya HTTPS kullanarak kimlik doğrulaması yapılan aramalar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda ileti depolamak için bir hizmettir. Tek bir sıra iletisi 64 kilobayt (KB) boyutuna kadar olabilir ve bir kuyruk, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Ayrıca [bkz.](../storage/queues/storage-dotnet-how-to-use-queues.md)

Başlamak için önce depolama hesabınızda bir Azure kuyruğu oluşturun. Bu makalede, C#'da bir sıranın nasıl oluşturulup gerçekleştirilebildiğiniz ve sıra iletilerini ekleme, değiştirme, okuma ve kaldırma gibi temel sıra işlemlerinin nasıl gerçekleştirildirilen gösterilmektedir.  Kod, .NET için Azure Depolama İstemci Kitaplığı'nı kullanır. ASP.NET hakkında daha fazla bilgi için [ASP.NET.](https://www.asp.net)

Azure Depolama API'lerinden bazıları eşzamanlıdır ve bu makaledeki kod, async yöntemlerinin kullanıldığını varsayar. Daha fazla bilgi için [Asynchronous programlamaya](https://docs.microsoft.com/dotnet/csharp/async) bakın.

## <a name="access-queues-in-code"></a>Koddaki erişim kuyrukları

ASP.NET Core projelerindeki kuyruklara erişmek için, Azure sıra depolama alanına erişen c# kaynak dosyasına aşağıdaki öğeleri ekleyin. Bu kodun tümünün ardından gelen bölümlerde kodun önünde kullanın.

1. Gerekli `using` ifadeleri ekleyin:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Depolama `CloudStorageAccount` hesabı bilgilerinizi temsil eden bir nesne alın. Azure hizmet yapılandırmasından depolama bağlantısı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Depolama `CloudQueueClient` hesabınızdaki sıra nesnelerine başvurmak için bir nesne alın:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Belirli `CloudQueue` bir kuyruğa başvurmak için bir nesne alın:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Kodda sıra oluşturma

Kodda Azure kuyruğuoluşturmak için: `CreateIfNotExistsAsync`

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme

Varolan bir kuyruğa ileti `CloudQueueMessage` eklemek için yeni `AddMessageAsync` bir nesne oluşturun ve ardından yöntemi çağırın. Bir `CloudQueueMessage` nesne bir dize (UTF-8 biçiminde) veya bayt dizisinden oluşturulabilir.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Kuyruktaki iletiyi okuma

`PeekMessageAsync` Yöntemi çağırarak sıranın önündeki iletiye kuyruktan çıkarmadan göz atabilirsiniz:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuma ve kaldırma

Kodunuz iki adımda bir iletiyi kuyruktan kaldırabilir (sırayı kaldırabilir).

1. Sıradaki `GetMessageAsync` bir sonraki iletiyi almak için arayın. Döndürülen `GetMessageAsync` ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
1. İletiyi kuyruktan kaldırmayı bitirmek `DeleteMessageAsync`için .

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. İleti işlendikten hemen sonra aşağıdaki kod çağrıları: `DeleteMessageAsync`

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>İletileri sıraya alma için ek seçenekler

Bir kuyruktan ileti alma özelleştirmek için iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneği, `GetMessages` tek bir aramada 20 ileti almak için yöntemi kullanır. Daha sonra her iletiyi bir `foreach` döngü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. Beş dakikalık zamanlayıcının tüm iletiler için aynı anda başladığını, böylece beş dakika geçtikten sonra silinmemiş iletilerin yeniden görünür hale geldiğini unutmayın.

```cs
// Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
//   delete each message after processing.

foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. Yöntem, `FetchAttributes` ileti sayısı da dahil olmak üzere sıra özniteliklerini almak için sıra hizmeti sorar. Özellik, `ApproximateMethodCount` sıra hizmetini çağırmadan `FetchAttributes` yöntem tarafından alınan son değeri döndürür.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Ortak sıra API'leri ile Async-Await deseni kullanma

Bu örnek, 'ile biten ortak sıra API'leri `Async`ile async-await deseni nasıl kullanılacağını gösterir. Bir async yöntemi kullanıldığında, async-await deseni arama tamamlanana kadar yerel yürütmeyi askıya atanır. Bu davranış, geçerli iş parçacığının performans darboğazlarını önlemeye yardımcı olan ve uygulamanızın genel yanıt verme yeteneğini artıran başka işler yapmasına olanak tanır.

```cs
// Create a message to add to the queue.
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message.
await messageQueue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruğve içinde bulunan tüm iletileri silmek için, sıra nesnesindeki `Delete` yöntemi arayın:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
