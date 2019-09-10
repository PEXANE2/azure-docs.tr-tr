---
title: 'Örnek: Gerçek zamanlı video analizi-Görüntü İşleme'
titleSuffix: Azure Cognitive Services
description: Görüntü İşleme API’sini kullanarak canlı video akışından alınan karelerde nasıl gerçek zamanlıya yakın analiz gerçekleştirileceğini öğreneceksiniz.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 09/09/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 25aed0f042050ebadbc6054fcbf0c68dbf782e5e
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70859081"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Videoları gerçek zamanlı olarak çözümleme

Bu kılavuzda, canlı video akışından alınan karelerde nasıl gerçek zamanlıya yakın analiz gerçekleştirileceği gösterilmektedir. Böyle bir sistemdeki temel bileşenler şunlardır:

- Video kaynağından kareleri alma
- Hangi karelerin analiz edileceğini seçme
- Bu kareleri API’ye gönderme
- API çağrısından döndürülen her analiz sonucunu kullanma

Bu örnekler C# dilinde yazılmıştır ve kod, buradaki GitHub’da bulunabilir: [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>Yaklaşım

Video akışlarında gerçek zamanlıya yakın analiz çalıştırma sorununu çözmenin birçok yolu vardır. Gelişmişlik düzeyini artırma konusunda üç yaklaşımı açıklayarak başlayacağız.

### <a name="a-simple-approach"></a>Basit Bir Yaklaşım

Gerçek zamanlıya yakın bir analiz sistemi için en basit tasarım, her yinelemede bir kare yakalayıp bunu analiz ettiğimiz ve sonra sonucu kullandığımız sonsuz bir döngüdür:

```csharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        AnalysisResult r = await Analyze(f);
        ConsumeResult(r);
    }
}
```

Analizimiz hafif bir istemci algoritmasından oluştuysa bu yaklaşım uygun olacaktır. Ancak analizimiz bulutta gerçekleşirken oluşan gecikme, bir API çağrısının birkaç saniye sürebileceği anlamına gelir; bu süre zarfında görüntü yakalamayız ve iş parçacığımız esasta herhangi bir şey yapmaz. Maksimum kare hızımız, API çağrılarının gecikme süresiyle sınırlıdır.

### <a name="parallelizing-api-calls"></a>API Çağrılarını Paralelleştirme

Basit bir tek iş parçacıklı döngü, hafif istemci tarafı algoritma için mantıklı olsa da, bulut API çağrılarında yer alan gecikme süresine tam uymaz. Bu sorunun çözümü, uzun süre çalıştırılan API çağrılarının kare ele geçirme ile paralel şekilde yürütülmesine izin verilmesidir. C# dilinde, Görev tabanlı paralellik kullanarak bunu elde edebiliriz; örneğin:

```csharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        var t = Task.Run(async () =>
        {
            AnalysisResult r = await Analyze(f);
            ConsumeResult(r);
        }
    }
}
```

Bu yaklaşım her bir analizi ayrı bir Görevde başlatır ve biz yeni kareleri ele geçirirken görev arka planda çalıştırılabilir. Bu, bir API çağrısının döndürülmesi beklenirken ana iş parçacığının engellenmesini önler, ancak basit sürümün sağladığı garantilerden bazılarını kaybettik; paralel olarak birden çok API çağrısı olabilir ve sonuçlar yanlış sırayla döndürülebilir. Bu yaklaşım, birden çok iş parçacığının eş zamanlı olarak ConsumeResult() işlevi girmesine neden olabilir; işlev iş parçacığı açısından güvenli değilse bu tehlikeli olabilir. Son olarak bu basit kod, oluşturulan Görevleri takip etmez, bu nedenle özel durumlar sessizce kaybolur. Bu nedenle eklememiz gereken son bileşen, analiz görevlerini izleyecek, özel durumları ortaya çıkaracak, uzun süre çalıştırılan görevleri durduracak ve sonuçların birer birer doğru sırayla kullanılmasını sağlayacak bir “tüketici” iş parçacığı eklemektir.

### <a name="a-producer-consumer-design"></a>Üretici-Tüketici Tasarımı

Son “üretici-tüketici” sistemimizde, önceki sonsuz döngümüze benzer görünen bir üretici iş parçacığımız vardır. Ancak üretici, kullanılabilir olduğu anda analiz sonuçlarını kullanmak yerine görevleri takip etmek için bir kuyruğa koyar.

```csharp
// Queue that will contain the API call tasks.
var taskQueue = new BlockingCollection<Task<ResultWrapper>>();

// Producer thread.
while (true)
{
    // Grab a frame.
    Frame f = GrabFrame();

    // Decide whether to analyze the frame.
    if (ShouldAnalyze(f))
    {
        // Start a task that will run in parallel with this thread.
        var analysisTask = Task.Run(async () =>
        {
            // Put the frame, and the result/exception into a wrapper object.
            var output = new ResultWrapper(f);
            try
            {
                output.Analysis = await Analyze(f);
            }
            catch (Exception e)
            {
                output.Exception = e;
            }
            return output;
        }

        // Push the task onto the queue.
        taskQueue.Add(analysisTask);
    }
}
```

Ayrıca görevleri kuyruktan çıkaran, tamamlanmasını bekleyen ve sonucu görüntüleyen veya oluşturulan özel durumu ortaya çıkaran bir tüketici iş parçacığımız da vardır. Kuyruğu kullanarak, sistemin maksimum kare hızını sınırlamadan sonuçların birer birer doğru sırayla kullanılmasını garanti edebiliriz.

```csharp
// Consumer thread.
while (true)
{
    // Get the oldest task.
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Await until the task is completed.
    var output = await analysisTask;

    // Consume the exception or result.
    if (output.Exception != null)
    {
        throw output.Exception;
    }
    else
    {
        ConsumeResult(output.Analysis);
    }
}
```

## <a name="implementing-the-solution"></a>Çözümü uygulama

### <a name="getting-started"></a>Başlarken

Uygulamanızı mümkün olduğunda hızlı şekilde çalışır duruma getirmek için, bir yandan kullanılabilir olmasını sağlarken diğer yandan birçok senaryoyu uygulamak için yeterince esnek olmak amacıyla yukarıda açıklanan sistemi uyguladık. Koda erişmek için [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis) sayfasına gidin.

Kitaplık, bir web kamerasından gelen video karelerini işlemek için yukarıda açıklanan üretici-tüketici sistemini uygulayan FrameGrabber sınıfını içerir. Kullanıcı, API çağrısının tam formunu belirtebilir ve sınıf, çağırma kodunun ne zaman yeni bir kare alındığını veya yeni bir analiz sonucunun mevcut olduğunu bilmesini sağlamak için olayları kullanır.

Bazı olasılıkları göstermek için, kitaplığı kullanan iki örnek uygulama vardır. Birincisi basit bir konsol uygulamasıdır ve bunun basitleştirilmiş bir sürümü aşağıda oluşturulmuştur. Varsayılan web kamerasından kareleri ele geçirir ve yüz algılama için Yüz Tanıma API’sine bunları gönderir.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;
using VideoFrameAnalyzer;

namespace BasicConsoleSample
{
    internal class Program
    {
        const string ApiKey = "<your API key>";
        const string Endpoint = "https://<your API region>.api.cognitive.microsoft.com";

        private static void Main(string[] args)
        {
            // Create grabber.
            FrameGrabber<DetectedFace[]> grabber = new FrameGrabber<DetectedFace[]>();

            // Create Face API Client.
            FaceClient faceClient = new FaceClient(new ApiKeyServiceClientCredentials(ApiKey))
            {
                Endpoint = Endpoint
            };

            // Set up a listener for when we acquire a new frame.
            grabber.NewFrameProvided += (s, e) =>
            {
                Console.WriteLine($"New frame acquired at {e.Frame.Metadata.Timestamp}");
            };

            // Set up Face API call.
            grabber.AnalysisFunction = async frame =>
            {
                Console.WriteLine($"Submitting frame acquired at {frame.Metadata.Timestamp}");
                // Encode image and submit to Face API.
                return (await faceClient.Face.DetectWithStreamAsync(frame.Image.ToMemoryStream(".jpg"))).ToArray();
            };

            // Set up a listener for when we receive a new result from an API call.
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.TimedOut)
                    Console.WriteLine("API call timed out.");
                else if (e.Exception != null)
                    Console.WriteLine("API call threw an exception.");
                else
                    Console.WriteLine($"New result received for frame acquired at {e.Frame.Metadata.Timestamp}. {e.Analysis.Length} faces detected");
            };

            // Tell grabber when to call API.
            // See also TriggerAnalysisOnPredicate
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running in the background.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for key press to stop.
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();

            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```

İkinci örnek uygulama biraz daha ilginçtir ve video karelerinde hangi API’yi çağıracağınızı seçmenize olanak sağlar. Sol kısımda uygulama, canlı videonun bir önizlemesini gösterir, sağ kısımdaysa ilgili karenin üzerine yerleştirilmiş şekilde en son API sonucunu gösterir.

Çoğu modda, soldaki canlı video ile sağdaki görselleştirilmiş analiz arasında görünür bir gecikme olacaktır. Bu gecikme, API çağrısı yapmak için geçen süredir. Bunun istisnası, Bilişsel Hizmetler’e görüntü göndermeden önce OpenCV kullanarak istemci bilgisayarda yerel olarak yüz algılama gerçekleştiren "EmotionsWithClientFaceDetect" modunda söz konusudur. Bunu yaparak, algılanan yüzü hemen görselleştirebilir ve sonra API çağrısı döndürüldükten sonra duyguları güncelleştirebilirsiniz. Bu, istemcide bazı basit işlemlerin gerçekleştirildiği ve gerektiğinde daha gelişmiş analizle bunu genişletmek için Bilişsel Hizmetler API’sinin kullanılabildiği "hibrit" yaklaşım olasılığını göstermektedir.

![Etiketlerin görüntülendiği bir görüntüyü gösteren LiveCameraSample uygulamasının ekran görüntüsü](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Kod temelinizle tümleştirme

Bu örneği kullanmaya başlamak için şu adımları izleyin:

1. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/)’den Görüntü İşleme API’leri için API anahtarlarını alın. Video karesi analizi için geçerli API’ler şunlardır:
    - [Görüntü İşleme API’si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Yüz Tanıma API’si](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub deposunu kopyalayın

3. Visual Studio 2015 veya sonraki sürümlerde örneği açın, örnek uygulamaları derleyin ve çalıştırın:
    - BasicConsoleSample için Yüz Tanıma API’si doğrudan [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs) dizininde sabit kodlanmıştır.
    - LiveCameraSample için anahtarlar, uygulamanın Ayarlar bölmesine girilmelidir. Oturumlarda kullanıcı verileri olarak kalıcı duruma getirilir.

Tümleştirmeye hazır olduğunuzda **kendi projelerinizden VideoFrameAnalyzer kitaplığına başvurmanız yeterlidir.**

VideoFrameAnalyzer görüntü, ses, video veya metin anlama özellikleri, Azure Bilişsel Hizmetler’i kullanır. Microsoft, (bu uygulama aracılığıyla) karşıya yüklediğiniz görüntüleri, ses, video ve diğer verileri alır ve hizmeti iyileştirme amacıyla bunları kullanabilir. Uygulamanızın verilerini Azure Bilişsel Hizmetler’e gönderdiği kişilerin korunması için yardımınızı rica ediyoruz.

## <a name="summary"></a>Özet

Bu kılavuzda, yüz ve Görüntü İşleme API 'Leri kullanarak canlı video akışları üzerinde neredeyse gerçek zamanlı analizler çalıştırmayı ve örnek kodumuzu kullanmaya başlamak için nasıl kullanabileceğinizi öğrendiniz. [Azure Bilişsel Hizmetler kayıt sayfasında](https://azure.microsoft.com/try/cognitive-services/) ücretsiz API anahtarları ile uygulamanızı derlemeye başlayabilirsiniz.

Lütfen [GitHub deposunda](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) rahatça geri bildirim ve öneriler sağlayın veya daha kapsamlı API geri bildirimi için [UserVoice sitemize](https://cognitive.uservoice.com/) bakın.

