---
title: Müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C ' de müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 10613bd2d6219272248f882e45ae1c33d2cc9d61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85384218"
---
# <a name="disable-email-verification-during-customer-sign-up-in-azure-active-directory-b2c"></a>Azure Active Directory B2C Müşteri kaydı sırasında e-posta doğrulamayı devre dışı bırakma

[!INCLUDE [disable email verification intro](../../includes/active-directory-b2c-disable-email-verification.md)]

E-posta doğrulamayı devre dışı bırakmak için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın
1. Azure AD B2C kiracınızı içeren dizini seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Kullanıcı akışları ' nı**seçin.
1. E-posta doğrulamasını devre dışı bırakmak istediğiniz kullanıcı akışını seçin. Örneğin, *B2C_1_signinsignup*.
1. **Sayfa düzenlerini**seçin.
1. **Yerel hesap kaydolma sayfası**' nı seçin.
1. **Kullanıcı öznitelikleri**altında **e-posta adresi**' ni seçin.
1. **Doğrulama gerektirir** açılır penceresinde **Hayır**' ı seçin.
1. **Kaydet**'i seçin. E-posta doğrulaması artık bu kullanıcı akışı için devre dışı bırakıldı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C içinde Kullanıcı arabirimini özelleştirmeyi](customize-ui-overview.md) öğrenin

