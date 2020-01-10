---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ac3e87d7f921da2c1089eb6f2c7e61fc2c432f9f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75463921"
---
Bölgesel olarak yedekli depolama (ZRS), verilerinizi tek bir bölgedeki üç depolama kümesi arasında eşzamanlı olarak çoğaltır. Her depolama kümesi diğerlerinden fiziksel olarak ayrılır ve kendi kullanılabilirlik bölgesinde (AZ) bulunur. Her bir kullanılabilirlik alanı&mdash;ve&mdash;içindeki ZRS kümesi özerk olup ayrı yardımcı programlar ve ağ özellikleri içerir. ZRS depolama hesabına yönelik yazma isteği, veriler yalnızca üç küme genelinde tüm yinelemelere yazıldıktan sonra başarıyla döndürülür.

Verilerinizi ZRS çoğaltmasını kullanarak bir depolama hesabında depoladığınızda, bir kullanılabilirlik bölgesi kullanılamaz hale gelirse verilerinize erişmeye ve bunları yönetmeye devam edebilirsiniz. ZRS, mükemmel performans ve düşük gecikme süresi sağlar. ZRS [yerel olarak yedekli depolama (LRS)](../articles/storage/common/storage-redundancy-lrs.md)ile aynı ölçeklenebilirlik hedeflerini sunmaktadır. Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../articles/storage/common/scalability-targets-standard-account.md).

Tutarlılık, dayanıklılık ve yüksek kullanılabilirlik gerektiren senaryolar için ZRS 'yi değerlendirin. Bir kesinti veya doğal olağanüstü durum bir kullanılabilirlik bölgesini kullanılamaz hale gelirse, ZRS belirli bir yıl boyunca en az% 99,9999999999 (12 9 ' a) depolama nesneleri için dayanıklılık sağlar.

Coğrafi bölge yedekli depolama (GZRS) (Önizleme), birincil bölgedeki üç Azure kullanılabilirlik bölgesinde verilerinizi eşzamanlı olarak çoğaltır ve sonra verileri zaman uyumsuz olarak ikincil bölgeye çoğaltır. GZRS, en yüksek dayanıklılık ile birlikte yüksek kullanılabilirlik sağlar. GZRS belirli bir yıl boyunca nesnelerin en az% 99.99999999999999 (16 9) oranında dayanıklılığını sağlamak üzere tasarlanmıştır. İkincil bölgedeki verilere yönelik okuma erişimi için, Okuma Erişimli Coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin. GZRS hakkında daha fazla bilgi için bkz. [coğrafi bölge yedekli depolama, yüksek oranda kullanılabilirlik ve en yüksek dayanıklılık (Önizleme) için](../articles/storage/common/storage-redundancy-gzrs.md).

Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [kullanılabilirlik alanları genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).
