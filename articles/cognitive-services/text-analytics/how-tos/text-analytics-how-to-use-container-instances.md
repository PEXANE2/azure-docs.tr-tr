---
title: Azure Kapsayıcı Örneklerini Çalıştır - Metin Analizi
titleSuffix: Azure Cognitive Services
description: Metin analizi kapsayıcılarını Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e4b61c6fe2f62745d0f5268221cbb5c84803eb10
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876476"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>Azure Kapsayıcı Örneklerine Metin Analizi kapsayıcısı dağıtma

Bilişsel Hizmetler [Metin Analizi][install-and-run-containers] kapsayıcısını Azure [Kapsayıcı Örneklerine][container-instances]nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Metin Analizi kaynağının oluşturulmasına, ilişkili bir Sentiment Analysis görüntüsünün oluşturulmasına ve ikisinin bir tarayıcıdan bu orkestrasyonunu uygulama yeteneğini örneklemektedir. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği kullanın. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

#### <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla [Bilişsel Hizmet Kapsayıcısı](../../cognitive-services-container-support.md) Kullanın
* Metin [Analizi ne bağlı servisini](../vs-text-connected-service.md) kullanın

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances