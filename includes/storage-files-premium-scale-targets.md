---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/10/2020
ms.author: rogarana
ms.openlocfilehash: 8dcb58499113b0b7ae0814419f0a76965a0ed945
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94680835"
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
|Dosya başına en fazla ıOPS      |1.000         |En fazla 8.000 *         |
|Eşzamanlı tutamaçlar     |2.000         |2.000         |
|Çıkış     |Bkz. standart dosya işleme değerleri         |300 MIB/sn (çok kanallı SMB önizlemesine sahip 1 GiB/sn 'ye kadar) * *         |
|Giriş     |Bkz. standart dosya işleme değerleri         |200 MIB/sn (çok kanallı SMB önizlemesine sahip 1 GiB/sn 'ye kadar) * *        |
|Aktarım hızı     |En fazla 60 MIB/sn         |Premium dosya giriş/çıkış değerlerini gör         |

\*<sup>IOS okuma ve yazma (genellikle daha küçük GÇ boyutları <= 64K) Için geçerlidir. Okuma ve yazma dışındaki meta veri işlemleri daha düşük olabilir. </sup>

\*\*<sup>Makine ağ sınırları, kullanılabilir bant genişliği, GÇ boyutları, sıra derinliği ve diğer faktörlere tabidir. Ayrıntılar için bkz. çok [kanallı SMB performansı](../articles/storage/files/storage-files-smb-multichannel-performance.md). </sup>
