---
title: Azure Disk Depolama'da uygulamanızı kıyaslama
description: Azure'da uygulamanızı kıyaslama işlemi hakkında bilgi edinin.
author: roygara
ms.author: rogarana
ms.date: 01/11/2019
ms.topic: conceptual
ms.service: virtual-machines-linux
ms.subservice: disks
ms.openlocfilehash: 640ec54e9634751d05c2cea90d7c03d02e7a3387
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75720064"
---
# <a name="benchmarking-a-disk"></a>Bir diski karşılaştırma

Kıyaslama, uygulamanızdaki farklı iş yüklerini simüle etme ve her iş yükü için uygulama performansını ölçme işlemidir. [Yüksek performanslı makale için tasarım](premium-storage-performance.md)açıklanan adımları kullanarak. Uygulamayı barındıran VM'lerde kıyaslama araçları çalıştırarak, Premium Depolama ile uygulamanızın elde edebileceği performans düzeylerini belirleyebilirsiniz. Bu makalede, Azure Premium Depolama diskleriyle birlikte sağlanan standart DS14 VM karşılaştırma örnekleri sayılmılıyız.

Windows ve Linux için sırasıyla Iometer ve FIO ortak kıyaslama araçları kullandık. Bu araçlar, iş yükü gibi bir üretimi taklit eden birden çok iş parçacığı yumurtlar ve sistem performansını ölçer. Araçları kullanarak, normalde bir uygulama için değiştiremeyeceğiniz blok boyutu ve sıra derinliği gibi parametreleri de yapılandırabilirsiniz. Bu, farklı uygulama iş yükleri için premium disklerle birlikte sağlanan yüksek ölçekli bir VM'de maksimum performansı artırmak için daha fazla esneklik sağlar. Her kıyaslama aracı hakkında daha fazla bilgi edinmek için [Iometer](http://www.iometer.org/) ve [FIO](http://freecode.com/projects/fio)adresini ziyaret edin.

Aşağıdaki örnekleri izlemek için bir Standart DS14 VM oluşturun ve VM'ye 11 Premium Depolama diski takın. 11 diskten, ana bilgisayar önbelleğe alınmış 10 diski "Yok" olarak yapılandırın ve bunları NoCacheWrites adı verilen bir ses düzeyine şeritleyin. Kalan diskte ana bilgisayar önbelleğe alma "ReadOnly" olarak yapılandırın ve bu diskle CacheReads adlı bir birim oluşturun. Bu kurulumu kullanarak, standart ds14 VM'den maksimum Okuma ve Yazma performansını görebilirsiniz. Premium disklere sahip bir DS14 VM oluşturma hakkında ayrıntılı adımlar [için, yüksek performans için Tasarım'a](premium-storage-performance.md)gidin.

[!INCLUDE [virtual-machines-disks-benchmarking](../../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek performans için tasarım](premium-storage-performance.md)bizim makaleye devam edin.

Bu makalede, prototip için varolan uygulamanıza benzer bir denetim listesi oluşturursunuz. Kıyaslama araçlarını kullanarak, prototip uygulamasındaki iş yüklerini simüle edebilir ve performansı ölçebilirsiniz. Bunu yaparak, hangi disk teklifinin uygulama performans gereksinimlerinize eşleşebileceğini veya aşabileceğini belirleyebilirsiniz. Daha sonra üretim uygulamanız için aynı yönergeleri uygulayabilirsiniz.