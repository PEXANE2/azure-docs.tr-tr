---
title: Azure altyapı güvenliği | Microsoft Docs
description: Makalede, Microsoft 'un Azure veri merkezlerimizi güvenli hale getirmek için nasıl çalıştığı açıklanır.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 3da9fef8c7de248f30100f94724cd5dcdb7db8e3
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727068"
---
# <a name="azure-infrastructure-security"></a>Azure altyapı güvenliği
Microsoft Azure, Microsoft tarafından yönetilen ve çalıştırılan veri merkezlerinde çalıştırılır. Bu coğrafi olarak dağınık veri merkezleri, güvenlik ve güvenilirlik için ISO/ıEC 27001:2013 ve NıST SP 800-53 gibi temel sektör standartlarıyla uyumlu değildir. Veri merkezleri Microsoft operasyon personeli tarafından yönetilir, izlenir ve yönetilir. Operasyon personeli, dünyanın en büyük çevrimiçi hizmetler 24 x 7 devamlılığını sunmaya yönelik yıllar yaşar.

Bu makale serisi, Microsoft 'un Azure altyapısını güvenli hale getirmeye yönelik bilgiler sağlar. Makale adresi:

- [Fiziksel güvenlik](physical-security.md)
- [Kullanılabilirlik](infrastructure-availability.md)
- [Bileşenler ve sınırlar](infrastructure-components.md)
- [Ağ mimarisi](infrastructure-network.md)
- [Üretim ağı](production-network.md)
- [SQL Veritabanı](infrastructure-sql.md)
- [İşlemler](infrastructure-operations.md)
- [İzleme](infrastructure-monitoring.md)
- [Doğruluğunu](infrastructure-integrity.md)
- [Veri koruma](protection-customer-data.md)

## <a name="shared-responsibility-model"></a>Paylaşılan sorumluluk modeli
Siz ve Microsoft arasındaki sorumluluk bölümünü anlamak önemlidir. Şirket içinde tüm yığınınızı sahipsiz, ancak buluta geçtiğinizde bazı sorumluluklar Microsoft 'a aktarır. Aşağıdaki grafikte, yığınınızın dağıtımına (hizmet olarak yazılım [SaaS], hizmet olarak platform [PaaS], hizmet olarak altyapı [IaaS] ve şirket içi) göre sorumluluk alanlarının gösterildiği gösterilmektedir.

![Sorumlulukları gösteren grafik](./media/infrastructure/responsibility-zones.png)

Dağıtım türünden bağımsız olarak, aşağıdakilerden her zaman sorumlu olursunuz:

- Data
- Uç Noktalar
- Hesap
- Erişim yönetimi

SaaS, PaaS ve IaaS dağıtımında, siz ve Microsoft arasındaki sorumluluk bölümünü anladığınızdan emin olun. Daha fazla bilgi için bkz. [bulut bilgi işlem Için paylaşılan sorumluluklar](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf).

## <a name="next-steps"></a>Sonraki adımlar
Microsoft 'un Azure altyapısını güvenli hale getirmeye yardımcı olma hakkında daha fazla bilgi edinmek için bkz.:

- [Azure tesisler, şirket içi ve fiziksel güvenlik](physical-security.md)
- [Azure altyapı kullanılabilirliği](infrastructure-availability.md)
- [Azure Information System bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri verileri koruması](protection-customer-data.md)


