---
title: Azure Container Instances 'de form tanıyıcı kapsayıcısını Çalıştır
titleSuffix: Azure Cognitive Services
description: Form tanıyıcı kapsayıcısını bir Azure Container örneğine dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: aahi
ms.openlocfilehash: de81fcb5ee62f9b295c93132b271507c040af46a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86512800"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Form tanıyıcı kapsayıcısını Azure Container Instances dağıtma

[!INCLUDE [Form Recognizer containers limit](includes/container-limit.md)]

Bilişsel Hizmetler [formu tanıyıcı](form-recognizer-container-howto.md) kapsayıcısını Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/)dağıtma hakkında bilgi edinin. Bu yordamda, bir Azure form tanıyıcı kaynağının oluşturulması gösterilmektedir. Ardından, ilişkili kapsayıcı görüntüsünü çekme hakkında tartışın. Son olarak, bir tarayıcıdan ikisini de düzenleme özelliğini vurgulayacağız. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
