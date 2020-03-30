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
ms.openlocfilehash: 86d03e980a233a60e4f3a6541462264dae0862af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74005573"
---
Bu makalede, Azure Premium Depolama kullanarak yüksek performanslı uygulamalar oluşturmak için yönergeler sağlanmaktadır. Bu belgede sağlanan yönergeleri, uygulamanız tarafından kullanılan teknolojiler için geçerli olan en iyi performans uygulamalarıyla birlikte kullanabilirsiniz. Yönergeleri göstermek için, Premium Depolama'da çalışan SQL Server'ı bu belge boyunca örnek olarak kullandık.

Bu makalede Depolama katmanı için performans senaryolarını ele alırken, uygulama katmanını en iyi duruma getirmeniz gerekir. Örneğin, Azure Premium Depolama'da bir SharePoint Farm barındırAn varsa, veritabanı sunucusunu optimize etmek için bu makaledeki SQL Server örneklerini kullanabilirsiniz. Ayrıca, en iyi performansı elde etmek için SharePoint Farm'ın Web sunucusunu ve Uygulama sunucusunu optimize edin.

Bu makale, Azure Premium Depolama'da uygulama performansını optimize etme ile ilgili sık sorulan soruların yanıtlanmasına yardımcı olacaktır,

* Uygulama performansınızı nasıl ölçeriz?  
* Neden beklenen yüksek performansı görmüyorsun?  
* Premium Depolama'daki uygulama performansınızı etkileyen faktörler nelerdir?  
* Bu faktörler Premium Depolama'daki uygulamanızın performansını nasıl etkiler?  
* IOPS, Bant Genişliği ve Gecikme Sürelerini nasıl optimize edebilirsiniz?  

Premium Depolama'da çalışan iş yükleri son derece performansa duyarlı olduğundan, bu yönergeleri özellikle Premium Depolama için sağladık. Uygun olan yerlerde örnekler verdik. Bu yönergelerden bazılarını Standart Depolama diskleri ile IaaS VM'lerde çalışan uygulamalara da uygulayabilirsiniz.