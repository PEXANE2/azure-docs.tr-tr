---
title: Azure VMWare çözüm ağı ve bağlantısı
description: Azure VMWare çözüm ağı ve bağlantı açıklaması.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 1d43a8a49fc56a6a5b534a89fd1dfc2f5ae2d478
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91574474"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware çözümü, şirket içi ve Azure tabanlı ortamların veya kaynakların erişebileceği özel bir bulut ortamı sunar. Azure ExpressRoute ve VPN bağlantıları gibi hizmetler bağlantı sağlar. Bu hizmetler, hizmetleri etkinleştirmek için belirli ağ adresi aralıkları ve güvenlik duvarı bağlantı noktaları gerektirir.

Özel bir bulut dağıtımında, yönetim, sağlama ve vMotion için özel ağlar oluşturulur. VCenter ve NSX-T Manager ve sanal makine vMotion veya dağıtımına erişmek için bu özel ağları kullanın.  ExpressRoute Global Reach, özel bulutları şirket içi ortamlara bağlamak için kullanılır. Bağlantı, aboneliğinizde bir ExpressRoute devresine sahip bir sanal ağ gerektiriyor.



>[!NOTE]
>İnternet ve Azure hizmetlerine erişim sağlanır ve özel bir bulut dağıtımında üretim ağlarında VM 'Leri tüketmek için sağlanır.  Varsayılan olarak, internet erişimi yeni özel bulutlar için devre dışıdır ve herhangi bir zamanda etkinleştirilebilir veya devre dışı bırakılabilir.