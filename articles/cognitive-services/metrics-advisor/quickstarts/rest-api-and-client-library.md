---
title: Ölçüm Danışmanı istemci kitaplıkları REST API
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Azure bilişsel hizmetler 'den ölçüm Danışmanı API 'sine bağlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: quickstart
ms.date: 10/14/2020
ms.author: mbullwin
zone_pivot_groups: programming-languages-metrics-monitor
ms.openlocfilehash: 5b3df5f4b41b2beeec68b667863f6ca7715df47b
ms.sourcegitcommit: 7a7b6c7ac0aa9dac678c3dfd4b5bcbc45dc030ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2020
ms.locfileid: "93186873"
---
# <a name="quickstart-use-the-client-libraries-or-rest-apis-to-customize-your-solution"></a>Hızlı başlangıç: çözümünüzü özelleştirmek için istemci kitaplıklarını veya REST API 'Lerini kullanma

Ölçüm Danışmanı REST API veya istemci kitaplıklarını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

Şunları yapmak için ölçüm Danışmanı 'nı kullanın:

* Veri kaynağından veri akışı ekleme
* Alma durumunu denetle
* Algılama ve Uyarıları yapılandırma 
* Anomali algılama sonuçlarını sorgulama
* Bozukluklar tanılayın


::: zone pivot="programming-language-csharp"

[!INCLUDE [REST API quickstart](../includes/quickstarts/csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [REST API quickstart](../includes/quickstarts/java.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [REST API quickstart](../includes/quickstarts/javascript.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [REST API quickstart](../includes/quickstarts/python.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

- [Web portalını kullanma](web-portal.md)
- [Veri akışlarınızı ekleme](../how-tos/onboard-your-data.md)
    - [Veri akışlarını yönetme](../how-tos/manage-data-feeds.md)
    - [Farklı veri kaynakları için yapılandırma](../data-feeds-from-different-sources.md)
- [Ölçümleri yapılandırma ve ince ayar algılama yapılandırması](../how-tos/configure-metrics.md)
- [Geri bildirimi kullanarak anomali algılamayı ayarlama](../how-tos/anomaly-feedback.md)