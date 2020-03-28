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
ms.openlocfilehash: 786efcb712557da4363384c9d05c33f4f16d6731
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122883"
---
[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/?view=azure-node-latest) |[Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-personalizer) | [Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-personalizer) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/node/sample.js)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Node.js](https://nodejs.org) ve NPM'nin geçerli sürümü.

## <a name="using-this-quickstart"></a>Bu hızlı başlatmayı kullanma


Bu hızlı başlatmayı kullanmak için birkaç adım vardır:

* Azure portalında bir Kisizer kaynağı oluşturun
* Azure portalında, Kiliker kaynağı için **Yapılandırma** sayfasında, model güncelleştirme sıklığını çok kısa bir aralıkla değiştirin
* Kod düzenleyicisinde, bir kod dosyası oluşturun ve kod dosyasını
* Komut satırına veya terminale, SDK komut satırından
* Komut satırında veya terminalde, kod dosyasını çalıştırın

[!INCLUDE [Create Azure resource for Personalizer](create-personalizer-resource.md)]

[!INCLUDE [!Change model frequency](change-model-frequency.md)]

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin.

```console
mkdir myapp && cd myapp
```

Bir `package.json` `npm init -y` dosya oluşturmak için komutu çalıştırın.

```console
npm init -y
```

## <a name="install-the-nodejs-library-for-personalizer"></a>Personalizer için Düğüm.js kitaplığını yükleyin

Node.js için Personalizer istemci kitaplığını aşağıdaki komutla yükleyin:

```console
npm install @azure/cognitiveservices-personalizer --save
```

Bu hızlı başlatma için kalan NPM paketlerini yükleyin:

```console
npm install @azure/ms-rest-azure-js @azure/ms-rest-js readline-sync uuid --save
```

## <a name="object-model"></a>Nesne modeli

Personalizer istemcisi, anahtarınızı içeren Microsoft.Rest.ServiceClient Credentials'ı kullanarak Azure'a doğrulayan bir [PersonalizerClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/personalizerclient?view=azure-node-latest) nesnesidir.

İçeriğin en iyi tek öğesini istemek için bir [RankRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/rankrequest?view=azure-node-latest)oluşturun ve [ardından istemciye geçirin. Sıralama](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/personalizerclient?view=azure-node-latest#rank-rankrequest--msrest-requestoptionsbase-) yöntemi. Rank yöntemi bir RankResponse döndürür.

Personalizer'a ödül göndermek için bir [RewardRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/rewardrequest?view=azure-node-latest)oluşturun ve ardından Etkinlikler sınıfındaki [Ödül](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-personalizer/events?view=azure-node-latest#reward-string--rewardrequest--servicecallback-void--) yöntemine geçirin.

Bu hızlı başlangıçta ödülü belirlemek önemsizdir. Bir üretim sisteminde, [ödül puanını](../concept-rewards.md) neyin etkilediğinin ve ne kadarının karmaşık bir süreç olabileceğinin belirlenmesi, zaman içinde değiştirmeye karar verebilirsiniz. Bu, Personalizer mimarinizdeki birincil tasarım kararlarından biri olmalıdır.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için Personalizer istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Bir Personalizer istemcisi oluşturma](#create-a-personalizer-client)
* [Sıralama API'si](#request-the-best-action)
* [Ödül API'si](#send-a-reward)

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE adlı `sample.js`yeni bir Düğüm.js uygulaması oluşturun.

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Tercih ettiğiniz düzenleyici veya IDE **örnek.js** dosyasını açın. NPM `requires` paketlerini eklemek için aşağıdakileri ekleyin:

[!code-javascript[Add module dependencies](~/samples-personalizer/quickstarts/node/sample.js?name=Dependencies)]

## <a name="add-personalizer-resource-information"></a>Personalizer kaynak bilgilerini ekleme

Kaynağınızın Azure tuşu ve bitiş noktası için, adlı `PERSONALIZER_KEY` ve `PERSONALIZER_ENDPOINT`. Uygulama başlatıldıktan sonra ortam değişkenlerini oluşturduysanız, düzenleyici, IDE veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra bu hızlı başlatmada oluşturulur.

[!code-javascript[Add Personalizer resource information](~/samples-personalizer/quickstarts/node/sample.js?name=AuthorizationVariables)]

## <a name="create-a-personalizer-client"></a>Bir Personalizer istemcisi oluşturma

Ardından, Bir Personalizer istemcisi döndürmek için bir yöntem oluşturun. Yöntemin parametresi `PERSONALIZER_RESOURCE_ENDPOINT` ve ApiKey olduğunu `PERSONALIZER_RESOURCE_KEY`.

[!code-javascript[Create a Personalizer client](~/samples-personalizer/quickstarts/node/sample.js?name=Client)]

## <a name="get-content-choices-represented-as-actions"></a>Eylem olarak temsil edilen içerik seçeneklerini alma

Eylemler, Personalizer'ın en iyi içerik öğesini seçmesini istediğiniz içerik seçeneklerini temsil ediyor. Eylem kümesini ve özelliklerini temsil etmek için Program sınıfına aşağıdaki yöntemleri ekleyin.

[!code-javascript[Create user features](~/samples-personalizer/quickstarts/node/sample.js?name=createUserFeatureTimeOfDay)]

[!code-javascript[Create actions](~/samples-personalizer/quickstarts/node/sample.js?name=getActions)]

## <a name="create-the-learning-loop"></a>Öğrenme döngüsü oluşturma

Personalizer öğrenme döngüsü [Rank](#request-the-best-action) ve [Reward](#send-a-reward) çağrıları bir döngüdür. Bu hızlı başlangıçta, içeriği kişiselleştirmek için yapılan her Rütbe çağrısını, Personalizer'a hizmetin ne kadar iyi performans gösterdiğini söylemek için bir Ödül çağrısı takip eder.

Aşağıdaki kod, kullanıcıya komut satırında tercihlerini sorma, bu bilgileri en iyi eylemi seçmesi için Personalizer'a gönderme, liste arasından seçim yapmak üzere müşteriye seçimi sunma ve ardından bir ödül gönderme döngüsü boyunca döngüler Personalizer, hizmetin seçiminde ne kadar iyi olduğunu işaret ediyor.

[!code-javascript[Create the learning loop](~/samples-personalizer/quickstarts/node/sample.js?name=mainLoop)]

Aşağıdaki bölümlerdeki sıralama ve ödül çağrılarına daha yakından bakın.

Kod [dosyasını çalıştırmadan](#get-content-choices-represented-as-actions)önce içerik seçeneklerini alan aşağıdaki yöntemleri ekleyin:

* getActionsList
* getContextFeaturesList

## <a name="request-the-best-action"></a>En iyi eylemi isteyin

Sıralama isteğini tamamlamak için program, kullanıcının tercihlerinden içerik seçenekleri oluşturmasını ister. İşlem, '. olarak `excludeActions`gösterilen eylemlerden hariç tutmak için içerik oluşturabilir. Sıralama isteği, sıralamalı yanıtı almak için [eylemlere](../concepts-features.md#actions-represent-a-list-of-options) ve özelliklerine, geçerli Bağlam özelliklerine, dışlama Eylemleri'ne ve benzersiz bir sıralama olay kimliğine ihtiyaç duyar.

Bu hızlı başlatma, günün saati ve kullanıcı gıda tercihinin basit bağlam özelliklerine sahiptir. Üretim [sistemlerinde, eylemlerin](../concepts-features.md) ve özelliklerin belirlenmesi ve [değerlendirilmesi](../concept-feature-evaluation.md) önemsiz olmayan bir konu olabilir.

[!code-javascript[The Personalizer learning loop ranks the request.](~/samples-personalizer/quickstarts/node/sample.js?name=rank)]

## <a name="send-a-reward"></a>Ödül gönder


Ödül isteğini göndermek için ödül puanını almak için program kullanıcının seçimini komut satırından alır, seçime sayısal bir değer atar, ardından benzersiz etkinlik kimliğini ve ödül puanını ödül API'sine sayısal değer olarak gönderir.

Bu hızlı başlangıç, ödül puanı olarak basit bir sayı atar, sıfır veya 1. Üretim sistemlerinde, [Ödül](../concept-rewards.md) çağrısına ne zaman ve ne zaman göndereceğiniz belirlenebilir, özel ihtiyaçlarınıza bağlı olarak önemsiz olmayan bir konu olabilir.

[!code-javascript[The Personalizer learning loop sends a reward.](~/samples-personalizer/quickstarts/node/sample.js?name=reward)]

## <a name="run-the-program"></a>Programı çalıştırma

Uygulamayı uygulama dizininizdeki Node.js ile çalıştırın.

```console
node sample.js
```

![Hızlı başlangıç programı, özellikler olarak bilinen kullanıcı tercihlerini toplamak için birkaç soru sorar ve ardından en üst eylemi sağlar.](../media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)
