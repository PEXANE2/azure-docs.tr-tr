---
title: Azure Sentinel ile çalışırken faydalı kaynaklar | Microsoft Docs
description: Bu belge, Azure Sentinel ile çalışırken yararlı kaynakların bir listesini sağlar.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 9b4c8e38-c986-4223-aa24-a71b01cb15ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2019
ms.author: rkarlin
ms.openlocfilehash: 98c76b2be6a5cc801f9e7b31357cbeb158a966fe
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771300"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Azure Sentinel ile çalışmaya yönelik faydalı kaynaklar



Bu makalede, Azure Sentinel ile çalışma hakkında daha fazla bilgi almanıza yardımcı olabilecek kaynaklar listelenmektedir.

Azure Logic Apps bağlayıcılar: <https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Denetim ve raporlama
Azure Sentinel 'in denetim günlükleri [Azure etkinlik günlüklerinde](../azure-monitor/platform/platform-logs-overview.md)tutulur.

Aşağıdaki desteklenen işlemler denetlenebilir.

|İşlem adı|    Kaynak türü|
|----|----|
|Çalışma kitabı oluştur veya güncelleştir  |Microsoft. Insights/çalışma kitapları|
|Çalışma kitabını Sil    |Microsoft. Insights/çalışma kitapları|
|Iş akışını ayarla   |Microsoft.Logic/workflows|
|Iş akışını Sil    |Microsoft.Logic/workflows|
|Kayıtlı arama oluştur    |Microsoft. Operationalınsights/çalışma alanları/Savedaramalar|
|Kayıtlı aramayı Sil    |Microsoft. Operationalınsights/çalışma alanları/Savedaramalar|
|Uyarı kurallarını Güncelleştir |Microsoft. Securityınsights/alertRules|
|Uyarı kurallarını Sil |Microsoft. Securityınsights/alertRules|
|Uyarı kuralı yanıt eylemlerini Güncelleştir |Microsoft. Securityınsights/alertRules|
|Uyarı kuralı yanıt eylemlerini Sil |Microsoft. Securityınsights/alertRules|
|Yer Imlerini güncelleştirme   |Microsoft. Securityınsights/yer işaretleri|
|Yer Imlerini Sil   |Microsoft. Securityınsights/yer işaretleri|
|Güncelleştirme çalışmaları   |Microsoft. Securityınsights/durumlar|
|Durum araştırmasını Güncelleştir  |Microsoft. Securityınsights/durumlar|
|Case açıklamaları oluşturma   |Microsoft. Securityınsights/durumlar|
|Veri bağlayıcılarını güncelleştirme |Microsoft. Securityınsights/veri bağlayıcıları|
|Veri bağlayıcıları silme |Microsoft. Securityınsights/veri bağlayıcıları|
|Ayarları Güncelleştir    |Microsoft. Securityınsights/ayarlar|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Azure Sentinel 'de denetim ve raporlama verilerini görüntüleme

Azure etkinlik günlüğünden bu verileri, daha sonra araştırma ve analiz gerçekleştirebileceğiniz bir Azure Sentinel 'e aktararak görüntüleyebilirsiniz.

1. [Azure etkinlik](connect-azure-activity.md) veri kaynağını bağlayın. Bunu yaptıktan sonra, denetim olayları AzureActivity adlı **Günlükler** ekranında yeni bir tabloya akışla kaydedilir.
2. Ardından, diğer tüm tablolar gibi KQL kullanarak verileri sorgulayın.



## <a name="vendor-documentation"></a>Satıcı belgeleri

| **Satıcı**  | **Azure Sentinel 'de olay kullanma** | **Bağlantı**|
|----|----|----|
| GitHub| Topluluk sayfasına erişmek için kullanılır| <https://github.com/Azure/Azure-Sentinel> |
| PaloAlto| CEF 'yi yapılandırma| <https://www.paloaltonetworks.com/documentation/misc/cef.html>|
| PluralSight | Kusto sorgu dili Kursu| [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)|

## <a name="blogs-and-forums"></a>Bloglar ve Forumlar

Sorularınızı Azure Sentinel için [Techcommunity alanına](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) gönderin.

[Techcommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) ve [Microsoft Azure](https://azure.microsoft.com/blog/tag/azure-sentinel/)Azure Sentinel blog yayınlarını görüntüleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel ile çalışırken yararlı olan kaynakların bir listesini aldınız. [Microsoft Azure Güvenlik ve uyumluluk blogunu](https://blogs.msdn.com/b/azuresecurity/)Azure Güvenlik ve uyumluluğu hakkında daha fazla bilgi bulacaksınız.
