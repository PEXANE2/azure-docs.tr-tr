---
title: Azure AD Application Proxy sık sorulan sorular | Microsoft Dokümanlar
description: Uzak kullanıcılara dahili, şirket içi uygulamaları yayınlamak için Azure AD Application Proxy'yi kullanma yla ilgili sık sorulan soruların (SSS) yanıtlarını öğrenin.
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
ms.date: 10/03/2019
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: 31587777ba22dd8b4cebf81f0ff98979bb30fade
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410712"
---
# <a name="active-directory-azure-ad-application-proxy-frequently-asked-questions"></a>Active Directory (Azure AD) Application Proxy sık sorulan sorular

Bu sayfa, Azure Etkin Dizin (Azure AD) Uygulama Proxy ile ilgili sık sorulan soruları yanıtlar.

## <a name="enabling-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy'sini Etkinleştirme

### <a name="what-license-is-required-to-use-azure-ad-application-proxy"></a>Azure AD Application Proxy'yi kullanmak için hangi lisans gereklidir?

Azure AD Application Proxy'yi kullanmak için Azure AD Premium P1 veya P2 lisansınolması gerekir. Lisanslama hakkında daha fazla bilgi için Azure [Etkin Dizin Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/active-directory/) bakın

### <a name="why-is-the-enable-application-proxy-button-grayed-out"></a>"Uygulama Proxy'sini Etkinleştir düğmesi neden gri mi?

En az bir Azure AD Premium P1 veya P2 lisansınve bir Azure AD Uygulama Proxy Bağlayıcısı yüklü olduğundan emin olun. İlk bağlayıcınızı başarıyla yükledikten sonra Azure AD Uygulama Proxy hizmeti otomatik olarak etkinleştirilir.

## <a name="connector-configuration"></a>Konektör yapılandırması

### <a name="can-application-proxy-connector-services-run-in-a-different-user-context-than-the-default"></a>Uygulama Proxy Bağlayıcısı hizmetleri varsayılandan farklı bir kullanıcı bağlamında çalıştırılabilir mi?

Hayır, bu senaryo desteklenmiyor. Varsayılan ayarlar şunlardır:

- Microsoft AAD Uygulama Proxy Bağlayıcısı - WAPCSvc - Ağ Hizmeti
- Microsoft AAD Uygulama Proxy Bağlayıcı Güncelleme - WAPCUpdaterSvc - NT Authority\System

### <a name="my-back-end-application-is-hosted-on-multiple-web-servers-and-requires-user-session-persistence-stickiness-how-can-i-achieve-session-persistence"></a>Benim arka uç uygulama birden çok web sunucularında barındırılan ve kullanıcı oturumu kalıcılığı (yapışkanlık) gerektirir. Oturum kalıcılığını nasıl sağlayabilirim? 

Öneriler için, [Uygulama Proxy bağlayıcılarınızın ve uygulamalarınızın yüksek kullanılabilirlik ve yük dengelemesi](application-proxy-high-availability-load-balancing.md)bölümüne bakın.

### <a name="can-i-place-a-forward-proxy-device-between-the-connector-servers-and-the-back-end-application-server"></a>Bağlayıcı sunucu(lar) ile arka uç Uygulama sunucusu arasına bir ileri proxy aygıtı yeralabilir miyim?

Hayır, bu senaryo desteklenmiyor. Yalnızca bağlayıcı ve güncelleştirme hizmetleri, Azure'a giden trafik için bir ileri proxy kullanacak şekilde yapılandırılabilir. Bkz. [Varolan şirket içi proxy sunucularıyla çalışma](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-connectors-with-proxy-servers)

### <a name="is-tls-termination-tlshttps-inspection-or-acceleration-on-traffic-from-the-connector-servers-to-azure-supported"></a>TLS sonlandırma (TLS/HTTPS denetimi veya ivmesi) bağlayıcı sunuculardan Azure'a kadar olan trafikte desteklendi mi?

Uygulama Proxy Bağlayıcısı, Azure'da sertifika tabanlı kimlik doğrulaması gerçekleştirir. TLS Sonlandırma (TLS/HTTPS denetimi veya ivmesi) bu kimlik doğrulama yöntemini bozar ve desteklenmez. Bağlayıcıdan Azure'a gelen trafik, TLS Sonlandırma gerçekleştiren tüm aygıtları atlamalıdır.  

### <a name="should-i-create-a-dedicated-account-to-register-the-connector-with-azure-ad-application-proxy"></a>Bağlayıcıyı Azure AD Application Proxy'ye kaydettirmek için özel bir hesap oluşturmalı mıyım?

Buna gerek yok. Herhangi bir genel yönetici veya uygulama yöneticisi hesabı çalışacaktır. Yükleme sırasında girilen kimlik bilgileri kayıt işleminden sonra kullanılmaz. Bunun yerine, bu noktadan itibaren kimlik doğrulaması için kullanılan bağlayıcıya bir sertifika verilir.

### <a name="how-can-i-monitor-the-performance-of-the-azure-ad-application-proxy-connector"></a>Azure AD Application Proxy bağlayıcısının performansını nasıl izleyebilirim?

Konektörle birlikte yüklenmiş Performans Monitörü sayaçları vardır. Bunları görüntülemek için:  

1. **Başlat'ı**seçin , "Perfmon" yazın ve ENTER tuşuna basın.
2. **Performans Monitörü'nü** **+** seçin ve yeşil simgesini tıklatın.
3. İzlemek istediğiniz **Microsoft AAD Application Proxy Bağlayıcı** sayaçlarını ekleyin.

### <a name="does-the-azure-ad-application-proxy-connector-have-to-be-on-the-same-subnet-as-the-resource"></a>Azure AD Application Proxy bağlayıcısının kaynakla aynı alt ağda olması gerekiyor mu?

Konektörün aynı alt ağda olması gerekmez. Ancak, kaynağa ad çözümlemesi (DNS, ana bilgisayar dosyası) ve gerekli ağ bağlantısı (kaynağa yönlendirme, kaynak üzerinde açık bağlantı noktaları, vb.) gerekir. Öneriler için Azure [Active Directory Application Proxy'yi kullanırken Ağ topolojisi hususlarına](application-proxy-network-topology.md)bakın.

## <a name="application-configuration"></a>Uygulama yapılandırması

### <a name="what-is-the-length-of-the-default-and-long-back-end-timeout-can-the-timeout-be-extended"></a>Varsayılan ve "uzun" arka uç zaman ayarı uzunluğu nedir? Zaman dilimi uzatılabilir mi?

Varsayılan uzunluk 85 saniyedir. "Uzun" ayarı 180 saniyedir. Zaman açısından sınır uzatılama

### <a name="how-do-i-change-the-landing-page-my-application-loads"></a>Başvurumun yüklerini alan sayfayı nasıl değiştirebilirim?

Uygulama Kayıtları sayfasından, ana sayfa URL'sini açılış sayfasının istenilen harici URL'si ile değiştirebilirsiniz. Uygulama Uygulamalarım'dan veya Office 365 Portalı'ndan başlatıldığında belirtilen sayfa yüklenir. Yapılandırma adımları için bkz: [Azure AD Uygulama Proxy'yi kullanarak yayımlanmış uygulamalar için özel bir giriş sayfası ayarlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-custom-home-page)

### <a name="can-only-iis-based-applications-be-published-what-about-web-applications-running-on-non-windows-web-servers-does-the-connector-have-to-be-installed-on-a-server-with-iis-installed"></a>Yalnızca IIS tabanlı uygulamalar yayınlanabilir mi? Windows dışı web sunucularında çalışan web uygulamaları ne olacak? Bağlayıcının IIS yüklü bir sunucuya yüklenmesi gerekiyor mu?

Hayır, yayınlanan uygulamalar için IIS gereksinimi yoktur. Windows Server dışındaki sunucularda çalışan web uygulamalarını yayımlayabilirsiniz. Ancak, web sunucusunun Negotiate 'i (Kerberos kimlik doğrulaması) desteklemesine bağlı olarak, Windows sunucusu olmayan bir sunucuyla ön kimlik doğrulamayı kullanamayabilirsiniz. Bağlayıcının yüklü olduğu sunucuda IIS gerekmez.

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

### <a name="when-should-i-use-the-principalsallowedtodelegatetoaccount-method-when-setting-up-kerberos-constrained-delegation-kcd"></a>Kerberos Kısıtlı Delegasyonu (KCD) kurarken PrincipalsAllowedToDelegateToAccount yöntemini ne zaman kullanmalıyım?

PrincipalsAllowedToDelegateToAccount yöntemi, bağlayıcı sunucular web uygulama hizmeti hesabından farklı bir etki alanında olduğunda kullanılır. Kaynak Tabanlı Kısıtlı Delegasyon kullanımını gerektirir.
Bağlayıcı sunucular ve web uygulama hizmeti hesabı aynı etki alanındaysa, bağlayıcı makine hesaplarının her birinde temsilci ayarlarını yapılandırmak için Etkin Dizin Kullanıcıları ve Bilgisayarlarını kullanarak hedef SPN'ye temsilci vermelerine izin verebilirsiniz.

Bağlayıcı sunucular ve web uygulama hizmeti hesabı farklı etki alanlarındaysa, Kaynak tabanlı temsilcilik kullanılır. Temsilcilik izinleri hedef web sunucusu ve web uygulama hizmeti hesabında yapılandırılır. Kısıtlı Delegasyon bu yöntem nispeten yenidir. Yöntem, kaynak (web hizmeti) sahibinin hangi makine ve hizmet hesaplarını ona devredebileceğini denetlemesine izin vererek etki alanı arası temsilciliği destekleyen Windows Server 2012'de tanıtıldı. Bu yapılandırmaya yardımcı olacak bir ui yok, bu nedenle PowerShell'i kullanmanız gerekir.
Daha fazla bilgi için, [Uygulama Proxy ile Kerberos Kısıtlı Delegasyon Anlayış](https://aka.ms/kcdpaper)beyaz kağıt bakın.

## <a name="pass-through-authentication"></a>Doğrudan kimlik doğrulama

### <a name="can-i-use-conditional-access-policies-for-applications-published-with-pass-through-authentication"></a>Geçiş kimlik doğrulaması ile yayınlanan uygulamalar için Koşullu Erişim İlkeleri kullanabilir miyim?

Koşullu Erişim İlkeleri yalnızca Azure AD'deki başarıyla önceden kimlik doğrulaması olan kullanıcılar için uygulanır. Geçiş kimlik doğrulaması Azure AD kimlik doğrulamasını tetiklemiyor, bu nedenle Koşullu Erişim İlkeleri uygulanamaz. Geçiş kimlik doğrulaması ile, MFA ilkeleri mümkünse şirket içi sunucuda veya Azure AD Application Proxy ile ön kimlik doğrulamayı etkinleştirerek uygulanmalıdır.

### <a name="can-i-publish-a-web-application-with-client-certificate-authentication-requirement"></a>İstemci sertifikası kimlik doğrulama gereksinimi olan bir web uygulaması yayımlayabilir miyim?

Hayır, Uygulama Proxy TLS trafiğini sonlandıracak çünkü bu senaryo desteklenmez.  

## <a name="remote-desktop-gateway-publishing"></a>Uzak Masaüstü Ağ Geçidi yayımlama

### <a name="how-can-i-publish-remote-desktop-gateway-over-azure-ad-application-proxy"></a>Azure AD Application Proxy üzerinden Uzak Masaüstü Ağ Geçidi'ni nasıl yayımlayabilirim?

Azure [AD Uygulama Proxy'si ile Uzak MasaüstüYayımla'ya](application-proxy-integrate-with-remote-desktop-services.md)bakın.

### <a name="can-i-use-kerberos-constrained-delegation-single-sign-on---windows-integrated-authentication-in-the-remote-desktop-gateway-publishing-scenario"></a>Uzak Masaüstü Ağ Geçidi yayımlama senaryosunda Kerberos Kısıtlı Temsilciliği 'ni (Tek Oturum Açma - Windows Tümleşik Kimlik Doğrulama) kullanabilir miyim?

Hayır, bu senaryo desteklenmiyor.  

### <a name="my-users-dont-use-internet-explorer-11-and-the-pre-authentication-scenario-doesnt-work-for-them-is-this-expected"></a>Kullanıcılarım Internet Explorer 11 kullanmıyor ve kimlik doğrulama öncesi senaryo onlar için çalışmıyor. Bu beklenen bir şey mi?

Evet, bekleniyor. Ön kimlik doğrulama senaryosu, üçüncü taraf tarayıcılarda desteklenmeyen bir ActiveX denetimi gerektirir.

### <a name="is-the-remote-desktop-web-client-html5-supported"></a>Uzak Masaüstü Web İstemci (HTML5) desteklendi mi?

Hayır, bu senaryo şu anda desteklenmiyor. Bu özellik teki güncellemeler için [UserVoice](https://aka.ms/aadapuservoice) geri bildirim forumumuzu takip edin.

### <a name="after-i-configured-the-pre-authentication-scenario-i-realized-that-the-user-has-to-authenticate-twice-first-on-the-azure-ad-sign-in-form-and-then-on-the-rdweb-sign-in-form-is-this-expected-how-can-i-reduce-this-to-one-sign-in"></a>Kimlik doğrulama öncesi senaryoyu yapılandırıldıktan sonra, kullanıcının iki kez kimlik doğrulaması gerektiğini fark ettim: önce Azure AD oturum açma formunda, sonra RDWeb oturum açma formunda. Bu beklenen bir şey mi? Bunu bir oturum açmaya nasıl indirebilirim?

Evet, bekleniyor. Kullanıcının bilgisayarı Azure AD'ye katıldıysa, kullanıcı otomatik olarak Azure AD'de kaydolur. Kullanıcının kimlik bilgilerini yalnızca RDWeb oturum açma formunda sağlaması gerekir.

## <a name="sharepoint-publishing"></a>SharePoint yayımlama

### <a name="how-can-i-publish-sharepoint-over-azure-ad-application-proxy"></a>Azure AD Application Proxy üzerinden SharePoint'i nasıl yayımlayabilirim?

Azure [AD Application Proxy ile SharePoint'e uzaktan erişimi etkinleştirme konusuna](application-proxy-integrate-with-sharepoint-server.md)bakın.

### <a name="can-i-use-the-sharepoint-mobile-app-ios-android-to-access-a-published-sharepoint-server"></a>Yayınlanan bir SharePoint sunucusuna erişmek için SharePoint mobil uygulamasını (iOS/ Android) kullanabilir miyim?

[SharePoint mobil uygulaması](https://docs.microsoft.com/sharepoint/administration/supporting-the-sharepoint-mobile-apps-online-and-on-premises) şu anda Azure Active Directory ön kimlik doğrulamasını desteklememektedir.

## <a name="active-directory-federation-services-ad-fs-publishing"></a>Aktif Dizin Federasyon Hizmetleri (AD FS) yayıncılık 

### <a name="can-i-use-azure-ad-application-proxy-as-ad-fs-proxy-like-web-application-proxy"></a>Azure AD Application Proxy'yi AD FS proxy olarak (Web Application Proxy gibi) kullanabilir miyim?

Hayır. Azure AD Application Proxy, Azure AD ile çalışmak üzere tasarlanmıştır ve AD FS proxy'si olarak hareket etme gereksinimlerini karşılamaz.

## <a name="websocket"></a>Websocket

### <a name="does-websocket-support-work-for-applications-other-than-qliksense"></a>WebSocket desteği QlikSense dışındaki uygulamalar için çalışıyor mu?

Şu anda, WebSocket iletişim kuralı desteği hala genel önizlemede dir ve diğer uygulamalar için çalışmayabilir. Bazı müşteriler, Diğer uygulamalarla WebSocket protokolünü kullanarak karışık başarı elde etmiştir. Bu tür senaryoları test ederseniz, sonuçlarınızı duymak isteriz. Lütfen geri bildiriminizi aadapfeedback@microsoft.combize gönderin.

Windows Yönetici Merkezi (WAC) veya Uzak Masaüstü Web İstemcisi'ndeki (HTML5) özellikler (Eventlogs, PowerShell ve Uzak Masaüstü Hizmetleri) şu anda Azure AD Application Proxy ile çalışmıyor.

## <a name="link-translation"></a>Bağlantı çevirisi

### <a name="does-using-link-translation-affect-performance"></a>Link çevirisini kullanmak performansı etkiler mi?

Evet. Bağlantı çevirisi performansı etkiler. Application Proxy hizmeti, uygulamayı sabit kodlu bağlantılar için tarar ve kullanıcıya sunmadan önce kendi yayınlanmış harici URL'leri ile değiştirir. 

En iyi performans için, özel etki alanlarını yapılandırarak aynı dahili ve harici [URL'leri](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-custom-domain)kullanmanızı öneririz. Özel etki alanlarını kullanmak mümkün değilse, Mobil cihazlarda Uygulamalarım Güvenli Oturum Açma Uzantısı veya Microsoft Edge Tarayıcısı'nı kullanarak bağlantı çevirisi performansını artırabilirsiniz. [Azure AD Application Proxy ile yayınlanan uygulamalar için yeniden yönlendirkodlu bağlantılara](application-proxy-configure-hard-coded-link-translation.md)bakın.

## <a name="wildcards"></a>Joker karakterler

### <a name="how-do-i-use-wildcards-to-publish-two-applications-with-the-same-custom-domain-name-but-with-different-protocols-one-for-http-and-one-for-https"></a>Aynı özel alan adı taşıyan, ancak biri HTTP, diğeri HTTPS için olmak üzere farklı protokollerle iki uygulama yayınlamak için joker karakterleri nasıl kullanırım?

Bu senaryo doğrudan desteklenmez. Bu senaryo için seçenekleriniz şunlardır:

1. Hem HTTP hem de HTTPS URL'lerini joker karakterli ayrı uygulamalar olarak yayımlayın, ancak her birine farklı bir özel etki alanı verin. Bu yapılandırma, farklı dış URL'leri olduğundan çalışır.

2. Joker karakter uygulaması aracılığıyla HTTPS URL'sini yayımlayın. Bu Application Proxy PowerShell cmdlets kullanarak ayrı ayrı HTTP uygulamaları yayımlayın:
   - [Uygulama Proxy Uygulama Yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management)
   - [Uygulama Proxy Konektörü Yönetimi](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_connector_management)
