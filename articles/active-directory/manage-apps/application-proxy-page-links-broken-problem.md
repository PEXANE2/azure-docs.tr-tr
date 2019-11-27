---
title: Sayfadaki bağlantılar uygulama proxy 'Si uygulaması için çalışmıyor
description: Azure AD ile tümleştirmiş olduğunuz uygulama proxy 'Si uygulamalarındaki kopuk bağlantılarla ilgili sorunları giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 570699fe83197a1b5442909d8b89e285a1dfa73b
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275426"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Sayfadaki bağlantılar uygulama proxy 'Si uygulaması için çalışmıyor

Bu makale, Azure Active Directory Uygulama Ara Sunucusu uygulamanızdaki bağlantıların neden düzgün şekilde çalışmadığını gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy 'SI uygulamasını yayımladıktan sonra, uygulamada varsayılan olarak çalışan tek bağlantılar, yayımlanan kök URL içinde bulunan hedeflere yönelik bağlantılardır. Uygulamalar içindeki bağlantılar çalışmıyor, uygulamanın iç URL 'SI büyük olasılıkla uygulamanın içindeki bağlantıların tüm hedeflerini içermez.

**Bunun nedeni nedir?** Uygulama proxy 'si uygulamadaki bir bağlantıya tıkladığınızda, URL 'yi aynı uygulamadaki iç URL ya da harici olarak kullanılabilir bir URL olarak çözümlemeye çalışır. Bağlantı, aynı uygulama içinde olmayan bir iç URL 'ye işaret ediyorsa, bu demetlerden birine ait değildir ve bulunamayan bir hataya yol açabilir.

## <a name="ways-you-can-resolve-broken-links"></a>Kopuk bağlantıları çözebilmeniz için yollar

Bu sorunu çözmek için üç yol vardır. Aşağıdaki seçimler artan karmaşıklıkta listelenmiştir.

1.  İç URL 'nin uygulama için ilgili tüm bağlantıları içeren bir kök olduğundan emin olun. Bu, tüm bağlantıların aynı uygulama içinde yayımlanan içerik olarak çözümlenme olanağı sağlar.

    İç URL 'YI değiştirir ancak kullanıcıların giriş sayfasını değiştirmek istemiyorsanız, giriş sayfası URL 'sini önceden yayımlanmış iç URL ile değiştirin. Bu işlem, "Azure Active Directory"-&gt; uygulama kayıtlarına gidip&gt; uygulama-&gt; özelliklerini seçmeniz durumunda yapılabilir. Bu Özellikler sekmesinde, istenen giriş sayfası olarak ayarlayabileceğiniz "giriş sayfası URL 'SI" alanını görürsünüz.

2.  Uygulamalarınız tam etki alanı adları (FQDN) kullanıyorsa, uygulamalarınızı yayımlamak için [özel etki alanlarını](application-proxy-configure-custom-domain.md) kullanın. Bu özellik hem dahili hem de harici olarak aynı URL 'nin kullanılmasına izin verir.

    Bu seçenek, uygulamadaki bağlantıların iç URL 'Lerle aynı zamanda dışarıdan tanındığından, uygulamanızdaki bağlantıların uygulama proxy 'Si aracılığıyla dışarıdan erişilebilir olmasını sağlar. Tüm bağlantıların hala yayımlanmış bir uygulamaya ait olması gerekir. Ancak, bu seçenekle bağlantıların aynı uygulamaya ait olması gerekmez ve birden çok uygulamaya ait olabilir.

3.  Bu seçeneklerden hiçbiri uygun değilse, satır içi bağlantı çevirisini etkinleştirmeye yönelik birden çok seçenek vardır. Bu seçenekler Intune Managed Browser, Uygulamalarım uzantısını veya uygulamanızdaki bağlantı çevirisi ayarını kullanmayı içerir. Bu seçeneklerin her biri ve nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [Azure AD uygulama ara sunucusu ile yayımlanan uygulamalar için sabit kodlanmış bağlantıları yeniden yönlendirme](application-proxy-configure-hard-coded-link-translation.md).

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucularıyla çalışma](application-proxy-configure-connectors-with-proxy-servers.md)

