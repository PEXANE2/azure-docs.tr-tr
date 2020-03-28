---
title: 'Quickstart: Text Analytics istemci kitaplığı v3 | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Azure Bilişsel Hizmetler'den Metin Analizi API'sine bağlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: cc3f48ec1113f954336cfae0bda2cba2499d9a1d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80240113"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Hızlı başlangıç: Text Analytics istemci kitaplığını kullanma

Text Analytics istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Gerçekleştirmek için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar ifade ayıklama

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Metin Analizi API en son önizleme `3.0-preview`sürümü, geliştirilmiş Duygu [Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [Adlandırılmış Varlık Tanıma (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı `2.1`sürümü .
>    * Yalnızca kullandığınız sürüm için yönergeleri uyguladığınıza emin olun.
> * Bu makaledeki kod, basitlik nedenleriyle eşzamanlı yöntemler ve güvenli olmayan kimlik bilgileri depolamasını kullanır. Üretim senaryoları için, performans ve ölçeklenebilirlik için toplu asenkron yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Bu hızlı başlangıç yalnızca, geliştirilmiş [Duygu Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [Adlandırılmış Varlık Tanıma (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içeren Text Analytics istemci kitaplığı sürümü `3.0-preview` içindir.
> * Bu makaledeki kod, basitlik nedenleriyle eşzamanlı yöntemler ve güvenli olmayan kimlik bilgileri depolamasını kullanır. Üretim senaryoları için, performans ve ölçeklenebilirlik için toplu asenkron yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Metin Analizi API en son önizleme `3.0-preview`sürümü, geliştirilmiş Duygu [Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [Adlandırılmış Varlık Tanıma (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı `2.1`sürümü .
>    * Yalnızca kullandığınız sürüm için yönergeleri uyguladığınıza emin olun.
> * Bu makaledeki kod, basitlik nedenleriyle eşzamanlı yöntemler ve güvenli olmayan kimlik bilgileri depolamasını kullanır. Üretim senaryoları için, performans ve ölçeklenebilirlik için toplu asenkron yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.
> * Metin Analizi istemci kitaplığı bu sürümünü [de tarayıcınızda çalıştırabilirsiniz.](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md)

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Metin Analizi API en son önizleme `3.0-preview`sürümü, geliştirilmiş Duygu [Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [Adlandırılmış Varlık Tanıma (NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı `2.1`sürümü .
>    * Yalnızca kullandığınız sürüm için yönergeleri uyguladığınıza emin olun.
> * Bu makaledeki kod, basitlik nedenleriyle eşzamanlı yöntemler ve güvenli olmayan kimlik bilgileri depolamasını kullanır. Üretim senaryoları için, performans ve ölçeklenebilirlik için toplu asenkron yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın. 

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-other"

## <a name="additional-language-support"></a>Ek dil desteği

Bu sekmeyi tıklattıysanız, büyük olasılıkla en sevdiğiniz programlama dilinde hızlı bir başlangıç görmediniz. Merak etmeyin, biz ek quickstarts mevcuttur. Programlama diliniz için doğru örneği bulmak için tabloyu kullanın.

| Dil | Kullanılabilir sürüm | 
|----------|------------------------|
| Ruby     | [Sürüm 2.1](ruby-sdk.md) | 
| Başlayın       | [Sürüm 2.1](go-sdk.md) | 

::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir çözüm keşfedin](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algılama](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
