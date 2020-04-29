---
title: 'Hızlı başlangıç: Metin Analizi istemci kitaplığı v3 | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Azure bilişsel hizmetler 'den Metin Analizi API'si bağlamak için bu hızlı başlangıcı kullanın.
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
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80240113"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Hızlı başlangıç: Metin Analizi istemci kitaplığını kullanma

Metin Analizi istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

Şunları yapmak için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar ifade ayıklama

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Metin Analizi API'si `3.0-preview`en son önizleme sürümü, geliştirilmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış varlık tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı sürüm `2.1`.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Bu hızlı başlangıç yalnızca, iyileştirilmiş `3.0-preview` [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış varlık tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel bir önizleme içeren metin analizi istemci kitaplığı sürümü içindir.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Metin Analizi API'si `3.0-preview`en son önizleme sürümü, geliştirilmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış varlık tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı sürüm `2.1`.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.
> * Metin Analizi istemci kitaplığı 'nın bu sürümünü [tarayıcınızda](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md)de çalıştırabilirsiniz.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Metin Analizi API'si `3.0-preview`en son önizleme sürümü, geliştirilmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış varlık tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içerir. En son kararlı sürüm `2.1`.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın. 

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-other"

## <a name="additional-language-support"></a>Ek dil desteği

Bu sekmeye tıkladıysanız, büyük olasılıkla en sevdiğiniz programlama dilinde bir hızlı başlangıç görmezsiniz. Endişelenmeyin, kullanılabilir ek hızlı başlangıçlara ihtiyacımız var. Programlama dilinizin doğru örneğini bulmak için tabloyu kullanın.

| Dil | Kullanılabilir sürüm | 
|----------|------------------------|
| Ruby     | [Sürüm 2,1](ruby-sdk.md) | 
| Başlayın       | [Sürüm 2,1](go-sdk.md) | 

::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çözüm keşfet](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Metin Analizine genel bakış](../overview.md)
* [Yaklaşım analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Varlık tanıma](../how-tos/text-analytics-how-to-entity-linking.md)
* [Dili algılama](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Dil tanıma](../how-tos/text-analytics-how-to-language-detection.md)
