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
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122925"
---
[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-personalizer) | [Paketi (pypi)](https://pypi.org/project/azure-cognitiveservices-personalizer/) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="using-this-quickstart"></a>Bu hızlı başlatmayı kullanma


Bu hızlı başlatmayı kullanmak için birkaç adım vardır:

* Azure portalında bir Kisizer kaynağı oluşturun
* Azure portalında, Kiliker kaynağı için **Yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralıkla değiştirin
* Kod düzenleyicisinde, bir kod dosyası oluşturun ve kod dosyasını
* Komut satırına veya terminale, SDK komut satırından
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="install-the-python-library-for-personalizer"></a>Kişiselleştirici için Python kitaplığını yükleme

Python için Personalizer istemci kitaplığını aşağıdaki komutla yükleyin:

```console
pip install azure-cognitiveservices-personalizer
```

## <a name="object-model"></a>Nesne modeli

Personalizer istemcisi, anahtarınızı içeren Microsoft.Rest.ServiceClient Credentials'ı kullanarak Azure'a doğrulayan bir [PersonalizerClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.personalizer_client.personalizerclient?view=azure-python) nesnesidir.

İçeriğin en iyi tek öğesini istemek için bir [RankRequest](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.models.rankrequest?view=azure-python)oluşturun ve ardından istemciye geçirin. Sıralama yöntemi. Rank yöntemi bir RankResponse döndürür.

Personalizer'a ödül puanı göndermek için event id'ini ve ödül puanını (değeri) EventOperations sınıfındaki [Ödül](https://docs.microsoft.com/python/api/azure-cognitiveservices-personalizer/azure.cognitiveservices.personalizer.operations.events_operations.eventsoperations?view=azure-python#reward-event-id--value--custom-headers-none--raw-false----operation-config-) yöntemine göndermek üzere ayarlayın.

Bu hızlı başlangıçta ödülü belirlemek önemsizdir. Bir üretim sisteminde, [ödül puanını](../concept-rewards.md) neyin etkilediğinin ve ne kadarının karmaşık bir süreç olabileceğinin belirlenmesi, zaman içinde değiştirmeye karar verebilirsiniz. Bu, Personalizer mimarinizdeki birincil tasarım kararlarından biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Personalizer istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bir Personalizer istemcisi oluşturma](#create-a-personalizer-client)
* [Sıralama API'si](#request-the-best-action)
* [Ödül API'si](#send-a-reward)

## <a name="create-a-new-python-application"></a>Yeni bir python uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE adlı `sample.py`yeni bir Python uygulaması oluşturun.

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih ettiğiniz düzenleyici veya IDE'deki **sample.py** dosyasını açın. Aşağıdakileri ekleyin:

[!code-python[Add module dependencies](~/samples-personalizer/quickstarts/python/sample.py?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Personalizer kaynak bilgilerini ekleme

Kaynağınızın Azure tuşu ve bitiş noktası için, adlı `PERSONALIZER_RESOURCE_KEY` ve `PERSONALIZER_RESOURCE_ENDPOINT`. Uygulama başlatıldıktan sonra ortam değişkenlerini oluşturduysanız, düzenleyici, IDE veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra bu hızlı başlatmada oluşturulur.

Kaynak adı bitiş noktası URL'sinin `https://<your-resource-name>.api.cognitive.microsoft.com/`bir parçasıdır: .

[!code-python[Create variables to hold the Personalizer resource key and endpoint values found in the Azure portal.](~/samples-personalizer/quickstarts/python/sample.py?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Bir Personalizer istemcisi oluşturma

Ardından, Bir Personalizer istemcisi döndürmek için bir yöntem oluşturun. Yöntemin parametresi `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey olduğunu `PERSONALIZER_RESOURCE_KEY`.

[!code-python[Create the Personalizer client](~/samples-personalizer/quickstarts/python/sample.py?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Eylem olarak temsil edilen içerik seçeneklerini alma

Eylemler, Personalizer'ın en iyi içerik öğesini seçmesini istediğiniz içerik seçeneklerini temsil ediyor. Eylem kümesini ve özelliklerini temsil etmek için Program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=getActions)]

[!code-python[Present time out day preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTimeOfDay)]

[!code-python[Present food taste preference to the user](~/samples-personalizer/quickstarts/python/sample.py?name=createUserFeatureTastePreference)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsü oluşturma

Personalizer öğrenme döngüsü [Rank](#request-the-best-action) ve [Reward](#send-a-reward) çağrıları bir döngüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için her rütbe çağrısı, Personalizer'a hizmetin ne kadar iyi performans gösterdiğini söylemek için bir ödül çağrısı yla takip edilir.

Aşağıdaki kod, kullanıcıya komut satırında tercihlerini sorma, bu bilgileri en iyi eylemi seçmesi için Personalizer'a gönderme, liste arasından seçim yapmak üzere müşteriye seçimi sunma ve ardından bir ödül gönderme döngüsü boyunca döngüler Personalizer, hizmetin seçiminde ne kadar iyi olduğunu işaret ediyor.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=mainLoop&highlight=9,10,29)]

Kod [dosyasını çalıştırmadan](#get-content-choices-represented-as-actions)önce içerik seçeneklerini alan aşağıdaki yöntemleri ekleyin:

* `get_user_preference`
* `get_user_timeofday`
* `get_actions`

## <a name="request-the-best-action"></a>En iyi eylemi isteyin


Sıralama isteğini tamamlamak için program, kullanıcının tercihlerinden `currentContent` içerik seçeneklerinden birini oluşturmasını ister. İşlem, '. olarak `excludeActions`gösterilen eylemlerden hariç tutmak için içerik oluşturabilir. Yanıtı almak için Sıralama isteğinin eylemlere ve özelliklerine, geçerli Bağlam özelliklerine, dışlama Eylemlerine ve benzersiz bir olay kimliğine ihtiyacı vardır.

Bu hızlı başlatma, günün saati ve kullanıcı gıda tercihinin basit bağlam özelliklerine sahiptir. Üretim [sistemlerinde, eylemlerin](../concepts-features.md) ve özelliklerin belirlenmesi ve [değerlendirilmesi](../concept-feature-evaluation.md) önemsiz olmayan bir konu olabilir.

[!code-python[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/python/sample.py?name=rank)]

## <a name="send-a-reward"></a>Ödül gönder


Ödül isteğini göndermek için ödül puanını almak için program kullanıcının seçimini komut satırından alır, seçime sayısal bir değer atar, ardından benzersiz etkinlik kimliğini ve ödül puanını ödül API'sine sayısal değer olarak gönderir.

Bu hızlı başlangıç, ödül puanı olarak basit bir sayı atar, sıfır veya 1. Üretim sistemlerinde, [Ödül](../concept-rewards.md) çağrısına ne zaman ve ne zaman göndereceğiniz belirlenebilir, özel ihtiyaçlarınıza bağlı olarak önemsiz olmayan bir konu olabilir.

[!code-python[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/python/sample.py?name=reward&highlight=9)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulama dizininizdeki python ile uygulamayı çalıştırın.

```console
python sample.py
```

![Hızlı başlangıç programı, özellikler olarak bilinen kullanıcı tercihlerini toplamak için birkaç soru sorar ve ardından en üst eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)