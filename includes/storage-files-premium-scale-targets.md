---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: d704c6026e9d007a7365a3b72649ca509585da4d
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057817"
---
#### <a name="additional-premium-file-share-level-limits"></a>Ek Premium dosya paylaşma düzeyi limitleri

|Alan  |Hedef  |
|---------|---------|
|En küçük boyut artış/azaltma    |1 GiB      |
|Temel ıOPS    |GiB başına 1 ıOPS, 100.000 'e kadar|
|IOPS patlama    |GiB başına 3x ıOPS, 100.000 'e kadar|
|Çıkış oranı         |60 MIB/s + 0,06 * sağlanan GiB        |
|Giriş oranı| 40 MIB/s + 0,04 * sağlanan GiB |

#### <a name="file-level-limits"></a>Dosya düzeyi sınırları

|Alan  |Standart dosya  |Premium dosya  |
|---------|---------|---------|
|Boyut     |1 TiB         |4 TiB         |
|Dosya başına en fazla ıOPS      |1.000         |5.000         |
|Eşzamanlı tutamaçlar     |2.000         |2.000         |
|Çıkış     |Bkz. standart dosya işleme değerleri         |300 MIB/sn         |
|Giriş     |Bkz. standart dosya işleme değerleri         |200 MIB/sn         |
|Aktarım hızı     |En fazla 60 MIB/sn         |Premium dosya giriş/çıkış değerlerini gör         |