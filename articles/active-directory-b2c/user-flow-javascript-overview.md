---
title: JavaScript ve sayfa düzeni sürümleri
titleSuffix: Azure AD B2C
description: JavaScript 'ı etkinleştirme ve Azure Active Directory B2C sayfa düzeni sürümlerini kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/10/2020
ms.custom: project-no-code, devx-track-javascript
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d85ffca8c72e7a247d5bbd1e593335fd7995b594
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87170151"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içindeki JavaScript ve sayfa düzeni sürümleri

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C, Kullanıcı akışlarınızda ve özel ilkelerindeki Kullanıcı arabirimi öğeleri için HTML, CSS ve JavaScript içeren bir paketlenmiş içerik kümesi sağlar.

Uygulamalarınız için JavaScript 'ı etkinleştirmek üzere:

* Azure portal kullanarak Kullanıcı akışında etkinleştirin
* [Sayfa düzeni](page-layout.md) seçin
* İsteklerinizi [b2clogin.com](b2clogin.md) kullanma

[JavaScript](javascript-samples.md) istemci tarafı kodunu etkinleştirmek Istiyorsanız, JavaScript 'i temel alan öğelerin sabit olması gerekir. Sabit olmadıkları takdirde, herhangi bir değişiklik Kullanıcı sayfalarınızda beklenmeyen davranışlara neden olabilir. Bu sorunları engellemek için, bir sayfa düzeni kullanımını zorunlu tutun ve JavaScript 'i temel alan içerik tanımlarının sabit olduğundan emin olmak için bir sayfa düzeni sürümü belirtin. JavaScript 'i etkinleştirmeyi amaçlamadığınız halde, sayfalarınız için bir sayfa düzeni sürümü belirtebilirsiniz.

## <a name="enable-javascript"></a>JavaScript'i etkinleştirme

Kullanıcı akışı **özelliklerinde**JavaScript 'i etkinleştirebilirsiniz. JavaScript 'in etkinleştirilmesi, sayfa düzeninin kullanımını da zorunlu kılar. Daha sonra, sonraki bölümde açıklandığı gibi Kullanıcı akışının sayfa düzeni sürümünü ayarlayabilirsiniz.

![JavaScript ayarlarını etkinleştir seçeneği vurgulanmış şekilde Kullanıcı akışı özellikleri sayfası](media/user-flow-javascript-overview/javascript-settings.png)

## <a name="select-a-page-layout-version"></a>Sayfa düzeni sürümü seçin

Kullanıcı akışınızın özelliklerinde JavaScript 'ı etkinleştirip etkinleştirmeyeceğinizi, Kullanıcı akış sayfalarınız için bir sayfa düzeni sürümü belirtebilirsiniz. Kullanıcı akışını açın ve **sayfa düzenleri**' ni seçin. **Düzen adı**altında, bir Kullanıcı akış sayfası seçin ve **sayfa düzeni sürümünü**seçin.

Farklı sayfa düzeni sürümleri hakkında daha fazla bilgi için bkz. [sayfa düzeni sürümü değişiklik günlüğü](page-layout.md).

![Portalda sayfa düzeni sürüm açılan listesini gösteren sayfa düzeni ayarları](media/user-flow-javascript-overview/page-layout-version.png)

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="next-steps"></a>Sonraki adımlar

JavaScript örneklerindeki JavaScript kullanım örneklerini [Azure Active Directory B2C ' de kullanmak üzere](javascript-samples.md)bulabilirsiniz.
