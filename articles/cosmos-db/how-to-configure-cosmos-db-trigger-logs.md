---
title: Cosmos DB için Azure Fonksiyonları tetikleyicisi ile günlükleri yapılandırma ve okuma
description: Cosmos DB için Azure Fonksiyonları tetikleyicisini kullanırken günlükleri Azure İşleme günlüğe kaydetme ardışık ardışık
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 5ff747b225f8984bcaafd80015e85a9f014bdb50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441838"
---
# <a name="how-to-configure-and-read-the-logs-when-using-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure Fonksiyonları tetikleyicisini kullanırken günlükleri yapılandırma ve okuma

Bu makalede, Cosmos DB günlükleri için Azure İşlevleri tetikleyicisini yapılandırılmış izleme [çözümünüze](../azure-functions/functions-monitoring.md)gönderecek şekilde Azure İşlevler ortamınızı nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="included-logs"></a>Dahil günlükleri

Cosmos DB için Azure İşlevleri tetikleyicisi, [Özet Akışı İşlemci Kitaplığını](./change-feed-processor.md) dahili olarak kullanır ve kitaplık, [sorun giderme amacıyla](./troubleshoot-changefeed-functions.md)iç işlemleri izlemek için kullanılabilecek bir sistem durumu günlüğü kümesi oluşturur.

Sistem durumu günlükleri, yük dengeleme senaryoları veya başlatma sırasında işlemleri denerken Cosmos DB için Azure Fonksiyonları tetikleyicisinin nasıl çalıştığını açıklar.

## <a name="enabling-logging"></a>Günlüğe kaydetmeyi etkinleştirme

Cosmos DB için Azure Fonksiyonlarını kullanırken `host.json` günlüğe kaydetmeyi etkinleştirmek için, Dosyayı Azure İşlevler projenizde veya Azure İşlevler Uygulamanızda bulun ve [gerekli günlüğe kaydetme düzeyini yapılandırın.](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson) Aşağıdaki örnekte gösterildiği `Host.Triggers.CosmosDB` gibi izleri etkinleştirmeniz gerekir:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Azure İşlevi güncelleştirilmiş yapılandırmayla dağıtıldıktan sonra, izlerinizin bir parçası olarak Cosmos DB günlükleri için Azure İşleme tetikleyicisini görürsünüz. Yapılandırılan günlük sağlayıcınızdaki günlükleri *Kategori* `Host.Triggers.CosmosDB`altında görüntüleyebilirsiniz.

## <a name="query-the-logs"></a>Günlükleri sorgula

[Azure Uygulama Öngörüleri'nin Analytics'inde](../azure-monitor/app/analytics.md)Cosmos DB için Azure Fonksiyonları tetikleyicisi tarafından oluşturulan günlükleri sorgulamak için aşağıdaki sorguyu çalıştırın:

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure İşlevleri uygulamalarınızda [izlemeyi etkinleştirin.](../azure-functions/functions-monitoring.md)
* Cosmos DB için Azure Fonksiyonları tetikleyicisini kullanırken [sık karşılaşılan sorunları](./troubleshoot-changefeed-functions.md) nasıl tanılayarak sorun gidermeyi öğrenin.