---
title: Yetkin kaynakları ölçekte izleme
description: Azure Monitör Günlükleri'ni, yönetmekte olduğunuz müşteri kiracıları arasında ölçeklenebilir bir şekilde nasıl etkili bir şekilde kullanacağınızı öğrenin.
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 217df3c55ab54b6569bae8cacb338764ecb7125d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122902"
---
# <a name="monitor-delegated-resources-at-scale"></a>Yetkin kaynakları ölçekte izleme

Bir hizmet sağlayıcısı olarak, Azure temsilci kaynak yönetimi için birden çok müşteri kiracısı yerleşik olabilir. [Azure Deniz Feneri,](../overview.md) hizmet sağlayıcıların aynı anda birden fazla kiracı da ölçekte işlem gerçekleştirmesine olanak sağlayarak yönetim görevlerini daha verimli hale getirir.

Bu konu, yönettiğiniz müşteri kiracıları arasında [Azure Monitör Günlükleri'ni](../../azure-monitor/platform/data-platform-logs.md) ölçeklenebilir bir şekilde nasıl kullanacağınızı gösterir.

## <a name="create-log-analytics-workspaces"></a>Günlük Analizi çalışma alanları oluşturma

Veri toplamak için Log Analytics çalışma alanları oluşturmanız gerekir. Bu Log Analytics çalışma alanları, Azure Monitor tarafından toplanan veriler için benzersiz ortamlardır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolayabilmek üzere yapılandırılır.

Bu çalışma alanlarını doğrudan müşteri kiracılarında oluşturmanızı öneririz. Bu şekilde verileri sizinkine ihraç edilmek yerine kiracılarında kalır. Bu aynı zamanda Log Analytics tarafından desteklenen kaynakların veya hizmetlerin merkezi olarak izlenmesine olanak sağlayarak, hangi veri türlerini izlediğiniz konusunda daha fazla esneklik sağlar.

[Azure portalını](../../azure-monitor/learn/quick-create-workspace.md)kullanarak , [Azure CLI'yi](../../azure-monitor/learn/quick-create-workspace-cli.md)kullanarak veya [Azure PowerShell'i](../../azure-monitor/learn/quick-create-workspace-posh.md)kullanarak bir Günlük Analizi çalışma alanı oluşturabilirsiniz.

## <a name="deploy-policies-that-log-data"></a>Verileri günlüğe kaydeden ilkeleri dağıtma

Log Analytics çalışma alanlarınızı oluşturduktan sonra, tanılama verilerinin her kiracıdaki uygun çalışma alanına gönderilmesi için [Azure İlkesini](../../governance/policy/index.yml) müşteri hiyerarşileriniz arasında dağıtabilirsiniz. Dağıtdığınız tam ilkeler, izlemek istediğiniz kaynak türlerine bağlı olarak değişebilir.

İlkeler oluşturma hakkında daha fazla bilgi edinmek için [bkz.](../../governance/policy/tutorials/create-and-manage.md) Bu [topluluk aracı,](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/tools/azure-diagnostics-policy-generator) seçtiğiniz belirli kaynak türlerini izlemek için ilkeler oluşturmanıza yardımcı olacak bir komut dosyası sağlar.

Hangi ilkelerin dağıtılacayacağını belirlediğinizde, [bunları yetkiverilen aboneliklerinize ölçekte dağıtabilirsiniz.](policy-at-scale.md)

## <a name="analyze-the-gathered-data"></a>Toplanan verileri analiz edin

İlkelerinizi dağıttıktan sonra, veriler her müşteri kiracısında oluşturduğunuz Log Analytics çalışma alanlarında günlüğe kaydedilir. Tüm yönetilen müşteriler arasında öngörüler elde etmek için, birden çok veri kaynağından bilgi toplamak ve analiz etmek için [Azure Monitor Çalışma Kitapları](../../azure-monitor/platform/workbooks-overview.md) gibi araçları kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Monitör](../../azure-monitor/index.yml)hakkında bilgi edinin.
- Azure [Monitör Günlükleri](../../azure-monitor/platform/data-platform-logs.md)hakkında bilgi edinin.
- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
