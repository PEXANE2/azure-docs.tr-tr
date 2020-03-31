---
title: include dosyası
description: include dosyası
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188724"
---
Azure Toplu İş'te çalışan bir görev çalıştığında çıktı verileri üretebilir. Görev çıktısı verilerinin genellikle işteki diğer görevler, işi çalıştıran istemci uygulaması veya her ikisi tarafından geri alınması için depolanması gerekir. Görevler çıktı verilerini Toplu İşlem düğümünün dosya sistemine yazar, ancak düğümdeki tüm veriler yeniden görüntülendiğinde veya düğüm havuzdan ayrıldığında kaybolur. Görevler, görev tarafından oluşturulan dosyaların silindikten sonra bir dosya bekletme süresine de sahip olabilir. Bu nedenlerden dolayı, daha sonra [Azure Depolama](https://docs.microsoft.com/azure/storage/)gibi bir veri deposunda ihtiyaç duyacağınız görev çıktısını kalıcı hale getirmek önemlidir.

Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](../articles/batch/batch-api-basics.md#azure-storage-account).
