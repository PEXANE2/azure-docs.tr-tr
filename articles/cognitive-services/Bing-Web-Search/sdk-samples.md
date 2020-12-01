---
title: Bing Web Araması SDK örnekleri
titleSuffix: Azure Cognitive Services
description: Python, Node.js, C# veya Java uygulamanıza arama özellikleri eklemek için Bing Web Araması SDK’sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: sample
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: f13250bb2d6829cf7f1bf3dd0417974b810a36fe
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96350441"
---
# <a name="bing-web-search-sdk-samples"></a>Bing Web Araması SDK örnekleri

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Web Araması SDK’sı; Python, Node.js, C# ve Java dilinde kullanılabilir. GitHub’da kod örnekleri, önkoşullar ve derleme yönergeleri sağlanır. Aşağıdaki senaryolar ele alınmaktadır:

* Tek bir sözcüğü sorgulayın ve web sayfaları, görüntüler, haber makaleleri ve videolar için ilk sonucun adını ve URL’sini yazdırın.
* Bir ifadeyi sorgulayın, sonuç sayısını doğrulayın ve birinci sonucun adını ve URL’sini yazdırın.
* Yanıt filtreleri `news` olarak ayarlanmış şekilde bir arama terimini sorgulayın ve haber sonuçlarının ayrıntılarını yazdırın.
* `answerCount` ve `promote` parametreleri ile bir arama terimini sorgulayın ve sonra sonuçların ayrıntılarını yazdırın.

## <a name="sdks-and-libraries"></a>SDK’lar ve kitaplıklar

Tercih ettiğiniz dil için SDK’ya erişmek amacıyla bu bağlantıları kullanın.

* [Python örneklerini](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) kullanmaya başlama
  * Tanımlar ve bağımlılıklar için ayrıca bkz. [Python kitaplıkları](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-websearch).
* [Node.js örneklerini](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) kullanmaya başlama
  * Ayrıca bkz. [Node.js Web araması](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesWebSearch).
* [.NET örneklerini](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) kullanmaya başlama
  * [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0)
  * Tanımlar ve bağımlılıklar için ayrıca bkz. [.NET kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingWebSearch).
* [Java örneklerini](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) kullanmaya başlama
  * Tanımlar ve bağımlılıklar için ayrıca bkz. [Java kitaplıkları](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingWebSearch).