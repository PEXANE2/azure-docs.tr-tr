---
title: 'Hızlı Başlangıç: Geri bildirim döngüsü oluşturma-kişiselleştirici'
titleSuffix: Azure Cognitive Services
description: Bu C# hızlı başlangıçta Içeriği kişiselleştirici hizmetiyle kişiselleştirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 06/11/2019
ms.author: diberry
ms.openlocfilehash: 54aa23071fef09058a1702218d6b7fc920363518
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662807"
---
# <a name="quickstart-personalize-content-using-c"></a>Hızlı Başlangıç: Kullanarak içeriği kişiselleştirmeC# 

Bu C# hızlı başlangıçta kişiselleştirilmiş Içeriği kişiselleştirici hizmeti ile görüntüleyin.

Bu örnek, için C# kişiselleştirici istemci kitaplığının aşağıdaki işlemleri gerçekleştirmek üzere nasıl kullanılacağını gösterir: 

 * Kişiselleştirmeye yönelik eylemlerin listesini sıralama.
 * Rapor, belirtilen olay için Kullanıcı seçimine dayalı olarak en üst dereceli eyleme ayırmayı yeniden sağlar.

Kişiselleştiriciye Başlarken aşağıdaki adımları içerir:

1. SDK 'ya başvurma 
1. Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod yazma,
1. Döngüyü eğitme için kod yazma.

## <a name="prerequisites"></a>Önkoşullar

* Abonelik anahtarınızı ve uç nokta hizmeti URL 'nizi almak için bir [kişiselleştirici hizmeti](how-to-settings.md) gereklidir. 
* [Visual Studio 2015 veya 2017](https://visualstudio.microsoft.com/downloads/).
* [Microsoft. Azure. Biliveservices. kişiselleştirici](https://go.microsoft.com/fwlink/?linkid=2092272) SDK NuGet paketi. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="change-the-model-update-frequency"></a>Model güncelleştirme sıklığını değiştirme

Azure portal kişiselleştirici kaynağında, **model güncelleştirme sıklığını** 10 saniye olarak değiştirin. Bu, hizmeti hızlı bir şekilde eğitecektir ve her yineleme için en iyi eylem değişikliğini görmenizi sağlar.

Bir kişiselleştirici döngüsü ilk kez oluşturulduğunda, üzerinden eğitelenecek bir API çağrısı olmadığından model yoktur. Sıralama çağrıları her öğe için eşit olasılıklara dönüşe sahip olur. Uygulamanız hala Rewarterctionıd çıkışını kullanarak içeriği her zaman derecelendirmelidir.

![Model güncelleştirme sıklığını Değiştir](./media/settings/configure-model-update-frequency-settings.png)

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Yeni bir konsol uygulaması oluşturma ve kişiselleştirici SDK 'ya başvurma 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Visual Studio'da yeni bir Visual C# Konsol Uygulaması oluşturun.
1. Kişiselleştirici istemci kitaplığı NuGet paketini yükler. Menüsünde **Araçlar**' ı seçin, **NuGet Paket Yöneticisi**' ni seçin ve ardından **çözüm için NuGet paketlerini yönetin**.
1. **Ön sürümü dahil**et 'i işaretleyin.
1. Araştır sekmesini **seçin** ve **arama** kutusuna yazın `Microsoft.Azure.CognitiveServices.Personalizer`.
1. Görüntülediğinde **Microsoft. Azure. Biliveservices. kişiselleştirici** ' ı seçin.
1. Proje adınızın yanındaki onay kutusunu işaretleyin ve ardından **Install**' ı seçin.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Kodu ekleyin ve kişiselleştirici ve Azure Anahtarlarınıza yerleştirin

1. Program.cs içeriğini şu kodla değiştirin. 
1. Değeri `serviceKey` geçerli kişiselleştirici abonelik anahtarınızla değiştirin.
1. Hizmet `serviceEndpoint` uç noktanızla değiştirin. `https://westus2.api.cognitive.microsoft.com/` bunun bir örneğidir.
1. Programı çalıştırın.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Kullanıcılarınıza göstermek istediğiniz eylemleri derecelendirmek için kod ekleyin

Aşağıdaki C# kod, SDK 'Yı kullanarak kişiselleştiriciye Kullanıcı bilgilerini, _özellikleri ve İçerikleriniz hakkındaki bilgileri geçirmek için kapsamlıbir listedir. Kişiselleştirici, Kullanıcı göstermek için en üstteki dereceli eylemi döndürür.  

```csharp
using Microsoft.Azure.CognitiveServices.Personalizer;
using Microsoft.Azure.CognitiveServices.Personalizer.Models;
using System;
using System.Collections.Generic;
using System.Linq;

namespace PersonalizerExample
{
    class Program
    {
        // The key specific to your personalizer service instance; e.g. "0123456789abcdef0123456789ABCDEF"
        private const string ApiKey = "";

        // The endpoint specific to your personalizer service instance; e.g. https://westus2.api.cognitive.microsoft.com/
        private const string ServiceEndpoint = "";

        static void Main(string[] args)
        {
            int iteration = 1;
            bool runLoop = true;

            // Get the actions list to choose from personalizer with their features.
            IList<RankableAction> actions = GetActions();

            // Initialize Personalizer client.
            PersonalizerClient client = InitializePersonalizerClient(ServiceEndpoint);

            do
            {
                Console.WriteLine("\nIteration: " + iteration++);

                // Get context information from the user.
                string timeOfDayFeature = GetUsersTimeOfDay();
                string tasteFeature = GetUsersTastePreference();

                // Create current context from user specified data.
                IList<object> currentContext = new List<object>() {
                    new { time = timeOfDayFeature },
                    new { taste = tasteFeature }
                };

                // Exclude an action for personalizer ranking. This action will be held at its current position.
                IList<string> excludeActions = new List<string> { "juice" };

                // Generate an ID to associate with the request.
                string eventId = Guid.NewGuid().ToString();

                // Rank the actions
                var request = new RankRequest(actions, currentContext, excludeActions, eventId);
                RankResponse response = client.Rank(request);

                Console.WriteLine("\nPersonalizer service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

                float reward = 0.0f;
                string answer = GetKey();

                if (answer == "Y")
                {
                    reward = 1;
                    Console.WriteLine("\nGreat! Enjoy your food.");
                }
                else if (answer == "N")
                {
                    reward = 0;
                    Console.WriteLine("\nYou didn't like the recommended food choice.");
                }
                else
                {
                    Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended food choice.");
                }

                Console.WriteLine("\nPersonalizer service ranked the actions with the probabilities as below:");
                foreach (var rankedResponse in response.Ranking)
                {
                    Console.WriteLine(rankedResponse.Id + " " + rankedResponse.Probability);
                }

                // Send the reward for the action based on user response.
                client.Reward(response.EventId, new RewardRequest(reward));

                Console.WriteLine("\nPress q to break, any other key to continue:");
                runLoop = !(GetKey() == "Q");

            } while (runLoop);
        }

        /// <summary>
        /// Initializes the personalizer client.
        /// </summary>
        /// <param name="url">Azure endpoint</param>
        /// <returns>Personalizer client instance</returns>
        static PersonalizerClient InitializePersonalizerClient(string url)
        {
            PersonalizerClient client = new PersonalizerClient(
                new ApiKeyServiceClientCredentials(ApiKey)) {Endpoint = url};

            return client;
        }

        /// <summary>
        /// Get users time of the day context.
        /// </summary>
        /// <returns>Time of day feature selected by the user.</returns>
        static string GetUsersTimeOfDay()
        {
            string[] timeOfDayFeatures = new string[] { "morning", "afternoon", "evening", "night" };

            Console.WriteLine("\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night");
            if (!int.TryParse(GetKey(), out int timeIndex) || timeIndex < 1 || timeIndex > timeOfDayFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + timeOfDayFeatures[0] + ".");
                timeIndex = 1;
            }

            return timeOfDayFeatures[timeIndex - 1];
        }

        /// <summary>
        /// Gets user food preference.
        /// </summary>
        /// <returns>Food taste feature selected by the user.</returns>
        static string GetUsersTastePreference()
        {
            string[] tasteFeatures = new string[] { "salty", "sweet" };

            Console.WriteLine("\nWhat type of food would you prefer (enter number)? 1. salty 2. sweet");
            if (!int.TryParse(GetKey(), out int tasteIndex) || tasteIndex < 1 || tasteIndex > tasteFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + tasteFeatures[0] + ".");
                tasteIndex = 1;
            }

            return tasteFeatures[tasteIndex - 1];
        }

        /// <summary>
        /// Creates personalizer actions feature list.
        /// </summary>
        /// <returns>List of actions for personalizer.</returns>
        static IList<RankableAction> GetActions()
        {
            IList<RankableAction> actions = new List<RankableAction>
            {
                new RankableAction
                {
                    Id = "pasta",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "medium" }, new { nutritionLevel = 5, cuisine = "italian" } }
                },

                new RankableAction
                {
                    Id = "ice cream",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionalLevel = 2 } }
                },

                new RankableAction
                {
                    Id = "juice",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionLevel = 5 }, new { drink = true } }
                },

                new RankableAction
                {
                    Id = "salad",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "low" }, new { nutritionLevel = 8 } }
                }
            };

            return actions;
        }

        private static string GetKey()
        {
            return Console.ReadKey().Key.ToString().Last().ToString().ToUpper();
        }
    }
}
```

## <a name="run-the-program"></a>Programı çalıştırma

Programı derleyin ve çalıştırın. Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

[Kişiselleştirici nasıl kullanılır?](how-personalizer-works.md)


