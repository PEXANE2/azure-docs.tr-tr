---
title: .NET-Azure Storage kullanarak Azure kuyruk depolama ile çalışmaya başlama
description: Azure Queues, uygulama bileşenleri arasında güvenilir ve zaman uyumsuz mesajlaşma sağlar. Bulut mesajlaşma özelliği uygulama bileşenlerinizin bağımsız olarak ölçeklendirilmesini sağlar.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/08/2020
ms.service: storage
ms.subservice: queues
ms.topic: how-to
ms.reviewer: dineshm
ms.openlocfilehash: b0415542d737fa2ab926eb572855dce5ef81690e
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84808828"
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>.NET kullanarak Azure Kuyruk Depolamaya başlayın

## <a name="overview"></a>Genel Bakış

Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Uygulamalar ölçeklendirme için tasarlanırken uygulama bileşenleri genellikle birbirinden bağımsız olarak ölçeklendirilebilecek şekilde ayrılır. Kuyruk depolama, bulutta, masaüstünde, şirket içi sunucuda veya mobil cihazlarda çalışan uygulama bileşenleri arasında zaman uyumsuz mesajlaşma sağlar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

### <a name="about-this-tutorial"></a>Bu öğretici hakkında

Bu öğreti, Azure kuyruk depolama kullanarak bazı genel senaryolar için .NET kodunun nasıl yazılacağını göstermektedir. Kapsanan senaryolara kuyruk oluşturma ve silme ile kuyruk iletileri ekleme, okuma ve silme dahildir.

**Tahmini tamamlanma süresi:** 45 dakika

### <a name="prerequisites"></a>Ön koşullar

- [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
- [.NET için Azure Storage ortak istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)
- [.NET için Azure depolama kuyruğu istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/)
- [.NET için Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/)
- Bir [Azure depolama hesabı](../common/storage-account-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Ardından, geliştirme ortamınızı Visual Studio’da ayarlayın; böylece bu kılavuzdaki kod örneklerini denemeye hazır olursunuz.

### <a name="create-a-windows-console-application-project"></a>Windows konsol uygulaması projesi oluşturma

Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlarda, Visual Studio 2019 ' de bir konsol uygulamasının nasıl oluşturulacağı gösterilmektedir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir.

1. **Dosya**  >  **Yeni**  >  **Proje** ' yi seçin
2. **Platform**  >  **pencerelerini** seçin
3. **Konsol Uygulaması (.NET Framework)** öğesini seçin
4. **İleri** Seç
5. **Proje adı** alanına uygulamanız için bir ad girin
6. **Oluştur** ' u seçin

Bu öğreticideki tüm kod örnekleri konsol uygulamanızın **program.cs** dosyasının **Main ()** yöntemine eklenebilir.

Azure depolama istemci kitaplıklarını, Azure bulut hizmeti veya Web uygulaması, masaüstü ve mobil uygulamalar dahil olmak üzere herhangi bir türde .NET uygulamasında kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

### <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Bu öğreticiyi tamamlayabilmeniz için projenizde aşağıdaki dört pakete başvurmanız gerekir:

- [.Net Için Azure çekirdek kitaplığı](https://www.nuget.org/packages/Azure.Core/): Bu paket, modern .net Azure SDK 'sı istemci kitaplıkları için paylaşılan temel elemanlar, soyutlamalar ve yardımcıları sağlar.
- [.Net Için Azure Storage ortak Istemci kitaplığı](https://www.nuget.org/packages/Azure.Storage.Common/): Bu paket, diğer Azure depolama istemci kitaplıkları tarafından paylaşılan altyapıyı sağlar.
- [.Net Için Azure depolama kuyruğu kitaplığı](https://www.nuget.org/packages/Azure.Storage.Queues/): Bu paket, bir istemci tarafından erişilebilecek iletileri depolamak Için azure depolama kuyruk hizmeti birlikte çalışmaya izin verebilir.
- [.NET için Configuration Manager kitaplığı](https://www.nuget.org/packages/System.Configuration.ConfigurationManager/): Bu paket, istemci uygulamaları için yapılandırma dosyalarına erişim sağlar.

Bu paketleri edinmek için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**' de projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.
1. **Gözatmayı** Seç
1. Çevrimiçi olarak "Azure. Storage. Queues" araması yapın ve depolama istemci kitaplığı 'nı ve bağımlılıklarını yüklemek için **yüklemeyi** seçin. Bu işlem, kuyruk kitaplığının bağımlılıkları olan Azure. Storage. Common ve Azure. Core kitaplıklarını da yükler.
1. Çevrimiçi olarak "System.Configuration.ConfigurationManager" araması yapın ve Configuration Manager yüklemek için **yüklemeyi** seçin.

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Bu öğreticiyi tamamlayabilmeniz için projenizde aşağıdaki üç pakete başvurmanız gerekir:

- [.Net Için ortak Istemci kitaplığı Microsoft Azure depolama](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/): Bu paket Depolama hesabınızdaki veri kaynaklarına programlı erişim sağlar.
- [.NET için Microsoft Azure depolama kuyruk kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/): Bu istemci kitaplığı, bir istemci tarafından erişilebilecek iletileri depolamak için Microsoft Azure depolama kuyruk hizmeti birlikte çalışmaya izin verebilir.
- [.NET için Microsoft Azure Configuration Manager Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Bu paketleri edinmek için NuGet kullanabilirsiniz. Şu adımları uygulayın:

1. **Çözüm Gezgini**' de projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin.
1. **Gözatmayı** Seç
1. Çevrimiçi olarak "Microsoft. Azure. Storage. Queue" araması yapın ve depolama istemci kitaplığı 'nı ve bağımlılıklarını yüklemek için **yüklemeyi** seçin. Bu, kuyruk kitaplığının bağımlılığı olan Microsoft. Azure. Storage. Common kitaplığını da yükler.
1. Çevrimiçi olarak "Microsoft.Azure.ConfigurationManager" araması yapın ve Azure Configuration Manager yüklemek için **yüklemeyi** seçin.

---

> [!NOTE]
> Depolama istemci kitaplıkları paketleri de [.net Için Azure SDK 'sına](https://azure.microsoft.com/downloads/)dahildir. Ancak, her zaman en son sürümlere sahip olduğunuzdan emin olmak için depolama istemci kitaplıklarını NuGet 'ten de yüklemenizi öneririz.
>
> .NET için depolama istemci kitaplıklarında ODataLib bağımlılıkları, WCF Veri Hizmetleri değil, NuGet 'de bulunan ODataLib paketleri tarafından çözümlenir. ODataLib kitaplıkları NuGet aracılığıyla doğrudan indirilebilir veya kod projenizle başvurulabilir. Depolama istemci kitaplıkları tarafından kullanılan belirli ODataLib paketleri [OData](https://nuget.org/packages/Microsoft.Data.OData/), [EDM](https://nuget.org/packages/Microsoft.Data.Edm/)ve [uzamsal](https://nuget.org/packages/System.Spatial/). Bu kitaplıklar Azure Tablo Depolama sınıfları tarafından kullanıldığından, depolama istemci kitaplıklarıyla programlama için gerekli bağımlılıklardır.

### <a name="determine-your-target-environment"></a>Hedef ortamınızı saptama

Bu kılavuzdaki örnekleri çalıştırmak için iki ortam seçeneğiniz vardır:

- Kodunuzu buluttaki bir Azure Storage hesabına karşı çalıştırabilirsiniz.
- Kodunuzun Azurite depolama öykünücüsünde karşı çalışmasını sağlayabilirsiniz. Azurite, buluttaki bir Azure depolama hesabına öykünen yerel bir ortamdır. Azurite, uygulamanız geliştirme aşamasında olduğunda kodunuzda test ve hata ayıklama için ücretsiz bir seçenektir. Öykünücü iyi bilinen hesabı ve anahtarı kullanır. Daha fazla bilgi için bkz. [Yerel Azure depolama geliştirme ve test Için Azurite öykünücüsünü kullanma](../common/storage-use-azurite.md).

> [!NOTE]
> Azure Storage ile ilişkili maliyetlerin oluşmasını önlemek için depolama öykünücüsünü hedefleyebilirsiniz. Ancak, buluttaki bir Azure Storage hesabını hedeflemeyi seçerseniz, bu öğreticiyi gerçekleştirme maliyetleri göz ardı edilecektir.

## <a name="get-your-storage-connection-string"></a>Depolama Bağlantı dizenizi alın

Depolama hizmetlerine erişim için uç noktaları ve kimlik bilgilerini yapılandırmak üzere bir depolama bağlantı dizesi kullanarak .NET için Azure depolama istemci kitaplıkları desteği. Daha fazla bilgi için bkz. [depolama hesabı erişim anahtarlarını yönetme](../common/storage-account-keys-manage.md).

### <a name="copy-your-credentials-from-the-azure-portal"></a>Azure portalından kimlik bilgilerinizi kopyalama

Örnek kodun, depolama hesabınıza erişim için yetki vermesi gerekir. Yetki vermek için, depolama hesabınızın kimlik bilgilerini bir bağlantı dizesi şeklinde uygulamaya sağlarsınız. Depolama hesabınızın kimlik bilgilerini görüntülemek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Depolama hesabınızı bulun.
3. Depolama hesabına genel bakışın **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Hesap erişim anahtarlarınız ve her bir anahtar için tam bağlantı dizesi görüntülenir.
4. **key1** bölümünde **Bağlantı dizesi** değerini bulun ve **Kopyala** düğmesine tıklayarak bağlantı dizesini kopyalayın. Sonraki adımda bir ortam değişkenine bağlantı dizesini ekleyeceksiniz.

    ![Azure portalından bağlantı dizesinin kopyalanmasını gösteren ekran görüntüsü](media/storage-dotnet-how-to-use-queues/portal-connection-string.png)

Bağlantı dizeleri hakkında daha fazla bilgi için bkz. [Azure depolama için bağlantı dizesi yapılandırma](../common/storage-configure-connection-string.md).

> [!NOTE]
> Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Depolama hesabı anahtarınızı korumak için her zaman özen gösterin. Diğer kullanıcılara dağıtmaktan, sabit kodlamaktan ve başkalarının erişebileceği düz metin dosyasına kaydetmekten kaçının. Anahtarınızın tehlikede olduğunu düşünüyorsanız, Azure portalını kullanarak hesap anahtarınızı yeniden oluşturun.

Depolama bağlantı dizenizi korumanın en iyi yolu bir yapılandırma dosyasında tutmaktır. Bağlantı dizenizi yapılandırmak için Çözüm Gezgini *app.config* dosyasını Visual Studio 'da açın. `\<appSettings\>` öğesinin içeriğini aşağıda gösterildiği gibi ekleyin. *Bağlantı dizesini* portaldaki depolama hesabınızdan kopyaladığınız değerle değiştirin:

```xml
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="connection-string" />
    </appSettings>
</configuration>
```

Örneğin, yapılandırma ayarınız şunun gibi görünür:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==EndpointSuffix=core.windows.net" />
```

Azurite depolama öykünücüsünü hedeflemek için, iyi bilinen hesap adı ve anahtarıyla eşleşen bir kısayolu kullanabilirsiniz. Bu durumda, bağlantı dizesi ayarı şöyle olur:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
```

### <a name="add-using-directives"></a>Using yönergeleri ekleme

Aşağıdaki `using` yönergelerini `Program.cs` dosyasının üst tarafına ekleyin:

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_UsingStatements":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
using System; // Namespace for Console output
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for CloudStorageAccount
using Microsoft.Azure.Storage.Queue; // Namespace for Queue storage types
```

---

### <a name="create-the-queue-service-client"></a>Kuyruk hizmeti istemcisi oluşturma

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

[Queueclient](/dotnet/api/azure.storage.queues.queueclient) sınıfı, kuyruk depolamada depolanan kuyrukları almanızı sağlar. Hizmet istemcisini oluşturma yöntemlerinden biri aşağıda verilmiştir:

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_CreateClient":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

[CloudQueueClient](/dotnet/api/microsoft.azure.storage.queue.cloudqueueclient?view=azure-dotnet-legacy) sınıfı, Kuyruk depolamada depolanan kuyrukları almanızı sağlar. Hizmet istemcisini oluşturma yöntemlerinden biri aşağıda verilmiştir:

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```

---

Artık Kuyruk depolamadan veri okuyan ve bu depolamaya veri yazan kodu yazmaya hazırsınız.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bu örnek, bir sıranın nasıl oluşturulacağını gösterir:

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_CreateQueue":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

---

## <a name="insert-a-message-into-a-queue"></a>Kuyruğa bir ileti yerleştirme

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Mevcut bir sıraya bir ileti eklemek için, [SendMessage](/dotnet/api/azure.storage.queues.queueclient.sendmessage) metodunu çağırın. İleti bir `string` (UTF-8 biçiminde) ya da bir `byte` dizi olabilir. Aşağıdaki kod bir sıra (yoksa) oluşturur ve bir ileti ekler:

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_InsertMessage":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir [CloudQueueMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage?view=azure-dotnet-legacy) oluşturun. Ardından [AddMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessage?view=azure-dotnet-legacy) yöntemini çağırın. Bir `CloudQueueMessage` `string` (UTF-8 biçiminde) ya da bir dizi içinden oluşturulabilir `byte` . Burada bir sıra (yoksa) oluşturan ve "Hello, World" iletisini ekleyen kod verilmiştir:

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();

// Create a message and add it to the queue
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

---

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Kuyruktaki iletilere, [PeekMessages](/dotnet/api/azure.storage.queues.queueclient.peekmessages) yöntemini çağırarak kuyruktan kaldırmadan göz atmayı sağlayabilirsiniz. *MaxMessages* parametresi için bir değer geçirmezseniz, varsayılan olarak tek bir iletiye göz atın.

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_PeekMessage":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

[PeekMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.peekmessage?view=azure-dotnet-legacy) yöntemini çağırarak iletiyi kuyruktan kaldırmadan kuyruğun önündeki iletiye göz atabilirsiniz.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

---

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme

Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_UpdateMessage":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent2("Updated contents.", false);
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

---

## <a name="de-queue-the-next-message"></a>Sonraki iletiyi sıradan çıkarmak

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

İki adımda kuyruktan bir iletiyi kuyruktan kaldır. [Receivemessages](/dotnet/api/azure.storage.queues.queueclient.receivemessages)'i çağırdığınızda kuyruktaki bir sonraki iletiyi alırsınız. Öğesinden döndürülen bir ileti, `ReceiveMessages` Bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için ayrıca [DeleteMessage](/dotnet/api/azure.storage.queues.queueclient.deletemessage)’ı çağırmanız gerekir. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. `DeleteMessage`İleti işlendikten sonra kodunuz doğru şekilde çağırır.

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DequeueMessage":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Kodunuz, bir iletiyi bir kuyruktan iki adımda çıkarır. [GetMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage?view=azure-dotnet-legacy)’ı çağırdığınzda, bir kuyruktaki bir sonraki iletiyi alırsınız. Öğesinden döndürülen bir ileti, `GetMessage` Bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için ayrıca [DeleteMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage?view=azure-dotnet-legacy)’ı çağırmanız gerekir. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. `DeleteMessage`İleti işlendikten sonra kodunuz doğru şekilde çağırır.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

---

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Genel Kuyruk depolama API’leri ile Zaman Uyumsuz-Bekleme yöntemini kullanma

Bu örnek, genel Kuyruk depolama API’leri ile Zaman Uyumsuz-Bekleme yönteminin nasıl kullanılacağını gösterir. Örnek, her yönteme eklenen *Async* soneki ile belirtildiği şekilde kullanılan yöntemlerin her birinin zaman uyumsuz sürümlerini çağırır. Zaman uyumsuz bir yöntem kullanıldığında, zaman uyumsuz-bekleme yöntemi çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış geçerli iş parçacığının başka işler yapmasını sağlar ve böylece performans sorunlarını engellemeye yardımcı olur, uygulamanızın genel yanıt hızını iyileştirir. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_AsyncQueue":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

---

## <a name="leverage-additional-options-for-de-queuing-messages"></a>İletilerin kuyruktan çıkarılması için ek seçenekleri kullanma

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz.

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Aşağıdaki kod örneği, bir çağrıda 20 ileti almak için [receivemessages](/dotnet/api/azure.storage.queues.queueclient.receivemessages) yöntemini kullanır. Ardından, her iletiyi bir döngü kullanarak işler `foreach` . Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık tüm iletiler için aynı anda başlayacağını unutmayın. bu nedenle, çağrısından bu yana 5 dakika geçtikten sonra `ReceiveMessages` , silinmemiş olan tüm iletiler yeniden görünür hale gelir.

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DequeueMessages":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için [GetMessages](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessages?view=azure-dotnet-legacy) yöntemi kullanılmıştır. Ardından, her iletiyi bir döngü kullanarak işler `foreach` . Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık tüm iletiler için aynı anda başlayacağını unutmayın. bu nedenle, çağrısından bu yana 5 dakika geçtikten sonra `GetMessages` , silinmemiş olan tüm iletiler yeniden görünür hale gelir.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

---

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. [GetProperties](/dotnet/api/azure.storage.queues.queueclient.getproperties) yöntemi, ileti sayısı dahil olmak üzere kuyruk hizmeti sıra özelliklerini almayı ister. [Beklenen Teiletisayısı](/dotnet/api/azure.storage.queues.models.queueproperties.approximatemessagescount) özelliği kuyruktaki iletilerin yaklaşık sayısını içerir. Bu sayı kuyruktaki gerçek ileti sayısından daha düşük değil, ancak daha yüksek olabilir.

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_GetQueueLength":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. [FetchAttributes](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.fetchattributes?view=azure-dotnet-legacy) yöntemi, ileti sayısı dahil olmak üzere Kuyruk hizmetinden kuyruk özniteliklerini almasını ister. [Beklenen Temessagecount](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.approximatemessagecount?view=azure-dotnet-legacy) özelliği, `FetchAttributes` kuyruk hizmeti çağrılmadan, yöntemin aldığı son değeri döndürür.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

---

## <a name="delete-a-queue"></a>Bir kuyruk silme

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki [Sil](/dotnet/api/azure.storage.queues.queueclient.delete) yöntemini çağırın.

:::code language="csharp" source="~/azure-storage-snippets/queues/howto/dotnet/dotnet-v12/QueueBasics.cs" id="snippet_DeleteQueue":::

# <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki [Sil](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.delete?view=azure-dotnet-legacy) yöntemini çağırın.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```

---

## <a name="next-steps"></a>Sonraki adımlar

Kuyruk depolamanın temellerini öğrendiğinize göre, daha karmaşık depolama görevleri hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin:

- Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Kuyruk hizmeti başvuru belgelerini görüntüleyin:
  - [.NET için depolama Istemci kitaplığı başvurusu](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  - [REST API başvurusu](https://msdn.microsoft.com/library/azure/dd179355)
- [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) kullanarak Azure Storage ile birlikte çalışmak üzere yazdığınız kodları nasıl sadeleştireceğinizi öğrenin.
- Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.
  - Yapılandırılmış verileri depolamak için [.NET kullanarak Azure Table Storage’ı kullanmaya başlayın](../../cosmos-db/table-storage-how-to-use-dotnet.md).
  - Yapılandırılmamış verileri depolamak için [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](../blobs/storage-dotnet-how-to-use-blobs.md).
  - İlişkisel verileri depolamak için [.NET (C#) kullanarak SQL Veritabanı'na bağlanın](../../azure-sql/database/connect-query-dotnet-core.md).

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating an Azure Project in Visual Studio]: https://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[OData]: https://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: https://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: https://nuget.org/packages/System.Spatial/5.0.2
