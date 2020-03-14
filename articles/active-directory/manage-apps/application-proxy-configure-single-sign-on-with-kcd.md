---
title: Uygulama proxy'si ile çoklu oturum açma | Microsoft Docs
description: Tek Azure AD uygulama ara sunucusu kullanarak oturum açmayı sağlamak nasıl etkinleştireceğinizi de açıklar.
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
ms.date: 08/13/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: H1Hack27Feb2017, it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5948fba67d3f071d77192f9ad89bc696fdc0c3cc
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79253461"
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-to-your-apps-with-application-proxy"></a>Kerberos kısıtlanmış temsil için çoklu oturum açma uygulamalarınıza uygulama ara sunucusu ile

Şirket içi için çoklu oturum açma sağlayabilir yayımlanan uygulamaları aracılığıyla uygulama proxy'si tümleşik Windows kimlik doğrulaması ile güvenli hale getirilir. Bu uygulamalara erişim için bir Kerberos anahtarı gerektirir. Uygulama proxy'si, bu uygulamaları desteklemek için Kerberos Kısıtlı temsilci (KCD) kullanır. 

Kullanıcının kimliğine bürünmek için Active Directory Uygulama Proxy bağlayıcıları izni vererek tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalarınızı çoklu oturum açmayı etkinleştirebilirsiniz. Bağlayıcılar, göndermek ve kendileri adına belirteçlerini almak için bu izni kullanın.

## <a name="how-single-sign-on-with-kcd-works"></a>KCD works ile nasıl çoklu oturum açma
Bu diyagramda, bir Kullanıcı ıWA kullanan bir şirket içi uygulamaya erişmeyi denediğinde akış açıklanır.

![Microsoft AAD kimlik doğrulaması akışı diyagramı](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Kullanıcı, uygulama proxy 'si aracılığıyla şirket içi uygulamaya erişmek için URL 'YI girer.
2. Uygulama Ara sunucusu için Azure AD kimlik doğrulama hizmetleri preauthenticate için istek yönlendirir. Bu noktada, Azure AD, tüm geçerli kimlik doğrulama ve yetkilendirme ilkeleri, çok faktörlü kimlik doğrulaması gibi uygulanır. Kullanıcı doğrulanmış olması durumunda, Azure AD belirteç oluşturulur ve kullanıcıya gönderir.
3. Kullanıcı belirteci uygulama ara sunucusuna iletir.
4. Uygulama proxy 'Si belirteci doğrular ve bundan sonra Kullanıcı asıl adını (UPN) alır ve bağlayıcı, kimliği doğrulanmış bir güvenli kanal aracılığıyla UPN 'yi ve hizmet asıl adı 'nı (SPN) çeker.
5. Bağlayıcı, şirket içi AD ile Kerberos kısıtlı temsili (KCD) anlaşması gerçekleştirerek, kullanıcının uygulamaya bir Kerberos belirteci almasını sağlar.
6. Active Directory Uygulama bağlayıcısı için Kerberos belirteci gönderir.
7. Bağlayıcı, özgün istek AD'den alınan Kerberos belirteci kullanarak uygulama sunucusuna gönderir.
8. Uygulama için uygulama proxy'si hizmeti ardından döndürülen bağlayıcı yanıta gönderir ve son kullanıcı.

## <a name="prerequisites"></a>Önkoşullar
IWA uygulamalar için çoklu oturum açma ile çalışmaya başlamadan önce aşağıdaki ayarları ve yapılandırmaları ile ortamınızı hazır olduğundan emin olun:

* Uygulamalarınızı, SharePoint Web uygulamaları gibi tümleşik Windows kimlik doğrulaması kullanacak şekilde ayarlanmıştır. Daha fazla bilgi için bkz. [Kerberos kimlik doğrulaması desteğini etkinleştirme](https://technet.microsoft.com/library/dd759186.aspx)veya SharePoint için bkz. [SharePoint 2013 'de Kerberos kimlik doğrulaması planı](https://technet.microsoft.com/library/ee806870.aspx).
* Tüm uygulamalarınızın [hizmet sorumlusu adları](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx)vardır.
* Bağlayıcıyı çalıştıran sunucu ve uygulama çalıştıran sunucunun etki alanına katılan ve aynı etki alanında veya güvenilen etki alanlarını bir parçası olan. Etki alanına ekleme hakkında daha fazla bilgi için bkz. [bir bilgisayarı etki alanına ekleme](https://technet.microsoft.com/library/dd807102.aspx).
* Bağlayıcısını çalıştıran sunucuda TokenGroupsGlobalAndUniversal öznitelik kullanıcılar için okuma erişimi var. Bu varsayılan ayarı ortam sağlamlaştırma güvenlik tarafından etkilenmiş.

### <a name="configure-active-directory"></a>Active Directory'yi yapılandırma
Active Directory yapılandırması, uygulama ara sunucusu bağlayıcısını ve uygulama sunucusu aynı etki alanında veya olmanıza bağlı olarak değişir.

#### <a name="connector-and-application-server-in-the-same-domain"></a>Bağlayıcı ve uygulama sunucusu aynı etki alanında
1. Active Directory ' de, **Kullanıcılar ve bilgisayarlar** > **Araçlar** ' a gidin.
2. Bağlayıcısını çalıştıran sunucuyu seçin.
3. Sağ tıklayın ve > temsili **Özellikler** 'i seçin.
4. **Bu bilgisayara yalnızca belirtilen hizmetlere yetki vermek Için güven**' i seçin. 
5. **Herhangi bir kimlik doğrulama protokolünü kullan**' ı seçin.
6. **Bu hesabın Temsilcili kimlik bilgileri Sunsunulabileceği hizmetler** ' in altında, uygulama sunucusunun SPN kimliği için değeri ekleyin. Bu listede tanımlanan uygulamalarla AD'de kullanıcının kimliğine bürünmek uygulama ara sunucusu Bağlayıcısı sağlar.

   ![Bağlayıcı SVR Özellikler penceresi ekran görüntüsü](./media/application-proxy-configure-single-sign-on-with-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Bağlayıcı ve uygulama sunucusu farklı etki alanlarında
1. Etki alanlarında KCD ile çalışmaya yönelik önkoşulların listesi için bkz. [etki alanları arasında Kerberos kısıtlanmış temsili](https://technet.microsoft.com/library/hh831477.aspx).
2. Uygulama proxy 'sinden (bağlayıcı) Kerberos kimlik doğrulaması temsilcisini etkinleştirmek için Web uygulamasının hizmet hesabının (bilgisayar veya adanmış etki alanı kullanıcı hesabı) `principalsallowedtodelegateto` özelliğini kullanın. Uygulama sunucusu `webserviceaccount` bağlamında çalışıyor ve temsilci sunucu `connectorcomputeraccount`. `webserviceaccount`etki alanında bir etki alanı denetleyicisinde (Windows Server 2012 R2 veya üstünü çalıştıran) aşağıdaki komutları çalıştırın. Her iki hesap için düz adlar (UPN olmayan) kullanın.

   `webserviceaccount` bir bilgisayar hesabı ise şu komutları kullanın:

   ```powershell
   $connector= Get-ADComputer -Identity connectorcomputeraccount -server dc.connectordomain.com

   Set-ADComputer -Identity webserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

   Get-ADComputer webserviceaccount -Properties PrincipalsAllowedToDelegateToAccount
   ```

   `webserviceaccount` bir kullanıcı hesabı ise şu komutları kullanın:

   ```powershell
   $connector= Get-ADComputer -Identity connectorcomputeraccount -server dc.connectordomain.com

   Set-ADUser -Identity webserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

   Get-ADUser webserviceaccount -Properties PrincipalsAllowedToDelegateToAccount
   ```

## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırma 
1. Uygulamanızı [uygulama proxy 'si Ile yayımlama](application-proxy-add-on-premises-application.md)bölümünde açıklanan yönergelere göre yayımlayın. **Ön kimlik doğrulama yöntemi**olarak **Azure Active Directory** ' yi seçtiğinizden emin olun.
2. Uygulamanız kurumsal uygulamalar listesinde görüntülendikten sonra, seçin ve **Çoklu oturum açma**' ya tıklayın.
3. Çoklu oturum açma modunu **Tümleşik Windows kimlik doğrulaması**olarak ayarlayın.  
4. Uygulama sunucusunun **Iç uygulama SPN** 'sini girin. Bu örnekte, http/www.contoso.com yayımlanan uygulamamız için SPN'dir. Bu SPN, istediğiniz bağlayıcıyı temsilci seçilen kimlik bilgilerini sunacağı Hizmetler listesinde olması gerekiyor. 
5. Bağlayıcının Kullanıcı adına kullanması için **yetkilendirilmiş oturum açma kimliğini** seçin. Daha fazla bilgi için bkz. [farklı şirket içi ve bulut kimlikleriyle çalışma](#working-with-different-on-premises-and-cloud-identities)

   ![Gelişmiş uygulama yapılandırması](./media/application-proxy-configure-single-sign-on-with-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Windows olmayan uygulamaları için SSO

Azure AD bulut kullanıcı kimliği doğruladığında Azure AD uygulama proxy'si Kerberos temsilci akışında başlatır. İstek, şirket içi geldikten sonra Azure AD uygulama ara sunucusu Bağlayıcısı'nı yerel Active Directory ile etkileşim kurarak kullanıcı adına bir Kerberos anahtarı verir. Bu işlem, Kerberos Kısıtlı temsilci (KCD) olarak adlandırılır. Sonraki aşamada, bu Kerberos anahtarı ile arka uç uygulaması için bir istek gönderilir. 

Bu tür istekleri göndermek nasıl tanımlayan birçok Protokolü vardır. Çoğu Windows olmayan sunucuları ile SPNEGO anlaşma beklenir. Bu protokol, Azure AD uygulama ara sunucusu desteklenir, ancak varsayılan olarak devre dışıdır. Bir sunucu SPNEGO veya standart KCD için yapılandırılmış, ancak ikisiyle olabilir.

Bağlayıcı makinesinde SPNEGO için yapılandırırsanız, diğer tüm bağlayıcılar, bağlayıcı grubunda SPNEGO ile yapılandırılmış olduğundan emin olun. Uygulama standart KCD SPNEGO için yapılandırılmamış olan diğer bağlayıcılar üzerinden yönlendirilmesi.
 

SPNEGO etkinleştirmek için:

1. Yönetici olarak çalıştırılan bir komut istemi açın.
2. Komut İstemi'nden SPNEGO gereken bağlayıcı sunucuları üzerinde aşağıdaki komutları çalıştırın.

    ```
    REG ADD "HKLM\SOFTWARE\Microsoft\Microsoft AAD App Proxy Connector" /v UseSpnegoAuthentication /t REG_DWORD /d 1
    net stop WAPCSvc & net start WAPCSvc
    ```

Kerberos hakkında daha fazla bilgi için bkz. [Kerberos kısıtlı temsili (KCD) hakkında bilgi edinmek](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd)istediğiniz.

Windows olmayan uygulamalar genellikle kullanıcı kullanıcı adını veya etki alanı yerine SAM hesabı adları e-posta adresleri. Bu durum, uygulamalarınız için geçerliyse, uygulama kimliklerinizi, bulut kimlikleri bağlamak için temsilci oturum açma kimlik alanı'nı yapılandırmanız gerekir. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Farklı şirket içi ve bulut kimlikleri ile çalışma
Uygulama proxy'si, kullanıcıların bulutta ve şirket içinde tam olarak aynı kimliğe sahip olduğunu varsayar. Ancak, bazı ortamlarda şirket ilkeleri veya uygulama bağımlılıkları nedeniyle, kuruluşların oturum açma için alternatif kimlikler kullanması gerekebilir. Bu gibi durumlarda, yine de KCD 'yi çoklu oturum açma için kullanmaya devam edebilirsiniz. Her uygulama için, çoklu oturum açma gerçekleştirirken hangi kimliğin kullanılması gerektiğini belirtmek üzere, **yetkilendirilmiş bir oturum açma kimliği** yapılandırın.  

Bu özellik, farklı şirket içi ve bulut kimlikleri SSO kullanıcıların farklı kullanıcı adları ve parolaları girmeye gerek kalmadan buluttan şirket içi uygulamalara sahip olan birçok kuruluş sağlar. Bu, kuruluşların içerir:

* Bulutta birden çok etki alanı (joe@us.contoso.com, joe@eu.contoso.com) ve tek bir etki alanı (joe@contoso.com) vardır.
* Bir bulutta (joe@contoso.usa) ve bir yasal olarak yönlendirilemeyen etki alanı adı vardır.
* Etki alanı adları iç kullanmayın (ALi)
* Şirket içinde ve bulutta farklı diğer adlar kullanın. Örneğin, joe-johns@contoso.com ve joej@contoso.com karşılaştırması  

Uygulama proxy'si, Kerberos anahtarını almak için kullanılacak kimliği seçebilirsiniz. Bu ayar bir uygulamadır. Bu seçeneklerden bazısı e-posta adresi biçimi kabul sistemler için uygundur, diğer alternatif oturum açma için tasarlanmıştır.

![Temsilci oturum açma kimlik parametresi ekran görüntüsü](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_upn.png)

Temsilci oturum açma kimliği kullanılırsa, değeri etki alanları veya kuruluşunuzdaki ormanlar arasında benzersiz olmayabilir. İki kez iki farklı bağlayıcı grupları kullanarak bu uygulamaları yayımlayarak, bu sorunu önleyebilirsiniz. Her uygulama farklı bir kullanıcı bir dinleyici olduğundan, kendi bağlayıcılar farklı bir etki alanına katılmasını sağlayabilirsiniz.

### <a name="configure-sso-for-different-identities"></a>SSO için farklı kimliklere yapılandırın
1. Asıl kimlik e-posta adresi (posta), bu nedenle, Azure AD Connect ayarlarını yapılandırın. Bu, eşitleme ayarlarındaki **Kullanıcı asıl adı** alanını değiştirerek özelleştirme işleminin bir parçası olarak yapılır. Bu ayarlar, ayrıca kullanıcıların Office 365, Windows 10 cihazları nasıl oturum belirler ve bunların kimlik deposu olarak Azure AD'yi kullanan diğer uygulamalar.  
   ![kullanıcıları tanımlama ekran görüntüsü-Kullanıcı asıl adı açılan listesi](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Değiştirmek istediğiniz uygulamanın uygulama yapılandırma ayarları ' nda, kullanılacak **atanmış oturum açma kimliğini** seçin:

   * Kullanıcı asıl adı (örneğin, joe@contoso.com)
   * Alternatif Kullanıcı asıl adı (örneğin, joed@contoso.local)
   * Kullanıcı adı, kullanıcı asıl adı (örneğin, ALi) parçası
   * Kullanıcı adı bölümü diğer kullanıcı asıl adı (örneğin, joed)
   * Şirket içi SAM hesabı adı (etki alanı denetleyicisi yapılandırmasına bağlıdır)

### <a name="troubleshooting-sso-for-different-identities"></a>SSO için farklı kimliklere sorunlarını giderme
SSO işleminde bir hata varsa, [sorun giderme](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)bölümünde açıklandığı gibi bağlayıcı makinesi olay günlüğünde görüntülenir.
Ancak, bu uygulama diğer çeşitli HTTP yanıtlarında yanıtlar karşın bazı durumlarda, istek başarıyla arka uç uygulaması için gönderilir. Bu gibi durumlarda sorun giderme bağlayıcı makinede uygulama ara sunucusu oturum olay günlüğündeki olay numarası 24029 inceleyerek başlamanız gerekir. Olay Ayrıntıları içinde "kullanıcı" alanında temsilci seçme için kullanılan kullanıcı kimliğini görünür. Oturum günlüğünü açmak için Olay Görüntüleyicisi Görünüm menüsünde **analitik ve hata ayıklama günlüklerini göster** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kerberos kısıtlanmış temsilciyi kullanmak için bir uygulama proxy uygulaması yapılandırma](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Uygulama proxy 'Si ile karşılaştığınız sorunları giderme](application-proxy-troubleshoot.md)


En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](https://blogs.technet.com/b/applicationproxyblog/) göz atın
