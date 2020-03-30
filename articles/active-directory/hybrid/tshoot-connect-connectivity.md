---
title: 'Azure AD Bağlantısı: Azure AD bağlantı sorunlarını giderme | Microsoft Dokümanlar'
description: Azure AD Connect ile bağlantı sorunlarını nasıl gidereceklerini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72dbb404d1b4d3618909e0233f332d2f98b51516
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049728"
---
# <a name="troubleshoot-azure-ad-connectivity"></a>Azure AD bağlantısıyla sorun giderme
Bu makalede, Azure AD Connect ve Azure AD arasındaki bağlantının nasıl çalıştığı ve bağlantı sorunlarının nasıl giderilenin açıklanmaktadır. Bu sorunlar büyük olasılıkla proxy sunucusu olan bir ortamda görülebilir.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Yükleme sihirbazında bağlantı sorunlarını giderme
Azure AD Connect, kimlik doğrulaması için Modern Kimlik Doğrulama'yı (ADAL kitaplığını kullanarak) kullanıyor. Yükleme sihirbazı ve eşitleme motoru düzgün bu iki .NET uygulamaları olduğundan düzgün yapılandırılmış olması için machine.config gerektirir.

Bu makalede, Fabrikam'ın proxy'si aracılığıyla Azure AD'ye nasıl bağlandığı gösteriş yapıyoruz. Proxy sunucusu fabrikamproxy adlı ve port 8080 kullanıyor.

Önce [**makine.config'in**](how-to-connect-install-prerequisites.md#connectivity) doğru bir şekilde yapılandırıldığından emin olmalıyız.  
![machineconfig](./media/tshoot-connect-connectivity/machineconfig.png)

> [!NOTE]
> Microsoft'a ait olmayan bazı bloglarda, değişikliklerin yerine miiserver.exe.config'de yapılması gerektiği belgelenmiştir. Ancak, bu dosya her yükseltmede üzerine yazılır, bu nedenle ilk yükleme sırasında çalışsa bile, sistem ilk yükseltme üzerinde çalışmayı durdurur. Bu nedenle, öneri machine.config yerine güncellemektir.
>
>

Proxy sunucusunun gerekli URL'leri de açmış olması gerekir. Resmi liste [Office 365 URL'leri ve IP adresi aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)belgelenmiştir.

Bu URL'ler arasında, Azure AD'ye hiç bağlanabilmek için aşağıdaki tablo mutlak minimumdur. Bu liste, parola yazma veya Azure AD Connect Health gibi isteğe bağlı özellikleri içermez. İlk yapılandırma için sorun giderme de yardımcı olmak için burada belgelenmiştir.

| URL'si | Bağlantı noktası | Açıklama |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*.verisign.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*.entrust.net |HTTP/80 |MFA için CRL listelerini indirmek için kullanılır. |
| \*.windows.net |HTTPS/443 SAYıLı |Azure AD'de oturum açmaiçin kullanılır. |
| secure.aadcdn.microsoftonline-p.com |HTTPS/443 SAYıLı |MFA için kullanılır. |
| \*.microsoftonline.com |HTTPS/443 SAYıLı |Azure AD dizininizi yapılandırmak ve veri aktarmak/aktarmak için kullanılır. |

## <a name="errors-in-the-wizard"></a>Sihirbazdaki hatalar
Yükleme sihirbazı iki farklı güvenlik bağlamı kullanıyor. Sayfada **Azure AD'ye bağlan,** şu anda oturum açmış kullanıcıyı kullanıyor. Sayfada **Yapılandırma**, [eşitleme motoru için hizmet çalıştıran hesaba](reference-connect-accounts-permissions.md#adsync-service-account)değişiyor. Bir sorun varsa, proxy yapılandırması genel olduğundan, büyük olasılıkla sihirbazdaki **Azure AD'ye Bağlan** sayfasında zaten görünür.

Aşağıdaki sorunlar yükleme sihirbazında karşılaştığınız en yaygın hatalardır.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Yükleme sihirbazı doğru şekilde yapılandırılmadı
Sihirbazın kendisi proxy'ye ulaşamıyorsa bu hata görüntülenir.  
![nomachineconfig](./media/tshoot-connect-connectivity/nomachineconfig.png)

* Bu hatayı görürseniz, [machine.config'in](how-to-connect-install-prerequisites.md#connectivity) doğru şekilde yapılandırıldığını doğrulayın.
* Bu doğruysa, sorunun sihirbazın dışında da bulunuyu pkıştırıp var olmadığını görmek için [proxy bağlantısını doğrula'daki](#verify-proxy-connectivity) adımları izleyin.

### <a name="a-microsoft-account-is-used"></a>Bir Microsoft hesabı kullanılır
**Okul veya kuruluş** hesabı yerine bir Microsoft **hesabı** kullanıyorsanız, genel bir hata görürsünüz.  
![Microsoft Hesabı kullanılır](./media/tshoot-connect-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA bitiş noktasına ulaşılamıyor
Bitiş noktasına **https://secure.aadcdn.microsoftonline-p.com** ulaşılamıyorsa ve global yöneticiniz MFA etkinleştirilmişse bu hata görüntülenir.  
![nomachineconfig](./media/tshoot-connect-connectivity/nomicrosoftonlinep.png)

* Bu hatayı görürseniz, bitiş noktası **secure.aadcdn.microsoftonline-p.com** proxy'ye eklendiğini doğrulayın.

### <a name="the-password-cannot-be-verified"></a>Parola doğrulanamıyor
Yükleme sihirbazı Azure AD'ye bağlanmada başarılı olursa, ancak parolanın kendisi doğrulanamıyorsa bu hatayı görürsünüz:  
![Kötü parola.](./media/tshoot-connect-connectivity/badpassword.png)

* Parola geçici bir parola mıdır ve değiştirilmelidir? Aslında doğru şifre mi? Oturum açmayı `https://login.microsoftonline.com` deneyin (Azure AD Connect sunucusundan başka bir bilgisayarda) ve hesabın kullanılabilir olduğunu doğrulayın.

### <a name="verify-proxy-connectivity"></a>Proxy bağlantısını doğrulama
Azure AD Connect sunucusunun Proxy ve Internet ile gerçek bağlantısı olup olmadığını doğrulamak için, proxy'nin web isteklerine izin verilip vermediğini görmek için bazı PowerShell'i kullanın. PowerShell isteminde çalıştırın. `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc` (Teknik olarak ilk çağrı `https://login.microsoftonline.com` ve bu URI de çalışır, ancak diğer URI yanıt vermek için daha hızlıdır.)

PowerShell proxy'ye başvurmak için machine.config'deki yapılandırmayı kullanır. winhttp/netsh'teki ayarlar bu cmdlets'i etkilememelidir.

Proxy doğru yapılandırılmışise, bir başarı durumu almalısınız: ![proxy200](./media/tshoot-connect-connectivity/invokewebrequest200.png)

Uzak **sunucuya bağlanamıyorsanız,** PowerShell proxy'yi kullanmadan doğrudan arama yapmaya çalışıyor veya DNS doğru şekilde yapılandırılamıyor. **Machine.config** dosyasının doğru şekilde yapılandırıldığından emin olun.
![bağlanamıyor](./media/tshoot-connect-connectivity/invokewebrequestunable.png)

Proxy doğru yapılandırılmamışsa, bir hata ![olsun: proxy200](./media/tshoot-connect-connectivity/invokewebrequest403.png)
![proxy407](./media/tshoot-connect-connectivity/invokewebrequest407.png)

| Hata | Hata Metni | Açıklama |
| --- | --- | --- |
| 403 |Yasak |Proxy istenen URL için açılmadı. Proxy yapılandırmasını yeniden ziyaret edin ve [URL'lerin](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açıldığından emin olun. |
| 407 |Proxy Kimlik Doğrulama Gerekli |Proxy sunucusu oturum açma gerektiriyordu ve hiçbiri sağlanmadı. Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, bu ayarın machine.config'de yapılandırıldığından emin olun. Ayrıca sihirbazı çalıştıran kullanıcı ve hizmet hesabı için etki alanı hesaplarını kullandığınızdan emin olun. |

### <a name="proxy-idle-timeout-setting"></a>Proxy boşta zaman ayarı
Azure AD Connect, Azure AD'ye bir dışa aktarım isteği gönderdiğinde, Yanıt oluşturmadan önce Azure AD'nin isteği işlemesi 5 dakika kadar sürebilir. Bu, özellikle aynı dışa aktarma isteğine dahil büyük grup üyelikleri olan grup nesneleri varsa gerçekleşebilir. Proxy boşta zaman adedinin 5 dakikadan büyük olacak şekilde yapılandırıldığından emin olun. Aksi takdirde, Azure AD Connect sunucusunda Azure AD ile aralıklı bağlantı sorunu görülebilir.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD Connect ve Azure AD arasındaki iletişim deseni
Tüm bu önceki adımları izlediyseniz ve hala bağlanamıyorsanız, bu noktada ağ günlüklerine bakmaya başlayabilirsiniz. Bu bölüm, normal ve başarılı bir bağlantı deseni belgelediriyor. Ayrıca ağ günlükleri okurken göz ardı edilebilir ortak kırmızı ringa listeleri.

* Aramalar `https://dc.services.visualstudio.com`var. Yüklemenin başarılı olması için proxy'de bu URL'nin açık olması gerekmez ve bu çağrılar yoksayılabilir.
* DNS çözümlemenin, dns ad alanında nsatc.net ve microsoftonline.com altında olmayan diğer ad alanlarında gerçek ana bilgisayarları listelediğini görürsünüz. Ancak, gerçek sunucu adlarında herhangi bir web hizmeti isteği yoktur ve bu URL'leri proxy'ye eklemeniz gerekmez.
* Bitiş noktaları adminwebservice ve provisioningapi bulma uç noktaları ve kullanmak için gerçek bitiş noktasını bulmak için kullanılır. Bu uç noktalar bölgenize bağlı olarak farklıdır.

### <a name="reference-proxy-logs"></a>Referans proxy günlükleri
Burada gerçek bir proxy günlüğünden bir döküm ve alındığı yerden yükleme sihirbazı sayfası (aynı uç noktaya yinelenen girişler kaldırıldı). Bu bölüm, kendi proxy ve ağ günlükleri için bir başvuru olarak kullanılabilir. Gerçek uç noktalar ortamınızda farklı olabilir (özellikle *italik*url'ler).

**Azure AD'ye Bağlanma**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:31 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:31 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://*bba800-çapa*.microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://*bwsc02-röle*.microsoftonline.com:443 |

**Yapılandır**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://*bba800-çapa*.microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba900-çapa*.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba800-çapa*.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://*bwsc02-röle*.microsoftonline.com:443 |

**İlk Eşitleme**

| Zaman | URL'si |
| --- | --- |
| 1/11/2016 8:48 |connect://login.windows.net:443 |
| 1/11/2016 8:49 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba900-çapa*.microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba800-çapa*.microsoftonline.com:443 |

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları
Bu bölüm, ADAL (Azure AD Connect tarafından kullanılan kimlik doğrulama kitaplığı) ve PowerShell'den döndürülebilen hataları kapsar. Açıklanan hata, sonraki adımlarınızı anlamanıza yardımcı olur.

### <a name="invalid-grant"></a>Geçersiz Hibe
Geçersiz kullanıcı adı veya parola. Daha fazla bilgi için [bkz.](#the-password-cannot-be-verified)

### <a name="unknown-user-type"></a>Bilinmeyen Kullanıcı Türü
Azure REKLAM dizininizde bulunamaz veya çözümlenemez. Doğrulanmamış bir etki alanında bir kullanıcı adı ile giriş yapmaya çalışıyor olabilirsiniz?

### <a name="user-realm-discovery-failed"></a>Kullanıcı Diyarı Bulma Başarısız Oldu
Ağ veya proxy yapılandırma sorunları. Ağa ulaşılamıyor. [Yükleme sihirbazındaki Sorun Giderme bağlantısı sorunlarına](#troubleshoot-connectivity-issues-in-the-installation-wizard)bakın.

### <a name="user-password-expired"></a>Kullanıcı Şifresi Nin Süresi Doldu
Kimlik bilgilerinizin süresi doldu. Parolanızı değiştirin.

### <a name="authorization-failure"></a>Yetkilendirme Hatası
Kullanıcıya Azure AD'de eylem gerçekleştirme yetkisi veremedi.

### <a name="authentication-canceled"></a>Kimlik Doğrulama İptal Edildi
Çok faktörlü kimlik doğrulama (MFA) mücadelesi iptal edildi.

<div id="connect-msolservice-failed">
<!--
  Empty div just to act as an alias for the "Connect To MS Online Failed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="connect-to-ms-online-failed"></a>MS Çevrimiçi'ne Bağlan Başarısız Oldu
Kimlik doğrulama başarılı oldu, ancak Azure AD PowerShell'in kimlik doğrulama sorunu vardır.

<div id="get-msoluserrole-failed">
<!--
  Empty div just to act as an alias for the "Azure AD Global Admin Role Needed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="azure-ad-global-admin-role-needed"></a>Azure AD Global Yönetici Rolü Gerekli
Kullanıcı başarıyla doğrulandı. Ancak kullanıcıya global yönetici rolü atanmamış. Bu şekilde kullanıcıya [genel yönetici rolü atayabilirsiniz.](../users-groups-roles/directory-assign-admin-roles.md) 

<div id="privileged-identity-management">
<!--
  Empty div just to act as an alias for the "Privileged Identity Management Enabled" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="privileged-identity-management-enabled"></a>Ayrıcalıklı Kimlik Yönetimi Etkin
Kimlik doğrulama başarılı oldu. Ayrıcalıklı kimlik yönetimi etkinleştirildi ve şu anda genel bir yönetici değilsiniz. Daha fazla bilgi için [bkz.](../privileged-identity-management/pim-getting-started.md)

<div id="get-msolcompanyinformation-failed">
<!--
  Empty div just to act as an alias for the "Company Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="company-information-unavailable"></a>Şirket Bilgileri Kullanılamıyor
Kimlik doğrulama başarılı oldu. Azure AD'den şirket bilgileri alınamadı.

<div id="get-msoldomain-failed">
<!--
  Empty div just to act as an alias for the "Domain Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="domain-information-unavailable"></a>Etki Alanı Bilgileri Kullanılamıyor
Kimlik doğrulama başarılı oldu. Etki alanı bilgilerini Azure AD'den alamadım.

### <a name="unspecified-authentication-failure"></a>Belirtilmemiş Kimlik Doğrulama Hatası
Yükleme sihirbazında Beklenmeyen hata olarak gösterilmiştir. **Okul veya kuruluş hesabı**yerine Microsoft **Hesabı** kullanmaya çalışırsanız ortaya çıkabilir.

## <a name="troubleshooting-steps-for-previous-releases"></a>Önceki sürümler için sorun giderme adımları.
1.1.105.0 ile başlayan sürümlerle (Şubat 2016'da yayımlanan) oturum açma asistanı emekliye ayrıldı. Bu bölüm ve yapılandırma artık gerekli olmamalıdır, ancak başvuru olarak tutulur.

Tek oturumdaki asistan çalışması için winhttp'nin yapılandırılması gerekir. Bu yapılandırma [**netsh**](how-to-connect-install-prerequisites.md#connectivity)ile yapılabilir.  
![Netsh](./media/tshoot-connect-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Oturum açma asistanı doğru şekilde yapılandırılmamıştır
Oturum açma asistanı proxy'ye ulaşamıyorsa veya proxy isteğe izin vermiyorsa bu hata görüntülenir.
![nonetsh](./media/tshoot-connect-connectivity/nonetsh.png)

* Bu hatayı görürseniz, [netsh](how-to-connect-install-prerequisites.md#connectivity) proxy yapılandırmabakmak ve doğru olduğunu doğrulamak.
  ![netshshow](./media/tshoot-connect-connectivity/netshshow.png)
* Bu doğruysa, sorunun sihirbazın dışında da bulunuyu pkıştırıp var olmadığını görmek için [proxy bağlantısını doğrula'daki](#verify-proxy-connectivity) adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
