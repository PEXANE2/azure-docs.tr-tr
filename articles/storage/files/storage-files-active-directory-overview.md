---
title: Genel Bakış - Azure Files kimlik tabanlı yetkilendirme
description: Azure Dosyaları, Azure Active Directory Etki Alanı Hizmetleri (AD DS) ve Active Directory aracılığıyla SMB (Sunucu İleti Bloğu) üzerinden kimlik tabanlı kimlik doğrulamayı destekler. Etki alanına katılan Windows sanal makineleriniz (VM'ler), Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: rogarana
ms.openlocfilehash: 7d9f8ccb4273d1378c4826dea420c4edca2f8ac3
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536598"
---
# <a name="overview-of-azure-files-identity-based-authentication-support-for-smb-access"></a>Kobİ erişimi için Azure Dosyaları kimlik tabanlı kimlik doğrulama desteğine genel bakış
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Azure dosya paylaşımları için şirket içi Active Directory Etki Alanı Hizmetleri kimlik doğrulamasını nasıl etkinleştireceklerini öğrenmek için (önizleme), azure [dosya paylaşımları için Kobİ üzerinden şirket içi Active Directory Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştir](storage-files-identity-auth-active-directory-enable.md)mecede bakın.

Azure dosya paylaşımları için Azure AD DS kimlik doğrulamasını nasıl etkinleştirin, [bkz.](storage-files-identity-auth-active-directory-domain-service-enable.md)

## <a name="glossary"></a>Sözlük 
Azure dosya paylaşımları için SMB üzerinden Azure AD Etki Alanı Hizmeti kimlik doğrulaması ile ilgili bazı önemli terimleri anlamak yararlıdır:

-   **Kerberos kimlik doğrulaması**

    Kerberos, bir kullanıcının veya ana bilgisayarın kimliğini doğrulamak için kullanılan bir kimlik doğrulama protokolüdür. Kerberos hakkında daha fazla bilgi için, [Bkz. Kerberos Kimlik Doğrulama Genel Bakış.](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-authentication-overview)

-  **Sunucu İleti Bloğu (SMB) protokolü**

    SMB endüstri standardı ağ dosya paylaşım protokolüdür. SMB, Ortak İnternet Dosya Sistemi veya CIFS olarak da bilinir. Kobİ hakkında daha fazla bilgi için [Microsoft Kobİ Protokolü ve CIFS Protokolüne Genel Bakış'a](https://docs.microsoft.com/windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview)bakın.

-   **Azure Etkin Dizin (Azure AD)**

    Azure Active Directory (Azure AD), Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure AD, temel dizin hizmetlerini, uygulama erişim yönetimini ve kimlik korumayı tek bir çözümde birleştirir. Azure AD'ye katılan Windows sanal makineleri (VM'ler), Azure REKLAM kimlik bilgilerinizle Azure dosya paylaşımlarına erişebilir. Daha fazla bilgi için Azure [Etkin Dizin nedir?](../../active-directory/fundamentals/active-directory-whatis.md)

-   **Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS)**

    Azure AD DS, etki alanı birleştirme, grup ilkeleri, LDAP ve Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Bu hizmetler Active Directory Domain Services ile tamamen uyumludur. Daha fazla bilgi için Azure [Etkin Dizin Etki Alanı Hizmetleri'ne](../../active-directory-domain-services/overview.md)bakın.

- **Şirket Içi Aktif Dizin Etki Alanı Hizmetleri (AD DS)**

    Şirket içi Active Directory Domain Services (AD DS) Azure Dosyaları (önizleme) ile tümleştirme, dizin verilerini depolama yöntemlerini sağlarken ağ kullanıcıları ve yöneticileri için kullanılabilir hale getirir. Güvenlik, oturum açma kimlik doğrulaması ve dizindeki nesnelere erişim denetimi yoluyla AD DS ile tümleştirilir. Yöneticiler, tek bir ağ oturumu yla dizin verilerini ve kuruluşlarını kendi ağları boyunca yönetebilir ve yetkili ağ kullanıcıları ağdaki her yerdeki kaynaklara erişebilir. AD DS genellikle şirket içi ortamlarda işletmeler tarafından benimsenen ve AD DS kimlik bilgileri erişim denetimi için kimlik olarak kullanılır. Daha fazla bilgi için [Active Directory Domain Services Genel Bakış'a](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)bakın.

-   **Azure Role Tabanlı Erişim Denetimi (RBAC)**

    Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC'ı kullanarak, kullanıcılara işlerini gerçekleştirmek için gereken en az izin vererek kaynaklara erişimi yönetebilirsiniz. RBAC hakkında daha fazla bilgi için [Azure'da rol tabanlı erişim denetimi (RBAC) nedir?](../../role-based-access-control/overview.md)

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Azure Dosyalarındaki Windows ALA'ları için kimlik tabanlı kimlik doğrulama ve destek, aşağıdaki kullanım örnekleri için en iyi şekilde yararlanılır:

### <a name="replace-on-premises-file-servers"></a>Şirket içi dosya sunucularını değiştirme

Dağınık şirket içi dosya sunucularının amortismana sokulmaması ve değiştirilmesi, her işletmenin BT modernizasyon yolculuğunda karşılaştığı yaygın bir sorundur. Azure dosya syon şirket içi AD DS (önizleme) kimlik doğrulaması ile paylaşır, verileri Azure Dosyalarına geçirebileceğiniz de en uygun şeydir. Tam bir geçiş, istemci tarafındaki değişiklikleri en aza indirirken yüksek kullanılabilirlik ve ölçeklenebilirlik avantajlarından yararlanmanızı sağlar. Son kullanıcılara sorunsuz bir geçiş deneyimi sağlar, böylece varolan etki alanı birleştirilmiş makineleri kullanarak aynı kimlik bilgileriyle verilerine erişmeye devam edebilirler.

### <a name="lift-and-shift-applications-to-azure"></a>Uygulamaları Azure'a taşıyın ve kaydırın

Uygulamaları kaldırıp buluta kaydırdığınızda, verileriniz için aynı kimlik doğrulama modelini tutmak istersiniz. Kimlik tabanlı erişim denetimi deneyimini Azure dosya paylaşımlarına genişletirken, uygulamanızı modern auth yöntemleriyle değiştirme ve bulut benimsenmesini hızlandırma gereksinimini ortadan kaldırır. Azure dosya paylaşımları, kimlik doğrulaması için Azure AD DS veya şirket içi AD DS (önizleme) ile tümleştirme seçeneği sağlar. Planınız %100 bulut ait olacaksa ve bulut altyapılarını yönetme çabalarını en aza indirecekse, Azure AD DS tam olarak yönetilen bir etki alanı hizmeti olarak daha uygun olacaktır. AD DS özellikleriyle tam uyumluluğa ihtiyacınız varsa, AD DS ortamınızı VM'lerde kendi kendine barındıran etki alanı denetleyicileri tarafından buluta genişletmeyi düşünebilirsiniz. Her iki şekilde de, işletme gereksinimlerinize uygun etki alanı hizmetlerini seçme esnekliği sağlarız.

### <a name="backup-and-disaster-recovery-dr"></a>Yedekleme ve olağanüstü durum kurtarma (DR)

Birincil dosya depolama alanınızı şirket içinde tutuyorsanız, Azure dosya paylaşımları iş sürekliliğini artırmak için yedekleme veya DR için ideal bir depolama alanı görevi görebilir. Windows DACL'leri korurken, mevcut dosya sunucularından verilerinizi yedeklemek için Azure dosya paylaşımlarını kullanabilirsiniz. DR senaryoları için, başarısız olduğu anda uygun erişim denetimi zorlamayı desteklemek için bir kimlik doğrulama seçeneği yapılandırabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki tablo, desteklenen Azure dosyasını Azure AD DS ve şirket içi AD DS (önizleme) için kimlik doğrulama senaryolarını paylaşır. Azure Dosyaları ile tümleştirme için istemci ortamınız için benimsediğiniz etki alanı hizmetini seçmenizi öneririz. AD DS (önizleme) zaten şirket içinde veya aygıtlarınızın ad'ınıza etki alanının birleştiği Azure'da kurulumunuz varsa, Azure dosya paylaşımları kimlik doğrulaması için AD DS (önizleme) seçeneğini seçmelisiniz. Benzer şekilde, Azure AD DS'yi (GA) zaten benimsediyseniz, bunu Azure dosya paylaşımları kimlik doğrulaması için kullanmanız gerekir.


|Azure AD DS kimlik doğrulaması  | şirket içi AD DS (önizleme) kimlik doğrulaması  |
|---------|---------|
|Azure AD DS'ye katılan Windows makineleri, Azure REKLAM kimlik bilgileriyle Azure dosya paylaşımlarına Kobİ üzerinden erişebilir.     |Şirket içi AD DS'ye katılan Windows makineleri, SMB üzerinden Azure AD'ye senkronize edilen şirket içi Active Directory kimlik bilgileriyle Azure dosya paylaşımlarına erişebilir.         |

### <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

- Azure AD DS ve şirket içi AD DS kimlik doğrulaması bilgisayar hesaplarına karşı kimlik doğrulamayı desteklemez. Bunun yerine bir hizmet oturum açma hesabı kullanmayı düşünebilirsiniz.
- Azure AD DS kimlik doğrulaması, Azure AD'ye bağlı aygıtlara karşı kimlik doğrulamayı desteklemez.

## <a name="advantages-of-identity-based-authentication"></a>Kimlik tabanlı kimlik doğrulamanın avantajları
Azure Dosyaları için kimlik tabanlı kimlik doğrulama, Paylaşılan Anahtar kimlik doğrulamasını kullanmanın çeşitli avantajları sunar:

-   **Şirket içi AD DS ve Azure AD DS ile geleneksel kimlik tabanlı dosya paylaşımı erişim deneyimini buluta genişletin**  
    Geleneksel dosya sunucularını Azure dosya paylaşımlarıyla değiştirerek uygulamanızı kaldırmayı ve buluta kaydırmayı planlıyorsanız, dosya verilerine erişmek için uygulamanızın şirket içinde AD DS veya Azure AD DS kimlik bilgileriyle kimlik doğrulamasını isteyebilirsiniz. Azure Dosyaları, şirket içi AD DS veya Azure AD DS alan adı birleştirilmiş VM'lerden Kobİ üzerinden Azure dosya paylaşımlarına erişmek için hem şirket içi AD DS hem de Azure AD DS kimlik bilgilerini kullanmayı destekler.

-   **Azure dosya paylaşımlarında parçalı erişim denetimini zorlama**  
    Paylaşım, dizin veya dosya düzeyinde belirli bir kimliğe izin verebilirsiniz. Örneğin, proje işbirliği için tek bir Azure dosya paylaşımını kullanan birkaç takımın olduğunu varsayalım. Tüm takımlara hassas olmayan dizinlere erişim izni verirken, hassas finansal verileri içeren dizinlere erişimi yalnızca Finans ekibinizle sınırlandırabilirsiniz. 

-   **Verilerinizle birlikte Windows ALA'larını (NTFS olarak da bilinir) yedekleme**  
    Mevcut şirket içi dosya paylaşımlarınızı yedeklemek için Azure dosya paylaşımlarını kullanabilirsiniz. Azure Dosyaları, bir dosya paylaşımını SMB üzerinden Azure dosya paylaşımlarına yedeklediğinizde, aPARA'larınızı ve verilerinizle birlikte korur.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure dosya paylaşımları, Azure AD DS veya şirket içi AD DS (önizleme) ile tümleştirme için Kerberos kimlik doğrulamasını destekler. Azure dosya paylaşımlarında kimlik doğrulamasını etkinleştirmeden önce etki alanı ortamınızı ayarlamanız gerekir. Azure AD DS kimlik doğrulaması için Azure AD Etki Alanı Hizmetlerini ve alan adı, dosya verilerine erişmesini planladığınız Sanal Verilere katılmayı etkinleştirmelisiniz. Etki alanına katılan VM'niz, Azure AD DS'inizle aynı sanal ağda (VNET) ikamet etmelidir. Benzer şekilde, şirket içi AD DS (önizleme) kimlik doğrulaması için etki alanı denetleyicinizi ayarlamanız ve etki alanımakinelerinize veya VM'lerinize katılmanız gerekir.

VM'de çalışan bir uygulamayla ilişkili bir kimlik Azure dosya paylaşımlarında verilere erişmeye çalıştığında, istek kimliği doğrulamak için Azure AD DS'ye gönderilir. Kimlik doğrulama başarılı olursa, Azure AD DS bir Kerberos belirteci döndürür. Uygulama, Kerberos belirteci içeren bir istek gönderir ve Azure dosya paylaşımları isteği yetkilendirmek için bu belirteci kullanır. Azure dosya paylaşımları yalnızca belirteç alır ve Azure AD DS kimlik bilgilerini kalıcı olarak sağlamaz. Şirket içi AD DS kimlik doğrulaması, AD DS'nizin Kerberos belirteci sağladığı benzer bir şekilde çalışır.

![Azure AD kimlik doğrulamadiyagramını Kobİ üzerinden gösteren ekran görüntüsü](media/storage-files-active-directory-overview/azure-active-directory-over-smb-for-files-overview.png)

### <a name="enable-identity-based-authentication"></a>Kimlik tabanlı kimlik doğrulamayı etkinleştirme

Yeni ve varolan depolama hesaplarınızdaki Azure dosya paylaşımları için Azure AD DS veya şirket içi AD DS (önizleme) ile kimlik tabanlı kimlik doğrulamayı etkinleştirebilirsiniz. Depo hesabındaki dosya erişimi kimlik doğrulaması için yalnızca bir etki alanı hizmeti kullanılabilir ve bu hizmet hesaptaki tüm dosya paylaşımları için geçerlidir. Makalemizde Azure REKLAM DS ile dosya paylaşımlarınızı kimlik doğrulama için ayarlamaya ilişkin ayrıntılı kılavuz Azure [Dosyaları'nda Azure Active Directory Etki Alanı Hizmetlerini etkinleştirme](storage-files-identity-auth-active-directory-domain-service-enable.md) ve diğer makalemizde şirket içi AD DS (önizleme) için kılavuz, [Azure dosya paylaşımları için SMB üzerinden Etkin Dizin Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştirin.](storage-files-identity-auth-active-directory-enable.md)

### <a name="configure-share-level-permissions-for-azure-files"></a>Azure Dosyaları için paylaşım düzeyi izinlerini yapılandırma

Azure AD DS veya şirket içi AD DS (önizleme) kimlik doğrulaması etkinleştirildiğinde, yerleşik RBAC rollerini kullanabilir veya Azure AD kimlikleri için özel roller yapabilir ve depolama hesaplarınızdaki tüm dosya paylaşımlarına erişim hakları atayabilirsiniz. Atanan izin, verilen kimliğin yalnızca, başka bir şeye, kök dizine bile erişmesine izin verir. Azure dosya paylaşımları için dizin veya dosya düzeyinde izinleri ayrı ayrı yapılandırmanız gerekir.

### <a name="configure-directory-or-file-level-permissions-for-azure-files"></a>Azure Dosyaları için dizin veya dosya düzeyinde izinleri yapılandırma

Azure dosya paylaşımları, kök dizini de dahil olmak üzere hem dizin hem de dosya düzeyinde standart Windows dosya izinlerini zorlar. Dizin veya dosya düzeyindeizinyapılandırması hem SMB hem de REST üzerinden desteklenir. Hedef dosya paylaşımını VM'nizden bağla ve Windows File Explorer, Windows [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls)veya [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl?view=powershell-6) komutunu kullanarak izinleri yapılandırın.

### <a name="use-the-storage-account-key-for-superuser-permissions"></a>Süper kullanıcı izinleri için depolama hesabı anahtarını kullanma

Depolama hesabı anahtarına sahip bir kullanıcı, üst kullanıcı izniyle Azure dosya paylaşımlarına erişebilir. Superuser izinleri tüm erişim denetimi kısıtlamalarını atlar.

> [!IMPORTANT]
> Önerilen en iyi güvenlik uygulamamız, depolama hesabı anahtarlarınızı paylaşmaktan kaçınmak ve mümkün olduğunca kimlik tabanlı kimlik doğrulamadan yararlanmaktır.

### <a name="preserve-directory-and-file-acls-when-importing-data-to-azure-file-shares"></a>Azure dosya paylaşımlarına veri aktarırken dizin ve dosya Aç'larını koruyun

Azure Dosyaları, verileri Azure dosya paylaşımlarına kopyalarken dizin veya dosya düzeyinde KISA'ların korunmasını destekler. Bir dizin veya dosyadaki ALA'ları Azure Dosya Eşitleme veya yaygın dosya hareketi araç kümelerini kullanarak Azure dosya paylaşımlarına kopyalayabilirsiniz. Örneğin, verileri ve ALA'ları Azure dosya paylaşımına kopyalamak için `/copy:s` [bayraklı robocopy'yi](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) kullanabilirsiniz. ACD'ler varsayılan olarak korunur, ACD'leri korumak için depolama hesabınızda kimlik tabanlı kimlik doğrulamayı etkinleştirmeniz gerekmez.

## <a name="pricing"></a>Fiyatlandırma
Depolama hesabınızda SPK üzerinden kimlik tabanlı kimlik doğrulamayı etkinleştirmek için ek bir hizmet ücreti yoktur. Fiyatlandırma hakkında daha fazla bilgi için [Azure Dosyaları fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/files/) ve [Azure AD Etki Alanı Hizmetleri fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-ds/)hakkında bilgi alın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları ve Kobİ üzerinden kimlik doğrulama hakkında daha fazla bilgi için şu kaynaklara bakın:

- [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)
- [Azure dosya paylaşımları için Kobİ üzerinden şirket içi Active Directory Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-enable.md)
- [Azure Dosyalarında Azure Etkin Dizin Etki Alanı Hizmetleri kimlik doğrulamasını etkinleştirme](storage-files-identity-auth-active-directory-domain-service-enable.md)
- [SSS](storage-files-faq.md)
