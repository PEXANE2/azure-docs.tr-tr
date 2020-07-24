---
title: Azure portal kullanarak Azure sanal makineler için bakım denetimine genel bakış
description: Bakım denetimini kullanarak Azure VM 'lerinize bakım uygulandığını nasıl denetleyeceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 04/22/2020
ms.author: cynthn
ms.openlocfilehash: c3e914b904b0f6f1d3a4fae6c43c81cdf4eae819
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080021"
---
# <a name="managing-platform-updates-with-maintenance-control"></a>Platform güncelleştirmelerini bakım denetimiyle yönetme 

Bakım denetimini kullanarak yeniden başlatma gerektirmeyen platform güncelleştirmelerini yönetin. Azure, güvenilirliği, performansı, güvenliği geliştirmek veya yeni özellikleri başlatmak için altyapısını sıklıkla güncelleştirir. Çoğu güncelleştirme kullanıcılara saydamdır. Oyun, medya akışı ve finans işlemleri gibi bazı hassas iş yükleri, bir VM 'nin bakım için donuyor veya bağlantısı kesilmesinin birkaç saniyesini de kabul edemez. Bakım denetimi, platform güncelleştirmelerini bekleme ve bunları 35 günlük bir sıralı pencere içinde uygulama seçeneği sunar. 

Bakım denetimi, yalıtılmış sanal makinelerinize ve Azure adanmış ana bilgisayarlara güncelleştirmelerin ne zaman uygulanacağına karar vermenizi sağlar.

Bakım denetimi ile şunları yapabilirsiniz:
- Toplu güncelleştirmeler tek bir güncelleştirme paketine sahiptir.
- Güncelleştirmelerin uygulanması için 35 güne kadar bekleyin. 
- [Azure işlevleri](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler)'ni kullanarak bakım pencereniz için platform güncelleştirmelerini otomatikleştirin.
- Bakım yapılandırması abonelikler ve kaynak grupları arasında çalışır. 

## <a name="limitations"></a>Sınırlamalar

- VM 'Lerin [ayrılmış bir konakta](./linux/dedicated-hosts.md)olması veya [yalıtılmış bir VM boyutu](isolation.md)kullanılarak oluşturulması gerekir.
- 35 gün sonra, bir güncelleştirme otomatik olarak uygulanır.
- Kullanıcının, **kaynak katılımcısı** erişimi olmalıdır.

## <a name="management-options"></a>Yönetim seçenekleri

Aşağıdaki seçeneklerden herhangi birini kullanarak, bakım yapılandırması oluşturabilir ve yönetebilirsiniz:

- [Azure CLI](maintenance-control-cli.md)
- [Azure PowerShell](maintenance-control-powershell.md)
- [Azure Portal](maintenance-control-portal.md)

Azure Işlevleri örneği için bkz. bakım [denetimi ve Azure işlevleri Ile bakım güncelleştirmelerini zamanlama](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [bakım ve güncelleştirmeler](maintenance-and-updates.md).
