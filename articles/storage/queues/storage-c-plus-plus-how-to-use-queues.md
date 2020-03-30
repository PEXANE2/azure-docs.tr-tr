---
title: Sıra depolama (C++) nasıl kullanılır - Azure Depolama
description: Azure'da Sıra depolama hizmetini nasıl kullanacağınızı öğrenin. Örnekler C++'da yazılır.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/11/2017
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 4fe543010df9514cb2b22c56482a4b592574e917
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941775"
---
# <a name="how-to-use-queue-storage-from-c"></a>C++’tan Kuyruk Depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure Kuyruk depolama hizmetini kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceğinizi gösterir. Örnekler C++ dilinde yazılmıştır ve [C++ için Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)’nı kullanır. Kapsanan senaryolar arasında kuyruk iletileri **ekleme,** **gözetleme,** **alma**ve **silme** nin yanı sıra **kuyruk oluşturma ve silme**yer alır.

> [!NOTE]
> Bu kılavuz C++ için Azure Depolama İstemci Kitaplığı sürüm 1.0.0 ve üzerini hedefler. Önerilen sürüm, [NuGet](https://www.nuget.org/packages/wastorage) ya da [GitHub](https://github.com/Azure/azure-storage-cpp/) üzerinden ulaşılabilen Depolama İstemci Kitaplığı 2.2.0’dır.
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, c++ uygulaması içinde çalıştırılabilen depolama özelliklerini kullanırsınız.

Bunu yapmak için, C++ için Azure Depolama İstemci Kitaplığı’nı yüklemeniz ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.

C++ için Azure Depolama İstemci Kitaplığı’nı aşağıdaki yöntemleri kullanarak yükleyebilirsiniz:

* **Linux:** [C++ README için Azure Depolama İstemci Kitaplığı'nda verilen yönergeleri izleyin: Linux sayfasından başlarken.](https://github.com/Azure/azure-storage-cpp#getting-started-on-linux)
* **Windows:** Windows'da, bağımlılık yöneticisi olarak [vcpkg](https://github.com/microsoft/vcpkg) kullanın. Vcpkg'ı başlatmak için [hızlı başlat'ı](https://github.com/microsoft/vcpkg#quick-start) izleyin. Ardından, kitaplığı yüklemek için aşağıdaki komutu kullanın:

```powershell
.\vcpkg.exe install azure-storage-cpp
```

[README](https://github.com/Azure/azure-storage-cpp#download--install) dosyasında kaynak kodunu niçin oluşturup NuGet'e dışa aktarabileceğinize yönelik bir kılavuz bulabilirsiniz.

## <a name="configure-your-application-to-access-queue-storage"></a>Uygulamanızı Sıra Depolama'ya erişmek için yapılandırın
Kuyruklara erişmek için Azure depolama API'lerini kullanmak istediğiniz C++ dosyasının üst bölümüne aşağıdaki ifadeleri ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ayarlama
Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. İstemci uygulamasında çalışırken, muhasebe hesabınızın adını ve *Hesap Adı* ve *Hesap Anahtarı* değerleri için [Azure Portalı'nda](https://portal.azure.com) listelenen depolama hesabının depolama erişim anahtarını kullanarak depolama bağlantı dizesini aşağıdaki biçimde sağlamanız gerekir. Depolama hesapları ve erişim anahtarları hakkında bilgi için [Azure Depolama Hesapları Hakkında'ya](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)bakın. Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Uygulamanızı yerel Windows bilgisayarınızda sınamak için Azure [SDK](https://azure.microsoft.com/downloads/)ile yüklü olan Microsoft Azure [depolama emülatörü](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) kullanabilirsiniz. Depolama emülatörü, yerel geliştirme makinenizde Azure'da bulunan Blob, Queue ve Tablo hizmetlerini taklit eden bir yardımcı programdır. Aşağıdaki örnekte bağlantı dizesini yerel depolama öykünücünüzde tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure depolama emülatörübaşlatmak için **Başlat** düğmesini seçin veya **Windows** tuşuna basın. Azure **Depolama Emülatörü**yazmaya başlayın ve uygulamalar listesinden **Microsoft Azure Depolama Emülatörü'nü** seçin.

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.

## <a name="retrieve-your-connection-string"></a>Bağlantı dizenizi alma
Depolama Hesabı bilgilerinizi temsil etmek için **cloud_storage_account** sınıfını kullanabilirsiniz. Depolama bağlantı dizesinden depolama hesabı bilgilerini almak için **parse** yöntemini kullanabilirsiniz.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Nasıl yapılı: Sıra oluşturma
**cloud_queue_client** nesnesi, kuyruklar için başvuru nesneleri almanızı sağlar. Aşağıdaki kod **cloud_queue_client** bir nesne oluşturur.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Kullanmak istediğiniz kuyruğa başvuru almak için **cloud_queue_client** nesnesini kullanın. Yoksa sırayı oluşturabilirsiniz.

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl yapilir: Sıraya ileti ekleme
Varolan bir kuyruğa ileti eklemek için önce yeni bir **cloud_queue_message**oluşturun. Sonra, **add_message** yöntemini arayın. **bir cloud_queue_message** dize veya **bayt** dizisinden oluşturulabilir. Burada, bir kuyruk oluşturan (eğer yoksa) ve 'Hello, World' iletisini yerleştiren bir kod yer almaktadır:

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılsın: Bir sonraki iletiye göz atın
**peek_message** yöntemini arayarak sıranın önündeki iletiye göz atabilirsiniz.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapilir: Sıralı iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti n seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-the-next-message"></a>Nasıl yapilir: Bir sonraki iletide sırayı silme
Kodunuz, bir iletiyi bir kuyruktan iki adımda çıkarır. **get_message**aradiğinizde, bir sonraki iletiyi kuyrukta alırsınız. **get_message** döndürülen ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez olur. İletiyi kuyruktan kaldırmayı bitirmek için **delete_message'ı**da aramanız gerekir. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. İleti işlendikten hemen sonra kodunuz **delete_message** çağırır.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Nasıl yapilir: İletileri sıraya alma için ek seçeneklerden yararlanın
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneği, tek bir aramada 20 ileti almak için **get_messages** yöntemini kullanır. Daha sonra her iletiyi **for** döngüsü kullanarak işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. Tüm iletiler için 5 dakikanın aynı anda başladığını, bu nedenle **get_messages'a**yapılan çağrıdan bu yana 5 dakika geçtikten sonra silinmemiş iletilerin yeniden görünür hale geldiğini unutmayın.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: Sıra uzunluğunu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **download_attributes** yöntemi, sıra sayısı da dahil olmak üzere sıra özniteliklerini almak için Sıra hizmeti ister. **approximate_message_count** yöntemi, kuyruktaki yaklaşık ileti sayısını alır.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılı: Sırayı silme
Bir kuyruğ ve içinde bulunan tüm iletileri silmek için, sıra nesnesindeki **delete_queue_if_exists** yöntemini arayın.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Sonraki adımlar
Sıra depolamanın temellerini öğrendiğiniz için Azure Depolama hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin.

* [C++'dan Blob Depolama nasıl kullanılır?](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [C++'dan Tablo Depolama nasıl kullanılır?](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Azure Depolama Kaynaklarını C++'da listele](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [C++ Başvurusu için Depolama İstemci Kitaplığı](https://azure.github.io/azure-storage-cpp)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
