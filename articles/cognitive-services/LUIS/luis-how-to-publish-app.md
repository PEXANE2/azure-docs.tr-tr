---
title: App-LUSıS Yayımla
titleSuffix: Azure Cognitive Services
description: Oluşturma ve etkin LUIS uygulamanızı test etme bitirdikten sonra istemci uygulamanız için kullanılabilir uç noktaya yayımlayarak kolaylaştırır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: diberry
ms.openlocfilehash: f92776072038c5684e9334d2dda1690ebb7bcaa8
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70257806"
---
# <a name="publish-your-active-trained-app-to-a-staging-or-production-endpoint"></a>Etkin, eğitilen uygulamanızı bir hazırlama veya üretim uç noktasına yayımlayın

Oluşturma ve etkin LUIS uygulamanızı test etme bitirdikten sonra istemci uygulamanız için kullanılabilir uç noktaya yayımlayarak kolaylaştırır. 

<a name="publish-your-trained-app-to-an-http-endpoint"></a>

## <a name="publishing"></a>Yayımlama

1. Uç noktaya yayımlamak için seçin **Yayımla** üst, sağ paneli. 

    ![En üstte Yayımla düğmesi, sağ gezinti çubuğu](./media/luis-how-to-publish-app/publish-top-nav-bar.png)

1. Yayımlanan tahmin uç noktası için ayarlarınızı seçip **Yayımla**' yı seçin.

    ![Yayınlama Ayarları ' nı ve ardından Yayınla düğmesini seçin](./media/luis-how-to-publish-app/publish-pop-up.png)

### <a name="publishing-slots"></a>Yayımlama Yuvaları

Açılır pencere görüntülendiğinde doğru yuvayı seçin: 

* Hazırlık.
* Üretiminden. 

Bu, yayımlama yuvalarını iki farklı uç noktada veya aynı sürüme sahip olmak üzere, hem yayımlama yuvaları hem de uygulamanızın iki farklı sürümünün kullanılabilmesini sağlar. 

### <a name="publishing-regions"></a>Yayımlama bölgeleri

Uygulama, Lue portalında eklenen LUSıS tahmin uç noktası kaynaklarıyla ilişkili tüm bölgelerde yayımlanır. 

Örneğin, [www.Luis.ai](https://www.luis.ai)üzerinde oluşturulan bir uygulama için, iki bölgede bir Luo kaynağı oluşturursanız, **westus** ve **eastus**ve bunları uygulamaya kaynak olarak eklerseniz, uygulama her iki bölgede da yayımlanır. LUIS bölgeleri hakkında daha fazla bilgi için bkz. [bölgeleri](luis-reference-regions.md).


## <a name="configuring-publish-settings"></a>Yayımlama ayarlarını yapılandırma

Yuvayı seçtikten sonra, yayımlama ayarlarını şu şekilde yapılandırın:

* Yaklaşım analizi
* Yazım denetimi düzeltmesi
* Konuşma primi 

Yayımladıktan sonra bu ayarlar, bölümün **Yayımlama ayarlarını** **Yönet** sayfasından gözden geçirilmek üzere kullanılabilir. Ayarları her Yayımla ile değiştirebilirsiniz. Bir yayımlamayı iptal ederseniz, yayımlama sırasında yaptığınız tüm değişiklikler de iptal edilir. 

### <a name="when-your-app-is-published"></a>Uygulamanız yayımlandığında

Uygulamanız başarıyla yayımlandığında bir yeşil bir başarı bildirim tarayıcı üst kısmında görüntülenir. Yeşil bildirim çubuğu, uç noktalarına bağlantıyı da içerir. 

![Uç nokta bağlantısı ile açılır pencere yayımlanıyor](./media/luis-how-to-publish-app/publish-success.png)

Uç nokta URL'sini gerekiyorsa, bağlantıyı seçin. Üst menüden **Yönet** ' i seçip sol menüden **Azure kaynakları** ' nı seçerek uç nokta URL 'lerine de ulaşabilirsiniz. 

## <a name="sentiment-analysis"></a>Yaklaşım analizi

<a name="enable-sentiment-analysis"></a>

Yaklaşım Analizi ile tümleştirmek LUIS sağlayan [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) yaklaşımını ve anahtar tümcecik analiz sağlamak için. 

Metin analizi anahtarı belirtmeniz gerekmez ve Azure hesabınızda bu hizmet için fatura ücret alınmaz. 

Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri. , `positive` `neutral`Ve 'ninyaklaşımetiketidesteklenenkültürbaşına.`negative` Şu anda yalnızca Ingilizce yaklaşım etiketlerini destekler. 

Yaklaşım Analizi ile JSON uç yanıtı hakkında daha fazla bilgi için bkz. [yaklaşım analizi](luis-concept-data-extraction.md#sentiment-analysis)

## <a name="spelling-correction"></a>Yazım denetimi düzeltmesi

, Azın Kullanıcı söylenişi tahminiyle önce yazım için düzeltmeler yapılır. Yanıtta yazım denetimi de dahil olmak üzere, özgün yazıdaki tüm değişiklikleri görebilirsiniz.

## <a name="speech-priming"></a>Konuşma primi

Konuşma primi, metin konuşmaya dönüştürme işleminden önce, konuşma Hizmetleri için LUSıS modelinin gönderilmesini kullanma sürecinden oluşur. Bu, konuşma hizmetinin modelinize daha doğru konuşma dönüştürmesi sağlamasına izin verir. Bu, bir konuşma araması yaparak ve bir LUO yanıtı geri alarak tek bir çağrıda botu ve LUSıS isteklerini ve yanıtlarını sağlar. Genel olarak daha az gecikme sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [anahtarları Yönet](./luis-how-to-azure-subscription.md) anahtarları için Azure aboneliği ekleme LUIS anahtarına ve Bing yazım denetimi yapma anahtar ve tüm hedefleri sonuçlarına dahil.
* Bkz: [eğitme ve uygulamanızı test](luis-interactive-test.md) yayımlanan uygulamanızı test konsolunda test etmek yönergeler.

