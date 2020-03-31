---
title: Visual Studio (WebJob projeleri) kullanarak blob depolama ile başlayın
description: Visual Studio'ya bağlı hizmetleri kullanarak bir Azure depolama alanına bağlandıktan sonra Bir WebJob projesinde Blob depolamasını kullanmaya nasıl başlarsınız?
services: storage
author: ghogen
manager: jillfra
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 90aa824b7df575eb2783ece5bd88322f0b55f0a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72299968"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Azure Blob depolama ve Visual Studio bağlantılı hizmetler (WebJob projeleri) ile başlayın
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, bir Azure blob oluşturulduğunda veya güncelleştirildiğinde bir işlemin nasıl tetiklenilen i Kod örnekleri [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) sürüm 1.x'i kullanır. Visual Studio **Add Connected Services** iletişim kutusunu kullanarak bir WebJob projesine bir depolama hesabı eklediğinizde, uygun Azure Depolama NuGet paketi yüklenir, projeye uygun .NET başvuruları eklenir ve depolama hesabının bağlantı dizeleri App.config dosyasında güncelleştirilir.

## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Bir blob oluşturulduğunda veya güncelleştirildiğinde işlev nasıl tetiklenir?
Bu bölümde **BlobTrigger** özniteliğinasıl kullanılacağı nı gösterilmektedir.

 **Not:** WebJobs SDK, yeni veya değiştirilmiş lekeleri izlemek için günlük dosyalarını tarar. Bu süreç doğal olarak yavaştır; bir işlev, blob oluşturulduktan sonra birkaç dakika veya daha uzun süre tetiklenmeyebilir.  Uygulamanızın blob'ları hemen işlemesi gerekiyorsa, önerilen yöntem, blob'u oluştururken bir sıra iletisi oluşturmak ve blob'u işleyen işlevde **BlobTrigger** özniteliği yerine **QueueTrigger** özniteliğini kullanmaktır.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Uzantılı blob adı için tek yer tutucu
Aşağıdaki kod *örneği, giriş* kapsayıcısında görünen metin bloblarını *çıktı* kapsayıcısına kopyalar:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Öznitelik oluşturucu, kapsayıcı adını ve blob adı için bir yer tutucubelirten bir dize parametresi alır. Bu örnekte, *giriş* kabında *Blob1.txt* adında bir blob *oluşturulursa,* işlev çıkış kabında *Blob1.txt* adında bir blob oluşturur.

Aşağıdaki kod örneğinde gösterildiği gibi, blob adı yer tutucusuyla birlikte bir ad deseni belirtebilirsiniz:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Bu kod yalnızca "özgün" ile başlayan adları olan lekeleri kopyalar. Örneğin, *giriş* kabındaki *orijinal Blob1.txt* *çıkış* kabındaki *copy-Blob1.txt'ye* kopyalanır.

Adında kıvırcık ayraçları olan blob adları için bir ad deseni belirtmeniz gerekiyorsa, kıvırcık ayraçları iki katına çıkar. Örneğin, *resim* kapsayıcısında şu gibi adları olan lekeler bulmak istiyorsanız:

        {20140101}-soundfile.mp3

desenin için bunu kullanın:

        images/{{20140101}}-{name}

Örnekte, *ad* yer tutucu değeri *soundfile.mp3*olacaktır.

### <a name="separate-blob-name-and-extension-placeholders"></a>Ayrı blob adı ve uzatma yer tutucuları
Aşağıdaki kod *örneği, giriş* kabında görünen lekeleri *çıktı* kapsayıcısına kopyalarken dosya uzantısını değiştirir. Kod *giriş* blob uzantısı nı kaydeder ve *çıktı* blob uzantısını *.txt*olarak ayarlar.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Blobs'a bağlayabileceğiniz türler
Aşağıdaki türlerde **BlobTrigger** özniteliğini kullanabilirsiniz:

* **Dize**
* **Textreader**
* **Akış**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder) tarafından deserialized diğer türleri

Doğrudan Azure depolama hesabıyla çalışmak istiyorsanız, yöntem imzasına bir **CloudStorageAccount** parametresi de ekleyebilirsiniz.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Dize bağlanarak metin blob içeriği alma
Metin lekeleri bekleniyorsa, **BlobTrigger** bir **dize** parametresine uygulanabilir. Aşağıdaki kod örneği, metin blob'u **logMessage**adlı bir **dize** parametresine bağlar. İşlev, blob'un içeriğini WebJobs SDK panosuna yazmak için bu parametreyi kullanır.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>ICloudBlobStreamBinder kullanarak serileştirilmiş blob içeriği alma
Aşağıdaki kod örneği, **BlobTrigger** özniteliğinin bir blob'u **WebImage** türüne bağlamasını etkinleştirmek için **ICloudBlobStreamBinder'ı** uygulayan bir sınıf kullanır.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

**WebImage** bağlama kodu **ICloudBlobStreamBinder**türetilen bir **WebImageBinder** sınıfında sağlanır.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Nasıl zehir lekeleri işlemek için
**BlobTrigger** işlevi başarısız olduğunda, hatanın geçici bir hatadan kaynaklanması durumunda SDK onu yeniden çağırır. Hata blob içeriğineden ise, işlevi blob işlemek için çalışır her zaman başarısız olur. Varsayılan olarak, SDK belirli bir leke için en fazla 5 kez bir işlev çağırır. Beşinci deneme başarısız olursa, SDK *webişleri-blobtrigger-poison*adlı bir kuyruğa bir ileti ekler.

En fazla yeniden deneme sayısı yapılandırılabilir. Aynı **MaxDequeueCount** ayarı zehirli leke işleme ve zehir sırası ileti işleme için kullanılır.

Zehirli lekeler için sıra iletisi aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId *({WebJob adı}* biçiminde. Işlev. *{Fonksiyon adı}*, örneğin: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" veya "PageBlob")
* ContainerName
* BlobName
* ETag (bir blob sürüm tanımlayıcı, örneğin: "0x8D1DC6E70A277EF")

Aşağıdaki kod örneğinde, **CopyBlob** işlevinin her çağrıldığında başarısız olmasına neden olan bir kodu vardır. SDK bunu maksimum yeniden deneme sayısı için çağırdıktan sonra, zehirli blob kuyruğunda bir ileti oluşturulur ve bu ileti **LogPoisonBlob** işlevi tarafından işlenir.

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK, JSON iletisini otomatik olarak deserialize eder. Burada **PoisonBlobMessage** sınıfı:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Blob yoklama algoritması
WebJobs SDK, uygulama başlangıcında **BlobTrigger** öznitelikleri tarafından belirtilen tüm kapsayıcıları tarar. Büyük bir depolama hesabında bu tazyik biraz zaman alabilir, bu nedenle yeni lekelerin bulunması ve **BlobTrigger** işlevlerinin yürütülmesi biraz zaman alabilir.

Uygulama başladıktan sonra yeni veya değiştirilmiş lekeleri algılamak için, SDK periyodik olarak blob depolama günlüklerinden okur. Blob günlükleri arabelleğe alınır ve yalnızca fiziksel olarak her 10 dakikada bir yazılır, bu nedenle ilgili **BlobTrigger** işlevi yürütülmeden önce bir blob oluşturulduktan veya güncelleştirildikten sonra önemli bir gecikme olabilir.

**Blob** özniteliğini kullanarak oluşturduğunuz lekeler için bir özel durum vardır. WebJobs SDK yeni bir blob oluşturduğunda, eşleşen **BlobTrigger** işlevlerine hemen yeni blob geçer. Bu nedenle, blob giriş ve çıkışları zinciriniz varsa, SDK bunları verimli bir şekilde işleyebilir. Ancak, blob işleme işlevlerinizi başka yollarla oluşturulan veya güncelleştiren blob'lar için çalıştıran düşük gecikme lirliği istiyorsanız, **BlobTrigger**yerine **QueueTrigger** kullanmanızı öneririz.

### <a name="blob-receipts"></a>Blob makbuzları
WebJobs SDK, hiçbir **BlobTrigger** işlevinin aynı yeni veya güncelleştirilmiş blob için birden fazla çağrılmamasını sağlar. Belirli bir blob sürümünün işlenilip işlenmediğini belirlemek için *blob makbuzlarını* koruyarak bunu yapar.

Blob girişleri, AzureWebJobsStorage bağlantı dizesi tarafından belirtilen Azure depolama hesabında *azure-webişleri-ana bilgisayarları* adlı bir kapsayıcıda depolanır. Bir blob makbuz aşağıdaki bilgileri vardır:

* Blob için çağrılan işlev ("*{WebJob adı}*. Işlev. *{Fonksiyon adı}*", örneğin: "WebJob1.Functions.CopyBlob")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (bir blob sürüm tanımlayıcı, örneğin: "0x8D1DC6E70A277EF")

Bir blob'un yeniden işlenmesini zorlamak istiyorsanız, bu blob için blob makbuzunun el ile silmesi *azure-webjobs-hosts* kapsayıcısından.

## <a name="related-topics-covered-by-the-queues-article"></a>Kuyruklar makale kapsamındaki ilgili konular
Bir sıra iletisi tarafından tetiklenen blob işleme nin nasıl işleyeceğiniz veya blob işlemeye özgü olmayan Web İşler SDK senaryoları hakkında bilgi için, [Web İşler SDK ile Azure kuyruk depolamasını nasıl kullanacağınız](https://github.com/Azure/azure-webjobs-sdk/wiki)konusuna bakın.

Bu makalede ele alınan ilgili konular şunlardır:

* Async fonksiyonları
* Birden çok örnek
* Zarif kapatma
* Bir işlevin gövdesinde WebJobs SDK özniteliklerini kullanma
* SDK bağlantı dizelerini kod halinde ayarlayın.
* WebJobs SDK oluşturucu parametreleri için değerleri kodda ayarlama
* Zehirli leke işleme için **MaxDequeueCount'u** yapılandırın.
* Bir işlevi el ile tetikleme
* Günlükleri yazma

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure lekeleri ile çalışmak için ortak senaryoların nasıl işleyeceğini gösteren kod örnekleri sağlanmıştır. Azure Web İşleri ve Web İşler SDK'nın nasıl kullanılacağı hakkında daha fazla bilgi için Azure [Web İşleri dokümankaynakları'na](https://go.microsoft.com/fwlink/?linkid=390226)bakın.

