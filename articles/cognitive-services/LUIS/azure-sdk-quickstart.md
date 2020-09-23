---
title: 'Hızlı başlangıç: Language Understanding (LUSıS) SDK istemci kitaplıkları'
description: Bu hızlı başlangıçta C#, Python veya JavaScript kullanarak lusıs SDK 'Sı istemci kitaplıkları ile bir LUO uygulaması oluşturun ve sorgulayın.
ms.topic: quickstart
ms.date: 09/14/2020
keywords: Azure, yapay zeka, AI, doğal dil işleme, NLP, LUO, Azure lusıs, doğal dil anlama, AI sohbet botu, sohbet botu Maker, doğal dili anlama
ms.custom: devx-track-python, devx-track-javascript, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-diberry-3core
ms.openlocfilehash: 64288bbffa2d2c24e2f719477d98426f52da4070
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90972626"
---
# <a name="quickstart-language-understanding-luis-sdk-client-libraries-to-create-and-query-your-luis-app"></a>Hızlı başlangıç: Language Understanding (LUSıS) SDK istemci kitaplıkları LUSıS uygulamanızı oluşturup sorgulama

Bu hızlı başlangıçta C#, Python veya JavaScript kullanarak bir Azure LUSıS yapay zeka (AI) uygulaması oluşturup bu hızlı başlangıç ile sorgulayın.

Language Understanding (LUU), bir kullanıcının konuşma için doğal dil işleme (NLP), genel anlamı tahmin etmek için doğal dil metnine uygulamanıza ve ilgili, ayrıntılı bilgiler almanıza olanak sağlar.

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
* Doğal dili anlama: [doğal dil anlama (NLU) ve doğal dil işleme (NLP)](artificial-intelligence.md)
* Botlar: [AI chatbots](luis-csharp-tutorial-bf-v4.md "sohbet botu Oluşturucusu öğreticisi")
