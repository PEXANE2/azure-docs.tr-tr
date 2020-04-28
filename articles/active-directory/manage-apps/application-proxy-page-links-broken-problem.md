---
title: Sayfadaki bağlantılar uygulama proxy 'Si uygulaması için çalışmıyor
description: Azure AD ile tümleştirmiş olduğunuz uygulama proxy 'Si uygulamalarındaki kopuk bağlantılarla ilgili sorunları giderme
services: active-directory
author: msmimart
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca782b5a190704911472d70c414d12afa7e558f0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75430237"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Sayfadaki bağlantılar uygulama proxy 'Si uygulaması için çalışmıyor

Bu makale, Azure Active Directory Uygulama Ara Sunucusu uygulamanızdaki bağlantıların neden düzgün şekilde çalışmadığını gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy 'SI uygulamasını yayımladıktan sonra, uygulamada varsayılan olarak çalışan tek bağlantılar, yayımlanan kök URL içinde bulunan hedeflere yönelik bağlantılardır. Uygulamalar içindeki bağlantılar çalışmıyor, uygulamanın iç URL 'SI büyük olasılıkla uygulamanın içindeki bağlantıların tüm hedeflerini içermez.

**Bunun nedeni nedir?** Uygulama proxy 'si uygulamadaki bir bağlantıya tıkladığınızda, URL 'yi aynı uygulamadaki iç URL ya da harici olarak kullanılabilir bir URL olarak çözümlemeye çalışır. Bağlantı, aynı uygulama içinde olmayan bir iç URL 'ye işaret ediyorsa, bu demetlerden birine ait değildir ve bulunamayan bir hataya yol açabilir.

## <a name="ways-you-can-resolve-broken-links"></a>Kopuk bağlantıları çözebilmeniz için yollar

Bu sorunu çözmek için üç yol vardır. Aşağıdaki seçimler artan karmaşıklıkta listelenmiştir.

1.  İç URL 'nin uygulama için ilgili tüm bağlantıları içeren bir kök olduğundan emin olun. Bu, tüm bağlantıların aynı uygulama içinde yayımlanan içerik olarak çözümlenme olanağı sağlar.

    İç URL 'YI değiştirir ancak kullanıcıların giriş sayfasını değiştirmek istemiyorsanız, giriş sayfası URL 'sini önceden yayımlanmış iç URL ile değiştirin. Bu, "Azure Active Directory"-&gt; uygulama kayıtları ' na giderek yapılabilir. uygulama&gt; &gt; markalamasını seçin. Marka bölümünde, istenen giriş sayfası olarak ayarlayabileceğiniz "giriş sayfası URL 'SI" alanını görürsünüz. Eski Uygulama kayıtları deneyimini kullanmaya devam ediyorsanız Özellikler sekmesinde "giriş sayfası URL 'SI" ayrıntıları gösterilir. 
    
    > [!IMPORTANT]
    > Yukarıdaki değişiklikleri yapmak için Azure AD 'de uygulama nesnelerini değiştirme haklarınızın olması gerekir. Kullanıcıya, Kullanıcı için Azure AD 'de uygulama modificaion hakları veren [Uygulama Yöneticisi](../users-groups-roles/roles-delegate-app-roles.md#assign-built-in-application-admin-roles) rolü atanması gerekir.
    >

2.  Uygulamalarınız tam etki alanı adları (FQDN) kullanıyorsa, uygulamalarınızı yayımlamak için [özel etki alanlarını](application-proxy-configure-custom-domain.md) kullanın. Bu özellik hem dahili hem de harici olarak aynı URL 'nin kullanılmasına izin verir.

    Bu seçenek, uygulamadaki bağlantıların iç URL 'Lerle aynı zamanda dışarıdan tanındığından, uygulamanızdaki bağlantıların uygulama proxy 'Si aracılığıyla dışarıdan erişilebilir olmasını sağlar. Tüm bağlantıların hala yayımlanmış bir uygulamaya ait olması gerekir. Ancak, bu seçenekle bağlantıların aynı uygulamaya ait olması gerekmez ve birden çok uygulamaya ait olabilir.

3.  Bu seçeneklerden hiçbiri uygun değilse, satır içi bağlantı çevirisini etkinleştirmeye yönelik birden çok seçenek vardır. Bu seçenekler Intune Managed Browser, Uygulamalarım uzantısını veya uygulamanızdaki bağlantı çevirisi ayarını kullanmayı içerir. Bu seçeneklerin her biri ve nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantıları yeniden yönlendirme](application-proxy-configure-hard-coded-link-translation.md).

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)

