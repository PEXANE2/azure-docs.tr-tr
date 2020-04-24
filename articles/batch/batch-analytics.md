---
title: Azure Batch Analizi
description: Batch Analytics 'teki konular, Batch hizmeti kaynakları için kullanılabilen olaylar ve uyarılar için başvuru bilgileri içerir.
ms.topic: article
ms.date: 04/20/2017
ms.openlocfilehash: d35bf249a3ae5dd06f42b89c3d5c2773f9508603
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82113077"
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