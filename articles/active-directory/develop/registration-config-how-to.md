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
ROBOTS: NOINDEX
ms.openlocfilehash: 778523869715916bf1e4c32af59ea7a0081df91b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100103320"
---
# <a name="how-to-discover-endpoints"></a>Uç noktaları bulma

Uygulamanız için kimlik doğrulama uç noktalarını [Azure Portal](https://portal.azure.com)bulabilirsiniz.

1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
1. **Azure Active Directory** seçin.
1. **Yönet**' in altında **uygulama kayıtları**' yi seçin ve ardından üst menüdeki **uç noktalar** ' ı seçin.

    Kiracınız için kimlik doğrulama uç noktalarını gösteren **uç noktalar** sayfası görüntülenir.
    
    Uygulamanıza özgü kimlik doğrulama isteğini yapmak için **uygulama (istemci) kimliğiyle** birlikte kullandığınız kimlik doğrulama protokolüyle eşleşen uç noktayı kullanın.

**Ulusal bulutların** (örneğin, Azure AD Çin, Almanya ve ABD kamu) kendi uygulama kayıt portalı ve Azure AD kimlik doğrulama uç noktaları vardır. [Ulusal bulutlara genel bakış](authentication-national-cloud.md)konusunda daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Farklı Azure ortamlarındaki uç noktalar hakkında daha fazla bilgi için bkz. [National bulutlar genel bakış](authentication-national-cloud.md).
