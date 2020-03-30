---
title: JavaScript ve sayfa düzeni sürümleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de JavaScript'i etkinleştirmeyi ve sayfa düzeni sürümlerini nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 23d345ea9f22be5c4dac20e6e8784a8de079bccb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185854"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de JavaScript ve sayfa düzeni sürümleri

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C, kullanıcı akışlarınızdaki ve özel ilkelerinizdeki kullanıcı arabirimi öğeleri için HTML, CSS ve JavaScript içeren bir paket içerik kümesi sağlar.

Uygulamalarınız için JavaScript'i etkinleştirmek için:

* Azure portalını kullanarak kullanıcı akışında etkinleştirme
* Sayfa [düzeni](page-layout.md) seçme
* İsteklerinizde [b2clogin.com](b2clogin.md) kullanın

[JavaScript](javascript-samples.md) istemci tarafı kodunu etkinleştirmek istiyorsanız, JavaScript'inizi temel aldığınız öğeler değişmez olmalıdır. Değişmez değillerse, herhangi bir değişiklik kullanıcı sayfalarınızda beklenmeyen davranışlara neden olabilir. Bu sorunları önlemek için, bir sayfa düzeninin kullanımını zorleyin ve JavaScript'inizi temel aldığınız içerik tanımlarının değişmez olduğundan emin olmak için bir sayfa düzeni sürümü belirtin. JavaScript'i etkinleştirmek istemeseniz bile, sayfalarınız için bir sayfa düzeni sürümü belirtebilirsiniz.

## <a name="enable-javascript"></a>JavaScript'i etkinleştirme

Kullanıcı akışı **Özellikleri'nde**JavaScript'i etkinleştirebilirsiniz. JavaScript'i etkinleştirmek, sayfa düzeninin kullanımını da zorlar. Daha sonra, bir sonraki bölümde açıklandığı gibi kullanıcı akışı için sayfa düzeni sürümünü ayarlayabilirsiniz.

![JavaScript ayarını etkinleştir ile kullanıcı akışı özellikleri sayfası vurgulanır](media/user-flow-javascript-overview/javascript-settings.png)

## <a name="select-a-page-layout-version"></a>Sayfa düzeni sürümü seçme

Kullanıcı akışınızın özelliklerinde JavaScript'i etkinleştirseniz de etkinleştirmeseniz de, kullanıcı akış sayfalarınız için bir sayfa düzeni sürümü belirtebilirsiniz. Kullanıcı akışını açın ve **Sayfa düzenlerini**seçin. **DÜZEN ADI**altında, bir kullanıcı akış sayfası seçin ve **Sayfa Düzeni Sürümünü**seçin.

Farklı sayfa düzeni sürümleri hakkında bilgi için [Sayfa düzeni sürüm değişikliği günlüğüne](page-layout.md)bakın.

![Portaldaki sayfa düzeni ayarları sayfa düzeni sürüm açılır bırakma](media/user-flow-javascript-overview/page-layout-version.png)

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C'de kullanılmak üzere JavaScript örneklerinde JavaScript](javascript-samples.md)kullanım örneklerini bulabilirsiniz.
