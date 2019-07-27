---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, JavaScript (tarayıcı)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak bir tarayıcıda JavaScript 'te konuşmayı tanımayı öğrenin
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 69ec6062d0dba1f1044f9a7dcfbe75924887d2b4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559412"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak bir tarayıcıda JavaScript 'te konuşmayı tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, konuşmayı metne dönüştürmek için Bilişsel Hizmetler Konuşma SDK’sının JavaScript bağlamasını kullanarak bir web sitesi oluşturmayı öğreneceksiniz.
Uygulama, JavaScript için konuşma SDK 'sını temel alır ([Sürüm 1.5.0 indir](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma tanıma hizmeti için bir abonelik anahtarı. Bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md).
* Çalışan bir mikrofon ile bir PC veya Mac.
* Bir metin düzenleyici.
* Chrome, Microsoft Edge veya Safari 'nin güncel bir sürümü.
* İsteğe bağlı olarak, PHP betiklerini barındırmayı destekleyen bir web sunucusu.

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturma

Yeni, boş bir klasör oluşturun. Örneği bir web sunucusunda barındırmak istiyorsanız, web sunucusunun klasöre erişebildiğinden emin olun.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>JavaScript için Konuşma SDK’sını bu klasöre çıkarın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Konuşma SDK’sını [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) olarak indirin ve yeni oluşturulan klasöre çıkarın. Bu, iki dosya için bir paketi, `microsoft.cognitiveservices.speech.sdk.bundle.js` ve `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
İkinci dosya isteğe bağlıdır ve SDK kodunda hata ayıklamak için faydalıdır.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Klasörde `index.html` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

1. Aşağıdaki HTML çatısını oluşturun:

   ```html
   <html>
   <head>
      <title>Speech SDK JavaScript Quickstart</title>
   </head>
   <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
   </body>
   </html>
   ```

1. Aşağıda kullanıcı arabirimi kodunu dosyanızda ilk açıklamanın altına ekleyin:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Konuşma SDK’sına bir başvuru ekleme

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Tanıma düğmesi, tanınma sonucu ve abonelik ile ilgili alanlar için Kullanıcı arabirimi kodu tarafından tanımlanan yukarı bağlantı işleyicileri:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Belirteç kaynağı oluşturma (isteğe bağlı)

Web sayfasını bir web sunucusunda barındırmak istiyorsanız, tanıtım uygulamanız için isteğe bağlı olarak bir belirteç kaynağı sağlayabilirsiniz.
Bu şekilde, abonelik anahtarınız hiçbir zaman sunucunuzdan çıkmaz ve kullanıcılar bir yetkilendirme kodu girmeden konuşma özelliklerini kullanabilir.

1. `token.php` adlı yeni bir dosya oluşturun. Bu örnekte web sunucunuzun PHP betik oluşturma dilini desteklediği varsayılmaktadır. Aşağıdaki kodu girin:

   [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. `index.html` dosyasını düzenleyin ve aşağıdaki kodu dosyanıza ekleyin:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Yetkilendirme belirteçlerinin sınırlı bir kullanım ömrü vardır.
> Bu basit örnekte yetkinlendirme belirteçlerinin otomatik olarak yenilenmesi gösterilmez. Kullanıcı olarak, sayfayı el ile yeniden yükleyebilir veya yenilemek için F5’e basabilirsiniz.

## <a name="build-and-run-the-sample-locally"></a>Örneği yerel olarak derleyin ve çalıştırın

Uygulamayı başlatmak için, index.html dosyasına çift tıklayın veya index.html dosyasını sık kullandığınız web tarayıcısıyla açın. Abonelik anahtarınızı ve [bölgenizi](regions.md) girmenize ve mikrofonu kullanarak bir tanıma tetiklemenize olanak sağlayan basit bir GUI sunar.

> [!NOTE]
> Bu yöntem Safari tarayıcısı üzerinde çalışmaz.
> Safari 'de, örnek Web sayfasının bir Web sunucusunda barındırılması gerekir; Safari, yerel bir dosyadan yüklenen Web sitelerinin mikrofonu kullanmasına izin vermez.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Web sunucusu aracılığıyla örnek derleme ve çalıştırma

Uygulamanızı başlatmak için, sık kullandığınız bir web tarayıcısını açın klasörü barındırdığınız ortak URL’ye işaret edin, [bölgenizi](regions.md) girin ve mikrofonu kullanarak bir tanıma tetikleyin. Yapılandırılırsa, belirteç kaynağınızdan bir belirteç alır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da JavaScript örneklerini keşfet](https://aka.ms/csspeech/samples)
