---
title: Genel Bakış-Azure dosyaları kimlik tabanlı yetkilendirme
description: Azure dosyaları, SMB (sunucu Ileti bloğu) üzerinden Azure Active Directory Domain Services (AD DS) ve Active Directory arasında kimlik tabanlı kimlik doğrulamasını destekler. Etki alanına katılmış Windows sanal makineleriniz (VM), Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/21/2020
ms.author: rogarana
ms.openlocfilehash: 673bf3be59d72b2cc62b9f92af353fee235c5ddc
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77598825"
---
# <a name="overview-of-azure-files-identity-based-authentication-support-for-smb-access"></a>Azure dosyalarına genel bakış SMB erişimi için kimlik tabanlı kimlik doğrulama desteği
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosya paylaşımları için AD kimlik doğrulamasını etkinleştirme hakkında bilgi edinmek için bkz. [Azure dosya paylaşımları IÇIN SMB üzerinde Active Directory kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-enable.md).

Azure dosya paylaşımları için Azure AD DS kimlik doğrulamasını etkinleştirme hakkında bilgi edinmek için bkz. [Azure dosyalarında Azure Active Directory Domain Services kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-domain-service-enable.md).

## <a name="glossary"></a>Sözlük 
Azure dosya paylaşımları için SMB üzerinden Azure AD etki alanı hizmet kimlik doğrulamasıyla ilgili bazı önemli koşulları anlamak yararlı olabilir:

-   **Kerberos kimlik doğrulaması**

    Kerberos, bir kullanıcının veya ana bilgisayarın kimliğini doğrulamak için kullanılan bir kimlik doğrulama protokolüdür. Kerberos hakkında daha fazla bilgi için bkz. [Kerberos kimlik doğrulamasına genel bakış](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview).

-  **Sunucu Ileti bloğu (SMB) protokolü**

    SMB, sektör standardı bir ağ dosya paylaşım protokolüdür. SMB, ortak Internet dosya sistemi veya CIFS olarak da bilinir. SMB hakkında daha fazla bilgi için bkz. [MICROSOFT SMB protokolü ve CIFS protokolüne genel bakış](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview).

-   **Azure Active Directory (Azure AD)**

    Azure Active Directory (Azure AD), Microsoft 'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD temel Dizin Hizmetleri, uygulama erişim yönetimi ve kimlik korumasını tek bir çözümde birleştirir. Azure AD, etki alanına katılmış Windows sanal makinelerinizin (VM 'Ler) Azure AD kimlik bilgilerinizle Azure dosya paylaşımlarına erişmesine olanak sağlar. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../../active-directory/fundamentals/active-directory-whatis.md)

-   **Azure AD Domain Services (Azure AD DS)**

    Azure AD Domain Services (GA), etki alanına katılması, Grup ilkeleri, LDAP ve Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı Hizmetleri sağlar. Bu hizmetler Windows Server Active Directory ile tamamen uyumludur. Daha fazla bilgi için bkz. [Azure Active Directory (ad) etki alanı Hizmetleri](../../active-directory-domain-services/overview.md).

- **Active Directory Domain Services (AD DS, AD olarak da adlandırılır)**

    Active Directory (AD) (Önizleme), ağ kullanıcıları ve yöneticiler tarafından kullanılabilir hale getirilmesi sırasında dizin verilerinin depolanması için yöntemler sağlar. Güvenlik, oturum açma kimlik doğrulaması ve dizindeki nesnelere erişim denetimi aracılığıyla Active Directory tümleşiktir. Tek bir ağ oturumu açma ile yöneticiler, ağ genelinde dizin verilerini ve organizasyonu yönetebilir ve yetkili ağ kullanıcıları ağ üzerinde herhangi bir yerde kaynaklara erişebilir. AD, genellikle şirket içinde kuruluşlar tarafından benimsenmiştir ve erişim denetimi için kimlik kimlik bilgilerini kullanır. Daha fazla bilgi için bkz. [Active Directory Domain Services genel bakış](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

-   **Azure rol tabanlı Access Control (RBAC)**

    Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kullanıcılara işlerini gerçekleştirmek için gereken en az izni vererek kaynaklara erişimi yönetebilirsiniz. RBAC hakkında daha fazla bilgi için bkz. [Azure 'da rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md).

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Azure dosyalarında Windows ACL 'Leri için kimlik tabanlı kimlik doğrulaması ve destek aşağıdaki kullanım durumları için en iyi yararlanılabilir:

### <a name="replace-on-premises-file-servers"></a>Şirket içi dosya sunucularını değiştirme

Dağınık şirket içi dosya sunucularının kullanım dışı bırakılması ve değiştirilmesi, her kuruluşun BT çalışırken yolculuğunda karşılaştığı yaygın bir sorundur. Azure dosya paylaşımları AD (Önizleme) kimlik doğrulaması, verileri Azure dosyalarına geçirebileceğiniz en iyi şekilde buraya uygundur. Tamamlanmış bir geçiş, yüksek oranda kullanılabilirlik ve ölçeklenebilirlik avantajlarından yararlanmanızı sağlarken, özellikle karmaşık AD etki alanı altyapısını de en aza indirmenizi sağlar. Son kullanıcılara sorunsuz bir geçiş deneyimi sağlar. bu sayede, mevcut etki alanına katılmış makineleri kullanarak verilerine aynı kimlik bilgileriyle erişmeye devam edebilirler.

### <a name="lift-and-shift-applications-to-azure"></a>Uygulamaları Azure 'a yükselt ve Kaydır

Uygulamaları buluta "kaldırma ve kaydırma" yaptığınızda, verileriniz için aynı kimlik doğrulama modelini tutmak istersiniz. Kimlik tabanlı erişim denetimi deneyimini Azure dosya paylaşımlarına genişlettiğimiz için, uygulamanızı modern kimlik doğrulama yöntemlerine değiştirme ve bulut benimseme sürecini ortadan kaldırır. Azure dosya paylaşımları, kimlik doğrulaması için Azure AD DS (GA) veya AD (Önizleme) ile tümleştirme seçeneği sağlar. Planınız %100 bulut Native olarak kullanılıyorsa ve bulut altyapılarını yönetme çabalarını en aza indiriyorsa Azure AD DS tam olarak yönetilen bir etki alanı hizmeti olarak daha iyi bir şekilde uyum sağlar. AD DS (GA) özellikleri ile tam uyumlulukla karşılaşırsanız, VM 'lerde Self-hosting etki alanı denetleyicileri tarafından AD ortamınızı buluta genişletmeyi düşünmek isteyebilirsiniz. Her iki durumda da, iş gereksinimlerinize uygun etki alanı hizmetlerini seçme esnekliği sağlıyoruz.

### <a name="backup-and-disaster-recovery-dr"></a>Yedekleme ve olağanüstü durum kurtarma (DR)

Birincil dosya depolama alanınızı şirket içinde saklarken Azure dosya paylaşımları, iş devamlılığını artırmak için yedekleme veya DR için ideal bir depolama alanı olarak görev yapabilir. Azure dosya paylaşımlarını, verilerinizi mevcut dosya sunucularından yedeklemek için kullanabilirsiniz, ancak Windows DACL 'Lerini korurken. DR senaryolarında, yük devretme sırasında uygun erişim denetimi zorlamayı desteklemek için bir kimlik doğrulama seçeneği yapılandırabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki tabloda Azure AD DS (GA) ve AD (Önizleme) için desteklenen Azure dosya paylaşımları kimlik doğrulama senaryoları özetlenmektedir. Azure dosyaları ile tümleştirme için, istemci ortamınız için benimsediğiniz etki alanı hizmetini seçmenizi öneririz. Şirket içinde veya cihazlarınızın etki alanına katıldığı Azure 'da AD (Önizleme) zaten varsa, Azure dosya paylaşımları kimlik doğrulaması için AD (Önizleme) özelliğinden faydalanmayı seçmeniz gerekir. Benzer şekilde, Azure AD DS 'yi (GA) zaten benimsediyseniz Azure dosya paylaşımları kimlik doğrulaması için bunu kullanmanız gerekir.


|Azure AD DS (GA) kimlik doğrulaması  |AD (Önizleme) kimlik doğrulaması  |
|---------|---------|
|Azure AD DS etki alanına katılmış Windows makineleri, SMB üzerinden Azure AD kimlik bilgileriyle Azure dosya paylaşımlarına erişebilir.     |AD etki alanına katılmış Windows makineleri, SMB üzerinden Azure AD 'ye eşitleniyor olan AD kimlik bilgileriyle Azure dosya paylaşımlarına erişebilir.         |

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

- Azure AD DS (GA) ve AD (Önizleme) kimlik doğrulaması, bilgisayar hesaplarında kimlik doğrulamasını desteklemez. Bunun yerine bir hizmet oturum açma hesabı kullanmayı düşünebilirsiniz.
- Azure AD DS (GA) kimlik doğrulaması, Azure AD bulutu ile Birleşik cihazlara yönelik kimlik doğrulamasını desteklemez.

## <a name="advantages-of-identity-based-authentication"></a>Kimlik tabanlı kimlik doğrulamanın avantajları
Azure dosyaları için kimlik tabanlı kimlik doğrulaması, paylaşılan anahtar kimlik doğrulamasını kullanarak çeşitli avantajlar sunar:

-   **AD ve Azure AD DS ile geleneksel kimlik tabanlı dosya paylaşma erişim deneyimini buluta genişletin**  
    Uygulamanızı buluta "kaldırma ve kaydırma" planlıyorsanız, geleneksel dosya sunucularını Azure dosya paylaşımlarıyla değiştirerek, uygulamanızın dosya verilerine erişmek için AD veya Azure AD kimlik bilgileriyle kimlik doğrulaması yapmasını isteyebilirsiniz. Azure dosyaları, AD veya Azure AD DS etki alanına katılmış VM 'lerden SMB üzerinden Azure dosya paylaşımlarına erişmek için hem AD hem de Azure AD kimlik bilgilerinin kullanılmasını destekler.

-   **Azure dosya paylaşımlarında ayrıntılı erişim denetimi uygulayın**  
    Paylaşıma, dizine veya dosya düzeyinde belirli bir kimliğe izin verebilirsiniz. Örneğin, proje işbirliği için tek bir Azure dosya paylaşımının kullanıldığı birkaç ekibin olduğunu varsayalım. Tüm takımlara hassas olmayan dizinlere erişim izni verebilirsiniz, ancak gizli finansal verileri içeren dizinlere erişimi yalnızca finans ekibinize kısıtlar. 

-   **Windows ACL 'Lerini (NTFS olarak da bilinir) verilerinizin yanı sıra yedekleyin**  
    Azure dosya paylaşımlarını, mevcut şirket içi dosya paylaşımlarınızı yedeklemek için kullanabilirsiniz. Azure dosyaları, SMB üzerinden Azure dosya paylaşımlarına bir dosya paylaşımı yedeklediğiniz zaman, ACL 'lerinizi verilerle birlikte korur.

## <a name="how-it-works"></a>Nasıl çalışır?
Azure dosya paylaşımları, Azure AD DS (GA) veya AD (Önizleme) ile tümleştirme için Kerberos kimlik doğrulamasını destekler. Azure dosya paylaşımlarında kimlik doğrulamasını etkinleştirebilmeniz için önce etki alanı ortamınızı ayarlamanız gerekir. Azure AD DS (GA) kimlik doğrulaması için, Azure AD Domain Services ve etki alanına, dosya verilerine erişmeyi planladığınız sanal makinelere katılmayı etkinleştirmeniz gerekir. Etki alanına katılmış VM 'niz Azure AD Domain Services aynı sanal ağda (VNET) bulunmalıdır. Benzer şekilde, AD (Önizleme) kimlik doğrulaması için Active Directory etki alanı denetleyicinizi ve etki alanınıza, makinelerinize veya VM 'Lere katılmanız gerekir.

Bir VM üzerinde çalışan bir uygulamayla ilişkili bir kimlik, Azure dosya paylaşımlarında verilere erişmeye çalıştığında, kimlik doğrulaması için Azure AD Domain Services gönderilir. Kimlik doğrulaması başarılı olursa Azure AD Domain Services, Kerberos belirteci döndürür. Uygulama, Kerberos belirtecini içeren bir istek gönderir ve Azure dosya paylaşımları isteği yetkilendirmek için bu belirteci kullanır. Azure dosya paylaşımları yalnızca belirteci alır ve Azure AD kimlik bilgilerini kalıcı hale almaz. Ad kimlik doğrulaması, AD Kerberos belirtecini sağlayan benzer bir biçimde çalışmaktadır.

![SMB üzerinden Azure AD kimlik doğrulaması diyagramını gösteren ekran görüntüsü](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-identity-based-authentication"></a>Kimlik tabanlı kimlik doğrulamasını etkinleştir

Yeni ve mevcut depolama hesaplarınızdaki Azure dosya paylaşımları için Azure AD DS (GA) veya AD (Önizleme) ile kimlik tabanlı kimlik doğrulamasını etkinleştirebilirsiniz. Yalnızca bir etki alanı hizmeti, hesaptaki tüm dosya paylaşımları için geçerli olan depolama hesabında dosya erişimi kimlik doğrulaması için kullanılabilir. Azure AD DS (GA) ile kimlik doğrulaması için dosya paylaşımlarını ayarlamaya yönelik ayrıntılı adım adım yönergeler (GA) makalemize ilişkin [Azure dosyalarında kimlik doğrulamasını Azure Active Directory Domain Services etkinleştirme](storage-files-identity-auth-active-directory-domain-service-enable.md) ve ad (Önizleme) Kılavuzu ' nda Azure [dosya paylaşımları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-enable.md).

### <a name="configure-share-level-permissions-for-azure-files"></a>Azure dosyaları için paylaşma düzeyi izinleri yapılandırma

Azure AD DS (GA) veya AD (Önizleme) kimlik doğrulaması etkinleştirildikten sonra, yerleşik RBAC rollerini kullanabilir ya da Azure AD kimlikleri için özel rolleri yapılandırabilir ve depolama hesaplarınızdaki herhangi bir dosya paylaşımına erişim hakları atayabilirsiniz. atanan izin, izin verilen kimliğin, kök dizin bile değil, yalnızca paylaşıma erişim almasına izin verir. Azure dosya paylaşımları için dizin veya dosya düzeyi izinlerini yine de yapılandırmanız gerekir.

### <a name="configure-directory-or-file-level-permissions-for-azure-files"></a>Azure dosyaları için dizin veya dosya düzeyi izinlerini yapılandırma

Azure dosya paylaşımları, kök dizin dahil olmak üzere hem dizin hem de dosya düzeyinde standart Windows dosya izinlerini zorlar. Dizin veya dosya düzeyi izinlerinin yapılandırılması hem SMB hem de REST üzerinde desteklenir. Hedef dosya paylaşımından sanal makineden bağlama yapın ve Windows Dosya Gezgini, Windows [ıacl 'leri](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls)veya [set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl?view=powershell-6) komutunu kullanarak izinleri yapılandırın.

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Süper Kullanıcı izinleri için depolama hesabı anahtarını kullan

Depolama hesabı anahtarını taşıyan bir Kullanıcı, Azure dosya paylaşımlarına Süper Kullanıcı izinleriyle erişebilir. Süper Kullanıcı izinleri tüm erişim denetimi kısıtlamalarını atlar.

> [!IMPORTANT]
> Önerilen en iyi güvenlik uygulaması, depolama hesabı anahtarlarınızı paylaşmayı ve mümkün olan her durumda kimlik tabanlı kimlik doğrulamasından faydalamamaktır.

### <a name="preserve-directory-and-file-acls-when-importing-data-to-azure-file-shares"></a>Azure dosya paylaşımlarına veri aktarırken dizin ve dosya ACL 'Lerini koruma

Azure dosyaları, verileri Azure dosya paylaşımlarına kopyalarken dizin veya dosya düzeyi ACL 'Lerinin korunması destekler. Bir dizin veya dosyadaki ACL 'Leri, Azure Dosya Eşitleme veya ortak dosya taşıma araç kümelerinden birini kullanarak Azure dosya paylaşımlarına kopyalayabilirsiniz. Örneğin, bir Azure dosya paylaşımında veri ve ACL 'Leri kopyalamak için `/copy:s` bayrağıyla [Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) kullanabilirsiniz. ACL 'ler varsayılan olarak korunur, ACL 'Leri korumak için depolama hesabınızda kimlik tabanlı kimlik doğrulamasını etkinleştirmeniz gerekmez.

## <a name="pricing"></a>Fiyatlandırma
Depolama hesabınızda SMB üzerinden kimlik tabanlı kimlik doğrulamasını etkinleştirmek için başka bir hizmet ücreti yoktur. Fiyatlandırma hakkında daha fazla bilgi için bkz. AAD DS bilgilerini arıyorsanız [Azure dosyaları fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/files/) ve [Azure AD Domain Services fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-ds/) sayfaları.

## <a name="next-steps"></a>Sonraki adımlar
SMB üzerinden Azure dosyaları ve kimlik tabanlı kimlik doğrulaması hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
- [Azure dosya paylaşımları için SMB üzerinden Active Directory kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-enable.md)
- [Azure dosyalarında Azure Active Directory Domain Services kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-domain-service-enable.md)
- [SSS](storage-files-faq.md)
