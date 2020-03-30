---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/14/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 73c2b742ede21a4e86d717d994f8ebc4f16389c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77157236"
---
Depolama hesabı için artıklık seçenekleri şunlardır:

* Yerel olarak yedekli depolama (LRS): Basit, düşük maliyetli artıklık stratejisi. Veriler birincil bölge içinde eşzamanlı olarak üç kez kopyalanır.
* Bölge yedekli depolama (ZRS): Yüksek kullanılabilirlik gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak kopyalanır.
* Coğrafi yedekli depolama (GRS): Bölgesel kesintilere karşı korunmak için bölgeler arası fazlalık. Veriler birincil bölgede üç kez eşzamanlı olarak kopyalanır ve ikincil bölgeye eşzamanlı olarak kopyalanır. İkincibölgedeki verilere okuma erişimi için, okuma erişimi coğrafi yedekli depolamayı (RA-GRS) etkinleştirin.
* Coğrafi bölge yedekli depolama (GZRS) (önizleme): Hem yüksek kullanılabilirlik hem de maksimum dayanıklılık gerektiren senaryolar için artıklık. Veriler birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak kopyalanır ve ardından ikincil bölgeye eş senkronize olarak kopyalanır. İkincibölgedeki verilere okuma erişimi için, okuma-erişim coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.

Azure Depolama'da artıklık seçenekleri hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](../articles/storage/common/storage-redundancy.md)bakın.
