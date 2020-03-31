---
title: Azure altyapısı kullanılabilirliği - Azure güvenliği
description: Bu makalede, Microsoft'un Azure altyapısını güvence altına almak ve müşterilerin verilerinin maksimum kullanılabilirliğini sağlamak için ne yaptığı hakkında bilgi verilmektedir.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: c50c4faf47caf0a7519d61fdc8989ec9fd809d78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727225"
---
# <a name="azure-infrastructure-availability"></a>Azure altyapısı kullanılabilirliği
Bu makalede, Microsoft'un Azure altyapısını güvence altına almak ve müşterilerin verilerinin maksimum kullanılabilirliğini sağlamak için ne yaptığı hakkında bilgi verilmektedir. Azure, sanallaştırma teknolojisiyle elde edilen kapsamlı fazlalığa dayalı olarak güçlü kullanılabilirlik sağlar.

## <a name="temporary-outages-and-natural-disaster"></a>Geçici kesintiler ve doğal afet
Microsoft Bulut Altyapısı ve Operasyonları ekibi bulut altyapısının güvenliğini tasarlar, oluşturur, çalışır ve geliştirir. Bu ekip, Azure altyapısının yüksek kullanılabilirlik ve güvenilirlik, yüksek verimlilik ve akıllı ölçeklenebilirlik sunmasını sağlar. Takım daha güvenli, özel ve güvenilir bir bulut sağlar.

Kesintisiz güç kaynakları ve geniş pil bankaları, kısa süreli bir güç kesintisi meydana geldiğinde elektriğin kesintisiz kalmasını sağlar. Acil durum jeneratörleri uzun kesintiler ve planlı bakım için yedek güç sağlar. Doğal bir felaket oluşursa, veri merkezi yerinde yakıt rezervlerini kullanabilir.

Yüksek hızlı ve sağlam fiber optik ağlar, veri merkezlerini diğer büyük hub'lara ve internet kullanıcılarına bağlar. Bilgi işlem düğümleri gecikme süresini azaltmak, coğrafi artıklığı sağlamak ve genel hizmet esnekliğini artırmak için iş yüklerini kullanıcılara daha yakın barındırıyor. Mühendislerden oluşan bir ekip, hizmetlerin sürekli olarak kullanılabilir olmasını sağlamak için 24 saat çalışır.

Microsoft, gelişmiş izleme ve olay yanıtı, hizmet desteği ve yedekleme başarısızlığı özelliği sayesinde yüksek kullanılabilirlik sağlar. Coğrafi olarak dağıtılmış Microsoft operasyon merkezleri 24/7/365 çalışır. Azure ağı dünyanın en büyük ağlarından biridir. Fiber optik ve içerik dağıtım ağı, yüksek performans ve güvenilirlik sağlamak için veri merkezlerini ve kenar düğümlerini birbirine bağlar.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma
Azure, verilerinizi iki konumda dayanıklı tutar. Yedekleme sitesinin konumunu seçebilirsiniz. Azure, her iki konumda da verilerinizin üç sağlıklı kopyasını sürekli olarak saklar.

## <a name="database-availability"></a>Veritabanı kullanılabilirliği
Azure, veritabanının sürekli veritabanı kullanılabilirliği olan bir internet ağ geçidi üzerinden Internet erişimine sahip olmasını sağlar. İzleme, etkin veritabanlarının durumunu ve durumunu beş dakikalık zaman aralıklarında değerlendirir.

## <a name="storage-availability"></a>Depolama kullanılabilirliği
Azure, bağlantı uç noktaları sağlayan yüksek ölçeklenebilir ve dayanıklı bir depolama hizmeti aracılığıyla depolama sağlar. Bu, bir uygulamanın depolama hizmetine doğrudan erişebileceği anlamına gelir. Depolama hizmeti, gelen depolama isteklerini işlem bütünlüğüyle verimli bir şekilde işler.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısının güvenliğini sağlamaya yardımcı olmak için neler yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
