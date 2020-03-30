---
title: Visual Studio 'u (bulut hizmetleri) kullanarak sıra depolamaya başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra Visual Studio'daki bir bulut hizmeti projesinde Azure Kuyruk depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 603bb2b9a862ad4ed2cbde63e2d82b9a82fbeaa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298782"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure Kuyruk Depolama ve Visual Studio’ya bağlı hizmetleri kullanmaya başlama (bulut hizmeti projeleri)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Visual Studio **Bağlantılı Hizmetler Ekle** iletişim kutusunu kullanarak bulut hizmetleri projesinde bir Azure depolama hesabı oluşturduktan veya başvurulan sonra Visual Studio'da Azure Kuyruk depolama sını kullanmaya nasıl başlanırsınız.

Kodda nasıl sıra oluşturacağınız size göstereceğiz. Ayrıca, sıra iletileri ekleme, değiştirme, okuma ve kaldırma gibi temel sıra işlemlerini nasıl gerçekleştireceğinizi de gösteririz. Örnekler C# koduyla yazılır ve [.NET için Microsoft Azure Depolama İstemci Kitaplığı'nı](https://msdn.microsoft.com/library/azure/dn261237.aspx)kullanır.

**Bağlı Hizmetleri Ekle** işlemi, projenizdeki Azure depolama alanına erişmek için uygun NuGet paketlerini yükler ve depolama hesabının bağlantı dizesini proje yapılandırma dosyalarınıza ekler.

* Bkz. Koddaki kuyrukları işleme hakkında daha fazla bilgi için [.NET'i kullanarak Azure Kuyruk depolama](../storage/queues/storage-dotnet-how-to-use-queues.md) alanına başlayın.
* Azure Depolama hakkında genel bilgiler için [Depolama belgelerine](https://azure.microsoft.com/documentation/services/storage/) bakın.
* Azure bulut hizmetleri hakkında genel bilgi için [Bulut Hizmetleri belgelerine](https://azure.microsoft.com/documentation/services/cloud-services/) bakın.
* Programlama ASP.NET uygulamaları hakkında daha fazla bilgi için [ASP.NET](https://www.asp.net) bakın.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir.

## <a name="access-queues-in-code"></a>Koddaki erişim kuyrukları
Visual Studio Bulut Hizmetleri projelerindeki kuyruklara erişmek için, Azure Kuyruk depolama alanına erişen herhangi bir C# kaynak dosyasına aşağıdaki öğeleri eklemeniz gerekir.

1. C# dosyasının üst kısmındaki ad alanı bildirimlerinin bunları **kullanarak** ifadeler içerdiğinden emin olun.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Depolama hesabı bilgilerinizi temsil eden bir **CloudStorageAccount** nesnesi alın. Azure hizmet yapılandırmasından depolama bağlantı dizenizi ve depolama hesabı bilgilerinizi almak için aşağıdaki kodu kullanın.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Depolama hesabınızdaki sıra nesnelerine başvurmak için bir **CloudQueueClient** nesnesi alın.  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Belirli bir kuyruğa başvurmak için bir **CloudQueue** nesnesi alın.
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**NOT:** Yukarıdaki kodun tümlerini aşağıdaki örneklerde kodun önünde kullanın.

## <a name="create-a-queue-in-code"></a>Kodda sıra oluşturma
Kodda sıra oluşturmak için **CreateIfNotExists'a**bir çağrı eklemeniz gerekir.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme
Varolan bir kuyruğa ileti eklemek için yeni bir **CloudQueueMessage** nesnesi oluşturun ve ardından **AddMessage** yöntemini arayın.

**CloudQueueMessage** nesnesi bir dize (UTF-8 biçiminde) veya bayt dizisinden oluşturulabilir.

Burada 'Merhaba, Dünya' iletisini ekleyen bir örnek verilmiştir.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Kuyruktaki iletiyi okuma
**PeekMessage** yöntemini çağırarak iletiyi kuyruktan kaldırmadan kuyruğun önündeki iletiye göz atabilirsiniz.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi okuma ve kaldırma
Kodunuz iki adımda bir iletiyi kuyruktan kaldırabilir (sırayı kaldırabilir).

1. Sıradaki bir sonraki iletiyi almak için **GetMessage'ı** arayın. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır.
2. İletiyi kuyruktan kaldırmayı bitirmek için **DeleteMessage'ı**arayın.

Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Aşağıdaki kod, ileti işlendikten hemen sonra **DeleteMessage'ı** çağırır.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Sıra iletilerini işlemek ve kaldırmak için ek seçenekler kullanma
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz.

* Bir yığın ileti alabilirsiniz (en fazla 32).
* Daha uzun veya daha kısa bir görünmezlik zaman aşımı ayarlayarak, kodunuzun her iletiyi tam olarak işlemek için daha fazla veya daha az zaman ayırmasına izin verebilirsiniz. Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için **GetMessages** yöntemi kullanılmıştır. Ardından her ileti bir **foreach** döngüsü ile işlenir. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık sürenin tüm iletiler için aynı zamanda başladığını unutmayın, bu nedenle **GetMessages** çağrısından itibaren 5 dakika geçtikten sonra silinmeyen tüm iletiler görünür olacaktır.

Bir örneği aşağıda verilmiştir:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **FetchAttributes** yöntemi, ileti sayısı dahil olmak üzere Kuyruk hizmetinden kuyruk özniteliklerini almasını ister. **Yaklaşık Yöntem Sayısı** özelliği, Sıra hizmetini çağırmadan **FetchÖzler** yöntemi tarafından alınan son değeri döndürür.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Ortak Azure Queue API'leri ile Async-Await Modelini kullanma
Bu örnek, Ortak Azure Queue API'leri ile Async-Await deseni nasıl kullanılacağını gösterir. Örnek, verilen yöntemlerin her birinin async sürümünü çağırır, bu her yöntemin **Async** post-fix tarafından görülebilir. Bir async yöntemi kullanıldığında async-await deseni arama tamamlanana kadar yerel yürütmeyi askıya atanır. Bu davranış, geçerli iş parçacığının performans darboğazlarını önlemeye yardımcı olan ve uygulamanızın genel yanıt verme yeteneğini artıran başka işler yapmasına olanak tanır. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Bir kuyruk silme
Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki **Sil** yöntemini çağırın.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

