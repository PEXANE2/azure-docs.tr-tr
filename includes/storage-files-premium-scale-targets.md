---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: c2bd10ab4c98fe2e77332c3cc2566ab2f0c7ad42
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841972"
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

|Alan  |Premium dosya  |Standart dosya |
|---------|---------|---------|
|Boyut                  |4 TiB         |1 TiB|
|Dosya başına en fazla ıOPS     |5.000         |1.000|
|Eşzamanlı tutamaçlar    |2.000         |2.000|
|Çıkış  |300 MIB/sn|      Bkz. standart dosya işleme değerleri|
|Giriş  |200 MIB/sn| Bkz. standart dosya işleme değerleri|
|Aktarım hızı| Premium dosya giriş/çıkış değerlerini gör| En fazla 60 MIB/sn|
