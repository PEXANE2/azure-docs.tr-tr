---
title: Azure Container Instances çalıştırma Metin Analizi
titleSuffix: Azure Cognitive Services
description: Metin analizi kapsayıcılarını Azure Container Instance 'a dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: 1dde59456126ce1221bdb71f09957c0fd3609427
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108727"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>Azure Container Instances Metin Analizi kapsayıcısını dağıtma

Bilişsel Hizmetler [metin analizi][install-and-run-containers] kapsayıcısını Azure [Container Instances][container-instances]'a dağıtmayı öğrenin. Bu yordam, bir Metin Analizi kaynağın oluşturulmasını, ilişkili bir Yaklaşım Analizi görüntüsünün oluşturulmasını ve bir tarayıcıdan bu iki düzenleme yapma özelliğini de bir şekilde uygulayabilir. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

#### <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

#### <a name="text-analytics-for-health"></a>[Sistem durumu için Metin Analizi](#tab/health)

[!INCLUDE [Verify the health container instance](../includes/verify-health-container.md)]

***

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla bilişsel [Hizmetler kapsayıcısı](../../cognitive-services-container-support.md) kullanın
* [Metin analizi bağlı hizmetini](../vs-text-connected-service.md) kullanma

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances