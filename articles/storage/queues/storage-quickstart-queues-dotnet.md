---
title: 'Hızlı başlangıç: Azure kuyruk depolama kitaplığı v12-.NET'
description: Bir kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure kuyruğu .NET V12 kitaplığı 'nı nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri okumayı ve silmeyi öğreneceksiniz. Ayrıca, bir kuyruğu silmeyi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 07/24/2020
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: fd60adf94e57be3892a264c2a8457b839e1c8778
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89011302"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-net"></a>Hızlı başlangıç: .NET için Azure kuyruk depolama istemci kitaplığı V12

.NET için Azure kuyruk depolama istemci kitaplığı sürüm 12 ' i kullanmaya başlayın. Azure kuyruk depolaması, daha sonra almak ve işlemek üzere çok sayıda ileti depolamaya yönelik bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere bu adımları izleyin.

.NET için Azure kuyruk depolama istemci kitaplığı V12 ' nı kullanarak şunları yapın:

* Bir kuyruk oluşturma
* Bir kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Kuyruktan ileti alma
* Kuyruktaki iletileri silme
* Bir kuyruk silme

Ek kaynaklar:

* [API başvuru belgeleri](/dotnet/api/azure.storage.queues)
* [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues)
* [Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Queues/12.0.0)
* [Örnekler](https://docs.microsoft.com/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı- [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) . Çalışma zamanını değil, SDK 'Yı aldığınızdan emin olun.

## <a name="setting-up"></a>Ayarlanıyor

Bu bölümde, bir projeyi .NET için Azure kuyruk depolama istemci kitaplığı V12 ile çalışacak şekilde hazırlama adımları gösterilmektedir.

### <a name="create-the-project"></a>Proje oluşturma

*QueuesQuickstartV12*adlı bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` *QueuesQuickstartV12*adlı yeni bir konsol uygulaması oluşturmak için komutunu kullanın. Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*.

   ```console
   dotnet new console -n QueuesQuickstartV12
   ```

1. Yeni oluşturulan *QueuesQuickstartV12* dizinine geçiş yapın.

   ```console
   cd QueuesQuickstartV12
   ```

### <a name="install-the-package"></a>Paketi yükler

Hala uygulama dizininde, komutunu kullanarak .NET için Azure kuyruk depolama istemci kitaplığı 'nı yükleyebilirsiniz `dotnet add package` .

```console
dotnet add package Azure.Storage.Queues
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. *Program.cs* dosyasını Düzenleyicinizde açın
1. İfadeyi Kaldır `Console.WriteLine("Hello World!");`
1. `using`Yönergeler ekleme
1. `Main` [Zaman uyumsuz kodu desteklemek](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7-1#async-main) için yöntem bildirimini güncelleştirme



Kod şu şekildedir:

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

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır. Kuyruk depolama, üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kuyruk
* Kuyruktaki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Kuyruk depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Şu kaynaklarla etkileşim kurmak için aşağıdaki .NET sınıflarını kullanın:

* [QueueServiceClient](/dotnet/api/azure.storage.queues.queueserviceclient):, `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenizi sağlar.
* [Queueclient](/dotnet/api/azure.storage.queues.queueclient): `QueueClient` sınıfı, tek bir kuyruğu ve iletilerini yönetmenizi ve düzenlemenizi sağlar.
* [Queuemessage](/dotnet/api/azure.storage.queues.models.queuemessage): `QueueMessage` sınıf, bir kuyrukta [receivemessages](/dotnet/api/azure.storage.queues.queueclient.receivemessages) çağrılırken döndürülen ayrı nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, .NET için Azure kuyruk depolama istemci kitaplığı ile aşağıdaki işlemlerin nasıl yapılacağını göstermektedir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Bir kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages-in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Kuyruktan ileti alma](#receive-messages-from-a-queue)
* [Kuyruktaki iletileri silme](#delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, depolama hesabı için bağlantı dizesini alır. Bağlantı dizesi, [depolama Bağlantı dizenizi yapılandırma](#configure-your-storage-connection-string) bölümünde oluşturulan ortam değişkeninde depolanır.

Bu kodu metodun içine ekleyin `Main` :

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

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kuyruk adına bir GUID değeri ekler.

> [!IMPORTANT]
> Kuyruk adları yalnızca küçük harf, sayı ve kısa çizgi içerebilir ve bir harf veya sayı ile başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad ayrıca 3 ila 63 karakter uzunluğunda olmalıdır. Adlandırma sıraları hakkında daha fazla bilgi için bkz. [adlandırma sıraları ve meta verileri](/rest/api/storageservices/naming-queues-and-metadata).


[Queueclient](/dotnet/api/azure.storage.queues.queueclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızda kuyruğu oluşturmak için [Createasync](/dotnet/api/azure.storage.queues.queueclient.createasync) yöntemini çağırın.

Bu kodu yönteminin sonuna ekleyin `Main` :

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

### <a name="add-messages-to-a-queue"></a>Bir kuyruğa ileti ekleme

Aşağıdaki kod parçacığı, [SendMessageAsync](/dotnet/api/azure.storage.queues.queueclient.sendmessageasync) yöntemini çağırarak zaman uyumsuz iletileri kuyruğa ekler. Ayrıca, bir çağrıdan döndürülen [Sendalındısı](/dotnet/api/azure.storage.queues.models.sendreceipt) de kaydeder `SendMessageAsync` . Okundu bilgisi, programın ilerleyen kısımlarında iletiyi güncelleştirmek için kullanılır.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
Console.WriteLine("\nAdding messages to the queue...");

// Send several messages to the queue
await queueClient.SendMessageAsync("First message");
await queueClient.SendMessageAsync("Second message");

// Save the receipt so we can update this message later
SendReceipt receipt = await queueClient.SendMessageAsync("Third message");
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

[PeekMessagesAsync](/dotnet/api/azure.storage.queues.queueclient.peekmessagesasync) yöntemini çağırarak kuyruktaki iletilere göz atın. `PeekMessagesAsync`Yöntemi, sıranın önüne bir veya daha fazla ileti alır ancak iletinin görünürlüğünü değiştirmez.

Bu kodu yönteminin sonuna ekleyin `Main` :

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

[Updatemessageasync](/dotnet/api/azure.storage.queues.queueclient.updatemessageasync) yöntemini çağırarak bir iletinin içeriğini güncelleştirin. `UpdateMessageAsync`Yöntemi, bir iletinin görünürlük zaman aşımını ve içeriğini değiştirebilir. İleti içeriği, boyutu 64 KB 'a kadar olan bir UTF-8 kodlu dize olmalıdır. İletinin yeni içeriğiyle birlikte, `SendReceipt` kodda daha önce kaydedilen değerleri ' den geçirin. `SendReceipt`Değerler güncelleştirilecek iletiyi belirler.

```csharp
Console.WriteLine("\nUpdating the third message in the queue...");

// Update a message using the saved receipt from sending the message
await queueClient.UpdateMessageAsync(receipt.MessageId, receipt.PopReceipt, "Third message has been updated");
```

### <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

[Receivemessagesasync](/dotnet/api/azure.storage.queues.queueclient.receivemessagesasync) yöntemini çağırarak önceden eklenmiş iletileri indirin.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
Console.WriteLine("\nReceiving messages from the queue...");

// Get messages from the queue
QueueMessage[] messages = await queueClient.ReceiveMessagesAsync(maxMessages: 10);
```

### <a name="delete-messages-from-a-queue"></a>Kuyruktaki iletileri silme

İşlenmeden sonra sıradaki iletileri silin. Bu durumda, işleme yalnızca konsolda iletiyi görüntülüyor.

Uygulama, `Console.ReadLine` iletileri işleyerek ve silmeden önce çağırarak kullanıcı girişi için duraklatılır. [Azure Portal](https://portal.azure.com) , kaynakların silinmeden önce doğru şekilde oluşturulduğunu doğrulayın. Açıkça silinmeyen tüm iletiler, daha sonra bu işlemleri işlemek için bir süre sonra sırada görünür hale gelir.

Bu kodu yönteminin sonuna ekleyin `Main` :

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

Aşağıdaki kod, [DeleteAsync](/dotnet/api/azure.storage.queues.queueclient.deleteasync) yöntemi kullanılarak sırayı silerek uygulamanın oluşturduğu kaynakları temizler.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
Console.WriteLine("\nPress Enter key to delete the queue...");
Console.ReadLine();

// Clean up
Console.WriteLine($"Deleting queue: {queueClient.Name}");
await queueClient.DeleteAsync();

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, bir Azure kuyruğuna üç ileti oluşturur ve ekler. Kod kuyruktaki iletileri listeler, ardından kuyruğu silmeden önce bunları alır ve siler.

Konsol pencerenizde uygulama dizininize gidip uygulamayı derleyin ve çalıştırın.

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

Uygulama iletileri almadan önce durakladığında, [Azure Portal](https://portal.azure.com)depolama hesabınızı kontrol edin. İletilerin kuyrukta olduğunu doğrulayın.

İletileri almak ve silmek için **ENTER** tuşuna basın. İstendiğinde, kuyruğu silmek ve tanıtımı sona ermesini sağlamak için **ENTER** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kuyruk oluşturmayı ve zaman uyumsuz .NET kodu kullanarak buna ileti eklemeyi öğrendiniz. Ardından iletileri göz atmayı, almayı ve silmeyi öğrendiniz. Son olarak, bir ileti sırasının nasıl silineceğini öğrendiniz.

Öğreticiler, örnekler, hızlı ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [.NET ve .NET Core geliştiricileri için Azure](https://docs.microsoft.com/dotnet/azure/)

* Daha fazla bilgi edinmek için bkz. [.net Için Azure depolama kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage).
* Daha fazla Azure kuyruk depolama örnek uygulaması görmek için [Azure kuyruk depolama v12 .NET istemci kitaplığı örneklerine](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Queues/samples)geçin.
* .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
