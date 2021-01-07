---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: a752b723980c45e1488f1471ee8139190853b54c
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97966666"
---
Bu hızlı başlangıçta, bir mikrofondan yakalanan ses verilerinden amaçları tanımak için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) ve Language UNDERSTANDING (Luu) hizmetini kullanacaksınız. Özellikle, konuşmayı yakalamak için konuşma SDK 'sını ve bir ışığı açma ve kapatma gibi LUYA 'ın giriş Otomasyonu amaçlarını belirlemek için de bir ön derlenmiş etki alanı kullanacaksınız. 

Birkaç önkoşulu karşıladıktan sonra, konuşma ve tanımlama amaçlarını bir mikrofondan tanımayı yalnızca birkaç adım sürer:

> [!div class="checklist"]
>
> * `SpeechConfig`Abonelik anahtarınızdan ve bölgenizde bir nesne oluşturun.
> * `IntentRecognizer`Yukarıdaki nesneyi kullanarak bir nesne oluşturun `SpeechConfig` .
> * Nesnesini kullanarak `IntentRecognizer` , tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ' i inceleyin `IntentRecognitionResult` .

> [!NOTE]
> FromEndpoint yöntemine bir uç nokta URL 'SI geçirerek bir LanguageUnderstandingModel oluşturabilirsiniz.
> Konuşma SDK 'Sı yalnızca LUSıS v 2.0 uç noktalarını destekler ve LUıN v 2.0 uç noktaları şu iki desenden birini her zaman izler:
> * `https://{AzureResourceName}.cognitiveservices.azure.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
> * `https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
