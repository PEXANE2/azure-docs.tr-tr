---
title: Azure Container Instances çalıştırma Metin Analizi
titleSuffix: Azure Cognitive Services
description: Metin analizi kapsayıcılarını, yaklaşım Analizi görüntüsüyle Azure Container Instance 'a dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9ef529c9d505e5b305602c80a8dbef906f52269c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552524"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Azure Container Instances Yaklaşım Analizi kapsayıcısını dağıtma

Bilişsel Hizmetler [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) kapsayıcısını Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/)yaklaşım Analizi görüntüsüyle dağıtmayı öğrenin. Bu yordam, bir Metin Analizi kaynağın oluşturulmasını, ilişkili bir Yaklaşım Analizi görüntüsünün oluşturulmasını ve bir tarayıcıdan bu iki düzenleme yapma özelliğini de bir şekilde uygulayabilir. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla bilişsel [Hizmetler kapsayıcısı](../../cognitive-services-container-support.md) kullanın
* [Metin analizi bağlı hizmetini](../vs-text-connected-service.md) kullanma
