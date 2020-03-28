---
title: 'Örnek: Gerçek zamanlı video analizi - Yüz'
titleSuffix: Azure Cognitive Services
description: Canlı video akışından alınan karelerde neredeyse gerçek zamanlı çözümleme gerçekleştirmek için Yüz hizmetini kullanın.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: ab3f596000216e8555bb84d0d47aff9a6e969eeb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169893"
---
# <a name="example-how-to-analyze-videos-in-real-time"></a>Örnek: Gerçek Zamanlı Videoları Analiz Etme

Bu kılavuzda, canlı video akışından alınan karelerde nasıl gerçek zamanlıya yakın analiz gerçekleştirileceği gösterilmektedir. Böyle bir sistemdeki temel bileşenler şunlardır:

- Video kaynağından kareleri alma
- Hangi karelerin analiz edileceğini seçme
- Bu kareleri API’ye gönderme
- API çağrısından döndürülen her analiz sonucunu kullanma

Bu örnekler C# ile yazılır ve kod GitHub'da bulunabilir: [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>Yaklaşım

Video akışlarında gerçek zamanlıya yakın analiz çalıştırma sorununu çözmenin birçok yolu vardır. Gelişmişlik düzeyini artırma konusunda üç yaklaşımı açıklayarak başlayacağız.

### <a name="a-simple-approach"></a>Basit Bir Yaklaşım

Neredeyse gerçek zamanlı analiz sistemi için en basit tasarım, her yinelemenin bir çerçeveyi kaptığı, analiz ettiği ve sonucu tükettiği sonsuz bir döngüdür:

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

Analizimiz hafif bir istemci algoritmasından oluştuysa bu yaklaşım uygun olacaktır. Ancak, çözümleme bulutta gerçekleştiğinde, ilgili gecikme, bir API çağrısının birkaç saniye sürebileceği anlamına gelir. Bu süre zarfında, biz görüntüleri yakalamak değildir ve bizim iş parçacığı aslında hiçbir şey yapıyor. Maksimum kare hızımız, API çağrılarının gecikme süresiyle sınırlıdır.

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

Bu kod her bir analizi ayrı bir Görevde başlatır ve biz yeni kareleri ele geçirirken görev arka planda çalıştırılabilir. Bu yöntemle, bir API çağrısının geri dönmesini beklerken ana iş parçacığının engellenmesini önleriz, ancak basit sürümün sağladığı garantilerin bazılarını kaybettik. Birden çok API çağrısı paralel olarak oluşabilir ve sonuçlar yanlış sırada döndürülebilir. Bu yaklaşım, birden çok iş parçacığının eş zamanlı olarak ConsumeResult() işlevi girmesine neden olabilir; işlev iş parçacığı açısından güvenli değilse bu tehlikeli olabilir. Son olarak bu basit kod, oluşturulan Görevleri takip etmez, bu nedenle özel durumlar sessizce kaybolur. Bu nedenle, son adım, analiz görevlerini izleyecek, özel durumları yükseltecek, uzun süren görevleri öldürecek ve sonuçların doğru sırada tüketilmesini sağlayacak bir "tüketici" iş parçacığı eklemektir.

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

Ayrıca görevleri sıradan çıkaran, bitirmelerini bekleyen ve sonucu görüntüleyen veya atılan özel durumu yükselten bir tüketici iş parçacığımız da vardır. Kuyruğu kullanarak, sistemin maksimum kare hızını sınırlamadan sonuçların birer birer doğru sırayla kullanılmasını garanti edebiliriz.

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

Uygulamanızı mümkün olan en kısa sürede çalışır hale getirmek için, yukarıda açıklanan sistemin esnek bir uygulamasını kullanırsınız. Koda erişmek için. [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis)

Kitaplık, bir web kamerasından gelen video karelerini işlemek için yukarıda açıklanan üretici-tüketici sistemini uygulayan FrameGrabber sınıfını içerir. Kullanıcı API çağrısının tam biçimini belirtebilir ve sınıf, arama kodunun yeni bir çerçeve kazanıldığında veya yeni bir analiz sonucu nun kullanılabilir olduğunda bildirin için olayları kullanır.

Bazı olasılıkları göstermek için, kitaplığı kullanan iki örnek uygulama vardır. İlk basit bir konsol uygulaması, ve basitleştirilmiş bir sürümü aşağıda çoğaltılır. Varsayılan web kamerasından kareler alır ve yüz algılama için Yüz hizmetine gönderir.

```csharp
using System;
using VideoFrameAnalyzer;
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;
     
namespace VideoFrameConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create grabber, with analysis type Face[]. 
            FrameGrabber<Face[]> grabber = new FrameGrabber<Face[]>();
            
            // Create Face Client. Insert your Face API key here.
            private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials("<subscription key>"),
            new System.Net.Http.DelegatingHandler[] { });

            // Set up our Face API call.
            grabber.AnalysisFunction = async frame => return await faceClient.DetectAsync(frame.Image.ToMemoryStream(".jpg"));

            // Set up a listener for when we receive a new result from an API call. 
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.Analysis != null)
                    Console.WriteLine("New result received for frame acquired at {0}. {1} faces detected", e.Frame.Metadata.Timestamp, e.Analysis.Length);
            };
            
            // Tell grabber to call the Face API every 3 seconds.
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for keypress to stop
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();
            
            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```

İkinci örnek uygulama biraz daha ilginçtir ve video karelerinde hangi API’yi çağıracağınızı seçmenize olanak sağlar. Sol kısımda uygulama, canlı videonun bir önizlemesini gösterir, sağ kısımdaysa ilgili karenin üzerine yerleştirilmiş şekilde en son API sonucunu gösterir.

Çoğu modda, soldaki canlı video ile sağdaki görselleştirilmiş analiz arasında görünür bir gecikme olacaktır. Bu gecikme, API çağrısı yapmak için geçen süredir. Bir istisna bilişsel hizmetler için herhangi bir görüntü göndermeden önce, OpenCV kullanarak istemci bilgisayarda yerel olarak yüz algılama gerçekleştirir "EmotionsWithClientFaceDetect" modudur. Bu şekilde, algılanan yüzü hemen görselleştirebilir ve API çağrısı döndükten sonra duyguları güncelleyebiliriz. Bu, istemcinin bazı basit işlemleri gerçekleştirebileceği bir "karma" yaklaşım örneğidir ve Bilişsel Hizmetler API'leri gerektiğinde bunu daha gelişmiş analizlerle güçlendirebilir.

![Video analiz etme](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Kod temelinizle tümleştirme

Bu örneği kullanmaya başlamak için şu adımları izleyin:

1. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/)’den Görüntü İşleme API’leri için API anahtarlarını alın. Video karesi analizi için geçerli API’ler şunlardır:
    - [Bilgisayarlı Vizyon API'si](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Yüz API'si](https://docs.microsoft.com/azure/cognitive-services/face/overview)

2. [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub deposunu kopyalayın

3. Örneği Visual Studio 2015'te açın ve örnek uygulamaları oluşturun ve çalıştırın:
    - BasicConsoleSample için, Yüz tuşu doğrudan [BasicConsoleSample/Program.cs'de](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs)sabit kodlanır.
    - LiveCameraSample için anahtarlar, uygulamanın Ayarlar bölmesine girilmelidir. Oturumlarda kullanıcı verileri olarak kalıcı duruma getirilir.
        

Tümleştirmeye hazır olduğunuzda, **kendi projelerinizden VideoFrameAnalyzer kitaplığına başvurun.** 

## <a name="summary"></a>Özet

Bu kılavuzda, Canlı video akışlarında Yüz, Bilgisayar Görüşü ve Duygu API'lerini kullanarak neredeyse gerçek zamanlı analiz yapmayı ve başlamak için örnek kodumuzu nasıl kullanacağınızı öğrendiniz. [Azure Bilişsel Hizmetler kayıt sayfasında](https://azure.microsoft.com/try/cognitive-services/)ücretsiz API anahtarları ile uygulamanızı oluşturmaya başlayabilirsiniz. 

[GitHub deposunda](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) veya daha geniş API geri bildirimi için [UserVoice sitemizde](https://cognitive.uservoice.com/)geri bildirim ve öneriler sunmakta çekinmeyin.

## <a name="related-topics"></a>İlgili Konular
- [Görüntüdeki Yüzleri Belirleme](HowtoIdentifyFacesinImage.md)
- [Görüntüdeki Yüzleri Algılama](HowtoDetectFacesinImage.md)
