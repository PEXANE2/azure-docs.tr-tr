---
title: Belirteçleri Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C ' de belirteç ömrünü ve uyumluluk ayarlarını yapılandırmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67bc9d6b35d4841999721a00592a6bbe23bff10f
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92340232"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Azure Active Directory B2C belirteçleri yapılandırma

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de [bir belirtecin yaşam süresini ve uyumluluğunu](tokens-overview.md) yapılandırmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların uygulamanıza kaydolmasını ve oturum açmasını sağlamak için [bir kullanıcı akışı oluşturun](tutorial-create-user-flows.md).

## <a name="configure-jwt-token-lifetime"></a>JWT belirteci ömrünü yapılandırma

Belirteç ömrünü herhangi bir Kullanıcı akışında yapılandırabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikler**’i seçin.
7. **Belirteç ömrü**altında, uygulamanızın gereksinimlerine uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portal belirteç ömrü özelliği ayarları](./media/configure-tokens/token-lifetime.png)

8. **Kaydet**’e tıklayın.

> [!NOTE]
> PKI CE ile yetkilendirme kodu akışını kullanan tek sayfalı uygulamalarda, her zaman 24 saat için yenileme belirteci ömrü vardır. [Tarayıcıda belirteçleri yenileme güvenlik etkileri hakkında daha fazla bilgi edinin](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser).

## <a name="configure-jwt-token-compatibility"></a>JWT belirteci uyumluluğunu yapılandırma

1. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
2. Daha önce oluşturduğunuz kullanıcı akışını açın.
3. **Özellikler**’i seçin.
4. **Belirteç uyumluluk ayarları**altında, uygulamanızın gereksinimlerine uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portal belirteç uyumluluk özelliği ayarları](./media/configure-tokens/token-compatibility.png)

5. **Kaydet**’e tıklayın.

## <a name="provide-optional-claims-to-your-app"></a>Uygulamanıza isteğe bağlı talepler sağlama

Uygulama talepleri, uygulamaya döndürülen değerlerdir. Kullanıcı akışınızı istenen talepleri içerecek şekilde güncelleştirin.

1. **Kullanıcı akışları (ilkeler)** seçeneğini belirleyin.
1. Daha önce oluşturduğunuz kullanıcı akışını açın.
1. **Uygulama talepleri**’ni seçin.
1. Uygulamanıza geri göndermek istediğiniz talepleri ve öznitelikleri seçin.
1. **Kaydet**’e tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

[Erişim belirteçleri isteme](access-tokens.md)hakkında daha fazla bilgi edinin.



