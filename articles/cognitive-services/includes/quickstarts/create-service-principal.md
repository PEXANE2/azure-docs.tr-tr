---
title: Azure hizmet sorumlusu oluşturma
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/01/2020
ms.author: pafarley
ms.openlocfilehash: 99487fe962228f775582aa2f0a6fb60fe52862ac
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95098086"
---
## <a name="create-an-azure-service-principal"></a>Azure hizmet sorumlusu oluşturma

Uygulamanızın Azure hesabınızla etkileşime geçmesini sağlamak için izinleri yönetmek üzere bir Azure hizmet sorumlusu gerekir. [Azure hizmet sorumlusu oluşturma](/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0)bölümündeki yönergeleri izleyin.

Bir hizmet sorumlusu oluşturduğunuzda gizli bir değer, bir KIMLIK ve bir uygulama KIMLIĞI olduğunu görürsünüz. Sonraki adımlar için uygulama KIMLIĞI ve gizli anahtarı geçici bir konuma kaydedin.