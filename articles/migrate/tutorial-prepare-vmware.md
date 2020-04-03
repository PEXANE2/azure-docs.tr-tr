---
title: Azure Geçişi ile değerlendirme/geçiş için VMware VM'leri hazırlayın
description: Azure Geçişi ile VMware VM'lerinin değerlendirilmesi/geçişi için nasıl hazırlanacağınızı öğrenin.
ms.topic: tutorial
ms.date: 11/19/2019
ms.custom: mvc
ms.openlocfilehash: 2e8aa72300c840832168138015e0a01ab054f954
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619421"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware VM'lerini değerlendirme ve Azure'a geçiş için hazırlama

Bu makale, [Azure Geçişi'ni](migrate-services-overview.md)kullanarak şirket içi VMware VM'lerin Azure'a değerlendirilmesi ve/veya geçişi için hazırlanmanıza yardımcı olur.



Bu öğretici, VMware VM'leri nasıl değerlendirip geçirteceklerini gösteren bir serinin ilk imasıdır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure'u Azure Geçir ile çalışmaya hazırlayın.
> * Azure Geçir:Sunucu Değerlendirmesi aracıyla VMware'i VM değerlendirmesine hazırlayın.
> * Azure Geçiş:Sunucu Geçişi aracıyla VMware'i VM geçişi için hazırlayın. 

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Dağıtım yapmayı öğrendiğiniz zaman ve hızlı bir kavram kanıtı olarak kullanışlıdır. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

VMware VM'leri değerlendirebilmeniz veya geçirebilmeniz için bu görevler için Azure'da bu izinlere ihtiyacınız vardır.

**Görev** | **Şey** 
--- | --- 
**Azure Geçiş projesi oluşturma** | Azure hesabınız, proje oluşturmak için Katılımcı veya Sahip izinlerine ihtiyaç duyar. 
**Kaynak sağlayıcılar kaydedin** | Azure Geçir, VMware VM'leri keşfetmek ve değerlendirmek ve Azure Geçir:Sunucu Değerlendirmesi ile Azure'a geçirmek için hafif bir Azure Geçir cihazı kullanır.<br/><br/> Cihaz kaydı sırasında, kaynak sağlayıcılar cihazda seçilen aboneye kaydedilir. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Kaynak sağlayıcılarını kaydetmek için abonelikte Bir Katılımcı veya Sahip rolüne ihtiyacınız var.
**Azure AD uygulamaları oluşturma** | Azure Geçiş, cihazı kaydederken Azure Etkin Dizin (Azure AD) uygulamaları oluşturur. <br/><br/> - İlk uygulama, cihazüzerinde çalışan aracılar ile Azure'da çalışan ilgili hizmetleri arasındaki iletişim için kullanılır.<br/><br/> - İkinci uygulama yalnızca aracısız VMware VM geçişi için kullanıcının aboneliğinde oluşturulan KeyVault'a erişmek için kullanılır. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD uygulamaları (Uygulama Geliştiricisi'nde kullanılabilir) rolü oluşturmak için izinlere ihtiyacınız vardır.
**Anahtar Kasası Oluşturma** | Aracısız geçiş kullanarak VMware VM'leri geçirmek için Azure Geçiş, aboneliğinizdeki çoğaltma depolama hesabının erişim anahtarlarını yönetmek için bir Anahtar Kasası oluşturur.<br/><br/> Kasa oluşturmak için, Azure Geçiş projesinin bulunduğu kaynak grubunda rol atama izinlerine ihtiyacınız vardır.




### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.
3. **Katkıda Bulunan** veya **Sahip** İzni niz olmalıdır.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.

### <a name="assign-permissions-to-register-the-appliance"></a>Cihazı kaydettirmek için izin ler atama

Cihazı kaydettirmek için, cihaz kaydı sırasında Azure AD uygulamalarını oluşturmak için Azure Geçiş izni atarsınız. İzinler aşağıdaki yöntemlerden biri kullanılarak atanabilir:

- **Hibe izinleri**: Kiracı/global yönetici, Azure AD uygulamaları oluşturmak ve kaydetmek için kiracıdaki kullanıcılara izin verebilir.
- **Uygulama geliştiricisi rolünü ata:** Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü (izinlere sahip) hesaba atayabilir.

> [!NOTE]
> - Uygulamaların abonelikte yukarıda açıklananlar dışında başka erişim izinleri yoktur.
> - Bu izinlere yalnızca yeni bir cihaz kaydettirdiğinizde ihtiyacınız vardır. Cihaz kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinlerini hibe

Kiracı/global yöneticinin izin vermesini istiyorsanız, bunu aşağıdaki gibi yapın:

1. Azure AD'de, kiracı/global yönetici **Azure Etkin Dizin** > **Kullanıcıları Kullanıcı** > **Ayarları'na**gitmelidir.
2. Yönetici, **Uygulama kayıtlarını** **Evet**olarak ayarlamalıdır. Bu, hassas olmayan varsayılan bir ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Uygulama Geliştiricisi rolünü atama

Alternatif olarak, kiracı/genel yönetici Uygulama Geliştiricisi rolünü bir hesaba atayabilir. Bir rol atama hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)

### <a name="assign-permissions-to-create-a-key-vault"></a>Anahtar Kasası oluşturmak için izin atama

Azure Geçiş'in bir Anahtar Kasası oluşturmasını sağlamak için izinleri aşağıdaki gibi atayın:

1. Azure portalındaki kaynak grubunda **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.

    - Sunucu değerlendirmesini çalıştırmak için **Katılımcı** izinleri yeterlidir.
    - Aracısız sunucu geçişini çalıştırmak için **Sahibi** (veya **Katılımcı** ve **Kullanıcı Erişim Yöneticisi)** izinlerine sahip olmalısınız.

3. Gerekli izinlere sahip değilseniz, bunları kaynak grubu sahibinden isteyin.



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM değerlendirmesine hazırlanın

VMware VM değerlendirmesine hazırlanmak için şunları yapmanız gerekir:

- **VMware ayarlarını doğrulayın.** Geçirmek istediğiniz vCenter Server ve VM'lerin gereksinimleri karşıladığından emin olun.
- **Değerlendirme için bir hesap ayarlayın.** Azure Geçir, vCenter Server'a erişmek ve değerlendirme için VM'leri keşfetmek için bu hesabı kullanır.
- **Cihaz gereksinimlerini doğrulayın.** Azure Geçiş cihazının dağıtım gereksinimlerini dağıtmadan önce doğrulayın.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulayın

1. [Kontrol et](migrate-support-matrix-vmware.md#vmware-requirements) Değerlendirme için VMware sunucu gereksinimleri.
2. İhtiyacınız olan bağlantı noktalarının vCenter Server'da açık [olduğundan emin olun.](migrate-support-matrix-vmware.md#port-access)
3. vCenter Server'da, hesabınızın BIR OVA dosyası kullanarak VM oluşturma izinlerine sahip olduğundan emin olun. Azure Geçir cihazını BIR OVA dosyası kullanarak VMware VM olarak dağıtMış olursunuz.


### <a name="set-up-an-account-for-assessment"></a>Değerlendirme için bir hesap ayarlama

Azure Geçir'in değerlendirme ve aracısız geçiş için VM'leri bulmak için vCenter Server'a erişmesi gerekir.

- Uygulamaları keşfetmeyi veya aracısız bir şekilde bağımlılığı görselleştirmeyi planlıyorsanız, **Sanal makineler** > **Konuk İşlemleri**için etkinleştirilen ayrıcalıklarla birlikte salt okunur erişime sahip bir vCenter Server hesabı oluşturun.

  ![vCenter Server hesap ayrıcalıkları](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Uygulama bulma ve aracısız bağımlılık görselleştirmesi yapmayı planlamamanız, vCenter Server için salt okunur hesabı ayarlayın.

### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için cihaz ayarlarını doğrulayın

Azure Geçir cihazını kurmadan ve bir sonraki öğreticide değerlendirmeye başlamadan önce, cihaz dağıtımına hazırlanın.

1. [Doğrula](migrate-appliance.md#appliance---vmware) Azure Geçiş cihazı gereksinimleri.
2. Cihazın erişmesi gereken Azure URL'lerini [gözden geçirin.](migrate-appliance.md#url-access) URL tabanlı bir güvenlik duvarı veya proxy kullanıyorsanız, gerekli URL'lere erişim sağladığından emin olun.
3. Cihazın keşif ve değerlendirme sırasında topladığı [verileri gözden geçirin.](migrate-appliance.md#collected-data---vmware)
4. Cihaz için bağlantı noktası erişim gereksinimlerini [not](migrate-support-matrix-vmware.md#port-access) edin.




## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçişine hazırlanın

VMware VM'lerin [aracısız geçişi](server-migrate-overview.md) için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) VMware sunucu gereksinimleri.
2. Azure Geçir'in vCenter Server'a erişmek için ihtiyaç duyduğu [izinleri gözden geçirin.](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers)
3. [İnceleme](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) VMware VM gereksinimleri.
4. Azure Geçiş cihaz gereksinimlerini [gözden geçirin.](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance)
5. URL [erişimi](migrate-appliance.md#url-access) ve [bağlantı noktası erişim](migrate-support-matrix-vmware-migration.md#agentless-ports) gereksinimlerine dikkat edin.

## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçişine hazırlanın

VMware VM'lerin [aracı tabanlı geçişi](server-migrate-overview.md) için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) VMware sunucu gereksinimleri.
2. [İzinleri gözden geçirin](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) Azure Geçir'in vCenter Server'a erişmesi gerekir.
2. [İnceleme](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) Geçiş yapmak istediğiniz her VM'ye Mobilite hizmetinin yüklenmesi de dahil olmak üzere VMware VM gereksinimleri.
3. Aracı tabanlı geçiş bir çoğaltma cihazı kullanır:
    - Çoğaltma cihazı için dağıtım gereksinimlerini [gözden geçirin.](migrate-replication-appliance.md#appliance-requirements)
    - Cihaza MySQL yükleme [seçeneklerini gözden geçirin.](migrate-replication-appliance.md#mysql-installation)
    - Çoğaltma cihazının [URL](migrate-replication-appliance.md#url-access) ve [bağlantı noktası](migrate-replication-appliance.md#port-access) erişim gereksinimlerini gözden geçirin.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure izinlerini ayarlayın.
> * Değerlendirme ve geçiş için VMware hazırlanmıştır.


Bir Azure Geçir projesi ayarlamak ve Azure'a geçiş için VMware VM'leri değerlendirmek için ikinci öğreticiye devam edin.

> [!div class="nextstepaction"]
> [VMware VM'leri değerlendirin](./tutorial-assess-vmware.md)
