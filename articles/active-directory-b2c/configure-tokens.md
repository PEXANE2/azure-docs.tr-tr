---
title: Belirteçleri Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C ' de belirteç ömrünü ve uyumluluk ayarlarını yapılandırmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a2b9cc0c69f0c559c9561a02de4192974ee7f521
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82926196"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Azure Active Directory B2C belirteçleri yapılandırma

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de [bir belirtecin yaşam süresini ve uyumluluğunu](tokens-overview.md) yapılandırmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların uygulamanızda kaydolup oturum açmasını sağlamak için [bir Kullanıcı akışı oluşturun](tutorial-create-user-flows.md) .

## <a name="configure-jwt-token-lifetime"></a>JWT belirteci ömrünü yapılandırma

Belirteç ömrünü herhangi bir Kullanıcı akışında yapılandırabilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikler**’i seçin.
7. **Belirteç ömrü**altında, uygulamanızın gereksinimlerine uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portal belirteç ömrü özelliği ayarları](./media/configure-tokens/token-lifetime.png)

8. **Kaydet**’e tıklayın.

## <a name="configure-jwt-token-compatibility"></a>JWT belirteci uyumluluğunu yapılandırma

1. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
2. Daha önce oluşturduğunuz kullanıcı akışını açın.
3. **Özellikler**’i seçin.
4. **Belirteç uyumluluk ayarları**altında, uygulamanızın gereksinimlerine uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portal belirteç uyumluluk özelliği ayarları](./media/configure-tokens/token-compatibility.png)

5. **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Erişim belirteçlerini kullanma](access-tokens.md)hakkında daha fazla bilgi edinin.



