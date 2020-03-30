---
title: include dosyası
description: include dosyası
services: virtual-machines
author: tanmaygore
ms.service: virtual-machines
ms.topic: include
ms.date: 02/06/2020
ms.author: tagore
ms.custom: include file
ms.openlocfilehash: 4e07334e859f2c1401547cc3f88988830b71c5e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77192961"
---
Bu makalede, Klasik Kaynak Yöneticisi dağıtım modellerinden hizmet (IaaS) kaynakları olarak altyapının nasıl geçirilen ve sanal ağı kullanarak aboneliğinizde bir arada bulunan iki dağıtım modelindeki kaynakların nasıl bağlanacağının ayrıntıları açıklanmaktadır. siteden siteye ağ geçitleri. [Azure Kaynak Yöneticisi özellikleri ve avantajları](../articles/azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="goal-for-migration"></a>Geçiş hedefi
Kaynak Yöneticisi şablonlar aracılığıyla karmaşık uygulamalar dağıtmayı sağlar, VM uzantıları kullanarak sanal makineleri yapılandırır ve erişim yönetimi ve etiketleme içerir. Azure Kaynak Yöneticisi, sanal makineler için kullanılabilirlik kümelerine ölçeklenebilir, paralel dağıtım içerir. Yeni dağıtım modeli, işlem, ağ ve depolamanın yaşam döngüsü yönetimini de bağımsız olarak sağlar. Son olarak, sanal ağdaki sanal makinelerin uygulanmasıyla varsayılan olarak güvenliği etkinleştirme üzerinde duruluyor.

Klasik dağıtım modelindeki hemen hemen tüm özellikler, Azure Kaynak Yöneticisi altında bilgi işlem, ağ ve depolama için desteklenir. Azure Kaynak Yöneticisi'ndeki yeni özelliklerden yararlanmak için, varolan dağıtımları Klasik dağıtım modelinden geçirebilirsiniz.

## <a name="supported-resources-for-migration"></a>Geçiş için desteklenen kaynaklar
Bu klasik IaaS kaynakları geçiş sırasında desteklenir

* Virtual Machines
* Kullanılabilirlik Kümeleri
* Depolama Hesapları
* Sanal Ağlar
* VPN Ağ Geçitleri
* Express Route Ağ Geçitleri _(yalnızca Sanal Ağ ile aynı abonelikte)_
* Ağ Güvenlik Grupları
* Yönlendirme Tabloları
* Ayrılmış IP’ler

## <a name="supported-scopes-of-migration"></a>Desteklenen geçiş kapsamları
İşlem, ağ ve depolama kaynaklarının geçişini tamamlamanın dört farklı yolu vardır:

* [Sanal makinelerin geçişi (SANAL ağda Değİl)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [Sanal makinelerin geçişi (sanal ağda)](#migration-of-virtual-machines-in-a-virtual-network)
* [Depolama hesaplarının geçişi](#migration-of-storage-accounts)
* [Bekar kaynakların geçişi](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Sanal makinelerin geçişi (SANAL ağda Değİl)
Kaynak Yöneticisi dağıtım modelinde, uygulamalarınız için varsayılan olarak güvenlik zorlanır. Tüm Sanal M'lerin Kaynak Yöneticisi modelinde sanal ağda olması gerekir. Azure platformu, geçişin `Deallocate`bir `Start`parçası olarak VM'leri yeniden başlatır.`Stop` Sanal Makineler'in geçirileceği sanal ağlar için iki seçeneğiniz vardır:

* Yeni bir sanal ağ oluşturmak ve sanal makineyi yeni sanal ağa geçirmek için platformu isteyebilirsiniz.
* Sanal makineyi Kaynak Yöneticisi'nde varolan bir sanal ağa geçirebilirsiniz.

> [!NOTE]
> Bu geçiş kapsamında, hem yönetim-düzlem işlemleri ne de veri düzlemi işlemlerigeçiş sırasında bir süre izin verilmeyebilir.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Sanal makinelerin geçişi (sanal ağda)
Çoğu VM yapılandırması için, Klasik ve Kaynak Yöneticisi dağıtım modelleri arasında yalnızca meta veriler geçiş yapmaktadır. Temel Deki VM'ler aynı donanımda, aynı ağda ve aynı depolama alanında çalışır. Yönetim-düzlem işlemleri geçiş sırasında belirli bir süre için izin verilmeyebilir. Ancak, veri düzlemi çalışmaya devam ediyor. Diğer bir deyişle, VM'lerin (klasik) üzerinde çalışan uygulamalarınız geçiş sırasında kapalı kalma süresine neden olmaz.

Aşağıdaki yapılandırmalar şu anda desteklenmez. Gelecekte destek eklenirse, bu yapılandırmadaki bazı VM'ler kapalı kalma süresine neden olabilir (stop, deallocate ve vm işlemlerini yeniden başlatın).

* Tek bir bulut hizmetinde ayarlanmış birden fazla kullanılabilirlik var.
* Tek bir bulut hizmetinde ayarlanmış bir kullanılabilirlik kümesinde olmayan bir veya daha fazla kullanılabilirlik kümeniz ve VM'niz vardır.

> [!NOTE]
> Bu geçiş kapsamında, yönetim düzlemine geçiş sırasında bir süre izin verilmeyebilir. Daha önce açıklandığı gibi belirli yapılandırmalar için veri düzlemi kapalı kalma süresi oluşur.
>

### <a name="migration-of-storage-accounts"></a>Depolama hesaplarının geçişi
Sorunsuz geçişe izin vermek için, Kaynak Yöneticisi VM'lerini klasik bir depolama hesabında dağıtabilirsiniz. Bu özellik sayesinde, işlem ve ağ kaynakları depolama hesaplarından bağımsız olarak geçirilebilir ve geçirilmelidir. Sanal Makineler ve Sanal Ağınız üzerinden geçiş yaptıktan sonra, geçiş işlemini tamamlamak için depolama hesaplarınız üzerinden geçiş yapmanız gerekir.

Depolama hesabınızda ilişkili diskler veya Sanal Makineler verileri yoksa ve yalnızca blob'lar, dosyalar, tablolar ve kuyruklar varsa, Azure Kaynak Yöneticisi'ne geçiş, bağımlılıklar olmadan bağımsız bir geçiş olarak yapılabilir.

> [!NOTE]
> Kaynak Yöneticisi dağıtım modelinde Klasik görüntüler ve diskler kavramı yoktur. Depolama hesabı geçirildiğinde, Kaynak Yöneticisi yığınında Klasik görüntüler ve diskler görünmez, ancak desteklenen VHD'ler depolama hesabında kalır.

Aşağıdaki ekran görüntüleri, Azure portalını kullanarak Bir Klasik depolama hesabının Azure Kaynak Yöneticisi depolama hesabına nasıl yükseltilir gösteriş yapar:
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Depolama hesabınıza gidin.
3. **Ayarlar** bölümünde **ARM'a Geçir'i**tıklatın.
4. Geçiş fizibilitesini belirlemek için **Doğrula'yı** tıklatın.
5. Doğrulama geçerse, geçirilen bir depolama hesabı oluşturmak için **Hazırla'yı** tıklatın.
6. Geçişi onaylamak için **evet** yazın ve geçişi tamamlamak için **Commit'i** tıklatın.

    ![Depolama Hesabını Doğrula](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-1.png)
    
    ![Depolama Hesabı Hazırla](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-2.png)
    
    ![Depolama Hesabı Geçişini Sonuçlandır](../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-3.png)

### <a name="migration-of-unattached-resources"></a>Bekar kaynakların geçişi
İlişkili diskleri veya Sanal Makineler verileri olmayan Depolama Hesapları bağımsız olarak geçirilebilir.

Ağ Güvenlik Grupları, Rota Tabloları & Sanal Makinelere ve Sanal Ağlara bağlı olmayan Ayrılmış IP'ler de bağımsız olarak geçirilebilir.

<br>

## <a name="unsupported-features-and-configurations"></a>Desteklenmeyen özellikler ve yapılandırmalar
Bazı özellikler ve yapılandırmalar şu anda desteklenmez; aşağıdaki bölümlerde çevrelerindeki önerilerimiz açıklayınız.

### <a name="unsupported-features"></a>Desteklenmeyen özellikler
Aşağıdaki özellikler şu anda desteklenmez. İsteğe bağlı olarak bu ayarları kaldırabilir, VM'leri geçirebilir ve ardından Kaynak Yöneticisi dağıtım modelindeki ayarları yeniden etkinleştirebilirsiniz.

| Kaynak sağlayıcısı | Özellik | Öneri |
| --- | --- | --- |
| İşlem | İlişkisiz sanal makine diskleri. | Depolama Hesabı geçirildiğinde bu disklerin arkasındaki VHD lekeleri geçirilir |
| İşlem | Sanal makine görüntüleri. | Depolama Hesabı geçirildiğinde bu disklerin arkasındaki VHD lekeleri geçirilir |
| Ağ | Uç nokta ACL’leri. | Endpoint ALA'ları kaldırın ve geçişi yeniden deneyin. |
| Ağ | Application Gateway | Geçişe başlamadan önce Uygulama Ağ Geçidi'ni kaldırın ve geçiş tamamlandıktan sonra Uygulama Ağ Geçidi'ni yeniden oluşturun. |
| Ağ | VNet Peering kullanan sanal ağlar. | Sanal Ağı Kaynak Yöneticisi'ne geçirin, ardından eşolun. [VNet Peering](../articles/virtual-network/virtual-network-peering-overview.md)hakkında daha fazla bilgi edinin. |

### <a name="unsupported-configurations"></a>Desteklenmeyen yapılandırmalar
Aşağıdaki yapılandırmalar şu anda desteklenmez.

| Hizmet | Yapılandırma | Öneri |
| --- | --- | --- |
| Resource Manager |Klasik kaynaklar için Rol Tabanlı Erişim Denetimi (RBAC) |Kaynakların URI'si geçişten sonra değiştirildiğinden, geçişten sonra gerçekleşmesi gereken RBAC ilke güncelleştirmelerini planlamanız önerilir. |
| İşlem |VM ile ilişkili birden çok alt ağ |Yalnızca bir alt ağa başvurmak için alt ağ yapılandırmasını güncelleştirin. Bu, VM'den ikincil bir NIC'i (başka bir alt ağa atıfta bulunan) kaldırmanızı ve geçiş tamamlandıktan sonra yeniden takmanızı gerektirebilir. |
| İşlem |Sanal ağa ait ancak atanmış açık bir alt ağı olmayan sanal makineler |İsteğe bağlı olarak VM'yi silebilirsiniz. |
| İşlem |Uyarıları olan sanal makineler, Otomatik Ölçeklendirme ilkeleri |Geçiş geçer ve bu ayarlar bırakılır. Geçiş yapmadan önce çevrenizi değerlendirmeniz önerilir. Alternatif olarak, geçiş tamamlandıktan sonra uyarı ayarlarını yeniden yapılandırabilirsiniz. |
| İşlem |XML VM uzantıları (BGInfo 1.*, Visual Studio Debugger, Web Dağıtma ve Uzaktan Hata Ayıklama) |Bu özellik desteklenmez. Geçişe devam etmek için bu uzantıları sanal makineden kaldırmanız önerilir veya geçiş işlemi sırasında otomatik olarak bırakılır. |
| İşlem |Premium depolama alanı ile önyükleme tanılama |Geçişe devam etmeden önce VM'ler için Önyükleme Tanılama özelliğini devre dışı. Geçiş tamamlandıktan sonra Kaynak Yöneticisi yığınında önyükleme tanılamayeniden etkinleştirebilirsiniz. Ayrıca, ekran görüntüsü ve seri günlükleri için kullanılan lekeler silinmelidir, böylece artık bu lekeler için ücretlendirilmezsiniz. |
| İşlem | Web/çalışan rolleri içeren bulut hizmetleri | Bu şu anda desteklenmiyor. |
| İşlem | Birden fazla kullanılabilirlik kümesi veya birden çok kullanılabilirlik kümesi içeren bulut hizmetleri. |Bu şu anda desteklenmiyor. Lütfen Sanal Makineler'i geçişten önce aynı kullanılabilirlik kümesine taşıyın. |
| İşlem | Azure Güvenlik Merkezi uzantısı ile VM | Azure Güvenlik Merkezi, güvenliklerini izlemek ve uyarıları yükseltmek için uzantıları Sanal Makinelerinize otomatik olarak yükler. Abonelikte Azure Güvenlik Merkezi ilkesi etkinse, bu uzantılar genellikle otomatik olarak yüklenir. Sanal Makineler'i geçirmek için, Güvenlik Merkezi izleme uzantısını Sanal Makineler'den kaldıran abonelikteki güvenlik merkezi ilkesini devre dışı kaldırın. |
| İşlem | Yedekleme veya anlık görüntü uzantısı ile VM | Bu uzantılar, Azure Yedekleme hizmetiyle yapılandırılan sanal makineye yüklenir. Bu VM'lerin geçişi desteklenmese de, geçişten önce alınan yedeklemeleri tutmak için [buradaki](/azure/virtual-machines/windows/migration-classic-resource-manager-faq#i-backed-up-my-classic-vms-in-a-vault-can-i-migrate-my-vms-from-classic-mode-to-resource-manager-mode-and-protect-them-in-a-recovery-services-vault) kılavuzu izleyin.  |
| İşlem | Azure Site Kurtarma uzantılı VM | Bu uzantılar, Azure Site Kurtarma hizmetiyle yapılandırılan sanal makineye yüklenir. Site Kurtarma ile kullanılan depolama geçişi çalışacak olsa da, geçerli çoğaltma etkilenir. Depolama geçişinden sonra VM çoğaltmayı devre dışı bırakıp etkinleştirmeniz gerekir. |
| Ağ |Sanal makineler ve web/çalışan rolleri içeren sanal ağlar |Bu şu anda desteklenmiyor. Lütfen geçiş yapmadan önce Web/İşçi rollerini kendi Sanal Ağlarına taşıyın. Klasik Sanal Ağ geçirildikten sonra, geçirilen Azure Kaynak Yöneticisi Sanal Ağ, öncekiyle benzer yapılandırmayı elde etmek için klasik Sanal Ağ ile eşlenebilir.|
| Ağ | Klasik Ekspres Rota devreleri |Bu şu anda desteklenmiyor. Bu devrelerin IaaS geçişine başlamadan önce Azure Kaynak Yöneticisi'ne geçirilmesi gerekir. Daha fazla bilgi için, [klasikten Kaynak Yöneticisi dağıtım modeline Taşıma ExpressRoute devreleri'ne](../articles/expressroute/expressroute-move.md)bakın.|
| Azure App Service |Uygulama Hizmeti ortamları içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure HDInsight |HDInsight hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Microsoft Dynamics Yaşam Döngüsü Hizmetleri |Dynamics Lifecycle Services tarafından yönetilen sanal makineler içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure AD Domain Services |Azure AD Etki Alanı hizmetleri içeren sanal ağlar |Bu şu anda desteklenmiyor. |
| Azure API Management |Azure API Yönetimi dağıtımları içeren sanal ağlar |Bu şu anda desteklenmiyor. IaaS VNET'i geçirmek için, kesinti işlemi olmayan API Yönetimi dağıtımının VNET'ini değiştirin. |
