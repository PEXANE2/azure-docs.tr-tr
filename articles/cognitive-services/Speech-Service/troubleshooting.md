---
title: Konuşma SDK-konuşma hizmeti sorunlarını giderme
titleSuffix: Azure Cognitive Services
description: Bu makalede, konuşma SDK 'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanmaktadır.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: jhakulin
ms.openlocfilehash: 421b9adf4ae5d2c641484e646bea096716d46cca
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74815422"
---
# <a name="troubleshoot-the-speech-sdk"></a>Konuşma SDK’sı sorunlarını giderme

Bu makalede, konuşma SDK 'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanmaktadır.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Hata: WebSocket yükseltmesi bir kimlik doğrulama hatasıyla başarısız oldu (403)

Bölgeniz veya hizmetiniz için yanlış uç noktaya sahip olabilirsiniz. Doğru olduğundan emin olmak için URI 'yi denetleyin.

Ayrıca, abonelik anahtarınız veya yetkilendirme belirtecinizle ilgili bir sorun olabilir. Daha fazla bilgi için sonraki bölüme bakın.

## <a name="error-http-403-forbidden-or-http-401-unauthorized"></a>Hata: HTTP 403 yasaklanmış veya HTTP 401 Yetkisiz

Bu hata genellikle kimlik doğrulama sorunlarından kaynaklanır. Geçerli `Ocp-Apim-Subscription-Key` veya `Authorization` üst bilgi içermeyen bağlantı istekleri, 403 veya 401 durumuyla reddedilir.

* Kimlik doğrulaması için bir abonelik anahtarı kullanıyorsanız şu hata nedeniyle hatayı görebilirsiniz:

    - Abonelik anahtarı eksik veya geçersiz
    - Aboneliğinizin kullanım kotasını aştınız

* Kimlik doğrulaması için bir yetkilendirme belirteci kullanıyorsanız şu hata nedeniyle hatayı görebilirsiniz:

    - Yetkilendirme belirteci geçersiz
    - Yetkilendirme belirtecinin geçerliliği zaman aşımına uğradı

### <a name="validate-your-subscription-key"></a>Abonelik anahtarınızı doğrulayın

Aşağıdaki komutlardan birini çalıştırarak geçerli bir abonelik anahtarınız olduğunu doğrulayabilirsiniz.

> [!NOTE]
> Ve `YOUR_SUBSCRIPTION_KEY` ' `YOUR_REGION` i kendi abonelik anahtarınızı ve ilişkili bölge ile değiştirin.

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

Geçerli bir abonelik anahtarı girdiyseniz, komut bir yetkilendirme belirteci döndürür, aksi takdirde bir hata döndürülür.

### <a name="validate-an-authorization-token"></a>Yetkilendirme belirtecini doğrulama

Kimlik doğrulaması için bir yetkilendirme belirteci kullanıyorsanız, yetkilendirme belirtecinin hala geçerli olduğunu doğrulamak için aşağıdaki komutlardan birini çalıştırın. Belirteçler 10 dakika için geçerlidir.

> [!NOTE]
> Önceden `YOUR_AUDIO_FILE` kaydedilmiş ses dosyasının yoluyla değiştirin. Önceki `YOUR_ACCESS_TOKEN` adımda döndürülen yetkilendirme belirteciyle değiştirin. Doğru `YOUR_REGION` bölge ile değiştirin.

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

Geçerli bir yetkilendirme belirteci girdiyseniz, komut ses dosyanız için dökümü döndürür, aksi takdirde bir hata döndürülür.

---

## <a name="error-http-400-bad-request"></a>Hata: HTTP 400 Hatalı Istek

Bu hata genellikle istek gövdesi geçersiz ses verileri içerdiğinde oluşur. Yalnızca WAV biçimi desteklenir. Ayrıca, ve `Content-Type` `Content-Length`için uygun değerleri belirttiğinizden emin olmak için isteğin üst bilgilerini denetleyin.

## <a name="error-http-408-request-timeout"></a>Hata: HTTP 408 Istek zaman aşımı

Büyük olasılıkla hata, hizmete hiçbir ses verisi gönderilmediğinden meydana gelir. Bu hataya ağ sorunları da neden olmuş olabilir.

## <a name="recognitionstatus-in-the-response-is-initialsilencetimeout"></a>Yanıtta "Recognıtionstatus" değeri "InitialSilenceTimeout"

Bu sorun genellikle ses verilerinden kaynaklanır. Şu hata nedeniyle karşılaşabilirsiniz:

* Sesin başlangıcında sessizlik uzun bir şekilde uzatılacağınızdır. Bu durumda, hizmet birkaç saniye sonra tanımayı sonlandırır ve döndürür `InitialSilenceTimeout`.

* Ses, ses verilerinin sessizlik olarak işlenmesine neden olan desteklenmeyen bir codec biçimi kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirin](releasenotes.md)
