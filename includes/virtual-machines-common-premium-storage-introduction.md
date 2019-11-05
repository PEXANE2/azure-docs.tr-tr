---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b1287f9c7e946c7b4d035b2ad6301947ffad3cea
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73524101"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Depolama: yüksek performans için tasarım

Bu makalede, Azure Premium depolama kullanan yüksek performanslı uygulamalar oluşturmaya yönelik yönergeler sağlanmaktadır. Bu belgede sunulan yönergeleri, uygulamanız tarafından kullanılan teknolojiler için geçerli olan performans en iyi uygulamalarıyla birlikte kullanabilirsiniz. Yönergeleri göstermek için, bu belgenin tamamında bir örnek olarak Premium depolamada çalışan SQL Server kullandık.

Bu makaledeki depolama katmanının performans senaryolarını ele alırken uygulama katmanını iyileştirmeniz gerekecektir. Örneğin, Azure Premium depolamada bir SharePoint grubu barındırıyorsanız, veritabanı sunucusunu iyileştirmek için bu makaledeki SQL Server örnekleri kullanabilirsiniz. Ayrıca, en iyi performansı almak için SharePoint grubunun Web sunucusunu ve uygulama sunucusunu iyileştirin.

Bu makale, Azure Premium depolamada uygulama performansını iyileştirmek için aşağıdaki yaygın soruların yanıtlanmasına yardımcı olur.

* Uygulamanızın performansını ölçme  
* Beklenen yüksek performansa neden görmüyorsunuz?  
* Premium depolamada uygulama performansınızı hangi etkenlere etkiler?  
* Bu faktörler, Premium depolamada uygulamanızın performansını nasıl etkiler?  
* IOPS, bant genişliği ve gecikme süresi için nasıl iyileştirebileceğinizi?  

Premium depolamada çalışan iş yükleri yüksek performansa duyarlı olduğundan, bu yönergeleri özellikle Premium Depolama için sağladık. Uygun yerlerde örnek sağladık. Ayrıca, bu yönergelerin bazılarını standart depolama diskleriyle IaaS VM 'lerinde çalışan uygulamalar için de uygulayabilirsiniz.