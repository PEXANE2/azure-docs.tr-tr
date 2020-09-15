---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 09/git14/2020
ms.author: alkohli
ms.openlocfilehash: 91f91b1260cc445f90c2608fc5259ad61acd37ac
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90533316"
---
Data Box cihaz için desteklenen depolama hesaplarının ve depolama türlerinin bir listesi aşağıda verilmiştir. Tüm farklı depolama hesabı türlerinin ve tüm yeteneklerini tam bir liste için bkz. [depolama hesabı türleri](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

İçeri aktarma siparişlerinde, aşağıdaki tabloda desteklenen depolama hesapları gösterilmektedir.

| **Depolama hesabı/desteklenen depolama türleri** | **Blok blobu** |**Sayfa Blobu*** |**Azure dosyaları** |**Notlar**|
| --- | --- | -- | -- | -- |
| Klasik Standart | Y | Y | Y |
| Genel amaçlı v1 standardı  | Y | Y | Y | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v1 Premium  |  | Y| | |
| Genel amaçlı v2 standart  | Y | Y | Y | Sık ve seyrek erişimli desteklenir.|
| Genel amaçlı v2 Premium  |  |Y | | |
| BLOB depolama standart |Y | | |Sık ve seyrek erişimli desteklenir. |

\**-Sayfa bloblarına yüklenen veriler, VHD 'ler gibi 512 bayt hizalı olmalıdır.*

Dışarı aktarma siparişlerinde, aşağıdaki tabloda desteklenen depolama hesapları gösterilmektedir.

| **Depolama hesabı/desteklenen depolama türleri** | **Blok blobu** |**Sayfa Blobu*** |**Azure dosyaları** |**Desteklenen erişim katmanları**|
| --- | --- | -- | -- | -- |
| Klasik Standart | Y | Y | Y | |
| Genel amaçlı v1 standardı  | Y | Y | Y | Sık erişimli, seyrek erişimli|
| Genel amaçlı v1 Premium  |  | Y| | |
| Genel amaçlı v2 standart  | Y | Y | Y | Sık erişimli, seyrek erişimli|
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


