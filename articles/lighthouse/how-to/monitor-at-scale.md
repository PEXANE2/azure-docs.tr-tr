---
title: Temsil edilen kaynakları ölçeklendirmeye göre izleme
description: Azure Izleyici günlüklerini, yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde nasıl verimli bir şekilde kullanacağınızı öğrenin.
ms.date: 07/07/2020
ms.topic: how-to
ms.openlocfilehash: 3be7494f929fc91e2b01bde0b4f26f7c2e4b907b
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86133486"
---
# <a name="monitor-delegated-resources-at-scale"></a>Temsil edilen kaynakları ölçeklendirmeye göre izleme

Hizmet sağlayıcı olarak, [Azure açık Thouse](../overview.md)için birden fazla müşteri kiracısından eklendi olabilirsiniz. Azure Mathouse, hizmet sağlayıcılarının aynı anda birkaç kiracıda bir ölçekte işlem gerçekleştirmesine olanak sağlayarak yönetim görevlerini daha verimli hale getirir.

Bu konu başlığı altında, [Azure Izleyici günlüklerinin](../../azure-monitor/platform/data-platform-logs.md) yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde nasıl kullanılacağı gösterilmektedir.

## <a name="create-log-analytics-workspaces"></a>Log Analytics çalışma alanları oluşturma

Veri toplamak için Log Analytics çalışma alanları oluşturmanız gerekir. Bu Log Analytics çalışma alanları, Azure Izleyici tarafından toplanan veriler için benzersiz ortamlardır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır.

Bu çalışma alanlarını doğrudan müşteri kiracılarında oluşturmanız önerilir. Bu şekilde, verileri kendi içine aktarılması yerine kiracılarında kalır. Bu Ayrıca, Log Analytics tarafından desteklenen herhangi bir kaynak ve hizmetin merkezi olarak izlenmesini sağlar ve bu sayede izlediğiniz veri türlerine daha fazla esneklik sunar.

[Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)kullanarak veya [Azure PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md)kullanarak [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md)kullanarak Log Analytics çalışma alanı oluşturabilirsiniz.

## <a name="deploy-policies-that-log-data"></a>Verileri günlüğe kaydetme ilkeleri dağıtma

Log Analytics çalışma alanlarınızı oluşturduktan sonra, tanılama verilerinin her Kiracıdaki uygun çalışma alanına gönderilmesi için [Azure ilkesini](../../governance/policy/index.yml) müşteri hiyerarşilerinizi genelinde dağıtabilirsiniz. Dağıttığınız tam ilkeler, izlemek istediğiniz kaynak türlerine bağlı olarak değişebilir.

İlke oluşturma hakkında daha fazla bilgi için bkz. [öğretici: uyumluluğu zorlamak için Ilke oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md). Bu [topluluk aracı](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) , seçtiğiniz belirli kaynak türlerini izlemek için ilkeler oluşturmanıza yardımcı olacak bir betik sağlar.

Hangi ilkelerin dağıtılacağını belirledikten sonra, [bunları uygun ölçekte uygun aboneliklerinize dağıtabilirsiniz](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Toplanan verileri analiz etme

İlkelerinizi dağıttıktan sonra, veriler her bir müşteri kiracısında oluşturduğunuz Log Analytics çalışma alanlarında günlüğe kaydedilir. Tüm yönetilen müşterilerle ilgili Öngörüler elde etmek için [Azure Izleyici çalışma kitapları](../../azure-monitor/platform/workbooks-overview.md) gibi araçları kullanarak birden çok veri kaynağından bilgi toplayabilir ve analiz edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure izleyici](../../azure-monitor/index.yml)hakkında bilgi edinin.
- [Azure Izleyici günlükleri](../../azure-monitor/platform/data-platform-logs.md)hakkında bilgi edinin.
- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
