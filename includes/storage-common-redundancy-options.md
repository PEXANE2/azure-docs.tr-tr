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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77157236"
---
Depolama hesabı için artıklık seçenekleri şunlardır:

* Yerel olarak yedekli depolama (LRS): basit, düşük maliyetli bir artıklık stratejisi. Veriler, birincil bölge içinde zaman uyumlu olarak üç kez kopyalanır.
* Bölgesel olarak yedekli depolama (ZRS): yüksek kullanılabilirlik gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır.
* Coğrafi olarak yedekli depolama (GRS): bölgesel kesintilere karşı korumak için çapraz bölgesel yedeklilik. Veriler birincil bölgede zaman uyumlu olarak üç kez kopyalanır ve ardından zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) etkinleştirin.
* Coğrafi bölge yedekli depolama (GZRS) (Önizleme): hem yüksek kullanılabilirlik hem de maksimum dayanıklılık gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır ve sonra zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için, Okuma Erişimli Coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.

Azure depolama 'daki artıklık seçenekleri hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../articles/storage/common/storage-redundancy.md).
