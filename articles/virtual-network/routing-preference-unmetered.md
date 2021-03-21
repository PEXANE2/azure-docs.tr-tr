---
title: Azure 'da tarifeli olmayan yönlendirme tercihi
description: Kaynaklardaki verileri CDN sağlayıcısına yönlendiren bir yönlendirme tercihini nasıl yapılandırabileceğiniz hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
Customer intent: As an Azure customer, I want to learn more about enabling routing preference for my CDN origin resources.
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2021
ms.author: mnayak
ms.openlocfilehash: 1fcc918e5b4b54a415fed0f38d210aeeaa62c008
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101680309"
---
# <a name="what-is-routing-preference-unmetered"></a>Yönlendirme tercihi tarifeli değil mi?

Yönlendirme tercihi, Azure 'da kaynak içeriklerini barındıran müşterilere sahip Content Delivery Network (CDN) sağlayıcıları için kullanılabilir. Hizmet, CDN sağlayıcılarının çeşitli konumlarda Microsoft Global Network Edge yönlendiricileriyle doğrudan eşleme bağlantısı kurmasını sağlar.

![Yönlendirme tercihi tarifeli değil](media/routing-preference-unmetered/unmetered.png)

Ağ trafiğiniz Azure 'daki kaynaktan, doğrudan bağlantıdan gelen CDN sağlayıcısı avantajlarına gidiyor.
* Bu doğrudan bağlantılar aracılığıyla yönlendirilen Azure kaynaklarınızdan gelen trafik yumurtından veri aktarımı faturanız ücretsizdir.
* Azure 'da CDN sağlayıcısı ve kaynak arasında doğrudan bağlantı, aralarında atlama olmadığı için en iyi performansı sağlar. Bu, genellikle kaynaktan veri getiren CDN iş yükünün avantajını sağlar.

## <a name="configuring-routing-preference-unmetered"></a>Yönlendirme tercih edilmemiş yapılandırma

Tarifeli olmayan yönlendirme tercihlerinden yararlanmak için, CDN sağlayıcılarınızın bu programın bir parçası olması gerekir. CDN sağlayıcınız programın bir parçası değilse CDN sağlayıcınızla görüşün.

Ardından, kaynaklarınız için yönlendirme tercihini yapılandırın ve yönlendirme tercihi türünü **Internet** olarak ayarlayın. Genel IP adresi oluştururken yönlendirme tercihini yapılandırabilir ve ardından genel IP 'yi sanal makineler, internet 'e yönelik yük dengeleyiciler gibi kaynaklarla ilişkilendirebilirsiniz. [Azure portal kullanarak genel IP adresi için yönlendirme tercihini yapılandırma hakkında bilgi edinin](routing-preference-portal.md)

Ayrıca depolama hesabınız için yönlendirme tercihini etkinleştirebilir ve ikinci bir uç nokta yayımlayabilirsiniz. Bu, depolama kaynağından veri getirmek için CDN sağlayıcısı tarafından kullanılması gerekir. Örneğin, *Storageaccounta* depolama hesabı için internet rotaya özgü bir uç nokta yayımlamak, depolama hizmetlerinizin ikinci uç noktasını aşağıda gösterildiği gibi yayımlayacak:

![Depolama hesapları için yönlendirme tercihi](media/routing-preference-unmetered/storage-endpoints.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell kullanarak bir VM için yönlendirme tercihini yapılandırma](configure-routing-preference-virtual-machine-powershell.md)
* [Azure CLı kullanarak bir VM için yönlendirme tercihini yapılandırma](configure-routing-preference-virtual-machine-cli.md)
* [Depolama hesabınız için yönlendirme tercihini yapılandırın](/azure/storage/common/network-routing-preference)