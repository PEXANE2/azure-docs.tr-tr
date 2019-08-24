---
title: Azure geçişi ile Azure 'a değerlendirme ve geçiş için VMware VM 'lerini hazırlayın | Microsoft Docs
description: Azure geçişi kullanılarak şirket içi VMware VM 'lerinin Azure 'a değerlendirmesi ve geçirilmesi için nasıl hazırlanılacağı açıklanmaktadır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5bd4926494e13c1dd0cfb6283292ade967471238
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013428"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>VMware VM 'lerini değerlendirme ve Azure 'a geçiş için hazırlama

Bu makalede, [Azure geçişi](migrate-services-overview.md)kullanılarak şirket Içi VMware VM 'lerinin Azure 'a değerlendirmesi ve geçirilmesi için hazırlanma açıklanır.

[Azure geçişi](migrate-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 


Bu öğretici, VMware VM 'lerini nasıl değerlendirmekte ve geçirebileceğiniz gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u hazırlayın. Azure hesabınız ve kaynaklarınızın Azure geçişi ile çalışması için izinleri ayarlayın.
> * VM değerlendirmesi için şirket içi VMware sunucularını ve VM 'Leri hazırlayın.
> * VM geçişi için şirket içi VMware sunucularını ve VM 'Leri hazırlayın.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için, VMware değerlendirmesi ve geçişi konusundaki yönergeleri gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

Şu Azure izinlerinin olması gerekir:

- Azure hesabınızın değerlendirme ve geçiş için bir Azure geçişi projesi oluşturmak üzere izinleri olması gerekir. 
- VMware VM 'lerinin değerlendirmesi ve aracısız geçişi için Azure geçişi, VM 'Leri bulan bir basit gereç çalıştırır ve Azure geçişi 'ne VM meta verilerini ve performans verilerini gönderir. Azure 'da Azure geçişi gereci kaydetme izinlerine sahip olmanız gerekir.
- Azure geçişi, Azure geçiş sunucusu geçişini kullanarak VMware VM 'lerini geçirmek için, erişim anahtarlarını aboneliğinizdeki çoğaltma depolama hesabına yönetmek üzere kaynak grubunda bir Key Vault oluşturur. Kasayı oluşturmak için Azure geçişi projesinin bulunduğu kaynak grubunda rol atama izinlerine sahip olmanız gerekir. 


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

### <a name="assign-permissions-to-register-the-appliance"></a>Gereci kaydetmek için izin atama

VM 'lerin aracısız geçişini değerlendirmek veya çalıştırmak için Azure geçiş gereci dağıtıyorsanız, kaydetmeniz gerekir.

- Gereç kaydı sırasında Azure geçişi, gereci benzersiz bir şekilde tanımlayan iki Azure Active Directory (Azure AD) uygulaması oluşturur
    - İlk uygulama Azure geçişi hizmet uç noktaları ile iletişim kurar.
    - İkinci uygulama kayıt sırasında oluşturulan ve Azure AD uygulama bilgileri ve gereç yapılandırma ayarlarını depolamak için bir Azure Key Vault erişir.
- Aşağıdaki yöntemlerden birini kullanarak, bu Azure AD uygulamalarını oluşturmak için Azure geçişi için izinler atayabilirsiniz:
    - Kiracı/Genel yönetici, Kiracıdaki kullanıcılara Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
    - Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

Buna dikkat edin:

- Uygulamalar, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinlerine sahip değildir.
- Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz. 


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yönetici, izinleri aşağıdaki gibi verebilir

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory** >  > Kullanıcı**Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama 

Kiracı/Genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-role-assignment-permissions"></a>Rol atama izinleri atama

Azure geçişi projesinin bulunduğu kaynak grubunda rol atama izinlerini aşağıdaki gibi atayın:

1. Azure portal içindeki kaynak grubunda, **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.

    - Sunucu değerlendirmesini çalıştırmak için, **katkıda bulunan** izinleri yeterlidir.
    - Aracısız sunucu geçişini çalıştırmak için **sahip** (veya **katkıda bulunan** ve **Kullanıcı erişimi Yöneticisi**) izinlerinizin olması gerekir.

3. Gerekli izinleriniz yoksa, bunları kaynak grubu sahibinden isteyin. 



## <a name="prepare-for-vmware-vm-assessment"></a>VMware VM değerlendirmesi için hazırlanma

VMware VM değerlendirmesi için hazırlanmak üzere VMware konak ve VM ayarlarını doğrulamanız ve gereç dağıtımı için ayarları doğrulamanız gerekir.

### <a name="verify-vmware-settings"></a>VMware ayarlarını doğrulama

1. [Doğrula](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) VM değerlendirmesi için VMware sunucusu gereksinimleri.
2. VCenter sunucularında gerekli bağlantı noktalarının açık [olduğundan emin olun](migrate-support-matrix-vmware.md#assessment-port-requirements) .


### <a name="set-up-an-account-for-assessment"></a>Değerlendirme için bir hesap ayarlayın

Azure geçişi 'nin değerlendirme ve aracısız geçiş için VM 'Leri bulması için vCenter Server erişmesi gerekir. Yalnızca değerlendirme için, vCenter Server için salt okuma hesabına sahip olmanız gerekir.

URL tabanlı bir güvenlik duvarı. Proxy kullanıyorsanız, gerekli [Azure URL 'lerine](migrate-support-matrix-vmware.md#assessment-url-access-requirements)erişime izin verin.

Proxy 'nin, URL 'Leri ararken alınan CNAME kayıtlarını çözümlediği emin olun.


### <a name="verify-appliance-settings-for-assessment"></a>Değerlendirme için gereç ayarlarını doğrulama

Sonraki öğreticide Azure geçişi gerecini ve değerlendirmeyi ayarlamadan önce, Gereç dağıtımına hazırlanın.

1. VMware 'de Azure geçişi gerecini ayarlamaya yönelik gereksinimleri [doğrulayın](migrate-support-matrix-vmware.md#assessment-appliance-requirements) .
2. Gerecin erişmesi gereken Azure URL 'Lerini [gözden geçirin](migrate-support-matrix-vmware.md#assessment-url-access-requirements) .
3. Bulma ve değerlendirme sırasında gereç tarafından toplanacak verileri gözden geçirin.
4. [](migrate-support-matrix-vmware.md#assessment-port-requirements) Gereç için bağlantı noktası erişim gereksinimleri.
5. Azure geçişi gereci bir OVA dosyası kullanarak bir VMware VM 'si olarak dağıtırsınız. VCenter Server, hesabınızın bir OVA dosyası kullanarak VM oluşturma izinlerine sahip olduğundan emin olun.


## <a name="prepare-for-agentless-vmware-migration"></a>Aracısız VMware geçişine hazırlanma

VMware VM 'lerinin aracısız geçişi için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) Aracısız geçiş için VMware sunucusu gereksinimleri.
2. Aracısız geçiş için [gerekli izinlere](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) sahip vCenter Server erişmek üzere bir hesap ayarlayın.
3. Aracısız geçiş kullanarak Azure 'a geçirmek istediğiniz VMware VM 'lerinin gereksinimlerini [unutmayın](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) .
4. Aracısız geçiş için gereç gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) .
5. Aracısız geçiş için gereç [URL 'si erişimi](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) ve [bağlantı noktası erişim](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) gereksinimleri.


## <a name="prepare-for-agent-based-vmware-migration"></a>Aracı tabanlı VMware geçişine hazırlanma

VMware VM 'lerinin aracı tabanlı geçişi için gereksinimleri gözden geçirin.

1. [İnceleme](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Aracısız geçiş için VMware sunucusu gereksinimleri. 
2. Aracısız geçiş için [gerekli izinlere](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) sahip vCenter Server erişmek üzere bir hesap ayarlayın.
3. Geçirmek istediğiniz her VM 'ye Mobility hizmetinin yüklenmesi dahil olmak üzere, Azure 'a geçiş yapmak istediğiniz VMware VM 'lerinin gereksinimlerini [unutmayın](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) .
4. [URL erişimi](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Her VM 'de çalışan Mobility hizmeti ve Azure geçişi yapılandırma sunucusu için [bağlantı noktası erişim](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements) gereksinimlerini gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:
 
> [!div class="checklist"] 
> * Azure izinlerini ayarlayın.
> * Değerlendirme ve geçiş için VMware hazırlandı.


Azure geçişi projesi ayarlamak ve Azure 'a geçiş için VMware VM 'lerini değerlendirmek için ikinci öğreticiye devam edin.

> [!div class="nextstepaction"] 
> [VMware VM 'lerini değerlendir](./tutorial-assess-vmware.md) 

