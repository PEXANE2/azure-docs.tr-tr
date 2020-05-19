---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: dapine
ms.openlocfilehash: 2ac2051bd75bdad550bc93323053db2d220095a2
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82979478"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md)

## <a name="create-a-new-website-folder"></a>Yeni bir Web sitesi klasörü oluşturma

Yeni, boş bir klasör oluşturun. Örneği bir web sunucusunda barındırmak istiyorsanız, web sunucusunun klasöre erişebildiğinden emin olun.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>JavaScript için Konuşma SDK’sını bu klasöre çıkarın

Konuşma SDK’sını [.zip paketi](https://aka.ms/csspeech/jsbrowserpackage) olarak indirin ve yeni oluşturulan klasöre çıkarın. Bu, iki dosya için bir paketi, `microsoft.cognitiveservices.speech.sdk.bundle.js` ve `microsoft.cognitiveservices.speech.sdk.bundle.js.map` .
İkinci dosya isteğe bağlıdır ve SDK kodunda hata ayıklamak için faydalıdır.

## <a name="create-an-indexhtml-page"></a>Bir index.html sayfası oluşturma

Klasörde `index.html` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

1. Aşağıdaki HTML çatısını oluşturun:

 [!code-html [SampleCode](~/samples-cognitive-services-speech-sdk/quickstart/javascript/browser/index-tts.html)]

## <a name="create-the-token-source-optional"></a>Belirteç kaynağı oluşturma (isteğe bağlı)

Web sayfasını bir web sunucusunda barındırmak istiyorsanız, tanıtım uygulamanız için isteğe bağlı olarak bir belirteç kaynağı sağlayabilirsiniz.
Bu şekilde, abonelik anahtarınız hiçbir zaman sunucunuzdan çıkmaz ve kullanıcılar bir yetkilendirme kodu girmeden konuşma özelliklerini kullanabilir.

`token.php` adlı yeni bir dosya oluşturun. Bu örnekte web sunucunuzun PHP betik oluşturma dilini desteklediği varsayılmaktadır. Aşağıdaki kodu girin:

```php
<?php
header('Access-Control-Allow-Origin: ' . $_SERVER['SERVER_NAME']);

// Replace with your own subscription key and service region (e.g., "westus").
$subscriptionKey = 'YourSubscriptionKey';
$region = 'YourServiceRegion';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://' . $region . '.api.cognitive.microsoft.com/sts/v1.0/issueToken');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{}');
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json', 'Ocp-Apim-Subscription-Key: ' . $subscriptionKey));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
echo curl_exec($ch);
?>
```

> [!NOTE]
> Yetkilendirme belirteçlerinin sınırlı bir kullanım ömrü vardır.
> Bu basit örnekte yetkinlendirme belirteçlerinin otomatik olarak yenilenmesi gösterilmez. Kullanıcı olarak, sayfayı el ile yeniden yükleyebilir veya yenilemek için F5’e basabilirsiniz.

## <a name="build-and-run-the-sample-locally"></a>Örneği yerel olarak derleyin ve çalıştırın

Uygulamayı başlatmak için, index.html dosyasına çift tıklayın veya index.html dosyasını sık kullandığınız web tarayıcısıyla açın. Bu, abonelik anahtarınızı ve [bölgenizi](../../../../regions.md) girmenizi ve giriş metninin sentiğini tetiklemeyi sağlayan basıt bir GUI sunacaktır.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Web sunucusu aracılığıyla örnek derleme ve çalıştırma

Uygulamanızı başlatmak için, en sevdiğiniz web tarayıcınızı açın ve klasörü barındırın bulunduğu genel URL 'ye işaret edin, [bölgenizi](../../../../regions.md)girin ve giriş metninde sensit öğesini tetikleyin. Yapılandırılırsa, belirteç kaynağınızdan bir belirteç alır.


## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]