---
title: Belirteçleri yapılandırma - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'de belirteç ömrü ve uyumluluk ayarlarını nasıl yapılandırabilirsiniz öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 160898f8535d3dad4811af016ebca779b7ef43a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189626"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Azure Etkin Dizin B2C'de belirteçleri yapılandırma

Bu makalede, Azure Etkin Dizin B2C'de (Azure AD B2C) [bir belirteç](tokens-overview.md) kullanım ömrünü ve uyumluluğunu nasıl yapılandırabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Kullanıcıların uygulamanıza kaydolmasını ve oturum açmalarını sağlamak için [bir kullanıcı akışı oluşturun.](tutorial-create-user-flows.md)

## <a name="configure-token-lifetime"></a>Belirteç ömrünü yapılandırma

Belirteç ömrünü herhangi bir kullanıcı akışında yapılandırabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Kullanıcı akışlarını (ilkeler)** seçin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikleri**seçin.
7. **Token ömrü**altında, uygulamanızın ihtiyaçlarına uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portalında belirteç ömür boyu özellik ayarları](./media/configure-tokens/token-lifetime.png)

8. **Kaydet**'e tıklayın.

## <a name="configure-token-compatibility"></a>Belirteç uyumluluğunu yapılandırma

1. **Kullanıcı akışlarını (ilkeler)** seçin.
2. Daha önce oluşturduğunuz kullanıcı akışını açın.
3. **Özellikleri**seçin.
4. **Belirteç uyumluluk ayarları**altında, uygulamanızın gereksinimlerine uyacak şekilde aşağıdaki özellikleri ayarlayın:

    ![Azure portalında belirteç uyumluluk özelliği ayarları](./media/configure-tokens/token-compatibility.png)

5. **Kaydet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Erişim belirteçlerini](access-tokens.md)nasıl kullanacağınız hakkında daha fazla bilgi edinin.



