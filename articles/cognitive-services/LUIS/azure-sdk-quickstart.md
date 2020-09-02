---
title: 'Hızlı başlangıç: Language Understanding (LUSıS) SDK istemci kitaplıkları'
description: Bu hızlı başlangıçta C#, Python veya JavaScript kullanarak lusıs SDK 'Sı istemci kitaplıkları ile bir LUO uygulaması oluşturun ve sorgulayın.
ms.topic: quickstart
ms.date: 09/01/2020
keywords: Azure, yapay zeka, AI, doğal dil işleme, NLP, doğal dil anlama, NLU, AI konuşması, konuşma sohbet botu, sohbet botu Maker, LUO, NLP AI, lusıs AI, Azure LUL, doğal dil anlama
ms.custom: devx-track-python, devx-track-javascript, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-diberry-3core
ms.openlocfilehash: 6bcdca85125d44475fadfd195c1dfda88f761f88
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323264"
---
# <a name="quickstart-language-understanding-luis-sdk-client-libraries-to-create-and-query-your-luis-app"></a>Hızlı başlangıç: Language Understanding (LUSıS) SDK istemci kitaplıkları LUSıS uygulamanızı oluşturup sorgulama

Bu hızlı başlangıçta C#, Python veya JavaScript kullanarak lusıs SDK 'Sı istemci kitaplıkları ile bir LUO uygulaması oluşturun ve sorgulayın.

Language Understanding (LUU), bir kullanıcının konuşma, doğal dil metnine, genel anlamı tahmin etmek ve ilgili ayrıntılı bilgileri almak için özel makine öğrenimi zekası uygulamanıza olanak sağlar.

* **AUTHORING SDK** istemci KITAPLıĞı, lusıs uygulamanızı oluşturmanıza, düzenlemenize, eğmenize ve yayımlamanıza olanak sağlar. * **Tahmin çalışma zamanı SDK 'sı** istemci kitaplığı, yayımlanan uygulamayı sorgulamanızı sağlar.

::: zone pivot="programming-language-csharp"
[!INCLUDE [LUIS development with C# SDK](./includes/sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [LUIS development with Node.js SDK](./includes/sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [LUIS development with Python SDK](./includes/sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamayı [Luo portalından](https://www.luis.ai) silebilir ve Azure kaynaklarını [Azure Portal](https://portal.azure.com/)silebilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

* İstemci kitaplığı kimlik doğrulaması hataları, genellikle yanlış anahtar & uç noktasının kullanıldığını gösterir. Bu hızlı başlangıç, tahmin çalışma zamanı için yazma anahtarını ve uç noktayı kullanır, ancak bu işlem yalnızca aylık kotayı henüz kullanmadıysanız çalışır. Yazma anahtarını ve uç noktayı kullanamıyoruz, tahmin çalışma zamanı SDK 'Sı istemci kitaplığına erişirken tahmin çalışma zamanı anahtarını ve uç noktasını kullanmanız gerekir.
* Varlık oluşturma-Bu öğreticide kullanılan iç içe makine öğrenme varlığını oluştururken bir hata alırsanız, kodu kopyaladığınızdan ve farklı bir varlık oluşturmak için kodu değiştirmediğinizden emin olun.
* Örnek oluşturma-Bu öğreticide kullanılan etiketli örneği oluştururken bir hata alırsanız, kodu kopyaladığınızdan ve farklı etiketlenmiş bir örnek oluşturmak için kodu değiştirmediğinizden emin olun.
* Eğitim-bir eğitim hatası alırsanız, bu genellikle boş bir uygulamayı (örnek bir amaç olmadan) veya yanlış biçimlendirilmiş amaçlar veya varlıklar içeren bir uygulamayla gösterir.
* Çeşitli hatalar-kod, istemci kitaplıklarına metin ve JSON nesneleriyle çağrı yaptığından kodu değiştirmemiş olduğunuzdan emin olun.

Diğer hatalar: Yukarıdaki listede kapsanmayan bir hata alırsanız, bu sayfanın en altında geri bildirim vererek bize bilgi verin. Yüklediğiniz istemci kitaplıklarının programlama dilini ve sürümünü ekleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Language Understanding (LUSıS) API nedir?](what-is-luis.md)
* [Yenilikler nelerdir?](whats-new.md)
* [Amaçlar](luis-concept-intent.md), [varlıklar](luis-concept-entity-types.md)ve [örnek](luis-concept-utterance.md)bildirimler ve [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code)' da bulunabilir.
