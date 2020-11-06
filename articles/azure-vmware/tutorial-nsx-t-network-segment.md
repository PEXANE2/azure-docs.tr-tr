---
title: Öğretici-Azure VMware çözümünde NSX-T ağ kesimi ekleme
description: VCenter 'daki sanal makineler (VM) için kullanmak üzere NSX-T ağ segmenti oluşturmayı öğrenin.
ms.topic: tutorial
ms.date: 11/09/2020
ms.openlocfilehash: 8ecb37a42e2986bd1c6261b8fe6c23382323b31d
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335067"
---
# <a name="tutorial-add-a-network-segment-in-azure-vmware-solution"></a>Öğretici: Azure VMware çözümünde bir ağ kesimi ekleme 

VCenter 'da oluşturulan sanal makineler (VM) NSX-T içinde oluşturulan ağ kesimlerine yerleştirilir ve vCenter 'da görünür.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ kesimleri eklemek için NSX-T Yöneticisi 'nde gezin
> * Yeni bir ağ kesimi Ekle
> * VCenter 'daki yeni ağ kesimini gözlemleyin

## <a name="prerequisites"></a>Önkoşullar

VCenter ve NSX-T Yöneticisi arabirimlerine erişimi olan bir Azure VMware çözümü özel bulutu. Daha fazla bilgi için bkz. [ağ Iletişimini yapılandırma](tutorial-configure-networking.md) öğreticisi.

## <a name="add-a-network-segment"></a>Ağ kesimi ekleme

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, vCenter 'daki VM 'Ler için kullanmak üzere bir NSX-T ağ kesimi oluşturdunuz. 

Artık şunları yapabilirsiniz: 

- [Azure VMware çözümü için DHCP oluşturma ve yönetme](manage-dhcp.md)
- [Azure VMware çözümünde VM dağıtmak için bir içerik kitaplığı oluşturma](deploy-vm-content-library.md) 
- [Şirket içi ortamlar için özel bir buluta](tutorial-expressroute-global-reach-private-cloud.md)


<!-- LINKS - external-->

<!-- LINKS - internal -->
