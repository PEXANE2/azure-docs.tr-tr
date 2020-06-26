---
title: Azure AD 'de Azure AD Connect bulut sağlama önkoşulları
description: Bu makalede, bulut sağlaması için ihtiyaç duyduğunuz ön koşullar ve donanım gereksinimleri açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/06/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7eb632405ef17ef4100503f30168c1207179f48
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85373871"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama önkoşulları
Bu makalede, kimlik çözümünüz olarak Azure Active Directory (Azure AD) bulut sağlamasını bağlama ve kullanma hakkında rehberlik sunulmaktadır.



## <a name="cloud-provisioning-agent-requirements"></a>Bulut sağlama Aracısı gereksinimleri
Bulut sağlamasını Azure AD Connect kullanmak için aşağıdakiler gerekir:
    
- Azure AD kiracınız için konuk kullanıcı olmayan bir karma kimlik yöneticisi hesabı.
- Windows 2012 R2 veya üzeri ile sağlama aracısına yönelik bir şirket içi sunucu.  Bu sunucu, [Active Directory Yönetim Katmanı modelini](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)temel alan bir katman 0 sunucusu olmalıdır.
- Şirket içi güvenlik duvarı konfigürasyonları.

>[!NOTE]
>Sağlama Aracısı Şu anda yalnızca Ingilizce dil sunucularına yüklenebilir. İngilizce olmayan bir sunucuya Ingilizce dil paketi yüklemek geçerli bir geçici çözüm değildir ve aracının yüklenememesi ile sonuçlanır. 

Belgenin geri kalanı, bu Önkoşullar için adım adım yönergeler sağlar.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim merkezinde

1. Azure AD kiracınızda yalnızca bulutta yer alan bir karma kimlik yöneticisi hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olursa veya kullanılamaz hale gelirse kiracınızın yapılandırmasını yönetebilirsiniz. [Yalnızca bulut karma kimlik yöneticisi hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımın tamamlanması, kiracınızdan kilitlenmemesini sağlamak açısından önemlidir.
1. Azure AD kiracınıza bir veya daha fazla [özel etki alanı adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu etki alanı adlarından biriyle oturum açabilir.

### <a name="in-your-directory-in-active-directory"></a>Active Directory dizininizde

Dizin özniteliklerini eşitlemeye hazırlamak için [ıddüzeltmesini aracını](https://docs.microsoft.com/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) çalıştırın.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. En az 4 GB RAM ve .NET 4.7.1 + çalışma zamanı ile Windows Server 2012 R2 veya üstünü çalıştıran etki alanına katılmış bir konak sunucusu belirler.

1. Yerel sunucudaki PowerShell yürütme ilkesinin tanımsız veya RemoteSigned olarak ayarlanması gerekir.

1. Sunucularınız ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Aracıların Azure AD 'ye aşağıdaki bağlantı noktaları üzerinden *giden* istekler yapabilmeleri için emin olun:

        | Bağlantı noktası numarası | Nasıl kullanılır? |
        | --- | --- |
        | **80** | TLS/SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL 'Ler) indirir.  |
        | **443** | Hizmetle tüm giden iletişimi işler. |
        | **8080** (isteğe bağlı) | Aracılar 443, 8080 numaralı bağlantı noktası kullanılamıyorsa, her 10 dakikada bir bu durumu bağlantı noktası üzerinden raporlar. Bu durum Azure AD portalında görüntülenir. |
     
   - Güvenlik duvarınız, kaynak kullanıcılara göre kuralları zorlarsa, ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın.
   - Güvenlik duvarınız veya ara sunucunuz güvenli sonekler belirtmenize izin veriyorsa, \* . msappproxy.net ve. ServiceBus.Windows.net öğesine bağlantı ekleyin \* . Aksi takdirde, haftalık olarak güncellenen [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verin.
   - Aracılarınızın ilk kayıt için login.windows.net ve login.microsoftonline.com 'e erişmesi gerekir. Bu URL 'Ler için güvenlik duvarınızı da açın.
   - Sertifika doğrulaması için şu URL 'Leri engellemeyi kaldırın: mscrl.microsoft.com:80, crl.microsoft.com:80, ocsp.msocsp.com:80 ve www \. Microsoft.com:80. Bu URL 'Ler diğer Microsoft ürünleriyle sertifika doğrulaması için kullanılır, bu nedenle bu URL 'Lerin engeli kaldırılmış olabilir.

>[!NOTE]
> Bulut sağlama aracısının Windows Server Core 'a yüklenmesi desteklenmez.


### <a name="additional-requirements"></a>Ek gereksinimler
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS gereksinimleri

>[!NOTE]
>Aktarım Katmanı Güvenliği (TLS), güvenli iletişimler için sağlayan bir protokoldür. TLS ayarlarının değiştirilmesi ormanın tamamını etkiler. Daha fazla bilgi için bkz. [Windows 'Da WinHTTP 'de varsayılan güvenli protokoller olarak tls 1,1 ve tls 1,2 ' i etkinleştirmek Için güncelleştirme](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi).

Azure AD Connect Cloud sağlama Aracısı 'nı barındıran Windows Server, yüklemeden önce TLS 1,2 ' i etkinleştirmiş olmalıdır.

TLS 1,2 ' i etkinleştirmek için aşağıdaki adımları izleyin.

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Sunucuyu yeniden başlatın.


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

