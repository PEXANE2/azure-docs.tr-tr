---
title: Azure Lab Services | bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme | Microsoft Docs
description: İçinde birden çok VM içeren bir şablon VM 'si oluşturmayı öğrenin.  Diğer bir deyişle, Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: a2f045cbf7c107e7f5dbeff0d78bf5e8d9d8fba6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445245"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştir

Şu anda Azure Lab Services, bir laboratuvarda bir şablon sanal makinesi ayarlamanıza ve tek bir kopyayı kullanıcılarınızın her biri için kullanılabilir hale getirmenizi sağlar. Eğitim ağı, güvenlik veya BT sınıflarındaysanız, öğrencilerinizin her birini, birden çok sanal makinenin bir ağ üzerinden konuşabilecek bir ortamla birlikte sağlamanız gerekebilir.

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesi içinde çoklu VM ortamı oluşturmanızı sağlar. Şablonu yayımlamak, her kullanıcıya, içindeki birden çok VM ile bir sanal makine oluşturacak şekilde laboratuvar sağlar.  Bu makalede, Azure Lab Services bir şablon makinesinde iç içe sanallaştırmanın nasıl ayarlanacağı ele alınmaktadır.

## <a name="what-is-nested-virtualization"></a>İç içe sanallaştırma nedir?

İç içe sanallaştırma, sanal makine içinde sanal makineler oluşturmanızı sağlar. İç içe sanallaştırma Hyper-V aracılığıyla yapılır ve yalnızca Windows VM 'lerinde kullanılabilir.

İç içe sanallaştırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure 'da iç içe sanallaştırma](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Azure VM 'de iç içe sanallaştırmayı etkinleştirme](../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Önemli noktalar

İç içe sanallaştırmaya sahip bir laboratuvar ayarlamadan önce göz önünde bulundurmanız gereken birkaç nokta vardır.

- Yeni bir laboratuvar oluştururken, sanal makine boyutu için **Orta (Iç içe sanallaştırma)** veya **büyük (iç içe sanallaştırma)** boyutlarını seçin. Bu sanal makine boyutları, iç içe sanallaştırmayı destekler.
- Hem konak hem de istemci sanal makineleri için iyi performans sağlayacak bir boyut seçin.  Sanallaştırmayı kullanırken, seçtiğiniz boyutun yalnızca bir makine değil, ana bilgisayarın yanı sıra aynı anda çalışan Hyper-V makineleri için yeterli olması gerektiğini unutmayın.
- İstemci sanal makineleri, Azure sanal ağındaki DNS sunucuları gibi Azure kaynaklarına erişemez.
- Konak sanal makinesi, istemci makinenin internet bağlantısına izin vermek için kurulum gerektirir.
- İstemci sanal makineleri bağımsız makineler olarak lisanslanır. Microsoft Operasyon sistemleri ve ürünleri için lisanslama hakkında bilgi için bkz. [Microsoft lisanslama](https://www.microsoft.com/licensing/default) . Şablon makinesini ayarlamadan önce kullanılan diğer yazılımlar için lisans sözleşmelerini denetleyin.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Şablon VM üzerinde iç içe yerleştirilmiş sanallaştırmayı etkinleştirme

Bu makalede, bir laboratuar hesabı ve Laboratuvarı oluşturmuş olduğunuz varsayılmaktadır.  Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [öğretici, laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md). Laboratuvar oluşturma hakkında daha fazla bilgi için bkz. [sınıf Laboratuvarı ayarlama öğreticisi](tutorial-setup-classroom-lab.md).

>[!IMPORTANT]
>Laboratuvar oluştururken sanal makine boyutu için **büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma, aksi takdirde çalışmaz.  

Şablon makinesine bağlanmak için bkz. [sınıf şablonu oluşturma ve yönetme](how-to-create-manage-template.md).

İç içe sanallaştırmayı etkinleştirmek için birkaç görev gerçekleştirmeniz gerekir.  

- **Hyper-V rolünü etkinleştirin**. Laboratuvar Hizmetleri sanal makinesi üzerinde Hyper-V sanal makinelerinin oluşturulması ve çalıştırılması için Hyper-V rolü etkinleştirilmelidir.
- **DHCP 'Yi etkinleştirin**.  Laboratuvar Hizmetleri sanal makinesinde DHCP rolü etkinleştirildiğinde, Hyper-V sanal makinelerine otomatik olarak bir IP adresi atanabilir.
- **Hyper-V VM 'leri IÇIN NAT ağı oluşturun**.  NAT ağı Hyper-V sanal makinelerinin internet erişimine sahip olmasını sağlayacak şekilde ayarlanır.  Hyper-V sanal makineleri birbirleriyle iletişim kurabilir.

>[!NOTE]
>Laboratuvar Hizmetleri sanal makinesinde oluşturulan NAT ağı, Hyper-V VM 'nin aynı Laboratuvar Hizmetleri sanal makinesinde internet ve diğer Hyper-V VM 'lerine erişmesine izin verir.  Hyper-V VM, Azure sanal ağındaki DNS sunucuları gibi Azure kaynaklarına erişemeyecektir.

Yukarıda listelenen görevlerin gerçekleştirilmesi, bir komut dosyası veya Windows araçları kullanılarak yapılabilir.  Daha fazla bilgi için aşağıdaki bölümleri okuyun.

### <a name="using-script-to-enable-nested-virtualization"></a>İç içe sanallaştırmayı etkinleştirmek için betiği kullanma

Windows Server 2016 veya Windows Server 2019 ile iç içe sanallaştırmaya yönelik otomatik kurulumu kullanmak için, [bir betik kullanarak Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı etkinleştirme](how-to-enable-nested-virtualization-template-vm-using-script.md)makalesine bakın. Hyper-V rolünü yüklemek için, [Laboratuvar Hizmetleri Hyper-v betiklerinden](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV) betikleri kullanacaksınız.  Ayrıca, Hyper-V sanal makinelerinin internet erişimi olması için betikler ağ oluşturacak.

### <a name="using-windows-tools-to-enable-nested-virtualization"></a>İç içe sanallaştırmayı etkinleştirmek için Windows araçlarını kullanma

Windows Server 2016 veya Windows Server 2019 için iç içe geçmiş sanallaştırma kurulumu Windows rolleri ve yönetim araçları kullanılarak, bkz. [Azure Lab Services bir şablon sanal makinesinde iç içe sanallaştırmayı El Ile etkinleştirme](how-to-enable-nested-virtualization-template-vm-ui.md).  Yönergeler Ayrıca, Hyper-V sanal makinelerinin internet erişimi olması için ağın nasıl ayarlanacağını de kapsar.
