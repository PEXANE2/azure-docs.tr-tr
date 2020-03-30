---
title: Azure Monitör'de System Center Operations Manager, Zabbix ve Nagios'tan gelen uyarıları yönetme
description: Azure Monitör'de System Center Operations Manager, Zabbix ve Nagios'tan gelen uyarıları yönetme
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: alerts
ms.openlocfilehash: 5ef03f522fb91302bfdec985f1e3c5cf8138ae97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667457"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>Azure Monitör'de System Center Operations Manager, Zabbix ve Nagios'tan gelen uyarıları yönetme

Artık uyarılarınızı [Azure Monitor'da](https://aka.ms/azure-alerts-overview)Nagios, Zabbix ve Sistem Merkezi İşlemleri Yöneticisi'nden görüntüleyebilirsiniz. Bu uyarılar, Nagios/Zabbix sunucuları veya System Center Operations Manager ile Log Analytics'e yapılan entegrasyonlardan gelir. 

## <a name="prerequisites"></a>Ön koşullar
Log Analytics deposunda bir tür Uyarı içeren tüm kayıtlar Azure Monitor'a aktarılır, bu nedenle bu kayıtları toplamak için gereken yapılandırmayı gerçekleştirmeniz gerekir.
1. **Nagios** ve **Zabbix** uyarıları için, bu [sunucuları](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json) Log Analytics'e uyarı gönderecek şekilde [yapılandırın.](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)
1. **Sistem Merkezi Operasyon Yöneticisi** uyarıları [için, Operasyon Yöneticisi yönetim grubunuzu Log Analytics çalışma alanınıza bağlayın.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) Bunu takiben, Azure çözümleri pazaryerinden [Uyarı Yönetimi](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution) çözümlerini dağıtın. Bir kez yapıldıktan sonra, System Center Operations Manager'da oluşturulan tüm uyarılar Log Analytics'e aktarılır.

## <a name="view-your-alert-instances"></a>Uyarı örneklerinizi görüntüleme
Giriş işlemlerini Log Analytics'e yapılandırdıktan sonra, [Azure Monitor'daki](https://aka.ms/azure-alerts-overview)bu izleme hizmetlerinden uyarı örneklerini görüntülemeye başlayabilirsiniz. Azure Monitor'da hazır bulunduktan sonra [uyarı örneklerinizi yönetebilir,](https://aka.ms/managing-alert-instances) [bu uyarılarda oluşturulan akıllı grupları yönetebilir](https://aka.ms/managing-smart-groups) ve [uyarılarınızın ve akıllı grupların durumunu değiştirebilirsiniz.](https://aka.ms/managing-alert-smart-group-states)

> [!NOTE]
>  1. Bu çözüm yalnızca Azure Monitor'da Sistem Merkezi Operasyon Yöneticisi/Zabbix/Nagios'un uyarı örneklerini görüntülemenize olanak tanır. Uyarı kuralı yapılandırması yalnızca ilgili izleme araçlarında görüntülenebilir/düzenlenebilir. 
>  1. Tüm uyarı örnekleri hem Azure Monitor'da hem de Azure Log Analytics'te kullanılabilir. Şu anda, iki veya sadece belirli ateş uyarıları yutmak arasında seçim yapmak için bir yol yoktur.
>  1. Sistem Merkezi Operasyon Yöneticisi, Zabbix ve Nagios'un tüm uyarıları, temel telemetri türü kullanılamadığından "Bilinmiyor" sinyal türüne sahiptir.
>  1. Nagios uyarıları durum lu değildir – örneğin, bir uyarının [monitör koşulu](https://aka.ms/azure-alerts-overview) "Kovuldu"dan "Çözüldü"ye geçmez. Bunun yerine, hem "Ateş" hem de "Çözüldü" ayrı uyarı örnekleri olarak görüntülenir. 

