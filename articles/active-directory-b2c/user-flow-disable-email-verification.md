---
title: Müşteri kayıt sırasında e-posta doğrulamayı devre dışı
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de müşteri kaydolma sırasında e-posta doğrulamayı nasıl devre dışı köğreneceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 369b4e13baa344a71a51b358ef810d1a66b4b6ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126729"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de müşteri kayıt sırasında e-posta doğrulamayı devre dışı

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

E-posta doğrulamasını devre dışı katmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com) oturum açın
1. Azure AD B2C kiracınızı içeren dizin seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Kullanıcı akışlarını**seçin.
1. E-posta doğrulamasını devre dışı bırakabileceğiniz kullanıcı akışını seçin. Örneğin, *B2C_1_signinsignup.*
1. **Sayfa düzenlerini**seçin.
1. **Yerel hesap kayıt sayfasını**seçin.
1. **Kullanıcı öznitelikleri**altında, **E-posta Adresi**seçin.
1. **GEREKİr DOĞRULAMA** açılır açılır, **Hayır'ı**seçin.
1. **Kaydet'i**seçin. E-posta doğrulaması artık bu kullanıcı akışı için devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C'de kullanıcı arabirimini nasıl özelleştirebilirsiniz](customize-ui-overview.md) öğrenin

