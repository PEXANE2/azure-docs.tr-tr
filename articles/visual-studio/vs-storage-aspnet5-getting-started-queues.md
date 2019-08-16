---
title: Kuyruk depolama ve Visual Studio bağlı hizmetler 'i kullanmaya başlama (ASP.NET Core) | Microsoft Docs
description: Visual Studio 'da bir ASP.NET Core projesinde Azure kuyruk depolamayı kullanmaya başlama
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
ms.openlocfilehash: d8e370c6f7c59da8522bb4fb1403b6107a9c9c41
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510990"
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Kuyruk depolama ve Visual Studio bağlı hizmetler 'i kullanmaya başlama (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

Bu makalede, Visual Studio **bağlı hizmetler** özelliğini kullanarak bir ASP.NET Core projesinde Azure Storage hesabı oluşturduktan veya başvurduktan sonra Visual Studio 'Da Azure kuyruk depolama hizmetini kullanmaya nasıl başlacağınız açıklanır. **Bağlı hizmetler** işlemi, projenizde Azure depolama 'ya erişmek ve depolama hesabı için bağlantı dizesini proje yapılandırma dosyalarınıza eklemek üzere uygun NuGet paketlerini yükleyerek ekler. (Azure depolama hakkında genel bilgi için bkz. [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) .)

Azure kuyruk depolama, HTTP veya HTTPS kullanılarak kimliği doğrulanmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletiyi depolamaya yönelik bir hizmettir. Tek bir kuyruk iletisi en fazla 64 kilobayt (KB) olabilir ve bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Ayrıca, kuyrukları programlama yoluyla düzenleme hakkındaki ayrıntılar için bkz. [.NET kullanarak Azure kuyruk depolama ile çalışmaya başlama](../storage/queues/storage-dotnet-how-to-use-queues.md) .

Başlamak için önce depolama hesabınızda bir Azure kuyruğu oluşturun. Bu makale daha sonra ' de C# bir sıranın nasıl oluşturulacağını ve sıra iletilerini ekleme, değiştirme, okuma ve kaldırma gibi temel sıra işlemlerinin nasıl gerçekleştirileceğini gösterir.  Kod, .NET için Azure Storage Istemci kitaplığı 'nı kullanır. ASP.NET hakkında daha fazla bilgi için bkz. [ASP.net](https://www.asp.net).

Bazı Azure depolama API 'Leri zaman uyumsuzdur ve bu makaledeki kodda zaman uyumsuz yöntemlerin kullanıldığı varsayılır. Daha fazla bilgi için bkz. [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) .

## <a name="access-queues-in-code"></a>Koddaki erişim kuyrukları

ASP.NET Core projelerindeki kuyruklara erişmek için, Azure kuyruk depolama 'ya erişen herhangi bir C# kaynak dosyasına aşağıdaki öğeleri ekleyin. İzleyen bölümlerde kodun önünde bu kodun tümünü kullanın.

1. Gerekli `using` deyimleri ekleyin:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Depolama hesabı `CloudStorageAccount` bilgilerinizi temsil eden bir nesne alın. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Depolama hesabınızdaki `CloudQueueClient` sıra nesnelerine başvuracak bir nesne alın:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Belirli bir `CloudQueue` kuyruğa başvurmak için bir nesne alın:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Kodda kuyruk oluşturma

Kodda Azure kuyruğu oluşturmak için şunu çağırın `CreateIfNotExistsAsync`:

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme

Mevcut bir kuyruğa bir ileti eklemek için, yeni `CloudQueueMessage` bir nesne oluşturun ve sonra `AddMessageAsync` yöntemi çağırın. Bir `CloudQueueMessage` nesne, bir dizeden (UTF-8 biçiminde) ya da bir bayt dizisinden oluşturulabilir.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuyun

`PeekMessageAsync` Yöntemi çağırarak sıradan kaldırmadan bir kuyruğun ön tarafındaki iletiye göz atmayı sağlayabilirsiniz:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuma ve kaldırma

Kodunuz, iki adımda kuyruktan bir iletiyi kaldırabilir (sıradan çıkarabilirsiniz).

1. Kuyruktaki `GetMessageAsync` bir sonraki iletiyi almak için çağırın. Öğesinden `GetMessageAsync` döndürülen bir ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
1. İletiyi kuyruktan kaldırmayı tamamlaması için çağırın `DeleteMessageAsync`.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod, ileti `DeleteMessageAsync` işlendikten hemen sonra çağırır:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>Dequeuing iletileri için ek seçenekler

İleti alımı bir kuyruktan özelleştirmenin iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneği, tek bir `GetMessages` çağrıda 20 ileti almak için yöntemini kullanır. Ardından, her iletiyi bir `foreach` döngü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. Beş dakikalık zamanlayıcının tüm iletiler için aynı anda başlayacağını unutmayın, bu nedenle, beş dakika geçtikten sonra silinmeyen tüm iletiler yeniden görünür hale gelir.

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

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. `FetchAttributes` Yöntemi, kuyruk hizmetini ileti sayısı dahil olmak üzere kuyruk özniteliklerini almayı ister. Özelliği, kuyruk hizmeti çağrılmadan `FetchAttributes` yöntemi tarafından alınan son değeri döndürür. `ApproximateMethodCount`

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Ortak kuyruk API 'Leri ile Async-Await modelini kullanın

Bu örnek, ile `Async`biten ortak kuyruk API 'leri ile Async-Await modelinin nasıl kullanılacağını gösterir. Zaman uyumsuz bir yöntem kullanıldığında, Async-Await model, çağrı tamamlanana kadar yerel yürütmeyi askıya alır. Bu davranış, geçerli iş parçacığının performans sorunlarını önlemeye yardımcı olan diğer işleri yapmasına izin verir ve uygulamanızın genel yanıt hızını artırır.

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

Bir kuyruğu ve içinde yer alan tüm iletileri silmek için, kuyruk nesnesi üzerinde `Delete` yöntemi çağırın:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
