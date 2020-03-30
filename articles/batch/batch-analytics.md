---
title: Azure Toplu İş Analitiği
description: Toplu İşanlı'daki konular, Toplu Hizmet kaynakları için mevcut olaylar ve uyarılar için referans bilgilerini içerir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025971"
---
# <a name="batch-analytics"></a>Batch Analizi
Toplu İşanlı'daki konular, Toplu Hizmet kaynakları için mevcut olaylar ve uyarılar için referans bilgilerini içerir.

Toplu iş tanı günlüklerini etkinleştirme ve tüketme hakkında daha fazla bilgi için [Azure Toplu tanı](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) günlüğe bakın.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Azure Toplu İşlem hizmeti, belirli Toplu İşlem kaynaklarının ömrü boyunca aşağıdaki tanılama günlüğü olaylarını yayır.

**Hizmet Günlüğü etkinlikleri**
* [Havuz oluşturma](batch-pool-create-event.md)
* [Havuz silme başlat](batch-pool-delete-start-event.md)
* [Havuz silme tamamlandı](batch-pool-delete-complete-event.md)
* [Havuz yeniden boyutlandırma başlat](batch-pool-resize-start-event.md)
* [Havuz yeniden boyutlandırma tamamlandı](batch-pool-resize-complete-event.md)
* [Görev başlangıcı](batch-task-start-event.md)
* [Görev tamamlandı](batch-task-complete-event.md)
* [Görev başarısız](batch-task-fail-event.md)