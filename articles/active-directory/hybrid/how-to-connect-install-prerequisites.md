---
title: 'Azure AD Connect: Önkoşullar ve donanım | Microsoft Docs'
description: Bu konu, Azure AD Connect için önkoşulları ve donanım gereksinimlerini açıklamaktadır.
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
ms.openlocfilehash: 6446b039d90e04c9fe7fca28b361f620183a0292
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80875750"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Azure AD Connect Önkoşulları
Bu konu, Azure AD Connect için önkoşulları ve donanım gereksinimlerini açıklamaktadır.

## <a name="before-you-install-azure-ad-connect"></a>Azure AD Connect yüklemeden önce
Azure AD Connect yüklemeden önce, ihtiyacınız olan birkaç şey vardır.

### <a name="azure-ad"></a>Azure AD
* Azure AD kiracısı. [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)ile bir tane alırsınız. Azure AD Connect yönetmek için aşağıdaki portallardan birini kullanabilirsiniz:
  * [Azure Portal](https://portal.azure.com).
  * [Office portalı](https://portal.office.com).  
* Azure AD 'de kullanmayı planladığınız [etki alanını ekleyin ve doğrulayın](../active-directory-domains-add-azure-portal.md) . Örneğin, kullanıcılarınız için contoso.com kullanmayı planlıyorsanız, bu etki alanının doğrulandığından ve yalnızca contoso.onmicrosoft.com varsayılan etki alanını kullandığınızdan emin olun.
* Bir Azure AD kiracısı varsayılan 50.000 nesnelerine izin verir. Etki alanınızı doğruladıktan sonra sınır, 300k nesnelerine yükseltilir. Azure AD 'de daha fazla nesne gerekiyorsa, sınırın daha da artması için bir destek talebi açmanız gerekir. 500 ' den fazla nesne gerekiyorsa, Office 365, Azure AD Temel, Azure AD Premium veya Enterprise Mobility ve Security gibi bir lisansa sahip olmanız gerekir.

### <a name="prepare-your-on-premises-data"></a>Şirket içi verilerinizi hazırlama
* Azure AD ve Office 365 ile eşitlemeden önce dizininizde bulunan yinelemeler ve biçimlendirme sorunları gibi hataları belirlemek için [ıddüzeltmesini](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) kullanın.
* [Azure AD 'de etkinleştirebileceğiniz isteğe bağlı eşitleme özelliklerini](how-to-connect-syncservice-features.md) gözden geçirin ve hangi özellikleri etkinleştirmeniz gerektiğini değerlendirin.

### <a name="on-premises-active-directory"></a>Şirket içi Active Directory
* AD şeması sürümü ve orman işlev düzeyi Windows Server 2003 veya sonraki bir sürümü olmalıdır. Şema ve Orman düzeyi gereksinimleri karşılandığı sürece etki alanı denetleyicileri herhangi bir sürümü çalıştırabilir.
* Özellik **parola geri yazma**özelliğini kullanmayı planlıyorsanız, etki alanı denetleyicilerinin Windows Server 2008 R2 veya sonraki bir sürümde olması gerekir.
* Azure AD tarafından kullanılan etki alanı denetleyicisi yazılabilir olmalıdır. RODC (salt okuma etki alanı denetleyicisi) kullanılması **desteklenmez** ve Azure AD Connect herhangi bir yazma yeniden yönlendirmelerini izlemez.
* Şirket içi ormanlarda/etki alanlarının "noktalı" kullanılarak kullanılması **desteklenmez** (ad bir nokta içerir ".") NetBIOS adları.
* [Active Directory geri dönüşüm kutusunun etkinleştirilmesi](how-to-connect-sync-recycle-bin.md)önerilir.

### <a name="azure-ad-connect-server"></a>Azure AD Connect sunucusu
>[!IMPORTANT]
>Azure AD Connect sunucusu kritik kimlik verileri içerir ve [Active Directory Yönetim Katmanı modelinde](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) belgelendiği şekilde katman 0 bileşeni olarak değerlendirilmelidir

* Azure AD Connect, 2019 öncesi Small Business Server veya Windows Server Essentials üzerine yüklenemez (Windows Server Essentials 2019 desteklenir). Sunucu, Windows Server Standard veya daha iyi bir using olmalıdır.
* Azure AD Connect doğru yüklenmesini engelleyebilecek güvenlik uygulamaları ve daha kısıtlayıcı ayarlar nedeniyle Azure AD Connect bir etki alanı denetleyicisine yüklenmesi önerilmez.
* Azure AD Connect sunucuda tam GUI yüklü olmalıdır. Sunucu Çekirdeği üzerine yüklenmesi **desteklenmez** .
>[!IMPORTANT]
>Small Business Server, Server Essentials veya Server Core üzerinde Azure AD Connect yükleme desteklenmez.

* Azure AD Connect, Windows Server 2012 veya sonraki bir sürüme yüklenmiş olmalıdır. Bu sunucu etki alanına katılmış olmalıdır ve bir etki alanı denetleyicisi veya üye sunucu olabilir.
* ADFS yapılandırmasını yönetmek için Azure AD Connect Sihirbazı kullanıyorsanız, Azure AD Connect sunucusu PowerShell döküm grup ilkesi etkin olmamalıdır. Eşitleme yapılandırmasını yönetmek için Azure AD Connect Sihirbazı kullanıyorsanız PowerShell dökümünü etkinleştirebilirsiniz.
* Active Directory Federasyon Hizmetleri (AD FS) dağıtılırsa, AD FS veya Web uygulaması proxy 'Sinin yüklü olduğu sunucular Windows Server 2012 R2 veya üzeri olmalıdır. Uzaktan Yükleme için bu sunucularda [Windows Uzaktan Yönetimi](#windows-remote-management) etkinleştirilmelidir.
* Active Directory Federasyon Hizmetleri (AD FS) dağıtılmışsa, [TLS/SSL sertifikalarına](#tlsssl-certificate-requirements)ihtiyacınız vardır.
* Active Directory Federasyon Hizmetleri (AD FS) dağıtılmışsa, [ad çözümlemesi](#name-resolution-for-federation-servers)yapılandırmanız gerekir.
* Genel yöneticileriniz MFA etkinleştirilmişse, URL **https://secure.aadcdn.microsoftonline-p.com** 'nin güvenilir siteler listesinde olması gerekir. MFA sınaması istendiğinde bu siteyi Güvenilen siteler listesine eklemeniz istenir ve daha önce eklenmediyse. Internet Explorer 'ı, güvenilen sitelerinize eklemek için kullanabilirsiniz.
* Microsoft, BT ortamınızın bu kritik bileşeninin güvenlik saldırısı yüzeyini azaltmak için Azure AD Connect sunucunuzu sağlamlaştırmanızı önerir.  Aşağıdaki önerilerin ardından kuruluşunuza yönelik güvenlik riskleri azalır.

* Etki alanına katılmış bir sunucuda Azure AD Connect dağıtın ve etki alanı yöneticilerine veya daha sıkı denetlenen güvenlik gruplarına yönetici erişimini kısıtlayın.

Daha fazla bilgi için bkz: 

* [Yöneticiler gruplarının güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Yerleşik yönetici hesaplarının güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Saldırı yüzeylerini azaltarak güvenlik iyileştirmesi ve sürdürülebilirlik](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Active Directory saldırı yüzeyini azaltma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

### <a name="sql-server-used-by-azure-ad-connect"></a>Azure AD Connect tarafından kullanılan SQL Server
* Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Varsayılan olarak bir SQL Server 2012 Express LocalDB (SQL Server Express bir açık sürümü) yüklenir. SQL Server Express, yaklaşık 100.000 nesneyi yönetmenizi sağlayan 10 GB boyut sınırına sahiptir. Dizin nesnelerinin daha yüksek bir birimini yönetmeniz gerekiyorsa, Yükleme Sihirbazı 'nı farklı bir SQL Server yüklemesine işaret etmeniz gerekir. SQL Server yüklemesinin türü [Azure AD Connect performansını](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-performance-factors#sql-database-factors)etkileyebilir.
* Farklı bir SQL Server yüklemesi kullanıyorsanız, bu gereksinimler geçerlidir:
  * Azure AD Connect, 2012 (en son hizmet paketiyle) Microsoft SQL Server tüm sürümlerini SQL Server 2019 ' e destekler. Microsoft Azure SQL Veritabanı veritabanı olarak **desteklenmez** .
  * Büyük/küçük harfe duyarsız bir SQL harmanlaması kullanmanız gerekir. Bu harmanlamalar, adında bir \_CI_ tanımlanır. CS_ tarafından **not supported** \_tanımlanan, büyük/küçük harfe duyarlı harmanlama kullanılması desteklenmez.
  * SQL örneği başına yalnızca bir eşitleme motoruna sahip olabilirsiniz. Bir SQL örneğinin FIM/MıM Sync, DirSync veya Azure AD Eşitleme paylaşılması **desteklenmez** .

### <a name="accounts"></a>Hesaplar
* Tümleştirmek istediğiniz Azure AD kiracısı için bir Azure AD Genel yönetici hesabı. Bu hesabın bir **okul veya kuruluş hesabı** olması ve bir **Microsoft hesabı**olamaz.
* [Hızlı ayarları](reference-connect-accounts-permissions.md#express-settings-installation) kullanıyorsanız veya DirSync 'ten yükseltirseniz, şirket içi Active Directory Için bir kuruluş yöneticisi hesabınız olmalıdır.
* Özel ayarlar yükleme yolu ' nu kullanırsanız, daha fazla seçeneğiniz vardır. Daha fazla bilgi için bkz. [özel yükleme ayarları](reference-connect-accounts-permissions.md#custom-installation-settings).

### <a name="connectivity"></a>Bağlantı
* Azure AD Connect sunucusu hem intranet hem de internet için DNS çözümüne ihtiyaç duyuyor. DNS sunucusu, adları hem şirket içi Active Directory hem de Azure AD uç noktalarına çözümleyebilmelidir.
* Intranetinizde güvenlik duvarları varsa ve Azure AD Connect sunucuları ile etki alanı denetleyicileriniz arasında bağlantı noktaları açmanız gerekiyorsa, daha fazla bilgi için [Azure AD Connect bağlantı noktaları](reference-connect-ports.md) bölümüne bakın.
* Proxy 'niz veya güvenlik duvarınız hangi URL 'Lere erişilebileceğini sınırlarıyorsa, [Office 365 URL 'lerinde ve IP adresi aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) belgelenen URL 'lerin açılması gerekir.
  * Almanya veya Microsoft Azure Kamu Bulutu 'nda Microsoft Bulut kullanıyorsanız, URL 'Ler için [Azure AD Connect eşitleme hizmeti örneklerine göz](reference-connect-instances.md) atın bölümüne bakın.
* Azure AD Connect (sürüm 1.1.614.0 ve sonrası), eşitleme altyapısı ile Azure AD arasındaki iletişimi şifrelemek için varsayılan olarak TLS 1,2 kullanır. TLS 1,2, temel işletim sisteminde kullanılabilir değilse, Azure AD Connect daha eski protokollere (TLS 1,1 ve TLS 1,0) artımlı olarak geri döner.
* Sürüm 1.1.614.0 ' den önce, eşitleme altyapısı ve Azure AD arasındaki iletişimi şifrelemek için varsayılan olarak Azure AD Connect TLS 1,0 kullanır. TLS 1,2 ' ye geçmek için [Azure AD Connect IÇIN tls 1,2](#enable-tls-12-for-azure-ad-connect)' yi etkinleştirme ' deki adımları izleyin.
* Internet 'e bağlanmak için bir giden Proxy kullanıyorsanız, Yükleme Sihirbazı için **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** dosyasında aşağıdaki ayar eklenmelidir ve Internet ve Azure AD 'ye bağlanabilmeleri için Azure AD Connect eşitleme yapılmalıdır. Bu metin, dosyanın alt kısmına girilmelidir. Bu kodda, &lt;PROXYADDRESS&gt; gerçek Proxy IP adresini veya ana bilgisayar adını temsil eder.

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

* Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, [hizmet hesabı](reference-connect-accounts-permissions.md#adsync-service-account) etki alanında yer almalıdır ve [özel bir hizmet hesabı](how-to-connect-install-custom.md#install-required-components)belirtmek için özelleştirilmiş ayarlar yükleme yolunu kullanmanız gerekir. Ayrıca Machine. config dosyasında farklı bir değişikliğe de ihtiyacınız vardır. Machine. config dosyasında bu değişiklik ile Yükleme Sihirbazı ve eşitleme altyapısı, proxy sunucusundan gelen kimlik doğrulama isteklerine yanıt verir. Tüm Yükleme Sihirbazı sayfalarında, **Yapılandır** sayfası hariç, oturum açan kullanıcının kimlik bilgileri kullanılır. Yükleme sihirbazının sonundaki **Yapılandır** sayfasında, bağlam sizin oluşturduğunuz [hizmet hesabına](reference-connect-accounts-permissions.md#adsync-service-account) geçiş yapılır. Machine. config bölümü şöyle görünmelidir.

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

* Azure AD Connect, Dizin eşitlemesinin bir parçası olarak Azure AD 'ye bir Web isteği gönderdiğinde, Azure AD 'nin yanıtlaması 5 dakika sürebilir. Proxy sunucularda bağlantı boşta kalma zaman aşımı yapılandırması olması yaygındır. Lütfen yapılandırmanın en az 6 dakika veya daha fazla ayarlandığından emin olun.

Daha fazla bilgi için bkz. [varsayılan proxy öğesi](https://msdn.microsoft.com/library/kd3cf2ex.aspx)hakkında MSDN.  
Bağlantı sorunlarıyla karşılaşırsanız daha fazla bilgi için bkz. [bağlantı sorunlarını giderme](tshoot-connect-connectivity.md).

### <a name="other"></a>Diğer
* İsteğe bağlı: eşitlemeyi doğrulamak için bir test Kullanıcı hesabı.

## <a name="component-prerequisites"></a>Bileşen önkoşulları
### <a name="powershell-and-net-framework"></a>PowerShell ve .NET Framework
Azure AD Connect, Microsoft PowerShell ve .NET Framework 4.5.1 bağımlıdır. Sunucunuzda bu sürüm veya sonraki bir sürümün yüklü olması gerekir. Windows Server sürümünüze bağlı olarak şunları yapın:

* Windows Server 2012R2
  * Microsoft PowerShell varsayılan olarak yüklüdür. İşlem yapmanız gerekmez.
  * .NET Framework 4.5.1 ve üzeri sürümler Windows Update aracılığıyla sunulur. Denetim Masası 'nda Windows Server için en son güncelleştirmeleri yüklediğinizden emin olun.
* Windows Server 2012
  * Microsoft PowerShell 'in en son sürümü, [Microsoft Indirme merkezi](https://www.microsoft.com/downloads)'Nde bulunan **Windows Management Framework 4,0**' de kullanılabilir.
  * .NET Framework 4.5.1 ve üzeri sürümler [Microsoft Indirme merkezi](https://www.microsoft.com/downloads)' nde bulunabilir.


### <a name="enable-tls-12-for-azure-ad-connect"></a>Azure AD Connect için TLS 1,2 'yi etkinleştirin
Sürüm 1.1.614.0 ' den önce, eşitleme altyapısı sunucusu ile Azure AD arasındaki iletişimi şifrelemek için varsayılan olarak Azure AD Connect TLS 1,0 kullanır. Bunu, .NET uygulamalarını sunucuda varsayılan olarak TLS 1,2 kullanacak şekilde yapılandırarak değiştirebilirsiniz. TLS 1,2 hakkında daha fazla bilgi için [Microsoft Güvenlik Danışmanlığı 2960358](https://technet.microsoft.com/security/advisory/2960358)' de bulabilirsiniz.

1.  İşletim sisteminiz için .NET 4.5.1 düzeltmesinin yüklü olduğundan emin olun, bkz. [Microsoft Güvenlik Danışmanlığı 2960358](https://technet.microsoft.com/security/advisory/2960358). Bu düzeltme veya daha sonraki bir sürümü sunucunuzda zaten yüklü olabilir.
    ```
2. For all operating systems, set this registry key and restart the server.
    ```
    HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\.NETFramework\v4.0.30319 "Schusestrongşifre" = DWORD: 00000001
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
* It's strongly recommended to use the same TLS/SSL certificate across all nodes of your AD FS farm and all Web Application proxy servers.
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
