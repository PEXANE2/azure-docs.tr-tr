---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file, devx-track-javascript
ms.date: 07/30/2020
ms.openlocfilehash: ba7885859adc1d9899c66917204306c8a0d0092f
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246499"
---
[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/?view=azure-node-latest)  | [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-personalizer)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-personalizer)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/Personalizer)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org) ve NPM 'nin geçerli sürümü.

## <a name="using-this-quickstart"></a>Bu hızlı başlangıcı kullanma


Bu hızlı başlangıcı kullanmanın birkaç adımı vardır:

* Azure portal, bir kişiselleştirici kaynağı oluşturun
* Azure portal, kişiselleştirici kaynağı için **yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralığa göre değiştirin
* Kod düzenleyicisinde bir kod dosyası oluşturun ve kod dosyasını düzenleyin
* Komut satırı veya terminalinde, komut satırından SDK 'Yı yükler
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin.

```console
mkdir myapp && cd myapp
```

`npm init -y`Bir dosya oluşturmak için komutunu çalıştırın `package.json` .

```console
npm init -y
```

## <a name="install-the-nodejs-library-for-personalizer"></a>Kişiselleştirici için Node.js kitaplığı 'nı yükler

Aşağıdaki komutla Node.js için kişiselleştirici istemci kitaplığını yükler:

```console
npm install @azure/cognitiveservices-personalizer --save
```

Bu hızlı başlangıç için kalan NPM paketlerini yükler:

```console
npm install @azure/ms-rest-azure-js @azure/ms-rest-js readline-sync uuid --save
```

## <a name="object-model"></a>Nesne modeli

Kişiselleştirici istemci, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan bir [Personizerclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/personalizerclient?view=azure-node-latest) nesnesidir.

İçeriğin tek bir en iyi öğesi için sormak üzere bir [Rankrequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/rankrequest?view=azure-node-latest)oluşturun ve ardından [istemciye geçirin. Rank](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/personalizerclient?view=azure-node-latest#rank-rankrequest--msrest-requestoptionsbase-) yöntemi. Rank yöntemi bir RankResponse döndürür.

Kişiselleştiriciye bir ödül göndermek için, bir [RewardRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/rewardrequest?view=azure-node-latest)oluşturun ve ardından bunu Events sınıfındaki [ödül](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/events?view=azure-node-latest#reward-string--rewardrequest--servicecallback-void--) yöntemine geçirin.

Bu hızlı başlangıçta önemsiz bir şekilde belirlenir. Bir üretim sisteminde, İleri [puanı](../concept-rewards.md) neyin etkilediğini ve ne kadar karmaşık bir süreç olabileceğini belirleme, zaman içinde değiştirmeye karar verebilirsiniz. Bu, kişiselleştirici mimarinizdeki birincil tasarım kararlarından biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için kişiselleştirici istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Bir kişiselleştirici istemci oluşturma](#create-a-personalizer-client)
* [Derecelendirme API 'SI](#request-the-best-action)
* [Reward API 'SI](#send-a-reward)

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı yeni bir Node.js uygulaması oluşturun `sample.js` .

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

**sample.js** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. `requires`NPM paketlerini eklemek için aşağıdakileri ekleyin:

[!code-javascript[Add module dependencies](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Kişiselleştirici kaynak bilgileri ekleme

Kaynak Azure anahtarı ve uç noktası için kod dosyasının en üstüne doğru olan anahtar ve uç nokta değişkenlerini düzenleyin. 

[!code-javascript[Add Personalizer resource information](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Bir kişiselleştirici istemci oluşturma

Sonra, bir kişiselleştirici istemci döndürmek için bir yöntem oluşturun. Yöntemine parametresi, `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey ' dir `PERSONALIZER_RESOURCE_KEY` .

[!code-javascript[Create a Personalizer client](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Eylem olarak temsil edilen içerik seçimlerini al

Eylemler, Kişiselleştiriciye en iyi içerik öğesini seçmesini istediğiniz içerik seçimlerini temsil eder. Eylemler kümesini ve bunların özelliklerini temsil etmek için program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-javascript[Create user features](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=createUserFeatureTimeOfDay)]

[!code-javascript[Create actions](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=getActions)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsünü oluşturma

Kişiselleştirici öğrenme döngüsü, bir [derece](#request-the-best-action) ve [yeniden](#send-a-reward) arama çağrısı döngüsüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her bir derecelendirme çağrısının ardından, hizmetin ne kadar iyi sonuç vereceğini söylemek için bir Reward çağrısı vardır.

Aşağıdaki kod, kullanıcıdan komut satırında tercihlerini isteyen bir döngüyle geçer, en iyi eylemi seçmek için bu bilgileri Kişiselleştiriciye göndermek üzere bu bilgileri kişisel olarak, liste arasından seçim yapmak üzere müşteriye sunarak, daha sonra da hizmetin seçimine ne kadar iyi olduğunu gösteren bir ödül gönderilmesini sağlar.

[!code-javascript[Create the learning loop](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=mainLoop)]

Aşağıdaki bölümlerde bulunan derece ve ödül çağrılarına daha yakından göz atın.

Kod dosyasını çalıştırmadan önce [içerik seçimlerini almak](#get-content-choices-represented-as-actions)için aşağıdaki yöntemleri ekleyin:

* getActionsList
* getContextFeaturesList

## <a name="request-the-best-action"></a>En iyi eylemi isteyin

Sıralama isteğini tamamlayabilmeniz için, program kullanıcının tercihlerine içerik seçenekleri oluşturmasını ister. İşlem, eylemlerden hariç tutulacak içerik oluşturabilir `excludeActions` . Sıralama isteğinin, sıralanmış yanıtı almak için [eylemleri](../concepts-features.md#actions-represent-a-list-of-options) ve özellikleri, CurrentContext özellikleri, excludeactions ve benzersiz bir sıra olay kimliği gerekir.

Bu hızlı başlangıçta, günün saati ve Kullanıcı yiyecek tercihi basit bağlam özelliklerine sahiptir. Üretim sistemlerinde, [eylemleri ve özellikleri](../concepts-features.md) belirlemek ve [değerlendirmek](../concept-feature-evaluation.md) önemsiz olmayan bir şekilde olabilir.

[!code-javascript[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=rank)]

## <a name="send-a-reward"></a>Bir ödül gönderin


Yeniden işleme isteği gönderilmesi için, program komut satırından kullanıcının seçimini alır, seçime bir sayısal değer atar, sonra benzersiz olay kimliği ve ödül Puanını sayısal değer olarak ödül API 'sine gönderir.

Bu hızlı başlangıç, bir sıfır veya 1 ya da bir yeniden puan olarak basit bir sayı atar. Üretim [sistemlerinde, bu çağrıya ne](../concept-rewards.md) zaman ve ne gönderileceğini belirlemek, özel gereksinimlerinize bağlı olarak önemsiz olmayan bir önemi olabilir.

[!code-javascript[The Personalizer learning loop sends a reward.](~/cognitive-services-quickstart-code/javascript/Personalizer/sample.js?name=reward)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı uygulama dizininizdeki Node.js çalıştırın.

```console
node sample.js
```

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)
