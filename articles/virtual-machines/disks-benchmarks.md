---
title: Azure Disk Depolama uygulamanızı kıyaslama
description: Uygulamanızı Azure 'da değerlendirme süreci hakkında bilgi edinin.
author: roygara
ms.author: rogarana
ms.date: 01/29/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: bfda14acc2e50005e25faafa3037805af871c1df
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99094651"
---
# <a name="benchmark-a-disk"></a>Disk kıyaslama

Sınama, uygulamanızdaki farklı iş yüklerini taklit etme ve her iş yükü için uygulama performansını ölçme işlemidir. [Yüksek performans için tasarlama makalesinde](premium-storage-performance.md)açıklanan adımları kullanarak, uygulama performansı gereksinimlerini toplamıştır. Uygulamayı barındıran VM 'lerde benchişaretleme araçları 'nı çalıştırarak, uygulamanızın Premium SSD 'Ler ile elde edilebileceği performans düzeylerini belirleyebilirsiniz. Bu makalede, Azure Premium SSD 'Ler ile sağlanan bir Standard_D8ds_v4 sanal makinesini sınama örnekleri sağlıyoruz.

Windows ve Linux için sırasıyla ortak bir benchişaretleme araçları DiskSpd ve FIO 'u kullandık. Bu araçlar, iş yükü gibi bir üretimi taklit eden birden çok iş parçacığı oluşturup sistem performansını ölçer. Araçları kullanarak, genellikle bir uygulama için değiştiremediğiniz blok boyutu ve sıra derinliği gibi parametreleri de yapılandırabilirsiniz. Bu sayede, farklı türlerde uygulama iş yükleri için Premium SSD 'Ler ile sağlanan yüksek ölçekli bir VM 'de maksimum performansı elde etmeniz daha fazla esneklik sağlar. Her bir benchişaretleme aracı hakkında daha fazla bilgi edinmek için [DiskSpd](https://github.com/Microsoft/diskspd/wiki/) ve [Fio](http://freecode.com/projects/fio)ziyaret edin.

Aşağıdaki örnekleri izlemek için bir Standard_D8ds_v4 oluşturun ve VM 'ye dört Premium SSD ekleyin. Dört diskte, ana bilgisayar önbelleğe alma ile üç tane yapılandırın ve bunları Nocacheyazmaları adlı bir birime ayırın. Ana bilgisayar önbelleğe almayı kalan diskte "ReadOnly" olarak yapılandırın ve bu diskle CacheReads adlı bir birim oluşturun. Bu kurulumu kullanarak, Standard_D8ds_v4 VM 'den en fazla okuma ve yazma performansını görebilirsiniz. Premium SSD 'Ler ile Standard_D8ds_v4 oluşturma hakkında ayrıntılı adımlar için bkz. [yüksek performans Için tasarlama](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Yüksek performans için tasarlamaya yönelik](premium-storage-performance.md)makalemize ilerleyin.

Bu makalede, prototip için mevcut uygulamanıza benzer bir denetim listesi oluşturacaksınız. Değerlendirme araçlarını kullanarak, iş yüklerinin benzetimini yapabilir ve prototip uygulamasındaki performansı ölçebilir. Bunu yaparak, hangi disk teklifini, uygulama performansı gereksinimlerinizin eşleşeceğini veya geçebileceği tespit edebilirsiniz. Daha sonra üretim uygulamanız için aynı yönergeleri uygulayabilirsiniz.
