---
title: Uygulama Sayfası Uygulama Proxy uygulaması için doğru görüntülenmiyor | Microsoft Dokümanlar
description: Azure AD ile tümleştirdiğiniz bir Uygulama Proxy Uygulamasında sayfa doğru görüntülemediği durumlarda kılavuz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13e73f0ed56648ce162f00d6df5e7b86a922ca01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68381421"
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Uygulama Proxy uygulaması için uygulama sayfası doğru görüntülemiyor

Bu makale, sayfaya gidince Azure Active Directory Application Proxy uygulamalarıyla ilgili sorunları gidermenize yardımcı olur, ancak sayfadaki bir şey doğru görünmüyor.

## <a name="overview"></a>Genel Bakış
Bir Uygulama Proxy uygulaması yayımladığınızda, uygulamaya erişirken yalnızca rootunuz altındaki sayfalara erişilebilir. Sayfa doğru görüntülemiyorsa, uygulama için kullanılan kök iç URL'de bazı sayfa kaynakları eksik olabilir. Çözmek için, uygulamanızın bir parçası olarak sayfanın *tüm* kaynaklarını yayımladığınızdan emin olun.

Ağ izleyicinizi (Fiddler veya Internet Explorer/Microsoft Edge'deki F12 araçları gibi) açarak, sayfayı yükleyerek ve 404 hata arayarak kaynakların eksik olup olmadığını doğrulayabilirsiniz. Bu, sayfaların şu anda bulunamaması ve bunları yayımlamanız gerektiğini gösterir.

Bu duruma örnek olarak, dahili URL'yi `http://myapps/expenses`kullanarak bir gider uygulaması yayımladığınızı varsayDığınızı varsayın, ancak uygulama stil sayfasını `http://myapps/style.css`kullanır. Bu durumda, stylesheet uygulamanızda yayınlanmaz, bu nedenle gider uygulaması yükleme style.css yüklemeye çalışırken bir 404 hata atmak. Bu örnekte, uygulama dahili bir URL `http://myapp/`ile yayımlayarak sorun çözülür.

## <a name="problems-with-publishing-as-one-application"></a>Tek bir uygulama olarak yayımlama ile ilgili sorunlar

Aynı uygulama içindeki tüm kaynakları yayımlamak mümkün değilse, birden çok uygulama yayımlamanız ve aralarındaki bağlantıları etkinleştirmeniz gerekir.

Bunu yapmak [için, özel etki alanları](application-proxy-configure-custom-domain.md) çözümlerini kullanmanızı öneririz. Ancak, bu çözüm etki alanınız için sertifikaya sahip olduğunuzu ve uygulamalarınızın tam nitelikli alan adları (FQDNs) kullanmasını gerektirir. Diğer seçenekler için, [bozuk bağlantılar belgelerinin sorun giderme sorununa](application-proxy-page-links-broken-problem.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
