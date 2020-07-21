---
title: System Center Operations Manager, Zabbix ve Nagios uyarılarını Azure İzleyici'de yönetme
description: System Center Operations Manager, Zabbix ve Nagios uyarılarını Azure İzleyici'de yönetme
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: alerts
ms.openlocfilehash: 9fbad920bdfa26419e4b865db5d4e53dad97acce
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539457"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>System Center Operations Manager, Zabbix ve Nagios uyarılarını Azure İzleyici'de yönetme

Artık, [Azure izleyici](https://aka.ms/azure-alerts-overview)'de Nagios, Zabbix ve System Center Operations Manager uyarılarını görüntüleyebilirsiniz. Bu uyarılar Nagios/Zabbix sunucularıyla tümleştirmelere veya Log Analytics System Center Operations Manager. 

## <a name="prerequisites"></a>Önkoşullar
Bir uyarı türünde Log Analytics deposundaki tüm kayıtlar Azure Izleyici 'ye içeri aktarıldığından, bu kayıtları toplamak için gereken yapılandırmayı gerçekleştirmeniz gerekir.
1. **Nagios** ve **Zabbix** uyarıları için, [Bu sunucuları](../learn/quick-collect-linux-computer.md) Log Analytics [uyarıları gönderecek](./data-sources-custom-logs.md?toc=/azure/azure-monitor/toc.json) şekilde yapılandırın.
1. **System Center Operations Manager** uyarılar için [Operations Manager yönetim grubunuzu Log Analytics çalışma alanınıza bağlayın](./om-agents.md). Bunu izleyerek [uyarı yönetimi](./alert-management-solution.md) çözümünü Azure çözüm marketi ' nden dağıtın. İşiniz bittiğinde, System Center Operations Manager oluşturulan tüm uyarılar Log Analytics içine aktarılır.

## <a name="view-your-alert-instances"></a>Uyarı örneklerinizi görüntüleme
İçeri aktarmayı Log Analytics yapılandırdıktan sonra, [Azure izleyici](https://aka.ms/azure-alerts-overview)'de bu izleme hizmetlerinden uyarı örneklerini görüntülemeye başlayabilirsiniz. Azure Izleyici 'de mevcut olduktan sonra, [Uyarı örneklerinizi yönetebilir](https://aka.ms/managing-alert-instances), [Bu uyarılarda oluşturulan akıllı grupları yönetebilir](https://aka.ms/managing-smart-groups) ve [uyarılarınızın ve akıllı grupların durumunu değiştirebilirsiniz](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  1. Bu çözüm yalnızca Azure Izleyici 'de System Center Operations Manager/Zabbix/Nagios tetiklenen uyarı örneklerini görüntülemenize izin verir. Uyarı kuralı yapılandırması, yalnızca ilgili izleme araçlarında görüntülenebilir/düzenlenebilir. 
>  1. Tetiklenen tüm uyarı örnekleri hem Azure Izleyici 'de hem de Azure Log Analytics kullanılabilir. Şu anda, yalnızca belirtilen iki veya daha fazla tetiklenen uyarı arasından seçim yapmanın bir yolu yoktur.
>  1. System Center Operations Manager, Zabbix ve Nagios 'ın tüm uyarıları, temeldeki telemetri türü kullanılamadığından "bilinmiyor" sinyal türüne sahip.
>  1. Nagios uyarıları durum bilgisi değildir; Örneğin, bir uyarının [izleme koşulu](https://aka.ms/azure-alerts-overview) "tetiklenir" durumundan "çözümlendi" durumuna geçmeyecektir. Bunun yerine, her ikisi de ayrı bir uyarı örneği olarak "tetiklenir" ve "çözümlendi" görüntülenir. 
