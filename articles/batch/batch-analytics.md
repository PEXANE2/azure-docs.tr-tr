---
title: Azure Batch Analizi
description: Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.
services: batch
author: ju-shim
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: jushiman
ms.openlocfilehash: 5f5063ff2b285a0613d8c7bf367007997afcb778
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76935252"
---
# <a name="batch-analytics"></a>Batch Analizi
Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.

Batch tanılama günlüklerini etkinleştirme ve kullanma hakkında daha fazla bilgi için bkz. [Azure Batch tanılama günlüğü](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) .

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Azure Batch hizmeti, belirli toplu kaynakların kullanım ömrü boyunca aşağıdaki tanılama günlüğü olaylarını yayar.

**Hizmet günlüğü olayları**
* [Havuz oluşturma](batch-pool-create-event.md)
* [Havuz silme başlangıç](batch-pool-delete-start-event.md)
* [Tam havuzunu Sil](batch-pool-delete-complete-event.md)
* [Havuz yeniden boyutlandırma başlangıç](batch-pool-resize-start-event.md)
* [Tam havuzunu boyutlandırma](batch-pool-resize-complete-event.md)
* [Görev Başlat](batch-task-start-event.md)
* [Görev tamamlandı](batch-task-complete-event.md)
* [Görev başarısız](batch-task-fail-event.md)