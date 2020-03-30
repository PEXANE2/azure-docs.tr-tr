---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulut'ta kimlik kaynağı olarak Azure AD'yi kullanın
description: Azure'dan CloudSimple'a erişen kullanıcıların kimliğini doğrulamak için CloudSimple Private Cloud'unuzda bir kimlik sağlayıcısı olarak Azure AD'nin nasıl ekleyeceğiniz açıklanır
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 674ca8bea110d60557d1e50e7b68c9c3f7a92bf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77564593"
---
# <a name="use-azure-ad-as-an-identity-provider-for-vcenter-on-cloudsimple-private-cloud"></a>CloudSimple Private Cloud'da vCenter için kimlik sağlayıcısı olarak Azure AD'yi kullanma

CloudSimple Private Cloud vCenter'ınızı, VMware yöneticilerinizin vCenter'a erişebilmek için Azure Active Directory (Azure AD) ile kimlik doğrulaması yapacak şekilde ayarlayabilirsiniz. Tek oturum açma kimlik kaynağı ayarlandıktan **sonra, bulut sahibi** kullanıcı kimlik kaynağından vCenter'a kullanıcı ekleyebilir.  

Active Directory etki alanı nızı ve etki alanı denetleyicilerinizi aşağıdaki yollardan herhangi birinde ayarlayabilirsiniz:

* Şirket içinde çalışan Etkin Dizin etki alanı ve etki alanı denetleyicileri
* Azure aboneliğinizde sanal makine olarak Azure'da çalışan Etkin Dizin etki alanı ve etki alanı denetleyicileri
* CloudSimple Private Cloud'unuzda çalışan yeni Active Directory etki alanı ve etki alanı denetleyicileri
* Azure Active Directory hizmeti

Bu kılavuzda, Azure AD'yi kimlik kaynağı olarak ayarlamak için gereken görevler açıklanmaktadır.  Azure'da çalışan şirket içi Active Directory veya Active Directory'yi kullanma hakkında daha fazla bilgi için, kimlik kaynağını ayarlamada ayrıntılı yönergeler için [Active Directory'yi kullanmak üzere vCenter kimlik kaynaklarını ayarlama](set-vcenter-identity.md) konusuna bakın.

## <a name="about-azure-ad"></a>Azure AD hakkında

Azure AD, Microsoft çok kiracılı, bulut tabanlı dizin ve kimlik yönetimi hizmetidir.  Azure AD, kullanıcıların Azure'da farklı hizmetlere kimlik doğrulamaları ve erişmeleri için ölçeklenebilir, tutarlı ve güvenilir bir kimlik doğrulama mekanizması sağlar.  Ayrıca, azure AD'yi kimlik doğrulama/kimlik kaynağı olarak kullanması için tüm üçüncü taraf hizmetleri için güvenli LDAP hizmetleri sağlar.  Azure AD, Özel Bulut'u yöneten kullanıcılar için Özel Bulut'unuza erişim sağlamak için kullanılabilecek temel dizin hizmetleri, gelişmiş kimlik yönetimi ve uygulama erişim yönetimini bir araya getirir.

Azure AD'yi vCenter ile kimlik kaynağı olarak kullanmak için Azure AD ve Azure AD etki alanı hizmetlerini ayarlamanız gerekir. Aşağıdaki talimatları izleyin:

1. [Azure AD ve Azure AD etki alanı hizmetleri nasıl ayarlanır?](#set-up-azure-ad-and-azure-ad-domain-services)
2. [Özel Bulut vCenter'ınızda kimlik kaynağı nasıl ayarlayınız?](#set-up-an-identity-source-on-your-private-cloud-vcenter)

## <a name="set-up-azure-ad-and-azure-ad-domain-services"></a>Azure AD ve Azure AD etki alanı hizmetlerini ayarlama

Başlamadan önce, Global Administrator ayrıcalıklarıyla Azure aboneliğinize erişmeniz gerekir.  Aşağıdaki adımlar genel yönergeler verir. Ayrıntılar Azure belgelerinde bulunur.

### <a name="azure-ad"></a>Azure AD

> [!NOTE]
> Azure AD'niz zaten varsa, bu bölümü atlayabilirsiniz.

1. [Azure AD belgelerinde](../active-directory/fundamentals/get-started-azure-ad.md)açıklandığı şekilde aboneliğinizde Azure AD'yi ayarlayın.
2. [Azure Active Directory Premium'a Kaydolun'da](../active-directory/fundamentals/active-directory-get-started-premium.md)açıklandığı gibi aboneliğinizde Azure Active Directory Premium'u etkinleştirin.
3. [Azure Etkin Dizini'ne özel bir etki alanı adı ekle'de](../active-directory/fundamentals/add-custom-domain.md)açıklandığı gibi özel bir etki alanı adı ayarlayın ve özel alan adı doğrulayın.
    1. Azure'da sağlanan bilgilerle etki alanı kayıt şirketinizde bir DNS kaydı ayarlayın.
    2. Özel etki alanı adını birincil etki alanı olarak ayarlayın.

İsteğe bağlı olarak diğer Azure AD özelliklerini yapılandırabilirsiniz.  Bunlar, Azure AD ile vCenter kimlik doğrulamasını etkinleştirmek için gerekli değildir.

### <a name="azure-ad-domain-services"></a>Azure AD etki alanı hizmetleri

> [!NOTE]
> Bu, Azure AD'yi vCenter için bir kimlik kaynağı olarak etkinleştirmek için önemli bir adımdır.  Herhangi bir sorundan kaçınmak için, tüm adımların doğru gerçekleştirildiğinden emin olun.

1. [Azure portalını kullanarak Azure Etkin Dizin etki alanı hizmetlerini etkinleştir'de](../active-directory-domain-services/active-directory-ds-getting-started.md)açıklandığı gibi Azure AD etki alanı hizmetlerini etkinleştirin.
2. [Azure portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştir'de](../active-directory-domain-services/active-directory-ds-getting-started-network.md)açıklandığı gibi Azure AD etki alanı hizmetleri tarafından kullanılacak ağı ayarlayın.
3. [Azure portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştir'de](../active-directory-domain-services/active-directory-ds-getting-started-admingroup.md)açıklandığı gibi Azure AD Etki Alanı Hizmetlerini yönetmek için Yönetici Grubu'nu yapılandırın.
4. Azure Etkin [Dizin Etki Alanı Hizmetlerini Etkinleştir'de](../active-directory-domain-services/active-directory-ds-getting-started-dns.md)açıklandığı gibi Azure AD Etki Alanı Hizmetleriniz için DNS ayarlarını güncelleştirin.  Internet üzerinden AD'ye bağlanmak istiyorsanız, Azure AD etki alanı hizmetlerinin genel IP adresi için DNS kaydını etki alanı adına ayarlayın.
5. Kullanıcılar için parola karma eşitlemesini etkinleştirin.  Bu adım, NT LAN Yöneticisi (NTLM) ve Kerberos kimlik doğrulaması için gereken parola kalıplarının Azure AD Etki Alanı Hizmetlerine eşitlenmesine olanak tanır. Parola karma eşitlemesini ayarladıktan sonra kullanıcılar, şirket kimlik bilgileri ile yönetilen etki alanında oturum açabilir. Bkz. [Azure Etkin Dizin Etki Alanı Hizmetleri için parola karma senkronizasyonunu etkinleştir.](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)
    1. Yalnızca bulut kullanıcıları varsa, parola açıkhanelerinin NTLM veya Kerberos tarafından gerekli biçimde depolanmasıiçin <a href="http://myapps.microsoft.com/" target="_blank">Azure AD erişim panelini</a> kullanarak parolalarını değiştirmeleri gerekir.  [Yalnızca bulut kullanıcı hesapları için yönetilen etki alanınıza parola karma senkronizasyonunu etkinleştirme](../active-directory-domain-services/tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)yönergelerini izleyin.  Bu adım, Azure portalı veya Azure AD PowerShell cmdlets kullanarak Azure REKLAM dizininizde oluşturulan tüm kullanıcılar ve tüm yeni kullanıcılar için yapılmalıdır. Azure AD etki alanı hizmetlerine erişim gerektiren kullanıcıların parolayı değiştirmek için <a href="http://myapps.microsoft.com/" target="_blank">Azure AD erişim panelini</a> kullanması ve profillerine erişmesi gerekir.

        > [!NOTE]
        > Kuruluşunuz yalnızca bulutta yer alan bir kullanıcı hesabına sahipse Azure Active Directory Domain Services'i kullanması gereken tüm kullanıcıların parolalarını değiştirmesi gerekir. Yalnızca bulutta yer alan bir kullanıcı hesabı, Azure portal veya Azure AD PowerShell cmdlet’leri kullanılarak Azure AD dizininizde oluşturulmuş bir hesaptır. Bu tür kullanıcı hesapları şirket içi dizinden eşitlenmez.

    2. Şirket içi Etkin dizininizdeki parolaları eşitlüyorsanız, [Etkin Dizin belgelerindeki](../active-directory-domain-services/active-directory-ds-getting-started-password-sync-synced-tenant.md)adımları izleyin.

6.  Azure AD Etki Alanı Hizmetleri yönetilen bir etki [alanı için Güvenli LDAP'yi (LDAPS) yapılandırmada](../active-directory-domain-services/tutorial-configure-ldaps.md)açıklandığı şekilde Azure Active Directory Etki Alanı Hizmetlerinizde güvenli LDAP'yi yapılandırın.
    1. Azure konusunda açıklandığı gibi güvenli LDAP tarafından kullanılmak üzere bir sertifika yükleyin [güvenli LDAP için bir sertifika edinin.](../active-directory-domain-services/tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap)  CloudSimple, vCenter'ın sertifikaya güvenebileceğinden emin olmak için sertifika yetkilisi tarafından verilen imzalı bir sertifika kullanmanızı önerir.
    2. Azure AD Etki [Alanı Hizmetleri yönetilen etki alanı için güvenli LDAP'yi (LDAPS) etkinleştirme](../active-directory-domain-services/tutorial-configure-ldaps.md)açıklandığı gibi güvenli LDAP'yi etkinleştirin.
    3. Kimlik kaynağını yapılandırırken vCenter ile kullanılmak üzere sertifikanın ortak kısmını (özel anahtar olmadan) .cer formatında kaydedin.
    4. Azure AD etki alanı hizmetlerine Internet erişimi gerekiyorsa, 'Internet üzerinden LDAP'ye güvenli erişime izin verin' seçeneğini etkinleştirin.
    5. TCP bağlantı noktası için Azure AD Etki Alanı hizmetleri NSG için gelen güvenlik kuralını ekleyin 636.

## <a name="set-up-an-identity-source-on-your-private-cloud-vcenter"></a>Özel Bulut vCenter'ınızda bir kimlik kaynağı ayarlama

1. Private Cloud vCenter'ınız için [ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)
2. Kimlik kaynağının ayarlanması için gereken yapılandırma parametrelerini toplayın.

    | **Seçeneği** | **Açıklama** |
    |------------|-----------------|
    | **Adı** | Kimlik kaynağının adı. |
    | **Kullanıcılar için Temel DN** | Kullanıcılar için temel ayırt edici ad.  Azure AD için `OU=AADDC Users,DC=<domain>,DC=<domain suffix>` şu `OU=AADDC Users,DC=cloudsimplecustomer,DC=com`leri kullanın: Örnek: .|
    | **Etki alanı adı** | Örneğin etki alanının FQDN'si example.com. Bu metin kutusunda bir IP adresi sağlamayın. |
    | **Etki alanı takma adı** | *(isteğe bağlı)* Etki alanı NetBIOS adı. SSPI kimlik doğrulamalarını kullanıyorsanız, Active Directory etki alanının NetBIOS adını kimlik kaynağının diğer adı olarak ekleyin. |
    | **Gruplar için Temel DN** | Gruplar için taban ayırt adı. Azure AD için `OU=AADDC Users,DC=<domain>,DC=<domain suffix>` şu leri kullanın: Örnek:`OU=AADDC Users,DC=cloudsimplecustomer,DC=com`|
    | **Birincil Sunucu URL'si** | Etki alanı için birincil etki alanı denetleyiciLDAP sunucusu.<br><br>Biçimi `ldaps://hostname:port`kullanın. Bağlantı noktası, LDAPS bağlantıları için genellikle 636'dır. <br><br>Birincil veya ikincil LDAP URL'sinde kullandığınızda `ldaps://` Active Directory sunucusunun LDAPS bitiş noktası için güven oluşturan bir sertifika gereklidir. |
    | **İkincil sunucu URL'si** | Failover için kullanılan ikincil bir etki alanı denetleyicildap sunucusunun adresi. |
    | **Sertifika seçin** | Active Directory LDAP Server veya OpenLDAP Server kimlik kaynağınızla LDAPS kullanmak istiyorsanız, URL `ldaps://` metin kutusuna yazdıktan sonra sertifika seç düğmesi görüntülenir. İkincil bir URL gerekli değildir. |
    | **Username** | Etki alanında, kullanıcılar ve gruplar için Base DN'ye en az salt okunur erişimi olan bir kullanıcının kimliği. |
    | **Parola** | Kullanıcı adı ile belirtilen kullanıcının şifresi. |

3. Ayrıcalıklar arttıktan sonra Özel Bulut vCenter'ınızda oturum açın.
4. Azure Active Directory'yi kimlik kaynağı olarak ayarlamak için önceki adımdaki değerleri kullanarak [vCenter'daki bir kimlik kaynağı ekle](set-vcenter-identity.md#add-an-identity-source-on-vcenter) yönergeleriizleyin.
5. VMware konusunda açıklandığı gibi Azure AD'den kullanıcıları/grupları vCenter gruplarına [ekleyin Üyeyi vCenter Tek Oturum Açma Grubuna ekleyin.](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.security.doc/GUID-CDEA6F32-7581-4615-8572-E0B44C11D80D.html)

> [!CAUTION]
> Yeni kullanıcılar yalnızca *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group,* *Cloud-Global-Network-Admin-Group* veya *Cloud-Global-VM-Admin-Group'a*eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılır.  *Yöneticiler* grubuna yalnızca hizmet hesapları eklenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Bulut izni modeli hakkında bilgi edinin](learn-private-cloud-permissions.md)
