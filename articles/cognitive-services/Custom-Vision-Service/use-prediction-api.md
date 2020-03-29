---
title: Görüntüleri sınıflandırıcı ile programlı olarak test etmek için tahmin bitiş noktasını kullanma - Özel Görme
titleSuffix: Azure Cognitive Services
description: Özel Görüntü İşleme sınıflandırıcınızla programlama yoluyla görüntüleri test etmek için API’nin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: anroth
ms.openlocfilehash: dcb12da680d70e1f0ce4cd763bee340bb3416c6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76169943"
---
# <a name="use-your-model-with-the-prediction-api"></a>Tahmin API'si ile modelinizi kullanın

Modelinizi eğitdikten sonra, görüntüleri Tahmin API bitiş noktasına göndererek programlı olarak sınayabilirsiniz.

> [!NOTE]
> Bu belgede, Tahmin API’sine görüntü göndermek için C# kullanımı gösterilmektedir. Daha fazla bilgi ve örnekler için [Tahmin API başvurusuna](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)bakın.

## <a name="publish-your-trained-iteration"></a>Eğitimli yinelemenizi yayımlayın

[Özel Görüntü İşleme web sayfasından](https://customvision.ai) projenizi ve __Performans__ sekmesini seçin.

Görüntüleri Tahmin API'sine göndermek için, önce __yayımla'yı__ seçerek ve yayımlanan yineleme için bir ad belirterek yapılabilecek tahmin yinelemenizi yayımlamanız gerekir. Bu, modelinizin Özel Vizyon Azure kaynağınızın Tahmin API'si için erişilebilir olmasını sağlar.

![Performans sekmesi, Yayımla düğmesini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/unpublished-iteration.png)

Modeliniz başarıyla yayımlandıktan sonra, sol kenar çubuğunda yinelemenizin yanında bir "Yayımlanmış" etiketi görünür ve adı yinelemenin açıklamasında görünür.

![Performans sekmesi, Yayımlanmış etiketi ni ve yayımlanmış yinelemenin adını çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

Modeliniz yayınlandıktan sonra, __Tahmin URL'sini__seçerek gerekli bilgileri alabilirsiniz. Bu, Tahmin URL'si ve __Tahmin__ __Anahtarı__da dahil olmak üzere Tahmin API'sını kullanmak için bilgi içeren bir iletişim kutusu açar.

![Performans sekmesi, Tahmin URL düğmesini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Performans sekmesi, bir resim dosyasını ve Tahmin Anahtarı değerini kullanmak için Tahmin URL değerini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/prediction-api-info.png)


Bu kılavuzda, yerel bir resim kullanacaksınız, bu nedenle **bir resim dosyanız varsa** URL'yi geçici bir konuma kopyalayın. İlgili __Tahmin-Anahtar__ değerini de kopyalayın.

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio'da yeni bir C# konsol uygulaması oluşturun.

1. __Program.cs__ dosyasının gövdesi olarak aşağıdaki kodu kullanın.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "<Your prediction key>");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "<Your prediction URL>";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

1. Aşağıdaki bilgileri değiştirin:
   * `namespace` Alanı projenizin adına ayarlayın.
   * Yer tutucuyu `<Your prediction key>` daha önce aldığınız anahtar değeriyle değiştirin.
   * Yer tutucuyu `<Your prediction URL>` daha önce aldığınız URL ile değiştirin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırdığınızda, konsoldaki bir resim dosyasına bir yol girmeniz istenir. Görüntü daha sonra Tahmin API'sine gönderilir ve tahmin sonuçları JSON biçimlendirilmiş dize olarak döndürülür. Aşağıda örnek bir yanıt verilmiştir.

```json
{
    "Id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "Project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "Iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "Created":"2019-03-20T16:47:31.322Z",
    "Predictions":[
        {"TagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","TagName":"cat", "Probability":1.0},
        {"TagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","TagName":"dog", "Probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, görüntüleri özel görüntü sınıflandırıcınıza/dedektörünüze göndermeyi ve C# SDK ile programlı bir yanıt almayı öğrendiniz. Ardından, C# ile uçlardan uca senaryoları nasıl tamamlayarak başka bir dil sdk kullanmaya nasıl başlanın.

* [Hızlı başlangıç: .NET SDK](csharp-tutorial.md)
* [Hızlı başlangıç: Python SDK](python-tutorial.md)
* [Quickstart: Java SDK](java-tutorial.md)
* [Quickstart: Düğüm SDK](node-tutorial.md)
* [Quickstart: SDK git](go-tutorial.md)
