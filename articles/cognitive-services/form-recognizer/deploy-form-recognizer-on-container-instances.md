---
title: Azure Kapsayıcı Örneklerinde Form Tanıyıcı kapsayıcısını çalıştırma
titleSuffix: Azure Cognitive Services
description: Form Tanıyıcı kapsayıcısını bir Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: dapine
ms.openlocfilehash: 9a3456eb4d30aa8d163488f558b571dd97c73bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605110"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Form Tanıyıcı kapsayıcısını Azure Kapsayıcı Örneklerine dağıtma

Bilişsel Hizmetler [Formu Tanıyıcı](form-recognizer-container-howto.md) kapsayıcısını Azure [Kapsayıcı Örneklerine](https://docs.microsoft.com/azure/container-instances/)nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Azure Form Tanıyıcı kaynağının oluşturulmasını gösterir. Sonra ilişkili konteyner görüntü çekme tartışmak. Son olarak, bir tarayıcıdan iki orkestrasyon egzersiz yeteneğini vurgulamak. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

> [!IMPORTANT]
> Form Tanıyıcı kapsayıcıları şu anda Form Tanıyıcı API sürümü 1.0 kullanın. Bunun yerine yönetilen hizmeti kullanarak API'nin en son sürümüne erişebilirsiniz.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Öncelikle kapsayıcıya erişim isteği nde bulunmak için [Bilişsel Hizmetler Formu TanıYıcı Konteynerleri erişim istek formunu](https://aka.ms/FormRecognizerContainerRequestAccess) doldurmanız ve göndermeniz gerekir. Bunu yapmak da Computer Vision için kaydolur. Bilgisayar Lı Vizyon istek formuna ayrıca kaydolmanız gerekmez. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
