---
title: Azure AD 'de Azure AD Connect bulut sağlama önkoşulları
description: Bu konuda, önkoşulların ve donanım gereksinimlerinin bulut sağlaması açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 155edf72a60e079a609853e953e3cf66024cc83c
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74794256"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama önkoşulları
Bu konu, kimlik çözümünüz olarak Azure AD Connect bulut sağlamasını seçme ve kullanma hakkında rehberlik sağlar.



## <a name="cloud-provisioning-agent-requirements"></a>Bulut sağlama Aracısı gereksinimleri
Bulut sağlamasını Azure AD Connect kullanmak için aşağıdakiler gerekir:
    
- Azure AD kiracınız için genel yönetici hesabı
- Windows 2012 R2 veya üzeri ile sağlama aracısına yönelik bir şirket içi sunucu
- Şirket içi güvenlik duvarı konfigürasyonları

Belgenin geri kalanı, bu Önkoşullar için adım adım yönergeler sağlar.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim merkezinde

1. Azure AD kiracınızda yalnızca bulutta yer alan bir genel yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olması veya kullanılamaz hale gelmesi için kiracınızın yapılandırmasını yönetebilirsiniz. [Yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımın tamamlanması, kiracınızdan kilitlenmemesini sağlamak açısından önemlidir.
2. Azure AD kiracınıza bir veya daha fazla [özel etki alanı adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu etki alanı adlarından biriyle oturum açabilir.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. En az 4 GB RAM ve .NET 4.7.1 + Runtime ile Windows Server 2012 R2 veya üzeri çalıştıran etki alanına katılmış bir ana bilgisayar sunucusu kimliği 

2. Sunucularınız ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Aracıların Azure AD 'ye aşağıdaki bağlantı noktaları üzerinden *giden* istekler yapabilmeleri için emin olun:

     | Bağlantı noktası numarası | Nasıl kullanılır? |
     | --- | --- |
     | **80** | SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL 'Ler) indirir |
     | **443** | Hizmetle tüm giden iletişimi işler |
     | **8080** (isteğe bağlı) | Aracılar, 8080 bağlantı noktası kullanılamıyorsa, her on dakikada bir 443, bağlantı noktası üzerinden durumlarını raporlar. Bu durum Azure AD portalında görüntülenir. 8080 numaralı bağlantı noktası Kullanıcı oturum açma işlemleri _için kullanılmaz._ |
     
     Güvenlik duvarınız, kaynak kullanıcılara göre kuralları zorlarsa, ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın.
   - Güvenlik duvarınız veya ara sunucunuz güvenli sonekler belirtmenize izin veriyorsa **\*. msappproxy.net** ve **\*. ServiceBus.Windows.net**öğesine bağlantı ekleyin. Aksi takdirde, haftalık olarak güncellenen [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verin.
   - Aracılarınızın ilk kayıt için **login.Windows.net** ve **login.microsoftonline.com** 'e erişmesi gerekir. Bu URL 'Ler için güvenlik duvarınızı da açın.
   - Sertifika doğrulaması için şu URL 'Leri engellemeyi kaldırın: **mscrl.Microsoft.com:80**, **CRL.Microsoft.com:80**, **OCSP.msocsp.com:80**ve **www\.Microsoft.com:80**. Bu URL 'Ler diğer Microsoft ürünleriyle sertifika doğrulaması için kullanıldığından, bu URL 'Lerin engeli kaldırılmış olabilir.

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama
Azure 'un 443 numaralı bağlantı noktasını dinlediğini ve aracınızın onunla iletişim kurabildiğini doğrulamak için aşağıdakileri kullanabilirsiniz:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 numaralı bağlantı noktası üzerinden Azure ile iletişim kurabildiğini doğrular.  Bir tarayıcı açın ve aracının yüklendiği sunucudan yukarıdaki URL 'ye gidin.
![Hizmetler](media/how-to-install/verify2.png)

### <a name="additional-requirements"></a>Ek gereksinimler
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS gereksinimleri

>[!NOTE]
>Aktarım Katmanı Güvenliği (TLS), güvenli iletişimler için sağlayan bir protokoldür.  TLS ayarlarının değiştirilmesi ormanın tamamını etkiler.  Daha fazla bilgi için bkz. [Windows 'Da WinHTTP 'de varsayılan güvenli protokoller olarak tls 1,1 ve tls 1,2 etkinleştirme güncelleştirmesi](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi)

Azure AD Connect bulut sağlama aracısının barındırması gereken Windows Server, yüklemeden önce TLS 1,2 ' i etkinleştirmiş olmalıdır.

TLS 1,2 ' i etkinleştirmek için:

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

