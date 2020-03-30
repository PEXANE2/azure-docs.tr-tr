---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulut'ta vCenter kimlik kaynaklarını ayarlama
description: VMware yöneticilerinin vCenter'a erişmeleri için Active Directory ile kimlik doğrulaması yapacak Özel Bulut vCenter'ınızı nasıl ayarlayabilirsiniz
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5355e43ca6ac075e76a76ceb51be135cf4b62b0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77564032"
---
# <a name="set-up-vcenter-identity-sources-to-use-active-directory"></a>Active Directory'yi kullanmak için vCenter kimlik kaynaklarını ayarlama

## <a name="about-vmware-vcenter-identity-sources"></a>VMware vCenter kimlik kaynakları hakkında

VMware vCenter, vCenter'a erişen kullanıcıların kimlik doğrulamaları için farklı kimlik kaynaklarını destekler.  CloudSimple Private Cloud vCenter'ınız, VMware yöneticilerinizin vCenter'a erişebildiği Active Directory ile kimlik doğrulaması yapacak şekilde ayarlanabilir. Kurulum tamamlandığında, bulut **sahibi** kullanıcı kimlik kaynağından vCenter'a kullanıcıları ekleyebilir.  

Active Directory etki alanı nızı ve etki alanı denetleyicilerinizi aşağıdaki yollardan herhangi birinde ayarlayabilirsiniz:

* Şirket içinde çalışan Etkin Dizin etki alanı ve etki alanı denetleyicileri
* Azure aboneliğinizde sanal makine olarak Azure'da çalışan Etkin Dizin etki alanı ve etki alanı denetleyicileri
* Özel Bulut'unuzda çalışan yeni Active Directory etki alanı ve etki alanı denetleyicileri
* Azure Active Directory hizmeti

Bu kılavuzda, aboneliklerinizde şirket içinde veya sanal makine olarak çalışan Active Directory etki alanı ve etki alanı denetleyicilerini ayarlama görevleri açıklanmaktadır.  Kimlik kaynağı olarak Azure AD'yi kullanmak istiyorsanız, kimlik kaynağını ayarlamayla ilgili ayrıntılı talimatlar için [CloudSimple Private Cloud'daki vCenter için kimlik sağlayıcısı olarak Azure AD'yi kullanın'a](azure-ad.md) bakın.

[Kimlik kaynağı eklemeden](#add-an-identity-source-on-vcenter) [önce, vCenter ayrıcalıklarınızı](escalate-private-cloud-privileges.md)geçici olarak artırın.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group,* *Cloud-Global-Network-Admin-Group* veya *Cloud-Global-VM-Admin-Group'a*eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılır.  *Yöneticiler* grubuna yalnızca hizmet hesapları eklenmelidir ve hizmet hesapları vSphere web UI'da oturum açmak için kullanılmamalıdır.   


## <a name="identity-source-options"></a>Kimlik kaynağı seçenekleri

* [Tek oturum açma kimlik kaynağı olarak şirket içi Active Directory ekleme](#add-on-premises-active-directory-as-a-single-sign-on-identity-source)
* [Özel Bulutta Yeni Active Directory'yi Ayarlama](#set-up-new-active-directory-on-a-private-cloud)
* [Azure'da Etkin Dizini Ayarlama](#set-up-active-directory-on-azure)

## <a name="add-on-premises-active-directory-as-a-single-sign-on-identity-source"></a>Tek İşaretli Kimlik Kaynağı Olarak Şirket Içi Etkin Dizini Ekle

Şirket içi Active Directory'nizi Tek Oturum Açma kimlik kaynağı olarak ayarlamak için şunları yapmanız gerekir:

* Şirket içi veri merkezinizden Özel Bulut'unuza [site-to-Site VPN bağlantısı.](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)
* Şirket içi DNS sunucu IP'si vCenter ve Platform Services Controller'a (PSC) eklendi.

Active Directory etki alanınızı ayarlarken aşağıdaki tablodaki bilgileri kullanın.

| **Seçeneği** | **Açıklama** |
|------------|-----------------|
| **Adı** | Kimlik kaynağının adı. |
| **Kullanıcılar için Temel DN** | Kullanıcılar için temel ayırt edici ad. |
| **Etki alanı adı** | Örneğin etki alanının FQDN'si example.com. Bu metin kutusunda bir IP adresi sağlamayın. |
| **Etki alanı takma adı** | Etki alanı NetBIOS adı. SSPI kimlik doğrulamalarını kullanıyorsanız, Active Directory etki alanının NetBIOS adını kimlik kaynağının diğer adı olarak ekleyin. |
| **Gruplar için Temel DN** | Gruplar için taban ayırt adı. |
| **Birincil Sunucu URL'si** | Etki alanı için birincil etki alanı denetleyiciLDAP sunucusu.<br><br>Biçimi `ldap://hostname:port` kullanın `ldaps://hostname:port`veya . Bağlantı noktası genellikle LDAP bağlantıları için 389 ve LDAPS bağlantıları için 636'dır. Active Directory çok etki alanı denetleyicisi dağıtımları için bağlantı noktası genellikle LDAP için 3268 ve LDAPS için 3269'dur.<br><br>Birincil veya ikincil LDAP URL'sinde kullandığınızda `ldaps://` Active Directory sunucusunun LDAPS bitiş noktası için güven oluşturan bir sertifika gereklidir. |
| **İkincil sunucu URL'si** | Failover için kullanılan ikincil bir etki alanı denetleyicildap sunucusunun adresi. |
| **Sertifika seçin** | Active Directory LDAP Server veya OpenLDAP Server kimlik kaynağınızla LDAPS kullanmak istiyorsanız, URL `ldaps://` metin kutusuna yazdıktan sonra sertifika seç düğmesi görüntülenir. İkincil bir URL gerekli değildir. |
| **Username** | Etki alanında, kullanıcılar ve gruplar için Base DN'ye en az salt okunur erişimi olan bir kullanıcının kimliği. |
| **Parola** | Kullanıcı adı ile belirtilen kullanıcının şifresi. |

Önceki tabloda bilgilere sahip olduğunuzda, şirket içi Active Directory'nizi vCenter'da Tek Oturum Açma kimlik kaynağı olarak ekleyebilirsiniz.

> [!TIP]
> [VMware dokümantasyon sayfasında](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.psc.doc/GUID-B23B1360-8838-4FF2-B074-71643C4CB040.html)Tek Oturum Açma kimlik kaynakları hakkında daha fazla bilgi bulabilirsiniz.

## <a name="set-up-new-active-directory-on-a-private-cloud"></a>Özel Bulutta Yeni Active Directory'yi Ayarlama

Özel Bulut'unuzda yeni bir Active Directory etki alanı ayarlayabilir ve tek oturum açma için kimlik kaynağı olarak kullanabilirsiniz.  Etkin Dizin etki alanı, varolan bir Active Directory ormanın bir parçası olabilir veya bağımsız bir orman olarak ayarlanabilir.

### <a name="new-active-directory-forest-and-domain"></a>Yeni Active Directory orman ve etki alanı

Yeni bir Active Directory ormanı ve etki alanı ayarlamak için şunları yapmanız gerekir:

* Microsoft Windows Server'ı çalıştıran bir veya daha fazla sanal makine, yeni Active Directory ormanı ve etki alanı için etki alanı denetleyicileri olarak kullanılır.
* Ad çözümlemesi için DNS hizmetini çalıştıran bir veya daha fazla sanal makine.

Ayrıntılı adımlar için [Yeni Windows Server 2012 Active Directory Forest yükle'ye](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-forest--level-200-) bakın.

> [!TIP]
> Hizmetlerin yüksek kullanılabilirliği için, birden çok etki alanı denetleyicisi ve DNS sunucusu ayarlamanızı öneririz.

Active Directory ormanını ve etki alanını kurduktan sonra, yeni Active Directory'niz için [vCenter'a bir kimlik kaynağı ekleyebilirsiniz.](#add-an-identity-source-on-vcenter)

### <a name="new-active-directory-domain-in-an-existing-active-directory-forest"></a>Varolan bir Active Directory ormanında yeni Active Directory etki alanı

Varolan bir Active Directory ormanında yeni bir Active Directory etki alanı ayarlamak için şunları yapmanız gerekir:

* Etkin Dizin orman konumunuza Siteden Siteye VPN bağlantısı.
* DNS Server, varolan Active Directory ormanınızın adını çözümlemek için.

Bkz. Ayrıntılı adımlar için [yeni bir Windows Server 2012 Active Directory alt veya ağaç etki alanı yükleyin.](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-child-or-tree-domain--level-200-)

Active Directory etki alanını kurduktan sonra, [vCenter'a](#add-an-identity-source-on-vcenter) yeni Active Directory için bir kimlik kaynağı ekleyebilirsiniz.

## <a name="set-up-active-directory-on-azure"></a>Azure'da Etkin Dizini Ayarlama

Azure'da çalışan Etkin Dizin, şirket içinde çalışan Active Directory'ye benzer.  VCenter'da Azure'da Çalışan Active Directory'yi tek oturum açma kimlik kaynağı olarak ayarlamak için vCenter sunucusu ve PSC'nin Active Directory hizmetlerinin çalıştığı Azure Sanal Ağı'na ağ bağlantısı olması gerekir.  Active Directory Services'in CloudSimple Private Cloud'da çalıştığı Azure sanal ağındaki [ExpressRoute'u kullanarak](azure-expressroute-connection.md) bu bağlantıyı Oluşturabilirsiniz.

Ağ bağlantısı kurulduktan sonra, Kimlik Kaynağı olarak eklemek için [Tek Oturum Açma Kimlik Kaynağı olarak Şirket Içi Etkin Dizini Ekle'deki](#add-on-premises-active-directory-as-a-single-sign-on-identity-source) adımları izleyin.  

## <a name="add-an-identity-source-on-vcenter"></a>vCenter'da kimlik kaynağı ekleme

1. Özel Bulut'unuzdaki [ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)

2. Özel Bulut'unuzun vCenter'ında oturum açın.

3. **Ev > İdaresi'ni**seçin.

    ![Yönetim](media/OnPremAD01.png)

4. **> Yapılandırmada Tek İşaret'i**seçin.

    ![Çoklu Oturum Açma](media/OnPremAD02.png)

5. Kimlik **Kaynakları** sekmesini **+** açın ve yeni bir kimlik kaynağı eklemek için tıklatın.

    ![Kimlik Kaynakları](media/OnPremAD03.png)

6. **LDAP Sunucusu Olarak Active Directory'yi** seçin ve **İleri'yi**tıklatın.

    ![Active Directory](media/OnPremAD04.png)

7. Ortamınız için kimlik kaynağı parametrelerini belirtin ve **İleri'yi**tıklatın.

    ![Active Directory](media/OnPremAD05.png)

8. Ayarları gözden geçirin ve **Bitir'e**tıklayın.
