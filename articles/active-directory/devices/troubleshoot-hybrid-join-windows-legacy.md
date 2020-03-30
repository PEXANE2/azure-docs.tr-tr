---
title: Eski karma Azure Active Directory'nin birleştiği aygıtları sorun giderme
description: Karma Azure Etkin Dizini'nde sorun giderme, alt düzey aygıtları birleştirdi.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74379114"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Karma Azure Etkin Dizini'nde sorun giderme, alt düzey aygıtları birleştirdi 

Bu makale yalnızca aşağıdaki aygıtlar için geçerlidir: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 

Windows 10 veya Windows Server 2016 için, [Windows 10 ve Windows Server 2016 aygıtlarına katılan Sorun Giderme karma Azure Active Directory'ye](troubleshoot-hybrid-join-windows-current.md)bakın.

Bu makalede, aşağıdaki senaryoları desteklemek için [karma Azure Etkin Dizin birleştirilmiş aygıtlar yapılandırdığınız](hybrid-azuread-join-plan.md) varsayar:

- Aygıt Tabanlı Koşullu Erişim

Bu makalede, olası sorunları çözmek için nasıl sorun giderme kılavuzu sağlar.  

**Bilmeniz gerekenler:** 

- Alt düzey Windows aygıtları için karma Azure AD birleştirme, Windows 10'dakinden biraz daha farklı çalışır. Birçok müşteri, AD FS (federated etki alanları için) veya Sorunsuz SSO yapılandırılmış (yönetilen etki alanları için) gerektiğini fark etmez.
- Federe etki alanlarını olan müşteriler için, Hizmet Bağlantı Noktası (SCP) yönetilen etki alanı adına işaret edecek şekilde yapılandırıldıysa (örneğin, contoso.com yerine contoso.onmicrosoft.com), alt düzey Windows aygıtları için Karma Azure AD Join işe yaramaz.
- Şu anda kullanıcı başına en fazla aygıt sayısı, alt düzey karma Azure AD birleştirilmiş aygıtlar için de geçerlidir. 
- Aynı fiziksel aygıt, birden çok etki alanı kullanıcısı alt düzey karma Azure AD aygıtlarına katıldığında Azure AD'de birden çok kez görünür.  Örneğin, *jdoe* ve *jharnett* bir cihazda oturum açma, **kullanıcı** bilgi sekmesinde her biri için ayrı bir kayıt (DeviceID) oluşturulur. 
- Ayrıca, işletim sisteminin yeniden yüklenmesi veya el ile yeniden kaydedilmesi nedeniyle kullanıcı bilgileri sekmesinde bir aygıt için birden çok giriş alabilirsiniz.
- Cihazların ilk kaydı / birleştirilmesi, oturum açma veya kilitleme / kilidini açma girişiminde bulunmak üzere yapılandırılır. Görev zamanlayıcısı görevi tarafından tetiklenen 5 dakikalık bir gecikme olabilir. 
- Windows 7 SP1 veya Windows Server 2008 R2 SP1 durumunda [KB4284842'nin](https://support.microsoft.com/help/4284842) yüklü olduğundan emin olun. Bu güncelleştirme, müşterinin parolayı değiştirdikten sonra korumalı anahtarlara erişimini kaybetmesinden kaynaklanan gelecekteki kimlik doğrulama hatalarını önler.

## <a name="step-1-retrieve-the-registration-status"></a>Adım 1: Kayıt durumunu alma 

**Kayıt durumunu doğrulamak için:**  

1. Karma Azure AD join gerçekleştiren kullanıcı hesabıyla oturum açın.
1. Komut istemini açma 
1. `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i` yazın

Bu komut, birleştirme durumu hakkında ayrıntılı bilgi sağlayan bir iletişim kutusu görüntüler.

![Windows için İşyeri Birleştirme](./media/troubleshoot-hybrid-join-windows-legacy/01.png)

## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Adım 2: Karma Azure AD birleştirme durumunu değerlendirin 

Aygıt karma Azure AD'ye katılmadıysa, "Katıl" düğmesini tıklayarak karma Azure AD join yapmayı deneyebilirsiniz. Karma Azure AD birleştirme girişimi başarısız olursa, hatayla ilgili ayrıntılar gösterilir.

**En sık karşılaşılan sorunlar şunlardır:**

- Yanlış yapılandırılmış BIR AD FS veya Azure AD veya Ağ sorunları

    ![Windows için İşyeri Birleştirme](./media/troubleshoot-hybrid-join-windows-legacy/02.png)
    
   - Autoworkplace.exe, Azure AD veya AD FS ile sessizce kimlik doğrulaması yapamaz. Bunun nedeni, eksik veya yanlış yapılandırılmış AD FS (federal etki alanları için) veya eksik veya yanlış yapılandırılmış Azure AD Sorunsuz Tek Oturum Açma (yönetilen etki alanları için) veya ağ sorunları olabilir. 
   - Çok faktörlü kimlik doğrulama (MFA) etkin leştirilmiş /kullanıcı için yapılandırılan ve WIAORMULTIAUTHN AD FS sunucusunda yapılandırılmamış olabilir. 
   - Başka bir olasılık, ana bölge bulma (HRD) sayfası sessizce bir belirteç isteyen **autoworkplace.exe** engeller kullanıcı etkileşimi için bekliyor olmasıdır.
   - AD FS ve Azure AD URL'leri IE'nin istemcideki intranet bölgesinde eksik olabilir.
   - Ağ bağlantısı **sorunları, autoworkplace.exe'nin** AD FS'ye veya Azure REKLAM URL'lerine ulaşmasını engelliyor olabilir. 
   - **Autoworkplace.exe** istemcinin istemciden kuruluşun şirket içi AD etki alanı denetleyicisine doğrudan görüş hattı olmasını gerektirir, bu da karma Azure AD join'in yalnızca istemci kuruluşun intranetine bağlı olduğunda başarılı olduğu anlamına gelir.
   - Kuruluşunuz Azure REKLAM Sorunsuz Tek `https://autologon.microsoftazuread-sso.com` Oturum `https://aadg.windows.net.nsatc.net` Açma kullanır veya aygıtın IE intranet ayarlarında bulunmaz ve Komut **dosyası üzerinden durum çubuğuna güncelleştirmelere izin** ver, Intranet bölgesi için etkinleştirilmez.
- Etki alanı kullanıcısı olarak oturum açmış değilseniz

   ![Windows için İşyeri Birleştirme](./media/troubleshoot-hybrid-join-windows-legacy/03.png)

   Bunun birkaç farklı nedeni vardır:

   - Oturum açmış kullanıcı bir etki alanı kullanıcısı değildir (örneğin, yerel bir kullanıcı). Alt düzey cihazlarda karma Azure AD join yalnızca etki alanı kullanıcıları için desteklenir.
   - İstemci bir etki alanı denetleyicisine bağlanamaz.    
- Kotaya ulaşıldı

    ![Windows için İşyeri Birleştirme](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Hizmet yanıt vermiyor 

    ![Windows için İşyeri Birleştirme](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Ayrıca altındaki olay günlüğünde durum bilgilerini de bulabilirsiniz: **Uygulamalar ve Hizmetler Günlüğü\Microsoft-İşyeri Join**
  
**Başarısız bir karma Azure AD birleştirmesinin en yaygın nedenleri şunlardır:** 

- Bilgisayarınız kuruluşunuzun dahili ağına veya şirket içi AD etki alanı denetleyicinize bağlantısı olan bir VPN'e bağlı değildir.
- Yerel bir bilgisayar hesabıyla bilgisayarınıza giriş yapmışsınız. 
- Hizmet yapılandırma sorunları: 
   - AD FS sunucusu **WIAORMULTIAUTHN'u**destekleyecek şekilde yapılandırılmamıştır. 
   - Bilgisayarınızın ormanında Azure AD'de doğrulanmış etki alanı adınızı gösteren Hizmet Bağlantı Noktası nesnesi yok 
   - Veya etki alanınız yönetiliyorsa, Seamless SSO yapılandırılmamış veya çalışmamıştır.
   - Bir kullanıcı aygıt sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız için [cihaz yönetimi SSS'ye](faq.md) bakın  
