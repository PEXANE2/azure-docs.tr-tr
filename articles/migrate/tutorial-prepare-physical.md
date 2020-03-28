---
title: Azure Geçişi ile değerlendirme/geçiş için fiziksel sunucular hazırlama
description: Azure Geçişi ile fiziksel sunucuların değerlendirilmesi/geçişi için nasıl hazırlanacağınızı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5f9048b08b3e77a0c8d5ae9a9d10c614a4e0af61
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80336694"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Fiziksel sunucuların değerlendirilmesine ve Azure'a geçişine hazırlanın

Bu makalede, [Azure Geçiş](migrate-services-overview.md)ile şirket içi fiziksel sunucuların değerlendirilmesi için nasıl hazırlanacağı açıklanmaktadır.

[Azure Geçiş,](migrate-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 

Bu öğretici, Azure Geçiş ile fiziksel sunucuları nasıl değerlendiracağınızı gösteren bir serinin ilk isidir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure'u hazırlayın. Azure hesabınız için izinler ve Azure Geçiş ile çalışmak için kaynaklar ayarlayın.
> * Sunucu değerlendirmesi için şirket içi fiziksel sunucular hazırlayın.


> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için fiziksel sunucu değerlendirmesi için Nasıl Yap'ı gözden geçirin.


Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

### <a name="azure-permissions"></a>Azure izinleri

Azure Geçiş dağıtımı için izin ayarlamanız gerekir.

**Görev** | **Şey** 
--- | --- 
**Azure Geçiş projesi oluşturma** | Azure hesabınız, proje oluşturmak için Contributer veya Owner izinlerine ihtiyaç duyar. | 
**Kaynak sağlayıcılar kaydedin** | Azure Geçir, Azure Geçir Sunucu Değerlendirmesi ile Hyper-V V'leri keşfetmek ve değerlendirmek için hafif bir Azure Geçir cihazı kullanır.<br/><br/> Cihaz kaydı sırasında, kaynak sağlayıcılar cihazda seçilen aboneye kaydedilir. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Kaynak sağlayıcılarını kaydetmek için abonelikte Bir Katılımcı veya Sahip rolüne ihtiyacınız var.
**Azure AD uygulaması oluşturma** | Azure Geçir, cihazı kaydederken, cihazüzerinde çalışan aracılar ile Azure'da çalışan ilgili hizmetleri arasında iletişim kurmak için kullanılan bir Azure Etkin Dizin (Azure AD) uygulaması oluşturur. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD uygulamaları (Uygulama Geliştiricisi'nde kullanılabilir) rolü oluşturmak için izinlere ihtiyacınız vardır.



### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

Bir Azure Geçiş projesi oluşturmak için izinlere sahip olup olduğunuzu denetleyin.

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.
3. **Katkıda Bulunan** veya **Sahip** İzni niz olmalıdır.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.


### <a name="assign-permissions-to-register-the-appliance"></a>Cihazı kaydettirmek için izin ler atama

Aşağıdaki yöntemlerden birini kullanarak, cihaz kaydı sırasında Azure AD uygulamasını oluşturmak için Azure Geçiş izni atayabilirsiniz:

- Bir kiracı/global yönetici, Azure AD uygulamaları oluşturmak ve kaydetmek için kiracıdaki kullanıcılara izin verebilir.
- Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü (izinlere sahip) hesaba atayabilir.

> [!NOTE]
> - Uygulamanın abonelikte yukarıda açıklananlar dışında başka erişim izinleri yoktur.
> - Bu izinlere yalnızca yeni bir cihaz kaydettirdiğinizde ihtiyacınız vardır. Cihaz kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinlerini hibe

Kiracı/global yönetici aşağıdaki gibi izinverebilir:

1. Azure AD'de, kiracı/global yönetici **Azure Etkin Dizin** > **Kullanıcıları Kullanıcı** > **Ayarları'na**gitmelidir.
2. Yönetici, **Uygulama kayıtlarını** **Evet**olarak ayarlamalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu, hassas olmayan varsayılan bir ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Uygulama Geliştiricisi rolünü atama

Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesine hazırlanın

Fiziksel sunucu değerlendirmesine hazırlanmak için fiziksel sunucu ayarlarını doğrulamanız ve cihaz dağıtımı ayarlarını doğrulamanız gerekir:

### <a name="verify-physical-server-settings"></a>Fiziksel sunucu ayarlarını doğrulama

1. Sunucu değerlendirmesi için [fiziksel sunucu gereksinimlerini](migrate-support-matrix-physical.md#physical-server-requirements) doğrulayın.
2. [Gerekli bağlantı noktalarının](migrate-support-matrix-physical.md#port-access) fiziksel sunucularda açık olduğundan emin olun.


### <a name="verify-appliance-settings"></a>Cihaz ayarlarını doğrulayın

Azure Geçir cihazını kurmadan ve bir sonraki öğreticide değerlendirmeye başlamadan önce, cihaz dağıtımına hazırlanın.

1. Fiziksel sunucular için cihaz gereksinimlerini [doğrulayın.](migrate-appliance.md#appliance---physical)
2. Cihazın erişmesi gereken Azure URL'lerini [gözden geçirin.](migrate-appliance.md#url-access)
3. Cihazın keşif ve değerlendirme sırasında toplayıp topladığını [gözden geçirin.](migrate-appliance.md#collected-data---vmware)
4. [Not](migrate-support-matrix-physical.md#port-access) bağlantı noktası erişim gereksinimleri fiziksel sunucu değerlendirmesi.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Fiziksel sunucu bulma için bir hesap ayarlama

Azure Geçir'in şirket içi sunucuları keşfetmek için izinlere ihtiyacı vardır.

- **Windows:** Keşfe eklemek istediğiniz tüm Windows sunucularında yerel bir kullanıcı hesabı ayarlayın. Kullanıcı hesabının aşağıdaki gruplara eklenmesi gerekir: - Uzaktan Yönetim Kullanıcıları - Performans Monitörü Kullanıcıları - Performans Günlüğü kullanıcıları
- **Linux:** Keşfetmek istediğiniz Linux sunucularında bir kök hesaba ihtiyacınız vardır.

## <a name="prepare-for-physical-server-migration"></a>Fiziksel sunucu geçişine hazırlanın

Fiziksel sunucuların geçişi için gereksinimleri gözden geçirin.

- Geçiş için fiziksel sunucu gereksinimlerini [gözden geçirin.](migrate-support-matrix-physical-migration.md#physical-server-requirements)
- Azure Geçişi: Sunucu Geçişi, fiziksel sunucu geçişi için bir çoğaltma sunucusu kullanır:
    - Çoğaltma cihazının dağıtım gereksinimlerini ve MySQL'i cihaza yükleme [seçeneklerini](migrate-replication-appliance.md#mysql-installation) [gözden geçirin.](migrate-replication-appliance.md#appliance-requirements)
    - Çoğaltma cihazı için [URL](migrate-replication-appliance.md#url-access) ve [bağlantı noktası] (geçiş-çoğaltma-appliance.md#port-access) erişim gereksinimlerini gözden geçirin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure hesap izinlerini ayarlayın.
> * Değerlendirme için fiziksel sunucular hazırladı.

Bir Azure Geçiş projesi oluşturmak ve Azure'a geçiş için fiziksel sunucuları değerlendirmek için bir sonraki öğreticiye devam edin

> [!div class="nextstepaction"]
> [Fiziksel sunucuları değerlendirme](./tutorial-assess-physical.md)
