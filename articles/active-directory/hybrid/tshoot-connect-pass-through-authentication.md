---
title: 'Azure AD Connect: Geçiş Kimlik Doğrulamayı Sorun Giderme | Microsoft Dokümanlar'
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması'nda nasıl sorun giderilen açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication sorun giderme, Active Directory'yi, Azure AD, SSO, Tek Oturum Açma için gerekli bileşenleri yükleme
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae83cea866367fa6a6596caa683d0287bea96c29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60456173"
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Azure Active Directory Geçişli Kimlik Doğrulaması Sorunlarını Giderme

Bu makale, Azure AD Geçiş Kimlik Doğrulaması ile ilgili sık karşılaşılan sorunlarla ilgili sorun giderme bilgilerini bulmanıza yardımcı olur.

>[!IMPORTANT]
>Geçiş Kimlik Doğrulama ile kullanıcı oturum açma sorunlarıyla karşı karşıyaysanız, yalnızca buluta yönelik bir Global Administrator hesabı olmadan özelliği devre dışı bırakmayın veya Geçiş Kimlik Doğrulama Aracıları'nı kaldırmayın. Yalnızca [buluta özel Global Administrator hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımı yapmak çok önemlidir ve kiracınızın dışında kalmamanızı sağlar.

## <a name="general-issues"></a>Genel sorunlar

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Özelliğin durumunu ve Kimlik Doğrulama Aracılarını denetleme

Geçiş Kimlik Doğrulama özelliğinin kiracınızda hala **Etkin** olduğundan ve Kimlik Doğrulama Aracılarının durumunun **Etkin**değil, **Etkin**olmadığından emin olun. Azure Active Directory yönetici merkezindeki **Azure AD Connect** bıtırıntına giderek durumu kontrol [edebilirsiniz.](https://aad.portal.azure.com/)

![Azure Active Directory yönetici merkezi - Azure AD Connect blade](./media/tshoot-connect-pass-through-authentication/pta7.png)

![Azure Active Directory yönetici merkezi - Geçiş Kimlik Doğrulama bıçağı](./media/tshoot-connect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Kullanıcıya bakan oturum açma hatası iletileri

Kullanıcı Geçiş Kimlik Doğrulaması'nı kullanarak oturum açamıyorsa, Azure AD oturum açma ekranında aşağıdaki kullanıcıya dönük hatalardan birini görebilir: 

|Hata|Açıklama|Çözüm
| --- | --- | ---
|AADSTS80001|Etkin Dizine bağlanılamıyor|Aracı sunucularının, parolalarının doğrulanması gereken ve Active Directory'ye bağlanabilen kullanıcılarla aynı AD ormanına üye olduğundan emin olun.  
|AADSTS8002|Etkin Dizin'e bağlanan bir zaman ası oluştu|Etkin Dizin'in kullanılabilir olduğundan ve aracılardan gelen isteklere yanıt verdiğinden emin olun.
|AADSTS80004|Aracıya geçen kullanıcı adı geçerli değildi|Kullanıcının doğru kullanıcı adı ile oturum açmaya çalıştığından emin olun.
|AADSTS80005|Doğrulama öngörülemeyen WebException karşılaştı|Geçici bir hata. İsteği yeniden deneyin. Başarısız olmaya devam ederse, Microsoft desteğine başvurun.
|AADSTS80007|Active Directory ile iletişim kurma hatası oluştu|Daha fazla bilgi için aracı günlüklerini denetleyin ve Active Directory'nin beklendiği gibi çalıştığını doğrulayın.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Azure Active Directory yönetici merkezinde oturum açma hatası nedenleri (Premium lisansa ihtiyaç duyar)

Kiracınızda bununla ilişkili bir Azure AD Premium lisansı varsa, [Azure Etkin Dizin yönetici merkezindeki](https://aad.portal.azure.com/)oturum açma faaliyet [raporuna](../reports-monitoring/concept-sign-ins.md) da bakabilirsiniz.

![Azure Active Directory yönetici merkezi - Oturum açma raporu](./media/tshoot-connect-pass-through-authentication/pta4.png)

Azure **Active Directory** -> yönetici merkezinde Azure Active [Directory](https://aad.portal.azure.com/) **Oturum** Açma'ya gidin ve belirli bir kullanıcının oturum açma etkinliğini tıklatın. **OTURUM AÇMA HATA KODU** alanına bakın. Aşağıdaki tabloyu kullanarak bu alanın değerini bir hata nedeni ve çözümle eşleştirin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 50144 | Kullanıcının Active Directory parolasının süresi doldu. | Şirket içi Active Directory'nizde kullanıcının parolasını sıfırla.
| 80001 | Kullanılabilir Kimlik Doğrulama Aracısı yok. | Kimlik Doğrulama Aracısı yükleyin ve kaydedin.
| 80002 | Kimlik Doğrulama Aracısı'nın parola doğrulama isteği zaman aşımına uğradı. | Etkin Dizininize Kimlik Doğrulama Aracısı'ndan erişilip erişilemeyip erişilemeyini kontrol edin.
| 80003 | Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. | Sorun birden çok kullanıcı arasında sürekli olarak tekrarlanabilirse, Active Directory yapılandırmanızı kontrol edin.
| 80004 | Oturum açma isteğinde yanlış Kullanıcı Asıl Adı (UPN) kullanıldı. | Kullanıcıdan doğru kullanıcı adı ile oturum açmasını isteyin.
| 80005 | Kimlik Doğrulama Aracısı: Hata oluştu. | Geçici hata. Daha sonra tekrar deneyin.
| 80007 | Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. | Etkin Dizininize Kimlik Doğrulama Aracısı'ndan erişilip erişilemeyip erişilemeyini kontrol edin.
| 80010 | Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. | Sorun sürekli olarak tekrarlanabilirse, yeni bir Kimlik Doğrulama Aracısı yükleyin ve kaydedin. Ve mevcut olanı kaldırın. 
| 80011 | Kimlik Doğrulama Aracısı şifre çözme anahtarını alamıyor. | Sorun sürekli olarak tekrarlanabilirse, yeni bir Kimlik Doğrulama Aracısı yükleyin ve kaydedin. Ve mevcut olanı kaldırın.

>[!IMPORTANT]
>Geçiş Kimlik Doğrulama Aracıları, Kullanıcı adlarını ve parolalarını Active Directory'ye karşı [Win32 LogonUser API'sini](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)arayarak doğrulayarak Azure AD kullanıcılarının kimliğini doğrular. Sonuç olarak, Iş istasyonu oturum açma erişimini sınırlamak için Active Directory'deki "Logon To" ayarını ayarladıysanız, Geçiş Kimlik Doğrulama Aracılarını barındıran sunucuları "Oturum Açma" sunucuları listesine de eklemeniz gerekir. Bunu yapmamak, kullanıcılarınızın Azure AD'de oturum açmalarını engeller.

## <a name="authentication-agent-installation-issues"></a>Kimlik Doğrulama Aracısı yükleme sorunları

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Sunucudan aracı günlüklerini toplayın](#collecting-pass-through-authentication-agent-logs) ve sorununuzun microsoft desteğine başvurun.

## <a name="authentication-agent-registration-issues"></a>Kimlik Doğrulama Aracısı kayıt sorunları

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Kimlik Doğrulama Aracısının kaydı engellenen bağlantı noktaları nedeniyle başarısız oldu

Kimlik Doğrulama Aracısı'nın yüklü olduğu sunucunun [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)listelenen hizmet URL'lerimiz ve bağlantı noktalarımızla iletişim kurabileceğinden emin olun.

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Kimlik Doğrulama Aracısının kaydı belirteç veya hesap yetkilendirme hataları nedeniyle başarısız oldu

Tüm Azure AD Connect veya bağımsız Kimlik Doğrulama Aracısı yükleme ve kayıt işlemleri için yalnızca buluta özel bir Global Administrator hesabı kullandığınızdan emin olun. MFA etkin Global Administrator hesaplarında bilinen bir sorun vardır; Geçici olarak MFA'yı geçici olarak kapatın (yalnızca işlemleri tamamlamak için).

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

[Sunucudan aracı günlüklerini toplayın](#collecting-pass-through-authentication-agent-logs) ve sorununuzun microsoft desteğine başvurun.

## <a name="authentication-agent-uninstallation-issues"></a>Kimlik Doğrulama Aracısı yükleme sorunlarını kaldırma

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Azure AD Connect'i yüklerken uyarı iletisi

Kiracınızda Geçiş Kimlik Doğrulaması etkinleştirilmişse ve Azure AD Connect'i kaldırmaya çalışıyorsanız, bu size şu uyarı iletisini gösterir: "Kullanıcılar, başka Geçiş Kimlik Doğrulama aracınız yoksa Azure AD'de oturum açamayacaktır diğer sunucular."

Kullanıcı oturum açmayı önlemek için Azure AD Connect'i kaldırmadan önce kurulumuuzun yüksek oranda [kullanılabilir](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability) olduğundan emin olun.

## <a name="issues-with-enabling-the-feature"></a>Özelliği etkinleştirme yle ilgili sorunlar

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Kullanılabilir Kimlik Doğrulama Aracısı olmadığından özelliği etkinleştirme

Kiracınızda Geçiş Kimlik Doğrulaması'nı etkinleştirmek için en az bir etkin Kimlik Doğrulama Aracısı'na sahip olmanız gerekir. Azure AD Connect'i veya bağımsız bir Kimlik Doğrulama Aracısı'nı yükleyerek bir Kimlik Doğrulama Aracısı yükleyebilirsiniz.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Engellenen bağlantı noktaları nedeniyle özelliği etkinleştirme

Azure AD Connect'in yüklü olduğu sunucunun [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)listelenen hizmet URL'lerimiz ve bağlantı noktalarımızla iletişim kurabileceğinden emin olun.

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Belirteç veya hesap yetkilendirme hataları nedeniyle başarısız olan özelliği etkinleştirme

Özelliği etkinleştirirken yalnızca buluta özel Global Administrator hesabı kullandığınızdan emin olun. Çok faktörlü kimlik doğrulama (MFA) etkin leştirilmiş Global Administrator hesapları ile bilinen bir sorun vardır; Geçici olarak MFA'yı geçici olarak kapatın (yalnızca işlemi tamamlamak için).

## <a name="collecting-pass-through-authentication-agent-logs"></a>Geçiş Kimlik Doğrulama Aracısı günlüklerini toplama

Sahip olabileceğiniz sorunun türüne bağlı olarak, Geçiş Kimlik Doğrulama Aracısı günlükleri için farklı yerlerde bakmanız gerekir.

### <a name="azure-ad-connect-logs"></a>Azure AD Connect günlükleri

Yüklemeyle ilgili hatalar için Azure AD Connect günlüklerini **%ProgramData%\AADConnect\trace-\*.log adresinden kontrol edin.**

### <a name="authentication-agent-event-logs"></a>Kimlik Doğrulama Aracısı olay günlükleri

Kimlik Doğrulama Aracısı ile ilgili hatalar için, sunucuda Olay Görüntüleyici uygulamasını açın ve **Uygulama ve Hizmet Günlükleri\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**altında kontrol edin.

Ayrıntılı analizler için "Oturum" günlüğünü etkinleştirin (bu seçeneği bulmak için Olay Görüntüleyicisi uygulamasının içine sağ tıklayın). Normal işlemler sırasında bu günlük etkinken Kimlik Doğrulama Aracısını çalıştırmayın; yalnızca sorun giderme için kullanın. Günlük içeriği yalnızca günlük yeniden devre dışı bırakıldıktan sonra görünür.

### <a name="detailed-trace-logs"></a>Ayrıntılı izleme günlükleri

Kullanıcı oturum açma hatalarını gidermek için **%ProgramData%\Microsoft\Azure AD Connect Authentication Agent\Trace\\**adresinde izleme günlüklerini arayın. Bu günlükler, geçiş kimlik doğrulama özelliğini kullanarak belirli bir kullanıcı oturum açmanın başarısız olmasının nedenlerini içerir. Bu hatalar, önceki oturum açma hatası nedenleri tablosunda gösterilen oturum açma hatası nedenleriyle de eşlenir. Aşağıda örnek bir günlük girişi verilmiştir:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Komut istemini açarak ve aşağıdaki komutu çalıştırarak hatanın açıklayıcı ayrıntılarını ('1328' önceki örnekte) alabilirsiniz (Not: Günlüklerinizde gördüğünüz gerçek hata numarasıyla '1328'i değiştirin):

`Net helpmsg 1328`

![Doğrudan Kimlik Doğrulama](./media/tshoot-connect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Etki Alanı Denetleyicigünlükleri

Denetim günlüğü etkinse, Etki Alanı Denetleyicilerinizin güvenlik günlüklerinde ek bilgiler bulunabilir. Geçiş Kimlik Doğrulama Aracıları tarafından gönderilen oturum açma isteklerini sorgulamanın basit bir yolu aşağıdaki gibidir:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Performans Monitörü sayaçları

Kimlik Doğrulama Aracılarını izlemenin başka bir yolu da, Kimlik Doğrulama Aracısı'nın yüklü olduğu her sunucudaki belirli Performans İzleyici sayaçlarını izlemektir. Aşağıdaki Genel sayaçları **(# PTA kimlik doğrulamaları,** **başarısız kimlik doğrulamaları #PTA** ve başarısız kimlik **doğrulamaları #PTA)** ve Hata sayaçlarını **(# PTA kimlik doğrulama hataları)** kullanın:

![Geçiş Kimlik Doğrulama Performans Monitörü sayaçları](./media/tshoot-connect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Geçiş Kimlik Doğrulaması, yük _dengelemesi değil,_ birden çok Kimlik Doğrulama Aracısı kullanarak yüksek kullanılabilirlik sağlar. Yapılandırmanıza bağlı _olarak,_ tüm Kimlik Doğrulama Aracılarınız kabaca _eşit_ sayıda istek almaz. Belirli bir Kimlik Doğrulama Aracısı hiç trafik alıyor olabilir.
