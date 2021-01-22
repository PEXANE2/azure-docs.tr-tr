---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: 86bf4911026e46c997469b956f9e7c75c4f17164
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98697939"
---
#### <a name="additional-premium-file-share-level-limits"></a>Ek Premium dosya paylaşma düzeyi limitleri

|Alan  |Hedef  |
|---------|---------|
|En küçük boyut artış/azaltma    |1 GiB      |
|Temel ıOPS    |GiB başına 400 + 1 ıOPS, en fazla 100.000|
|IOPS patlama    |Maksimum (3x ıOPS, GiB başına), 100.000 'e kadar|
|Çıkış oranı         |60 MIB/s + 0,06 * sağlanan GiB        |
|Giriş oranı| 40 MIB/s + 0,04 * sağlanan GiB |

#### <a name="file-level-limits"></a>Dosya düzeyi sınırları

|Alan  |Standart dosya  |Premium dosya  |
|---------|---------|---------|
|Boyut     |1 TiB         |4 TiB         |
|Dosya başına en fazla ıOPS      |1.000         |En fazla 8.000 *         |
|Eşzamanlı tutamaçlar     |2.000         |2.000         |
|Çıkış     |Bkz. standart dosya işleme değerleri         |300 MIB/sn (çok kanallı SMB önizlemesine sahip 1 GiB/sn 'ye kadar) * *         |
|Giriş     |Bkz. standart dosya işleme değerleri         |200 MIB/sn (çok kanallı SMB önizlemesine sahip 1 GiB/sn 'ye kadar) * *        |
|Aktarım hızı     |En fazla 60 MIB/sn         |Premium dosya giriş/çıkış değerlerini gör         |

\*<sup>IOS okuma ve yazma (genellikle daha küçük GÇ boyutları <= 64K) Için geçerlidir. Okuma ve yazma dışındaki meta veri işlemleri daha düşük olabilir. </sup>

\*\*<sup>Makine ağ sınırları, kullanılabilir bant genişliği, GÇ boyutları, sıra derinliği ve diğer faktörlere tabidir. Ayrıntılar için bkz. çok [kanallı SMB performansı](../articles/storage/files/storage-files-smb-multichannel-performance.md). </sup>
