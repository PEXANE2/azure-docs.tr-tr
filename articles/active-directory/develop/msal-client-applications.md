---
title: Genel ve gizli istemci uygulamaları (MSAL) | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı'ndaki (MSAL) ortak istemci ve gizli istemci uygulamaları hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/25/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: d59819c0ab614b0f6cc102c7ebe8c760fb851599
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084127"
---
# <a name="public-client-and-confidential-client-applications"></a>Kamu müşteri ve gizli müşteri uygulamaları
Microsoft Kimlik Doğrulama Kitaplığı (MSAL), iki tür istemci tanımlar: ortak istemciler ve gizli istemciler. İki istemci türü, yetkilendirme sunucusuyla güvenli bir şekilde kimlik doğrulama ve istemci kimlik bilgilerinin gizliliğini koruma yetenekleriyle ayırt edilir. Buna karşılık, Azure AD Kimlik Doğrulama Kitaplığı (ADAL), *kimlik doğrulama bağlamı* (Azure AD bağlantısı) olarak adlandırılan içeriği kullanır.

- **Gizli istemci uygulamaları** sunucularda çalışan uygulamalardır (web uygulamaları, Web API uygulamaları, hatta hizmet/daemon uygulamaları). Bunlara erişimin zor olduğu düşünülür ve bu nedenle bir uygulamayı gizli tutabilme yeteneğine sahiptirler. Gizli istemciler yapılandırma zamanı sırlarını saklayabilir. İstemcinin her örneğinin ayrı bir yapılandırması vardır (istemci kimliği ve istemci sırrı dahil). Bu değerleri son kullanıcılar için ayıklamak zordur. Web uygulaması en yaygın gizli istemcidir. İstemci kimliği web tarayıcısı üzerinden açıklanır, ancak gizli sadece arka kanalda geçirilir ve doğrudan maruz asla.

    Gizli istemci uygulamaları: <BR>
    ![Web](media/msal-client-applications/web-app.png) ![uygulaması Web](media/msal-client-applications/web-api.png) ![API Daemon/service](media/msal-client-applications/daemon-service.png)

- **Ortak istemci uygulamaları,** aygıtlarda veya masaüstü bilgisayarlarda veya bir web tarayıcısında çalışan uygulamalardır. Uygulama sırlarını güvenli bir şekilde saklamaları için güvenilir değildir, bu nedenle web API'lerine yalnızca kullanıcı adına erişirler. (Yalnızca genel müşteri akışlarını desteklerler.) Genel müşteriler yapılandırma zamanı sırlarını tutamaz, bu yüzden müşteri sırları yoktur.

    Genel istemci uygulamaları: <BR>
    ![Masaüstü](media/msal-client-applications/desktop-app.png) ![uygulaması Browserless](media/msal-client-applications/browserless-app.png) ![API Mobil uygulaması](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> MSAL.js'de, genel ve gizli istemci uygulamaları arasında ayrım yoktur.  MSAL.js, istemci uygulamalarını kullanıcı aracısı tabanlı uygulamalar, istemci kodunun bir web tarayıcısı gibi bir kullanıcı aracısında yürütüldürün ortak istemcileri olarak temsil eder. Tarayıcı bağlamına açıkça erişilebildiğinden, bu istemciler sırları saklamaz.

## <a name="comparing-the-client-types"></a>İstemci türlerini karşılaştırma
Ortak istemci ve gizli istemci uygulamaları arasındaki bazı benzerlikler ve farklar şunlardır:

- Her iki uygulama türü de kullanıcı belirteci önbelleğini korur ve sessizce bir belirteç elde edebilir (belirteç zaten belirteç önbelleğindeyse). Gizli istemci uygulamaları, uygulamanın kendisi için olan belirteçler için bir uygulama belirteç önbelleğine de sahiptir.
- Her iki uygulama türü de kullanıcı hesaplarını yönetebilir ve kullanıcı belirteci önbelleğinden bir hesap alabilir, tanımlayıcısından bir hesap alabilir veya bir hesabı kaldırabilir.
- Genel istemci uygulamalarının bir belirteç (dört kimlik doğrulama akışı) edinmenin dört yolu vardır. Gizli istemci uygulamalarının bir belirteci edinmenin üç yolu vardır (ve kimlik sağlayıcısının URL'sini hesaplamanın bir yolu bitiş noktasını yetkilendirmek için). Daha fazla bilgi için [bkz.](msal-acquire-cache-tokens.md)

ADAL kullandıysanız, ADAL'ın kimlik doğrulama bağlamının aksine, MSAL'da istemci kimliğinin *(uygulama kimliği* veya *uygulama kimliği*olarak da adlandırılır) uygulamanın yapımında bir kez geçirildiğini fark edebilirsiniz. Uygulama bir belirteç edindiğinde yeniden geçirilmesi gerekmez. Bu, hem genel hem de gizli istemci uygulamaları için geçerlidir. Gizli istemci uygulamalarının oluşturucuları da istemci kimlik bilgileri ne aktarılır: kimlik sağlayıcısıyla paylaştıkları sır.

## <a name="next-steps"></a>Sonraki adımlar
Şunları öğrenin:
- [İstemci uygulama yapılandırma seçenekleri](msal-client-application-configuration.md)
- [MSAL.NET kullanarak istemci uygulamalarını anında kullanma](msal-net-initializing-client-applications.md)
- [MSAL.js kullanarak müşteri uygulamalarını anında kullanma](msal-js-initializing-client-applications.md)
