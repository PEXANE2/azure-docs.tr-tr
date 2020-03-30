---
title: 'Hızlı başlangıç: Azure Sıra depolama kitaplığı v12 - .NET'
description: Kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure Queue .NET v12 kitaplığını nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri nasıl okuyup sildiğinizi öğrenirsiniz. Ayrıca bir sırayı nasıl sildiğinizi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/22/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.openlocfilehash: c69aa91596ff203445aa4fa3ccd59001ffe16649
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78197496"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-net"></a>Quickstart: .NET için Azure Queue depolama istemcikitapv12

.NET için Azure Queue depolama istemcisi kitaplığı sürümü 12 ile başlayın. Azure Sıra depolama, daha sonra alma ve işleme için çok sayıda ileti depolamak için bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

> [!NOTE]
> Önceki SDK sürümüne başlamak [için Quickstart: Bir sırayı yönetmek için .NET için Azure Depolama SDK v11'ini kullanın.](storage-quickstart-queues-dotnet-legacy.md)

.NET için Azure Queue depolama istemcisi kitaplığını v12'yi kullanın:

* Bir kuyruk oluşturma
* Kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* İletileri kuyruktan silme
* Bir kuyruk silme

[API başvuru belgeleri](/dotnet/api/azure.storage.queues) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues) | [Paketi (NuGet)](https://www.nuget.org/packages/Azure.Storage.Queues/12.0.0) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [.NET Core SDK.](https://dotnet.microsoft.com/download/dotnet-core) SDK değil, çalışma zamanı almak için emin olun.

## <a name="setting-up"></a>Ayarlama

Bu bölüm, .NET için Azure Queue depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlama konusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*QueuesQuickstartV12*adlı bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` *QueuesQuickstartV12*adını taşıyan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir "Hello World" C# projesi oluşturur: *Program.cs*.

   ```console
   dotnet new console -n QueuesQuickstartV12
   ```

1. Yeni oluşturulan *QueuesQuickstartV12* dizinine geçin.

   ```console
   cd QueuesQuickstartV12
   ```

### <a name="install-the-package"></a>Paketi yükleyin

Uygulama dizinindeyken,.NET paketi için Azure Kuyruk depolama istemcisi `dotnet add package` kitaplığını komutu kullanarak yükleyin.

```console
dotnet add package Azure.Storage.Queues
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Düzenleyicinizdeki *Program.cs* dosyasını açma
1. İfadeyi `Console.WriteLine("Hello World!");` kaldırma
1. `using` Yönergeekleme
1. Async `Main` kodunu [desteklemek](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7-1#async-main) için yöntem bildirimini güncelleştirme



İşte kod:

```csharp
using Azure;
using Azure.Storage.Queues;
using Azure.Storage.Queues.Models;
using System;
using System.Threading.Tasks;

namespace QueuesQuickstartV12
{
    class Program
    {
        static async Task Main(string[] args)
        {
        }
    }
}
```

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır. Sıra depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabında bir sıra
* Sıra içindeki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Sıra depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Bu kaynaklarla etkileşimkurmak için aşağıdaki .NET sınıflarını kullanın:

* [QueueServiceClient](/dotnet/api/azure.storage.queues.queueserviceclient): `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenize olanak tanır.
* [QueueClient](/dotnet/api/azure.storage.queues.queueclient): `QueueClient` Sınıf, tek bir sırayı ve iletilerini yönetmenize ve işlemenize olanak tanır.
* [QueueMessage](/dotnet/api/azure.storage.queues.models.queuemessage): `QueueMessage` Sınıf, bir sırada [Alİleti'leri](/dotnet/api/azure.storage.queues.queueclient.receivemessages) ararken döndürülen tek tek nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, .NET için Azure Kuyruk depolama istemcisi kitaplığıyla aşağıdaki işlemleri nasıl yapacağınızı gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [İletileri kuyruktan silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod depolama hesabının bağlantı dizesini alır. Bağlantı dizesi, depolama bağlantı dizesi bölümüne [Yapılandır'da](#configure-your-storage-connection-string) oluşturulan ortam değişkeninde depolanır.

Bu kodu yöntemin `Main` içine ekleyin:

```csharp
Console.WriteLine("Azure Queue storage v12 - .NET quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable called
// AZURE_STORAGE_CONNECTION_STRING on the machine running the application.
// If the environment variable is created after the application is launched
// in a console or with Visual Studio, the shell or application needs to be
// closed and reloaded to take the environment variable into account.
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
```

### <a name="create-a-queue"></a>Bir kuyruk oluşturma

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için sıra adına bir GUID değeri ekler.

> [!IMPORTANT]
> Sıra adları yalnızca küçük harfler, sayılar ve tireler içerebilir ve bir harf veya sayıyla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad da 3 ve 63 karakter uzunluğunda olmalıdır. Sıraları adlandırma hakkında daha fazla bilgi için [Bkz.](/rest/api/storageservices/naming-queues-and-metadata)


[QueueClient](/dotnet/api/azure.storage.queues.queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızda sıra oluşturmak için [CreateAsync](/dotnet/api/azure.storage.queues.queueclient.createasync) yöntemini arayın.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
// Create a unique name for the queue
string queueName = "quickstartqueues-" + Guid.NewGuid().ToString();

Console.WriteLine($"Creating queue: {queueName}");

// Instantiate a QueueClient which will be
// used to create and manipulate the queue
QueueClient queueClient = new QueueClient(connectionString, queueName);

// Create the queue
await queueClient.CreateAsync();
```

### <a name="add-messages-to-a-queue"></a>Kuyruğa ileti ekleme

Aşağıdaki kod snippet asynchronously [SendMessageAsync](/dotnet/api/azure.storage.queues.queueclient.sendmessageasync) yöntemini çağırarak sıraya iletiekler. Ayrıca, bir `SendMessageAsync` çağrıdan döndürülen bir [SendReceipt'ı](/dotnet/api/azure.storage.queues.models.sendreceipt) da kaydeder. Makbuz, iletinin daha sonra programda güncelleştirilen fiş için kullanılır.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("\nAdding messages to the queue...");

// Send several messages to the queue
await queueClient.SendMessageAsync("First message");
await queueClient.SendMessageAsync("Second message");

// Save the receipt so we can update this message later
SendReceipt receipt = await queueClient.SendMessageAsync("Third message");
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

[PeekMessagesAsync](/dotnet/api/azure.storage.queues.queueclient.peekmessagesasync) yöntemini arayarak kuyruktaki iletilere göz atın. Yöntem, `PeekMessagesAsync` sıranın önünden bir veya daha fazla ileti alır, ancak iletinin görünürlüğünü değiştirmez.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("\nPeek at the messages in the queue...");

// Peek at messages in the queue
PeekedMessage[] peekedMessages = await queueClient.PeekMessagesAsync(maxMessages: 10);

foreach (PeekedMessage peekedMessage in peekedMessages)
{
    // Display the message
    Console.WriteLine($"Message: {peekedMessage.MessageText}");
}
```

### <a name="update-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi güncelleştirme

[UpdateMessageAsync](/dotnet/api/azure.storage.queues.queueclient.updatemessageasync) yöntemini arayarak iletinin içeriğini güncelleştirin. Yöntem, `UpdateMessageAsync` iletinin görünürlük zaman anına ve içeriğini değiştirebilir. İleti içeriği 64 KB boyutuna kadar olan UTF-8 kodlanmış bir dize olmalıdır. İletiiçin yeni içerikle birlikte, kodda daha `SendReceipt` önce kaydedilen değerlere aktarın. Değerler `SendReceipt` hangi iletiyi güncelleştireceklerini belirler.

```csharp
Console.WriteLine("\nUpdating the third message in the queue...");

// Update a message using the saved receipt from sending the message
await queueClient.UpdateMessageAsync(receipt.MessageId, receipt.PopReceipt, "Third message has been updated");
```

### <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

Daha önce eklenen iletileri [ReceiveMessagesAsync](/dotnet/api/azure.storage.queues.queueclient.receivemessagesasync) yöntemini arayarak indirin.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("\nReceiving messages from the queue...");

// Get messages from the queue
QueueMessage[] messages = await queueClient.ReceiveMessagesAsync(maxMessages: 10);
```

### <a name="delete-messages-from-a-queue"></a>İletileri kuyruktan silme

İletileri işlendikten sonra kuyruktan silin. Bu durumda, işleme yalnızca konsolda ileti yi görüntüler.

Uygulama, iletileri işleyip silmeden önce arayarak `Console.ReadLine` kullanıcı girişi için duraklatır. [Azure portalınızda](https://portal.azure.com) kaynakların silinmeden önce doğru oluşturulduğunu doğrulayın. Açıkça silinmeyen iletiler, bunları işlemek için başka bir şans için kuyrukta tekrar görünür hale gelir.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("\nPress Enter key to 'process' messages and delete them from the queue...");
Console.ReadLine();

// Process and delete messages from the queue
foreach (QueueMessage message in messages)
{
    // "Process" the message
    Console.WriteLine($"Message: {message.MessageText}");

    // Let the service know we're finished with
    // the message and it can be safely deleted.
    await queueClient.DeleteMessageAsync(message.MessageId, message.PopReceipt);
}
```

### <a name="delete-a-queue"></a>Bir kuyruk silme

Aşağıdaki kod, [DeleteAsync](/dotnet/api/azure.storage.queues.queueclient.deleteasync) yöntemini kullanarak sırayı silerek oluşturulan kaynakları temizler.

Bu kodu `Main` yöntemin sonuna ekleyin:

```csharp
Console.WriteLine("\nPress Enter key to delete the queue...");
Console.ReadLine();

// Clean up
Console.WriteLine($"Deleting queue: {queueClient.Name}");
await queueClient.DeleteAsync();

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, Azure kuyruğuna üç ileti oluşturur ve ekler. Kod, kuyruktaki iletileri listeler, ardından sırayı silmeden önce alır ve siler.

Konsol pencerenizde, uygulama dizininize gidin ve ardından uygulamayı oluşturun ve çalıştırın.

```console
dotnet build
```

```console
dotnet run
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Queue storage v12 - .NET quickstart sample

Creating queue: quickstartqueues-5c72da2c-30cc-4f09-b05c-a95d9da52af2

Adding messages to the queue...

Peek at the messages in the queue...
Message: First message
Message: Second message
Message: Third message

Updating the third message in the queue...

Receiving messages from the queue...

Press Enter key to 'process' messages and delete them from the queue...

Message: First message
Message: Second message
Message: Third message has been updated

Press Enter key to delete the queue...

Deleting queue: quickstartqueues-5c72da2c-30cc-4f09-b05c-a95d9da52af2
Done
```

Uygulama ileti almadan önce durakladığında, [Azure portalındaki](https://portal.azure.com)depolama hesabınızı kontrol edin. İletilerin sırada olduğunu doğrulayın.

İletileri almak ve silmek için **Enter** tuşuna basın. İstendiğinde, sırayı silmek ve demoyu bitirmek için **Enter** tuşuna tekrar basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, asynchronous .NET kodunu kullanarak kuyruk oluşturmayı ve ona ileti eklemeyi öğrendiniz. Ardından iletileri gözetlemeyi, almayı ve silmeyi öğrendiniz. Son olarak, ileti kuyruğunun nasıl silinir öğrenilir.

Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [.NET ve .NET Core geliştiricileri için Azure](https://docs.microsoft.com/dotnet/azure/)

* Daha fazla bilgi için [.NET için Azure Depolama kitaplıklarına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage)bakın.
* Daha fazla Azure Kuyruğu depolama örneği uygulaması görmek için [Azure Queue depolama v12 .NET istemci kitaplığı örneklerine](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples)devam edin.
* .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
