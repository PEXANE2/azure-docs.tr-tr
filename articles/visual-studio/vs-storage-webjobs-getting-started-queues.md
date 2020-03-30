---
title: Visual Studio (WebJob projeleri) kullanarak sıra depolama ya da sıra depolama ile başlarken
description: Visual Studio'ya bağlı hizmetleri kullanarak bir depolama hesabına bağlandıktan sonra WebJob projesinde Azure Kuyruk depolama sını kullanmaya nasıl başlanırsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ffba203bafaf3837cd2d7fc1a6fd962a6926b186
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72298757"
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Azure Queue depolama ve Visual Studio'ya bağlı hizmetler (WebJob Projects) ile başlarken
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Visual Studio **Add Connected Services** iletişim kutusunu kullanarak bir Azure depolama hesabı oluşturduktan veya başvurulan bir Görsel Stüdyo Azure Webİş projesinde Azure Kuyruk depolama alanı kullanmaya nasıl başlanır. Visual Studio **Add Connected Services** iletişim kutusunu kullanarak bir WebJob projesine bir depolama hesabı eklediğinizde, uygun Azure Depolama NuGet paketleri yüklenir, projeye uygun .NET başvuruları eklenir ve depolama hesabının bağlantı dizeleri App.config dosyasında güncelleştirilir.  

Bu makalede, Azure Sırası depolama hizmetiyle Azure Web İşleri SDK sürüm 1.x'in nasıl kullanılacağını gösteren C# kodu örnekleri verilmektedir.

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir. Bkz. Daha fazla bilgi için [.NET'i kullanarak Azure Sıra Depolama'ya başlayın.](../storage/queues/storage-dotnet-how-to-use-queues.md) ASP.NET hakkında daha fazla bilgi için [ASP.NET.](https://www.asp.net)

## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Sıra iletisi alındığı zaman işlev tetikleme
Bir sıra iletisi aldığında WebJobs SDK'nın çağırdığı bir işlev yazmak için **QueueTrigger** özniteliğini kullanın. Öznitelik oluşturucu, yoklama sırasının adını belirten bir dize parametresi alır. Sıra adının dinamik olarak nasıl ayarlanabildiğini görmek [için Yapılandırma Seçenekleri'ni nasıl ayarlayacağına](#how-to-set-configuration-options)göz atın.

### <a name="string-queue-messages"></a>Dize sıra iletileri
Aşağıdaki örnekte, sıra bir dize iletisi içerdiğinden, **QueueTrigger,** queue iletisinin içeriğini içeren **logMessage** adlı bir dize parametresine uygulanır. İşlev [Pano'ya bir günlük iletisi yazar.](#how-to-write-logs)

```csharp
public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    logger.WriteLine(logMessage);
}
```

**Dize**yanı sıra, parametre bir bayt dizisi, bir **CloudQueueMessage** nesnesi veya tanımladığınız bir POCO olabilir.

### <a name="poco-plain-old-clr-object-queue-messages"></a>POCO [(Düz Eski CLR](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)Nesnesi) sıra iletileri
Aşağıdaki örnekte, sıra iletisi **BlobName** özelliği içeren bir **BlobInformation nesnesi** için JSON içerir. SDK nesneyi otomatik olarak deserialize eder.

```csharp
public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
{
    logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
}
```

SDK, iletileri seri hale getirmek ve deserialize etmek için [Newtonsoft.Json NuGet paketini](https://www.nuget.org/packages/Newtonsoft.Json) kullanır. Webİşler SDK'sını kullanmayan bir programda sıra iletileri oluşturursanız, SDK'nın ayrıştırabileceği bir POCO sıra iletisi oluşturmak için aşağıdaki örnekteki gibi kod yazabilirsiniz.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

### <a name="async-functions"></a>Async fonksiyonları
Aşağıdaki async işlevi [Pano'ya bir günlük yazar.](#how-to-write-logs)

```csharp
public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
{
    await logger.WriteLineAsync(logMessage);
}
```

Async işlevleri, aşağıdaki örnekte gösterildiği gibi, bir blob kopyalayan bir [iptal belirteci](https://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken)alabilir. **(QueueTrigger** yer tutucusu yla ilgili bir açıklama için [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) bölümüne bakın.)

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
    CancellationToken token)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="types-the-queuetrigger-attribute-works-with"></a>QueueTrigger özniteliğinin çalışmasını türleri
**QueueTrigger'ı** aşağıdaki türlerle kullanabilirsiniz:

* **Dize**
* JSON olarak serihale getirilen bir POCO türü
* **bayt[]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Yoklama algoritması
SDK, boşta sıra yoklamanın depolama işlem maliyetleri üzerindeki etkisini azaltmak için rasgele bir üstel geri leme algoritması uygular.  Bir ileti bulunduğunda, SDK iki saniye bekler ve sonra başka bir ileti için denetler; hiçbir ileti bulunduğunda yeniden denemeden önce yaklaşık dört saniye bekler. Bir sıra iletisi almak için sonraki başarısız girişimleri sonra, bekleme süresi varsayılan bir dakika maksimum bekleme süresine ulaşana kadar artmaya devam eder. [Maksimum bekleme süresi yapılandırılabilir.](#how-to-set-configuration-options)

## <a name="multiple-instances"></a>Birden çok örnek
Web uygulamanız birden çok örnekte çalışıyorsa, her makinede sürekli bir Webİşler çalışır ve her makine tetikleyicileri bekler ve işlevleri çalıştırmayı dener. Bazı senaryolarda bu, bazı işlevlerin aynı verileri iki kez işlemesine yol açabilir, bu nedenle işlevler idempotent olmalıdır (aynı giriş verileriyle tekrar tekrar aramanın yinelenen sonuçlar oluşturmaması için yazılır).  

## <a name="parallel-execution"></a>Paralel yürütme
Farklı kuyruklarda dinleme birden çok işleviniz varsa, iletiler aynı anda alındığı zaman SDK bunları paralel olarak çağırır.

Aynı durum, tek bir sıra için birden çok ileti alındığı zaman da geçerlidir. Varsayılan olarak, SDK bir seferde 16 sıra iletisi toplu alır ve bunları paralel olarak işleyen işlevi yürütür. [Toplu iş boyutu yapılandırılabilir.](#how-to-set-configuration-options) İşlenen sayı toplu iş boyutunun yarısına düştüğünde, SDK başka bir toplu iş alır ve bu iletileri işlemeye başlar. Bu nedenle, işlev başına işlenen en fazla eşzamanlı ileti sayısı, toplu iş boyutunun bir buçuk katıdır. Bu sınır, **QueueTrigger** özniteliği olan her işlev için ayrı ayrı geçerlidir. Bir kuyruğa alınan iletiler için paralel yürütme istemiyorsanız, toplu iş boyutunu 1 olarak ayarlayın.

## <a name="get-queue-or-queue-message-metadata"></a>Sıra veya sıra iletisi meta verisi alma
Yöntem imzasına parametreler ekleyerek aşağıdaki ileti özelliklerini alabilirsiniz:

* **DateTimeOffset** sona erme Zamanı
* **DateTimeOffset** eklemeTime
* **DateTimeOffset** nextVisibleTime
* **string** queueTrigger (ileti metni içerir)
* **string** id
* **dize** popReceipt
* **int** dequeueCount

Doğrudan Azure depolama API'si ile çalışmak istiyorsanız, bir **CloudStorageAccount** parametresi de ekleyebilirsiniz.

Aşağıdaki örnek, tüm bu meta verileri bir BİlGİ uygulama günlüğüne yazar. Örnekte, hem logMessage hem de queueTrigger, sıra iletisinin içeriğini içerir.

```csharp
public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
    DateTimeOffset expirationTime,
    DateTimeOffset insertionTime,
    DateTimeOffset nextVisibleTime,
    string id,
    string popReceipt,
    int dequeueCount,
    string queueTrigger,
    CloudStorageAccount cloudStorageAccount,
    TextWriter logger)
{
    logger.WriteLine(
        "logMessage={0}\n" +
        "expirationTime={1}\ninsertionTime={2}\n" +
        "nextVisibleTime={3}\n" +
        "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
        "queue endpoint={7} queueTrigger={8}",
        logMessage, expirationTime,
        insertionTime,
        nextVisibleTime, id,
        popReceipt, dequeueCount,
        cloudStorageAccount.QueueEndpoint,
        queueTrigger);
}
```

Burada örnek kodu tarafından yazılmış bir örnek günlük:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Zarif kapatma
Sürekli bir WebJob'ta çalışan bir işlev, WebJob sonlandırılmak üzereyken işletim sisteminin işlevi bildirmesini sağlayan Bir **İptal Token** parametresini kabul edebilir. Bu bildirimi, işlevin beklenmeyen bir şekilde sonlandırmadığından emin olmak için verileri tutarsız bir durumda bırakabilecek şekilde kullanabilirsiniz.

Aşağıdaki örnek, bir işlevde yaklaşan WebJob sonlandırma sını nasıl denetleyire işaret eder.

```csharp
public static void GracefulShutdownDemo(
            [QueueTrigger("inputqueue")] string inputText,
            TextWriter logger,
            CancellationToken token)
{
    for (int i = 0; i < 100; i++)
    {
        if (token.IsCancellationRequested)
        {
            logger.WriteLine("Function was cancelled at iteration {0}", i);
            break;
        }
        Thread.Sleep(1000);
        logger.WriteLine("Normal processing for queue message={0}", inputText);
    }
}
```

**Not:** Pano, kapatılan işlevlerin durumunu ve çıktısını doğru şekilde göstermeyebilir.

Daha fazla bilgi için [Bkz. Webİşler Zarif Kapatma.](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR)   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>Sıra iletisini işlerken sıra iletisi oluşturma
Yeni bir sıra iletisi oluşturan bir işlev yazmak için **Sıra** özniteliğini kullanın. **QueueTrigger**gibi, sıra adını dize olarak geçerveya [sıra adını dinamik olarak ayarlayabilirsiniz.](#how-to-set-configuration-options)

### <a name="string-queue-messages"></a>Dize sıra iletileri
Aşağıdaki non-async kod örneği, "giriş sırası" adlı sırada alınan sıra iletisiyle aynı içeriğe sahip "outputqueue" adlı kuyrukta yeni bir sıra iletisi oluşturur. (Async işlevleri için bu bölümde daha sonra gösterildiği gibi **iAsyncCollector\<T>** kullanın.)

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] out string outputQueueMessage )
{
    outputQueueMessage = queueMessage;
}
```

### <a name="poco-plain-old-clr-object-queue-messages"></a>POCO [(Düz Eski CLR](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)Nesnesi) sıra iletileri
Dize yerine POCO içeren bir sıra iletisi oluşturmak için, **POCO** türünü Sıra özniteliği oluşturucuya çıktı parametresi olarak geçirin.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
    [Queue("outputqueue")] out BlobInformation blobInfoOutput )
{
    blobInfoOutput = blobInfoInput;
}
```

SDK nesneyi otomatik olarak JSON'a serileştirir. Nesne null olsa bile, her zaman bir sıra iletisi oluşturulur.

### <a name="create-multiple-messages-or-in-async-functions"></a>Birden çok ileti veya async işlevi oluşturma
Birden çok ileti oluşturmak için, aşağıdaki örnekte gösterildiği gibi, çıkış sırası **ICollector\<T>** veya **IAsyncCollector\<T>** için parametre türü yapın.

```csharp
public static void CreateQueueMessages(
    [QueueTrigger("inputqueue")] string queueMessage,
    [Queue("outputqueue")] ICollector<string> outputQueueMessage,
    TextWriter logger)
{
    logger.WriteLine("Creating 2 messages in outputqueue");
    outputQueueMessage.Add(queueMessage + "1");
    outputQueueMessage.Add(queueMessage + "2");
}
```

Her sıra iletisi, **Ekle** yöntemi çağrıldığında hemen oluşturulur.

### <a name="types-that-the-queue-attribute-works-with"></a>Sıra özniteliğinin birlikte çalıştığı türler
Aşağıdaki parametre türlerinde **Sıra** özniteliğini kullanabilirsiniz:

* **out string** (işlev sona erdiğinde parametre değeri null değilse sıra iletisi oluşturur)
* **out byte[]** **(dize**gibi çalışır )
* **out CloudQueueMessage** **(string**gibi çalışır)
* **poco** (serializable türü, işlev sona erdiğinde parametre null ise null nesnesi ile bir ileti oluşturur)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (doğrudan Azure Depolama API'sini el ile kullanarak ileti oluşturmak için)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Bir işlevin gövdesinde WebJobs SDK özniteliklerini kullanma
**Sıra,** **Blob**veya **Tablo**gibi bir WebJobs SDK özniteliği kullanmadan önce işlevinizde bazı işler yapmanız gerekiyorsa, **IBinder** arabirimini kullanabilirsiniz.

Aşağıdaki örnek, bir giriş sırası iletisi alır ve çıktı kuyruğunda aynı içeriğe sahip yeni bir ileti oluşturur. Çıkış sıra adı işlevin gövdesinde kod tarafından ayarlanır.

```csharp
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

**IBinder** **arabirimi, Tablo** ve **Blob** öznitelikleriyle de kullanılabilir.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Sıra iletisini işlerken lekeler ve tablolar nasıl okunur ve yazılır?
**Blob** ve **Tablo** öznitelikleri, lekeleri ve tabloları okumanızı ve yazmanızı sağlar. Bu bölümdeki örnekler lekeler için geçerlidir. Blobs oluşturulduğunda veya güncelleştirildiğinde işlemleri nasıl tetikleyeceklerini gösteren kod örnekleri [için, Web İşler SDK ile Azure blob depolama](https://github.com/Azure/azure-webjobs-sdk/wiki)sını nasıl kullanacağınız abakın.
<!-- , and for code samples that read and write tables, see [How to use Azure table storage with the WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md). -->

### <a name="string-queue-messages-triggering-blob-operations"></a>Blob işlemlerini tetikleyen dize sıra iletileri
Dize içeren bir sıra iletisi için **queueTrigger,** **Blob** özniteliğinin iletinin içeriğini içeren **blobPath** parametresinde kullanabileceğiniz bir yer tutucudur.

Aşağıdaki örnek, damlaları okumak ve yazmak için **Akış** nesnelerini kullanır. Sıra iletisi textblobs kapsayıcısında bulunan bir blob adıdır. Blob'un ada eklenen "-yeni" bir kopyası aynı kapsayıcıda oluşturulur.

```csharp
public static void ProcessQueueMessage(
    [QueueTrigger("blobcopyqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

**Blob** öznitelik oluşturucu kapsayıcı ve blob adını belirten bir **blobPath** parametresi alır. Bu yer tutucu hakkında daha fazla bilgi için, [Webİşler SDK ile Azure blob depolama nasıl kullanılır](https://github.com/Azure/azure-webjobs-sdk/wiki)bakın.

Öznitelik bir **Akış** nesnesini dekore ettiğinde, başka bir oluşturucu parametre **FileAccess** modunu okuma, yazma veya okuma/yazma olarak belirtir.

Aşağıdaki örnek, bir blob silmek için bir **CloudBlockBlob** nesnesi kullanır. Sıra iletisi blob adıdır.

```csharp
public static void DeleteBlob(
    [QueueTrigger("deleteblobqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
{
    blobToDelete.Delete();
}
```

### <a name="poco-plain-old-clr-object-queue-messages"></a>POCO [(Düz Eski CLR](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)Nesnesi) sıra iletileri
Sıra iletisinde JSON olarak depolanan bir POCO için, **Sıra** özniteliğinin **blobPath** parametresinde nesnenin özelliklerini adlandıran yer tutucular kullanabilirsiniz. Sıra meta veri özellik adlarını yer tutucu olarak da kullanabilirsiniz. Bkz. [Sıra veya sıra iletisi meta veri alın.](#get-queue-or-queue-message-metadata)

Aşağıdaki örnek, farklı bir uzantılı yeni bir blob için bir blob kopyalar. Sıra iletisi **BlobName** ve BlobNameWithoutExtension özelliklerini içeren bir **BlobInformation** **nesnesidir.** Özellik adları **Blob** öznitelikleri için blob yolunda yer tutucu olarak kullanılır.

```csharp
public static void CopyBlobPOCO(
    [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
    [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}
```

SDK, iletileri seri hale getirmek ve deserialize etmek için [Newtonsoft.Json NuGet paketini](https://www.nuget.org/packages/Newtonsoft.Json) kullanır. Webİşler SDK'sını kullanmayan bir programda sıra iletileri oluşturursanız, SDK'nın ayrıştırabileceği bir POCO sıra iletisi oluşturmak için aşağıdaki örnekteki gibi kod yazabilirsiniz.

```csharp
BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
logQueue.AddMessage(queueMessage);
```

Bir blob'u bir nesneye bağlamadan önce işlevinizde bazı işler yapmanız gerekiyorsa, [bir işlevin gövdesinde Webİşler SDK özniteliklerini kullan'da](#use-webjobs-sdk-attributes-in-the-body-of-a-function)gösterildiği gibi işlevin gövdesindeki özniteliği kullanabilirsiniz.

### <a name="types-you-can-use-the-blob-attribute-with"></a>Blob özniteliğini kullanabileceğiniz türleri
**Blob** özniteliği aşağıdaki türlerle kullanılabilir:

* **Akış** (Okuma veya yazma, FileAccess oluşturucu parametresi kullanılarak belirtilir)
* **Textreader**
* **TextWriter**
* **dize** (okundu)
* **out string** (yazma; yalnızca dize parametresi işlev döndüğünde null yoksa bir blob oluşturur)
* POCO (okundu)
* poco çıkış (yazma; her zaman bir blob oluşturur, poco parametresi işlev döndüğünde null ise null nesne olarak oluşturur)
* **CloudBlobStream** (yazmak)
* **ICloudBlob** (okuma veya yazma)
* **CloudBlockBlob** (okuma veya yazma)
* **CloudPageBlob** (okuma veya yazma)

## <a name="how-to-handle-poison-messages"></a>Zehirli iletilerle nasıl işlenir?
İçeriği bir işlevin başarısız olması nedeniyle *iletilere zehirli ileti denir.* İşlev başarısız olduğunda, sıra iletisi silinmez ve sonunda tekrar alınır ve döngü yinelenir. SDK, sınırlı sayıda yinelemeden sonra döngüyü otomatik olarak kesintiye uğratabilir veya el ile yapabilirsiniz.

### <a name="automatic-poison-message-handling"></a>Otomatik zehir iletisi işleme
SDK, bir sıra iletisini işlemek için en fazla 5 kez bir işlev çağırır. Beşinci deneme başarısız olursa, ileti zehirli sıraya taşınır. [Yapılandırma seçeneklerini ayarlama da](#how-to-set-configuration-options)yeniden maksimum sayıda yapılandırma nasıl görebilirsiniz.

Zehir sırası *{originalqueuename}*-poison olarak adlandırılır. İletileri günlüğe kaydederek veya el ile dikkat gerektiren bir bildirim göndererek zehir kuyruğundan gelen iletileri işlemek için bir işlev yazabilirsiniz.

Aşağıdaki örnekte, bir sıra iletisi var olmayan bir blob'un adını içerdiğinde **CopyBlob** işlevi başarısız olur. Bu durumda, ileti copyblobqueue sırasından copyblobqueue-zehir kuyruğuna taşınır. **ProcessPoisonMessage** daha sonra zehirli iletiyi kaydeder.

```csharp
public static void CopyBlob(
    [QueueTrigger("copyblobqueue")] string blobName,
    [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
{
    blobInput.CopyTo(blobOutput, 4096);
}

public static void ProcessPoisonMessage(
    [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
{
    logger.WriteLine("Failed to copy blob, name=" + blobName);
}
```

Aşağıdaki resimde, zehirli bir ileti işlendiğinde bu işlevlerden konsol çıktısı gösterilmektedir.

![Zehirli mesaj işleme için konsol çıkışı](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Manuel zehir ileti işleme
İşlevinize **dequeueCount** adlı **bir int** parametresi ekleyerek bir iletinin işlenmek üzere kaç kez alındığını alabilirsiniz. Daha sonra işlev kodundaki sıra silme sayısını denetleyebilir ve aşağıdaki örnekte gösterildiği gibi, sayı bir eşiği aştığında kendi zehir iletisi işlemegerçekleştirebilirsiniz.

```csharp
public static void CopyBlob(
    [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
    [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
    [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
    TextWriter logger)
{
    if (dequeueCount > 3)
    {
        logger.WriteLine("Failed to copy blob, name=" + blobName);
    }
    else
    {
        blobInput.CopyTo(blobOutput, 4096);
    }
}
```

## <a name="how-to-set-configuration-options"></a>Yapılandırma seçenekleri nasıl ayarlanır?
Aşağıdaki yapılandırma seçeneklerini ayarlamak için **JobHostConfiguration** türünü kullanabilirsiniz:

* SDK bağlantı dizelerini kod halinde ayarlayın.
* En fazla sıra silme sayısı gibi **QueueTrigger** ayarlarını yapılandırın.
* Yapılandırmadan sıra adları alın.

### <a name="set-sdk-connection-strings-in-code"></a>SDK bağlantı dizelerini kodda ayarlama
SDK bağlantı dizelerini kod olarak ayarlamak, aşağıdaki örnekte gösterildiği gibi yapılandırma dosyalarında veya ortam değişkenlerinde kendi bağlantı dize adlarınızı kullanmanıza olanak tanır.

```csharp
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    var _serviceBusConn = ConfigurationManager
        .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    config.ServiceBusConnectionString = _serviceBusConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="configure-queuetrigger--settings"></a>QueueTrigger ayarlarını yapılandırma
Sıra iletisi işleme için geçerli olan aşağıdaki ayarları yapılandırabilirsiniz:

* Aynı anda aynı anda alınan ve paralel olarak yürütülecek en fazla sıra iletisi sayısı (varsayılan değer 16'dır).
* Bir sıra iletisi zehirli sıraya gönderilmeden önce en fazla yeniden deneme sayısı (varsayılan değer 5'tir).
* Bir kuyruk boşolduğunda yeniden yoklamadan önceki maksimum bekleme süresi (varsayılan değer 1 dakikadır).

Aşağıdaki örnek, bu ayarların nasıl yapılandırılabildiğini gösterir:

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>WebJobs SDK oluşturucu parametreleri için değerleri kodda ayarlama
Bazen bir sıra adı, bir blob adı veya kapsayıcı veya kod içinde bir tablo adı yerine sabit kod belirtmek istiyorum. Örneğin, bir yapılandırma dosyasında veya ortam değişkeninde **QueueTrigger** için sıra adını belirtmek isteyebilirsiniz.

Bunu, **JobHostConfiguration** türüne bir **NameResolver** nesnesi geçirerek yapabilirsiniz. Yüzde ile çevrili özel yer tutucular (%) WebJobs SDK öznitelik oluşturucu parametrelerindeki işaretler ve **NameResolver** kodunuz bu yer tutucuların yerine kullanılacak gerçek değerleri belirtir.

Örneğin, test ortamında logqueuetest adlı bir sıra ve üretimde logqueue prod adlı bir sözcük kullanmak istediğinizi varsayalım. Sabit kodlanmış bir sıra adı yerine, **uygulama Ayarları** koleksiyonunda gerçek sıra adına sahip bir girişin adını belirtmek istiyorsunuz. **appSettings** tuşu logqueue ise, işleviniz aşağıdaki örnek gibi görünebilir.

```csharp
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

**NameResolver** sınıfınız aşağıdaki örnekte gösterildiği gibi **appSettings'ten** sıra adını alabilir:

```csharp
public class QueueNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Aşağıdaki örnekte gösterildiği gibi **JobHost** nesnesine **NameResolver** sınıfını geçersiniz.

```csharp
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new QueueNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

**Not:** Sıra, tablo ve blob adları her işlev çağrıldığında çözülür, ancak blob kapsayıcı adları yalnızca uygulama başladığında çözülür. İş çalışırken blob kapsayıcı adını değiştiremezsiniz.

## <a name="how-to-trigger-a-function-manually"></a>Bir işlevi el ile tetikleme
Bir işlevi el ile tetiklemek için, aşağıdaki örnekte gösterildiği gibi **JobHost** nesnesindeki **Çağrı** veya **CallAsync** yöntemini ve işlevdeki **NoAutomaticTrigger** özniteliğini kullanın.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        JobHost host = new JobHost();
        host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
    }

    [NoAutomaticTrigger]
    public static void CreateQueueMessage(
        TextWriter logger,
        string value,
        [Queue("outputqueue")] out string message)
    {
        message = value;
        logger.WriteLine("Creating queue message: ", message);
    }
}
```

## <a name="how-to-write-logs"></a>Günlükleri nasıl yazılır
Pano günlükleri iki yerde gösterir: WebJob için sayfa ve belirli bir WebJob çağırma sayfası.

![WebJob sayfasında oturum açar](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![İşlev çağırma sayfasında oturum açar](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Bir işlevde veya **Main()** yönteminde çağırdığınız Konsol yöntemlerinden çıkış, belirli bir yöntem çağırma sı için sayfada değil, WebJob'un Pano sayfasında görünür. Yöntem imzanızdaki bir parametreden aldığınız TextWriter nesnesinden gelen çıktı, yöntem çağrısı için Pano sayfasında görüntülenir.

Konsol tek iş parçacığı olduğundan, konsol çıkışı belirli bir yöntem çağırmasına bağlanamaz, ancak birçok iş işlevi aynı anda çalışıyor olabilir. Bu nedenle SDK her işlev çağırmasını kendi benzersiz günlük yazar nesnesi ile sağlar.

Uygulama [izleme günlüklerini](../app-service/troubleshoot-dotnet-visual-studio.md#logsoverview)yazmak için **Console.Out** (INFO olarak işaretlenmiş günlükler oluşturur) ve **Console.Error** (HATA olarak işaretlenmiş günlükler oluşturur) kullanın. Alternatif olarak, Bilgi ve Hatay'a ek olarak Verbose, Warning ve Kritik düzeyleri sağlayan [Trace veya TraceSource'u](https://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)kullanmak da mümkündür. Azure web uygulamanızı nasıl yapılandırdığınıza bağlı olarak web uygulaması günlük dosyalarında, Azure tablolarında veya Azure lekelerinde uygulama izleme günlükleri görünür. Tüm Konsol çıktıları için olduğu gibi, en son 100 uygulama günlüğü de WebJob için Pano sayfasında görünür, bir işlev çağırma için sayfa değil.

Konsol çıktısı, program yerel olarak veya başka bir ortamda çalışıyorsa değil, yalnızca bir Azure WebJob'ta çalışıyorsa, Pano'da görünür.

Pano bağlantı dizesini null'a ayarlayarak günlüğe kaydetmeyi devre dışı kullanabilirsiniz. Daha fazla bilgi için [Yapılandırma Seçenekleri'ni nasıl ayarlayacağına](#how-to-set-configuration-options)bakın.

Aşağıdaki örnek, günlükleri yazmak için çeşitli yollar gösterir:

```csharp
public static void WriteLog(
    [QueueTrigger("logqueue")] string logMessage,
    TextWriter logger)
{
    Console.WriteLine("Console.Write - " + logMessage);
    Console.Out.WriteLine("Console.Out - " + logMessage);
    Console.Error.WriteLine("Console.Error - " + logMessage);
    logger.WriteLine("TextWriter - " + logMessage);
}
```

WebJobs SDK Panosu'nda, belirli bir işlev çağrısı için sayfaya gittiğinizde **textwriter** nesnesinden gelen çıktı belirir ve **Geçiş Çıktısı'nı**seçin:

![Çağırma bağlantısı](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![İşlev çağırma sayfasında oturum açar](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

WebJobs SDK Panosu'nda, WebJob sayfasına gittiğinizde Konsol çıktısının en son 100 satırı (işlev çağırma için değil) ve **Geçiş Çıktısı'nı**seçtiğinizde ortaya çıkar.

![Çıkışı geçiş](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

Sürekli bir WebJob'ta, uygulama günlükleri web uygulaması dosya sisteminde /data/jobs/continuous/*{webjobname}*/job_log.txt adresinde yer aşar.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Bir Azure blob uygulama günlükleri şu şekilde görünür: 2014-09-26T21:01:13,Bilgi,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Merhaba dünya!, 2014-09-26T21:01:13,Hata,contosoadsnew,491e54, 635473620738373502,0,17404,19,Console.Error - Merhaba dünya!, 2014-09-26T21:01:13,Bilgi,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Merhaba dünya!,

Azure tablosunda **Console.Out** ve **Console.Error** günlükleri aşağıdaki gibi görünür:

![Bilgi giriş tablosunda](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Hata günlüğü tablosunda](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure kuyrukları ile çalışmak için ortak senaryolar nasıl işleyeceğini gösteren kod örnekleri sağlanmıştır. Azure Web İşleri ve Web İşler SDK'nın nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Web İşleri dokümankaynakları'na](https://go.microsoft.com/fwlink/?linkid=390226)bakın.

