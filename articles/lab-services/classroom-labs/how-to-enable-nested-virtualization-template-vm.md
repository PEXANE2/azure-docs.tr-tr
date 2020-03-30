---
title: Azure Laboratuvar Hizmetleri'nde bir şablon VM'de iç içe sanallaştırmayı etkinleştirin | Microsoft Dokümanlar
description: İçinde birden çok VM içeren bir şablon VM'yi nasıl oluşturabilirsiniz öğrenin.  Başka bir deyişle, Azure Lab Hizmetlerinde bir şablon VM'de iç içe sanallaştırmayı etkinleştirin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: 59b32834369f76d39bb4a253dad4ec541e7ef999
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502018"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde şablon sanal makinede iç içe sanallaştırmayı etkinleştirme

Şu anda Azure Laboratuvar Hizmetleri, bir laboratuvarda bir şablon sanal makine ayarlamanızı ve tek bir kopyayı kullanıcılarınızın her biri için kullanılabilir hale getirmenizi sağlar. Ağ, güvenlik veya BT sınıflarını öğreten bir profesörseniz, her öğrencinize birden fazla sanal makinenin ağ üzerinden birbirleriyle konuşabileceği bir ortam sağlamanız gerekebilir.

İç içe sanallaştırma, bir laboratuvarın şablon sanal makinesinin içinde çoklu VM ortamı oluşturmanıza olanak tanır. Şablonun yayımı, laboratuardaki her kullanıcıya içinde birden çok VM'nin yer verdiği sanal bir makine sağlar.  Bu makalede, Azure Lab Hizmetleri'nde bir şablon makinesinde iç içe sanallaştırmanın nasıl ayarlanılır.

## <a name="what-is-nested-virtualization"></a>İç içe sanallaştırma nedir?

İç içe sanallaştırma, sanal bir makine içinde sanal makineler oluşturmanıza olanak tanır. İç içe sanallaştırma Hyper-V üzerinden yapılır ve yalnızca Windows VM'lerde kullanılabilir.

İç içe sanallaştırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure'da İç Içe Sanallaştırma](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Bir Azure VM'de iç içe sanallaştırmayı etkinleştirme](../../virtual-machines/windows/nested-virtualization.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

İç içe sanallaştırma içeren bir laboratuvar kurmadan önce, göz önünde bulundurulması gereken birkaç şey vardır.

- Yeni bir laboratuvar oluştururken, sanal makine boyutu için **Orta (İç içe sanallaştırma)** veya **Büyük (İç içe sanallaştırma)** boyutlarını seçin. Bu sanal makine boyutları iç içe sanallaştırmayı destekler.
- Hem ana bilgisayar hem de istemci sanal makineleri için iyi performans sağlayacak bir boyut seçin.  Sanallaştırma kullanırken, seçtiğiniz boyutun yalnızca bir makine için değil, aynı anda çalıştırılması gereken istemci makineler için de yeterli olması gerektiğini unutmayın.
- İstemci sanal makineleri, Azure sanal ağındaki DNS sunucuları gibi Azure kaynaklarına erişmez.
- Ana bilgisayar sanal makine istemci makine internet bağlantısı na sahip olmak için izin vermek için kurulum gerektirir.
- İstemci sanal makineleri bağımsız makineler olarak lisanslanır. Microsoft işletim sistemleri ve ürünleri için lisanslama hakkında bilgi için [Microsoft Lisanslama'ya](https://www.microsoft.com/licensing/default) bakın. Şablon makinesini ayarlamadan önce kullanılan diğer yazılımlar için lisans sözleşmelerini denetleyin.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Şablon VM üzerinde iç içe yerleştirilmiş sanallaştırmayı etkinleştirme

Bu makalede, bir laboratuvar hesabı ve laboratuvar oluşturduğunuz varsayar.  Yeni bir laboratuvar hesabı oluşturma hakkında daha fazla bilgi [için, bir Laboratuvar Hesabı kurmak için öğreticiye](tutorial-setup-lab-account.md)bakın. Laboratuvar oluşturma hakkında daha fazla bilgi için sınıf [laboratuarı öğreticisini ayarlamaya](tutorial-setup-classroom-lab.md)bakın.

>[!IMPORTANT]
>Laboratuarı oluştururken sanal makine boyutu için **Büyük (iç içe sanallaştırma)** veya **Orta (iç içe sanallaştırma)** seçeneğini belirleyin.  İç içe sanallaştırma aksi takdirde çalışmaz.  

Şablon makinesine bağlanmak için [sınıf şablonu oluşturma ve yönetme'ye](how-to-create-manage-template.md)bakın.

### <a name="using-script-to-enable-nested-virtualization"></a>İç içe sanallaştırmayı etkinleştirmek için komut dosyasını kullanma

Windows Server 2016 veya Windows Server 2019 ile iç içe sanallaştırma için otomatik kurulumu kullanmak için, [azure Lab Hizmetleri'nde bir komut dosyası kullanarak bir şablon sanal makinede iç içe sanallaştırmayı etkinleştir'e](how-to-enable-nested-virtualization-template-vm-using-script.md)bakın. Hyper-V rolünü yüklemek için [Lab Services Hyper-V komut dosyaları](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV) kullanırsınız.  Komut dosyaları, Hyper-V sanal makinelerin internet erişimine sahip olması için ağ kurma özelliğini de belirler.

### <a name="using-windows-tools-to-enable-nested-virtualization"></a>İç içe sanallaştırmayı etkinleştirmek için Windows araçlarını kullanma

Windows rolleri ve yönetim araçlarını kullanarak Windows Server 2016 veya Windows Server 2019 için iç içe [sanallaştırma,](how-to-enable-nested-virtualization-template-vm-ui.md)bkz.  Talimatlar ayrıca Hyper-V sanal makinelerin internet erişimine sahip olması için ağ kurmanın nasıl yapılacağını da kapsayacaktır.
