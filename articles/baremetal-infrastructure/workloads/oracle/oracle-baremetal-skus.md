---
title: Oracle iş yükleri için BareMetal SKU 'Ları
description: Oracle BareMetal altyapı iş yükleri için SKU 'Lar hakkında bilgi edinin.
ms.topic: reference
ms.subservice: workloads
ms.date: 04/14/2021
ms.openlocfilehash: 2482f8ed682a982ee3c8c4907e21b4e6c6ebbb4f
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559215"
---
# <a name="baremetal-skus-for-oracle-workloads"></a>Oracle iş yükleri için BareMetal SKU 'Ları

Bu makalede, Oracle iş yükleri için özelleştirilmiş BareMetal altyapı SKU 'Larına göz atalım.

Oracle SKU 'Larının BareMetal altyapısı, iki yuvadan en fazla dört yuva arasındadır. Ayrıca, iş yükünüzün gereksinimlerini karşılamak için çeşitli CPU çekirdekleri ve bellek boyutları arasından seçim yapabilirsiniz. İşte kullanılabilir SKU 'ların özelliklerini özetleyen bir tablo.
 
| **Oracle sertifikalı**  **donanım** | **Modelleme** | **Toplam bellek** | **Depolama** | **Kullanılabilirlik** |
| --- | --- | --- | --- | --- |
| EVET | Azure S32m üzerinde SAP HANA-2 x Intel® Xeon® Işlemci I623416 CPU çekirdekleri ve 32 CPU iş parçacıkları | 1,5 TB | --- | Kullanılabilir |
| EVET | SAP HANA Azure S64m-4 x Intel® Xeon® Işlemci I623432 CPU çekirdekleri ve 64 CPU iş parçacıkları | 3,0 TB | --- | Kullanılabilir |
| EVET | Azure S96 üzerinde SAP HANA – 2 x Intel® Xeon® Işlemci E7-8890 v448 CPU çekirdekleri ve 96 CPU iş parçacıkları | 768 GB | 3,0 TB | Kullanılabilir |
| EVET | Azure S224 üzerinde SAP HANA – 4 x Intel® Xeon® Platinum 8276 işlemci 112 CPU çekirdekleri ve 224 CPU iş parçacıkları | 3,0 TB | 6,3 TB | Kullanılabilir |
| EVET | Azure S224m üzerinde SAP HANA – 4 x Intel® Xeon® Platinum 8276 işlemci 112 CPU çekirdekleri ve 224 CPU iş parçacıkları | 6,0 TB | 10,5 TB | Kullanılabilir |

- CPU çekirdekleri = sunucu biriminin hiper iş parçacıklı olmayan CPU çekirdekleri (Toplam fiziksel işlemci sayısı) toplamı. 
- CPU iş parçacıkları = sunucu biriminin hiper iş parçacıklı CPU çekirdekleri (Toplam mantıksal işlemci sayısı) tarafından sunulan işlem iş parçacıklarının toplamı. Çoğu birim, Hyper-Threading teknolojisini kullanmak için varsayılan olarak yapılandırılır.
- Sunucular müşterilere ayrılmıştır.
- Müşterinin kök erişimi (hiper yönetici yok) vardır.
- Sunucular doğrudan Azure VNET 'lerde değildir.

## <a name="next-steps"></a>Sonraki adımlar

Oracle için BareMetal altyapısı tarafından sunulan depolama hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Oracle iş yükleri için BareMetal 'da depolama](oracle-baremetal-storage.md)
