---
title: Azure Geçiş ile VMware VM keşfinin kapsamını ayarlama
description: Azure Geçişi ile VMware VM değerlendirmesi ve geçişi için keşif kapsamının nasıl ayarlanır olduğunu açıklar.
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 29b3077ead168cef2790468d6ac62d1c59c24c11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337642"
---
# <a name="set-discovery-scope-for-vmware-vms"></a>VMware VM'ler için bulma kapsamı ayarlama

Bu makalede, [Azure Geçiş cihazı](migrate-appliance-architecture.md)tarafından keşfedilen VMware VM'ler için keşif kapsamının nasıl sınırlandırılabildiğini açıklanmaktadır.

Azure Geçir cihazı, şirket içi VMware VM'leri aşağıdaki leri keşfeder: 

- VMware VM'leri Azure'a geçiş için değerlendirmek için [Azure Geçir:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanın.
- VMware VM'lerin [Azure'a aracısız geçişi](server-migrate-overview.md) için [Azure Geçiş:Sunucu Geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool) aracısını kullanın.

## <a name="set-discovery-scope"></a>Keşif kapsamını ayarlama


VMware VM değerlendirmesi veya geçişi için Azure Geçir cihazını ayarladıktan sonra, cihaz VM'leri keşfetmeye ve Azure'a VM meta verilerini göndermeye başlar. Cihazı keşif için vCenter Server'a bağlamadan önce, keşif kapsamını keşfi vCenter Server veri merkezleri, kümeler, kümeler klasörü, ana bilgisayarlar, ana bilgisayarlar klasörü veya tek tek VM'lerle sınırlayacak şekilde ayarlayabilirsiniz.

Kapsamı ayarlamak için, Azure Geçir'in vCenter Sunucusuna erişmek için kullandığı hesapta izinler atarsınız.

## <a name="create-a-vcenter-user-account"></a>vCenter kullanıcı hesabı oluşturma

Azure Geçir cihazının VMware VM'leri keşfetmek, değerlendirmek ve geçirmek için kullandığı bir vCenter kullanıcı hesabı nı henüz ayarlamadıysanız, bunu önce yapın.

1.    vCenter Server yöneticisi olarak vSphere Web Client'da oturum açın.
2.    **Yönetim** > **SSO kullanıcılarını ve Gruplarını**seçin ve **Kullanıcılar** sekmesini tıklatın.
3.    Yeni **Kullanıcı** simgesini seçin.
4.    **Yeni**kullanıcı bilgilerini tamam > doldurun.

Hesap izinleri, ne dağıttığınız için bağlıdır.

**Özellik** | **Hesap izinleri**
--- | ---
[Değerlendirmek](tutorial-assess-vmware.md)| Hesabın salt okunur erişime ihtiyacı vardır.
[Uygulamaları/rolleri/özellikleri bulma](how-to-discover-applications.md) | Sanal makineler > Konuk İşlemleri için etkinleştirilen ayrıcalıklar sayesinde hesabın salt okunur erişime ihtiyacı vardır.
[Bağımlılıkları analiz et (aracısız)](how-to-create-group-machine-dependencies-agentless.md) | Sanal makineler > Konuk İşlemleri için etkinleştirilen ayrıcalıklar sayesinde hesabın salt okunur erişime ihtiyacı vardır.
[Geçiş (aracısız)](tutorial-migrate-vmware.md) | Doğru izinlere atanmış bir role ihtiyacınız var.<br/><br/> Bir rol oluşturmak için vCenter Server yöneticisi olarak vSphere Web Client'da oturum açın. **rol oluştur>** vCenter Server örneğini seçin. Bir rol adı belirtin, örneğin <em>Azure_Migrate</em>ve [role gerekli izinleri](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) atayın.


## <a name="assign-permissions-on-vcenter-objects"></a>vCenter nesnelerinde izin atama

İki yöntemden birini kullanarak stok nesnelerinde izinatlayabilirsiniz:

- Keşfetmek/geçirmek istediğiniz VM'leri barındıran tüm üst nesneler için, kullanmakta olduğunuz hesaba gerekli izinleri içeren bir rol atayın.
- Alternatif olarak, rolü ve kullanıcı hesabını veri merkezi düzeyinde atayabilir ve bunları alt nesnelere çoğaltabilirsiniz. Ardından, keşfetmek/geçirmek istemediğiniz her nesne için hesaba **erişim yok** rolü verin. Hantal olduğu için bu yaklaşımı önermiyoruz ve her yeni alt nesnenin ebeveynden devralınan erişime otomatik olarak verildiği için erişim denetimlerini ortaya çıkarabiliriz.

İlgili tüm nesneler için kullandığınız hesaba bir rol atamak için aşağıdakileri yapın:

- **Değerlendirme için**: VM değerlendirmesi için, keşfetmek istediğiniz VM'leri barındıran tüm ana nesneler için vCenter kullanıcı hesabına **salt okunur** rolünü uygulayın. Ana nesneler dahil: ana bilgisayar, ana bilgisayar klasörü, küme ve hiyerarşideki kümelerin klasörü, veri merkezine kadar. Bu izinleri hiyerarşideki alt nesnelere yayıltın.

    ![İzin atama](./media/tutorial-assess-vmware/assign-perms.png)

- **Aracısız geçiş için**: Aracısız geçiş için, keşfetmek istediğiniz VM'leri barındıran tüm ana nesneler için kullanıcı hesabına gerekli [izinlerle](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) kullanıcı tanımlı bir rol uygulayın. Rolü <em>Azure_Migrate</em>adlandırabilirsiniz.

### <a name="scope-support"></a>Kapsam desteği

Şu anda, vCenter hesabı vCenter VM klasör düzeyinde verilen erişim eki varsa, Sunucu Değerlendirme aracı VM'leri keşfedemez. Ana bilgisayar ve kümelerin klasörleri desteklenir.

Keşfinizi VM klasörlerine göre kapsamda kapsamda genişletmek istiyorsanız, vCenter hesabının VM düzeyinde atanmış salt okunur erişime sahip olduğundan emin olmak için bir sonraki yordamı tamamlayın.

1. Keşif için kapsama almak istediğiniz VM klasörlerinde ki tüm VM'lere salt okunur izinleri atayın.
2. VM'leri barındıran tüm üst nesnelere salt okunur erişimi tanıyın.
    - Veri merkezine kadar hiyerarşideki tüm üst nesneler (ana bilgisayar, ana bilgisayar klasörü, küme, kümeler klasörü) dahildir.
    - İzinleri tüm alt nesnelere yaymanız gerekmez.
3. Veri merkezini **Koleksiyon Kapsamı**olarak seçerek keşif için kimlik bilgilerini kullanın. Rol tabanlı erişim denetimi kurulumu, ilgili vCenter kullanıcı hesabının yalnızca kiracıya özgü VM'lere erişmesini sağlar.


## <a name="next-steps"></a>Sonraki adımlar

[Cihazı ayarlayın](how-to-set-up-appliance-vmware.md) ve [sürekli keşif başlatın.](how-to-set-up-appliance-vmware.md#start-continuous-discovery-by-providing-vcenter-server-and-vm-credential)
