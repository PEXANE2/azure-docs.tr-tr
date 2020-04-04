---
title: Azure AD Etki Alanı Hizmetlerini Klasik sanal ağdan geçirin | Microsoft Dokümanlar
description: Varolan bir Azure AD Etki Alanı Hizmetleri yönetilen etki alanı örneğini Klasik sanal ağ modelinden Kaynak Yöneticisi tabanlı sanal ağa nasıl geçirebilirsiniz öğrenin.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/22/2020
ms.author: iainfou
ms.openlocfilehash: 6acf9301367ae2c6947f6935c43f420d3d7cac65
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655003"
---
# <a name="migrate-azure-ad-domain-services-from-the-classic-virtual-network-model-to-resource-manager"></a>Azure AD Etki Alanı Hizmetlerini Klasik sanal ağ modelinden Kaynak Yöneticisi'ne geçirin

Azure Active Directory Domain Services (AD DS), Klasik sanal ağ modelini kullanan müşteriler için kaynak yöneticisi sanal ağ modeline tek seferlik bir geçişi destekler. Kaynak Yöneticisi dağıtım modelini kullanan Azure AD DS yönetilen etki alanları, ince taneli parola ilkesi, denetim günlükleri ve hesap kilitleme koruması gibi ek özellikler sağlar.

Bu makalede, geçiş için yararları ve hususlar, sonra başarılı bir şekilde varolan bir Azure AD DS örneğini geçirmek için gerekli adımları özetler.

> [!NOTE]
> 2017 yılında Azure AD Etki Alanı Hizmetleri, bir Azure Kaynak Yöneticisi ağında barındırılabilmek için kullanılabilir hale geldi. O zamandan beri, Azure Kaynak Yöneticisi'nin modern özelliklerini kullanarak daha güvenli bir hizmet oluşturabildik. Azure Kaynak Yöneticisi dağıtımları klasik dağıtımların tamamen yerini aldığı için, Azure AD DS klasik sanal ağ dağıtımları 1 Mart 2023'te kullanımdan kaldırılacaktır.
>
> Daha fazla bilgi için [resmi amortisman bildirimine](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/) bakın

## <a name="overview-of-the-migration-process"></a>Geçiş sürecine genel bakış

Geçiş işlemi, Klasik sanal ağda çalışan varolan bir Azure AD DS örneğini alır ve varolan bir Kaynak Yöneticisi sanal ağa taşır. Geçiş PowerShell kullanılarak gerçekleştirilir ve yürütme iki ana aşamaları vardır - *hazırlık* ve *geçiş.*

![Azure AD DS için geçiş sürecine genel bakış](media/migrate-from-classic-vnet/migration-overview.png)

*Hazırlama* aşamasında, Azure AD DS, yönetilen etki alanına senkronize edilmiş kullanıcıların, grupların ve parolaların en son anlık görüntüsünü almak için etki alanının yedeğini alır. Daha sonra eşitleme devre dışı bırakılır ve Azure AD DS yönetilen etki alanını barındıran bulut hizmeti silinir. Hazırlama aşamasında, Azure AD DS yönetilen etki alanı kullanıcıların kimliğini doğrulayamıyor.

![Azure AD DS'nin geçiş için hazırlık aşaması](media/migrate-from-classic-vnet/migration-preparation.png)

*Geçiş* aşamasında, Klasik Azure AD DS yönetilen etki alanından etki alanı denetleyicilerinin altında yatan sanal diskler, Kaynak Yöneticisi dağıtım modelini kullanarak VM'ler oluşturmak üzere kopyalanır. Azure AD DS yönetilen etki alanı, LDAPS ve DNS yapılandırmasını içeren yeniden oluşturulur. Azure AD'ye eşitleme yeniden başlatılır ve LDAP sertifikaları geri yüklenir. Azure AD DS yönetilen etki alanına herhangi bir makineye yeniden katılmanız gerekmez , bunlar yönetilen etki alanına katılmaya ve değişiklik yapmadan çalışmaya devam eder.

![Azure AD DS geçişi](media/migrate-from-classic-vnet/migration-process.png)

## <a name="migration-benefits"></a>Geçiş avantajları

Bu geçiş işlemini kullanarak bir Azure AD DS yönetilen etki alanını taşıdığınızda, makineleri yönetilen etki alanına yeniden katılma veya Azure AD DS örneğini silme ve sıfırdan bir tane oluşturma gereksinimini önlersiniz. VM'ler geçiş işleminin sonunda Azure AD DS yönetilen etki alanına katılmaya devam eder.

Geçişten sonra Azure AD DS, Kaynak Yöneticisi sanal ağlarını kullanan etki alanları için kullanılabilen birçok özellik sağlar:

* İnce taneli parola ilkesi desteği.
* AD hesap kilitleme koruması.
* Azure AD DS yönetilen etki alanında uyarıların e-posta bildirimleri.
* Azure Monitor'u kullanarak günlükleri denetle.
* Azure Dosyaları entegrasyonu
* HD Insights entegrasyonu

Kaynak Yöneticisi sanal ağ kullanan Azure AD DS yönetilen etki alanları, en son yeni özelliklerle güncel kalmanıza yardımcı olur. Klasik sanal ağları kullanarak Azure AD DS desteği gelecekte azaltılacak.

## <a name="example-scenarios-for-migration"></a>Geçiş için örnek senaryolar

Azure AD DS yönetilen etki alanını geçirmek için bazı yaygın senaryolar aşağıdaki örnekleri içerir.

> [!NOTE]
> Başarılı bir geçişi onaylayane kadar Klasik sanal ağı dönüştürmeyin. Sanal ağı dönüştürme, geçiş ve doğrulama aşamalarında herhangi bir sorun olduğunda Azure AD DS yönetilen etki alanını geri alma veya geri yükleme seçeneğini kaldırır.

### <a name="migrate-azure-ad-ds-to-an-existing-resource-manager-virtual-network-recommended"></a>Azure AD DS'yi varolan bir Kaynak Yöneticisi sanal ağına geçirin (önerilir)

Yaygın bir senaryo, varolan diğer Klasik kaynakları kaynak yöneticisi dağıtım modeline ve sanal ağa zaten taşıdığınız senaryodur. Daha sonra Peering, Kaynak Yöneticisi sanal ağından Azure AD DS'yi çalıştırmaya devam eden Klasik sanal ağa kadar kullanılır. Bu yaklaşım, Kaynak Yöneticisi uygulamalarının ve hizmetlerinin Klasik sanal ağdaki Azure AD DS yönetilen etki alanının kimlik doğrulama ve yönetim işlevselliğini kullanmasına olanak tanır. Geçirildikten sonra, tüm kaynaklar Kaynak Yöneticisi dağıtım modeli ve sanal ağ kullanılarak çalıştırılır.

![Azure AD DS'yi varolan bir Kaynak Yöneticisi sanal ağına geçirin](media/migrate-from-classic-vnet/migrate-to-existing-vnet.png)

Bu örnek geçiş senaryosunda yer alan üst düzey adımlar aşağıdaki bölümleri içerir:

1. Klasik sanal ağda yapılandırılan mevcut VPN ağ geçitlerini veya sanal ağ eşlemini kaldırın.
1. Bu makalede özetlenen adımları kullanarak Azure AD DS yönetilen etki alanını geçirin.
1. Başarılı bir geçişi test edin ve onaylayın, ardından Klasik sanal ağı silin.

### <a name="migrate-multiple-resources-including-azure-ad-ds"></a>Azure AD DS dahil olmak üzere birden çok kaynağı geçirme

Bu örnek senaryoda, Azure AD DS'yi ve diğer ilişkili kaynakları Klasik dağıtım modelinden Kaynak Yöneticisi dağıtım modeline geçirebilirsiniz. Bazı kaynaklar Azure AD DS yönetilen etki alanının yanında Klasik sanal ağda çalıştırmaya devam ettiyse, bunların tümü Kaynak Yöneticisi dağıtım modeline geçişten yararlanabilir.

![Kaynak Yöneticisi dağıtım modeline birden çok kaynak geçirme](media/migrate-from-classic-vnet/migrate-multiple-resources.png)

Bu örnek geçiş senaryosunda yer alan üst düzey adımlar aşağıdaki bölümleri içerir:

1. Klasik sanal ağda yapılandırılan mevcut VPN ağ geçitlerini veya sanal ağ eşlemini kaldırın.
1. Bu makalede özetlenen adımları kullanarak Azure AD DS yönetilen etki alanını geçirin.
1. Klasik sanal ağ ve Kaynak Yöneticisi ağı arasında sanal ağ eşleme ayarlayın.
1. Başarılı bir geçişi test edin ve onaylayın.
1. [VM'ler gibi ek Klasik kaynakları taşıyın.][migrate-iaas]

### <a name="migrate-azure-ad-ds-but-keep-other-resources-on-the-classic-virtual-network"></a>Azure AD DS'yi geçirin, ancak diğer kaynakları Klasik sanal ağda tutun

Bu örnek senaryoda, bir oturumda minimum kapalı kalma süresi miktarına sahipsiniz. Azure AD DS'yi yalnızca Kaynak Yöneticisi sanal ağına geçirin ve Varolan kaynakları Klasik dağıtım modeli nde ve sanal ağda saklarsınız. Takip eden bir bakım döneminde, ek kaynakları Klasik dağıtım modelinden ve sanal ağdan istediğiniz gibi geçirebilirsiniz.

![Kaynak Yöneticisi dağıtım modeline yalnızca Azure AD DS'yi geçirin](media/migrate-from-classic-vnet/migrate-only-azure-ad-ds.png)

Bu örnek geçiş senaryosunda yer alan üst düzey adımlar aşağıdaki bölümleri içerir:

1. Klasik sanal ağda yapılandırılan mevcut VPN ağ geçitlerini veya sanal ağ eşlemini kaldırın.
1. Bu makalede özetlenen adımları kullanarak Azure AD DS yönetilen etki alanını geçirin.
1. Klasik sanal ağ ile yeni Kaynak Yöneticisi sanal ağı arasında sanal ağ izleme ayarlayın.
1. Daha sonra, gerektiğinde Klasik sanal ağdan [ek kaynakları geçirin.][migrate-iaas]

## <a name="before-you-begin"></a>Başlamadan önce

Azure AD DS yönetilen bir etki alanı hazırlayıp geçirerken, kimlik doğrulama ve yönetim hizmetlerinin kullanılabilirliği yle ilgili bazı hususlar vardır. Azure AD DS yönetilen etki alanı geçiş sırasında belirli bir süre için kullanılamaz. Azure AD DS'ye dayanan uygulamalar ve hizmetler geçiş sırasında kapalı kalma süresi yaşar.

> [!IMPORTANT]
> Geçiş işlemini başlatmadan önce bu geçiş makalesinin ve kılavuzun tümünün okunmasını izleyin. Geçiş işlemi, Azure AD DS etki alanı denetleyicilerinin zaman dilimleri için kullanılabilirliğini etkiler. Kullanıcılar, hizmetler ve uygulamalar geçiş işlemi sırasında yönetilen etki alanına karşı kimlik doğrulaması yapamaz.

### <a name="ip-addresses"></a>IP adresleri

Bir Azure AD DS yönetilen etki alanı değişikliği için etki alanı denetleyicisi IP adresleri geçişten sonra. Bu değişiklik, güvenli LDAP bitiş noktasının genel IP adresini içerir. Yeni IP adresleri, Kaynak Yöneticisi sanal ağındaki yeni alt ağ için adres aralığının içindedir.

Geri alma durumunda, IP adresleri geri döndükten sonra değişebilir.

Azure AD DS genellikle adres aralığında ki ilk iki kullanılabilir IP adresini kullanır, ancak bu garanti değildir. Geçişten sonra kullanılacak IP adreslerini şu anda belirtemezsiniz.

### <a name="downtime"></a>Downtime

Geçiş işlemi, etki alanı denetleyicilerinin belirli bir süre için çevrimdışı olmayı içerir. Azure AD DS Kaynak Yöneticisi dağıtım modeline ve sanal ağa geçirilirken etki alanı denetleyicilerine erişilemez. Ortalama olarak, kesinti süresi yaklaşık 1 ila 3 saattir. Bu süre, etki alanı denetleyicilerinin çevrimdışı duruma geldiği andan ilk etki alanı denetleyicisinin çevrimiçi olduğu ana kadardır. Bu ortalama, ikinci etki alanı denetleyicisinin çoğaltılması için gereken süreyi veya ek kaynakları Kaynak Yöneticisi dağıtım modeline geçirmek için geçen süreyi içermez.

### <a name="account-lockout"></a>Hesap kilitleme

Azure AD DS yönetilen etki alanlarında Klasik sanal ağlarda çalışan AD hesabı kilitleme ilkeleri yoktur. VM'ler internete maruz kalırsa, saldırganlar hesaplara kaba kuvvet sokmak için parola püskürtme yöntemlerini kullanabilirler. Bu girişimleri durdurmak için hesap kilitleme politikası yok. Kaynak Yöneticisi dağıtım modelini ve sanal ağları kullanan Azure AD DS yönetilen etki alanları için, AD hesap kilitleme ilkeleri bu parola püskürtme saldırılarına karşı koruma sağlar.

Varsayılan olarak, 2 dakika içinde 5 kötü parola denemesi bir hesabı 30 dakika kilitler.

Oturum açma için kilitlenmiş bir hesap kullanılamaz ve bu da Azure AD DS yönetilen etki alanını veya hesap tarafından yönetilen uygulamaları yönetme yeteneğini etkileyebilir. Azure AD DS yönetilen etki alanı geçirildikten sonra, hesaplar oturum açma girişimlerinin tekrarlanan başarısız olması nedeniyle kalıcı bir lokavt gibi hissettirebilir. Geçişten sonra iki yaygın senaryo şunlardır:

* Süresi dolmuş bir parola kullanan bir hizmet hesabı.
    * Hizmet hesabı, hesabı kilitleyen süresi dolmuş bir parolayla oturum açmaya çalışır. Bunu düzeltmek için, süresi dolmuş kimlik bilgilerini içeren uygulamayı veya VM'yi bulun ve parolayı güncelleştirin.
* Kötü niyetli bir varlık hesaplarda oturum açma girişimleri kullanır.
    * VM'ler internete maruz kaldığında, saldırganlar genellikle imzalamaya çalışırken sık sık yaygın kullanıcı adı ve parola kombinasyonlarını denerler. Bu yinelenen başarısız oturum açma denemeleri hesapları kilitleyebilir. Yönetici hesaplarının kilitlenmesini en aza indirmek için yönetici hesaplarının *yönetici* veya *yönetici*gibi genel adlara sahip kullanılması önerilmez.
    * Internet'e maruz kalan VM sayısını en aza indirin. Azure portalını kullanarak VM'lere güvenli bir şekilde bağlanmak için [Azure Bastion'ı][azure-bastion] kullanabilirsiniz.

Bazı hesapların geçişten sonra kilitlenebileceğinden şüpheleniyorsanız, son geçiş adımları denetimi etkinleştirmeyi veya ince taneli parola ilkesi ayarlarını nasıl değiştireceğinizi ana hatlar.

### <a name="roll-back-and-restore"></a>Geri alma ve geri yükleme

Geçiş başarılı olmazsa, Azure AD DS yönetilen etki alanını geri alma veya geri yükleme işlemi vardır. Geri alma, yönetilen etki alanının durumunu geçiş girişiminden önceye hemen döndürmek için bir self servis seçeneğidir. Azure destek mühendisleri, yönetilen bir etki alanını son çare olarak yedeklemeden geri de yükleyebilir. Daha fazla bilgi için, [başarısız bir geçişten nasıl geri döneceğine veya geri yükleyilen bilgilere](#roll-back-and-restore-from-migration)bakın.

### <a name="restrictions-on-available-virtual-networks"></a>Kullanılabilir sanal ağlarla ilgili kısıtlamalar

Sanal ağlarda Azure AD DS yönetilen etki alanının geçirilebileceği bazı kısıtlamalar vardır. Hedef Kaynak Yöneticisi sanal ağ aşağıdaki gereksinimleri karşılamalıdır:

* Kaynak Yöneticisi sanal ağı, Azure AD DS'nin şu anda dağıtıldığı Klasik sanal ağla aynı Azure aboneliğinde olmalıdır.
* Kaynak Yöneticisi sanal ağı, Azure AD DS'nin şu anda dağıtıldığı Klasik sanal ağla aynı bölgede olmalıdır.
* Kaynak Yöneticisi sanal ağın alt ağında en az 3-5 kullanılabilir IP adresi olmalıdır.
* Kaynak Yöneticisi sanal ağının alt ağı, Azure AD DS için özel bir alt ağ olmalıdır ve başka iş yüklerini barındırmamalıdır.

Sanal ağ gereksinimleri hakkında daha fazla bilgi için [Sanal ağ tasarımı konuları ve yapılandırma seçenekleri][network-considerations]ne bakın.

## <a name="migration-steps"></a>Geçiş adımları

Kaynak Yöneticisi dağıtım modeline ve sanal ağa geçiş 5 ana adıma ayrılır:

| Adım    | Üzerinden gerçekleştirilen  | Tahmini süre  | Downtime  | Geri dön/Geri Yükleme? |
|---------|--------------------|-----------------|-----------|-------------------|
| [Adım 1 - Yeni sanal ağı güncelleştirin ve bulun](#update-and-verify-virtual-network-settings) | Azure portal | 15 dakika | Kapalı kalma süresi gerekmez | Yok |
| [Adım 2 - Azure AD DS yönetilen etki alanını geçiş için hazırlama](#prepare-the-managed-domain-for-migration) | PowerShell | 15 - Ortalama 30 dakika | Azure AD DS'nin kapalı kalma süresi, bu komut tamamlandıktan sonra başlar. | Geri dön ve kullanılabilir geri yükleyin. |
| [Adım 3 - Azure AD DS yönetilen etki alanını varolan bir sanal ağa taşıma](#migrate-the-managed-domain) | PowerShell | 1 – Ortalama 3 saat | Bu komut tamamlandıktan ve kapalı kalma süresi sona erdiğinde bir etki alanı denetleyicisi kullanılabilir. | Hata da, hem geri alma (self servis) hem de geri yükleme kullanılabilir. |
| [Adım 4 - Test edin ve çoğaltma etki alanı denetleyicisini bekleyin](#test-and-verify-connectivity-after-the-migration)| PowerShell ve Azure portalı | Test sayısına bağlı olarak 1 saat veya daha fazla | Her iki etki alanı denetleyicisi de kullanılabilir ve normal çalışması gerekir. | Yok. İlk VM başarıyla geçirildikten sonra geri alma veya geri yükleme seçeneği yoktur. |
| [Adım 5 - İsteğe bağlı yapılandırma adımları](#optional-post-migration-configuration-steps) | Azure portalı ve VM'ler | Yok | Kapalı kalma süresi gerekmez | Yok |

> [!IMPORTANT]
> Ek kapalı kalma süresini önlemek için, geçiş işlemini başlatmadan önce bu geçiş makalesinin ve kılavuzun tümünün okunmasını izleyin. Geçiş işlemi, Azure AD DS etki alanı denetleyicilerinin belirli bir süre için kullanılabilirliğini etkiler. Kullanıcılar, hizmetler ve uygulamalar geçiş işlemi sırasında yönetilen etki alanına karşı kimlik doğrulaması yapamaz.

## <a name="update-and-verify-virtual-network-settings"></a>Sanal ağ ayarlarını güncelleştirme ve doğrulama

Geçiş işlemine başlamadan önce, aşağıdaki ilk denetimleri ve güncelleştirmeleri tamamlayın. Bu adımlar geçişten önce herhangi bir zamanda gerçekleşebilir ve Azure AD DS yönetilen etki alanının çalışmasını etkilemez.

1. Yerel Azure PowerShell ortamınızı en son sürüme güncelleyin. Geçiş adımlarını tamamlamak için en az sürüm *2.3.2'ye*ihtiyacınız var.

    PowerShell sürümünüzü nasıl kontrol edip güncelleştireceğine ilişkin bilgi için [Azure PowerShell'e genel bakış][azure-powershell]bilgisine bakın.

1. Varolan bir Kaynak Yöneticisi sanal ağı oluşturun veya seçin.

    Ağ ayarlarının Azure AD DS için gerekli bağlantı noktalarını engellemediğinden emin olun. Bağlantı noktaları hem Klasik sanal ağda hem de Kaynak Yöneticisi sanal ağında açık olmalıdır. Bu ayarlar rota tabloları (rota tabloları nın kullanılması önerilmese de) ve ağ güvenlik gruplarını içerir.

    Gerekli bağlantı noktalarını görüntülemek için [Ağ güvenlik gruplarına ve gerekli bağlantı noktalarına][network-ports]bakın. Ağ iletişimi sorunlarını en aza indirmek için, geçiş başarıyla tamamlandıktan sonra bir ağ güvenlik grubu veya rota tablosunu Kaynak Yöneticisi sanal ağına beklemeniz ve uygulamanız önerilir.

    Bu hedef kaynak grubunu not edin, sanal ağı hedefle ve hedef sanal ağ alt ağını hedefle. Bu kaynak adları geçiş işlemi sırasında kullanılır.

1. Azure portalında Azure AD DS yönetilen etki alanı durumunu kontrol edin. Yönetilen etki alanı için herhangi bir uyarınız varsa, geçiş işlemini başlatmadan önce bunları çözümleyin.
1. İsteğe bağlı olarak, diğer kaynakları Kaynak Yöneticisi dağıtım modeline ve sanal ağa taşımayı planlıyorsanız, bu kaynakların geçirilebileceğini onaylayın. Daha fazla bilgi için, [IaaS kaynaklarının Klasik'ten Kaynak Yöneticisi'ne Platform destekli geçişine][migrate-iaas]bakın.

    > [!NOTE]
    > Klasik sanal ağı Kaynak Yöneticisi sanal ağına dönüştürmeyin. Bunu yaparsanız, Azure AD DS yönetilen etki alanını geri alma veya geri yükleme seçeneği yoktur.

## <a name="prepare-the-managed-domain-for-migration"></a>Yönetilen etki alanını geçişiçin hazırlama

Azure PowerShell, Azure AD DS yönetilen etki alanını geçiş için hazırlamak için kullanılır. Bu adımlar arasında yedekleme, eşitleme yi duraklatma ve Azure AD DS barındıran bulut hizmetini silme yer alıyor. Bu adım tamamlandığında, Azure AD DS bir süre için çevrimdışı na alınır. Hazırlık adımı başarısız olursa, [önceki duruma geri dönebilirsiniz.](#roll-back)

Azure AD DS yönetilen etki alanını geçişe hazırlamak için aşağıdaki adımları tamamlayın:

1. Komut `Migrate-Aaads` dosyasını [PowerShell Galerisi'nden][powershell-script]yükleyin. Bu PowerShell geçiş komut dosyası, Azure AD mühendislik ekibi tarafından dijital olarak imzalanmış bir komut dosyasıdır.

    ```powershell
    Install-Script -Name Migrate-Aadds
    ```

1. [Get-Credential][get-credential] cmdlet kullanarak geçiş komut dosyası tarafından kimlik bilgilerini tutmak için bir değişken oluşturun.

    Belirttiğiniz kullanıcı hesabı, azure AD DS'nizi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ve ardından Azure aboneliğinizde *katılımcı* ayrıcalıklarına ihtiyaç duyar ve gerekli Azure AD DS kaynaklarını oluşturur.

    İstendiğinde, uygun bir kullanıcı hesabı ve parola girin:

    ```powershell
    $creds = Get-Credential
    ```

1. Şimdi `Migrate-Aadds` *-Hazırla* parametresini kullanarak cmdlet çalıştırın. *aaddscontoso.com*gibi kendi Azure AD DS yönetilen etki alanı nız için *-ManagedDomainFqdn'yi* sağlayın:

    ```powershell
    Migrate-Aadds `
        -Prepare `
        -ManagedDomainFqdn aaddscontoso.com `
        -Credentials $creds
    ```

## <a name="migrate-the-managed-domain"></a>Yönetilen etki alanını geçirin

Azure AD DS yönetilen etki alanı hazırlanıp yedeklendirildiği için etki alanı geçirilebilir. Bu adım, Kaynak Yöneticisi dağıtım modelini kullanarak Azure AD Etki Alanı Hizmetleri etki alanı denetleyicisi VM'leri yeniden oluşturur. Bu adımın tamamlanması 1 ila 3 saat sürebilir.

`Migrate-Aadds` *-Commit* parametresini kullanarak cmdlet'i çalıştırın. *Aaddscontoso.com*gibi önceki bölümde hazırlanan kendi Azure AD DS yönetilen etki alanı için *-ManagedDomainFqdn* sağlayın:

*MyResourceGroup*gibi Azure AD DS'ye geçirmek istediğiniz sanal ağı içeren hedef kaynak grubunu belirtin. *myVnet*gibi hedef sanal ağı ve *DomainServices*gibi alt ağı sağlayın.

Bu komut çalıştırıladıktan sonra geri alamazsınız:

```powershell
Migrate-Aadds `
    -Commit `
    -ManagedDomainFqdn aaddscontoso.com `
    -VirtualNetworkResourceGroupName myResourceGroup `
    -VirtualNetworkName myVnet `
    -VirtualSubnetName DomainServices `
    -Credentials $creds
```

Komut dosyası, yönetilen etki alanının geçiş için hazırolduğunu doğruladıktan sonra, geçiş işlemini başlatmak için *Y* girin.

> [!IMPORTANT]
> Geçiş işlemi sırasında Klasik sanal ağı Kaynak Yöneticisi sanal ağına dönüştürmeyin. Sanal ağı dönüştürürseniz, orijinal sanal ağ artık var olmayacağından Azure AD DS yönetilen etki alanını geri alamaz veya geri yükleyemezsiniz.

Geçiş işlemi sırasında her iki dakikada bir, bir ilerleme göstergesi aşağıdaki örnek çıktıda gösterildiği gibi geçerli durumu bildirir:

![Azure AD DS geçişinin ilerleme göstergesi](media/migrate-from-classic-vnet/powershell-migration-status.png)

PowerShell komut dosyasını kapatsanız bile geçiş işlemi devam ediyor. Azure portalında, yönetilen etki alanının durumu *Geçirilme*olarak raporlanmaktadır.

Geçiş başarıyla tamamlandığında, ilk etki alanı denetleyicinizin IP adresini Azure portalında veya Azure PowerShell üzerinden görüntüleyebilirsiniz. Kullanılabilen ikinci etki alanı denetleyicisi hakkında bir zaman tahmini de gösterilir.

Bu aşamada, varolan diğer kaynakları isteğe bağlı olarak Klasik dağıtım modelinden ve sanal ağdan taşıyabilirsiniz. Veya, Kaynakları Klasik dağıtım modelinde tutabilir ve Azure AD DS geçişi tamamlandıktan sonra sanal ağları birbirine eşleyebilirsiniz.

## <a name="test-and-verify-connectivity-after-the-migration"></a>Geçişten sonra bağlantıyı test edin ve doğrulayın

İkinci etki alanı denetleyicisinin Başarıyla dağıtılması ve Azure AD DS yönetilen etki alanında kullanılabilir olması biraz zaman alabilir.

Kaynak Yöneticisi dağıtım modeliyle, Azure AD DS yönetilen etki alanının ağ kaynakları Azure portalında veya Azure PowerShell'de gösterilir. Bu ağ kaynaklarının ne olduğu ve ne yaptığı hakkında daha fazla bilgi edinmek için Azure [AD DS tarafından kullanılan Ağ kaynaklarına][network-resources]bakın.

En az bir etki alanı denetleyicisi kullanılabilir olduğunda, VM'lerle ağ bağlantısı için aşağıdaki yapılandırma adımlarını tamamlayın:

* **DNS sunucu ayarlarını güncelleştirme** Kaynak Yöneticisi sanal ağındaki diğer kaynakların Azure AD DS yönetilen etki alanını çözmesine ve kullanmasına izin vermek için, DNS ayarlarını yeni etki alanı denetleyicilerinin IP adresleriyle güncelleştirin. Azure portalı bu ayarları sizin için otomatik olarak yapılandırabilir. Kaynak Yöneticisi sanal ağını nasıl yapılandırılabildiğini öğrenmek [için Azure sanal ağı için DNS'yi güncelleştir ayarlarına][update-dns]bakın.
* **Etki alanına katılan VM'leri yeniden başlatın** - Azure AD DS etki alanı denetleyicilerinin DNS sunucusu IP adresleri değiştikçe, etki alanına katılan VM'leri yeniden başlatın, böylece yeni DNS sunucu ayarlarını kullanın. Uygulamalar veya VM'ler DNS ayarlarını el ile yapılandırmışsa, bunları Azure portalında gösterilen etki alanı denetleyicilerinin yeni DNS sunucusu IP adresleriyle el ile güncelleştirin.

Şimdi sanal ağ bağlantısını ve ad çözümlemini test edin. Kaynak Yöneticisi sanal ağına bağlı veya ona bakan bir VM'de aşağıdaki ağ iletişim testlerini deneyin:

1. Etki alanı denetleyicilerinden birinin IP adresini pingedip pinglemdiğinizi kontrol edin, örneğin`ping 10.1.0.4`
    * Etki alanı denetleyicilerinin IP adresleri, Azure portalındaki Azure AD DS yönetilen etki alanının **Özellikleri** sayfasında gösterilir.
1. Yönetilen etki alanının ad çözümlemesi gibi`nslookup aaddscontoso.com`
    * DNS ayarlarının doğru olduğunu ve çözüldüğünü doğrulamak için kendi Azure AD DS yönetilen etki alanınızın DNS adını belirtin.

İkinci etki alanı denetleyicisi geçiş cmdlet bittikten 1-2 saat sonra kullanılabilir olmalıdır. İkinci etki alanı denetleyicisinin kullanılabilir olup olmadığını kontrol etmek için Azure portalındaki Azure AD DS yönetilen etki alanının **Özellikler** sayfasına bakın. İki IP adresi gösteriliyorsa, ikinci etki alanı denetleyicisi hazırdır.

## <a name="optional-post-migration-configuration-steps"></a>İsteğe bağlı geçiş sonrası yapılandırma adımları

Geçiş işlemi başarıyla tamamlandığında, bazı isteğe bağlı yapılandırma adımları denetim günlüklerini veya e-posta bildirimlerini etkinleştirmeveya özel olarak verilen parola ilkesini güncelleştirmeyi içerir.

### <a name="subscribe-to-audit-logs-using-azure-monitor"></a>Azure Monitör'ü kullanarak denetim günlüklerine abone olun

Azure AD DS, etki alanı denetleyicilerinde sorun gidermeye ve olayları görüntülemeye yardımcı olmak için denetim günlüklerini ortaya çıkarır. Daha fazla bilgi için denetim [günlüklerini etkinleştir ve kullanın.][security-audits]

Günlüklerde açığa çıkan önemli bilgileri izlemek için şablonlar kullanabilirsiniz. Örneğin, denetim günlüğü çalışma kitabı şablonu Azure AD DS yönetilen etki alanında olası hesap kilitlemeleri izleyebilir.

### <a name="configure-azure-ad-domain-services-email-notifications"></a>Azure AD Etki Alanı Hizmetleri e-posta bildirimlerini yapılandırma

Azure AD DS yönetilen etki alanında bir sorun algılandığında haberdar olmak için Azure portalındaki e-posta bildirimi ayarlarını güncelleştirin. Daha fazla bilgi için [bkz.][notifications]

### <a name="update-fine-grained-password-policy"></a>İnce taneli parola ilkesini güncelleştirme

Gerekirse, ince taneli parola ilkesini varsayılan yapılandırmadan daha az kısıtlayıcı olacak şekilde güncelleştirebilirsiniz. Denetim günlüklerini, daha az kısıtlayıcı bir ayarın anlamlı olup olmadığını belirlemek için kullanabilir, ardından ilkeyi gerektiği gibi yapılandırabilirsiniz. Geçişten sonra tekrar tekrar kilitlenmiş olan hesapların ilke ayarlarını gözden geçirmek ve güncelleştirmek için aşağıdaki üst düzey adımları kullanın:

1. Azure AD DS yönetilen etki alanında daha az kısıtlama için [parola ilkesini yapılandırın][password-policy] ve denetim günlüklerinde olayları gözlemleyin.
1. Herhangi bir hizmet hesabı denetim günlüklerinde tanımlanan süresi dolmuş parolaları kullanıyorsa, bu hesapları doğru parolayla güncelleştirin.
1. Bir VM internete maruz kalırsa, *yönetici,* *kullanıcı*veya yüksek oturum açma girişimleri olan *konuk* gibi genel hesap adlarını gözden geçirin. Mümkün olduğunda, daha az genel adlandırılmış hesapları kullanmak için bu VM'leri güncelleştirin.
1. Saldırıların kaynağını bulmak ve bu IP adreslerinin oturum açma girişiminde bulunmasını engellemek için VM'deki ağ izlemesini kullanın.
1. En az kilitleme sorunu olduğunda, ince taneli parola ilkesini gerektiği kadar kısıtlayıcı olacak şekilde güncelleştirin.

### <a name="creating-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Azure AD DS' nin yönetilen etki alanı için gereken bağlantı noktalarını güvenli hale getirmek ve diğer tüm gelen trafiği engellemek için bir ağ güvenlik grubuna ihtiyacı vardır. Bu ağ güvenlik grubu, yönetilen etki alanına erişimi kilitlemek için ek bir koruma katmanı görevi görür ve otomatik olarak oluşturulmaz. Ağ güvenlik grubunu oluşturmak ve gerekli bağlantı noktalarını açmak için aşağıdaki adımları gözden geçirin:

1. Azure portalında Azure AD DS kaynağınızı seçin. Genel bakış sayfasında, Azure AD Etki Alanı Hizmetleri ile ilişkili hiçbir şey yoksa ağ güvenlik grubu oluşturmak için bir düğme görüntülenir.
1. Güvenli LDAP kullanıyorsanız, *TCP* bağlantı noktası *636*için gelen trafiğe izin vermek için ağ güvenlik grubuna bir kural ekleyin. Daha fazla bilgi için [bkz.][secure-ldap]

## <a name="roll-back-and-restore-from-migration"></a>Geri alma ve geçişten geri yükleme

Geçiş işleminde belirli bir noktaya kadar, Azure AD DS yönetilen etki alanını geri almayı veya geri yüklemeyi seçebilirsiniz.

### <a name="roll-back"></a>Geri alma

2. adımda geçişe hazırlanmak için PowerShell cmdlet'i çalıştırdığınızda veya 3. adımdaki geçişe hazırlanmak için bir hata varsa, Azure AD DS yönetilen etki alanı özgün yapılandırmaya geri dönebilir. Bu geri dönüş için orijinal Klasik sanal ağ gerektirir. IP adreslerinin geri alma sonrasında da değişebileceğini unutmayın.

`Migrate-Aadds` *-Abort* parametresini kullanarak cmdlet çalıştırın. *aaddscontoso.com*ve *myClassicVnet*gibi Klasik sanal ağ adı gibi önceki bir bölümde hazırlanmış kendi Azure AD DS yönetilen etki alanı için *-ManagedDomainFqdn* sağlayın:

```powershell
Migrate-Aadds `
    -Abort `
    -ManagedDomainFqdn aaddscontoso.com `
    -ClassicVirtualNetworkName myClassicVnet `
    -Credentials $creds
```

### <a name="restore"></a>Geri Yükleme

Son çare olarak, Azure AD Etki Alanı Hizmetleri kullanılabilir son yedeklemeden geri yüklenebilir. En güncel yedeklemenin kullanılabilir olduğundan emin olmak için geçişin 1. Bu yedekleme 30 gün saklanır.

Azure AD DS yönetilen etki alanını yedeklemeden geri yüklemek için, [Azure portalını kullanarak bir destek örneği bileti açın.][azure-support] Dizin kimliğinizi, etki alanı adınızı ve geri yükleme nedenini sağlayın. Destek ve geri yükleme işleminin tamamlanması birkaç gün sürebilir.

## <a name="troubleshooting"></a>Sorun giderme

Kaynak Yöneticisi dağıtım modeline geçişten sonra sorun yaşıyorsanız, aşağıdaki sık karşılaşılan sorun giderme alanlarından bazılarını gözden geçirin:

* [Sorun giderme etki alanı birleştirme sorunları][troubleshoot-domain-join]
* [Sorun giderme hesabı kilitleme sorunları][troubleshoot-account-lockout]
* [Hesap oturum açma sorunları][troubleshoot-sign-in]
* [Sorun giderme güvenli LDAP bağlantı sorunları][tshoot-ldaps]

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS yönetilen etki alanınızın Kaynak Yöneticisi dağıtım modeline geçirildiği bir windows [vm oluşturup etki alanına katılın][join-windows] ve ardından [yönetim araçlarını yükleyin.][tutorial-create-management-vm]

<!-- INTERNAL LINKS -->
[azure-bastion]: ../bastion/bastion-overview.md
[network-considerations]: network-considerations.md
[azure-powershell]: /powershell/azure/overview
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Set-AzContext]: /powershell/module/az.accounts/set-azcontext
[Get-AzResource]: /powershell/module/az.resources/get-azresource
[Set-AzResource]: /powershell/module/az.resources/set-azresource
[network-resources]: network-considerations.md#network-resources-used-by-azure-ad-ds
[update-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[security-audits]: security-audit-events.md
[notifications]: notifications.md
[password-policy]: password-policy.md
[secure-ldap]: tutorial-configure-ldaps.md
[migrate-iaas]: ../virtual-machines/windows/migration-classic-resource-manager-overview.md
[join-windows]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[troubleshoot-domain-join]: troubleshoot-domain-join.md
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[troubleshoot-sign-in]: troubleshoot-sign-in.md
[tshoot-ldaps]: tshoot-ldaps.md
[get-credential]: /powershell/module/microsoft.powershell.security/get-credential

<!-- EXTERNAL LINKS -->
[powershell-script]: https://www.powershellgallery.com/packages/Migrate-Aadds/
