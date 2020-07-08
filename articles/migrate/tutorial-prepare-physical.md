---
title: Azure geçişi ile değerlendirme/geçiş için fiziksel sunucuları hazırlama
description: Azure geçişi ile fiziksel sunucuların değerlendirilmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 31db91b512a4532cca144dc012282ea58a87514f
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86113260"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Fiziksel sunucuların değerlendirmesi ve Azure 'a geçirilmesi için hazırlanma

Bu makalede, [Azure geçişi](migrate-services-overview.md)ile şirket içi fiziksel sunucu değerlendirmesi için nasıl hazırlanılacağı açıklanır.

[Azure geçişi](./migrate-services-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 

Bu öğretici, Azure geçişi ile fiziksel sunucuları nasıl değerlendirmenizi gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u hazırlayın. Azure hesabınız ve kaynaklarınızın Azure geçişi ile çalışması için izinleri ayarlayın.
> * Sunucu değerlendirmesi için şirket içi fiziksel sunucuları hazırlayın.


> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için, fiziksel sunucu değerlendirmesi için nasıl yapılır konusunu gözden geçirin.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure-for-server-assessment"></a>Sunucu değerlendirmesi için Azure 'u hazırlama

Azure 'ı Azure geçişi ile çalışacak şekilde ayarlayın. 

**Görev** | **Ayrıntılar** 
--- | --- 
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için katkıda bulunan veya sahip izinlerinin olması gerekir. 
**Kaynak sağlayıcılarını kaydetme (yalnızca değerlendirme)** | Azure geçişi, Azure geçişi: Sunucu değerlendirmesi ile makineler bulma ve değerlendirme için basit bir Azure geçiş gereci kullanır.<br/><br/> Gereç kaydı sırasında kaynak sağlayıcıları, Gereç içinde seçilen aboneliğe kaydedilir. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Kaynak sağlayıcılarını kaydetmek için abonelikte bir katkıda bulunan veya sahip rolü gerekir.
**Azure AD uygulaması oluşturma (yalnızca değerlendirme)** | Gereci kaydederken Azure geçişi, Gereç üzerinde çalışan aracılar ile Azure üzerinde çalışan hizmetlerle ilgili iletişim için kullanılan bir Azure Active Directory (Azure AD) uygulaması oluşturur. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD uygulamaları (uygulama geliştiricisi 'nde bulunur) rolünde oluşturma izinlerine sahip olmanız gerekir.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama 

Azure geçişi projesi oluşturma izniniz olup olmadığını denetleyin.

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.


### <a name="assign-permissions-to-register-the-appliance"></a>Gereci kaydetmek için izin atama 

Aşağıdaki yöntemlerden birini kullanarak, Gereç kaydı sırasında Azure AD uygulaması oluşturmak için Azure geçişi için izinler atayabilirsiniz:

- Kiracı/Genel yönetici, Kiracıdaki kullanıcılara Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
- Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

> [!NOTE]
> - Uygulamanın, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinleri yoktur.
> - Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yönetici, izinleri aşağıdaki gibi verebilir:

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory**  >  **Kullanıcı**  >  **Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](../active-directory/develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama

Kiracı/Genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).


## <a name="prepare-azure-for-physical-server-migration"></a>Azure 'u fiziksel sunucu geçişi için hazırlama

Sunucu geçişini kullanarak Azure 'u fiziksel sunucuları geçirmeye hazırlayın.

**Görev** | **Ayrıntılar**
--- | ---
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için katılımcısı veya sahip izinlerine ihtiyacı vardır.
**Azure hesabınız için izinleri doğrulama** | Azure hesabınızın bir VM oluşturmak ve Azure yönetilen diskine yazmak için izinleri olması gerekir.
**Azure ağı oluşturma** | Azure 'da bir ağ kurun.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.


### <a name="assign-azure-account-permissions"></a>Azure hesabı izinleri atama

Sanal makine katılımcısı rolünü Azure hesabına atayın. Bu izinler şunları sağlar:
  - Seçilen kaynak grubunda sanal makine oluşturma.
  - Seçilen sanal ağda sanal makine oluşturma.
  - Azure yönetilen diskine yazın. 

### <a name="create-an-azure-network"></a>Azure ağı oluşturma

Bir Azure sanal ağı (VNet) [ayarlayın](../virtual-network/manage-virtual-network.md#create-a-virtual-network) . Azure 'a çoğaltma yaptığınızda Azure VM 'Ler oluşturulur ve geçişi ayarlarken belirttiğiniz Azure VNet 'e birleştirilir.


## <a name="prepare-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesi için hazırlanma

Fiziksel sunucu değerlendirmesi için hazırlanmak üzere, fiziksel sunucu ayarlarını doğrulamanız ve gereç dağıtımı için ayarları doğrulamanız gerekir:

### <a name="verify-physical-server-settings"></a>Fiziksel sunucu ayarlarını doğrulama

1. Sunucu değerlendirmesi için [fiziksel sunucu gereksinimlerini](migrate-support-matrix-physical.md#physical-server-requirements) doğrulayın.
2. Fiziksel sunucularda [gerekli bağlantı noktalarının](migrate-support-matrix-physical.md#port-access) açık olduğundan emin olun.


### <a name="verify-appliance-settings"></a>Gereç ayarlarını doğrulama

Sonraki öğreticide Azure geçişi gerecini ve değerlendirmeyi ayarlamadan önce, Gereç dağıtımına hazırlanın.

1. Fiziksel sunucular için gereç gereksinimlerini [doğrulayın](migrate-appliance.md#appliance---physical) .
2. Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken Azure URL 'lerini gözden geçirin.
3. Gerecin bulma ve değerlendirme sırasında toplanacağını [gözden geçirin](migrate-appliance.md#collected-data---vmware) .
4. [Note](migrate-support-matrix-physical.md#port-access) bağlantı noktası erişim gereksinimleri fiziksel sunucu değerlendirmesi.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Fiziksel sunucu keşfi için bir hesap ayarlayın

Azure geçişi 'nin şirket içi sunucuları bulması için izinleri olması gerekir.

- **Windows:** Keşfetmesini istediğiniz tüm Windows sunucularında bir etki alanı yöneticisi veya yerel yönetici olmanız gerekir. Kullanıcı hesabı şu gruplara eklenmelidir: uzak yönetim kullanıcıları, performans Izleyicisi kullanıcıları ve performans günlüğü kullanıcıları.
- **Linux:** Bulunmasını istediğiniz Linux sunucularında bir kök hesabınız olması gerekir.

## <a name="prepare-for-physical-server-migration"></a>Fiziksel sunucu geçişine hazırlanma

Fiziksel sunucuların geçirilmesi için gereksinimleri gözden geçirin.

> [!NOTE]
> Fiziksel makineleri geçirirken Azure geçişi: sunucu geçişi, Azure Site Recovery hizmetinde aracı tabanlı olağanüstü durum kurtarma ile aynı çoğaltma mimarisini kullanır ve bazı bileşenler aynı kod tabanını paylaşır. Bazı içerikler Site Recovery belgelerine bağlantı verebilir.

1. Geçiş için fiziksel sunucu gereksinimlerini [gözden geçirin](migrate-support-matrix-physical-migration.md#physical-server-requirements) .
2. Azure geçişi: sunucu geçişi fiziksel sunucu geçişi için bir çoğaltma sunucusu kullanır:
    - Çoğaltma gereci için dağıtım gereksinimlerini ve gereçte MySQL yükleme [seçeneklerini](migrate-replication-appliance.md#mysql-installation) [gözden geçirin](migrate-replication-appliance.md#appliance-requirements) .
    - Çoğaltma gerecinin ortak ve kamu bulutları 'na erişmesi için gereken [Azure URL 'lerini](migrate-appliance.md#url-access) gözden geçirin.
    - Çoğaltma gereci için [port] (Migrate-Replication-gereci. MD # Port-Access) erişim gereksinimlerini gözden geçirin.
3. Azure 'a geçirmeden önce VM 'lerde gereken bazı değişiklikler vardır.
    - Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.
    - Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure hesabı izinlerini ayarlayın.
> * Değerlendirme için fiziksel sunucular hazırlandı.

Azure geçişi projesi oluşturmak ve Azure 'a geçiş için fiziksel sunucuları değerlendirmek üzere bir sonraki öğreticiye devam edin

> [!div class="nextstepaction"]
> [Fiziksel sunucuları değerlendirme](./tutorial-assess-physical.md)
