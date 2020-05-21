---
title: Azure Batch Analizi
description: Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: b07d362c3cc2d3b086cf0a82325025604907a393
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726868"
---
# <a name="batch-analytics"></a>Batch Analizi
Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.

Batch tanılama günlüklerini etkinleştirme ve kullanma hakkında daha fazla bilgi için bkz. [Azure Batch tanılama günlüğü](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) .

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Azure Batch hizmeti, belirli toplu kaynakların kullanım ömrü boyunca aşağıdaki tanılama günlüğü olaylarını yayar.

**Hizmet günlüğü olayları**
* [Havuz oluşturma](batch-pool-create-event.md)
* [Havuz silme başlangıcı](batch-pool-delete-start-event.md)
* [Havuz silme Tamam](batch-pool-delete-complete-event.md)
* [Havuz yeniden boyutlandırma başlangıcı](batch-pool-resize-start-event.md)
* [Havuz yeniden boyutlandırma Tamam](batch-pool-resize-complete-event.md)
* [Görev başlangıcı](batch-task-start-event.md)
* [Görev tamamlanma](batch-task-complete-event.md)
* [Görev başarısız](batch-task-fail-event.md)