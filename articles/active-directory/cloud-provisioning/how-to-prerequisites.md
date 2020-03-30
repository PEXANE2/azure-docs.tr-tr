---
title: Azure AD'de Azure AD Connect bulut sağlama için ön koşullar
description: Bu makalede, bulut sağlama için gereken ön koşullar ve donanım gereksinimleri açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45648170f69d513b15e79cdd76f56e66bbc88bfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332079"
---
# <a name="prerequisites-for-azure-ad-connect-cloud-provisioning"></a>Azure AD Connect bulut sağlama için ön koşullar
Bu makalede, kimlik çözümünüz olarak Azure Active Directory (Azure AD) Connect bulut sağlama seçeneğini ve bunları nasıl kullanacağınıza ilişkin kılavuzlar sağlanmaktadır.



## <a name="cloud-provisioning-agent-requirements"></a>Bulut sağlama aracıgereksinimleri
Azure AD Connect bulut sağlama yı kullanmak için aşağıdakilere ihtiyacınız var:
    
- Azure AD kiracınız için konuk kullanıcı olmayan genel bir yönetici hesabı.
- Windows 2012 R2 veya daha sonra olan sağlama aracısı için şirket içi sunucu.
- Şirket içi güvenlik duvarı yapılandırmaları.

>[!NOTE]
>Sağlama aracısı şu anda yalnızca İngilizce dil sunucularında yüklenebilir. İngilizce olmayan bir sunucuya İngilizce dil paketi yüklemek geçerli bir geçici çözüm değildir ve aracının yüklenememesine neden olur. 

Belgenin geri kalanı bu ön koşullar için adım adım yönergeler sağlar.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Etkin Dizin yönetici merkezinde

1. Azure AD kiracınızda yalnızca buluta özel bir global yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olursa veya kullanılamıyorsa kiracınızın yapılandırmasını yönetebilirsiniz. Yalnızca [buluta özel genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımı bitirmek, kiracınızın dışında kaldığınızdan emin olmak için çok önemlidir.
1. Azure AD kiracınıza bir veya daha fazla [özel alan adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu alan adlarından biriyle oturum açabilir.

### <a name="in-your-directory-in-active-directory"></a>Active Directory'deki dizininizde

Dizin özniteliklerini eşitleme için hazırlamak için [IdFix aracını](https://docs.microsoft.com/office365/enterprise/prepare-directory-attributes-for-synch-with-idfix) çalıştırın.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. En az 4 GB RAM ve .NET 4.7.1+ çalışma süresiyle Windows Server 2012 R2 veya daha büyük çalışan etki alanı bir leştirilmiş ana bilgisayar sunucusu tanımlayın.

1. Sunucularınız ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Aracıların Aşağıdaki bağlantı noktaları üzerinden Azure AD'ye *giden* isteklerde bulunabilmesini sağlayın:

        | Bağlantı noktası numarası | Nasıl kullanılır? |
        | --- | --- |
        | **80** | TLS/SSL sertifikasını doğrularken sertifika iptal listelerini (CRLs) karşıdan yükler.  |
        | **443** | Hizmetle ilgili tüm giden iletişimi işler. |
        | **8080** (isteğe bağlı) | Aracılar, bağlantı noktası 443 kullanılamıyorsa, 8080 portu üzerinden her 10 dakikada bir durumlarını bildirir. Bu durum Azure AD portalında görüntülenir. |
     
   - Güvenlik duvarınız kuralları oluşturan kullanıcılara göre zorluyorsa, bu bağlantı noktalarını ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafiğe açın.
   - Güvenlik duvarınız veya proxy'niz güvenli sonekler belirtmenize izin \*veriyorsa, .msappproxy.net ve \*.servicebus.windows.net bağlantılarını ekleyin. Değilse, haftalık olarak güncelleştirilen [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişin.
   - Aracılarınızın ilk kayıt için login.windows.net ve login.microsoftonline.com erişimi ne kadar gerekiyorsa. Bu URL'ler için de güvenlik duvarınızı açın.
   - Sertifika doğrulama için aşağıdaki URL'lerin engelini kaldırın: mscrl.microsoft.com:80,\.crl.microsoft.com:80, ocsp.msocsp.com:80 ve www microsoft.com:80. Bu URL'ler diğer Microsoft ürünleriyle sertifika doğrulama için kullanıldığından, bu URL'lerin engelini zaten kaldırmış olabilirsiniz.

### <a name="verify-the-port"></a>Bağlantı noktasını doğrulama
Azure'un 443 no'lu bağlantı noktasından dinlediğini ve aracınızın bu bağlantı noktasıyla iletişim kurabileceğini doğrulamak için aşağıdaki URL'yi kullanın:

https://aadap-portcheck.connectorporttest.msappproxy.net/ 

Bu test, aracılarınızın 443 portu üzerinden Azure ile iletişim kurabileceğini doğrular. Bir tarayıcı açın ve aracının yüklü olduğu sunucudan önceki URL'ye gidin.

![Port ulaşılabilirliğinin doğrulanması](media/how-to-install/verify2.png)

### <a name="additional-requirements"></a>Ek gereksinimler
- [Microsoft .NET Framework 4.7.1](https://www.microsoft.com/download/details.aspx?id=56116) 

#### <a name="tls-requirements"></a>TLS gereksinimleri

>[!NOTE]
>Aktarım Katmanı Güvenliği (TLS), güvenli iletişim sağlayan bir protokoldür. TLS ayarlarını değiştirmek tüm ormanı etkiler. Daha fazla bilgi için, [Windows'da WinHTTP'de varsayılan güvenli protokoller olarak TLS 1.1 ve TLS 1.2'yi etkinleştirmek için Güncelleştirme'ye](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi)bakın.

Azure AD Connect bulut sağlama aracısını barındıran Windows sunucusunun yüklemeden önce TLS 1.2 etkinleştirilmiş olması gerekir.

TLS 1.2'yi etkinleştirmek için aşağıdaki adımları izleyin.

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

