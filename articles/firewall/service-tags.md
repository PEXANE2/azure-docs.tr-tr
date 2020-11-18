---
title: Azure Güvenlik Duvarı hizmet etiketlerine genel bakış
description: Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 47377817b62d33e8af79e4a0d2dceb68ba9dbdc5
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658656"
---
# <a name="azure-firewall-service-tags"></a>Azure Güvenlik Duvarı hizmeti etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

Azure Güvenlik Duvarı hizmet etiketleri ağ kuralları hedef alanında kullanılabilir. Bunları belirli IP adresleri yerine kullanabilirsiniz.

## <a name="supported-service-tags"></a>Desteklenen hizmet etiketleri

Azure Güvenlik Duvarı ağ kurallarında kullanılabilecek hizmet etiketlerinin listesi için bkz. [güvenlik grupları](../virtual-network/network-security-groups-overview.md#service-tags) .

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik duvarı kuralları hakkında daha fazla bilgi edinmek için bkz. [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).