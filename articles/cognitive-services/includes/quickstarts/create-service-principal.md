---
title: Azure hizmet sorumlusu oluşturma
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 5cff103d17138608f4932e36444847437f6da236
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89321649"
---
## <a name="create-an-azure-service-principal"></a>Azure hizmet sorumlusu oluşturma

Uygulamanızın Azure hesabınızla etkileşime geçmesini sağlamak için izinleri yönetmek üzere bir Azure hizmet sorumlusu gerekir. [Azure hizmet sorumlusu oluşturma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0)bölümündeki yönergeleri izleyin.

Bir hizmet sorumlusu oluşturduğunuzda gizli bir değer, bir KIMLIK ve bir uygulama KIMLIĞI olduğunu görürsünüz. Sonraki adımlar için uygulama KIMLIĞI ve gizli anahtarı geçici bir konuma kaydedin.