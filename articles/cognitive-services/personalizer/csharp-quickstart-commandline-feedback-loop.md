---
title: 'Hızlı başlangıç: .NET için kişiselleştirici istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, öğrenme döngüsü kullanılarak .NET için kişiselleştirici istemci kitaplığı ile çalışmaya başlama gösterilmektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: c17bf54d89e3a98ca667eeba40f2d2b166550833
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446382"
---
# <a name="quickstart-personalizer-client-library-for-net"></a>Hızlı başlangıç: .NET için kişiselleştirici istemci kitaplığı

Bu C# hızlı başlangıçta kişiselleştirilmiş Içeriği kişiselleştirici hizmeti ile görüntüleyin.

.NET için kişiselleştirici istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

 * Kişiselleştirmeye yönelik eylemlerin listesini sıralama.
 * En çok kullanılan derecelendirme eyleminin başarısını belirten rapor ödül.

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.Personalizer?view=azure-dotnet-preview) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Personalizer) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Personalizer/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.

## <a name="using-this-quickstart"></a>Bu hızlı başlangıcı kullanma

Bu hızlı başlangıcı kullanmanın birkaç adımı vardır:

* Azure portal, bir kişiselleştirici kaynağı oluşturun
* Azure portal, kişiselleştirici kaynağı için **yapılandırma** sayfasında, model güncelleştirme sıklığını değiştirin
* Kod düzenleyicisinde bir kod dosyası oluşturun ve kod dosyasını düzenleyin
* Komut satırı veya terminalinde, komut satırından SDK 'Yı yükler
* Komut satırında veya terminalde, kod dosyasını çalıştırın

## <a name="create-a-personalizer-azure-resource"></a>Bir kişiselleştirici Azure kaynağı oluşturma

Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak kişiselleştirici için bir kaynak oluşturun. Aşağıdakileri de yapabilirsiniz:

* [Deneme anahtarını](https://azure.microsoft.com/try/cognitive-services) ücretsiz olarak 7 gün boyunca geçerli olacak şekilde öğrenin. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)mevcut olacaktır.  
* [Azure Portal](https://portal.azure.com/)kaynağı görüntüleyin.

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra, iki [ortam değişkeni](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)oluşturun:

* Kaynak anahtarı için `PERSONALIZER_RESOURCE_KEY`.
* Kaynak uç noktası için `PERSONALIZER_RESOURCE_ENDPOINT`.

Azure portal, hem anahtar hem de uç nokta değerleri **hızlı başlangıç** sayfasından kullanılabilir.

## <a name="change-the-model-update-frequency"></a>Model güncelleştirme sıklığını değiştirme

Azure portal, **yapılandırma** sayfasındaki kişiselleştirici kaynağında, **model güncelleştirme sıklığını** 10 saniye olarak değiştirin. Bu kısa süre, hizmeti hızlı bir şekilde eğitecektir ve her yineleme için en iyi eylem değişikliğini görmenizi sağlar.

![Model güncelleştirme sıklığını Değiştir](./media/settings/configure-model-update-frequency-settings.png)

Bir kişiselleştirici döngüsü ilk kez oluşturulduğunda, üzerinden eğitelenecek bir API çağrısı olmadığından model yoktur. Sıralama çağrıları her öğe için eşit olasılıklara dönüşe sahip olur. Uygulamanız hala Rewarterctionıd çıkışını kullanarak içeriği her zaman derecelendirmelidir.

## <a name="create-a-new-c-application"></a>Yeni C# bir uygulama oluşturun

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `personalizer-quickstart`adında yeni bir konsol uygulaması oluşturmak için DotNet `new` komutunu kullanın. Bu komut, tek bir kaynak dosyası olan C# basit bir "Merhaba Dünya" projesi oluşturur: `Program.cs`. 

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

## <a name="install-the-sdk"></a>SDK yükle

Uygulama dizini içinde, aşağıdaki komutla .NET için kişiselleştirici istemci Kitaplığı ' nı yükleyeceksiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Personalizer --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Kişiselleştirici istemci, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan bir [Personizerclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclient?view=azure-dotnet) nesnesidir.

İçeriğin bir derecesini sormak için bir [Rankrequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rankrequest?view=azure-dotnet-preview)oluşturun ve ardından [istemciye geçirin. Rank](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.rank?view=azure-dotnet-preview) yöntemi. Rank yöntemi, derecelendirilen içeriği içeren bir RankResponse döndürür. 

Kişiselleştiriciye bir ödül göndermek için bir [RewardRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.models.rewardrequest?view=azure-dotnet-preview)oluşturun ve ardından [istemciye geçirin. Reward](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer.personalizerclientextensions.reward?view=azure-dotnet-preview) yöntemi. 

Bu hızlı başlangıçta önemsiz bir şekilde belirlenir. Bir üretim sisteminde, İleri [puanı](concept-rewards.md) neyin etkilediğini ve ne kadar karmaşık bir süreç olabileceğini belirleme, zaman içinde değiştirmeye karar verebilirsiniz. Bu tasarım kararı, kişiselleştirici mimarinizdeki birincil kararlardan biri olmalıdır. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için kişiselleştirici istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını gösterir:

* [Bir kişiselleştirici istemci oluşturma](#create-a-personalizer-client)
* [Bir derece iste](#request-a-rank)
* [Bir ödül gönderin](#send-a-reward)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, **program.cs** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Mevcut `using` kodunu aşağıdaki `using` yönergeleriyle değiştirin:

[!code-csharp[Using statements](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Kişiselleştirici kaynak bilgileri ekleme

**Program** sınıfında, kaynak Azure anahtarı ve `PERSONALIZER_RESOURCE_KEY` ve `PERSONALIZER_RESOURCE_ENDPOINT`adlı ortam değişkenlerinden çekilme uç noktası için değişkenler oluşturun. Uygulama başlatıldıktan sonra ortam değişkenlerini oluşturduysanız, değişkene erişmek için bu uygulamayı çalıştıran düzenleyici, IDE veya kabuğun kapatılıp yeniden yüklenmesi gerekir. Yöntemler daha sonra bu hızlı başlangıçta oluşturulacaktır.

[!code-csharp[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=classVariables)]

## <a name="create-a-personalizer-client"></a>Bir kişiselleştirici istemci oluşturma

Sonra, bir kişiselleştirici istemci döndürmek için bir yöntem oluşturun. Yöntemine yönelik parametre `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey `PERSONALIZER_RESOURCE_KEY`.

[!code-csharp[Create the Personalizer client](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=authorization)]

## <a name="get-food-items-as-rankable-actions"></a>Yiyecek öğelerini aralıklı eylemler olarak al

Eylemler, Kişiselleştiriciye göre derecelendirmek istediğiniz içerik seçimlerini temsil eder. Sıralama yapılacak eylem kümesini temsil etmek için program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-csharp[Food items as actions](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createAction)]

## <a name="get-user-preferences-for-context"></a>Bağlam için Kullanıcı tercihlerini al

Komut satırından günün saati ve geçerli yiyecek tercihi için bir kullanıcının girişini almak üzere program sınıfına aşağıdaki yöntemleri ekleyin. Bunlar, eylemleri derecelendirerek bağlam olarak kullanılacaktır.

[!code-csharp[Present time out day preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTimeOfDay)]

[!code-csharp[Present food taste preference to the user](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=createUserFeatureTastePreference)]

Her iki yöntem de, komut satırından kullanıcının seçimini okumak için `GetKey` yöntemini kullanır. 

[!code-csharp[Read user's choice from the command line](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=readCommandLine)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsünü oluşturma

Kişiselleştirici öğrenme döngüsü, bir derece ve yeniden arama çağrısı döngüsüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her bir derecelendirme çağrısı, kişiselleştirmeden hizmetin içeriği derecelendirerek ne kadar iyi bir şekilde bilgi verdiğini söyleyerek bir ödül çağrısıyla izlenir. 

Programın `main` yönteminde aşağıdaki kod, kullanıcıya komut satırında tercihlerini sorma, bu bilgileri kişisel olarak sıralamaya ve bu bilgilerin bir liste arasından seçim yapmak üzere müşteriye sunmaya, sonra da hizmetin seçimi derecelendirmesinin ne kadar iyi olduğunu işaret eden bir şekilde göndermeye yönelik bir döngüyle geçer.

[!code-csharp[Learning loop](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=mainLoop)]

Kod dosyasını çalıştırmadan önce [içerik seçimlerini almak](#get-food-items-as-rankable-actions)için aşağıdaki yöntemleri ekleyin:

* `GetActions`
* `GetUsersTimeOfDay`
* `GetUsersTastePreference`
* `GetKey`

## <a name="request-a-rank"></a>Bir derece iste

Program, derecelendirme isteğini gerçekleştirmek için kullanıcının tercihlerine içerik seçeneklerinin `currentContent` oluşturmasını ister. İşlem, `excludeActions`olarak gösterilen derecenin dışında tutulacak içerik oluşturabilir. Sıralama isteğinin, sıralanmış yanıtı almak için Eylemler, currentContext, excludeActions ve benzersiz bir sıra olay KIMLIĞI (GUID olarak) gerekir. 

Bu hızlı başlangıçta, günün saati ve Kullanıcı yiyecek tercihi basit bağlam özelliklerine sahiptir. Üretim sistemlerinde, [eylemleri ve özellikleri](concepts-features.md) belirlemek ve [değerlendirmek](concept-feature-evaluation.md) önemsiz olmayan bir şekilde olabilir.  

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=rank)]

## <a name="send-a-reward"></a>Bir ödül gönderin

Yeniden isteği tamamlamaya yönelik olarak, program kullanıcının seçimini komut satırından alır, her seçime bir sayısal değer atar, sonra benzersiz sıralama olay kimliğini ve sayısal değeri ödül yöntemine gönderir.

Bu hızlı başlangıç, sıfır veya 1 olarak basit bir sayı atar. Üretim [sistemlerinde, bu çağrıya ne](concept-rewards.md) zaman ve ne gönderileceğini belirlemek, özel gereksinimlerinize bağlı olarak önemsiz olmayan bir önemi olabilir. 

[!code-csharp[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/csharp/PersonalizerExample/Program.cs?name=reward)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulama dizininizde DotNet `run` komutuyla uygulamayı çalıştırın.

```console
dotnet run
```

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

[Bu hızlı başlangıç için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/csharp/PersonalizerExample/Program.cs) , kişiselleştirici örnekleri GitHub deposunda bulunur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Kişiselleştirici nasıl kullanılır?](how-personalizer-works.md)

* [Kişiselleştirici nedir?](what-is-personalizer.md)
* [Kişiselleştirici 'yi nerede kullanabilirsiniz?](where-can-you-use-personalizer.md)
* [Sorun giderme](troubleshooting.md)
