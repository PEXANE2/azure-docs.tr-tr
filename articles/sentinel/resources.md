---
title: Azure Sentinel ile çalışırken yararlı kaynaklar| Microsoft Dokümanlar
description: Bu belge, Azure Sentinel ile çalışırken size yararlı kaynakların bir listesini sağlar.
services: sentinel
documentationcenter: na
author: yelevin
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
ms.author: yelevin
ms.openlocfilehash: 2110d3319cebf693ef06deec26a29fa655e35035
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585280"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Azure Sentinel ile çalışmak için kullanışlı kaynaklar



Bu makalede, Azure Sentinel ile çalışma hakkında daha fazla bilgi edinmenize yardımcı olabilecek kaynaklar listelanmaktadır.

Azure Mantık Uygulamaları bağlayıcıları:<https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Denetim ve raporlama
Azure Sentinel denetim günlükleri [Azure Etkinlik Günlükleri'nde](../azure-monitor/platform/platform-logs-overview.md)tutulur.

Desteklenen aşağıdaki işlemler denetlenebilir.

|İşlem adı|    Kaynak türü|
|----|----|
|Çalışma kitabı oluşturma veya güncelleştirme  |Microsoft.Insights/çalışma kitapları|
|Çalışma Kitabını Sil    |Microsoft.Insights/çalışma kitapları|
|İş Akışını Ayarla   |Microsoft.Logic/iş akışları|
|İş Akışını Silme    |Microsoft.Logic/iş akışları|
|Kayıtlı Arama Oluşturma    |Microsoft.OperationalInsights/çalışma alanları/savedSearches|
|Kaydedilen Aramayı Sil    |Microsoft.OperationalInsights/çalışma alanları/savedSearches|
|Uyarı Kurallarını Güncelleştir |Microsoft.SecurityInsights/alertRules|
|Uyarı Kurallarını Sil |Microsoft.SecurityInsights/alertRules|
|Uyarı Kuralı Yanıt Eylemlerini Güncelleştir |Microsoft.SecurityInsights/alertRules/actions|
|Uyarı Kuralı Yanıt Eylemlerini Sil |Microsoft.SecurityInsights/alertRules/actions|
|Yer İşaretlerini Güncelleştir   |Microsoft.SecurityInsights/yer imleri|
|Yer İşaretlerini Sil   |Microsoft.SecurityInsights/yer imleri|
|Servis Taleplerini Güncelleştir   |Microsoft.SecurityInsights/Cases|
|Servis Talebi Araştırmayı Güncelleştir  |Microsoft.SecurityInsights/Durumlar/soruşturmalar|
|Büyük/Küçük Harf Açıklamaları Oluşturma   |Microsoft.SecurityInsights/Cases/comments|
|Veri Bağlayıcılarını Güncelleştir |Microsoft.SecurityInsights/dataConnectors|
|Veri Bağlayıcılarını Sil |Microsoft.SecurityInsights/dataConnectors|
|Ayarları Güncelleştir    |Microsoft.SecurityInsights/settings|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Azure Sentinel'de denetim ve raporlama verilerini görüntüleme

Bu verileri Azure Etkinliği günlüğünden Azure Sentinel'e aktararak görüntüleyebilir ve bu veriler üzerinde araştırma ve analiz gerçekleştirebilirsiniz.

1. Azure [Etkinliği](connect-azure-activity.md) veri kaynağını bağlayın. Bunu yaptıktan sonra, denetim olayları Azure Activity adlı **Günlükler** ekranında yeni bir tabloya akışı.
2. Ardından, diğer tabloda olduğu gibi KQL kullanarak verileri sorgulayın.



## <a name="vendor-documentation"></a>Satıcı belgeleri

| **Satıcı**  | **Azure Sentinel'de olayı kullanma** | **Bağlantı**|
|----|----|----|
| GitHub| Topluluk sayfasına erişmek için kullanılır| <https://github.com/Azure/Azure-Sentinel> |
| PaloAlto| CEF'i yapılandır| <https://www.paloaltonetworks.com/documentation/misc/cef.html>|
| Çoğul Görüş | Kusto Sorgu Dili kursu| [https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)|

## <a name="blogs-and-forums"></a>Bloglar ve forumlar

Sorularınızı Azure Sentinel için [TechCommunity alanında](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) yayınlayın.

[TechCommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) ve [Microsoft Azure'daki](https://azure.microsoft.com/blog/tag/azure-sentinel/)Azure Sentinel blog gönderilerini görüntüleyin.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel ile çalışırken yararlı olan kaynakların bir listesi vardır. Microsoft Azure Güvenlik ve [Uyumluluk blogunda](https://blogs.msdn.com/b/azuresecurity/)Azure güvenliği ve uyumluluğu hakkında ek bilgiler bulabilirsiniz.
