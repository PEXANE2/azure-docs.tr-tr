---
title: Azure Batch Analizi
description: Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: d9538461dfa48a961f9325566b2adaf265e8daeb
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025971"
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