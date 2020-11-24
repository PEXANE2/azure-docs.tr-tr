---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 09/git14/2020
ms.author: alkohli
ms.openlocfilehash: 25c2ea04cd062554a975c63aae9b97846e646d68
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95556191"
---
Data Box cihaz için desteklenen depolama hesaplarının ve depolama türlerinin bir listesi aşağıda verilmiştir. Tüm farklı depolama hesabı türlerinin ve tüm yeteneklerini tam bir liste için bkz. [depolama hesabı türleri](../articles/storage/common/storage-account-overview.md#types-of-storage-accounts).

İçeri aktarma siparişlerinde, aşağıdaki tabloda desteklenen depolama hesapları gösterilmektedir.

| **Depolama hesabı/desteklenen depolama türleri** | **Blok blobu** |**Sayfa Blobu** _ |_ *Azure dosyaları** |**Notlar**|
| --- | --- | -- | -- | -- |
| Klasik Standart | E | E | E |
| Genel amaçlı v1 standardı  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v1 Premium  |  | Y| | |
| Genel amaçlı v2 standart  | E | E | E | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v2 Premium  |  |Y | | |
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
| BLOB depolama standart |Y | | |Sık erişimli, seyrek erişimli |
| Blok Blobu depolama Premium |Y | | |Sık erişimli, seyrek erişimli |
| Sayfa Blobu depolama Premium | |Y | | |

> [!IMPORTANT]
> - Genel amaçlı hesaplar için Data Box içeri aktarma siparişleri için sırayı, tabloyu ve disk depolama türlerini desteklemez. Data Box, genel amaçlı hesaplar için kuyruk, tablo, disk ve Azure Data Lake Gen 2 depolama türlerini desteklemez.
> - Data Box BLOB depolama ve blok BLOB depolama hesapları için ekleme bloblarını desteklemez.
> - Data Box Premium dosya depolama hesaplarını desteklemez.
> - Sayfa bloblarına yüklenen veriler, VHD 'ler gibi 512 bayt hizalı olmalıdır.
> - En fazla 80 TB aktarılabilir.
> - Dosya Geçmişi ve BLOB anlık görüntüleri dışarıya aktarılmaz.