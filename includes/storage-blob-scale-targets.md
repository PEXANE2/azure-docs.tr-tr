---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 2ed88d8abb7cbe96093b68d89030e6e464a35541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75392390"
---
| Kaynak | Hedef        |
|----------|---------------|
| Tek leke konteynermaksimum boyutu | Maksimum depolama hesabı kapasitesiyle aynı |
| Bir blok blob veya ek blob blok maksimum sayısı | 50.000 blok |
| Blok blob'undaki bloğun maksimum boyutu | 100 MiB |
| Bir blok blob maksimum boyutu | 50.000 X 100 MiB (yaklaşık 4.75 TiB) |
| Bir ek blob bir blok maksimum boyutu | 4 MiB |
| Bir ek blob maksimum boyutu | 50.000 x 4 MiB (yaklaşık 195 GiB) |
| Bir sayfa lekesinin maksimum boyutu | 8 TiB |
| Blob kapsayıcıbaşına maksimum depolanan erişim ilkeleri sayısı | 5 |
|Tek bir blob için hedef istek oranı | Saniyede en fazla 500 istek |
|Tek bir sayfa blob için hedef iş çıktısı | Saniyede 60 MiB'e kadar |
|Tek bir blok blob için hedef iş girişi |Depolama hesabı girişine kadar giriş/çıkış limitleri<sup>1</sup> |

<sup>1</sup> Tek bir blob için iş ortası, eşzamanlılık, istek boyutu, performans katmanı, yüklemeler için kaynak hızı ve indirmeler için hedef dahil ancak bunlarla sınırlı olmamak üzere çeşitli etkenlere bağlıdır. [Yüksek iş bölümü blok lekelerinin](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/)performans geliştirmelerinden yararlanmak için daha büyük lekeler veya bloklar yükleyin. Özellikle, standart depolama hesapları için 4 [MiB'den](/rest/api/storageservices/put-blob) büyük bir blob veya blok boyutuyla Put Blob veya [Put Block](/rest/api/storageservices/put-block) işlemini arayın. Premium blok blob veya Data Lake Storage Gen2 depolama hesapları için, 256 KiB'den büyük bir blok veya blob boyutu kullanın.
