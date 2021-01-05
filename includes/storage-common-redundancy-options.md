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
ms.openlocfilehash: 65729934ea7c4037d6857aec10b14cdddd616368
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97805602"
---
Depolama hesabı için artıklık seçenekleri şunlardır:

* Yerel olarak yedekli depolama (LRS): basit, düşük maliyetli bir artıklık stratejisi. Veriler, birincil bölgedeki tek bir fiziksel konum içinde zaman uyumlu olarak üç kez kopyalanır.
* Bölgesel olarak yedekli depolama (ZRS): yüksek kullanılabilirlik gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır.
* Coğrafi olarak yedekli depolama (GRS): bölgesel kesintilere karşı korumak için çapraz bölgesel yedeklilik. Veriler birincil bölgede zaman uyumlu olarak üç kez kopyalanır ve ardından zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) etkinleştirin.
* Coğrafi bölge yedekli depolama (GZRS) (Önizleme): hem yüksek kullanılabilirlik hem de maksimum dayanıklılık gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır ve sonra zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için, Okuma Erişimli Coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.

Azure depolama 'daki artıklık seçenekleri hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../articles/storage/common/storage-redundancy.md).
