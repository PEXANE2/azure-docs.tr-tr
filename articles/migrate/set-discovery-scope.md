---
title: Azure geçişi ile VMware VM keşfi kapsamını ayarlama
description: Azure geçişi ile VMware VM değerlendirmesi ve geçişi için bulma kapsamının nasıl ayarlanacağını açıklar.
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 29b3077ead168cef2790468d6ac62d1c59c24c11
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80337642"
---
# <a name="set-discovery-scope-for-vmware-vms"></a>VMware VM 'Leri için bulma kapsamını ayarlama

Bu makalede, [Azure geçişi](migrate-appliance-architecture.md)gereci tarafından bulunan VMware VM 'leri için bulma kapsamının nasıl sınırlandırılacağını açıklanmaktadır.

Azure geçişi gereci, şu durumlarda şirket içi VMware VM 'lerini bulur: 

- Azure 'a geçiş için VMware VM 'lerini değerlendirmek üzere [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanın.
- VMware VM 'lerinden Azure 'a [daha az geçiş](server-migrate-overview.md) Için [Azure geçişi: sunucu geçiş](migrate-services-overview.md#azure-migrate-server-migration-tool) aracı 'nı kullanın.

## <a name="set-discovery-scope"></a>Bulma kapsamını ayarla


VMware VM 'Leri değerlendirmesi veya geçiş için Azure geçişi gereci ayarladıktan sonra, Gereç VM 'Leri bulmaya ve VM meta verilerini Azure 'a göndermeye başlar. Gereci bulma için vCenter Server 'ye bağlanmadan önce bulma kapsamını, bulmayı vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, konaklar klasörü veya ayrı VM 'Ler için sınırlamak üzere ayarlayabilirsiniz.

Kapsamı ayarlamak için, Azure geçişi 'nin vCenter Server erişmek için kullandığı hesaba izinler atarsınız.

## <a name="create-a-vcenter-user-account"></a>VCenter Kullanıcı hesabı oluşturma

Azure geçişi gerecinin VMware VM 'lerini keşfetmek, değerlendirmek ve geçirmek için kullandığı bir vCenter Kullanıcı hesabı ayarlamadıysanız, önce bunu yapın.

1.    VCenter Server Yöneticisi olarak vSphere Web Istemcisinde oturum açın.
2.    **Yönetim** > **SSO kullanıcıları ve grupları**' nı seçin ve **Kullanıcılar** sekmesine tıklayın.
3.    **Yeni Kullanıcı** simgesini seçin.
4.    **Tamam**> Yeni Kullanıcı bilgilerini girin.

Hesap izinleri, dağıtmakta olduğunuz işe bağlıdır.

**Özellik** | **Hesap izinleri**
--- | ---
[Lamayı](tutorial-assess-vmware.md)| Hesabın salt okuma erişimi olması gerekir.
[Uygulamaları/rolleri/özellikleri bulma](how-to-discover-applications.md) | Hesabın, Konuk Işlemleri > sanal makineler için etkinleştirilen ayrıcalıklarla salt okuma erişimi olması gerekir.
[Bağımlılıkları çözümleme (aracısız)](how-to-create-group-machine-dependencies-agentless.md) | Hesabın, Konuk Işlemleri > sanal makineler için etkinleştirilen ayrıcalıklarla salt okuma erişimi olması gerekir.
[Geçir (aracısız)](tutorial-migrate-vmware.md) | Doğru izinleri atanmış bir role ihtiyacınız vardır.<br/><br/> Rol oluşturmak için, vSphere Web Istemcisinde vCenter Server yönetici olarak oturum açın. **Rol oluştur**> vCenter Server örneğini seçin. Rol adı belirtin, örneğin <em>Azure_Migrate</em>ve [gerekli izinleri](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) role atayın.


## <a name="assign-permissions-on-vcenter-objects"></a>VCenter nesnelerinde izin atama

İki yöntemden birini kullanarak sayım nesnelerinde izin atayabilirsiniz:

- Bulma/geçirme yapmak istediğiniz VM 'Leri barındıran tüm üst nesneler için, gerekli izinlere sahip bir rolü, kullanmakta olduğunuz hesaba atayın.
- Alternatif olarak, rol ve Kullanıcı hesabını veri merkezi düzeyinde atayabilir ve bunları alt nesnelere yayabilirsiniz. Sonra, keşfetmek/geçirmek istemediğiniz her nesne için hesaba **erişim** rolü vermeyin. Her yeni alt nesneye otomatik olarak devralınan erişim izni verildiğinden, bu yaklaşımın bir bütün olduğundan bu yaklaşımı önermiyoruz.

Tüm ilgili nesneler için kullanmakta olduğunuz hesaba bir rol atamak için şunları yapın:

- **Değerlendirme için**: VM değerlendirmesi için, keşfi yapmak Istediğiniz VM 'leri barındıran tüm üst nesneler için vCenter Kullanıcı hesabına **salt okuma** rolünü uygulayın. Dahil edilen üst nesneler: veri merkezine kadar, hiyerarşide ana bilgisayar, konak ve küme klasörünün klasörü. Bu izinleri hiyerarşideki alt nesnelere yay.

    ![İzin atama](./media/tutorial-assess-vmware/assign-perms.png)

- **Aracısız geçiş için**: aracısız geçiş için, keşfetmek Istediğiniz VM 'leri barındıran tüm üst nesneler için Kullanıcı hesabı için [gerekli izinlere](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) sahip kullanıcı tanımlı bir rol uygulayın. Rolü <em>Azure_Migrate</em>adı verebilirsiniz.

### <a name="scope-support"></a>Kapsam desteği

Şu anda, vCenter hesabının vCenter VM klasör düzeyinde erişimi varsa sunucu değerlendirmesi aracı VM 'Leri bulamaz. Konaklar ve kümelerin klasörleri desteklenir.

Bulma işlemini VM klasörlerine göre yapmak istiyorsanız, vCenter hesabının bir VM düzeyinde salt okuma erişimi olduğundan emin olmak için bir sonraki yordamı izleyin.

1. Bulma için kapsama eklemek istediğiniz VM klasörlerindeki tüm VM 'lerde salt okunurdur izinleri atayın.
2. VM 'Leri barındıran tüm üst nesnelere salt okuma erişimi verin.
    - Veri merkezine kadar olan hiyerarşide tüm üst nesneler (ana bilgisayar, konak klasörü, küme, küme klasörü) bulunur.
    - İzinleri tüm alt nesnelere yaymaya gerek yoktur.
3. Veri merkezini **koleksiyon kapsamı**olarak seçerek bulma için kimlik bilgilerini kullanın. Rol tabanlı erişim denetimi kurulumu, karşılık gelen vCenter Kullanıcı hesabının yalnızca kiracıya özgü VM 'lere erişimi olmasını sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Gereci [kurun](how-to-set-up-appliance-vmware.md) ve [sürekli bulmayı başlatın](how-to-set-up-appliance-vmware.md#start-continuous-discovery-by-providing-vcenter-server-and-vm-credential).
