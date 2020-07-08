---
title: 'Azure AD Connect: Azure AD bağlantı sorunlarını giderme | Microsoft Docs'
description: Azure AD Connect ile ilgili bağlantı sorunlarını nasıl giderebileceğinizi açıklar.
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
ms.topic: troubleshooting
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: a329ec32e241d88a56fc7031904777888ac194ae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85356415"
---
# <a name="troubleshoot-azure-ad-connectivity"></a>Azure AD bağlantısı sorunlarını giderme
Bu makalede, Azure AD Connect ile Azure AD arasındaki bağlantının nasıl çalıştığı ve bağlantı sorunlarını nasıl giderebileceğiniz açıklanır. Bu sorunlar büyük olasılıkla ara sunucu içeren bir ortamda görülebilir.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Yükleme sihirbazında bağlantı sorunlarını giderme
Azure AD Connect, kimlik doğrulaması için modern kimlik doğrulaması (ADAL kitaplığı kullanılarak) kullanıyor. Yükleme Sihirbazı ve eşitleme altyapısının uygun olması, bu iki .NET uygulaması olduğundan machine.config düzgün şekilde yapılandırılmasını gerektirir.

Bu makalede, Fabrikam 'ın proxy 'si aracılığıyla Azure AD 'ye nasıl bağlandığını göstereceğiz. Proxy sunucusu fabrikamproxy olarak adlandırılır ve 8080 numaralı bağlantı noktasını kullanıyor.

İlk olarak [**machine.config**](how-to-connect-install-prerequisites.md#connectivity) doğru yapılandırıldığından emin olmak istiyoruz.
![MachineConfig](./media/tshoot-connect-connectivity/machineconfig.png)

> [!NOTE]
> Bazı Microsoft dışı bloglarda, bunun yerine miiserver.exe.config yapılması gerektiğine ilişkin değişiklikler belgelenmiştir. Ancak, bu dosyanın her yükseltme sırasında üzerine yazılır, böylece ilk yüklemede çalışıyor olsa bile sistem ilk yükseltme sırasında çalışmayı durduruyor. Bu nedenle, bunun yerine machine.config güncelleştirme önerisi.
>
>

Proxy sunucusunda gerekli URL 'Lerin açılması da gerekir. Resmi liste, [Office 365 URL 'lerinde ve IP adresi aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)belgelenmiştir.

Bu URL 'Lerde, her bir Azure AD 'ye bağlanabilmek için aşağıdaki tablo, tam olarak en düşük üyeliktir. Bu liste, parola geri yazma veya Azure AD Connect Health gibi isteğe bağlı özellikler içermez. İlk yapılandırma için sorun gidermeye yardımcı olmak üzere burada belgelenmiştir.

| URL | Bağlantı noktası | Açıklama |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. verisign.com |HTTP/80 |CRL listelerini indirmek için kullanılır. |
| \*. entrust.net |HTTP/80 |MFA için CRL listelerini indirmek için kullanılır. |
| \*.windows.net |HTTPS/443 |Azure AD 'de oturum açmak için kullanılır. |
| secure.aadcdn.microsoftonline-p.com |HTTPS/443 |MFA için kullanılır. |
| \*.microsoftonline.com |HTTPS/443 |Azure AD dizininizi yapılandırmak ve verileri içeri/dışarı aktarmak için kullanılır. |

## <a name="errors-in-the-wizard"></a>Sihirbazdaki hatalar
Yükleme Sihirbazı iki farklı güvenlik bağlamı kullanıyor. **Azure AD 'ye bağlanma**sayfasında, şu anda oturum açmış olan kullanıcıyı kullanıyor. **Yapılandırma**sayfasında, [eşitleme altyapısı için hizmeti çalıştıran hesapla](reference-connect-accounts-permissions.md#adsync-service-account)değişiklik yapılır. Bir sorun varsa, proxy yapılandırması Global olduğundan, bu durum büyük olasılıkla sihirbazda **Azure AD 'ye Bağlan** sayfasında bulunur.

Aşağıdaki sorunlar, Yükleme sihirbazında karşılaştığınız en yaygın hatalardır.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Yükleme Sihirbazı doğru yapılandırılmadı
Sihirbazın proxy 'sine ulaşamadığınızda bu hata görüntülenir.
![nomachineconfig](./media/tshoot-connect-connectivity/nomachineconfig.png)

* Bu hatayı görürseniz [machine.config](how-to-connect-install-prerequisites.md#connectivity) doğru şekilde yapılandırıldığını doğrulayın.
* Bu doğru görünüyorsa, sorunun sihirbazın dışında mevcut olup olmadığını görmek için [Proxy bağlantısını doğrulama](#verify-proxy-connectivity) bölümündeki adımları izleyin.

### <a name="a-microsoft-account-is-used"></a>Microsoft hesabı kullanılır
**Okul veya kuruluş** hesabı yerine bir **Microsoft hesabı** kullanıyorsanız, genel bir hata görürsünüz.
![Bir Microsoft hesabı kullanılır](./media/tshoot-connect-connectivity/unknownerror.png)

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA uç noktasına ulaşılamıyor
Bu hata, uç noktaya **https://secure.aadcdn.microsoftonline-p.com** ulaşılamadığından ve genel YÖNETICINIZIN MFA 'nın etkin olması halinde görünür.
![nomachineconfig](./media/tshoot-connect-connectivity/nomicrosoftonlinep.png)

* Bu hatayı görürseniz, uç nokta **Secure.aadcdn.microsoftonline-p.com** 'nin ara sunucuya eklendiğini doğrulayın.

### <a name="the-password-cannot-be-verified"></a>Parola doğrulanamıyor
Yükleme Sihirbazı Azure AD 'ye bağlanmada başarılı olursa, ancak parolanın kendisi doğrulanamazsa şu hatayı görürsünüz: ![ Hatalı parola.](./media/tshoot-connect-connectivity/badpassword.png)

* Parolanın geçici bir parola olması ve değiştirilmesi gerekiyor mu? Gerçekten doğru parola mi? ' Da `https://login.microsoftonline.com` (Azure AD Connect sunucusundan başka bir bilgisayarda) oturum açmayı deneyin ve hesabın kullanılabilir olduğunu doğrulayın.

### <a name="verify-proxy-connectivity"></a>Proxy bağlantısını doğrulama
Azure AD Connect sunucusunun proxy ve Internet ile gerçek bağlantı olup olmadığını doğrulamak için, proxy 'nin Web isteklerine izin vermeyi öğrenmek için bazı PowerShell kullanın. Bir PowerShell isteminde çalıştırın `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc` . (Teknik olarak ilk çağrı, `https://login.microsoftonline.com` ve bu URI de çalışır, ancak DIĞER URI yanıt vermek daha hızlıdır.)

PowerShell, proxy ile iletişim kurmak için machine.config ' deki yapılandırmayı kullanır. WinHTTP/Netsh 'deki ayarlar bu cmdlet 'leri etkilememelidir.

Proxy doğru yapılandırılmışsa, bir başarı durumu almalısınız: ![ proxy200](./media/tshoot-connect-connectivity/invokewebrequest200.png)

**Uzak sunucuya bağlanamıyorsanız**, PowerShell proxy kullanılmadan doğrudan çağrı yapmayı DENIYOR veya DNS doğru şekilde yapılandırılmamış. **machine.config** dosyasının doğru yapılandırıldığından emin olun.
![bağlanılamıyor](./media/tshoot-connect-connectivity/invokewebrequestunable.png)

Proxy doğru yapılandırılmamışsa bir hata alırsınız: ![ proxy200 ](./media/tshoot-connect-connectivity/invokewebrequest403.png)
 ![ proxy407](./media/tshoot-connect-connectivity/invokewebrequest407.png)

| Hata | Hata metni | Yorum |
| --- | --- | --- |
| 403 |Yasak |Proxy, istenen URL için açılmadı. Proxy yapılandırmasını yeniden ziyaret edin ve [URL 'lerin](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) açık olduğundan emin olun. |
| 407 |Proxy kimlik doğrulaması gerekli |Ara sunucu bir oturum açma işlemi gerektirdi ve hiçbiri sağlanmadı. Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, bu ayarın machine.config yapılandırıldığından emin olun. Ayrıca, Sihirbazı çalıştıran kullanıcı için ve hizmet hesabı için etki alanı hesapları kullandığınızdan emin olun. |

### <a name="proxy-idle-timeout-setting"></a>Proxy boşta zaman aşımı ayarı
Azure AD Connect, Azure AD 'ye bir dışarı aktarma isteği gönderdiğinde, yanıt oluşturmadan önce Azure AD 'nin isteği işlemesi 5 dakikaya kadar sürebilir. Bu, özellikle de aynı dışa aktarma isteğine büyük grup üyeliğine sahip bir dizi Grup nesnesi varsa gerçekleşebilir. Ara sunucu boşta kalma süresinin 5 dakikadan fazla olacak şekilde yapılandırıldığından emin olun. Aksi takdirde, Azure AD ile aralıklı bağlantı sorunu Azure AD Connect sunucuda gözlemlenebilir.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Azure AD Connect ile Azure AD arasındaki iletişim kalıbı
Önceki tüm adımları izlediyseniz ve bağlanamıyorsa hala bağlanamıyorsanız, bu noktada ağ günlüklerine bakıyor olabilirsiniz. Bu bölüm, normal ve başarılı bir bağlantı deseninin belgelenmesinde. Ayrıca, ağ günlüklerini okurken yok sayılacak, sık kullanılan kırmızı herhalkaları da listelüyor.

* İçin çağrılar vardır `https://dc.services.visualstudio.com` . Yüklemenin başarılı olması için bu URL 'nin ara sunucuda açılması gerekmez ve bu çağrılar yoksayılabilir.
* DNS çözümünden, DNS ad alanı nsatc.net ve microsoftonline.com altında olmayan diğer ad alanlarında olacak gerçek Konakları listeleyen görüntülenir. Bununla birlikte, gerçek sunucu adlarında herhangi bir Web hizmeti isteği yoktur ve bu URL 'Leri ara sunucuya eklemeniz gerekmez.
* Adminwebservice ve provisioningapı uç noktaları bulma uç noktalardır ve kullanılacak gerçek uç noktayı bulmak için kullanılır. Bu uç noktalar, bölgenize bağlı olarak farklılık açmış.

### <a name="reference-proxy-logs"></a>Ara sunucu günlüklerine başvur
İşte gerçek bir ara sunucu günlüğünden ve Yükleme Sihirbazı sayfasından alındığı yerden bir döküm alındı (aynı uç noktaya yinelenen girdiler kaldırılmıştır). Bu bölüm, kendi proxy 'niz ve ağ günlüklerinizin bir başvurusu olarak kullanılabilir. Gerçek uç noktalar ortamınızda farklı olabilir (Bu URL 'Lerde *italik*olarak).

**Azure AD'ye Bağlanma**

| Saat | URL |
| --- | --- |
| 1/11/2016 8:31 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:31 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://*bba800-bağlayıcısını*. microsoftonline.com:443 |
| 1/11/2016 8:32 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |connect://*bwsc02-Relay*. microsoftonline.com:443 |

**Yapılandır**

| Saat | URL |
| --- | --- |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://*bba800-bağlayıcısını*. microsoftonline.com:443 |
| 1/11/2016 8:43 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba900-bağlayıcısını*. microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://*bba800-bağlayıcısını*. microsoftonline.com:443 |
| 1/11/2016 8:44 |connect://login.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |connect://*bwsc02-Relay*. microsoftonline.com:443 |

**İlk eşitleme**

| Saat | URL |
| --- | --- |
| 1/11/2016 8:48 |connect://login.windows.net:443 |
| 1/11/2016 8:49 |connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba900-bağlayıcısını*. microsoftonline.com:443 |
| 1/11/2016 8:49 |connect://*bba800-bağlayıcısını*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları
Bu bölümde, ADAL (Azure AD Connect tarafından kullanılan kimlik doğrulama kitaplığı) ve PowerShell aracılığıyla döndürülebilecek hatalar ele alınmaktadır. Açıklanan hata, sonraki adımlarınızı anlamanıza yardımcı olmalıdır.

### <a name="invalid-grant"></a>Geçersiz Izin
Geçersiz Kullanıcı adı veya parola. Daha fazla bilgi için, bkz. [parola doğrulanamıyor](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Bilinmeyen Kullanıcı türü
Azure AD dizininiz bulunamıyor veya çözümlenemiyor. Doğrulanmamış bir etki alanında Kullanıcı adıyla oturum açmaya çalışıyor musunuz?

### <a name="user-realm-discovery-failed"></a>Kullanıcı bölgesi bulma başarısız oldu
Ağ veya ara sunucu yapılandırma sorunları. Ağa ulaşılamıyor. Bkz. [Yükleme sihirbazındaki bağlantı sorunlarını giderme](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Kullanıcı parolasının geçerliliği geçildi
Kimlik bilgilerinizin geçerliliği bitti. Parolanızı değiştirin.

### <a name="authorization-failure"></a>Yetkilendirme hatası
Azure AD 'de Kullanıcı için eylem gerçekleştirme yetkisi verilemedi.

### <a name="authentication-canceled"></a>Kimlik doğrulama Iptal edildi
Multi-Factor Authentication (MFA) sınaması iptal edildi.

<div id="connect-msolservice-failed">
<!--
  Empty div just to act as an alias for the "Connect To MS Online Failed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="connect-to-ms-online-failed"></a>MS online 'A bağlanma başarısız oldu
Kimlik doğrulaması başarılı oldu, ancak Azure AD PowerShell 'de bir kimlik doğrulama sorunu oluştu.

<div id="get-msoluserrole-failed">
<!--
  Empty div just to act as an alias for the "Azure AD Global Admin Role Needed" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="azure-ad-global-admin-role-needed"></a>Azure AD Genel yönetici rolü gerekli
Kullanıcının kimliği başarıyla doğrulandı. Ancak kullanıcıya genel yönetici rolü atanmaz. Kullanıcıya [genel yönetici rolü atayabilirsiniz](../users-groups-roles/directory-assign-admin-roles.md) .

<div id="privileged-identity-management">
<!--
  Empty div just to act as an alias for the "Privileged Identity Management Enabled" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="privileged-identity-management-enabled"></a>Privileged Identity Management etkin
Kimlik doğrulama başarılı oldu. Ayrıcalıklı kimlik yönetimi etkinleştirilmiştir ve şu anda genel yönetici değilsiniz. Daha fazla bilgi için bkz. [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md).

<div id="get-msolcompanyinformation-failed">
<!--
  Empty div just to act as an alias for the "Company Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="company-information-unavailable"></a>Şirket bilgileri kullanılamıyor
Kimlik doğrulama başarılı oldu. Şirket bilgileri Azure AD 'den alınamadı.

<div id="get-msoldomain-failed">
<!--
  Empty div just to act as an alias for the "Domain Information Unavailable" header
  because we used the mentioned id in the code to jump to this section.
-->
</div>

### <a name="domain-information-unavailable"></a>Etki alanı bilgileri kullanılamıyor
Kimlik doğrulama başarılı oldu. Azure AD 'den etki alanı bilgileri alınamadı.

### <a name="unspecified-authentication-failure"></a>Belirtilmeyen kimlik doğrulama hatası
Yükleme sihirbazında beklenmeyen bir hata olarak gösteriliyor. **Okul veya kuruluş hesabı**yerine bir **Microsoft hesabı** kullanmayı denerseniz, bu durum oluşabilir.

## <a name="troubleshooting-steps-for-previous-releases"></a>Önceki sürümler için sorun giderme adımları.
Yayın numarası 1.1.105.0 (2016 ' de yayımlanmıştır) ile başlayan yayınlar sayesinde, oturum açma Yardımcısı kullanımdan kaldırıldı. Bu bölüm ve yapılandırma artık gerekli değildir, ancak başvuru olarak tutulur.

Çoklu oturum açma Yardımcısı 'nın çalışması için, WinHTTP 'nin yapılandırılması gerekir. Bu yapılandırma, [**netsh**](how-to-connect-install-prerequisites.md#connectivity)ile yapılabilir.
![komutlarına](./media/tshoot-connect-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Oturum açma Yardımcısı doğru yapılandırılmadı
Bu hata, oturum açma Yardımcısı ara sunucuya ulaşamadığınızda veya proxy isteğe izin verilmediğinde görüntülenir.
![etme dışı](./media/tshoot-connect-connectivity/nonetsh.png)

* Bu hatayı görürseniz, [netsh](how-to-connect-install-prerequisites.md#connectivity) 'deki proxy yapılandırmasına bakın ve doğru olduğunu doğrulayın.
  ![netshshow](./media/tshoot-connect-connectivity/netshshow.png)
* Bu doğru görünüyorsa, sorunun sihirbazın dışında mevcut olup olmadığını görmek için [Proxy bağlantısını doğrulama](#verify-proxy-connectivity) bölümündeki adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
