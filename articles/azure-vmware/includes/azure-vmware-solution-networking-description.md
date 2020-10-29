---
title: Azure VMware Çözüm ağı ve bağlantısı
description: Azure VMware Çözüm ağı ve bağlantı açıklaması.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 47b5fab8430f4e462f1e4a0e8cb6ddb69ec38c02
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92924984"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware çözümü, şirket içi ve Azure tabanlı ortamların veya kaynakların erişebileceği özel bir bulut ortamı sunar. Azure ExpressRoute ve VPN bağlantıları gibi hizmetler bağlantı sağlar. Bu hizmetler, hizmetleri etkinleştirmek için belirli ağ adresi aralıkları ve güvenlik duvarı bağlantı noktaları gerektirir.

Özel bir bulut dağıtımında, yönetim, sağlama ve vMotion için özel ağlar oluşturulur. VCenter ve NSX-T Manager ve sanal makine vMotion veya dağıtımına erişmek için bu özel ağları kullanın.  ExpressRoute Global Reach, özel bulutları şirket içi ortamlara bağlamak için kullanılır. Bağlantı, aboneliğinizde bir ExpressRoute devresine sahip bir sanal ağ gerektiriyor.

Web sunucuları ve sanal makineler gibi kaynaklara Azure sanal WAN genel IP işlevselliği aracılığıyla internet üzerinden erişilebilir.  Varsayılan olarak, internet erişimi yeni özel bulutlar için devre dışıdır. Daha fazla bilgi için bkz. [Azure VMware çözümünde genel IP işlevselliğini kullanma](../public-ip-usage.md).