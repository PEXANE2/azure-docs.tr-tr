---
title: Azure Kapsayıcı Örneklerinde Bilgisayar Lı Vizyon kapsayıcısı çalıştırma
titleSuffix: Azure Cognitive Services
description: Computer Vision kapsayıcısını bir Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: ecb0b6a6577161a5349d9d701dd6c116f69411f0
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876112"
---
# <a name="deploy-the-computer-vision-container-to-azure-container-instances"></a>Bilgisayar Lı Vizyon kapsayıcısını Azure Kapsayıcı Örneklerine dağıtma

Bilişsel Hizmetler [Bilgisayar Vizyonu](computer-vision-how-to-install-containers.md) kapsayıcısını Azure Kapsayıcı Örneklerine nasıl [dağıtabileceğinizi](https://docs.microsoft.com/azure/container-instances/)öğrenin. Bu yordam, Bilgisayar Lı Vizyon kaynağının oluşturulmasını gösterir. Sonra ilişkili konteyner görüntü çekme tartışmak. Son olarak, bir tarayıcıdan iki orkestrasyon egzersiz yeteneğini vurgulamak. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel konteyner kayıt defterine erişim isteği

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Computer Vision resource](includes/create-computer-vision-resource.md)]

[!INCLUDE [Create the Computer Vision on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]