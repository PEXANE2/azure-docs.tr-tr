---
title: Azure Güvenlik Duvarı çalışma kitabını kullanarak günlükleri izleme
description: Azure Güvenlik Duvarı çalışma kitapları, Azure Güvenlik Duvarı veri analizi için esnek bir tuval ve Azure portal içinde zengin görsel raporların oluşturulmasını sağlar.
services: firewall
author: gopimsft
ms.service: firewall
ms.topic: how-to
ms.date: 09/22/2020
ms.author: victorh
ms.openlocfilehash: e63336b0e84d303b51eda56f90ca6fb453d8ae0d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99831850"
---
# <a name="monitor-logs-using-azure-firewall-workbook"></a>Azure Güvenlik Duvarı çalışma kitabını kullanarak günlükleri izleme

Azure Güvenlik Duvarı çalışma kitabı, Azure Güvenlik Duvarı veri analizi için esnek bir tuval sağlar. Azure portal içinde zengin görsel raporlar oluşturmak için kullanabilirsiniz. Azure üzerinde dağıtılan birden çok Güvenlik duvarınıza dokunabilir ve bunları Birleşik etkileşimli deneyimler halinde birleştirebilirsiniz.

Azure Güvenlik Duvarı olayları hakkında öngörüler elde edebilir, uygulamanız ve ağ kurallarınız hakkında bilgi edinebilir ve URL 'Ler, bağlantı noktaları ve adresler arasında güvenlik duvarı etkinlikleri için istatistikleri görebilirsiniz. Azure Güvenlik Duvarı çalışma kitabı, güvenlik duvarlarınızı ve Kaynak gruplarınızı filtrelemenizi ve günlüklerinizin bir sorunu araştırırken veri kümelerini kolaylıkla kolayca filtrelemenizi sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, Azure portal aracılığıyla [Tanılama günlüğünü etkinleştirmeniz](firewall-diagnostics.md#enable-diagnostic-logging-through-the-azure-portal) gerekir. Ayrıca, Azure Güvenlik Duvarı için kullanılabilen tanılama günlüklerine ve ölçümlere genel bir bakış için [Azure Güvenlik Duvarı günlüklerini ve ölçümlerini](logs-and-metrics.md) okuyun.

## <a name="get-started"></a>başlarken

Çalışma kitabını dağıtmak için Azure [güvenlik duvarı Için Azure Izleyici çalışma kitabına](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20Firewall/Workbook%20-%20Azure%20Firewall%20Monitor%20Workbook) gidin ve sayfadaki yönergeleri izleyin. Azure Güvenlik Duvarı çalışma kitabı, çok kiracılı ve çoklu aboneliklerde çalışmak üzere tasarlanmıştır ve birden çok güvenlik duvarını filtrelenebilir.

## <a name="overview-page"></a>Genel bakış sayfası

Genel Bakış sayfası, çalışma alanları, zaman ve güvenlik duvarları arasında filtre uygulamak için bir yol sağlar. Güvenlik duvarları ve günlük türleri arasında zamana göre olayları gösterir (uygulama, ağlar, tehdit Intel, DNS proxy).

:::image type="content" source="./media/firewall-workbook/firewall-workbook-overview.png" alt-text="Azure Güvenlik Duvarı çalışma kitabına genel bakış":::

## <a name="application-rule-log-statistics"></a>Uygulama kuralı günlük istatistikleri

Bu sayfada zaman içindeki benzersiz IP adresi kaynakları, uygulama kuralı sayısı kullanımı, zaman içinde reddedilen/izin verilen FQDN ve filtrelenmiş veriler gösterilir. IP adresine göre verileri filtreleyebilirsiniz.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-application-rule.png" alt-text="Azure Güvenlik Duvarı çalışma kitabı uygulama kuralı günlüğü":::

## <a name="network-rule-log-statistics"></a>Ağ kuralı günlük istatistikleri

Bu sayfa, bir kuralla bir görünüm sağlar – izin ver/Reddet, IP ve DNAT tarafından zaman içinde hedef bağlantı noktası. Eylem, bağlantı noktası ve hedef türüne göre de filtre uygulayabilirsiniz.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-network-rule.png" alt-text="Azure Güvenlik Duvarı çalışma kitabı ağ kuralı günlüğü":::

Ayrıca, günlükleri zaman penceresine göre filtreleyebilirsiniz:

:::image type="content" source="./media/firewall-workbook/firewall-workbook-network-rule-time.png" alt-text="Azure Güvenlik Duvarı çalışma kitabı ağ kuralı günlük zamanı penceresi":::

## <a name="investigations"></a>Araştırmalar

Günlüklere bakabilir ve kaynak IP adresine göre kaynak hakkında daha fazla bilgi bulabilirsiniz. Sanal makine adı ve ağ arabirimi adı gibi bilgiler edinebilirsiniz. Günlüklerden kaynağa filtre uygulamak basittir.

:::image type="content" source="./media/firewall-workbook/firewall-workbook-investigation.png" alt-text="Azure Güvenlik Duvarı çalışma kitabı araştırması":::

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı tanılama](firewall-diagnostics.md) hakkında daha fazla bilgi edinin
