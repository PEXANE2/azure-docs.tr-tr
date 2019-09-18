---
title: Yerel istemci uygulaması ekleme-Azure Active Directory B2C | Microsoft Docs
description: Active Directory B2C kiracınıza yerel istemci uygulaması eklemeyi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 6fc953d5c6109fbc6eacbd946ecd112db4639fa5
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064586"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracınıza yerel istemci uygulaması ekleme

Uygulamanızın Azure Active Directory B2C ile iletişim kurabilmesi için önce yerel istemci kaynaklarının kiracınızda kayıtlı olması gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
2. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
3. **İçerme Web uygulaması/Web API 'si**için **Hayır**' ı seçin.
4. **Yerel Istemciyi Ekle**için **Evet**' i seçin.
5. **Yeniden yönlendirme URI 'si**için, özel bir şemaya sahip geçerli bir yeniden yönlendirme URI 'si girin. Yeniden yönlendirme URI 'SI seçerken dikkat etmeniz gereken iki önemli nokta vardır:

    - **Unique** -YENIDEN yönlendirme URI 'sinin şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contoso.appname://redirect/path` ,`com.onmicrosoft.contoso.appname` şemadır. Bu düzenin izlenmesi gerekir. İki uygulama aynı düzeni paylaşıyorsa, kullanıcıya bir uygulama seçme seçeneği verilir. Kullanıcı yanlış bir seçim yapıyorsa, oturum açma işlemi başarısız olur.
    - **Tam** -yeniden yönlendirme URI 'si bir şemaya ve bir yola sahip olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//contoso/` `//contoso` çalışıp başarısız olur. Yeniden yönlendirme URI 'sinin alt çizgi gibi özel karakterler içermediğinden emin olun.

6. **Oluştur**'a tıklayın.
7. Özellikler sayfasında, yerel istemci uygulamanızı yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.
