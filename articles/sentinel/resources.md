---
title: Azure Sentinel ile çalışırken faydalı kaynaklar | Microsoft Docs
description: Bu belge, Azure Sentinel ile çalışırken yararlı kaynakların bir listesini sağlar.
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
ms.date: 01/27/2021
ms.author: yelevin
ms.openlocfilehash: c404aa93669cd95dccb0ad185d71d2ec16256d0d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100570435"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Azure Sentinel ile çalışmaya yönelik faydalı kaynaklar



Bu makalede, Azure Sentinel ile çalışma hakkında daha fazla bilgi almanıza yardımcı olabilecek kaynaklar listelenmektedir.

- **Azure Logic Apps bağlayıcılar**: <https://docs.microsoft.com/connectors/>


## <a name="auditing-and-reporting"></a>Denetim ve raporlama
Azure Sentinel 'in denetim günlükleri [Azure etkinlik günlüklerinde](../azure-monitor/essentials/platform-logs-overview.md)tutulur.

Aşağıdaki desteklenen işlemler denetlenebilir.

|İşlem adı|    Kaynak türü|
|----|----|
|Çalışma kitabı oluştur veya güncelleştir  |Microsoft. Insights/çalışma kitapları|
|Çalışma kitabını Sil    |Microsoft. Insights/çalışma kitapları|
|Iş akışını ayarla   |Microsoft. Logic/iş akışları|
|Iş akışını Sil    |Microsoft. Logic/iş akışları|
|Kayıtlı arama oluştur    |Microsoft. Operationalınsights/çalışma alanları/Savedaramalar|
|Kayıtlı aramayı Sil    |Microsoft. Operationalınsights/çalışma alanları/Savedaramalar|
|Uyarı kurallarını Güncelleştir |Microsoft. Securityınsights/alertRules|
|Uyarı kurallarını Sil |Microsoft. Securityınsights/alertRules|
|Uyarı kuralı yanıt eylemlerini Güncelleştir |Microsoft. Securityınsights/alertRules/Actions|
|Uyarı kuralı yanıt eylemlerini Sil |Microsoft. Securityınsights/alertRules/Actions|
|Yer Imlerini güncelleştirme   |Microsoft. Securityınsights/yer işaretleri|
|Yer Imlerini Sil   |Microsoft. Securityınsights/yer işaretleri|
|Güncelleştirme çalışmaları   |Microsoft. Securityınsights/durumlar|
|Durum araştırmasını Güncelleştir  |Microsoft. Securityınsights/durumlar/araştırmalar|
|Case açıklamaları oluşturma   |Microsoft. Securityınsights/Cases/Comments|
|Veri bağlayıcılarını güncelleştirme |Microsoft. Securityınsights/veri bağlayıcıları|
|Veri bağlayıcıları silme |Microsoft. Securityınsights/veri bağlayıcıları|
|Ayarları Güncelleştir    |Microsoft. Securityınsights/ayarlar|

### <a name="view-audit-and-reporting-data-in-azure-sentinel"></a>Azure Sentinel 'de denetim ve raporlama verilerini görüntüleme

Azure etkinlik günlüğünden bu verileri, daha sonra araştırma ve analiz gerçekleştirebileceğiniz bir Azure Sentinel 'e aktararak görüntüleyebilirsiniz.

1. [Azure etkinlik](connect-azure-activity.md) veri kaynağını bağlayın. Bunu yaptıktan sonra, denetim olayları AzureActivity adlı **Günlükler** ekranında yeni bir tabloya akışla kaydedilir.

1. Ardından, diğer tüm tablolar gibi KQL kullanarak verileri sorgulayın.

    Örneğin, belirli bir analiz kuralını kimin düzenleyen Son Kullanıcı olduğunu öğrenmek için aşağıdaki sorguyu kullanın ( `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` denetlemek istediğiniz kuralın kural kimliğiyle değiştirin):

    ```kusto
    AzureActivity
    | where OperationNameValue startswith "MICROSOFT.SECURITYINSIGHTS/ALERTRULES/WRITE"
    | where Properties contains "alertRules/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    | project Caller , TimeGenerated , Properties
    ```


## <a name="blogs-and-forums"></a>Bloglar ve Forumlar

Kullanıcılarımızdan aldığımız bir duyuyoruz!

- **Sorularınızı** Azure Sentinel Için [Techcommunity alanına](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel) gönderin. 

- [Kullanıcı sesli](https://feedback.azure.com/forums/920458-azure-sentinel) programımız aracılığıyla **geliştirmeler için öneriler gönderin** .

- Azure Sentinel blog gönderilerimizi **görüntüleyin ve açıklama ekleyin** :

    - [TechCommunity](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog) 
    - [Microsoft Azure](https://azure.microsoft.com/blog/tag/azure-sentinel/)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sertifikalı alın!](/learn/paths/security-ops-sentinel/)

> [!div class="nextstepaction"]
> [Müşteri kullanım örneği hikayelerini okuyun](https://customers.microsoft.com/en-us/search?sq=%22Azure%20Sentinel%20%22&ff=&p=0&so=story_publish_date%20desc)

