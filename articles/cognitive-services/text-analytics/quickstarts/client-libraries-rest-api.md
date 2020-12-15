---
title: 'Hızlı başlangıç: Metin Analizi istemci kitaplığını kullanarak metin madenciliği'
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'deki Metin Analizi API'si kullanarak yaklaşım analizini ve daha fazlasını gerçekleştirmek için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 12/02/2020
ms.author: aahi
keywords: metin araştırma, yaklaşım analizi, metin analizi
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: a1eba2f599a0365f43707ff7c9035d91e8637481
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505826"
---
# <a name="quickstart-use-the-text-analytics-client-library-and-rest-api"></a>Hızlı başlangıç: Metin Analizi istemci kitaplığı ve REST API kullanın

Metin Analizi istemci kitaplığı ve REST API kullanmaya başlamak için bu makaleyi kullanın. Araştırma metni için örnek kodu denemek için aşağıdaki adımları izleyin:

* Yaklaşım analizi
* Görüşün madenciliği
* Dil algılama
* Varlık tanıma
* Kişisel tanımlama bilgisi tanıma
* Anahtar ifade ayıklama


::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Metin Analizi API'si en son kararlı sürümü `3.0` .
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.
> * Durum veya zaman uyumsuz işlemler için metin analizi kullanmak istiyorsanız, [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) veya [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) için GitHub 'daki örneklere bakın

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Metin Analizi API'si en son kararlı sürümü `3.0` .
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.
Durum veya zaman uyumsuz işlemler için metin analizi kullanmak istiyorsanız, [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) veya [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) için GitHub 'daki örneklere bakın

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Metin Analizi API'si en son kararlı sürümü `3.0` .
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.
> * Metin Analizi istemci kitaplığı 'nın bu sürümünü [tarayıcınızda](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md)de çalıştırabilirsiniz.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Metin Analizi API'si en son kararlı sürümü `3.0` .
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın. Durum veya zaman uyumsuz işlemler için metin analizi kullanmak istiyorsanız, [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) veya [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) için GitHub 'daki örneklere bakın

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="rest-api"

> [!IMPORTANT]
> * Metin Analizi API'si en son kararlı sürümü `3.0` .
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çözüm keşfet](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algıla](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
