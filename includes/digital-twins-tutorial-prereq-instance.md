---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek ayarlama için önkoşul
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 5c41f7516cecdb6bbc42a66d118a90986dd7de56
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87827369"
---
### <a name="prepare-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği hazırlama

Bu öğreticiyi tamamlayabilmeniz için, bir **Azure dijital TWINS örneğine** karşı programlama yapmanız gerekir. Önceki çalışmalardan daha önceden ayarlanmış bir Azure dijital TWINS örneğiniz varsa, bu örneği kullanabilirsiniz.

* Aksi takdirde, [*nasıl yapılır: bir örneği ve kimlik doğrulamasını ayarlama*](../articles/digital-twins/how-to-set-up-instance-scripted.md)konusundaki yönergeleri kullanarak **bir örnek ve kimlik doğrulaması ayarlayın** . Yönergeler Ayrıca, her adımı başarıyla tamamladığınızı ve yeni örneğinizi kullanarak üzerine geçmeye başlamaya yönelik adımları da içerir.

Bu öğreticide, örneğinizi ayarlarken aşağıdaki değerlere ihtiyaç duyarsınız. Bu değerleri yeniden toplamanız gerekiyorsa, [Azure Portal](https://portal.azure.com)için kurulum makalesindeki ilgili bölümlere aşağıdaki bağlantıları kullanın.
* Azure dijital TWINS örnek **_adı_** ([portalda bul](../articles/digital-twins/how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure Digital TWINS örnek **_ana bilgisayar adı_** ([portalda bul](../articles/digital-twins/how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure AD uygulama kayıt **_uygulaması (istemci) kimliği_** ([portalda bul](../articles/digital-twins/how-to-set-up-instance-portal.md#collect-important-values))
* Azure AD uygulama kayıt **_dizini (kiracı) kimliği_** ([portalda bul](../articles/digital-twins/how-to-set-up-instance-portal.md#collect-important-values))