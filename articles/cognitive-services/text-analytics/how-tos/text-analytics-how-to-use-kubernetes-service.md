---
title: Azure Kubernetes Hizmetini Çalıştırın - Metin Analizi
titleSuffix: Azure Cognitive Services
description: Text Analytics kapsayıcı görüntüsünü Azure Kubernetes Hizmeti'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 3264ec5a83277e6bb4befad46cd1337175e911c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383469"
---
# <a name="deploy-a-text-analytics-container-to-azure-kubernetes-service"></a>Azure Kubernetes Hizmetine Metin Analizi kapsayıcısı dağıtma

Azure Bilişsel Hizmetler Metin [Analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) kapsayıcı görüntüsünü Azure Kubernetes Hizmetine (AKS) nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Metin Analizi kaynağının nasıl oluşturulup oluşturulabildiğini, ilişkili bir duygu analizi görüntüsünün nasıl oluşturulturaz olduğunu ve bir tarayıcıdan ikisinin bu orkestrasyonunun nasıl uygulanış ını gösterir. Kapsayıcıları kullanmak dikkatinizi altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırmaya kaydırabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu yordam, yüklenmesi ve yerel olarak çalıştırılması gereken çeşitli araçlar gerektirir. Azure Bulut Su Bulutu'nu kullanmayın. Aşağıdakileri yapmanız gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* Bir metin editörü, örneğin, [Visual Studio Code](https://code.visualstudio.com/download).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yüklendi.
* [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) yüklendi.
* Doğru fiyatlandırma katmanına sahip bir Azure kaynağı. Tüm fiyatlandırma katmanları bu kapsayıcıyla çalışmaz:
    * **Yalnızca** F0 veya standart fiyatlandırma katmanlarına sahip Azure Metin Analizi kaynağı.
    * S0 fiyatlandırma katmanına sahip **Azure Bilişsel Hizmetler** kaynağı.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

#### <a name="key-phrase-extraction"></a>[Anahtar İfade Ayıklama](#tab/keyphrase)

[!INCLUDE [Key Phrase Extraction Kubernetes config and deploy steps](../includes/key-phrase-extraction-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detection"></a>[Dil Algılama](#tab/language)

[!INCLUDE [Language Detection Kubernetes config and deploy steps](../includes/language-detection-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysis"></a>[Duygusallık Analizi](#tab/sentiment)

[!INCLUDE [Sentiment Analysis Kubernetes config and deploy steps](../includes/sentiment-analysis-kubernetes-config-deploy.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla [Bilişsel Hizmet kapsayıcısı](../../cognitive-services-container-support.md) kullanın
* Metin [Analizi ne bağlı servisini](../vs-text-connected-service.md) kullanın
