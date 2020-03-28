---
title: Videoları neredeyse gerçek zamanlı olarak analiz edin - Bilgisayar Lı Vizyon
titleSuffix: Azure Cognitive Services
description: BilgisayarLı Vizyon API'sini kullanarak canlı video akışından alınan karelerde gerçek zamanlı olarak nasıl analiz edilenlerle ilgili bilgi edinin.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 09/09/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 18b158b7a4881619b93ab404de67f7bb25f92b6a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76166822"
---
# <a name="analyze-videos-in-near-real-time"></a>Videoları neredeyse gerçek zamanlı olarak analiz edin

Bu makalede, Bilgisayar Lı Vizyon API'sını kullanarak canlı video akışından alınan kareler üzerinde gerçek zamanlı çözümlemenin nasıl yapılacağını gösterin. Böyle bir analizin temel unsurları şunlardır:

- Bir video kaynağından kare alma.
- Hangi çerçevelerin çözümlenenegerektiğini seçme.
- Bu çerçeveleri API'ye gönderme.
- API çağrısından döndürülen her analiz sonucunu tüketme.

Bu makaledeki örnekler C# olarak yazılmıştır. Koda erişmek için GitHub'daki [Video çerçeve analizi örnek](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) sayfasına gidin.

## <a name="approaches-to-running-near-real-time-analysis"></a>Gerçek zamanlı analize yakın çalışma yaklaşımları

Çeşitli yaklaşımlar kullanarak video akışlarında gerçek zamanlı çözümlemenin yakınında çalışma sorununu çözebilirsiniz. Bu makalede, sofistike artan düzeylerde, bunlardan üç özetliyor.

### <a name="design-an-infinite-loop"></a>Sonsuz bir döngü tasarla

Yakın gerçek zamanlı analiz için en basit tasarım sonsuz bir döngüdür. Bu döngünün her yinelemesinde, bir çerçeve kapmak, analiz ve sonra sonucu tüketmek:

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

Analiziniz hafif, istemci tarafı algoritması oluşacaksa, bu yaklaşım uygun olacaktır. Ancak, çözümleme bulutta gerçekleştiğinde, ortaya çıkan gecikme, bir API çağrısının birkaç saniye sürebileceği anlamına gelir. Bu süre zarfında, görüntüleri yakalayamamanız ve iş parçacığınız aslında hiçbir şey yapmıyor. Maksimum kare hızınız, API çağrılarının gecikme süresiyle sınırlıdır.

### <a name="allow-the-api-calls-to-run-in-parallel"></a>API çağrılarının paralel çalışmasına izin ver

Basit, tek iş parçacığı döngü hafif, istemci tarafı algoritması için mantıklı olsa da, bir bulut API aramasının gecikmesiyle iyi uyum sağlar. Bu sorunun çözümü, uzun süredir devam eden API çağrısının çerçeve kapma ile paralel olarak çalışmasına izin vermektir. C#'da bunu görev tabanlı paralellik kullanarak yapabilirsiniz. Örneğin, aşağıdaki kodu çalıştırabilirsiniz:

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

Bu yaklaşımla, her çözümlemesi ayrı bir görevde başlatın. Yeni kareler kapma devam ederken görev arka planda çalıştırabilirsiniz. Bir API çağrısının geri dönmesini beklerken yaklaşım ana iş parçacığının engellenmesini önler. Ancak, yaklaşım bazı dezavantajları sunabilir:
* Bu basit sürümü sağlanan bazı garantiler maliyeti. Diğer bir deyişle, birden çok API çağrısı paralel olarak oluşabilir ve sonuçlar yanlış sırada döndürülebilir. 
* Ayrıca, birden çok iş parçacığının Aynı anda ConsumeResult() işlevine girmesine neden olabilir ve bu da işlev iş parçacığı güvenli değilse tehlikeli olabilir. 
* Son olarak, bu basit kod oluşturulan görevleri izlemez, bu nedenle özel durumlar sessizce kaybolur. Bu nedenle, analiz görevlerini izleyen, özel durumları artıran, uzun süren görevleri öldüren ve sonuçların teker teker doğru sırada tüketilmesini sağlayan bir "tüketici" iş parçacığı eklemeniz gerekir.

### <a name="design-a-producer-consumer-system"></a>Üretici-tüketici sistemi tasarlama

Son yaklaşımınız için, bir "üretici-tüketici" sistemi tasarlayarak, daha önce bahsedilen sonsuz döngüye benzeyen bir üretici iş parçacığı oluşturursunuz. Ancak, analiz sonuçlarını kullanılabilir olur olmaz tüketmek yerine, üretici görevleri izlemek için sıraya yerleştirir.

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

Ayrıca, görevleri sıranın dışına çıkaran, bunların tamamlanmasını bekleyen ve sonucu görüntüleyen veya atılan özel durumu yükselten bir tüketici iş parçacığı da oluşturursunuz. Sırayı kullanarak, sonuçların sistemin maksimum kare hızını sınırlamadan doğru sırada teker teker tüketilmelerini garanti edebilirsiniz.

```csharp
// Consumer thread.
while (true)
{
    // Get the oldest task.
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Wait until the task is completed.
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

## <a name="implement-the-solution"></a>Çözümü uygulayın

### <a name="get-started-quickly"></a>Hızlı bir şekilde başlayın

Uygulamanızın mümkün olan en kısa sürede çalışır hale gelmelerine yardımcı olmak için, önceki bölümde açıklanan sistemi uyguladık. Kullanımı kolay olmakla birlikte, birçok senaryoyu barındıracak kadar esnek olması amaçlanmıştır. Koda erişmek için GitHub'daki [Video çerçeve analizi örnek](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) sayfasına gidin.

Kitaplık, `FrameGrabber` web kamerasından video çerçevelerini işlemek için daha önce tartışılan üretici-tüketici sistemini uygulayan sınıfı içerir. Kullanıcılar API çağrısının tam biçimini belirtebilir ve sınıf, arama kodunun yeni bir çerçeve ne zaman kazanıldığında veya yeni bir analiz sonucu nun ne zaman kullanılabilebileceğini bildirmek için olayları kullanır.

Bazı olasılıkları göstermek için, kitaplığı kullanan iki örnek uygulama sağladık. 

İlk örnek uygulama, varsayılan web kamerasından çerçeveleri yakalayan ve daha sonra yüz algılama için Face hizmetine gönderen basit bir konsol uygulamasıdır. Uygulamanın basitleştirilmiş bir sürümü aşağıdaki kodda yeniden üretilir:

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

        private static async Task Main(string[] args)
        {
            // Create grabber.
            FrameGrabber<DetectedFace[]> grabber = new FrameGrabber<DetectedFace[]>();

            // Create Face Client.
            FaceClient faceClient = new FaceClient(new ApiKeyServiceClientCredentials(ApiKey))
            {
                Endpoint = Endpoint
            };

            // Set up a listener for when we acquire a new frame.
            grabber.NewFrameProvided += (s, e) =>
            {
                Console.WriteLine($"New frame acquired at {e.Frame.Metadata.Timestamp}");
            };

            // Set up a Face API call.
            grabber.AnalysisFunction = async frame =>
            {
                Console.WriteLine($"Submitting frame acquired at {frame.Metadata.Timestamp}");
                // Encode image and submit to Face service.
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

            // Tell grabber when to call the API.
            // See also TriggerAnalysisOnPredicate
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running in the background.
            await grabber.StartProcessingCameraAsync();

            // Wait for key press to stop.
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();

            // Stop, blocking until done.
            await grabber.StopProcessingAsync();
        }
    }
}
```

İkinci örnek uygulama biraz daha ilginç. Video karelerinde hangi API'yi çağırabileceğinizi seçmenize olanak tanır. Sol tarafta, uygulama canlı videonun önizlemesini gösterir. Sağda, ilgili çerçevedeki en son API sonucunu yerle bir eder.

Çoğu modda, soldaki canlı video ile sağdaki görselleştirilmiş analiz arasında gözle görülür bir gecikme vardır. Bu gecikme, API araması yapmak için gereken süredir. Bir istisna, herhangi bir görüntüyü Azure Bilişsel Hizmetler'e göndermeden önce OpenCV'yi kullanarak istemci bilgisayarda yerel olarak yüz algılama gerçekleştiren "EmotionsWithClientFaceDetect" modundadır. 

Bu yaklaşımı kullanarak, algılanan yüzü hemen görselleştirebilirsiniz. Daha sonra, API araması döndükten sonra duyguları güncelleştirebilirsiniz. Bu bir "melez" yaklaşım olasılığını göstermektedir. Diğer bir tarihte, istemci üzerinde bazı basit işlemler gerçekleştirilebilir ve daha sonra Bilişsel Hizmetler API'leri gerektiğinde bu işlemi daha gelişmiş analizlerle genişletmek için kullanılabilir.

![Etiketlerle görüntü görüntüleyen LiveCameraSample uygulaması](../../Video/Images/FramebyFrame.jpg)

### <a name="integrate-the-samples-into-your-codebase"></a>Örnekleri kod tabanınıza entegre edin

Bu örnekle başlamak için aşağıdakileri yapın:

1. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/)’den Görüntü İşleme API’leri için API anahtarlarını alın. Video kare analizi için geçerli hizmetler şunlardır:
    - [BilgisayarLı Vizyon](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. [Bilişsel-Örnekler-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub repo klonlamak.

3. Örneği Visual Studio 2015 veya sonraki bir tarihte açın ve örnek uygulamaları oluşturun ve çalıştırın:
    - BasicConsoleSample için, Yüz tuşu doğrudan [BasicConsoleSample/Program.cs'de](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs)sabit kodlanır.
    - LiveCameraSample için, tuşları uygulamanın **Ayarlar** bölmesine girin. Anahtarlar, kullanıcı verileri olarak oturumlar arasında kalıcıdır.

Örnekleri tümleştirmeye hazır olduğunuzda, kendi projelerinizden VideoFrameAnalyzer kitaplığına başvurun.

VideoFrameAnalyzer'ın görüntü, ses, video ve metin anlama özellikleri Azure Bilişsel Hizmetleri'ni kullanır. Microsoft yüklediğiniz görüntüleri, sesi, videoyu ve diğer verileri (bu uygulama aracılığıyla) alır ve bunları hizmet geliştirme amacıyla kullanabilir. Uygulamanızın verilerini Azure Bilişsel Hizmetler’e gönderdiği kişilerin korunması için yardımınızı rica ediyoruz.

## <a name="summary"></a>Özet

Bu makalede, Yüz ve Bilgisayar Görme hizmetlerini kullanarak canlı video akışlarında gerçek zamanlı analize yakın çalıştırmayı öğrendiniz. Ayrıca, başlamak için örnek kodumuzu nasıl kullanabileceğinizi de öğrendiniz. Ücretsiz API tuşlarını kullanarak uygulamanızı oluşturmaya başlamak için [Azure Bilişsel Hizmetler kayıt sayfasına](https://azure.microsoft.com/try/cognitive-services/)gidin.

[GitHub deposunda](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/)geri bildirim ve öneriler sunmakta çekinmeyin. Daha geniş API geri bildirimi sağlamak için [UserVoice sitemize](https://cognitive.uservoice.com/)gidin.

