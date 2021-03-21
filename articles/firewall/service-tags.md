---
title: Azure Güvenlik Duvarı hizmet etiketlerine genel bakış
description: Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 83e9a96573bbc72e0afff61cc0f151f95b081e30
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97031588"
---
# <a name="azure-firewall-service-tags"></a>Azure Güvenlik Duvarı hizmeti etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

Azure Güvenlik Duvarı hizmet etiketleri ağ kuralları hedef alanında kullanılabilir. Bunları belirli IP adresleri yerine kullanabilirsiniz.

## <a name="supported-service-tags"></a>Desteklenen hizmet etiketleri

Azure Güvenlik Duvarı ağ kurallarında kullanılabilecek hizmet etiketlerinin listesi için bkz. [sanal ağ hizmeti etiketleri](../virtual-network/service-tags-overview.md#available-service-tags) .

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik duvarı kuralları hakkında daha fazla bilgi edinmek için bkz. [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).
