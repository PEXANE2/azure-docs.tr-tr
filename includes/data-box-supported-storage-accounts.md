---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 12/23/2020
ms.author: alkohli
ms.openlocfilehash: 11958c54dd1f54e424b71eb00780f5309a1c0bab
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209563"
---
Data Box cihaz için desteklenen depolama hesaplarının ve depolama türlerinin listesi aşağıda verilmiştir. Tüm depolama hesabı türlerinde tüm yeteneklerin tümünün listesi için bkz. [depolama hesabı türleri](../articles/storage/common/storage-account-overview.md#types-of-storage-accounts).

İçeri aktarma siparişlerinde, aşağıdaki tabloda desteklenen depolama hesapları gösterilmektedir.

| **Depolama hesabı/desteklenen depolama türleri** | **Blok blobu** |**Sayfa Blobu** _ |_ *Azure dosyaları** |**Notlar**|
| --- | --- | -- | -- | -- |
| Klasik Standart | E | E | E |
| Genel amaçlı v1 standardı  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v1 Premium  |  | Y| | |
| Genel amaçlı v2 standart  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v2 Premium  |  |Y | | |
| Azure Premium dosya depolaması |  |  | Y |  |  
| BLOB depolama standart |Y | | |Sık ve seyrek erişimli desteklenir. |

\**-Sayfa bloblarına yüklenen veriler, VHD 'ler gibi 512 bayt hizalı olmalıdır.*

Dışarı aktarma siparişlerinde, aşağıdaki tabloda desteklenen depolama hesapları gösterilmektedir.

| **Depolama hesabı/desteklenen depolama türleri** | **Blok blobu** |**Sayfa Blobu** _ |_ *Azure dosyaları** |**Desteklenen erişim katmanları**|
| --- | --- | -- | -- | -- |
| Klasik Standart | E | E | E | |
| Genel amaçlı v1 standardı  | E | E | E | Sık erişimli, seyrek erişimli|
| Genel amaçlı v1 Premium  |  | Y| | |
| Genel amaçlı v2 standart  | E | E | E | Sık erişimli, seyrek erişimli|
| Genel amaçlı v2 Premium  |  |Y | | |
| Azure Premium dosya depolaması |  |  | Y |  |
| BLOB depolama standart |Y | | |Sık erişimli, seyrek erişimli |
| Blok Blobu depolama Premium |Y | | |Sık erişimli, seyrek erişimli |
| Sayfa Blobu depolama Premium | |Y | | |

> [!IMPORTANT]
> - Genel amaçlı hesaplar için Data Box içeri aktarma siparişleri için sırayı, tabloyu ve disk depolama türlerini desteklemez. Data Box, genel amaçlı hesaplar için kuyruk, tablo, disk ve Azure Data Lake Gen 2 depolama türlerini desteklemez.
> - Data Box BLOB depolama ve blok BLOB depolama hesapları için ekleme bloblarını desteklemez.
> - Sayfa bloblarına yüklenen veriler, VHD 'ler gibi 512 bayt hizalı olmalıdır.
> - En fazla 80 TB aktarılabilir.
> - Dosya Geçmişi ve BLOB anlık görüntüleri dışarıya aktarılmaz.