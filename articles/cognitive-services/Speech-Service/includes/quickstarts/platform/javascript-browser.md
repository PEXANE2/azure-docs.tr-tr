---
title: "Hızlı başlangıç: JavaScript için konuşma SDK 'Sı (tarayıcı) Platform Kurulumu-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Bunu konuşma hizmeti SDK 'Sı ile JavaScript (tarayıcı) kullanmaya yönelik platformunuzu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/11/2019
ms.author: erhopf
ms.openlocfilehash: fa8bf79c047911ca283bf60261bba0cbdd6816a7
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82980202"
---
Bu kılavuzda, bir Web sayfasıyla kullanılmak üzere JavaScript için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturma

Yeni, boş bir klasör oluşturun. Örneği bir web sunucusunda barındırmak istiyorsanız, web sunucusunun klasöre erişebildiğinden emin olun.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>JavaScript için Konuşma SDK’sını bu klasöre çıkarın

Konuşma SDK’sını [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) olarak indirin ve yeni oluşturulan klasöre çıkarın. Bu, aşağıdaki dört dosyanın dağıtılmasına neden olur:
* `microsoft.cognitiveservices.speech.sdk.bundle.js`Konuşma SDK 'sının okunabilir bir sürümü.
* `microsoft.cognitiveservices.speech.sdk.bundle.js.map`SDK kodunda hata ayıklamak için kullanılan bir eşleme dosyası.
* `microsoft.cognitiveservices.speech.sdk.bundle.d.ts`TypeScript ile kullanılacak nesne tanımları
* `microsoft.cognitiveservices.speech.sdk.bundle-min.js`Konuşma SDK 'sının küçültülmüş bir sürümü.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Klasörde `index.html` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]