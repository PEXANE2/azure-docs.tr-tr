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
ms.openlocfilehash: aab4a59a35b098589adb462f2f0d6385802a9875
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246212"
---
[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-personalizer)  |  [Paket (Pypı)](https://pypi.org/project/azure-cognitiveservices-personalizer/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/Personalizer)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Python 3.x](https://www.python.org/)

## <a name="using-this-quickstart"></a>Bu hızlı başlangıcı kullanma


Bu hızlı başlangıcı kullanmanın birkaç adımı vardır:

* Azure portal, bir kişiselleştirici kaynağı oluşturun
* Azure portal, kişiselleştirici kaynağı için **yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralığa göre değiştirin
* Kod düzenleyicisinde bir kod dosyası oluşturun ve kod dosyasını düzenleyin
* Komut satırı veya terminalinde, komut satırından SDK 'Yı yükler
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [Change model frequency](change-model-frequency.md)]

## <a name="install-the-python-library-for-personalizer"></a>Kişiselleştirici için Python kitaplığını yükler

Python için kişiselleştirici istemci kitaplığını aşağıdaki komutla birlikte yükler:

```console
pip install azure-cognitiveservices-personalizer
```

## <a name="object-model"></a>Nesne modeli

Kişiselleştirici istemci, anahtarınızı içeren Microsoft. Rest. ServiceClientCredentials kullanarak Azure 'da kimlik doğrulaması yapan bir [Personizerclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.personalizer_client.personalizerclient?view=azure-python) nesnesidir.

İçeriğin tek bir en iyi öğesi için sormak üzere bir [Rankrequest](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.models.rankrequest?view=azure-python)oluşturun ve ardından istemciye geçirin. Rank yöntemi. Rank yöntemi bir RankResponse döndürür.

Kişiselleştiriciye bir ödül puanı göndermek için, olay kimliği ve ödül puanı (değer) ' i eventoperations sınıfında [ödül](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.operations.events_operations.eventsoperations?view=azure-python#reward-event-id--value--custom-headers-none--raw-false----operation-config-) yöntemine gönderecek şekilde ayarlayın.

Bu hızlı başlangıçta önemsiz bir şekilde belirlenir. Bir üretim sisteminde, İleri [puanı](../concept-rewards.md) neyin etkilediğini ve ne kadar karmaşık bir süreç olabileceğini belirleme, zaman içinde değiştirmeye karar verebilirsiniz. Bu, kişiselleştirici mimarinizdeki birincil tasarım kararlarından biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için kişiselleştirici istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Bir kişiselleştirici istemci oluşturma](#create-a-personalizer-client)
* [Derecelendirme API 'SI](#request-the-best-action)
* [Reward API 'SI](#send-a-reward)

## <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı yeni bir Python uygulaması oluşturun `sample.py` .

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, **Sample.py** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdakileri ekleyin:

[!code-python[Add module dependencies](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Kişiselleştirici kaynak bilgileri ekleme

Kaynak Azure anahtarı ve uç noktası için kod dosyasının en üstüne doğru olan anahtar ve uç nokta değişkenlerini düzenleyin. 

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Bir kişiselleştirici istemci oluşturma

Sonra, bir kişiselleştirici istemci döndürmek için bir yöntem oluşturun. Yöntemine parametresi, `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey ' dir `PERSONALIZER_RESOURCE_KEY` .

[!code-python[Create the Personalizer client](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Eylem olarak temsil edilen içerik seçimlerini al

Eylemler, Kişiselleştiriciye en iyi içerik öğesini seçmesini istediğiniz içerik seçimlerini temsil eder. Eylemler kümesini ve bunların özelliklerini temsil etmek için program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-python[Present time out day preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsünü oluşturma

Kişiselleştirici öğrenme döngüsü, bir [derece](#request-the-best-action) ve [yeniden](#send-a-reward) arama çağrısı döngüsüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her bir derecelendirme çağrısının ardından, hizmetin ne kadar iyi sonuç vereceğini söylemek için bir ödül çağrısı vardır.

Aşağıdaki kod, kullanıcıdan komut satırında tercihlerini isteyen bir döngüyle geçer, en iyi eylemi seçmek için bu bilgileri Kişiselleştiriciye göndermek üzere bu bilgileri kişisel olarak, liste arasından seçim yapmak üzere müşteriye sunarak, daha sonra da hizmetin seçimine ne kadar iyi olduğunu gösteren bir ödül gönderilmesini sağlar.

[!code-python[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=mainLoop&highlight=9,10,29)]

Kod dosyasını çalıştırmadan önce [içerik seçimlerini almak](#get-content-choices-represented-as-actions)için aşağıdaki yöntemleri ekleyin:

* `get_user_preference`
* `get_user_timeofday`
* `get_actions`

## <a name="request-the-best-action"></a>En iyi eylemi isteyin


Sıralama isteğini gerçekleştirmek için, program kullanıcının tercihlerini `currentContent` içerik seçimlerinden birini oluşturacak şekilde sorar. İşlem, eylemlerden hariç tutulacak içerik oluşturabilir `excludeActions` . Yanıtları almak için sıralama isteğinin eylemleri ve özellikleri, currentContext özellikleri, excludeActions ve benzersiz bir olay KIMLIĞI olması gerekir.

Bu hızlı başlangıçta, günün saati ve Kullanıcı yiyecek tercihi basit bağlam özelliklerine sahiptir. Üretim sistemlerinde, [eylemleri ve özellikleri](../concepts-features.md) belirlemek ve [değerlendirmek](../concept-feature-evaluation.md) önemsiz olmayan bir şekilde olabilir.

[!code-python[The Personalizer learning loop ranks the request.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=rank)]

## <a name="send-a-reward"></a>Bir ödül gönderin


Yeniden işleme isteği gönderilmesi için, program komut satırından kullanıcının seçimini alır, seçime bir sayısal değer atar, sonra benzersiz olay kimliği ve ödül Puanını sayısal değer olarak ödül API 'sine gönderir.

Bu hızlı başlangıç, bir sıfır veya 1 ya da bir yeniden puan olarak basit bir sayı atar. Üretim [sistemlerinde, bu çağrıya ne](../concept-rewards.md) zaman ve ne gönderileceğini belirlemek, özel gereksinimlerinize bağlı olarak önemsiz olmayan bir önemi olabilir.

[!code-python[The Personalizer learning loop sends a reward.](~/cognitive-services-quickstart-code/python/Personalizer/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı uygulama dizininizdeki Python ile çalıştırın.

```console
python sample.py
```

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)