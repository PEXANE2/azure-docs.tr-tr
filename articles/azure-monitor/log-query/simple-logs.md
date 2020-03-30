---
title: Azure Monitör'de Basit Günlükler deneyimi (Önizleme) | Microsoft Dokümanlar
description: Basit Günlükler deneyimi, KQL ile doğrudan etkileşimde kalmadan Azure Monitor'da temel sorgular oluşturmanıza olanak tanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/12/2019
ms.openlocfilehash: 59bcb42edaf7d46498a3514b4f1c919c6e8cc0c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77660266"
---
# <a name="simple-logs-experience-in-azure-monitor-preview"></a>Azure Monitör'de Basit Günlükler deneyimi (Önizleme)
Azure Monitor, KQL dilini kullanarak [günlük sorguları](log-query-overview.md) oluşturmak için zengin bir [deneyim](get-started-portal.md) sağlar. KQL'nin tam gücüne ihtiyaç duymayabilir ve temel sorgu gereksinimleri için basitleştirilmiş bir deneyimi tercih edebilirsiniz. Basit Günlükler deneyimi, KQL ile doğrudan etkileşime girmeden temel sorgular oluşturmanıza olanak tanır. Daha karmaşık sorgular gerektirdiğiniz için Basit Günlükleri KQL için bir öğrenme aracı olarak da kullanabilirsiniz.

> [!NOTE]
> Basit Günlükler şu anda sadece Cosmos DB ve Key Vaults için bir test olarak uygulanmaktadır. Bu özelliği genişletip yayınlayamayacağımızı belirlememize yardımcı olmak için lütfen [Deneyiminizi Kullanıcı Sesi](https://feedback.azure.com/forums/913690-azure-monitor) aracılığıyla Microsoft ile paylaşın.


## <a name="scope"></a>Kapsam
Basit Günlükler deneyimi, seçilen kaynak için *AzureDiagnostics*, *AzureMetrics*ve *AzureActivity* tablosundan veri alır. 

## <a name="using-simple-logs"></a>Basit Günlükleri Kullanma
[Log Analytics çalışma alanında günlükleri toplamak üzere yapılandırılan tanı ayarlarıyla](../platform/resource-logs-collect-storage.md)Azure aboneliğinizdeki herhangi bir Cosmos DB veya Key Vault'a gidin. Basit **Günlükler** deneyimini açmak için **İzleme** menüsünde Günlükler'i tıklatın.

![Menü](media/simple-logs/menu.png)

Bir **Alan** ve **Operatör** seçin ve karşılaştırma için bir **Değer** belirtin. Ek **+** ölçütler eklemek için **Ve/Veya'yi** tıklatın ve belirtin.

![Ölçütler](media/simple-logs/criteria.png)

Sorgu sonuçlarını görüntülemek için **Çalıştır'ı** tıklatın.

## <a name="view-and-edit-kql"></a>KQL'yi görüntüleme ve güncelleme
Tam Log Analytics deneyiminde Basit Günlükler sorgusu tarafından oluşturulan KQL'yi açmak için **Sorgu düzenleyicisini** seçin. 

![Sorgu düzenleyicisi](media/simple-logs/query-editor.png)

Sonuçlarınızı daha da hassaslaştırmak için KQL'yi doğrudan dinleyebilir ve Log Analytics'te filtreler gibi diğer özellikleri kullanabilirsiniz.

![KQL'yi edit](media/simple-logs/edit-kql.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında Log Analytics'i kullanma](get-started-portal.md)yla ilgili bir eğitimi tamamlayın.
- [Günlük sorguları yazma](get-started-portal.md)bir öğretici tamamlayın.
