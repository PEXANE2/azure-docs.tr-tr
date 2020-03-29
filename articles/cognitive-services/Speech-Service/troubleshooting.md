---
title: Konuşma SDK sorun giderme - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, Konuşma SDK'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanıyor.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: jhakulin
ms.openlocfilehash: 421b9adf4ae5d2c641484e646bea096716d46cca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74815422"
---
# <a name="troubleshoot-the-speech-sdk"></a>Konuşma SDK’sı sorunlarını giderme

Bu makalede, Konuşma SDK'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanıyor.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Hata: WebSocket Yükseltme kimlik doğrulama hatası (403) ile başarısız oldu

Bölgeniz veya hizmetiniz için yanlış bitiş noktasına sahip olabilirsiniz. Doğru olduğundan emin olmak için URI'yi kontrol edin.

Ayrıca, abonelik anahtarınız veya yetkilendirme belirtecinizde bir sorun olabilir. Daha fazla bilgi için sonraki bölüme bakın.

## <a name="error-http-403-forbidden-or-http-401-unauthorized"></a>Hata: HTTP 403 Yasak veya HTTP 401 Yetkisiz

Bu hata genellikle kimlik doğrulama sorunları neden olur. Geçerli `Ocp-Apim-Subscription-Key` veya `Authorization` üstbilgisiz bağlantı istekleri 403 veya 401 durumuyla reddedilir.

* Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız, aşağıdakiler için hatayı görebilirsiniz:

    - Abonelik anahtarı eksik veya geçersiz
    - Aboneliğinizin kullanım kotası aştınız

* Kimlik doğrulaması için bir yetkilendirme belirteci kullanıyorsanız, aşağıdakiler için hatayı görebilirsiniz:

    - Yetkilendirme belirteci geçersizdir
    - Yetkilendirme belirteci süresi doldu

### <a name="validate-your-subscription-key"></a>Abonelik anahtarınızı doğrulayın

Aşağıdaki komutlardan birini çalıştırarak geçerli bir abonelik anahtarınız olduğunu doğrulayabilirsiniz.

> [!NOTE]
> `YOUR_SUBSCRIPTION_KEY` Değiştirin `YOUR_REGION` ve kendi abonelik anahtarınızı ve ilişkili bölgenizi değiştirin.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

Geçerli bir abonelik anahtarı girdiyseniz, komut bir yetkilendirme jetonu döndürür, aksi takdirde bir hata döndürülür.

### <a name="validate-an-authorization-token"></a>Yetkilendirme belirteci doğrulama

Kimlik doğrulaması için bir yetkilendirme belirteci kullanıyorsanız, yetkilendirme belirteci hala geçerli olduğunu doğrulamak için aşağıdaki komutlardan birini çalıştırın. Jetonlar 10 dakika geçerlidir.

> [!NOTE]
> Önceden `YOUR_AUDIO_FILE` kaydedilmiş ses dosyanıza giden yol ile değiştirin. Önceki `YOUR_ACCESS_TOKEN` adımda döndürülen yetkilendirme belirteciile değiştirin. Doğru `YOUR_REGION` bölgeyle değiştirin.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'

    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }

    # Read audio into byte array.
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

    # Show the result.
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

Geçerli bir yetkilendirme belirteci girdiyseniz, komut ses dosyanızın transkripsiyonunu döndürür, aksi takdirde bir hata döndürülür.

---

## <a name="error-http-400-bad-request"></a>Hata: HTTP 400 Kötü İstek

Bu hata genellikle istek gövdesi geçersiz ses verileri içerdiğinde oluşur. Yalnızca WAV biçimi desteklenir. Ayrıca, için uygun değerleri belirttiğinden emin olmak için `Content-Type` `Content-Length`isteğin üstbilgilerini kontrol edin ve .

## <a name="error-http-408-request-timeout"></a>Hata: HTTP 408 İstek Zaman Dilimi

Hizmete ses verisi gönderilmediği için hata büyük olasılıkla oluşur. Bu hata, ağ sorunları ndan da kaynaklanabilir.

## <a name="recognitionstatus-in-the-response-is-initialsilencetimeout"></a>Yanıtta "RecognitionStatus" "InitialSilenceTimeout"

Bu sorun genellikle ses verilerinden kaynaklanır. Bu hatayı görebilirsiniz, çünkü:

* Sesin başında uzun bir sessizlik var. Bu durumda, hizmet tanımayı birkaç saniye sonra `InitialSilenceTimeout`durdurur ve döndürür.

* Ses, ses verilerinin sessizlik olarak ele alınmasına neden olan desteklenmeyen bir codec biçimi kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirme](releasenotes.md)
