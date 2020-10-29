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
ms.openlocfilehash: a5354581be519172c498e57d25510f9fc5c0daa4
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92911265"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Yüz kapsayıcısını Azure Container Instances dağıtım

> [!IMPORTANT]
> Yüz Tanıma v1 kapsayıcısı kullanıcılarının sınırına ulaşıldı. Şu anda Yüz Tanıma kapsayıcısı için yeni uygulama kabul etmiyoruz.

Bilişsel [Hizmetler kapsayıcısı](../face-how-to-install-containers.md) kapsayıcısını Azure [Container Instances](../../../container-instances/index.yml)dağıtma hakkında bilgi edinin. Bu yordamda, bir Azure yüz kaynağı oluşturma işlemi gösterilir. Ardından, ilişkili kapsayıcı görüntüsünü çekme hakkında tartışın. Son olarak, bir tarayıcıdan ikisini de düzenleme özelliğini vurgulayacağız. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]