---
title: Eski karma Azure Active Directory katılmış cihazlarda sorun giderme
description: Karma Azure Active Directory katılmış alt düzey cihazlarda sorun giderme.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: e168deea1ba442d48f483264c1e97ce618040f18
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74379114"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Karma Azure Active Directory katılmış alt düzey cihazlarda sorun giderme 

Bu makale yalnızca aşağıdaki cihazlarda geçerlidir: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 

Windows 10 veya Windows Server 2016 için bkz. [karma Azure Active Directory katılmış Windows 10 ve Windows server 2016 cihazlarda sorun giderme](troubleshoot-hybrid-join-windows-current.md).

Bu makalede, [karma Azure Active Directory katılmış cihazları](hybrid-azuread-join-plan.md) aşağıdaki senaryoları destekleyecek şekilde yapılandırdığınız varsayılır:

- Cihaz tabanlı koşullu erişim

Bu makale, olası sorunların nasıl çözüleceği hakkında sorun giderme kılavuzu sağlar.  

**Bilmeniz gerekenler:** 

- Alt düzey Windows cihazları için karma Azure AD katılımı, Windows 10 ' da olduğundan biraz farklı çalışır. Birçok müşteri, AD FS (federe etki alanları için) veya sorunsuz SSO (yönetilen etki alanları için) ihtiyacı olduğunu fark etmez.
- Federasyon etki alanları olan müşteriler için, hizmet bağlantı noktası (SCP), yönetilen etki alanı adını (örneğin, contoso.onmicrosoft.com) işaret edecek şekilde yapılandırıldıysa, alt düzey Windows cihazlarına yönelik karma Azure AD katılımı çalışmaz.
- Şu anda Kullanıcı başına en fazla cihaz sayısı alt düzey karma Azure AD 'ye katılmış cihazlar için de geçerlidir. 
- Birden çok etki alanı kullanıcısı, alt düzey karma Azure AD 'ye katılmış cihazlarda oturum açtığında aynı fiziksel cihaz Azure AD 'de birden çok kez görünür.  Örneğin, *jtikan* ve *jharnett* bir cihazda oturum açtığında, her biri için **Kullanıcı** bilgileri sekmesinde ayrı bir kayıt (DeviceID) oluşturulur. 
- Ayrıca, işletim sisteminin yeniden yüklenmesi veya el ile yeniden kayıt olması nedeniyle Kullanıcı bilgileri sekmesinde bir cihaz için birden çok giriş alabilirsiniz.
- Cihazların ilk kaydı/katılması, oturum açma ya da kilit/kilit açma girişimi için bir girişim gerçekleştirecek şekilde yapılandırılmıştır. Bir görev zamanlayıcı görevi tarafından tetiklenen 5 dakikalık bir gecikme olabilir. 
- Windows 7 SP1 veya Windows Server 2008 R2 SP1 söz konusu olduğunda [KB4284842](https://support.microsoft.com/help/4284842) yüklendiğinden emin olun. Bu güncelleştirme, parola değiştirildikten sonra müşterinin korunan anahtarlara erişim kaybı nedeniyle gelecekteki kimlik doğrulama başarısızlıklarını önler.

## <a name="step-1-retrieve-the-registration-status"></a>1. Adım: kayıt durumunu alma 

**Kayıt durumunu doğrulamak için:**  

1. Karma Azure AD katılımı gerçekleştiren kullanıcı hesabıyla oturum açın.
1. Komut istemi 'ni açın 
1. `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i` yazın

Bu komut, size JOIN durumu hakkında ayrıntılar sağlayan bir iletişim kutusu görüntüler.

![Windows için Workplace Join](./media/troubleshoot-hybrid-join-windows-legacy/01.png)

## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>2. Adım: karma Azure AD JOIN durumunu değerlendirme 

Cihaz karma Azure AD 'ye katılmış değilse, "katıl" düğmesine tıklayarak karma Azure AD katılımı yapmayı deneyebilirsiniz. Karma Azure AD JOIN işlemi başarısız olursa, hata hakkındaki ayrıntılar gösterilir.

**En yaygın sorunlar şunlardır:**

- Yanlış yapılandırılmış AD FS veya Azure AD veya ağ sorunları

    ![Windows için Workplace Join](./media/troubleshoot-hybrid-join-windows-legacy/02.png)
    
   - Autoworkplace.exe, Azure AD veya AD FS ile sessizce kimlik doğrulaması yapamıyor. Bunun nedeni, eksik veya yanlış yapılandırılmış AD FS (federe etki alanları için) veya eksik ya da yanlış yapılandırılmış Azure AD sorunsuz çoklu oturum açma (yönetilen etki alanları için) veya ağ sorunları olabilir. 
   - Bu, Multi-Factor Authentication (MFA) özelliğinin etkinleştirilmesi/yapılandırılması ve WIAORMULTIAUTHN 'nin AD FS sunucuda yapılandırılmamış olması olabilir. 
   - Diğer bir deyişle, giriş bölgesi bulma (HRD) sayfasının kullanıcı etkileşimini beklediği, bu da **autoworkplace.exe** sessizce bir belirteç istemekte engel olur.
   - AD FS ve Azure AD URL 'Lerinin istemci üzerindeki intranet bölgesinde eksik olması olabilir.
   - Ağ bağlantısı sorunları, **autoworkplace.exe** AD FS veya Azure AD URL 'lerine ulaşmasını engelleyebilir. 
   - **Autoworkplace.exe** istemcinin, istemciden kuruluşun ŞIRKET içi ad etki alanı denetleyicisine doğrudan bir görüş satırı olmasını gerektirir. Bu, karma Azure AD 'ye yalnızca istemci kuruluşun intranetine bağlıyken başarılı olduğu anlamına gelir.
   - Kuruluşunuz Azure AD sorunsuz çoklu oturum açma 'yı kullanır `https://autologon.microsoftazuread-sso.com` veya `https://aadg.windows.net.nsatc.net` cihazın IE intranet ayarlarında mevcut değildir ve intranet bölgesi için **komut dosyası aracılığıyla durum çubuğundaki güncelleştirmelerin** etkin olmadığından izin vermez.
- Bir etki alanı kullanıcısı olarak oturum açmadınız

   ![Windows için Workplace Join](./media/troubleshoot-hybrid-join-windows-legacy/03.png)

   Bunun gerçekleşebileceği birkaç farklı neden vardır:

   - Oturum açan kullanıcı bir etki alanı kullanıcısı değil (örneğin, yerel bir Kullanıcı). Alt düzey cihazlarda karma Azure AD katılımı yalnızca etki alanı kullanıcıları için desteklenir.
   - İstemci bir etki alanı denetleyicisine bağlanamıyor.    
- Kotaya ulaşıldı

    ![Windows için Workplace Join](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Hizmet yanıt vermiyor 

    ![Windows için Workplace Join](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Durum bilgilerini olay günlüğü 'nde de bulabilirsiniz: **uygulamalar ve hizmetler Log\Microsoft-Workplace JOIN**
  
**Başarısız bir karma Azure AD birleştirmesi için en yaygın nedenler şunlardır:** 

- Bilgisayarınız kuruluşunuzun iç ağına veya şirket içi AD etki alanı denetleyicinize bağlantısı olan bir VPN 'ye bağlı değil.
- Bilgisayarınızda yerel bir bilgisayar hesabıyla oturum açtınız. 
- Hizmet yapılandırma sorunları: 
   - AD FS sunucusu **Wiaormultiauthn**'yi destekleyecek şekilde yapılandırılmamış. 
   - Bilgisayarınızın ormanında, Azure AD 'de doğrulanmış etki alanı adınızı işaret eden bir hizmet bağlantı noktası nesnesi yok 
   - Ya da etki alanınız yönetiliyorsa, sorunsuz SSO Yapılandırılmadı veya çalışmıyor.
   - Bir Kullanıcı cihaz sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Sorular için bkz. [cihaz YÖNETIMI SSS](faq.md)  
