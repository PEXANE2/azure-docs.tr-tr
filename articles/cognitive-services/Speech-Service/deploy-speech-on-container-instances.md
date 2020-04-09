---
title: Azure Kapsayıcı Örneklerini Çalıştır - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmet kapsayıcısını bir Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 610d5ce095097a31ee92c67f0112d1657424858e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878757"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>Konuşma hizmet kapsayıcısını Azure Kapsayıcı Örneklerine dağıtma

Bilişsel Hizmetler [Konuşma hizmet](speech-container-howto.md) kapsayıcısını Azure [Kapsayıcı Örnekleri'ne](https://docs.microsoft.com/azure/container-instances/)nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Azure Konuşma hizmeti kaynağının oluşturulmasını gösterir. Sonra ilişkili konteyner görüntü çekme tartışmak. Son olarak, bir tarayıcıdan iki orkestrasyon egzersiz yeteneğini vurgulamak. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Öncelikle kapsayıcıya erişim isteği nde bulunmak için [Bilişsel Hizmetler Konuşma Kapları İstek formunu](https://aka.ms/speechcontainerspreview/) doldurmanız ve göndermeniz gerekir. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create a Speech service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
