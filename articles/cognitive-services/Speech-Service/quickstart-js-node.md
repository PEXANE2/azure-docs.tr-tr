---
title: 'Hızlı Başlangıç: Konuşmayı, Node. js-konuşma hizmetini tanıma'
titleSuffix: Azure Cognitive Services
description: Node. js için konuşma SDK 'sını kullanarak bir konuşmaya metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 1a3d1bc62e995aff43d9538f49b436a7de16f1e9
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68554074"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Hızlı Başlangıç: Node. js için konuşma SDK 'Sı ile konuşmayı tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, konuşmayı metne dönüştürme amacıyla Azure bilişsel hizmetler için konuşma SDK 'sının JavaScript bağlamasını kullanarak bir Node. js projesi oluşturma gösterilmektedir.
Uygulama, [JavaScript Için konuşma SDK 'sını](https://aka.ms/csspeech/npmpackage)temel alır.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* [Node. js](https://nodejs.org)' nin güncel bir sürümü.

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Yeni bir klasör oluşturun ve projeyi başlatın:

```sh
npm init -f
```

Bu komut **Package. JSON** dosyalarını varsayılan değerleriyle başlatır. Muhtemelen bu dosyayı daha sonra düzenlemek isteyeceksiniz.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Konuşma SDK 'sını Node. js projenize ekleyin:

```
npm install microsoft-cognitiveservices-speech-sdk
```

Bu komut, konuşma SDK 'sının en son sürümünü ve **npmjs**' den gerekli önkoşulları indirir ve yükler. SDK, proje klasörünüzün içindeki `node_modules` dizine yüklenir.

## <a name="use-the-speech-sdk"></a>Konuşma SDK 'sını kullanma

Adlı `index.js`klasörde yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyicisiyle açın.

> [!NOTE]
> Node. js ' de, konuşma SDK 'Sı mikrofonu veya **Dosya** veri türünü desteklemez. Her ikisi de yalnızca tarayıcılarda desteklenir. Bunun yerine, `AudioInputStream.createPullStream()`ya  da aracılığıyla `AudioInputStream.createPushStream()` konuşma SDK 'sı için Stream arabirimini kullanın.

Bu örnekte, `PushAudioInputStream` arabirimini kullanırız.

Şu JavaScript kodunu ekleyin:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Örneği çalıştırma

Uygulamayı açmak için yapılandırmanıza, ve `YourSubscriptionKey` `YourAudioFile.wav` ' `YourServiceRegion`a uyum sağlayın. Ardından şu komutu çağırarak çalıştırın:

```sh
node index.js
```

Belirtilen dosya adını kullanarak bir tanımayı tetikler. Ayrıca, çıktıyı konsolda gösterir.

Bu örnek, abonelik anahtarını güncelleştirdikten ve dosyasını `index.js` `whatstheweatherlike.wav`kullandıktan sonra çalıştırdığınızda çıktı örneğidir:

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Konuşma SDK 'sını Visual Studio Code ile yükleyip kullanma

Visual Studio Code örneği de çalıştırabilirsiniz. Hızlı başlangıcı yüklemek, açmak ve çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Code başlatın. **Klasörü aç**' ı seçin. Ardından Hızlı Başlangıç klasörüne gidin.

   ![Klasörü aç](media/sdk/qs-js-node-01-open_project.png)

1. Visual Studio Code bir Terminal açın.

   ![Terminal penceresi](media/sdk/qs-js-node-02_open_terminal.png)

1. Bağımlılıklarını `npm` yüklemek için ' i çalıştırın.

   ![NPM yüklemesi](media/sdk/qs-js-node-03-npm_install.png)

1. Artık bir kesme noktası açıp `index.js`ayarlamaya hazırsınız.

   ![16. satırdaki kesme noktasıyla index. js](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Hata ayıklamayı başlatmak için F5 ' i seçin ya da menüden hata **Ayıkla/hata ayıklamayı Başlat** ' ı seçin.

   ![Hata ayıklama menüsü](media/sdk/qs-js-node-05-start_debugging.png)

1. Kesme noktası isabet edildiğinde, çağrı yığınını ve değişkenlerini inceleyebilirsiniz.

   ![Hata Ayıklayıcısı](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Tüm çıktılar hata ayıklama konsolu penceresinde gösterilir.

   ![Hata ayıklama konsolu](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Node. js örneklerini keşfet](https://aka.ms/csspeech/samples)
