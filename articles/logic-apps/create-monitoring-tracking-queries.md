---
title: Azure Monitor günlüklerinde mantık uygulamaları için sorguları görüntüleme ve oluşturma
description: Azure Logic Apps için Azure Monitor günlüklerinde sorguları görüntüleme ve oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 46989ed2468469443d4e91a1834bc20b12c25a1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76908078"
---
# <a name="view-and-create-queries-for-monitoring-and-tracking-in-azure-monitor-logs-for-azure-logic-apps"></a>Azure Mantıksal Uygulamaları için Azure Monitor günlüklerinde izleme ve izleme sorgularını görüntüleme ve oluşturma

[Azure Monitor günlüklerinden](../log-analytics/log-analytics-overview.md) sonuçları üreten temel sorguları görüntüleyebilir ve sonuçları özel ölçütlerinize göre filtreleyen sorgular oluşturabilirsiniz. Örneğin, iletileri belirli bir değişim denetim numarasını temel alan bulabilirsiniz. Sorgular, farklı sonuçları görüntülemek istiyorsanız, edinebileceğiniz [Kusto sorgu dilini](https://aka.ms/LogAnalyticsLanguageReference)kullanır. Daha fazla bilgi için [Azure Monitor günlük sorgularına](../azure-monitor/log-query/query-language.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Log Analytics çalışma alanı. Log Analytics çalışma alanınız yoksa, [Log Analytics çalışma alanı oluşturmayı](../azure-monitor/learn/quick-create-workspace.md)öğrenin.

* Azure Monitor günlüğe kaydetme ile ayarlanmış ve bu bilgileri bir Log Analytics çalışma alanına gönderen bir mantık uygulaması. [Mantık uygulamanız için Azure Monitor günlüklerini nasıl ayarlayizleyeceğinizi](../logic-apps/monitor-logic-apps.md)öğrenin.

* Bir entegrasyon hesabı kullanıyorsanız, bu bilgileri bir Log Analytics çalışma alanına göndermek için Azure Monitor günlüğe kaydederek hesabı ayarladığınızdan emin olun. [Azure Monitör'ün tümleştirme hesabınız için günlüğe](../logic-apps/monitor-b2b-messages-log-analytics.md)kaydetmesini nasıl ayarlayizleyeceğinizi öğrenin.

## <a name="view-queries-behind-results"></a>Sonuçların arkasındaki sorguları görüntüleme

Çalışma alanı özetinizde sonuçları oluşturan sorguyu görüntülemek veya bunları görüntülemek için aşağıdaki adımları izleyin:

1. Herhangi bir sonuç sayfasında, en **altta, Tümünü Gör'i**seçin.

   ![Tüm sonuçları görün](./media/create-monitoring-tracking-queries/logic-app-see-all.png)

   Günlükler sayfası açılır ve önceki sonuçlar sayfasının arkasındaki sorguyu gösterir.

   ![Günlükler sayfası - sorgu görünümü](./media/create-monitoring-tracking-queries/view-query-behind-results.png)

1. **Günlükler** sayfasında şu seçenekleri seçebilirsiniz:

   * Sorgu sonuçlarını tablo olarak görüntülemek için, sorgu düzenleyicisinin altında **Tablo'yu**seçin.

   * Sorguyu değiştirmek için sorgu dizesini güncelleştirin ve tablodaki sonuçları görüntülemek için **Çalıştır'ı** seçin.

## <a name="create-your-own-query"></a>Kendi sorgunuzu oluşturun

Belirli özelliklere veya değerlere dayalı sonuçları bulmak veya filtrelemek için, boş bir sorgudan başlayarak kendi sorgunuzu oluşturabilir veya varolan bir sorguyı kullanabilirsiniz. Daha fazla bilgi için Azure [Monitor'daki günlük sorgularına başlayın' a](../azure-monitor/log-query/get-started-queries.md)bakın.

1. Azure [portalında](https://portal.azure.com)Günlük Analizi çalışma alanınızı bulun ve seçin.

1. Çalışma alanı menüsünde, **Genel**altında **Günlükler'i**seçin.

1. Boş bir sorgudan veya kullanılabilir varolan sorgulardan başlayın.

   * Varolan sorguların kullanılabilir olup olmadığını denetlemek için, sorgu araç çubuğunda, önceki sorgu çalıştırmalarından sorguları gösteren **Örnekler sorguları** > **Geçmişi'ni**veya önceden oluşturulmuş sorguları gösteren **Sorgu gezgini'ni**seçin.

     Örneğin, Logic Apps B2B çözümü önceden oluşturulmuş şu sorguları sağlar:

     ![Önceden oluşturulmuş sorgular "Logic Apps B2B" çözümü ile başlayın](./media/create-monitoring-tracking-queries/b2b-prebuilt-queries.png)

   * Boş bir sorgudan başlamak için, sorgu düzenleyicisinde, sorgunuzun [Kusto sorgu dilini](../azure-monitor/log-query/query-language.md) yazmaya başlayın.

     ![Boş sorguyla başlayın](./media/create-monitoring-tracking-queries/create-query-from-blank.png)

## <a name="next-steps"></a>Sonraki adımlar

* [AS2 izleme şemaları](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12 izleme şemaları](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Özel izleme şemaları](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)