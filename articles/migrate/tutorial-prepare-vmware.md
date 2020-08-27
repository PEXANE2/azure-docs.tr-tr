---
title: Azure geçişi ile VMware VM 'lerini değerlendirme/geçiş için hazırlama
description: Azure geçişi ile VMware VM 'lerinin değerlendirmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 06/08/2020
ms.custom: mvc
ms.openlocfilehash: 8d4d6ac1149c397442a8ca7dd01f46f04ffc89b4
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88927315"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware VM'lerini değerlendirme ve Azure'a geçiş için hazırlama

Bu makale, [Azure geçişi](migrate-services-overview.md)'ni kullanarak şirket Içi VMware VM 'lerinin Azure 'a değerlendirmesi ve geçişine hazırlanmanıza yardımcı olur.

Bu öğretici, VMware VM 'lerini nasıl değerlendirmekte ve geçirebileceğiniz gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u Azure geçişi ile çalışacak şekilde hazırlayın.
> * Azure geçişi: Sunucu değerlendirmesi aracı ile VMware VM 'lerini değerlendirmeye hazırlayın.
> * VMware VM 'lerini Azure geçişi: sunucu geçiş aracı ile geçirmeye hazırlanın. 

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Bu, hızlı bir kavram kanıtı olarak faydalıdır. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

Tablo, Azure 'da gerçekleştirmeniz gereken görevleri özetler. Her bir görevle ilgili yönergeler tabloyu izler.

**Görev** | **Ayrıntılar** | **İzinler**
--- | --- | ---
**Azure geçişi projesi oluşturma** | Azure geçişi projesi, Azure geçiş araçları, Microsoft araçları ve üçüncü taraf teklifleriyle değerlendirmeleri ve geçişleri yönetmek ve yönetmek için merkezi bir konum sağlar. | Azure hesabınız, projenin bulunduğu kaynak grubunda katkıda bulunan veya sahip izinlerine ihtiyaç duyuyor.
**Gereci Kaydet** | Azure geçişi, VM 'Leri keşfetmek, bunları sunucu değerlendirmesi aracı ile değerlendirmek ve sunucu geçiş aracı ile aracısız geçiş kullanarak geçirmek için basit bir Azure geçiş gereci kullanır. Kayıt hakkında [daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration) . | Gereci kaydettirmek için Azure hesabınızın Azure aboneliğinde katkıda bulunan veya sahip izinlerinin olması gerekir.
**Azure AD uygulamaları oluşturma** | Bir gereç kaydedilirken Azure geçişi, Azure iki Active Directory (Azure AD) uygulaması oluşturur. <br/><br/> -İlk uygulama, Gereç ve Azure geçişi üzerinde çalışan aracılar arasındaki iletişim için kullanılır. <br/><br/> -İkinci uygulama, yalnızca kullanıcının aracısız VMware VM geçişine yönelik abonelikte oluşturulan anahtar kasasına erişmek için kullanılır.   | Azure hesabınız, Azure AD uygulamaları oluşturmak için bu [izinlere](https://docs.microsoft.com/azure/migrate/tutorial-prepare-vmware#assign-permissions-to-create-azure-ad-apps) ihtiyaç duyuyor.
**Anahtar kasası oluşturma** | -İlk Key Vault, Gereç kaydının bir parçası olarak oluşturulur ve yapılandırması sırasında gereç üzerinde indirilen sertifikanın yönetimi için kullanılır. <br/><br/> -Azure geçişi, aracısız geçiş kullanarak VMware VM 'lerini geçirmek için, erişim anahtarlarını aboneliğinizdeki çoğaltma hesabına yönetmek üzere başka bir Key Vault oluşturur.| Azure geçişi 'nin Key Vault oluşturmasına izin vermek için, Azure geçişi projesinin bulunduğu kaynak grubunda izinleri (sahip veya katkıda bulunan ve Kullanıcı erişimi Yöneticisi) ayarlarsınız.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

### <a name="assign-permissions-to-create-azure-ad-apps"></a>Azure AD uygulamaları oluşturmak için izin atama

Gereci kaydettirmek için, Azure hesabınızın Azure AD uygulamaları oluşturmak için gereken izinlere ihtiyacı vardır. Aşağıdaki yöntemlerden birini kullanarak izinleri atayın:

- **Yöntem 1: hesaba Izin verme**: Kiracı/Genel yönetici, Azure AD uygulamaları oluşturmak ve kaydettirmek için Kiracıdaki Kullanıcı hesaplarına izin verebilir.
- **Yöntem 2: bir kullanıcı hesabına yönelik izinlere sahip bir rol atama**: Kiracı/Genel yönetici, uygulama geliştirici rolünü (gerekli izinlere sahip) Kullanıcı hesabına atayabilir.

> [!NOTE]
> Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="method-1-grant-permissions-to-the-account"></a>Yöntem 1: hesaba izin verme

Hesaba aşağıdaki şekilde izin verin:

1. Kiracı veya genel yönetici olduğunuzdan emin olun. Ardından, Azure AD 'de **Azure Active Directory**  >  **kullanıcıları**  >  **Kullanıcı ayarları**' na gidin.
2. **Uygulama kayıtları** **Evet**olarak ayarlayın. Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](../active-directory/develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)



#### <a name="method-2-assign-application-developer-role"></a>Yöntem 2: uygulama geliştirici rolü atama

Alternatif olarak, kiracı/genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. Rol atama hakkında [daha fazla bilgi edinin](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md) .

### <a name="assign-permissions-to-create-a-key-vault"></a>Key Vault oluşturmak için izin atama

Key Vault oluşturmak için Azure geçişi 'ni etkinleştirmek üzere izinleri aşağıdaki gibi atayın:

1. Azure portal içindeki kaynak grubunda, **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.

    - Sunucu değerlendirmesini çalıştırmak için, **katkıda bulunan** izinleri yeterlidir.
    - Aracısız sunucu geçişini çalıştırmak için **sahip** (veya **katkıda bulunan** ve **Kullanıcı erişimi Yöneticisi**) izinlerinizin olması gerekir.

3. Gerekli izinleriniz yoksa, bunları kaynak grubu sahibinden isteyin.

## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlanma

VMware VM değerlendirmesi için hazırlanmak üzere şunları yapmanız gerekir:

1. **VMware ayarlarını doğrulayın**. Geçirmek istediğiniz vCenter Server ve VM 'Lerin gereksinimleri karşıladığından emin olun.
2. **Değerlendirme için Izinleri ayarlayın**. Azure geçişi, VM 'Leri keşfetmek ve değerlendirmek için vCenter Server erişmek üzere bir vCenter hesabı kullanır.
3. **Gereç gereksinimlerini doğrulayın**. Sonraki öğreticide dağıtmadan önce Azure geçişi gereci için dağıtım gereksinimlerini doğrulayın.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulama

1. Değerlendirme için [VMware gereksinimlerini denetleyin](migrate-support-matrix-vmware.md#vmware-requirements) .
2. İhtiyacınız olan bağlantı noktalarının vCenter Server açık [olduğundan emin olun](migrate-support-matrix-vmware.md#port-access-requirements) .
3. VCenter Server, hesabınızın bir OVA dosyası kullanarak VM oluşturma izinlerine sahip olup olmadığını denetleyin. Bu, Azure geçişi gereci bir VMware sanal makinesi olarak bir OVA dosyası kullanarak dağıtırken gereklidir.
4. Azure 'a geçirmeden önce VM 'lerde gereken bazı değişiklikler vardır.

    - Bazı işletim sistemleri için Azure geçişi bu değişiklikleri otomatik olarak yapar. 
    - Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.
    - Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.


### <a name="set-up-permissions-for-assessment"></a>Değerlendirme için izinleri ayarlama

Azure geçişi 'nin değerlendirme ve aracısız geçiş için VM 'Leri bulabilmesi için Azure geçişi 'nin vCenter Server erişmesi gerekir. Bir hesabı aşağıdaki şekilde ayarlayın:

1. VSphere Web istemcisinde, **Yönetim**  >  **erişimi**  >  **SSO kullanıcıları ve grupları**' nı açın.
2. **Kullanıcılar**' da, **Yeni Kullanıcı** simgesine tıklayın.
3. Yeni Kullanıcı ayrıntılarına yazın.
4. Tablo değerlerine uygun olarak izinleri seçin.

    **Özellik** | **Hesap izinleri**
    --- | ---
    [VM 'Leri değerlendirme](tutorial-assess-vmware.md) | Değerlendirme için hesabın salt okuma erişimi olması gerekir.
    [VM uygulamalarını, rolleri ve özellikleri bulma](how-to-discover-applications.md) | Uygulama bulma 'yı kullanmak istiyorsanız, değerlendirme için kullanılan salt okuma hesabına **sanal makineler**  >  **Konuk işlemleri**için etkinleştirilmiş ayrıcalıkları gerekir.
    [VM 'lerde bağımlılıkları çözümleme (aracısız)](how-to-create-group-machine-dependencies-agentless.md) | Bağımlılıkları çözümlemek istiyorsanız, değerlendirme için kullanılan salt okuma hesabının, **sanal makineler**  >  **Konuk işlemleri**için etkinleştirilmiş ayrıcalıkları olması gerekir.
    
> [!NOTE]
> Değerlendirme için VM bulmayı belirli bir kapsama sınırlamak istiyorsanız [Bu makaleyi](set-discovery-scope.md#assign-a-role-for-assessment)gözden geçirin.

### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için gereç ayarlarını doğrulama

Bir [sonraki öğreticide](tutorial-assess-vmware.md) Azure geçişi gereci ayarlamaya ve değerlendirmeye başlamaya başlamanızı sağlayabilirsiniz. Bunu yapmadan önce, Gereç gereksinimlerini aşağıdaki şekilde gözden geçirin: 

1. Azure geçişi gerecini dağıtmaya yönelik [gereksinimleri gözden geçirin](migrate-appliance.md#appliance---vmware) .
2. Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken Azure URL 'lerini gözden geçirin.
3. Gerecin kullandığı bağlantı noktalarını [unutmayın](migrate-support-matrix-vmware.md#port-access-requirements) .
4. Bulma ve değerlendirme sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---vmware) .

## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçişine hazırlanma

[Aracısız veya aracı tabanlı geçiş](server-migrate-overview.md)kullanarak VMware VM 'leri geçirebilirsiniz. Bu bölümde aracısız geçiş için gereksinimler özetlenmektedir.

1. Aracısız geçiş kullanmak isteyip [Istemediğinize karar verin](server-migrate-overview.md#compare-migration-methods) .
2. Geçirmek istediğiniz makineler için hiper yönetici gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) .
3. Aracısız geçiş kullanarak geçirmek istediğiniz VMware VM 'lerinin gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#vm-requirements-agentless) .
4. Aracısız geçiş için Azure geçiş gereç gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#appliance-requirements-agentless) .
5. [Ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) BULUTLARı için gereken URL erişimini aklınızda yapın.
6. [Bağlantı noktası erişim](migrate-support-matrix-vmware-migration.md#port-requirements-agentless) gereksinimlerini gözden geçirin.
7. Sonraki yordamda açıklandığı gibi aracısız geçiş için izinleri ayarlayın.


### <a name="assign-permissions-to-an-account"></a>Hesaba izin atama

Azure geçişi gereci, sanal makineleri aracısız geçişe göre bulup geçirmek için vCenter Server bağlanır. Gerecin Kullanıcı hesabına ihtiyacı olan izinleri atayabilir veya izinlerle bir rol oluşturabilir ve bu rolü bir kullanıcı hesabına atayabilirsiniz.

1. VSphere Web istemcisinde, **Yönetim**  >  **erişimi**  >  **SSO kullanıcıları ve grupları**' nı açın.
2. **Kullanıcılar**' da, **Yeni Kullanıcı** simgesine tıklayın.
3. Yeni Kullanıcı ayrıntılarına yazın.
4. [Bu izinleri](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) ata

#### <a name="create-a-role-and-assign-to-an-account"></a>Rol oluşturma ve hesaba atama

Alternatif olarak, bir hesap oluşturabilirsiniz. Ardından bir rol oluşturun ve hesabı aşağıdaki gibi hesaba atayın:

1. vCenter Server yönetici olarak Sphere Web Istemcisinde oturum açın
2. **Rol oluştur**> vCenter Server örneğini seçin.
3. Rol adı belirtin, örneğin <em>Azure_Migrate</em>ve [gerekli izinleri](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) role atayın.

    ![vCenter Server hesabı ayrıcalıkları](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

4. Şimdi bir hesap oluşturun ve rolü hesaba atayın.

> [!NOTE]
> VM bulmayı aracısız geçiş için belirli bir kapsama sınırlamak istiyorsanız [Bu makaleyi](set-discovery-scope.md#assign-a-role-for-agentless-migration)gözden geçirin.



## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçişine hazırlanma

[Aracısız veya aracı tabanlı geçiş](server-migrate-overview.md)kullanarak VMware VM 'leri geçirebilirsiniz. Bu bölüm, aracı tabanlı geçişe yönelik gereksinimleri özetler.

1. Aracı tabanlı geçiş kullanmak isteyip [Istemediğinize karar verin](server-migrate-overview.md#compare-migration-methods) .
1. Geçirmek istediğiniz makineler için hiper yönetici gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware-migration.md#vmware-requirements-agent-based) .
2. [İnceleme](migrate-support-matrix-vmware-migration.md#vm-requirements-agent-based) Geçirmek istediğiniz her VM 'ye Mobility hizmetinin yüklenmesi dahil olmak üzere VMware VM gereksinimleri.
3. Aracı tabanlı geçiş bir çoğaltma gereci kullanır:
    - Çoğaltma gereci için dağıtım gereksinimlerini [gözden geçirin](migrate-replication-appliance.md#appliance-requirements) .
    - Gereçte MySQL yükleme [seçeneklerini gözden geçirin](migrate-replication-appliance.md#mysql-installation) .
    - [Ortak](migrate-replication-appliance.md#url-access) ve [kamu](migrate-replication-appliance.md#azure-government-url-access) BULUTLARı için gereken URL erişimini aklınızda yapın.
    - Çoğaltma gereci için [bağlantı noktası erişim](migrate-replication-appliance.md#port-access) gereksinimlerini gözden geçirin.
4. Azure 'a geçirebilmeniz için önce VM 'lerde gereken bazı değişiklikler vardır. Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure izinlerini ayarlayın.
> * Değerlendirme ve geçiş için VMware hazırlandı.


Azure geçişi projesi ayarlamak ve Azure 'a geçiş için VMware VM 'lerini değerlendirmek için ikinci öğreticiye devam edin.

> [!div class="nextstepaction"]
> [VMware VM 'lerini değerlendir](./tutorial-assess-vmware.md)
