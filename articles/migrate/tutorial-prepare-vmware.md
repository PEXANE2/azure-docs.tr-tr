---
title: Azure geçişi ile VMware VM 'lerini değerlendirme/geçiş için hazırlama
description: Azure geçişi ile VMware VM 'lerinin değerlendirmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: a3f9716d6302c41f139d9a2a8b1f994b79afd199
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81677293"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware VM'lerini değerlendirme ve Azure'a geçiş için hazırlama

Bu makale, [Azure geçişi](migrate-services-overview.md)'ni kullanarak şirket Içi VMware VM 'lerinden Azure 'a değerlendirme ve/veya geçiş için hazırlık yapmanıza yardımcı olur.



Bu öğretici, VMware VM 'lerini nasıl değerlendirmekte ve geçirebileceğiniz gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u Azure geçişi ile çalışacak şekilde hazırlayın.
> * Azure geçişi: Sunucu değerlendirmesi aracı ile VM değerlendirmesi için VMware 'yi hazırlayın.
> * Azure geçişi: sunucu geçiş aracı ile VM 'ye geçiş için VMware 'yi hazırlayın. 

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Dağıtım ve hızlı bir kavram kanıtı olarak nasıl ayarlanacağını öğrenirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

VMware VM 'lerini değerlendirebilmeniz veya geçirebilmeniz için Azure 'da bu görevler için bu izinlere ihtiyacınız vardır.

**Görev** | **Bilgileri** 
--- | --- 
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için katkıda bulunan veya sahip izinlerinin olması gerekir. 
**Kaynak sağlayıcılarını kaydetme** | Azure geçişi, VMware VM 'lerini bulup değerlendirmek ve Azure geçişi: Sunucu değerlendirmesi ile Azure 'a geçirmek için basit bir Azure geçiş gereci kullanır.<br/><br/> Gereç kaydı sırasında kaynak sağlayıcıları, Gereç içinde seçilen aboneliğe kaydedilir. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Kaynak sağlayıcılarını kaydetmek için abonelikte bir katkıda bulunan veya sahip rolü gerekir.
**Azure AD uygulamaları oluşturma** | Gereci kaydederken Azure geçişi Azure Active Directory (Azure AD) uygulamaları oluşturur. <br/><br/> -İlk uygulama, Gereç üzerinde çalışan aracılar ve Azure üzerinde çalışan ilgili hizmetleri arasındaki iletişim için kullanılır.<br/><br/> -İkinci uygulama, yalnızca kullanıcının aracısız VMware VM geçişine yönelik abonelikte oluşturulan anahtar kasasına erişmek için kullanılır. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD uygulamaları (uygulama geliştiricisi 'nde bulunur) rolünde oluşturma izinlerine sahip olmanız gerekir.
**Anahtar kasası oluşturma** | Azure geçişi, aracısız geçiş kullanarak VMware VM 'lerini geçirmek için, erişim anahtarlarını aboneliğinizdeki çoğaltma depolama hesabına yönetmek üzere bir Key Vault oluşturur.<br/><br/> Kasayı oluşturmak için Azure geçişi projesinin bulunduğu kaynak grubunda rol atama izinlerine sahip olmanız gerekir.




### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

### <a name="assign-permissions-to-register-the-appliance"></a>Gereci kaydetmek için izin atama

Gereci kaydettirmek için, Azure geçişi 'nin gereç kaydı sırasında Azure AD uygulamalarını oluşturmak üzere izinleri atarsınız. İzinler aşağıdaki yöntemlerden biri kullanılarak atanabilir:

- **Izin verme**: Kiracı/Genel yönetici, Kiracıdaki KULLANıCıLARA Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
- **Uygulama geliştirici rolü atama**: Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

> [!NOTE]
> - Uygulamalar, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinlerine sahip değildir.
> - Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yöneticinin izin vermesini istiyorsanız, bunu aşağıdaki gibi yapın:

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory** > **Users** > Kullanıcı**Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır. Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama

Alternatif olarak, kiracı/genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. Rol atama hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) .

### <a name="assign-permissions-to-create-a-key-vault"></a>Key Vault oluşturmak için izin atama

Key Vault oluşturmak için Azure geçişi 'ni etkinleştirmek üzere izinleri aşağıdaki gibi atayın:

1. Azure portal içindeki kaynak grubunda, **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.

    - Sunucu değerlendirmesini çalıştırmak için, **katkıda bulunan** izinleri yeterlidir.
    - Aracısız sunucu geçişini çalıştırmak için **sahip** (veya **katkıda bulunan** ve **Kullanıcı erişimi Yöneticisi**) izinlerinizin olması gerekir.

3. Gerekli izinleriniz yoksa, bunları kaynak grubu sahibinden isteyin.



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM değerlendirmesi için hazırlanma

VMware VM değerlendirmesi için hazırlanmak üzere şunları yapmanız gerekir:

- **VMware ayarlarını doğrulayın**. Geçirmek istediğiniz vCenter Server ve VM 'Lerin gereksinimleri karşıladığından emin olun.
- **Değerlendirme için bir hesap ayarlayın**. Azure geçişi, değerlendirme için VM 'Leri bulmaya yönelik vCenter Server erişmek için bu hesabı kullanır.
- **Gereç gereksinimlerini doğrulayın**. Dağıtmadan önce Azure geçişi gereci için dağıtım gereksinimlerini doğrulayın.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulama

1. [Denetle](migrate-support-matrix-vmware.md#vmware-requirements) Değerlendirme için VMware sunucusu gereksinimleri.
2. İhtiyacınız olan bağlantı noktalarının vCenter Server açık [olduğundan emin olun](migrate-support-matrix-vmware.md#port-access) .
3. VCenter Server, hesabınızın bir OVA dosyası kullanarak VM oluşturma izinlerine sahip olduğundan emin olun. Azure geçişi gereci bir OVA dosyası kullanarak bir VMware VM 'si olarak dağıtırsınız.


### <a name="set-up-an-account-for-assessment"></a>Değerlendirme için bir hesap ayarlayın

Azure geçişi 'nin değerlendirme ve aracısız geçiş için VM 'Leri bulması için vCenter Server erişmesi gerekir.

- Uygulamaları bulmayı veya bağımlılığı aracısız bir şekilde görselleştirmeyi planlıyorsanız, **sanal makineler** > **Konuk işlemleri**için etkinleştirilen ayrıcalıklarla birlikte salt okuma erişimli bir vCenter Server hesabı oluşturun.

  ![vCenter Server hesabı ayrıcalıkları](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Uygulama bulmayı ve aracısız bağımlılık görselleştirmesini yapma planlandıysanız, vCenter Server için salt okunurdur bir hesap ayarlayın.

### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için gereç ayarlarını doğrulama

Sonraki öğreticide Azure geçişi gerecini ve değerlendirmeyi ayarlamadan önce, Gereç dağıtımına hazırlanın.

1. [Doğrula](migrate-appliance.md#appliance---vmware) Azure geçiş gereç gereksinimleri.
2. Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken Azure URL 'lerini gözden geçirin.
3. Bulma ve değerlendirme sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---vmware) .
4. Gereç [için bağlantı noktası](migrate-support-matrix-vmware.md#port-access) erişim gereksinimleri.




## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçişine hazırlanma

VMware VM 'lerinin [aracısız geçişi](server-migrate-overview.md) için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) VMware sunucusu gereksinimleri.
2. VCenter Server erişmek için Azure geçişi 'nin ihtiyaç duyacağı [Izinleri gözden geçirin](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) .
3. [İnceleme](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) VMware VM 'Leri gereksinimleri.
4. Azure geçişi gereç gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance) .
5. [Ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) BULUTLARı için gereken URL erişimini aklınızda yapın.
6. [Bağlantı noktası erişim](migrate-support-matrix-vmware-migration.md#agentless-ports) gereksinimlerini gözden geçirin.

## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçişine hazırlanma

VMware VM 'lerinin [aracı tabanlı geçişi](server-migrate-overview.md) için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) VMware sunucusu gereksinimleri.
2. [Izinleri gözden geçirin](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) Azure geçişi 'nin vCenter Server erişmesi gerekir.
2. [İnceleme](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) Geçirmek istediğiniz her VM 'ye Mobility hizmetinin yüklenmesi dahil olmak üzere VMware VM gereksinimleri.
3. Aracı tabanlı geçiş bir çoğaltma gereci kullanır:
    - Çoğaltma gereci için dağıtım gereksinimlerini [gözden geçirin](migrate-replication-appliance.md#appliance-requirements) .
    - Gereçte MySQL yükleme [seçeneklerini gözden geçirin](migrate-replication-appliance.md#mysql-installation) .
    - [Ortak](migrate-replication-appliance.md#url-access) ve [kamu](migrate-replication-appliance.md#azure-government-url-access) BULUTLARı için gereken URL erişimini aklınızda yapın.
    - Çoğaltma gereci için [bağlantı noktası erişim](migrate-replication-appliance.md#port-access) gereksinimlerini gözden geçirin.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure izinlerini ayarlayın.
> * Değerlendirme ve geçiş için VMware hazırlandı.


Azure geçişi projesi ayarlamak ve Azure 'a geçiş için VMware VM 'lerini değerlendirmek için ikinci öğreticiye devam edin.

> [!div class="nextstepaction"]
> [VMware VM 'lerini değerlendir](./tutorial-assess-vmware.md)
