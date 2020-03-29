---
title: Azure portalını kullanarak bağlantı noktalarını VM'ye açma
description: Azure Portalı'ndaki kaynak yöneticisi dağıtım modelini kullanarak windows vm'nize bir bağlantı noktası / bitiş noktası oluşturma yı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: 130d3315b5a9a6f175bd3d67ed33a034ab5f8dda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371420"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Azure portalı ile bağlantı noktaları sanal bir makineye nasıl açılır?
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]


## <a name="sign-in-to-azure"></a>Azure'da oturum açma
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

1. VM için kaynak grubunu arayın ve seçin, **Ekle'yi**seçin, ardından **Ağ güvenlik grubunu**arayın ve seçin.

2. **Oluştur'u**seçin.

    **Ağ güvenlik grubu oluştur** penceresi açılır.

    ![Ağ güvenlik grubu oluşturma](./media/nsg-quickstart-portal/create-nsg.png)

2. Ağ güvenlik grubunuz için bir ad girin. 

3. Bir kaynak grubu seçin veya oluşturun, ardından bir konum seçin.

4. Ağ güvenlik grubu oluşturmak için **Oluştur'u** seçin.

## <a name="create-an-inbound-security-rule"></a>Gelen güvenlik kuralı oluşturma

1. Yeni ağ güvenlik grubunuzu seçin. 

2. **Gelen güvenlik kurallarını**seçin, ardından **Ekle'yi**seçin.

    ![Gelen kuralı ekleme](./media/nsg-quickstart-portal/add-inbound-rule.png)

3. **Gelişmiş**'i seçin. 

4. Açılır menüden **HTTP**gibi ortak bir **Hizmet** seçin. Kullanmak için belirli bir bağlantı noktası sağlamak istiyorsanız, **Özel'i** de seçebilirsiniz. 

5. İsteğe bağlı olarak, **Öncelik** veya **Adı**değiştirin. Öncelik, kuralların uygulandığı sırayı etkiler: sayısal değer ne kadar düşükse, kural o kadar erken uygulanır.

6. Kuralı oluşturmak için **Ekle'yi** seçin.

## <a name="associate-your-network-security-group-with-a-subnet"></a>Ağ güvenlik grubunuzu bir alt ağla ilişkilendirme

Gerçekleştirmeniz gereken son adım, ağ güvenlik grubunuzu bir alt ağ ile veya belirli bir ağ arabirimiyle ilişkilendirmektir. Bu örnekte, ağ güvenlik grubunu bir alt ağla ilişkilendiriz. 

1. **Alt Ağlar'ı**seçin, ardından **Ilişkilendir'i**seçin.

    ![Ağ güvenlik grubunu bir alt ağla ilişkilendirme](./media/nsg-quickstart-portal/associate-subnet.png)

2. Sanal ağınızı ve ardından uygun alt ağı seçin.

    ![Bir ağ güvenlik grubunu sanal ağla ilişkilendirme](./media/nsg-quickstart-portal/select-vnet-subnet.png)

    Bu alt ağa bağlandığınız tüm VM'lere artık 80 bağlantı noktasından ulaşılabilir.

## <a name="additional-information"></a>Ek bilgiler

[Bu makaledeki adımları Azure PowerShell'i kullanarak](nsg-quickstart-powershell.md)da gerçekleştirebilirsiniz.

Bu makalede açıklanan komutlar, trafiğin VM'nize hızla akmasını sağlar. Ağ güvenlik grupları, kaynaklarınıza erişimi denetlemek için birçok harika özellik ve ayrıntılılık sağlar. Daha fazla bilgi için ağ [güvenlik grubuyla filtre ağı trafiğine](../../virtual-network/tutorial-filter-network-traffic.md)bakın.

Yüksek kullanılabilir web uygulamaları için, VM'lerinizi bir Azure yük dengeleyicinin arkasına yerleştirmeyi düşünün. Yük dengeleyicisi, trafik filtreleme sağlayan bir ağ güvenlik grubuyla trafiği VM'lere dağıtır. Daha fazla bilgi için, [yüksek kullanılabilir bir uygulama oluşturmak için Azure'daki Yük dengesi Windows sanal makineleri'ne](tutorial-load-balancer.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir ağ güvenlik grubu oluşturdunuz, bağlantı noktası 80'deki HTTP trafiğine izin veren gelen bir kural oluşturdunuz ve sonra bu kuralı bir alt ağla ilişkilendirdin. 

Aşağıdaki makalelerde daha ayrıntılı ortamlar oluşturma hakkında bilgi bulabilirsiniz:
- [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)
- [Güvenlik grupları](../../virtual-network/security-overview.md)