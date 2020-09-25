---
title: Azure VMWare çözüm ağı ve bağlantısı
description: Azure VMWare çözüm ağı ve bağlantı açıklaması.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 6c45043ebbbc5099f512a1d746e48d2a8a065e6f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91316910"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware çözümü, şirket içi ve Azure tabanlı ortamların veya kaynakların erişebileceği özel bir bulut ortamı sunar. Azure ExpressRoute ve VPN bağlantıları gibi hizmetler bağlantı sağlar. Bu hizmetler, hizmetleri etkinleştirmek için belirli ağ adresi aralıkları ve güvenlik duvarı bağlantı noktaları gerektirir.

Özel bir bulut dağıtımında, yönetim, sağlama ve vMotion için özel ağlar oluşturulur. VCenter ve NSX-T Manager ve sanal makine vMotion veya dağıtımına erişmek için bu özel ağları kullanın.  ExpressRoute Global Reach, özel bulutları şirket içi ortamlara bağlamak için kullanılır. Bağlantı, aboneliğinizde bir ExpressRoute devresine sahip bir sanal ağ gerektiriyor.


>[!NOTE]
>İnternet ve Azure hizmetlerine erişim sağlanır ve özel bir bulut dağıtımında üretim ağlarında VM 'Leri tüketmek için sağlanır.  Varsayılan olarak, internet erişimi yeni özel bulutlar için devre dışıdır ve herhangi bir zamanda etkinleştirilebilir veya devre dışı bırakılabilir.