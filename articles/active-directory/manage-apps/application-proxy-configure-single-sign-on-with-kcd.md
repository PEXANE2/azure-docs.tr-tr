---
title: Uygulama Proxy ile tek oturum açma | Microsoft Dokümanlar
description: Azure AD Application Proxy kullanarak tek oturum açma nın nasıl sağlayabileceğini kapsar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253461"
---
# <a name="kerberos-constrained-delegation-for-single-sign-on-to-your-apps-with-application-proxy"></a>Uygulama Proxy ile uygulamalarınızda tek oturum açma için Kerberos Kısıtlı Delegasyonu

Tümleşik Windows Kimlik Doğrulama ile güvenli olan Application Proxy aracılığıyla yayınlanan şirket içi uygulamalar için tek oturum açma sağlayabilirsiniz. Bu uygulamalar için erişim için bir Kerberos bilet gerektirir. Application Proxy bu uygulamaları desteklemek için Kerberos Kısıtlı Delegasyon (KCD) kullanır. 

Etkin Dizin'de Application Proxy bağlayıcılarına kullanıcıların kimliğine bürünme izni vererek, Tümleşik Windows Kimlik Doğrulama (IWA) kullanarak uygulamalarınızda tek oturum açma olanağı sağlayabilirsiniz. Bağlayıcılar, kendi adlarına jeton göndermek ve almak için bu izni kullanır.

## <a name="how-single-sign-on-with-kcd-works"></a>KCD ile tek oturum açma nasıl çalışır?
Bu diyagram, bir kullanıcı IWA kullanan bir şirket içi uygulamaya erişmeye çalıştığında akışı açıklar.

![Microsoft AAD kimlik doğrulama akış diyagramı](./media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. Kullanıcı, Uygulama Proxy aracılığıyla şirket içi uygulamaya erişmek için URL'yi girer.
2. Application Proxy, isteği Azure AD kimlik doğrulama hizmetlerine önceden kimlik doğrulamaya yönlendirir. Bu noktada, Azure AD çok faktörlü kimlik doğrulama gibi geçerli kimlik doğrulama ve yetkilendirme ilkeleri uygular. Kullanıcı doğrulanırsa, Azure AD bir belirteç oluşturur ve kullanıcıya gönderir.
3. Kullanıcı belirteci Uygulama Proxy'sine geçirir.
4. Application Proxy belirteci doğrular ve kullanıcı ana adı (UPN) ondan alır ve sonra Bağlayıcı upn çeker ve Servis Ana Adı (SPN) usulüne uygun olarak doğrulanmış güvenli bir kanal üzerinden.
5. Bağlayıcı uygulama için bir Kerberos belirteci almak için kullanıcı taklit, tesislerinde AD ile Kerberos Kısıtlı Delegasyon (KCD) müzakere gerçekleştirir.
6. Active Directory, uygulama için Kerberos belirteci'ni Bağlayıcı'ya gönderir.
7. Bağlayıcı, AD'den aldığı Kerberos belirteci kullanarak orijinal isteği uygulama sunucusuna gönderir.
8. Uygulama yanıtı Bağlayıcı'ya gönderir ve bu yanıt daha sonra Uygulama Proxy hizmetine ve son olarak da kullanıcıya döndürülür.

## <a name="prerequisites"></a>Ön koşullar
IWA uygulamaları için tek oturum açma ile başlamadan önce, ortamınızın aşağıdaki ayarlar ve yapılandırmalarla hazır olduğundan emin olun:

* SharePoint Web uygulamaları gibi uygulamalarınız Tümleşik Windows Kimlik Doğrulaması'nı kullanacak şekilde ayarlanmıştır. Daha fazla bilgi [için Kerberos Kimlik Doğrulaması Desteğini Etkinleştir'e](https://technet.microsoft.com/library/dd759186.aspx)veya SharePoint için [SharePoint 2013'teki Kerberos kimlik doğrulaması planına](https://technet.microsoft.com/library/ee806870.aspx)bakın.
* Tüm uygulamalarınızda [Servis Müdürü Adları](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx)vardır.
* Bağlayıcıyı çalıştıran sunucu ve uygulamayı çalıştıran sunucu, etki alanına katılır ve aynı etki alanının veya güvenilen etki alanlarının bir parçasıdır. Etki alanı birleştirme hakkında daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/dd807102.aspx)
* Bağlayıcıyı çalıştıran sunucu, kullanıcılar için TokenGroupsGlobalAndUniversal özniteliğini okumaya erişebilir. Bu varsayılan ayar, güvenlik ortamının sertleştirmesi tarafından etkilenmiş olabilir.

### <a name="configure-active-directory"></a>Active Directory'yi yapılandırma
Etkin Dizin yapılandırması, Uygulama Proxy bağlayıcınızın ve uygulama sunucunuzun aynı etki alanında olup olmadığına bağlı olarak değişir.

#### <a name="connector-and-application-server-in-the-same-domain"></a>Aynı etki alanında bağlayıcı ve uygulama sunucusu
1. Etkin Dizin'de **Araçlar** > **Kullanıcıları ve Bilgisayar'a**gidin.
2. Bağlayıcıyı çalıştıran sunucuyu seçin.
3. Sağ tıklayın ve **Özellikler** > **Delegasyonu**seçin.
4. **Bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**'i seçin. 
5. **Herhangi bir kimlik doğrulama protokolünü kullan**'ı seçin.
6. **Bu hesabın temsilci kimlik bilgilerini sunabileceği Hizmetler** altında, uygulama sunucusunun SPN kimliği için değer eklenir. Bu, Uygulama Proxy Bağlayıcısı'nın listede tanımlanan uygulamalara karşı AD'deki kullanıcıları taklit etmesini sağlar.

   ![Bağlayıcı-SVR Özellikleri pencere ekran görüntüsü](./media/application-proxy-configure-single-sign-on-with-kcd/Properties.jpg)

#### <a name="connector-and-application-server-in-different-domains"></a>Farklı etki alanlarında bağlayıcı ve uygulama sunucusu
1. Etki alanları arasında KCD ile çalışmak için ön koşulların bir listesi için, [etki alanları arasında Kerberos Kısıtlı Delegasyon](https://technet.microsoft.com/library/hh831477.aspx)bakın.
2. Uygulama `principalsallowedtodelegateto` Proxy 'sinden (bağlayıcı) Kerberos kimlik doğrulama delegasyonu sağlamak için web uygulamasının hizmet hesabının (bilgisayar veya özel etki alanı kullanıcı hesabı) özelliğini kullanın. Uygulama sunucusu bağlamında çalışıyor `webserviceaccount` ve delegating `connectorcomputeraccount`sunucusu. Aşağıdaki komutları etki alanında (Windows Server 2012 R2 veya sonraki `webserviceaccount`çalıştırma) etki alanında çalıştırın. Her iki hesap için düz adlar (UPN olmayan) kullanın.

   Bilgisayar `webserviceaccount` hesabı ise, aşağıdaki komutları kullanın:

   ```powershell
   $connector= Get-ADComputer -Identity connectorcomputeraccount -server dc.connectordomain.com

   Set-ADComputer -Identity webserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

   Get-ADComputer webserviceaccount -Properties PrincipalsAllowedToDelegateToAccount
   ```

   Kullanıcı `webserviceaccount` hesabı ise şu komutları kullanın:

   ```powershell
   $connector= Get-ADComputer -Identity connectorcomputeraccount -server dc.connectordomain.com

   Set-ADUser -Identity webserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

   Get-ADUser webserviceaccount -Properties PrincipalsAllowedToDelegateToAccount
   ```

## <a name="configure-single-sign-on"></a>Çoklu oturum açmayı yapılandırma 
1. [Uygulama Proxy ile uygulamaları yayımla'da](application-proxy-add-on-premises-application.md)açıklanan talimatlara göre başvurunuzu yayınlayın. **Ön Authentication Yöntemi**olarak **Azure Active Directory'yi** seçtiğinizden emin olun.
2. Uygulamanız kurumsal uygulamalar listesinde göründükten sonra, uygulamayı seçin ve **Tek oturum açma'yı**tıklatın.
3. Tek oturum açma modunu **Tümleşik Windows Kimlik Doğrulama'ya**ayarlayın.  
4. Uygulama sunucusunun **İç Uygulama SPN'sini** girin. Bu örnekte, yayınlanan uygulamamız için SPN http/www.contoso.com'dur. Bu SPN'nin bağlayıcının temsilci kimlik bilgilerini sunabileceği hizmetler listesinde olması gerekir. 
5. Bağlayıcının kullanıcılarınız adına kullanması için **Devredilen Giriş Kimliğini** seçin. Daha fazla bilgi için bkz: [Farklı şirket içi ve bulut kimlikleriyle çalışma](#working-with-different-on-premises-and-cloud-identities)

   ![Gelişmiş Uygulama Yapılandırması](./media/application-proxy-configure-single-sign-on-with-kcd/cwap_auth2.png)  


## <a name="sso-for-non-windows-apps"></a>Windows dışı uygulamalar için SSO

Azure AD Uygulama Proxy'deki Kerberos temsilci akışı, Azure AD'nin kullanıcıyı bulutta doğrulaması sırasında başlar. İstek şirket içinde geldiğinde, Azure AD Application Proxy bağlayıcısı yerel Active Directory ile etkileşimkurarak kullanıcı adına bir Kerberos bileti verir. Bu süreç Kerberos Kısıtlı Delegasyonu (KCD) olarak adlandırılır. Bir sonraki aşamada, bu Kerberos bileti ile arka uç uygulamasına bir istek gönderilir. 

Bu tür isteklerin nasıl gönderilebildiğini tanımlayan çeşitli protokoller vardır. Windows'a ait olmayan sunucuların çoğu SPNEGO ile görüşmeyi bekler. Bu protokol Azure AD Application Proxy'de desteklenir, ancak varsayılan olarak devre dışı bırakılır. Bir sunucu SPNEGO veya standart KCD için yapılandırılabilir, ancak her ikisi için de yapılandırılamaz.

SPNEGO için bir konnektör makinesini yapılandırırsanız, bu Bağlayıcı grubundaki diğer tüm konektörlerin de SPNEGO ile yapılandırıldığından emin olun. Standart KCD bekleyen uygulamalar, SPNEGO için yapılandırılmamış diğer bağlayıcılar aracılığıyla yönlendirilmelidir.
 

SPNEGO'u etkinleştirmek için:

1. Yönetici olarak çalışan bir komut istemi açın.
2. Komut isteminden, SPNEGO'ya ihtiyaç duyan bağlayıcı sunucularda aşağıdaki komutları çalıştırın.

    ```
    REG ADD "HKLM\SOFTWARE\Microsoft\Microsoft AAD App Proxy Connector" /v UseSpnegoAuthentication /t REG_DWORD /d 1
    net stop WAPCSvc & net start WAPCSvc
    ```

Kerberos hakkında daha fazla bilgi için, [Tüm Kerberos Kısıtlı Delegasyon (KCD) hakkında bilmek istiyorum](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd)bakın.

Windows dışı uygulamalar genellikle etki alanı e-posta adresleri yerine kullanıcı kullanıcı adları veya SAM hesap adları. Bu durum uygulamalarınız için geçerliyse, bulut kimliklerinizi uygulama kimliklerinize bağlamak için devredilen oturum açma kimlik alanını yapılandırmanız gerekir. 

## <a name="working-with-different-on-premises-and-cloud-identities"></a>Farklı şirket içi ve bulut kimlikleriyle çalışma
Application Proxy, kullanıcıların bulutta ve şirket içinde tam olarak aynı kimliğe sahip olduğunu varsayar. Ancak bazı ortamlarda, şirket ilkeleri veya uygulama bağımlılıkları nedeniyle, kuruluşların oturum açma için alternatif kimlikler kullanması gerekebilir. Bu gibi durumlarda, kcd'yi tek oturum açma için kullanmaya devam edebilirsiniz. Tek oturum açma yaparken hangi kimliğin kullanılması gerektiğini belirtmek için her uygulama için **Bir Temsilci giriş kimliği** yapılandırın.  

Bu özellik, farklı şirket içi ve bulut kimliklerine sahip birçok kuruluşun, kullanıcıların farklı kullanıcı adları ve parolalar girmelerine gerek kalmadan SSO'yu buluttan şirket içi uygulamalara sahip olmasını sağlar. Bu, şunları içeren organizasyonları içerir:

* Birden çok etki alanınajoe@us.contoso.com joe@eu.contoso.com(, ) ve buluttajoe@contoso.comtek bir etki alanına ( ) sahip olun.
* Dahili olarak routable olmayanjoe@contoso.usaetki alanı adı () ve bulutta yasal bir ad var.
* Alan adlarını dahili olarak kullanmayın (joe)
* Şirket içinde ve bulutta farklı takma adlar kullanın. Örneğin, joe-johns@contoso.com vs.joej@contoso.com  

Application Proxy ile Kerberos biletini almak için hangi kimliği kullanacağınızı seçebilirsiniz. Bu ayar uygulama başınadır. Bu seçeneklerden bazıları e-posta adresi biçimini kabul etmemektedir sistemleri için uygundur, diğerleri alternatif giriş için tasarlanmıştır.

![Temsilci giriş kimlik parametresi ekran görüntüsü](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_upn.png)

Devredilen oturum açma kimliği kullanılırsa, değer kuruluşunuzdaki tüm etki alanlarında veya ormanlarda benzersiz olmayabilir. İki farklı Bağlayıcı grup kullanarak bu uygulamaları iki kez yayımlayarak bu sorunu önleyebilirsiniz. Her uygulamanın farklı bir kullanıcı kitlesi olduğundan, bağlayıcılarını farklı bir etki alanına katılabilirsiniz.

### <a name="configure-sso-for-different-identities"></a>Farklı kimlikler için SSO'ya yapı
1. Azure AD Connect ayarlarını ana kimliğin e-posta adresi (posta) olması için yapılandırın. Bu, eşitleme ayarlarındaki **Kullanıcı Temel Adı** alanını değiştirerek özelleştirme işleminin bir parçası olarak yapılır. Bu ayarlar, kullanıcıların Office365, Windows10 aygıtlarında ve kimlik deposu olarak Azure AD kullanan diğer uygulamalarda nasıl oturum açtıklarını da belirler.  
   ![Kullanıcıların ekran görüntüsünü belirleme - Kullanıcı Adı açılır](./media/application-proxy-configure-single-sign-on-with-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. Değiştirmek istediğiniz uygulamanın Uygulama Yapılandırma ayarlarında, kullanılacak **Temsilci Giriş Kimliğini** seçin:

   * Kullanıcı Adı (örneğin, joe@contoso.com)
   * Alternatif Kullanıcı Anaadı (örneğin, joed@contoso.local)
   * Kullanıcı Adı'nın kullanıcı adı bölümü (örneğin, joe)
   * Alternatif Kullanıcı Adı'nın kullanıcı adı bölümü (örneğin, joed)
   * Şirket içi SAM hesap adı (etki alanı denetleyiciyapılandırması bağlıdır)

### <a name="troubleshooting-sso-for-different-identities"></a>Farklı kimlikler için SSO sorun giderme
SSO işleminde bir hata varsa, [Sorun Giderme'de](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)açıklandığı gibi bağlayıcı makine olay günlüğünde görünür.
Ancak, bazı durumlarda, bu uygulama çeşitli diğer HTTP yanıtlarında yanıtlar ise istek başarıyla arka uç uygulamasına gönderilir. Bu servis taleplerini gidermeye, Uygulama Proxy oturumu olay günlüğündeki bağlayıcı makinedeki 24029 olay numarasını inceleyerek başlamalıdır. Temsilcilik için kullanılan kullanıcı kimliği, olay ayrıntıları içinde "kullanıcı" alanında görünür. Oturum günlüğünü açmak için, olay görüntüleyicisi görünüm menüsünde **analitik ve hata ayıklama günlüklerini göster'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kerberos Kısıtlı Temsilciliği kullanmak için bir Uygulama Proxy uygulaması yapılandırmak için nasıl](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)


En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](https://blogs.technet.com/b/applicationproxyblog/) göz atın
