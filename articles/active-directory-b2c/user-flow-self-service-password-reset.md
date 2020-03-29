---
title: Azure Active Directory B2C'de self servis parola sıfırlama | Microsoft Dokümanlar
description: Azure Active Directory B2C'deki müşterileriniz için self servis parola sıfırlama nın nasıl ayarlan olduğunu gösterir
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d6dad52c8a3e63c64bb8e0e0030e8c50b5bab42c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183117"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Müşterileriniz için self servis parola sıfırlama

Self servis parola sıfırlama özelliği sayesinde, yerel hesaplara kaydolan müşterileriniz parolalarını kendi başlarına sıfırlayabilir. Bu, özellikle uygulamanızın düzenli olarak kullanan milyonlarca müşterisi varsa, destek personelinizin üzerindeki yükü önemli ölçüde azaltır. Şu anda, doğrulanmış bir e-posta adresi kullanarak desteklenen tek kurtarma yöntemidir.

> [!NOTE]
> Bu makale, kimlik sağlayıcısı olarak **Yerel Hesap SignIn'i** kullanan V1 **Sign in** user akışı bağlamında kullanılan self servis parola sıfırlama için geçerlidir. Uygulamanızdan çağrılan kullanıcı akışlarını tamamen özelleştirilebilir parola sıfırlamanız gerekiyorsa, [bu makaleye](user-flow-overview.md)bakın.
>
>

Varsayılan olarak, dizininizin self servis parola sıfırlaması açık değildir. Açmak için aşağıdaki adımları kullanın:

1. Abonelik Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın. Bu, iş veya okul hesabı veya dizini oluşturmak için kullandığınız Microsoft hesabıyla aynıdır.
2. **Azure Etkin Dizini'ni** açın (sol taraftaki gezinti çubuğunda).
3. Seçenekler bıçağını aşağı kaydırın ve **Parola sıfırlama'yı**seçin.
4. **Tüm'e** **etkin olan Self servis parola sıfırlamayı** ayarlayın.
5. Sayfanın üst kısmından **Kaydet**'e tıklayın. Senin bitirdin!

Sınamak için, kimlik sağlayıcısı olarak yerel hesapları olan oturum açma kullanıcı akışında "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma sayfasında (e-posta adresi ve parola veya kullanıcı adı ve parola girdiğiniz yer) müşteri deneyimini doğrulamak için **hesabınıza erişemez mi?**

> [!NOTE]
> Self servis parola sıfırlama [sayfaları, şirket markalandırma özelliği](../active-directory/fundamentals/customize-branding.md)kullanılarak özelleştirilebilir.
>
>

