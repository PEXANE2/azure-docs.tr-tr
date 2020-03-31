---
title: Mantıksal uygulamalarınızı ve runbook'larınızı güncelleyerek Azure Monitörü'ne klasik uyarılar geçişine hazırlanın
description: Gönüllü geçişe hazırlanmak için web hook'larınızı, mantık uygulamalarınızı ve runbook'larınızı nasıl değiştirip değiştirebilirsiniz öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 03/19/2018
ms.subservice: alerts
ms.openlocfilehash: 9219e105acb98424939030af76b526d475585619
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665601"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Mantıksal uygulamalarınızı ve runbook'larınızı klasik uyarı kurallarının geçişine hazırlama

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure Monitor'daki klasik uyarılar Eylül 2019'da kullanımdan kaldırılmaktadır (başlangıçta Temmuz 2019'du). Azure portalında, klasik uyarı kuralları kullanan ve geçişi kendileri tetiklemek isteyen müşteriler için bir geçiş aracı kullanılabilir.

> [!NOTE]
> Geçiş aracının kullanıma sunulmasındaki gecikme nedeniyle, klasik uyarı geçişi için emeklilik tarihi 30 Haziran 2019 tarihinden itibaren 31 Ağustos 2019'a uzatıldı.

Klasik uyarı kurallarınızı gönüllü olarak yeni uyarı kurallarına geçirmeyi seçerseniz, iki sistem arasında bazı farklar olduğunu unutmayın. Bu makalede, bu farklılıklar ve nasıl değişiklik için hazırlayabilirsiniz açıklar.

## <a name="api-changes"></a>API değişiklikleri

Klasik uyarı kurallarını oluşturan ve`microsoft.insights/alertrules`yöneten API'ler, yeni metrik uyarıları oluşturan`microsoft.insights/metricalerts`ve yöneten API'lerden farklıdır ( ). Bugün klasik uyarı kurallarını programlı bir şekilde oluşturur ve yönetirseniz, dağıtım komut dosyalarınızı yeni API'lerle çalışacak şekilde güncelleştirin.

Aşağıdaki tablo, hem klasik hem de yeni uyarılar için programlı arabirimlere bir başvurudur:

|         |Klasik uyarılar  |Yeni metrik uyarılar |
|---------|---------|---------|
|REST API     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure CLI     | [az monitör uyarısı](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [az monitör ölçümleri uyarısı](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Başvuru](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |  [Başvuru](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Azure Resource Manager şablonu | [Klasik uyarılar için](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Yeni metrik uyarılar için](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Bildirim yükü değişiklikleri

Bildirim yükü biçimi klasik uyarı [kuralları](alerts-webhooks.md) ve [yeni metrik uyarılar](alerts-metric-near-real-time.md#payload-schema)arasında biraz farklıdır. Klasik uyarı kuralları tarafından tetiklenen herhangi bir webhook, mantık uygulaması veya runbook eyleminiz varsa, yeni metrik uyarıların yük biçimini kabul etmek için bu bildirim uç noktalarını güncelleştirmeniz gerekir.

Webhook yük alanlarını klasik biçimden yeni biçime eşlemek için aşağıdaki tabloyu kullanın:

|  |Klasik uyarılar  |Yeni metrik uyarılar |
|---------|---------|---------|
|Uyarı etkinleştirildi mi yoksa çözüldü mü?    | **durum**       | **data.status** |
|Uyarı hakkında bağlamsal bilgiler     | **Bağlam**        | **data.context**        |
|Uyarının etkinleştirildiği veya çözüldüğü saat damgası     | **context.timestamp**       | **data.context.timestamp**        |
| Uyarı kuralı kimliği | **context.id** | **data.context.id** |
| Uyarı kuralı adı | **context.name** | **data.context.name** |
| Uyarı kuralının açıklaması | **context.description** | **data.context.description** |
| Uyarı kuralı koşulu | **bağlam.koşul** | **data.context.condition** |
| Ölçüm adı | **context.condition.metricName** | **data.context.condition.allOf[0].metricName** |
| Zaman toplama (metrik değerlendirme penceresi üzerinde nasıl toplanır)| **context.condition.timeAggregation** | **context.condition.timeAggregation** |
| Değerlendirme dönemi | **context.condition.windowSize** | **data.context.condition.windowSize** |
| İşletici (toplanan metrik değer in eşikle karşılaştırıldığında nasıl) | **context.condition.operator** | **data.context.condition.operator** |
| Eşik | **context.condition.threshold** | **data.context.condition.allOf[0].threshold** |
| Metrik değer | **context.condition.metricValue** | **data.context.condition.allOf[0].metricValue** |
| Abonelik Kimliği | **context.subscriptionId** | **data.context.subscriptionId** |
| Etkilenen kaynağın kaynak grubu | **context.resourceGroup** | **data.context.resourceGroup** |
| Etkilenen kaynağın adı | **context.resourceName** | **data.context.resourceName** |
| Etkilenen kaynağın türü | **context.resourceType** | **data.context.resourceType** |
| Etkilenen kaynağın kaynak kimliği | **context.resourceId** | **data.context.resourceId** |
| Portal kaynak özeti sayfasına doğrudan bağlantı | **context.portalLink** | **data.context.portalLink** |
| Webhook veya mantık uygulamasına geçirilecek özel yük alanları | **Özellikler** | **data.properties** |

Gördüğünüz gibi yükler benzer. Aşağıdaki bölüm şunları sunar:

- Mantık uygulamalarını yeni biçimle çalışacak şekilde değiştirme yle ilgili ayrıntılar.
- Yeni uyarılar için bildirim yükünü ayrışdıran bir runbook örneği.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Metrik uyarı bildirimi almak için bir mantık uygulamasını değiştirme

Klasik uyarılara sahip mantık uygulamaları kullanıyorsanız, yeni metrik uyarıların yükünü ayrışdırmak için mantık uygulama kodunuzu değiştirmeniz gerekir. Şu adımları uygulayın:

1. Yeni bir mantık uygulaması oluşturun.

1. "Azure Monitörü - Ölçümler Uyarı İşleyicisi" şablonuna kullanın. Bu şablonda, tanımlanan uygun şema ile bir **HTTP istek** tetikleyicisi vardır.

    ![mantık-uygulama şablonu](media/alerts-migration/logic-app-template.png "Metrik uyarı şablonu")

1. İşlem mantığınızı barındırmak için bir eylem ekleyin.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Metrik uyarı bildirimi alan bir otomasyon runbook'u kullanma

Aşağıdaki örnek, çalışma kitabınızda kullanılacak PowerShell kodunu sağlar. Bu kod, hem klasik metrik uyarı kuralları hem de yeni metrik uyarı kuralları için yükleri ayrışdırabilir.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

Bir uyarı tetiklendiğinde sanal makineyi durduran runbook'un tam bir örneği için [Azure Otomasyon belgelerine](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook)bakın.

## <a name="partner-integration-via-webhooks"></a>Webhooks ile ortak entegrasyonu

Klasik [uyarılarla entegre olan ortaklarımızın](https://docs.microsoft.com/azure/azure-monitor/platform/partners) çoğu, entegrasyonları aracılığıyla zaten yeni metrik uyarıları destekler. Zaten yeni metrik uyarıları ile çalışan bilinen tümleştirmeler şunlardır:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Burada listelenmemiş bir iş ortağı tümleştirmesi kullanıyorsanız, tümleştirme sağlayıcısıyla tümleştirmenin yeni metrik uyarılarıyla çalıştığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)
