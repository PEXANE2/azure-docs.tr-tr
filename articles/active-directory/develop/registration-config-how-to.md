---
title: Azure AD uygulama kaydı için uç noktaları al
titleSuffix: Microsoft identity platform
description: Azure AD ile geliştirdiğiniz veya kaydetmekte olduğunuz özel bir uygulama için kimlik doğrulama uç noktalarını bulma.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: ryanwi
ms.openlocfilehash: dc37ce474ad3226f5dbf7e6efd687ddf55f7da03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82926689"
---
# <a name="how-to-discover-endpoints"></a>Uç noktaları bulma

Uygulamanız için kimlik doğrulama uç noktalarını [Azure Portal](https://portal.azure.com)bulabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Azure Active Directory**seçin.
1. **Yönet**' in altında **uygulama kayıtları**' yi seçin ve ardından üst menüdeki **uç noktalar** ' ı seçin.

    Kiracınız için kimlik doğrulama uç noktalarını gösteren **uç noktalar** sayfası görüntülenir.
    
    Uygulamanıza özgü kimlik doğrulama isteğini yapmak için **uygulama (istemci) kimliğiyle** birlikte kullandığınız kimlik doğrulama protokolüyle eşleşen uç noktayı kullanın.

**Ulusal bulutların** (örneğin, Azure AD Çin, Almanya ve ABD kamu) kendi uygulama kayıt portalı ve Azure AD kimlik doğrulama uç noktaları vardır. [Ulusal bulutlara genel bakış](authentication-national-cloud.md)konusunda daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Farklı Azure ortamlarındaki uç noktalar hakkında daha fazla bilgi için bkz. [National bulutlar genel bakış](authentication-national-cloud.md).
