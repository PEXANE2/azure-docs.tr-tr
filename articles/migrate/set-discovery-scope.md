---
title: Azure geçişi ile VMware VM keşfi kapsamını ayarlama
description: Azure geçişi ile VMware VM değerlendirmesi ve geçişi için bulma kapsamının nasıl ayarlanacağını açıklar.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: e53eb0d01df2152aeced2901335f75879885fd22
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84770399"
---
# <a name="set-discovery-scope-for-vmware-vms"></a>VMware VM 'Leri için bulma kapsamını ayarlama

Bu makalede, aşağıdaki durumlarda keşfettiğiniz VMware VM 'lerinin kapsamını nasıl sınırlandırılacağını açıklanmaktadır:

- Azure geçişi: Sunucu değerlendirmesi aracını kullanırken [Azure geçiş](migrate-appliance-architecture.md) gereci Ile VM 'ler bulunuyor.
- VMware VM 'lerinin Azure 'a daha az geçirilmesi için Azure geçişi: sunucu geçiş aracı 'nı kullanırken Azure [geçişi](migrate-appliance-architecture.md) Ile sanal makineler bulunuyor.

Gereci ayarlarken vCenter Server bağlanır ve bulmayı başlatır. Gereci vCenter Server 'e bağlanmadan önce, bulmayı vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, konaklar klasörü veya ayrı VM 'Ler ile sınırlayabilirsiniz. Kapsamı ayarlamak için, gerecin vCenter Server erişmek için kullandığı hesaba izin atarsınız.

## <a name="before-you-start"></a>Başlamadan önce

Azure geçişi 'nin bulma için kullandığı bir vCenter Kullanıcı hesabı ayarlamadıysanız, artık [değerlendirme](tutorial-prepare-vmware.md#set-up-permissions-for-assessment) veya [aracısız geçiş](tutorial-prepare-vmware.md#assign-permissions-to-an-account)için bunu yapın.


## <a name="assign-permissions-and-roles"></a>İzin ve rol atama

İki yöntemden birini kullanarak VMware Inventory Objects 'e izinler atayabilirsiniz:

- Gereç tarafından kullanılan hesapta, kapsama eklemek istediğiniz nesneler üzerinde gerekli izinlere sahip bir rol atayın.
- Alternatif olarak, veri merkezi düzeyinde hesaba bir rol atayın ve alt nesnelere yayın. Ardından hesaba, kapsam içinde istemediğiniz her nesne için bir **erişim** rolü vermeyin. Her yeni alt nesneye otomatik olarak devralınan erişim izni verildiğinden, bu yaklaşımın bir bütün olduğundan bu yaklaşımı önermiyoruz.

, VCenter VM klasör düzeyinde envanter bulmayı kapsamaçamazsınız. Bir VM klasöründeki VM 'Lerde bulma kapsamını belirlemeniz gerekiyorsa, bir kullanıcı oluşturun ve her bir gerekli sanal makineye ayrı ayrı erişim verin. Konak ve küme klasörleri desteklenir.


### <a name="assign-a-role-for-assessment"></a>Değerlendirme için rol atama

1. Bulma için kullandığınız gereç vCenter hesabında, bulmak ve değerlendirmek istediğiniz VM 'Leri barındıran tüm üst nesneler için **salt okuma** rolünü uygulayın (konak, küme, Hosts klasörü, kümeler klasörü, veri merkezine kadar).
2. Bu izinleri hiyerarşideki alt nesnelere yay.

    ![İzin atama](./media/tutorial-assess-vmware/assign-perms.png)

### <a name="assign-a-role-for-agentless-migration"></a>Aracısız geçiş için rol atama

1. Geçiş için kullanmakta olduğunuz gereç vCenter hesabında, [gereken izinlere](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless)sahip kullanıcı tanımlı bir rolü, bulma ve geçirme Istediğiniz VM 'leri barındıran tüm üst nesnelere uygulayın.
2. Rolü, daha kolay tanımlanacak bir şekilde ad verebilirsiniz. Örneğin, <em>Azure_Migrate</em>.

## <a name="work-around-vm-folder-restriction"></a>VM klasörü kısıtlaması etrafında çalışma

Şu anda, vCenter VM klasör düzeyinde erişim verildiğinde sunucu değerlendirmesi aracı VM 'Leri bulamaz. Bulma ve değerlendirmenize VM klasörlerine göre kapsam yapmak istiyorsanız, bu geçici çözümü kullanın.

1. Bulma ve değerlendirme için kapsama eklemek istediğiniz VM klasörlerinde bulunan tüm VM 'lerde salt okunurdur izinleri atayın.
2. VM 'Leri ana bilgisayar, küme, konaklar klasörü, kümeler klasörünü, veri merkezine kadar olan tüm üst nesnelere salt okunurdur erişimi verin. İzinleri tüm alt nesnelere yaymaya gerek yoktur.
3. Bulma için kimlik bilgilerini kullanmak için, veri merkezini **koleksiyon kapsamı**olarak seçin.


Rol tabanlı erişim denetimi kurulumu, karşılık gelen vCenter Kullanıcı hesabının yalnızca kiracıya özgü VM 'lere erişimi olmasını sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Gereci [ayarlayın](how-to-set-up-appliance-vmware.md)ve [sürekli bulma işlemini başlatın](how-to-set-up-appliance-vmware.md#start-continuous-discovery-by-providing-vcenter-server-and-vm-credential).
