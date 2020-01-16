---
title: Azure geçişi ile VMware VM 'lerini değerlendirme/geçiş için hazırlama
description: Azure geçişi ile VMware VM 'lerinin değerlendirmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 4dec76140f61c433561ccfea07b833d9821acfc5
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028897"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware VM 'lerini değerlendirme ve Azure 'a geçiş için hazırlama

Bu makale, [Azure geçişi](migrate-services-overview.md)'ni kullanarak şirket Içi VMware VM 'lerinden Azure 'a değerlendirme ve/veya geçiş için hazırlık yapmanıza yardımcı olur.

[Azure geçişi](migrate-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.


Bu öğretici, VMware VM 'lerini nasıl değerlendirmekte ve geçirebileceğiniz gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u Azure geçişi ile çalışacak şekilde hazırlayın.
> * VMware 'yi VM değerlendirmesi için hazırlayın.
> * VM geçişi için VMware 'yi hazırlayın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Dağıtım ve hızlı bir kavram kanıtı olarak nasıl ayarlanacağını öğrenirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için, VMware değerlendirmesi ve geçiş için nasıl yapılır konusunu gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

Bu izinlere ihtiyacınız vardır.

**Görev** | **İzinler**
--- | ---
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için izinleri olması gerekir.
**Azure geçişi gereci kaydetme** | Azure geçişi, Azure geçişi sunucu değerlendirmesi ile VMware VM 'lerini değerlendirmek ve Azure geçişi sunucu geçişi ile VMware VM 'lerinin [aracısız geçişini](server-migrate-overview.md) çalıştırmak için basit bir Azure geçiş gereci kullanır. Bu gereç VM 'Leri bulur ve Azure geçişi 'ne VM meta verilerini ve performans verilerini gönderir.<br/><br/>Kayıt sırasında Azure geçişi, gereci benzersiz bir şekilde tanımlayan ve bu uygulamaları oluşturmak için gereken izinlere sahip iki Azure Active Directory (Azure AD) uygulaması oluşturur.<br/> -İlk uygulama Azure geçişi hizmet uç noktaları ile iletişim kurar.<br/> -İkinci uygulama, Azure AD uygulama bilgileri ve gereç yapılandırma ayarlarını depolamak için kayıt sırasında oluşturulan bir Azure Key Vault erişir.
**Key Vault oluşturma** | Azure geçişi, VMware VM 'lerini Azure geçişi sunucu geçişi ile geçirmek için, erişim anahtarlarını aboneliğinizdeki çoğaltma depolama hesabına yönetmek üzere bir Key Vault oluşturur. Kasayı oluşturmak için Azure geçişi projesinin bulunduğu kaynak grubunda rol atama izinlerine sahip olmanız gerekir.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

### <a name="assign-permissions-to-register-the-appliance"></a>Gereci kaydetmek için izin atama

Gereci kaydettirmek için, Azure geçişi 'nin gereç kaydı sırasında Azure AD uygulamalarını oluşturmak üzere izinleri atarsınız. İzinler aşağıdaki yöntemlerden biri kullanılarak atanabilir:

- Kiracı/Genel yönetici, Kiracıdaki kullanıcılara Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
- Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

> [!NOTE]
> - Uygulamalar, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinlerine sahip değildir.
> - Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yönetici, izinleri aşağıdaki gibi verebilir

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory** > **Kullanıcılar** > **Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır. Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama

Kiracı/Genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

### <a name="assign-role-assignment-permissions"></a>Rol atama izinleri atama

Key Vault oluşturmak için Azure geçişi 'ni etkinleştirmek üzere rol atama izinlerini aşağıdaki gibi atayın:

1. Azure portal içindeki kaynak grubunda, **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.

    - Sunucu değerlendirmesini çalıştırmak için, **katkıda bulunan** izinleri yeterlidir.
    - Aracısız sunucu geçişini çalıştırmak için **sahip** (veya **katkıda bulunan** ve **Kullanıcı erişimi Yöneticisi**) izinlerinizin olması gerekir.

3. Gerekli izinleriniz yoksa, bunları kaynak grubu sahibinden isteyin.



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM değerlendirmesi için hazırlanma

VMware VM değerlendirmesi için hazırlanmak üzere şunları yapmanız gerekir:

- **VMware ayarlarını doğrulayın**. Geçirmek istediğiniz vCenter Server ve VM 'Lerin gereksinimleri karşıladığından emin olun.
- **Bir değerlendirme hesabı ayarlayın**. Azure geçişi 'nin değerlendirmede VM 'Leri bulması için vCenter Server erişmesi gerekir.
- **Gereç gereksinimlerini doğrulayın**. Değerlendirme için kullanılan Azure geçiş gereci için dağıtım gereksinimlerini doğrulayın.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulama

1. [Denetle](migrate-support-matrix-vmware.md#vmware-requirements) Değerlendirme için VMware sunucusu gereksinimleri.
2. İhtiyacınız olan bağlantı noktalarının vCenter Server açık [olduğundan emin olun](migrate-support-matrix-vmware.md#port-access) .
3. VCenter Server, hesabınızın bir OVA dosyası kullanarak VM oluşturma izinlerine sahip olduğundan emin olun. Azure geçişi gereci bir OVA dosyası kullanarak bir VMware VM 'si olarak dağıtırsınız.


### <a name="set-up-an-account-for-assessment"></a>Değerlendirme için bir hesap ayarlayın

Azure geçişi 'nin değerlendirme ve aracısız geçiş için VM 'Leri bulması için vCenter Server erişmesi gerekir.

- Uygulamalar bulmayı veya bağımlılığı aracısız bir şekilde görselleştirmeyi planlıyorsanız, **Konuk işlemleri** > **sanal makineler** için etkinleştirilen ayrıcalıklarla birlikte salt okuma erişimli bir vCenter Server hesabı oluşturun.

  ![vCenter Server hesabı ayrıcalıkları](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Uygulama bulmayı ve aracısız bağımlılık görselleştirmesini yapma planlandıysanız, vCenter Server için salt okunurdur bir hesap ayarlayın.

### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için gereç ayarlarını doğrulama

Sonraki öğreticide Azure geçişi gerecini ve değerlendirmeyi ayarlamadan önce, Gereç dağıtımına hazırlanın.

1. VMware VM 'Leri için gereç gereksinimlerini [doğrulayın](migrate-appliance.md#appliance---vmware) .
2. Gerecin erişmesi gereken Azure URL 'Lerini [gözden geçirin](migrate-appliance.md#url-access) . URL tabanlı bir güvenlik duvarı veya proxy kullanıyorsanız, gereken URL 'lere erişim izni verdiğinden emin olun.
3. Gerecin bulma ve değerlendirme sırasında toplanacağını [gözden geçirin](migrate-appliance.md#collected-data---vmware) .
4. Gereç [için bağlantı noktası](migrate-support-matrix-vmware.md#port-access) erişim gereksinimleri.




## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçişine hazırlanma

VMware VM 'lerinin aracısız geçişi için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) VMware sunucusu gereksinimleri ve Azure geçişi ['Nin Azure](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) geçişi sunucu geçişini kullanarak aracısız geçiş için vCenter Server erişmesi gerekir.
2. Aracısız geçiş kullanarak Azure 'a geçirmek istediğiniz VMware VM 'lerinin gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) .
4. Aracısız geçiş için Azure geçişi gereci kullanma gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance) .
5. Aracısız geçiş için gereken [URL erişimi](migrate-appliance.md#url-access) ve [bağlantı noktası erişimini](migrate-support-matrix-vmware-migration.md#agentless-ports) aklınızda yapın.


## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçişine hazırlanma

VMware VM 'lerinin [aracı tabanlı geçişi](server-migrate-overview.md) için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) VMware sunucusu gereksinimleri ve Azure geçişi 'nin Azure geçişi sunucu geçişini kullanarak aracı tabanlı geçiş için vCenter Server erişmesi gerekir.
2. Geçirmek istediğiniz her sanal makineye Mobility hizmeti yüklemesi de dahil olmak üzere, Azure 'a geçiş yapmak istediğiniz VMware VM 'lerinin gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) .
3. Aracı tabanlı geçişler bir çoğaltma gereci kullanır:
    - Çoğaltma gereci için dağıtım gereksinimlerini ve gereçte MySQL yükleme [seçeneklerini](migrate-replication-appliance.md#mysql-installation) [gözden geçirin](migrate-replication-appliance.md#appliance-requirements) .
    - Çoğaltma gereci için [URL](migrate-replication-appliance.md#url-access) ve [bağlantı noktası](migrate-replication-appliance.md#port-access) erişim gereksinimlerini gözden geçirin.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure izinlerini ayarlayın.
> * Değerlendirme ve geçiş için VMware hazırlandı.


Azure geçişi projesi ayarlamak ve Azure 'a geçiş için VMware VM 'lerini değerlendirmek için ikinci öğreticiye devam edin.

> [!div class="nextstepaction"]
> [VMware VM 'lerini değerlendir](./tutorial-assess-vmware.md)
