---
title: Temsil edilen kaynakları ölçeklendirmeye göre izleme
description: Azure Izleyici günlüklerini, yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde nasıl verimli bir şekilde kullanacağınızı öğrenin.
ms.date: 02/11/2021
ms.topic: how-to
ms.openlocfilehash: aadd14bb3e4aad61fb2afc0735b5714deedfe301
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100593118"
---
# <a name="monitor-delegated-resources-at-scale"></a>Temsil edilen kaynakları ölçeklendirmeye göre izleme

Hizmet sağlayıcı olarak, [Azure açık Thouse](../overview.md)için birden fazla müşteri kiracısından eklendi olabilirsiniz. Azure Mathouse, hizmet sağlayıcılarının aynı anda birkaç kiracıda bir ölçekte işlem gerçekleştirmesine olanak sağlayarak yönetim görevlerini daha verimli hale getirir.

Bu konu başlığı altında, [Azure Izleyici günlüklerinin](../../azure-monitor/logs/data-platform-logs.md) yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde nasıl kullanılacağı gösterilmektedir. Bu konudaki hizmet sağlayıcılarına ve müşterilere başvurduğumuz halde, bu kılavuz [birden çok kiracıyı yönetmek Için Azure açık Thouse kullanan kuruluşlar](../concepts/enterprise.md)için de geçerlidir.

> [!NOTE]
> Yönetim kiracılarınızdaki kullanıcılara, temsilcili müşteri aboneliklerinizde [Log Analytics çalışma alanlarını yönetmek için gerekli rollerin](../../azure-monitor/logs/manage-access.md#manage-access-using-azure-permissions) verildiğinden emin olun.

## <a name="create-log-analytics-workspaces"></a>Log Analytics çalışma alanları oluşturma

Veri toplamak için Log Analytics çalışma alanları oluşturmanız gerekir. Bu Log Analytics çalışma alanları, Azure Izleyici tarafından toplanan veriler için benzersiz ortamlardır. Her çalışma alanının kendi veri deposu ve yapılandırması vardır ve veri kaynakları ve çözümleri verilerini belirli bir çalışma alanında depolamak üzere yapılandırılır.

Bu çalışma alanlarını doğrudan müşteri kiracılarında oluşturmanız önerilir. Bu şekilde, verileri kendi içine aktarılması yerine kiracılarında kalır. Bu Ayrıca, Log Analytics tarafından desteklenen herhangi bir kaynak ve hizmetin merkezi olarak izlenmesini sağlar ve bu sayede izlediğiniz veri türlerine daha fazla esneklik sunar.

> [!TIP]
> Log Analytics çalışma alanındaki verilere erişmek için kullanılan herhangi bir Otomasyon hesabının, çalışma alanıyla aynı kiracıda oluşturulması gerekir.

[Azure CLI](../../azure-monitor/logs/quick-create-workspace-cli.md)kullanarak veya [Azure PowerShell](../../azure-monitor/logs/powershell-workspace-configuration.md)kullanarak [Azure Portal](../../azure-monitor/logs/quick-create-workspace.md)kullanarak Log Analytics çalışma alanı oluşturabilirsiniz.

> [!IMPORTANT]
> Tüm çalışma alanları müşteri kiracısında oluşturulsa da Microsoft. Insights kaynak sağlayıcısı 'nın, yönetim kiracısındaki bir abonelikte kayıtlı olması gerekir.

## <a name="deploy-policies-that-log-data"></a>Verileri günlüğe kaydetme ilkeleri dağıtma

Log Analytics çalışma alanlarınızı oluşturduktan sonra, tanılama verilerinin her Kiracıdaki uygun çalışma alanına gönderilmesi için [Azure ilkesini](../../governance/policy/index.yml) müşteri hiyerarşilerinizi genelinde dağıtabilirsiniz. Dağıttığınız tam ilkeler, izlemek istediğiniz kaynak türlerine bağlı olarak değişebilir.

İlke oluşturma hakkında daha fazla bilgi için bkz. [öğretici: uyumluluğu zorlamak için Ilke oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md). Bu [topluluk aracı](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) , seçtiğiniz belirli kaynak türlerini izlemek için ilkeler oluşturmanıza yardımcı olacak bir betik sağlar.

Hangi ilkelerin dağıtılacağını belirledikten sonra, [bunları uygun ölçekte uygun aboneliklerinize dağıtabilirsiniz](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Toplanan verileri analiz etme

İlkelerinizi dağıttıktan sonra, veriler her bir müşteri kiracısında oluşturduğunuz Log Analytics çalışma alanlarında günlüğe kaydedilir. Tüm yönetilen müşterilerle ilgili Öngörüler elde etmek için [Azure Izleyici çalışma kitapları](../../azure-monitor/visualize/workbooks-overview.md) gibi araçları kullanarak birden çok veri kaynağından bilgi toplayabilir ve analiz edebilirsiniz.

## <a name="view-alerts-across-customers"></a>Müşteriler arasında uyarıları görüntüleme

Yönettiğiniz müşteri kiracılarında Temsilcili abonelikler için [Uyarı](../../azure-monitor/alerts/alerts-overview.md) görüntüleyebilirsiniz.

Yönetim kiracınızdan, Azure portal veya API 'Ler ile yönetim araçları aracılığıyla [etkinlik günlüğü uyarılarını oluşturabilir, görüntüleyebilir ve yönetebilirsiniz](../../azure-monitor/platform/alerts-activity-log.md) .

Uyarıları birden fazla müşteri genelinde otomatik olarak yenilemek için, uyarıları filtrelemek üzere bir [Azure Kaynak Grafiği](../../governance/resource-graph/overview.md) sorgusu kullanın. Sorguyu panonuza sabitleyebilir ve tüm uygun müşterileri ve abonelikleri seçebilirsiniz. Örneğin, aşağıdaki sorguda önem derecesi 0 ve 1 uyarı görüntülenir, her 60 dakikada bir yenileme yapılır.

```kusto
alertsmanagementresources
| where type == "microsoft.alertsmanagement/alerts"
| where properties.essentials.severity =~ "Sev0" or properties.essentials.severity =~ "Sev1"
| where properties.essentials.monitorCondition == "Fired"
| where properties.essentials.startDateTime > ago(60m)
| project StartTime=properties.essentials.startDateTime,name,Description=properties.essentials.description, Severity=properties.essentials.severity, subscriptionId
| sort by tostring(StartTime)
```

## <a name="next-steps"></a>Sonraki adımlar

- GitHub 'daki [etki alanı çalışma kitabına göre etkinlik günlüklerini](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) deneyin.
- Birden çok Log Analytics çalışma alanı genelinde [güncelleştirme yönetimi günlüklerini sorgulayarak](../../automation/update-management/query-logs.md) düzeltme eki uyumluluk raporlamasını Izleyen bu [MVP tarafından oluşturulan örnek çalışma kitabını](https://github.com/scautomation/Azure-Automation-Update-Management-Workbooks)keşfet. 
- Diğer [çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
