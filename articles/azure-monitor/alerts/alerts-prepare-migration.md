---
title: Uyarılar geçişi için Runbook 'ları & mantıksal uygulamaları güncelleştirme
description: Web kancaları, Logic Apps ve Runbook 'larınızı gönüllü geçiş için hazırlamak üzere nasıl değiştireceğinizi öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2021
ms.subservice: alerts
ms.openlocfilehash: c88d0b8595434298eb564034a44665c5375457c4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101701051"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Mantıksal uygulamalarınızı ve runbook'larınızı klasik uyarı kurallarının geçişine hazırlama

> [!NOTE]
> [Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure izleyici 'deki klasik uyarılar, genel bulut kullanıcıları için devre dışı bırakılır, ancak yine de **31 Mayıs 2021** tarihine kadar sınırlı kullanımda. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.
>

Klasik uyarı kurallarınızı yeni uyarı kurallarına göre gönüllü olarak geçirmeyi seçerseniz, iki sistem arasında bazı farklılıklar vardır. Bu makalede bu farklılıklar ve değişikliğe nasıl hazırlanacağı açıklanmaktadır.

## <a name="api-changes"></a>API değişiklikleri

Klasik uyarı kuralları () oluşturan ve yöneten API 'ler `microsoft.insights/alertrules` , yeni ölçüm uyarıları () oluşturan ve yöneten API 'lerden farklıdır `microsoft.insights/metricalerts` . Hemen klasik uyarı kuralları oluşturup yönetiyorsanız, dağıtım betiklerinizi yeni API 'lerle çalışacak şekilde güncelleştirin.

Aşağıdaki tabloda hem klasik hem de yeni uyarılar için programlı arabirimlerin bir başvurusu verilmiştir:

| Dağıtım betiği türü | Klasik uyarılar | Yeni ölçüm uyarıları |
| ---------------------- | -------------- | ----------------- |
|REST API     | [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules)         | [Microsoft. Insights/metricalerts](/rest/api/monitor/metricalerts)       |
|Azure CLI     | [az Monitor Alert](/cli/azure/monitor/alert)        | [az Monitor ölçümleri uyarısı](/cli/azure/monitor/metrics/alert)        |
|PowerShell      | [Başvuru](/powershell/module/az.monitor/add-azmetricalertrule)       |  [Başvuru](/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Azure Resource Manager şablonu | [Klasik uyarılar için](./alerts-enable-template.md)|[Yeni ölçüm uyarıları için](./alerts-metric-create-templates.md)|

## <a name="notification-payload-changes"></a>Bildirim yükü değişiklikleri

Bildirim yükü biçimi, [Klasik uyarı kuralları](alerts-webhooks.md) ve [yeni ölçüm uyarıları](alerts-metric-near-real-time.md#payload-schema)arasında biraz farklıdır. Web kancası, mantıksal uygulama veya Runbook eylemleriyle klasik uyarı kurallarınızı varsa, yeni yük biçimini kabul etmek için hedefleri güncelleştirmeniz gerekir.

Web kancası yük alanlarını klasik biçimden yeni biçime eşlemek için aşağıdaki tabloyu kullanın:

| Bildirim uç noktası türü | Klasik uyarılar | Yeni ölçüm uyarıları |
| -------------------------- | -------------- | ----------------- |
|Uyarı etkinleştirildi veya çözüldü mı?    | **durumlarına**       | **Data. Status** |
|Uyarı hakkında bağlamsal bilgiler     | **bağlam**        | **Data. Context**        |
|Uyarının etkinleştirildiği veya çözümlendiği zaman damgası     | **Context. Timestamp**       | **Data. Context. Timestamp**        |
| Uyarı kuralı KIMLIĞI | **context.id** | **data.context.id** |
| Uyarı kuralının adı | **context.name** | **data.context.name** |
| Uyarı kuralının açıklaması | **Context. Description** | **Data. Context. Description** |
| Uyarı kuralı koşulu | **Context. Condition** | **Data. Context. Condition** |
| Ölçüm adı | **Context. Condition. metricName** | **Data. Context. Condition. allOf [0]. metricName** |
| Zaman toplama (ölçüm değerlendirme penceresinde nasıl toplanır)| **Context. Condition. Timetoplamasını** | **Context. Condition. Timetoplamasını** |
| Değerlendirme süresi | **Context. Condition. windowSize** | **Data. Context. Condition. windowSize** |
| İşleç (toplanan ölçüm değeri eşiğe göre nasıl karşılaştırılır) | **Context. Condition. işleci** | **Data. Context. Condition. işleci** |
| Eşik | **Context. Condition. Threshold** | **Data. Context. Condition. allOf [0]. eşik** |
| Ölçüm değeri | **Context. Condition. metricValue** | **Data. Context. Condition. allOf [0]. metricValue** |
| Abonelik Kimliği | **Context. SubscriptionID** | **Data. Context. SubscriptionID** |
| Etkilenen kaynağın kaynak grubu | **Context. resourceGroup** | **Data. Context. resourceGroup** |
| Etkilenen kaynağın adı | **Context. resourceName** | **Data. Context. resourceName** |
| Etkilenen kaynağın türü | **Context. resourceType** | **Data. Context. resourceType** |
| Etkilenen kaynağın kaynak KIMLIĞI | **Context. ResourceID** | **Data. Context. ResourceID** |
| Portal kaynağı özet sayfasına doğrudan bağlantı | **Context. portalLink** | **Data. Context. portalLink** |
| Web kancası veya mantıksal uygulamaya geçirilecek özel yük alanları | **özelliklerinin** | **Data. Properties** |

, Görebileceğiniz gibi yükleri benzerdir. Aşağıdaki bölümde şunlar sunulmaktadır:

- Mantıksal uygulamaları yeni biçimle çalışacak şekilde değiştirme hakkında ayrıntılar.
- Yeni uyarılar için bildirim yükünü çözümleyen bir runbook örneği.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Bir mantıksal uygulamayı ölçüm Uyarısı bildirimi alacak şekilde değiştirme

Klasik uyarılarla Logic Apps kullanıyorsanız, yeni ölçüm uyarıları yükünü ayrıştırmak için mantıksal uygulama kodunuzu değiştirmeniz gerekir. Şu adımları izleyin:

1. Yeni bir mantıksal uygulama oluşturun.

1. "Azure Izleyici-ölçüm uyarısı Işleyicisi" şablonunu kullanın. Bu şablonda uygun şema tanımlı bir **http isteği** tetikleyicisi vardır.

    ![Ekran görüntüsünde iki düğme, boş mantıksal uygulama ve Azure Izleyici – ölçüm uyarısı Işleyicisi gösterilmektedir.](media/alerts-prepare-migration/logic-app-template.png "Ölçüm uyarısı şablonu")

1. İşlem mantığınızı barındırmak için bir eylem ekleyin.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Ölçüm uyarı bildirimi alan bir Otomasyon Runbook 'u kullanma

Aşağıdaki örnek, runbook 'unuzla kullanmak için PowerShell kodu sağlar. Bu kod, hem klasik ölçüm uyarı kuralları hem de yeni ölçüm uyarısı kuralları için yükleri ayrıştırabilirler.

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

Bir uyarı tetiklendiğinde bir sanal makineyi durduran runbook 'un tam bir örneği için bkz. [Azure Otomasyonu belgeleri](../../automation/automation-create-alert-triggered-runbook.md).

## <a name="partner-integration-via-webhooks"></a>Web kancaları aracılığıyla iş ortağı tümleştirmesi

[Klasik uyarılarla tümleştirilen iş ortaklarımızın](../partners.md) çoğu, tümleştirmelerine göre zaten yeni ölçüm uyarılarını desteklemektedir. Yeni ölçüm uyarıları ile zaten çalışan bilinen tümleştirmeler şunlardır:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Sıgnl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Burada listelenmeyen bir iş ortağı tümleştirmesi kullanıyorsanız, sağlayıcıya yeni ölçüm uyarıları ile çalıştığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)