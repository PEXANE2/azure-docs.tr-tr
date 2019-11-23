---
title: Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri | Microsoft Docs
description: Azure Güvenlik Merkezi'nin, Azure kaynaklarınızın genel güvenliğini geliştirmek amacıyla iş ortaklarıyla nasıl tümleştirildiğini öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: 435ae9f08f718a9310fd1687fb7859058edf8b45
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74384240"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Azure Güvenlik Merkezi'ndeki tümleşik güvenlik çözümleri
Bu belge Azure Güvenlik Merkezi'ne bağlanmış olan güvenlik çözümlerini yönetmenize ve yenilerini eklemenize yardımcı olur.

> [!NOTE]
> A subset of security solutions has been retired on July 31st, 2019. For more information and alternative services, see [Retirement of Security Center features (July 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Tümleşik Azure güvenlik çözümleri
Güvenlik Merkezi, Azure'daki tümleşik güvenlik çözümlerini etkinleştirmeyi kolaylaştırır. Faydaları şunlardır:

- **Basitleştirilmiş dağıtım**: Güvenlik Merkezi, tümleşik iş ortağı çözümlerinin kolay bir şekilde sağlanmasını mümkün kılar. For solutions like antimalware and vulnerability assessment, Security Center can provision the agent on your virtual machines. For firewall appliances, Security Center can take care of much of the network configuration required.
- **Tümleşik algılamalar**: İş ortağı çözümlerinden gelen güvenlik olayları otomatik olarak toplanır, birleştirilir ve Güvenlik Merkezi uyarıları ve olaylarının bir parçası olarak görüntülenir. Gelişmiş tehdit algılama özellikleri sağlanması amacıyla, bu olaylar diğer kaynaklardan alınan algılamalarla da birleştirilir.
- **Birleşik sistem durumu izleme ve yönetim**: Müşteriler tüm iş ortağı çözümlerini bir bakışta izlemek için tümleşik sistem durumu olaylarını kullanabilir. İş ortağı çözümü kullanarak gelişmiş kuruluma kolayca erişim olanağıyla temel yapılandırma seçeneği sunulur.

Currently, integrated security solutions include vulnerability assessment by [Qualys](https://www.qualys.com/public-cloud/#azure) and [Rapid7](https://www.rapid7.com/products/insightvm/) and Microsoft Application Gateway Web application firewall.

> [!NOTE]
> Security Center does not install the Microsoft Monitoring Agent on partner virtual appliances because most security vendors prohibit external agents running on their appliances.
>
>

## <a name="how-security-solutions-are-integrated"></a>Güvenlik çözümlerinin tümleştirilme şekli
Güvenlik Merkezinden dağıtılan Azure güvenlik çözümleri otomatik olarak bağlanır. You can also connect other security data sources, including computers running on-premises or in other clouds.

![İş ortağı çözümlerini tümleştirme](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Tümleşik Azure güvenlik çözümlerini ve diğer veri kaynaklarını yönetme

1. [Azure Portal](https://azure.microsoft.com/features/azure-portal/)’ında oturum açın.

2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

3. Güvenlik Merkezi menüsünde **Güvenlik çözümleri**’ni seçin.

   ![Güvenlik Merkezine Genel Bakış](./media/security-center-partner-integration/overview.png)

In **Security solutions**, you can see the health of integrated Azure security solutions and run basic management tasks.

### <a name="connected-solutions"></a>Bağlantılı çözümler

The **Connected solutions** section includes security solutions that are currently connected to Security Center. It also shows the health status of each solution.  

![Bağlantılı çözümler](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

The status of a partner solution can be:

* Healthy (green) - no health issues.
* Unhealthy (red) - there's a health issue that requires immediate attention.
* Health issues (orange) - the solution has stopped reporting its health.
* Not reported (gray) - the solution hasn't reported anything yet and no health data is available. A solution's status may be unreported if it was connected recently and is still deploying.

> [!NOTE]
> If health status data is not available, Security Center shows the date and time of the last event received to indicate whether the solution is reporting or not. If no health data is available and no alerts were received within the last 14 days, Security Center indicates that the solution is unhealthy or not reporting.
>
>

1. Select **VIEW** for additional information and options such as:

   - **Solution console**. Opens the management experience for this solution.
   - **Link VM**. Opens the Link Applications page. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   - **Delete solution**.
   - **Configure**.

   ![İş ortağı çözümü ayrıntısı](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Bulunan çözümler

Security Center automatically discovers security solutions running in Azure but not connected to Security Center and displays the solutions in the **Discovered solutions** section. These  solutions include Azure solutions, like [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), and partner solutions.

> [!NOTE]
> Güvenlik Merkezi’nin Standart katmanı, bulunan çözümler özelliği için abonelik düzeyinde gereklidir. See [Pricing](security-center-pricing.md) to learn more about the pricing tiers.
>
>

Select **CONNECT** under a solution to integrate with Security Center and be notified of security alerts.

### <a name="add-data-sources"></a>Veri kaynağı ekleme

**Veri kaynakları ekleyin** bölümünde bağlanabilecek diğer veri kaynakları bulunur. Bu kaynaklardan veri ekleme talimatları için **EKLE**'ye tıklayın.

![Veri kaynakları](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Exporting data to a SIEM

You can configure your SIEMs or other monitoring tools to receive Azure Security Center events.

All events from Azure Security Center are published to Azure Monitor's Azure [Activity log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Azure Monitor uses [a consolidated pipeline](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) to  stream the data to an Event Hub where it can then be pulled into your monitoring tool.

The next sections describe how you can configure data to be streamed to an event hub. The steps assume that you already have Azure Security Center configured in your Azure subscription.

### <a name="high-level-overview"></a>Yüksek düzey genel bakış

![High-Level overview](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>What is the Azure security data exposed to SIEM?

In this version, we expose the [security alerts.](../security-center/security-center-managing-and-responding-alerts.md) In upcoming releases, we will enrich the data set with security recommendations.

### <a name="how-to-set-up-the-pipeline"></a>How to set up the pipeline

#### <a name="create-an-event-hub"></a>Olay Hub'ı oluşturma

Before you begin, [create an Event Hubs namespace](../event-hubs/event-hubs-create.md) - the destination for all your monitoring data.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream the Azure Activity Log to Event Hubs

See the following article [stream activity log to Event Hubs](../azure-monitor/platform/activity-logs-stream-event-hubs.md)

#### <a name="install-a-partner-siem-connector"></a>Install a partner SIEM connector 

Routing your monitoring data to an Event Hub with Azure Monitor enables you to easily integrate with partner SIEM and monitoring tools.

See the following article for the list of [supported SIEMs](../azure-monitor/platform/resource-logs-stream-event-hubs.md#what-you-can-do-with-resource-logs-sent-to-an-event-hub)

### <a name="example-for-querying-data"></a>Example for Querying data 

Here are some Splunk queries you can use to pull alert data:

| **Description of Query** | **Sorgu** |
|----|----|
| All Alerts| index=main Microsoft.Security/locations/alerts|
| Summarize count of operations by their name| index=main sourcetype="amal:security" \| table operationName \| stats count by operationName|
| Get Alerts info: Time, Name, State, ID, and Subscription | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde iş ortağı çözümlerinin nasıl tümleştirileceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Güvenlik Merkezi kullanımı ile ilgili sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
