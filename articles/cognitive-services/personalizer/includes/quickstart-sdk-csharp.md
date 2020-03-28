---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: 7fd63841cabd91d46dd311f571fd100bbcfdd0fe
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122890"
---
[Referans belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core)geçerli sürümü.

## <a name="using-this-quickstart"></a>Bu hızlı başlatmayı kullanma

Bu hızlı başlatmayı kullanmak için birkaç adım vardır:

* Azure portalında bir Kisizer kaynağı oluşturun
* Azure portalında, Kiliker kaynağı için **Yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralıkla değiştirin
* Kod düzenleyicisinde, bir kod dosyası oluşturun ve kod dosyasını
* Komut satırına veya terminale, SDK komut satırından
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni bir .NET Core uygulaması oluşturun.

Konsol penceresinde (cmd, PowerShell veya Bash gibi), adında `new` `personalizer-quickstart`yeni bir konsol uygulaması oluşturmak için dotnet komutunu kullanın. Bu komut, tek bir kaynak dosyası ile basit bir `Program.cs`"Hello World" C# projesi oluşturur: .

```console
dotnet new console -n personalizer-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

## <a name="install-the-sdk"></a>SDK yükle

Uygulama dizininde ,.NET için Personalizer istemci kitaplığını aşağıdaki komutla yükleyin:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Personalizer istemcisi, anahtarınızı içeren Microsoft.Rest.ServiceClient Credentials'ı kullanarak Azure'a doğrulayan bir [PersonalizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) nesnesidir.

İçeriğin en iyi tek öğesini istemek için bir [RankRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)oluşturun ve [ardından istemciye geçirin. Sıralama](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview) yöntemi. Rank yöntemi bir RankResponse döndürür.

Personalizer'a ödül puanı göndermek için bir [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview)oluşturun ve [ardından müşteriye aktarın. Ödül](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview) yöntemi.

Ödül puanını belirlemek, bu hızlı başlangıçta önemsizdir. Bir üretim sisteminde, [ödül puanını](../concept-rewards.md) neyin etkilediğinin ve ne kadarının karmaşık bir süreç olabileceğinin belirlenmesi, zaman içinde değiştirmeye karar verebilirsiniz. Bu tasarım kararı, Personalizer mimarinizdeki birincil kararlardan biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için Personalizer istemci kitaplığı ile aşağıdaki görevleri nasıl yapacağınızı gösterir:

* [Bir Personalizer istemcisi oluşturma](#create-a-personalizer-client)
* [Sıralama API'si](#request-the-best-action)
* [Ödül API'si](#send-a-reward)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki **Program.cs** dosyasını açın. Varolan `using` kodu aşağıdaki `using` yönergelerle değiştirin:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Personalizer kaynak bilgilerini ekleme

**Program** sınıfında, kaynağınızın Azure tuşu ve bitiş noktası için, adlı `PERSONALIZER_RESOURCE_KEY` ve . `PERSONALIZER_RESOURCE_ENDPOINT` Uygulama başlatıldıktan sonra ortam değişkenlerini oluşturduysanız, düzenleyici, IDE veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra bu hızlı başlatmada oluşturulur.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Bir Personalizer istemcisi oluşturma

Ardından, Bir Personalizer istemcisi döndürmek için bir yöntem oluşturun. Yöntemin parametresi `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey olduğunu `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Gıda maddelerini sıralanabilir eylemler olarak alın

Eylemler, Personalizer'ın en iyi içerik öğesini seçmesini istediğiniz içerik seçeneklerini temsil ediyor. Eylem kümesini ve özelliklerini temsil etmek için Program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-csharp[Food items as actions](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Bağlam için kullanıcı tercihlerini alma

Günün saati ve geçerli gıda tercihi için komut satırından kullanıcının girişini almak için Program sınıfına aşağıdaki yöntemleri ekleyin. Bunlar bağlam özellikleri olarak kullanılacaktır.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Her iki `GetKey` yöntem de komut satırından kullanıcının seçimini okumak için yöntemi kullanır.

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsü oluşturma

Personalizer öğrenme döngüsü [Rank](#request-the-best-action) ve [Reward](#send-a-reward) çağrıları bir döngüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için yapılan her Rütbe çağrısını, Personalizer'a hizmetin ne kadar iyi performans gösterdiğini söylemek için bir Ödül çağrısı takip eder.

Aşağıdaki kod, kullanıcıya komut satırında tercihlerini sorma, bu bilgileri en iyi eylemi seçmesi için Personalizer'a gönderme, liste arasından seçim yapmak üzere müşteriye seçimi sunma ve ardından bir ödül puanı gönderme döngüsü boyunca döngüler Personalizer, hizmetin seçiminde ne kadar iyi olduğunu işaret ediyor.

[!code-csharp[Learning loop](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=mainLoop)]

Kod [dosyasını çalıştırmadan](#get-food-items-as-rankable-actions)önce içerik seçeneklerini alan aşağıdaki yöntemleri ekleyin:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-the-best-action"></a>En iyi eylemi isteyin

Sıralama isteğini tamamlamak için program, kullanıcının tercihlerinden `currentContent` içerik seçeneklerinden birini oluşturmasını ister. İşlem, '. olarak `excludeActions`gösterilen eylemlerden hariç tutmak için içerik oluşturabilir. Yanıtı almak için Sıralama isteğinin eylemlere ve özelliklerine, geçerli Bağlam özelliklerine, dışlama Eylemlerine ve benzersiz bir olay kimliğine ihtiyacı vardır.

Bu hızlı başlatma, günün saati ve kullanıcı gıda tercihinin basit bağlam özelliklerine sahiptir. Üretim [sistemlerinde, eylemlerin](../concepts-features.md) ve özelliklerin belirlenmesi ve [değerlendirilmesi](../concept-feature-evaluation.md) önemsiz olmayan bir konu olabilir.

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Ödül gönder

Ödül isteğini göndermek için ödül puanını almak için program kullanıcının seçimini komut satırından alır, seçime sayısal bir değer atar, ardından benzersiz etkinlik kimliğini ve ödül puanını ödül API'sine sayısal değer olarak gönderir.

Bu hızlı başlangıç, ödül puanı olarak basit bir sayı atar, sıfır veya 1. Üretim sistemlerinde, [Ödül](../concept-rewards.md) çağrısına ne zaman ve ne zaman göndereceğiniz belirlenebilir, özel ihtiyaçlarınıza bağlı olarak önemsiz olmayan bir konu olabilir.

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulama dizininizdeki dotnet `run` komutu yla uygulamayı çalıştırın.

```console
dotnet run
```

![Hızlı başlangıç programı, özellikler olarak bilinen kullanıcı tercihlerini toplamak için birkaç soru sorar ve ardından en üst eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

[Bu hızlı başlatmanın kaynak kodu,](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) Personalizer örnekleri GitHub deposunda kullanılabilir.
