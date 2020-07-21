---
title: Azure Container Instances 'da yüz kapsayıcısını Çalıştır
titleSuffix: Azure Cognitive Services
description: Yüz kapsayıcısını bir Azure Container örneğine dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 07/16/2020
ms.author: aahi
ms.openlocfilehash: 5fbe316580cfa2ca280a2587536df037146e8d02
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86538131"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Yüz kapsayıcısını Azure Container Instances dağıtım

> [!IMPORTANT]
> Yüz kapsayıcı kullanıcıları için sınıra ulaşıldı. Şu anda yüz kapsayıcısı için yeni uygulamalar kabul etmiyoruz.

Bilişsel [Hizmetler kapsayıcısı](../face-how-to-install-containers.md) kapsayıcısını Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/)dağıtma hakkında bilgi edinin. Bu yordamda, bir Azure yüz kaynağı oluşturma işlemi gösterilir. Ardından, ilişkili kapsayıcı görüntüsünü çekme hakkında tartışın. Son olarak, bir tarayıcıdan ikisini de düzenleme özelliğini vurgulayacağız. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]