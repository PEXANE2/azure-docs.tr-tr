---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 5ab03b682dd0ed1dc7b198e89c86e7a74c6275cd
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "67457581"
---
| Resource | Hedef |
|----------|---------------|
| Tek bir tablonun en büyük boyutu | 500 TiB |
| En büyük tablo varlığı boyutu | 1 MIB |
| Bir tablo varlığındaki en fazla özellik sayısı | üç sistem özelliği içeren 255: PartitionKey, RowKey ve timestamp |
| Bir varlıktaki özellik değerlerinin en büyük toplam boyutu | 1 MIB |
| Bir varlıktaki tek bir özelliğin en büyük toplam boyutu | Özellik türüne göre farklılık gösterir. Daha fazla bilgi için bkz. [Tablo hizmeti veri modelini anlama](/rest/api/storageservices/understanding-the-table-service-data-model)Içindeki **özellik türleri** . |
| Tablo başına en fazla depolanan erişim ilkesi sayısı | 5 |
| Depolama hesabı başına en fazla istek oranı | saniyede 20.000 işlem, 1-KiB varlık boyutunu varsayan |
| Tek bir tablo bölümü için hedef aktarım hızı (1 KiB-varlıklar) | Saniyede 2.000 adede kadar varlık |