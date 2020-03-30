---
title: Azure Monitör Günlükleri ile Azure Fonksiyonlarını İzleme
description: İşlev yürütmelerini izlemek için Azure İşlevleriyle Azure Monitör Günlükleri'ni nasıl kullanacağınızı öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: cshoe
ms.openlocfilehash: 13c72a1cf8a0dd4a1124e51b9ceee04ae04bf261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649883"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Azure Monitör Günlükleri ile Azure Fonksiyonlarını İzleme

Azure İşlevleri, işlevleri izlemek için [Azure Monitör Günlükleri](../azure-monitor/platform/data-platform-logs.md) ile bir tümleştirme sunar. Bu makalede, Azure Denetim Girişleri'ne sistem tarafından oluşturulan ve kullanıcı tarafından oluşturulan günlükleri göndermek için Azure Işlevlerini nasıl yapılandırabileceğiniz gösterilmektedir.

Azure Monitör Günlükleri, toplanan verileri hızla almak, birleştirmek ve analiz etmek için [sorgularla](../azure-monitor/log-query/log-query-overview.md) analiz edilebilen aynı çalışma alanında farklı kaynaklardan gelen günlükleri birleştirme olanağı sağlar.  Azure portalında [Log Analytics'i](../azure-monitor/log-query/portals.md) kullanarak sorgular oluşturabilir ve sınayabilir ve ardından bu araçları kullanarak verileri doğrudan analiz edebilir veya [görselleştirmeler](../azure-monitor/visualizations.md) veya [uyarı kurallarıyla](../azure-monitor/platform/alerts-overview.md)kullanılmak üzere sorguları kaydedebilirsiniz.

Azure Monitor, Azure Veri Gezgini tarafından kullanılan ve basit günlük sorguları için uygun olan ancak toplamalar, birleştirmeler ve akıllı analizler gibi gelişmiş işlevleri de içeren [Kusto sorgu dilinin](/azure/kusto/query/) bir sürümünü kullanır. [Birden fazla dersi](../azure-monitor/log-query/get-started-queries.md)kullanarak sorgu dilini hızlı bir şekilde öğrenebilirsiniz.

> [!NOTE]
> Azure Monitör Günlükleri ile tümleştirme şu anda Windows Tüketimi, Premium ve Özel barındırma planlarında çalışan işlev uygulamaları için genel önizlemededir.

## <a name="setting-up"></a>Ayarlama

**İzleme** bölümünden **Tanılama ayarlarını** seçin ve ardından **tanıayar ını ekle'yi**tıklatın.

![Tanılama ayarı ekleme](media/functions-monitor-log-analytics/diagnostic-settings-add.png)

**Tanılama ayarları** sayfasında, **Günlük Analitiği**Gönder'i seçin ve ardından Log Analytics çalışma alanınızı seçin. **Günlük** altında **FunctionAppLogs**seçin , Bu tablo istenilen günlükleri içerir.

![Tanılama ayarı ekleme](media/functions-monitor-log-analytics/choose-table.png)

## <a name="user-generated-logs"></a>Kullanıcı tarafından oluşturulan günlükler

Özel günlükler oluşturmak için, dilinize bağlı olarak belirli günlük deyimini kullanabilirsiniz, örnek kod parçacıkları şunlardır:


# <a name="c"></a>[C #](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Günlükleri sorgulama

Oluşturulan günlükleri sorgulamak için, işlev günlüklerini göndermek için yapılandırdığınız Log Analytics çalışma alanına gidin ve **Günlükler'i**tıklatın.

![LA çalışma alanında sorgu penceresi](media/functions-monitor-log-analytics/querying.png)

Azure Fonksiyonları Tüm günlükleri **FunctionAppLogs** tablosuna yazar, işte bazı örnek sorgular.

### <a name="all-logs"></a>Tüm günlükler

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="a-specific-function-logs"></a>Belirli bir işlev günlükleri

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Özel durumlar

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Sonraki adımlar

- Azure [İşlevlerini](functions-overview.md) gözden geçirme
- [Azure Monitör Günlükleri](../azure-monitor/platform/data-platform-logs.md) hakkında daha fazla bilgi edinin
- [Sorgu dili](../azure-monitor/log-query/get-started-queries.md)hakkında daha fazla bilgi edinin.
