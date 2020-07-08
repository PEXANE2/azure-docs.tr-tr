---
title: dosya dahil etme
description: dosya dahil etme
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: eaa76153fe96b5fd41166b20770e0a969aa9260d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83797114"
---
Azure Batch çalıştıran bir görev, çalıştırıldığında çıktı verileri üretebilir. Görev çıktısı verilerinin genellikle işteki diğer görevler tarafından alınması, işi yürüten istemci uygulaması veya her ikisinin de saklanması gerekir. Görevler, bir Batch işlem düğümünün dosya sistemine çıkış verileri yazar, ancak düğüm, havuzdan ayrıldığında veya düğüm havuzdan ayrıldığında, düğümdeki tüm veriler kaybedilir. Görevler, görev tarafından oluşturulan dosyaların silindiği bir dosya saklama süresine de sahip olabilir. Bu nedenlerden dolayı, daha sonra ihtiyaç duyacağınız görev çıkışının [Azure depolama](https://docs.microsoft.com/azure/storage/)gibi bir veri deposuna kalıcı olması önemlidir.

Batch 'teki depolama hesabı seçenekleri için bkz. [Batch hesapları ve Azure depolama hesapları](../articles/batch/accounts.md#azure-storage-accounts).
