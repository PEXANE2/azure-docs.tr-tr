---
title: 'Azure AD Connect: doğrudan kimlik doğrulama sorunlarını giderme | Microsoft Docs'
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulamanın nasıl giderileceği açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması, Active Directory yüklemesi, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler hakkında sorun giderme
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
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "60456173"
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Azure Active Directory Geçişli Kimlik Doğrulaması Sorunlarını Giderme

Bu makale, Azure AD geçişli kimlik doğrulamasıyla ilgili yaygın sorunlar hakkında sorun giderme bilgilerini bulmanıza yardımcı olur.

>[!IMPORTANT]
>Doğrudan kimlik doğrulamasıyla Kullanıcı oturum açma sorunlarınız varsa, geri dönebilmeniz için özelliği devre dışı bırakıp doğrudan kimlik doğrulama aracılarını, yalnızca bulutta yer alan bir genel yönetici hesabına sahip olmadan kaldırın. [Yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımın yapılması kritik bir öneme sahiptir ve kiracınızdan kilitlenmemesini sağlar.

## <a name="general-issues"></a>Genel sorunlar

### <a name="check-status-of-the-feature-and-authentication-agents"></a>Özelliğin ve kimlik doğrulama aracılarının durumunu denetleme

Doğrudan kimlik doğrulama özelliğinin kiracınızda hala **etkinleştirildiğinden** ve kimlik doğrulama aracılarının durumunun **etkin** **ve etkin olmadığından emin olun.** [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com/) **Azure AD Connect** dikey penceresine giderek durumu kontrol edebilirsiniz.

![Azure Active Directory Yönetim Merkezi-Azure AD Connect dikey pencere](./media/tshoot-connect-pass-through-authentication/pta7.png)

![Azure Active Directory Yönetim Merkezi-geçişli kimlik doğrulama dikey penceresi](./media/tshoot-connect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Kullanıcıya yönelik oturum açma hata iletileri

Kullanıcı geçişli kimlik doğrulaması kullanarak oturum açamıyor ise Azure AD oturum açma ekranında aşağıdaki kullanıcıya yönelik hatalardan birini görebilirler: 

|Hata|Açıklama|Çözüm
| --- | --- | ---
|AADSTS80001|Active Directory ile bağlantı kurulamıyor|Aracı sunucularının, parolalarının doğrulanması gereken kullanıcılarla aynı AD ormanının üyesi olduğundan ve Active Directory bağlanabildiklerinden emin olun.  
|AADSTS8002|Active Directory bağlantısında bir zaman aşımı oluştu|Active Directory kullanılabilir olduğundan ve aracılardan gelen isteklere yanıt verdiğinden emin olun.
|AADSTS80004|Aracıya geçirilen Kullanıcı adı geçerli değil|Kullanıcının doğru kullanıcı adıyla oturum açmaya çalıştığından emin olun.
|AADSTS80005|Doğrulama öngörülemeyen WebException ile karşılaştı|Geçici bir hata. İsteği yeniden deneyin. Başarısız olmaya devam ederse, Microsoft destek 'e başvurun.
|AADSTS80007|Active Directory ile iletişim kurulurken bir hata oluştu|Daha fazla bilgi için aracı günlüklerine bakın ve Active Directory beklendiği gibi çalıştığını doğrulayın.

### <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center-needs-premium-license"></a>Azure Active Directory Yönetim merkezinde oturum açma hatası nedenleri (Premium lisans gerekir)

Kiracınızda ilişkili bir Azure AD Premium lisansı varsa, [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com/) [oturum açma etkinlik raporuna](../reports-monitoring/concept-sign-ins.md) da bakabilirsiniz.

![Azure Active Directory Yönetim Merkezi-oturum açma işlemleri raporu](./media/tshoot-connect-pass-through-authentication/pta4.png)

[Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com/) 'nde **Azure Active Directory** -> **oturum açma** bölümüne gidin ve belirli bir kullanıcının oturum açma etkinliğine tıklayın. **Oturum açma hata kodu** alanını bulun. Aşağıdaki tabloyu kullanarak bu alanın değerini bir hata nedeni ve çözümüyle eşleyin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 50144 | Kullanıcının Active Directory parolasının süresi doldu. | Şirket içi Active Directory kullanıcının parolasını sıfırlayın.
| 80001 | Kullanılabilir Kimlik Doğrulama Aracısı yok. | Bir kimlik doğrulama Aracısı yükleyip kaydedin.
| 80002 | Kimlik Doğrulama Aracısı'nın parola doğrulama isteği zaman aşımına uğradı. | Active Directory kimlik doğrulama aracısından erişilebilir olup olmadığını denetleyin.
| 80003 | Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. | Sorun birden çok Kullanıcı genelinde sürekli olarak tekrarlanabilir ise Active Directory yapılandırmanızı denetleyin.
| 80004 | Oturum açma isteğinde yanlış Kullanıcı Asıl Adı (UPN) kullanıldı. | Kullanıcıdan doğru kullanıcı adıyla oturum açmasını isteyin.
| 80005 | Kimlik Doğrulama Aracısı: Hata oluştu. | Geçici hata. Daha sonra tekrar deneyin.
| 80007 | Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. | Active Directory kimlik doğrulama aracısından erişilebilir olup olmadığını denetleyin.
| 80010 | Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. | Sorun sürekli olarak tekrarlanabilir ise, yeni bir kimlik doğrulama Aracısı yükleyip kaydedin. Ve geçerli olanı kaldırın. 
| 80011 | Kimlik Doğrulama Aracısı şifre çözme anahtarını alamıyor. | Sorun sürekli olarak tekrarlanabilir ise, yeni bir kimlik doğrulama Aracısı yükleyip kaydedin. Ve geçerli olanı kaldırın.

>[!IMPORTANT]
>Doğrudan kimlik doğrulama aracıları, [Win32 LogonUser API](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx)'sini çağırarak kullanıcı adlarını ve parolalarını Active Directory karşı DOĞRULAYARAK Azure AD kullanıcılarının kimliğini doğrular. Sonuç olarak, iş istasyonu oturum erişimini sınırlandırmak için Active Directory ' de "oturum aç" ayarını ayarladıysanız, doğrudan kimlik doğrulama aracılarını barındıran sunucuları da "oturum açma" sunucuları listesine eklemeniz gerekir. Bunun başarısız olması, kullanıcılarınızın Azure AD 'de oturum açmasını engeller.

## <a name="authentication-agent-installation-issues"></a>Kimlik doğrulama Aracısı yükleme sorunları

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

Sunucudan [Aracı günlüklerini toplayın](#collecting-pass-through-authentication-agent-logs) ve sorununuzla Microsoft desteği başvurun.

## <a name="authentication-agent-registration-issues"></a>Kimlik doğrulama Aracısı kayıt sorunları

### <a name="registration-of-the-authentication-agent-failed-due-to-blocked-ports"></a>Engellenen bağlantı noktaları nedeniyle kimlik doğrulama aracısının kaydı başarısız oldu

Kimlik doğrulama aracısının yüklü olduğu sunucunun hizmet URL 'lerimiz ve [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)listelenen bağlantı noktalarıyla iletişim kurabildiğinden emin olun.

### <a name="registration-of-the-authentication-agent-failed-due-to-token-or-account-authorization-errors"></a>Kimlik doğrulama aracısının kaydı, belirteç veya hesap yetkilendirme hataları nedeniyle başarısız oldu

Tüm Azure AD Connect veya tek başına kimlik doğrulama Aracısı yüklemesi ve kayıt işlemleri için yalnızca bulutta yer alan bir genel yönetici hesabı kullandığınızdan emin olun. MFA özellikli genel yönetici hesaplarında bilinen bir sorun var; geçici çözüm olarak MFA 'yı geçici olarak devre dışı bırakın (yalnızca işlemleri gerçekleştirmek için).

### <a name="an-unexpected-error-occurred"></a>Beklenmeyen bir hata oluştu

Sunucudan [Aracı günlüklerini toplayın](#collecting-pass-through-authentication-agent-logs) ve sorununuzla Microsoft desteği başvurun.

## <a name="authentication-agent-uninstallation-issues"></a>Kimlik doğrulama Aracısı kaldırma sorunları

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Azure AD Connect kaldırılırken uyarı iletisi

Kiracınızda geçiş kimlik doğrulaması etkinse ve Azure AD Connect kaldırmayı denerseniz, şu uyarı iletisini gösterir: "diğer sunucularda başka bir geçiş kimlik doğrulama Aracısı yüklü değilse, kullanıcılar Azure AD 'de oturum açamaz."

Kullanıcı oturumu kesmemek için Azure AD Connect kaldırmadan önce, kurulum uygulamanızın [yüksek oranda kullanılabilir](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability) olduğundan emin olun.

## <a name="issues-with-enabling-the-feature"></a>Özelliği etkinleştirme ile ilgili sorunlar

### <a name="enabling-the-feature-failed-because-there-were-no-authentication-agents-available"></a>Kullanılabilir kimlik doğrulama Aracısı olmadığından özelliğin etkinleştirilmesi başarısız oldu

Kiracınızda doğrudan kimlik doğrulamayı etkinleştirmek için en az bir etkin kimlik doğrulama aracısına sahip olmanız gerekir. Azure AD Connect veya bağımsız bir kimlik doğrulama Aracısı yükleyerek bir kimlik doğrulama aracısı yükleyebilirsiniz.

### <a name="enabling-the-feature-failed-due-to-blocked-ports"></a>Engellenen bağlantı noktaları nedeniyle özelliğin etkinleştirilmesi başarısız oldu

Azure AD Connect yüklü olduğu sunucunun hizmet URL 'lerimiz ve [burada](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)listelenen bağlantı noktalarıyla iletişim kurabildiğinden emin olun.

### <a name="enabling-the-feature-failed-due-to-token-or-account-authorization-errors"></a>Belirteç veya hesap yetkilendirme hataları nedeniyle özelliğin etkinleştirilmesi başarısız oldu

Özelliği etkinleştirirken yalnızca bulutta yer alan bir genel yönetici hesabı kullandığınızdan emin olun. Multi-Factor Authentication (MFA) özellikli genel yönetici hesaplarında bilinen bir sorun vardır; geçici çözüm olarak MFA 'yı geçici olarak devre dışı bırakın (yalnızca işlemi gerçekleştirmek için).

## <a name="collecting-pass-through-authentication-agent-logs"></a>Doğrudan kimlik doğrulama Aracısı günlüklerini toplama

Sahip olduğunuz sorunun türüne bağlı olarak, geçişli kimlik doğrulama Aracısı günlükleri için farklı yerlere bakmanız gerekir.

### <a name="azure-ad-connect-logs"></a>Azure AD Connect günlükleri

Yüklemeyle ilgili hatalar için **%ProgramData%\aadconnect\trace-\*. log**konumundaki Azure AD Connect günlüklerine bakın.

### <a name="authentication-agent-event-logs"></a>Kimlik doğrulama Aracısı olay günlükleri

Kimlik doğrulama aracısıyla ilgili hatalar için, sunucuda Olay Görüntüleyicisi uygulamasını açın ve **uygulama ve hizmet Logs\microsoft\azureadconnect\authenticationt\0\ yönetici**' nin altına bakın.

Ayrıntılı analiz için, "oturum" günlüğünü etkinleştirin (Bu seçeneği bulmak için Olay Görüntüleyicisi uygulamasının içine sağ tıklayın). Kimlik Doğrulama aracısını normal işlemler sırasında bu günlük etkin olarak çalıştırmayın; yalnızca sorun giderme için kullanın. Günlük içeriği yalnızca günlük yeniden devre dışı bırakıldıktan sonra görünür.

### <a name="detailed-trace-logs"></a>Ayrıntılı izleme günlükleri

Kullanıcı oturum açma hatalarıyla ilgili sorunları gidermek için **%ProgramData%\microsoft\azure AD Connect Authentication\\**me \ izleme konumundaki izleme günlüklerini arayın. Bu Günlükler, belirli bir Kullanıcı oturum açma özelliğinin doğrudan kimlik doğrulama özelliğini kullanarak başarısız olma nedenlerini içerir. Bu hatalar, yukarıdaki oturum açma hatası nedenleri tablosunda gösterilen oturum açma hatası nedenlerinden de eşleştirilir. Aşağıda örnek bir günlük girişi verilmiştir:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Komut istemi 'ni açıp aşağıdaki komutu çalıştırarak (Yukarıdaki örnekte ' 1328 ') hatanın açıklayıcı ayrıntılarını alabilirsiniz (Note: ' 1328 ' ifadesini günlüklerinizi gördüğünüz gerçek hata numarasıyla değiştirin):

`Net helpmsg 1328`

![Doğrudan Kimlik Doğrulama](./media/tshoot-connect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri

Denetim günlüğü etkinse, etki alanı denetleyicilerinizin güvenlik günlüklerinde ek bilgiler bulunabilir. Doğrudan kimlik doğrulama aracıları tarafından gönderilen oturum açma isteklerini sorgulamak için basit bir yol aşağıdaki gibidir:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

## <a name="performance-monitor-counters"></a>Performans Izleyicisi sayaçları

Kimlik doğrulama aracılarını izlemenin bir diğer yolu da, kimlik doğrulama aracısının yüklendiği her bir sunucuda belirli performans Izleyicisi sayaçlarını izlemedir. Aşağıdaki genel sayaçları (**# PTA kimlik doğrulamaları**, **#PTA başarısız kimlik** doğrulamaları ve **#PTA başarılı kimlik doğrulamaları**) ve hata sayaçlarını (**# PTA kimlik doğrulama hataları**) kullanın:

![Geçişli kimlik doğrulaması performans Izleyicisi sayaçları](./media/tshoot-connect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Geçişli kimlik doğrulaması, yük dengelemesi _değil_ birden fazla kimlik doğrulama Aracısı kullanarak yüksek kullanılabilirlik sağlar. Yapılandırmanıza bağlı olarak, tüm kimlik doğrulama aracılarınız kabaca _eşit_ sayıda _istek almaz._ Belirli bir kimlik doğrulama aracısının hiçbir trafik alabilmesi mümkündür.
