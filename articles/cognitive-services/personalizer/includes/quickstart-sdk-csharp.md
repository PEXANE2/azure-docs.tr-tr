---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 07/30/2020
ms.openlocfilehash: 41665193ab78f4d29785171ef3fee9a7ce1d9cea
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246055"
---
[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="using-this-quickstart"></a>Bu hızlı başlangıcı kullanma

Bu hızlı başlangıcı kullanmanın birkaç adımı vardır:

* Azure portal, bir kişiselleştirici kaynağı oluşturun
* Azure portal, kişiselleştirici kaynağı için **yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralığa göre değiştirin
* Kod düzenleyicisinde bir kod dosyası oluşturun ve kod dosyasını düzenleyin
* Komut satırı veya terminalinde, komut satırından SDK 'Yı yükler
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `new` ad ile yeni bir konsol uygulaması oluşturmak için DotNet komutunu kullanın `personalizer-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: `Program.cs` .

```console
dotnet new console -n personalizer-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>SDK Yükleme

Uygulama dizini içinde, aşağıdaki komutla .NET için kişiselleştirici istemci Kitaplığı ' nı yükleyeceksiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Kişiselleştirici istemci, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan bir [Personizerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) nesnesidir.

İçeriğin tek bir en iyi öğesi için sormak üzere bir [Rankrequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)oluşturun ve ardından [istemciye geçirin. Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview) yöntemi. Rank yöntemi bir RankResponse döndürür.

Kişiselleştiriciye bir ödül puanı göndermek için bir [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview)oluşturun ve ardından [istemciye geçirin. Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview) yöntemi.

Yeniden temellendirme belirlenmesi, bu hızlı başlangıçta önemsiz. Bir üretim sisteminde, İleri [puanı](../concept-rewards.md) neyin etkilediğini ve ne kadar karmaşık bir süreç olabileceğini belirleme, zaman içinde değiştirmeye karar verebilirsiniz. Bu tasarım kararı, kişiselleştirici mimarinizdeki birincil kararlardan biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için kişiselleştirici istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [Bir kişiselleştirici istemci oluşturma](#create-a-personalizer-client)
* [Derecelendirme API 'SI](#request-the-best-action)
* [Reward API 'SI](#send-a-reward)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, **program.cs** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Mevcut `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Kişiselleştirici kaynak bilgileri ekleme

**Program** sınıfında, kaynak Azure anahtarı ve uç noktası için kod dosyasının en üstüne doğru olan anahtar ve uç nokta değişkenlerini düzenleyin. 

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Bir kişiselleştirici istemci oluşturma

Sonra, bir kişiselleştirici istemci döndürmek için bir yöntem oluşturun. Yöntemine parametresi, `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey ' dir `PERSONALIZER_RESOURCE_KEY` .

[!code-csharp[Create the Personalizer client](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Yiyecek öğelerini aralıklı eylemler olarak al

Eylemler, Kişiselleştiriciye en iyi içerik öğesini seçmesini istediğiniz içerik seçimlerini temsil eder. Eylemler kümesini ve bunların özelliklerini temsil etmek için program sınıfına aşağıdaki yöntemleri ekleyin. 

[!code-csharp[Food items as actions](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Bağlam için Kullanıcı tercihlerini al

Komut satırından günün saati ve geçerli yiyecek tercihi için bir kullanıcının girişini almak üzere program sınıfına aşağıdaki yöntemleri ekleyin. Bunlar, bağlam özellikleri olarak kullanılacaktır.

[!code-csharp[Present time out day preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=createUserFeatureTastePreference)]

Her iki yöntem de, `GetKey` komut satırından kullanıcının seçimini okumak için yöntemini kullanır.

[!code-csharp[Read user's choice from the command line](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsünü oluşturma

Kişiselleştirici öğrenme döngüsü, bir [derece](#request-the-best-action) ve [yeniden](#send-a-reward) arama çağrısı döngüsüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her bir derecelendirme çağrısının ardından, hizmetin ne kadar iyi sonuç vereceğini söylemek için bir Reward çağrısı vardır.

Aşağıdaki kod, kullanıcıya komut satırında tercihlerini sorma, en iyi eylemi seçmek için bu bilgileri Kişiselleştiriciye göndermek üzere bu bilgileri kişisel olarak, liste arasından seçim yapmak üzere müşteriye sunan ve sonra da hizmetin seçimine ne kadar iyi olduğunu işaret eden bir ödül puanı gönderen bir döngüyle geçer.

[!code-csharp[Learning loop](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=mainLoop)]

Kod dosyasını çalıştırmadan önce [içerik seçimlerini almak](#get-food-items-as-rankable-actions)için aşağıdaki yöntemleri ekleyin:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-the-best-action"></a>En iyi eylemi isteyin

Sıralama isteğini gerçekleştirmek için, program kullanıcının tercihlerini `currentContent` içerik seçimlerinden birini oluşturacak şekilde sorar. İşlem, eylemlerden hariç tutulacak içerik oluşturabilir `excludeActions` . Yanıtları almak için sıralama isteğinin eylemleri ve özellikleri, currentContext özellikleri, excludeActions ve benzersiz bir olay KIMLIĞI olması gerekir.

Bu hızlı başlangıçta, günün saati ve Kullanıcı yiyecek tercihi basit bağlam özelliklerine sahiptir. Üretim sistemlerinde, [eylemleri ve özellikleri](../concepts-features.md) belirlemek ve [değerlendirmek](../concept-feature-evaluation.md) önemsiz olmayan bir şekilde olabilir.

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Bir ödül gönderin

Yeniden işleme isteği gönderilmesi için, program komut satırından kullanıcının seçimini alır, seçime bir sayısal değer atar, sonra benzersiz olay kimliği ve ödül Puanını sayısal değer olarak ödül API 'sine gönderir.

Bu hızlı başlangıç, bir sıfır veya 1 ya da bir yeniden puan olarak basit bir sayı atar. Üretim [sistemlerinde, bu çağrıya ne](../concept-rewards.md) zaman ve ne gönderileceğini belirlemek, özel gereksinimlerinize bağlı olarak önemsiz olmayan bir önemi olabilir.

[!code-csharp[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/dotnet/Personalizer/Program.cs?name=reward)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulama dizininizde DotNet komutuyla uygulamayı çalıştırın `run` .

```console
dotnet run
```

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

[Bu hızlı başlangıç için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/Personalizer) kullanılabilir.
