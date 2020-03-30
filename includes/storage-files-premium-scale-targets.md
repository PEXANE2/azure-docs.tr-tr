---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: b28427b3ede0cfaeb9e08d3c73b15ea7f2961f1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71180079"
---
#### <a name="additional-premium-file-share-level-limits"></a>Ek premium dosya paylaşım düzeyi sınırları

|Alan  |Hedef  |
|---------|---------|
|Minimum boyut artış/azalma    |1 GİB      |
|Temel IOPS    |GiB başına 1 IOPS, 100.000'e kadar|
|IOPS patlama    |GiB başına 3x IOPS, 100.000'e kadar|
|Çıkış oranı         |60 MiB/s + 0,06 * karşılıklı GİB        |
|Giriş hızı| 40 MiB/s + 0,04 * karşılıklı GİB |

#### <a name="file-level-limits"></a>Dosya düzeyi sınırları

|Alan  |Premium dosya  |Standart dosya |
|---------|---------|---------|
|Boyut                  |1 TiB         |1 TiB|
|Dosya başına Maksimum IOPS     |5.000         |1000|
|Eşzamanlı tutamaçları    |2.000         |2.000|
|Çıkış  |300 MiB/sn|      Standart dosya iş girdisi değerlerine bakın|
|Giriş  |200 MiB/sn| Standart dosya iş girdisi değerlerine bakın|
|Aktarım hızı| Bkz. premium dosya girişi/çıkış değerleri| 60 MiB/sn'ye kadar|
