---
title: Azure AD Galerisi uygulaması için parola SSO yapılandırma sorunları
description: Azure AD Uygulama Galerisi'nde zaten listelenen uygulamalar için Parola Tek Oturum Açma'yı yapılandırırken insanların karşılaştığı sık karşılaşılan sorunları anlama
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
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: e933c82229415a71182096a6aca9a2e535934d89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77159055"
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galerisi uygulaması için parola tek oturum açma sorunu

Bu makale, bir Azure REKLAM Galerisi uygulamasıyla **Parola Tek Oturum** Açma'yı yapılandırırken insanların karşılaştığı sık karşılaşılan sorunları anlamanıza yardımcı olur.

## <a name="credentials-are-filled-in-but-the-extension-does-not-submit-them"></a>Kimlik bilgileri doldurulur, ancak uzantı bunları göndermez

Bu sorun genellikle, uygulama satıcısı bir alan eklemek için oturum açma sayfasını son zamanlarda değiştirmişse, kullanıcı adı ve parola alanlarını algılamak için kullanılan bir tanımlayıcıyı değiştirmişse veya oturum açma deneyiminin uygulamaları için nasıl çalıştığını değiştirmişse genellikle olur. Neyse ki, birçok durumda, Microsoft hızla bu sorunları çözmek için uygulama satıcıları ile çalışabilir.

Microsoft'un tümleştirmeler ne zaman kırılmadığını otomatik olarak algılatacak teknolojileri olsa da, sorunları hemen bulmak mümkün olmayabilir veya sorunların giderilmesi biraz zaman alabilir. Bu tümleştirmelerden biri düzgün çalışmadığında, mümkün olduğunca çabuk düzeltilebilmek için bir destek örneği açın.

**Bu uygulamanın satıcısıyla temas halindeyseniz, Microsoft'un** uygulamalarını Azure Active Directory ile yerel olarak tümleştirmek için onlarla çalışabilmesi için onlara yolumuzdan gönderin. Satıcıyı, başlatmaları için [Satıcıyı Azure Active Directory uygulama galerisindeki uygulamanızı Listeleme'ye](../azuread-dev/howto-app-gallery-listing.md) gönderebilirsiniz.

## <a name="credentials-are-filled-in-and-submitted-but-the-page-indicates-the-credentials-are-incorrect"></a>Kimlik bilgileri doldurulur ve gönderilir, ancak sayfa kimlik bilgilerinin yanlış olduğunu gösterir

Bu sorunu gidermek için önce şu şeyleri deneyin:

- Kullanıcının önce uygulama web sitesinde kendileri için depolanan kimlik bilgileriyle **doğrudan oturum açmayı** denemesini sorun.

  * Oturum açma işe yarıyorsa, kullanıcının [Uygulama Erişim Paneli'nin](https://myapps.microsoft.com/) **Uygulamalar** bölümündeki Uygulama Döşemesi'ndeki **Kimlik Bilgilerini Güncelle** düğmesini tıklatarak bunları bilinen en son çalışan kullanıcı adı ve parolayla güncelleştirmesini sağlar. **Application Tile**

  * Siz veya başka bir yönetici bu kullanıcı için kimlik bilgilerini atadıysa, uygulamanın **Kullanıcılar & Grupları** sekmesine yönlendirerek, atamayı seçerek ve Kimlik Bilgilerini **Güncelleştir** düğmesini tıklatarak kullanıcının veya grubun uygulama atamasını bulun.

- Kullanıcı kendi kimlik bilgilerini atadıysa, **parolalarının uygulamada süresinin dolmadığından emin olmak için kullanıcıyı denetleyin** ve bu **durumda, süresi dolmuş parolasını** doğrudan uygulamaya oturum açtırarak güncelleştirin.

  * Uygulamada parola güncelleştirildikten sonra, kullanıcıdan [Uygulama Erişim Paneli'nin](https://myapps.microsoft.com/) **Uygulamalar** bölümündeki Uygulama Döşemesi'ndeki **kimlik bilgilerini** güncelleve en son bilinen kullanıcı adı ve parolayla güncellemesini isteyin. **Application Tile**

  * Siz veya başka bir yönetici bu kullanıcı için kimlik bilgilerini atadıysa, uygulamanın **Kullanıcılar & Grupları** sekmesine yönlendirerek, atamayı seçerek ve Kimlik Bilgilerini **Güncelleştir** düğmesini tıklatarak kullanıcının veya grubun uygulama atamasını bulun.

- Access Panel Tarayıcı uzantısı bölümünde aşağıdaki adımları izleyerek kullanıcının erişim paneli tarayıcı [uzantısını](#how-to-install-the-access-panel-browser-extension) güncelleştirmesini sağlar.

- Erişim paneli tarayıcı uzantısının kullanıcınızın tarayıcısında çalıştırDığından ve etkinleştirildiğinden emin olun.

- Kullanıcılarınızın **gizli, özel veya Özel moddayken**erişim panelinden uygulamada oturum açmaya çalışmadığından emin olun. Erişim paneli uzantısı bu modlarda desteklenmez.

Önceki önerilerin işe yaraması durumunda, uygulama tarafında uygulamanın Azure AD ile tümleştirmesini geçici olarak bozan bir değişiklik meydana geldi olabilir. Örneğin, uygulama satıcısı kendi sayfasında el ile vs otomatik giriş için farklı davranan ve bizimki gibi otomatik tümleştirmenin kırılmasına neden olan bir komut dosyası tanıttığında bu durum oluşabilir. Neyse ki, birçok durumda, Microsoft hızla bu sorunları çözmek için uygulama satıcıları ile çalışabilir.

Microsoft'un uygulama tümleştirmeleri ne zaman kırılmadığını otomatik olarak algılatacak teknolojileri olsa da, sorunları hemen bulmak mümkün olmayabilir veya sorunların giderilmesi biraz zaman alabilir. Tümleştirme doğru çalışmadığında, mümkün olan en kısa sürede düzeltilmesi için bir destek servis talebi açabilirsiniz. 

Buna ek olarak, **bu uygulamanın satıcısıyla iletişim halindeyseniz,** uygulamalarını Azure Active Directory ile yerel olarak entegre etmek için onlarla çalışabilmemiz için **onlara yolumuzdan gönderin.** Satıcıyı, başlatmaları için [Satıcıyı Azure Active Directory uygulama galerisindeki uygulamanızı Listeleme'ye](../azuread-dev/howto-app-gallery-listing.md) gönderebilirsiniz.

## <a name="the-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>Uzantı Chrome ve Firefox'ta çalışır, ancak Internet Explorer'da çalışmaz

Bu sorunun iki ana nedeni vardır:

- Internet Explorer'da etkinleştirilen güvenlik ayarlarına bağlı olarak, web sitesi **Güvenilir Bölge'nin**bir parçası değilse, bazen komut dosyamızın uygulama için yürütülmesi engellenir.

  *  Bunu çözmek için, kullanıcıya internet **gezgini güvenlik ayarlarında** **uygulamanın web sitesini** Güvenilir **Siteler** listesine eklemesini talimatını ver. Kullanıcılarınızı güvenilir siteler [liste makaleme](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) nasıl site ekleyebileceğinize ayrıntılı talimatlar için gönderebilirsiniz.

- Nadir durumlarda, Internet Explorer'ın güvenlik doğrulaması bazen sayfanın komut dosyamızın yürütülmesinden daha yavaş yüklenmesine neden olabilir.

  * Ne yazık ki, bu durum tarayıcı sürümüne, bilgisayar hızına veya ziyaret edilen siteye bağlı olarak değişebilir. Bu durumda, bu özel uygulamanın tümleştirmesini düzeltebilmemiz için desteğe başvurmanızı öneririz.

Buna ek olarak, **bu uygulamanın satıcısıyla iletişim halindeyseniz,** uygulamalarını Azure Active Directory ile yerel olarak entegre etmek için onlarla çalışabilmemiz için **onlara yolumuzdan gönderin.** Satıcıyı, başlatmaları için [Satıcıyı Azure Active Directory uygulama galerisindeki uygulamanızı Listeleme'ye](../azuread-dev/howto-app-gallery-listing.md) gönderebilirsiniz.

## <a name="check-if-the-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Uygulamanın giriş sayfasının yakın zamanda değişip değişmediğini veya ek bir alan gerektiremediğini kontrol edin

Uygulamanın giriş sayfası büyük ölçüde değişmişse, bazen bu tümleştirmelerimizin kırılmasına neden olur. Buna bir örnek, bir uygulama satıcısının deneyimlerine oturum açma alanı, bir captcha veya çok faktörlü kimlik doğrulaması eklediği dir. Neyse ki, birçok durumda, Microsoft hızla bu sorunları çözmek için uygulama satıcıları ile çalışabilir.

Microsoft'un uygulama tümleştirmeleri ne zaman kırılmadığını otomatik olarak algılatacak teknolojileri olsa da, sorunları hemen bulmak mümkün olmayabilir veya sorunların giderilmesi biraz zaman alabilir. Tümleştirme doğru çalışmadığında, mümkün olan en kısa sürede düzeltilmesi için bir destek servis talebi açabilirsiniz. 

Buna ek olarak, **bu uygulamanın satıcısıyla iletişim halindeyseniz,** uygulamalarını Azure Active Directory ile yerel olarak entegre etmek için onlarla çalışabilmemiz için **onlara yolumuzdan gönderin.** Satıcıyı, başlatmaları için [Satıcıyı Azure Active Directory uygulama galerisindeki uygulamanızı Listeleme'ye](../azuread-dev/howto-app-gallery-listing.md) gönderebilirsiniz.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Access Panel Tarayıcı uzantısı nasıl yüklenir?

Access Panel Tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Desteklenen tarayıcılardan birinde [Erişim Paneli'ni](https://myapps.microsoft.com) açın ve Azure AD'nizde **kullanıcı** olarak oturum açın.

2.  Access Paneli'ndeki bir **parola-SSO uygulamasını** tıklatın.

3.  Yazılımı yüklemek isteyen istemi, **Şimdi Yükle'yi**seçin.

4.  Tarayıcınıza bağlı olarak, indirme bağlantısına yönlendirilirsiniz. Uzantıyı tarayıcınıza **ekleyin.**

5.  Tarayıcınız sorarsa, uzantıyı **etkinleştir** veya **ona izin ver'i** seçin.

6.  Yüklendikten sonra tarayıcı oturumunuzu **yeniden başlatın.**

7.  Access Paneli'nde oturum açın ve parola-SSO uygulamalarınızı **başlatıp başlatamayacağınızı** görün

Chrome ve Firefox için uzantıyı aşağıdaki doğrudan linklerden de indirebilirsiniz:

-   [Chrome Erişim Paneli Uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox Erişim Paneli Uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

