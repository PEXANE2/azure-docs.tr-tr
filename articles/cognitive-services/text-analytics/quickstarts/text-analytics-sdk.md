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
ms.date: 01/23/2020
ms.author: aahi
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: 79dd6160b65469d6a44167913adb7e319f0f583a
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77525354"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Hızlı başlangıç: Metin Analizi istemci kitaplığını kullanma

Metin Analizi istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

Şunları yapmak için Metin Analizi istemci kitaplığını kullanın:

* Yaklaşım analizi
* Dil algılama
* Varlık tanıma
* Anahtar tümcecik ayıklama

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Metin Analizi API'si en son önizleme sürümü, Gelişmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içeren `3.0-preview`. En son kararlı sürüm `2.1`.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Bu hızlı başlangıç yalnızca, Gelişmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içeren Metin Analizi istemci kitaplığının sürüm `3.0-preview` içindir.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Metin Analizi API'si en son önizleme sürümü, Gelişmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içeren `3.0-preview`. En son kararlı sürüm `2.1`.
>    * Yalnızca kullandığınız sürüme ait yönergeleri izlediğinizden emin olun.
> * Bu makaledeki kod, basitlik nedenlerle zaman uyumlu Yöntemler ve güvenli olmayan kimlik bilgileri depolaması kullanır. Üretim senaryolarında, performans ve ölçeklenebilirlik için toplu zaman uyumsuz yöntemleri kullanmanızı öneririz. Aşağıdaki başvuru belgelerine bakın.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Metin Analizi API'si en son önizleme sürümü, Gelişmiş [yaklaşım Analizi](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) ve [adlandırılmış VARLıK tanıma (ner)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)için genel önizleme içeren `3.0-preview`. En son kararlı sürüm `2.1`.
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
| Git       | [Sürüm 2,1](go-sdk.md) | 

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
