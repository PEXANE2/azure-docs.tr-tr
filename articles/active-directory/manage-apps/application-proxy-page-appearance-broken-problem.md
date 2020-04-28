---
title: Uygulama sayfası uygulama proxy 'Si uygulaması için doğru görüntülenmez | Microsoft Docs
description: Azure AD ile Tümleştirdiğiniz bir uygulama proxy uygulamasında sayfanın doğru görüntülenmediği kılavuz
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "68381421"
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Uygulama sayfası bir uygulama proxy 'Si uygulaması için doğru görüntülenmez

Bu makale, sayfaya gittiğinizde Azure Active Directory Uygulama Ara Sunucusu uygulamalarla ilgili sorunları gidermenize yardımcı olur, ancak sayfadaki bir şey doğru görünmüyor.

## <a name="overview"></a>Genel Bakış
Bir uygulama proxy 'Si uygulaması yayımladığınızda, uygulamaya erişirken yalnızca köklerinizin altındaki sayfalara erişilebilir. Sayfa doğru görüntülenmiyorsa, uygulama için kullanılan kök iç URL 'nin bazı sayfa Kaynakları eksik olabilir. Sorunu çözmek için, sayfanın *Tüm* kaynaklarını uygulamanızın bir parçası olarak yayımladığınızdan emin olun.

Ağ izleyicinizi (ör. Fiddler veya Internet Explorer/Microsoft Edge 'de F12 araçları) açarak, sayfayı yükleyerek ve 404 hata arayarak sorunu eksik kaynaklarda doğrulayabilirsiniz. Bu, şu an sayfaların bulunamadığını ve bunları yayımlamanız gerektiğini gösterir.

Bu durumda bir örnek olarak, iç URL `http://myapps/expenses`'yi kullanarak bir giderler uygulaması yayımladığınızı varsayın, ancak uygulama stil sayfasını `http://myapps/style.css`kullanır. Bu durumda, stil sayfası uygulamanızda yayımlanmaz, böylece giderler uygulamasının yüklenmesi, Style. css ' yi yüklemeye çalışırken 404 hatası oluşturur. Bu örnekte, bir iç URL `http://myapp/`ile uygulama yayımlayarak sorun çözülür.

## <a name="problems-with-publishing-as-one-application"></a>Tek bir uygulama olarak yayımlama sorunları

Aynı uygulamadaki tüm kaynakları yayımlamak mümkün değilse, birden fazla uygulama yayımlamanız ve aralarında bağlantıları etkinleştirmeniz gerekir.

Bunu yapmak için [özel etki alanları](application-proxy-configure-custom-domain.md) çözümünü kullanmanızı öneririz. Ancak bu çözüm, etki alanınız için sertifikaya sahip olmanızı gerektirir ve uygulamalarınız tam etki alanı adlarını (FQDN) kullanır. Diğer seçenekler için bkz. [bozuk bağlantıları sorun giderme belgeleri](application-proxy-page-links-broken-problem.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
