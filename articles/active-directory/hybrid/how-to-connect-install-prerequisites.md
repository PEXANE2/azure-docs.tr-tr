---
title: 'Azure AD Connect: Ön koşullar ve donanım | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect için ön koşulları ve donanım gereksinimlerini açıklar
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79741557e6eea1b4252e5ab4d9976b124cea1169
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346898"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD Connect Önkoşulları
Bu konu, Azure AD Connect için ön koşulları ve donanım gereksinimlerini açıklar.

## <a name="before-you-install-azure-ad-connect"></a>Azure AD Connect'i yüklemeden önce
Azure AD Connect'i yüklemeden önce ihtiyacınız olan birkaç şey vardır.

### <a name="azure-ad"></a>Azure AD
* Azure AD kiracısı. [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)sürümü yle bir tane elde elabilirsiniz. Azure AD Connect'i yönetmek için aşağıdaki portallardan birini kullanabilirsiniz:
  * [Azure portalı.](https://portal.azure.com)
  * [Ofis portalı.](https://portal.office.com)  
* Azure AD'de kullanmayı planladığınız [etki alanını ekleyin ve doğrulayın.](../active-directory-domains-add-azure-portal.md) Örneğin, kullanıcılarınız için contoso.com kullanmayı planlıyorsanız, bu etki alanının doğrulandığından ve yalnızca contoso.onmicrosoft.com varsayılan etki alanını kullanmadığınızdan emin olun.
* Azure AD kiracısı varsayılan olarak 50k nesnelerine izin verir. Etki alanınızı doğruladığınızda, sınır 300k nesnelere yükseltilir. Azure AD'de daha fazla nesneye ihtiyacınız varsa, sınırın daha da artırılması için bir destek örneği açmanız gerekir. 500.000'den fazla nesneye ihtiyacınız varsa, Office 365, Azure AD Basic, Azure AD Premium veya Kurumsal Mobilite ve Güvenlik gibi bir lisansa ihtiyacınız vardır.

### <a name="prepare-your-on-premises-data"></a>Şirket içi verilerinizi hazırlayın
* Azure AD ve Office 365 ile eşitleme yapmadan önce dizininizdeki yinelenenler ve biçimlendirme sorunları gibi hataları tanımlamak için [IdFix'i](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) kullanın.
* [Azure AD'de etkinleştirebileceğiniz isteğe bağlı eşitleme özelliklerini](how-to-connect-syncservice-features.md) gözden geçirin ve hangi özellikleri etkinleştirmeniz gerektiğini değerlendirin.

### <a name="on-premises-active-directory"></a>Şirket içi Active Directory
* AD şema sürümü ve orman işlevsel düzeyi Windows Server 2003 veya daha sonra olmalıdır. Etki alanı denetleyicileri, şema ve orman düzeyi gereksinimleri karşılandığı sürece herhangi bir sürümü çalıştırabilir.
* Özellik **parolasını geri**yazmayı planlıyorsanız, Etki Alanı Denetleyicileri Windows Server 2008 R2 veya sonraki adresinde olmalıdır.
* Azure AD tarafından kullanılan etki alanı denetleyicisinin yazılabilir olması gerekir. RODC (salt okunur etki alanı denetleyicisi) kullanmak **desteklenmez** ve Azure AD Connect herhangi bir yazma yönlendirmesini izlemez.
* "Noktalı" (ad bir dönem "içerir") kullanılarak şirket içi ormanların/etki alanlarının kullanılması **desteklenmez.** NetBios isimleri.
* [Active Directory geri dönüşüm kutusunu etkinleştirmek](how-to-connect-sync-recycle-bin.md)önerilir.

### <a name="azure-ad-connect-server"></a>Azure AD Connect sunucusu
>[!IMPORTANT]
>Azure AD Connect sunucusu kritik kimlik verileri içerir ve [Active Directory yönetim katmanı modelinde](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) belgelenmiş olarak Katman 0 bileşeni olarak ele alınmalıdır

* Azure AD Connect, Small Business Server veya Windows Server Essentials'a 2019'dan önce yüklenemez (Windows Server Essentials 2019 desteklenir). Sunucu Windows Server standart veya daha iyi kullanıyor olmalıdır.
* Güvenlik uygulamaları ve Azure AD Connect'in düzgün yüklenmesini engelleyebilecek daha kısıtlayıcı ayarlar nedeniyle Etki Alanı Denetleyicisine Azure AD Connect'in yüklenmesi önerilmez.
* Azure AD Connect sunucusunda tam bir GUI yüklü olmalıdır. Sunucu çekirdeğine yüklemek için **desteklenmez.**
>[!IMPORTANT]
>Azure AD Connect'in küçük işletme sunucusuna, sunucu temellerine veya sunucu çekirdeğine yüklenmesi desteklenmez.

* Azure AD Connect, Windows Server 2012 veya sonraki bir tarihte yüklenmiş olmalıdır. Bu sunucu etki alanı birleştirilmiş olmalıdır ve bir etki alanı denetleyicisi veya bir üye sunucu olabilir.
* ADFS yapılandırmasını yönetmek için Azure AD Connect sihirbazını kullanıyorsanız Azure AD Connect sunucusunda PowerShell Transkripsiyon Grubu İlkesi etkin olmamalıdır. Eşitleme yapılandırmasını yönetmek için Azure AD Connect sihirbazı kullanıyorsanız PowerShell transkripsiyonünü etkinleştirebilirsiniz.
* Active Directory Federation Services dağıtılıyorsa, AD FS veya Web Application Proxy'nin yüklü olduğu sunucular Windows Server 2012 R2 veya sonraki olmalıdır. [Uzaktan](#windows-remote-management) yükleme için bu sunucularda Windows uzaktan yönetimi etkinleştirilmelidir.
* Active Directory Federation Services dağıtılıyorsa, [TLS/SSL Sertifikalarına](#tlsssl-certificate-requirements)ihtiyacınız vardır.
* Etkin Dizin Federasyon Hizmetleri dağıtılıyorsa, ad [çözümünü](#name-resolution-for-federation-servers)yapılandırmanız gerekir.
* Global yöneticilerinizin MFA etkinleştirilmiş olması **https://secure.aadcdn.microsoftonline-p.com** durumunda, URL'nin güvenilir siteler listesinde olması gerekir. MFA mücadelesi için istendiğinde ve daha önce eklemediğinde bu siteyi güvenilir siteler listesine eklemeniz istenir. Internet Explorer'ı güvendiğiniz sitelere eklemek için kullanabilirsiniz.
* Microsoft, BT ortamınızın bu kritik bileşeni için güvenlik saldırı yüzeyini azaltmak için Azure AD Connect sunucunuzun sertleştirmesini önerir.  Aşağıdaki önerileri izleyerek kuruluşunuz için güvenlik riskleri azalacaktır.

* Azure AD Connect'i birleştirilmiş bir etki alanında dağıtın ve etki alanı yöneticilerine veya sıkı denetimaltındaki diğer güvenlik gruplarına yönetim erişimi kısıtlayın.

Daha fazla bilgi için bkz: 

* [Yönetici gruplarını güvence altına alma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Yerleşik yönetici hesaplarının güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Saldırı yüzeylerini azaltarak güvenlik iyileştirme ve sürdürme](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Active Directory saldırı yüzeyini azaltma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

### <a name="sql-server-used-by-azure-ad-connect"></a>Azure AD Connect tarafından kullanılan SQL Server
* Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Varsayılan olarak bir SQL Server 2012 Express LocalDB (SQL Server Express'in hafif bir sürümü) yüklenir. SQL Server Express, yaklaşık 100.000 nesneyi yönetmenize olanak tanıyan 10 GB boyut sınırına sahiptir. Daha yüksek bir dizin nesnesi hacmini yönetmeniz gerekiyorsa, yükleme sihirbazını farklı bir SQL Server yüklemesine işaret etmeniz gerekir. SQL Server yükleme türü [Azure AD Connect performansını](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors#sql-database-factors)etkileyebilir.
* SQL Server'ın farklı bir yüklemesini kullanıyorsanız, bu gereksinimler geçerlidir:
  * Azure AD Connect, Microsoft SQL Server'ın 2012'den SQL Server 2019'a kadar tüm sürümlerini destekler. Microsoft Azure SQL Veritabanı veritabanı olarak **desteklenmez.**
  * Büyük/küçük harf duyarsız SQL harmanlama kullanmanız gerekir. Bu harmanlamalar kendi \_adlarına bir CI_ ile tanımlanır. CS_ tarafından \_kendi adlarına tanımlanan büyük/küçük harf duyarlı bir harmanlama kullanmak **desteklenmez.**
  * SQL örneği başına yalnızca bir eşitleme motorunuz olabilir. BIR SQL örneğini FIM/MIM Sync, DirSync veya Azure AD Eşitlemesi ile paylaşmak **desteklenmez.**

### <a name="accounts"></a>Hesaplar
* Tümleştirmek istediğiniz Azure AD kiracısı için bir Azure AD Global Administrator hesabı. Bu hesap bir **okul veya kuruluş hesabı** olmalıdır ve microsoft **hesabı**olamaz.
* [Açık ayarları](reference-connect-accounts-permissions.md#express-settings-installation) kullanıyorsanız veya DirSync'den yükseltme yapıyorsunuz, şirket içi Active Directory'niz için bir Kurumsal Yönetici hesabınız olması gerekir.
* Özel ayarlar yükleme yolunu kullanırsanız, Daha fazla seçeneğiniz var: [Active Directory'deki Hesaplar'a](reference-connect-accounts-permissions.md#custom-installation-settings) bakın

### <a name="connectivity"></a>Bağlantı
* Azure AD Connect sunucusunun hem intranet hem de internet için DNS çözünürlüğüne ihtiyacı vardır. DNS sunucusu, adları hem şirket içi Etkin Dizininizde hem de Azure AD uç noktalarınıza çözümleyebilmeli.
* İntranet'inizde güvenlik duvarları varsa ve Azure AD Connect sunucuları ile etki alanı denetleyicileriniz arasında bağlantı noktaları açmanız gerekiyorsa, daha fazla bilgi için [Azure AD Bağlantı Bağlantı Noktaları'na](reference-connect-ports.md) bakın.
* URL'lere erişilebilen proxy veya güvenlik duvarı sınırınız varsa, Office [365 URL'lerinde ve IP adresi aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) belgelenen URL'lerin açılması gerekir.
  * Microsoft Bulut'u Almanya'da veya Microsoft Azure Kamu bulutunda kullanıyorsanız, URL'ler için [Azure AD Connect eşitleme hizmeti örneklerine](reference-connect-instances.md) bakın.
* Azure AD Connect (sürüm 1.1.614.0 ve sonrası) varsayılan olarak eşitleme motoru ile Azure AD arasındaki iletişimi şifrelemek için TLS 1.2 kullanır. TLS 1.2 temel işletim sisteminde kullanılamıyorsa, Azure AD Connect kademeli olarak eski protokollere (TLS 1.1 ve TLS 1.0) geri döner.
* Azure AD Connect, sürüm 1.1.614.0 sürümünden önce, eşitleme motoru ile Azure AD arasındaki iletişimi şifrelemek için varsayılan olarak TLS 1.0 kullanır. TLS 1.2'ye geçiş yapmak [için Azure AD Connect için TLS 1.2'yi etkinleştir'deki](#enable-tls-12-for-azure-ad-connect)adımları izleyin.
* Internet'e bağlanmak için giden bir proxy kullanıyorsanız, Yükleme sihirbazı ve Azure AD Bağlantısı eşitlemesi için Yükleme sihirbazı ve Azure AD Connect eşitlemesi için **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** dosyasındaki aşağıdaki ayarı eklemeniz gerekir. Bu metin dosyanın altına girilmelidir. Bu kodda &lt;PROXYADDRESS&gt; gerçek proxy IP adresini veya ana bilgisayar adını temsil eder.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, [hizmet hesabının](reference-connect-accounts-permissions.md#adsync-service-account) etki alanında bulunması gerekir ve özel bir [hizmet hesabı](how-to-connect-install-custom.md#install-required-components)belirtmek için özelleştirilmiş ayarlar yükleme yolunu kullanmanız gerekir. Ayrıca makine.config farklı bir değişiklik gerekir. machine.config'deki bu değişiklikle, yükleme sihirbazı ve eşitleme altyapısı proxy sunucusundan gelen kimlik doğrulama isteklerine yanıt verir. **Yapılandırma** sayfası hariç tüm yükleme sihirbazı sayfalarında, kullanıcının kimlik bilgilerinde imzalanan oturum kullanılır. Yükleme sihirbazının sonundaki **Yapıla** sayfasında, bağlam sizin oluşturduğunuz [hizmet hesabına](reference-connect-accounts-permissions.md#adsync-service-account) geçer. Makine.config bölümü böyle görünmeli.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Azure AD Connect dizin eşitlemesi kapsamında Azure AD'ye bir web isteği gönderdiğinde, Azure AD'nin yanıt vermesi 5 dakika kadar sürebilir. Proxy sunucuların bağlantı boşta zaman alabildiğine yapılandırması yaygındır. Lütfen yapılandırmanın en az 6 dakika veya daha uzun bir saat olarak ayarlandığından emin olun.

Daha fazla bilgi için varsayılan [proxy Öğesi](https://msdn.microsoft.com/library/kd3cf2ex.aspx)hakkında MSDN'ye bakın.  
Bağlantıyla ilgili sorunlarınız olduğunda daha fazla bilgi için [Sorun Giderme bağlantısı sorunlarına](tshoot-connect-connectivity.md)bakın.

### <a name="other"></a>Diğer
* İsteğe bağlı: Eşitlemayı doğrulamak için bir test kullanıcı hesabı.

## <a name="component-prerequisites"></a>Bileşen ön koşulları
### <a name="powershell-and-net-framework"></a>PowerShell ve .NET Çerçevesi
Azure AD Connect, Microsoft PowerShell ve .NET Framework 4.5.1'e bağlıdır. Bu sürümün veya sunucunuzda daha sonraki bir sürümün yüklü bir sürümün yüklü olduğunu bilmeniz gerekir. Windows Server sürümünüze bağlı olarak aşağıdakileri yapın:

* Windows Server 2012R2
  * Microsoft PowerShell varsayılan olarak yüklenir. İşlem yapmanız gerekmez.
  * .NET Framework 4.5.1 ve sonraki sürümler Windows Update aracılığıyla sunulur. Denetim Masası'nda Windows Server için en son güncelleştirmeleri yüklediğinizden emin olun.
* Windows Server 2012
  * Microsoft PowerShell'in en son sürümü **Windows Management Framework 4.0'da**mevcuttur, [Microsoft Download Center'da](https://www.microsoft.com/downloads)mevcuttur.
  * .NET Framework 4.5.1 ve sonraki sürümler [Microsoft Download Center'da](https://www.microsoft.com/downloads)mevcuttur.


### <a name="enable-tls-12-for-azure-ad-connect"></a>Azure AD Connect için TLS 1.2'yi etkinleştirme
Azure AD Connect, sürüm 1.1.614.0 sürümünden önce, eşitleme motoru sunucusu ile Azure AD arasındaki iletişimi şifrelemek için varsayılan olarak TLS 1.0 kullanır. Bunu, sunucuda varsayılan olarak TLS 1.2 kullanacak .NET uygulamalarını yapılandırarak değiştirebilirsiniz. TLS 1.2 hakkında daha fazla bilgiyi [Microsoft Security Advisory 2960358'de](https://technet.microsoft.com/security/advisory/2960358)bulabilirsiniz.

1.  İşletim sisteminiz için .NET 4.5.1 düzeltmesinin yüklü olduğundan emin olun, [bkz.](https://technet.microsoft.com/security/advisory/2960358) Bu düzeltmeyi veya daha sonraki bir sürümü sunucunuzda zaten yüklü olabilir.
    ```
2. For all operating systems, set this registry key and restart the server.
    ```
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319 "SchUseStrongCrypto"=dword:00000001
    ```
4. If you also want to enable TLS 1.2 between the sync engine server and a remote SQL Server, then make sure you have the required versions installed for [TLS 1.2 support for Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## Prerequisites for federation installation and configuration
### Windows Remote Management
When using Azure AD Connect to deploy Active Directory Federation Services or the Web Application Proxy, check these requirements:

* If the target server is domain joined, then ensure that Windows Remote Managed is enabled
  * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
* If the target server is a non-domain joined WAP machine, then there are a couple of additional requirements
  * On the target machine (WAP machine):
    * Ensure the winrm (Windows Remote Management / WS-Management) service is running via the Services snap-in
    * In an elevated PowerShell command window, use command `Enable-PSRemoting –force`
  * On the machine on which the wizard is running (if the target machine is non-domain joined or untrusted domain):
    * In an elevated PowerShell command window, use the command `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * add DMZ WAP host to machine pool (server manager -> Manage -> Add Servers...use DNS tab)
      * Server Manager All Servers tab: right click WAP server and choose Manage As..., enter local (not domain) creds for the WAP machine
      * To validate remote PowerShell connectivity, in the Server Manager All Servers tab: right click WAP server and choose Windows PowerShell. A remote PowerShell session should open to ensure remote PowerShell sessions can be established.

### TLS/SSL Certificate Requirements
* It’s strongly recommended to use the same TLS/SSL certificate across all nodes of your AD FS farm and all Web Application proxy servers.
* The certificate must be an X509 certificate.
* You can use a self-signed certificate on federation servers in a test lab environment. However, for a production environment, we recommend that you obtain the certificate from a public CA.
  * If using a certificate that is not publicly trusted, ensure that the certificate installed on each Web Application Proxy server is trusted on both the local server and on all federation servers
* The identity of the certificate must match the federation service name (for example, sts.contoso.com).
  * The identity is either a subject alternative name (SAN) extension of type dNSName or, if there are no SAN entries, the subject name specified as a common name.  
  * Multiple SAN entries can be present in the certificate, provided one of them matches the federation service name.
  * If you are planning to use Workplace Join, an additional SAN is required with the value **enterpriseregistration.** followed by the User Principal Name (UPN) suffix of your organization, for example, **enterpriseregistration.contoso.com**.
* Certificates based on CryptoAPI next generation (CNG) keys and key storage providers are not supported. This means you must use a certificate based on a CSP (cryptographic service provider) and not a KSP (key storage provider).
* Wild-card certificates are supported.

### Name resolution for federation servers
* Set up DNS records for the AD FS federation service name (for example sts.contoso.com) for both the intranet (your internal DNS server) and the extranet (public DNS through your domain registrar). For the intranet DNS record, ensure that you use A records and not CNAME records. This is required for windows authentication to work correctly from your domain joined machine.
* If you are deploying more than one AD FS server or Web Application Proxy server, then ensure that you have configured your load balancer and that the DNS records for the AD FS federation service name (for example sts.contoso.com) point to the load balancer.
* For windows integrated authentication to work for browser applications using Internet Explorer in your intranet, ensure that the AD FS federation service name (for example sts.contoso.com) is added to the intranet zone in IE. This can be controlled via group policy and deployed to all your domain joined computers.

## Azure AD Connect supporting components
The following is a list of components that Azure AD Connect installs on the server where Azure AD Connect is installed. This list is for a basic Express installation. If you choose to use a different SQL Server on the Install synchronization services page, then SQL Express LocalDB is not installed locally.

* Azure AD Connect Health
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013 Redistribution Package

## Hardware requirements for Azure AD Connect
The table below shows the minimum requirements for the Azure AD Connect sync computer.

| Number of objects in Active Directory | CPU | Memory | Hard drive size |
| --- | --- | --- | --- |
| Fewer than 10,000 |1.6 GHz |4 GB |70 GB |
| 10,000–50,000 |1.6 GHz |4 GB |70 GB |
| 50,000–100,000 |1.6 GHz |16 GB |100 GB |
| For 100,000 or more objects the full version of SQL Server is required | | | |
| 100,000–300,000 |1.6 GHz |32 GB |300 GB |
| 300,000–600,000 |1.6 GHz |32 GB |450 GB |
| More than 600,000 |1.6 GHz |32 GB |500 GB |

The minimum requirements for computers running AD FS or Web Application Proxy Servers is the following:

* CPU: Dual core 1.6 GHz or higher
* MEMORY: 2 GB or higher
* Azure VM: A2 configuration or higher

## Next steps
Learn more about [Integrating your on-premises identities with Azure Active Directory](whatis-hybrid-identity.md).
