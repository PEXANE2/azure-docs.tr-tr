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
ms.openlocfilehash: fe800280a7a652b5d9a397a21a5b0a66b40af4dc
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122925"
---
[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer?view=azure-python) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-personalizer) | [paketi (Pypı)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Python 3. x](https://www.python.org/)

## <a name="using-this-quickstart"></a>Bu hızlı başlangıcı kullanma


Bu hızlı başlangıcı kullanmanın birkaç adımı vardır:

* Azure portal, bir kişiselleştirici kaynağı oluşturun
* Azure portal, kişiselleştirici kaynağı için **yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralığa göre değiştirin
* Kod düzenleyicisinde bir kod dosyası oluşturun ve kod dosyasını düzenleyin
* Komut satırı veya terminalinde, komut satırından SDK 'Yı yükler
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

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

Tercih ettiğiniz düzenleyicide veya IDE 'de `sample.py`adlı yeni bir Python uygulaması oluşturun.

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, **Sample.py** dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdakileri ekleyin:

[!code-python[Add module dependencies](~/samples-personalizer/quickstarts/python/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Kişiselleştirici kaynak bilgileri ekleme

`PERSONALIZER_RESOURCE_KEY` ve `PERSONALIZER_RESOURCE_ENDPOINT`adlı ortam değişkenlerinden çekilen kaynak Azure anahtarı ve uç nokta için değişkenler oluşturun. Uygulama başlatıldıktan sonra ortam değişkenlerini oluşturduysanız, değişkene erişmek için bu uygulamayı çalıştıran düzenleyici, IDE veya kabuğun kapatılıp yeniden yüklenmesi gerekir. Yöntemler daha sonra bu hızlı başlangıçta oluşturulacaktır.

Kaynak adı, uç nokta URL 'sinin bir parçası: `https://<your-resource-name>.api.cognitive.microsoft.com/`.

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/python/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Bir kişiselleştirici istemci oluşturma

Sonra, bir kişiselleştirici istemci döndürmek için bir yöntem oluşturun. Yöntemine yönelik parametre `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey `PERSONALIZER_RESOURCE_KEY`.

[!code-python[Create the Personalizer client](~/samples-personalizer/quickstarts/python/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Eylem olarak temsil edilen içerik seçimlerini al

Eylemler, Kişiselleştiriciye en iyi içerik öğesini seçmesini istediğiniz içerik seçimlerini temsil eder. Eylemler kümesini ve bunların özelliklerini temsil etmek için program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsünü oluşturma

Kişiselleştirici öğrenme döngüsü, bir [derece](#request-the-best-action) ve [yeniden](#send-a-reward) arama çağrısı döngüsüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her bir derecelendirme çağrısının ardından, hizmetin ne kadar iyi sonuç vereceğini söylemek için bir ödül çağrısı vardır.

Aşağıdaki kod, kullanıcıdan komut satırında tercihlerini sorma, en iyi eylemi seçmek için bu bilgileri Kişiselleştiriciye göndermek üzere bu bilgileri kişisel olarak, liste arasından seçim yapmak üzere müşteriye sunarak ve sonra bir yeniden Kişiselleştirici, hizmetin seçiminde ne kadar iyi olduğunu işaret ediyor.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=mainLoop&highlight=9,10,29)]

Kod dosyasını çalıştırmadan önce [içerik seçimlerini almak](#get-content-choices-represented-as-actions)için aşağıdaki yöntemleri ekleyin:

* `get_user_preference`
* `get_user_timeofday`
* `get_actions`

## <a name="request-the-best-action"></a>En iyi eylemi isteyin


Program, derecelendirme isteğini gerçekleştirmek için kullanıcının tercihlerine içerik seçeneklerinin `currentContent` oluşturmasını ister. İşlem, `excludeActions`olarak gösterilen eylemlerden dışlamak için içerik oluşturabilir. Yanıtları almak için sıralama isteğinin eylemleri ve özellikleri, currentContext özellikleri, excludeActions ve benzersiz bir olay KIMLIĞI olması gerekir.

Bu hızlı başlangıçta, günün saati ve Kullanıcı yiyecek tercihi basit bağlam özelliklerine sahiptir. Üretim sistemlerinde, [eylemleri ve özellikleri](../concepts-features.md) belirlemek ve [değerlendirmek](../concept-feature-evaluation.md) önemsiz olmayan bir şekilde olabilir.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=rank)]

## <a name="send-a-reward"></a>Bir ödül gönderin


Yeniden işleme isteği gönderilmesi için, program komut satırından kullanıcının seçimini alır, seçime bir sayısal değer atar, sonra benzersiz olay kimliği ve ödül Puanını sayısal değer olarak ödül API 'sine gönderir.

Bu hızlı başlangıç, bir sıfır veya 1 ya da bir yeniden puan olarak basit bir sayı atar. Üretim [sistemlerinde, bu çağrıya ne](../concept-rewards.md) zaman ve ne gönderileceğini belirlemek, özel gereksinimlerinize bağlı olarak önemsiz olmayan bir önemi olabilir.

[!code-python[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/python/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı uygulama dizininizdeki Python ile çalıştırın.

```console
python sample.py
```

![Hızlı başlangıç programı, özellikler olarak bilinen Kullanıcı tercihlerini toplamak için birkaç soru sorar ve sonra en iyi eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)