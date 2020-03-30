---
title: Azure altyapı izleme
description: Bu makalede, Azure üretim ağının izlenmesi anlatılmaktadır.
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
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: af9c157b4644156edc6dcdb1b53c141263576500
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727163"
---
# <a name="azure-infrastructure-monitoring"></a>Azure altyapı izleme   

## <a name="configuration-and-change-management"></a>Yapılandırma ve değişim yönetimi
Azure, donanım, yazılım ve ağ aygıtlarının yapılandırma ayarlarını ve temel yapılandırmalarını her yıl gözden geçirir ve güncelleştirir. Değişiklikler, geliştirme ve/veya test ortamından üretim ortamına girmeden önce geliştirilir, test edilir ve onaylanır.

Azure tabanlı hizmetler için gerekli olan temel yapılandırmalar, Azure güvenlik ve uyumluluk ekibi ve hizmet ekipleri tarafından gözden geçirilir. Servis ekibi incelemesi, üretim hizmetinin dağıtımından önce gerçekleşen sınamanın bir parçasıdır.

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi
Güvenlik güncelleştirmesi yönetimi, sistemlerin bilinen güvenlik açıklarından korunmasına yardımcı olur. Azure, Microsoft yazılımları için güvenlik güncelleştirmelerinin dağıtımını ve yüklenmesini yönetmek için tümleşik dağıtım sistemlerini kullanır. Azure, Microsoft Güvenlik Yanıt Merkezi'nin (MSRC) kaynaklarından da yararlanabiliyor. MSRC, yılın her günü güvenlik olaylarını ve bulut açıklarını 24 saat tanımlar, izler, yanıtlar ve giderir.

## <a name="vulnerability-scanning"></a>Güvenlik açığı taraması
Güvenlik açığı taraması sunucu işletim sistemlerinde, veritabanlarında ve ağ aygıtlarında gerçekleştirilir. Güvenlik açığı taramaları en az üç ayda bir gerçekleştirilir. Azure, Azure sınırının penetrasyon testini gerçekleştirmek için bağımsız denetçilerle sözleşme yapar. Kırmızı takım egzersizleri de rutin olarak gerçekleştirilir ve sonuçlar güvenlik iyileştirmeleri yapmak için kullanılır.

## <a name="protective-monitoring"></a>Koruyucu izleme
Azure güvenliği, etkin izleme için gereksinimleri tanımlamıştır. Servis ekipleri, etkin izleme araçlarını bu gereksinimlere uygun olarak yapılandırır. Etkin izleme araçları arasında Microsoft İzleme Aracısı (MMA) ve Sistem Merkezi İşlemleri Yöneticisi yer almaktadır. Bu araçlar, hemen harekete geçilmesini gerektiren durumlarda Azure güvenlik personeline zaman uyarıları sağlayacak şekilde yapılandırılmıştır.

## <a name="incident-management"></a>Olay yönetimi
Microsoft, olaylara eşgüdümlü bir yanıt vermek için bir güvenlik olayı yönetimi işlemi uygular.

Microsoft, ekipmanında veya tesislerinde depolanan müşteri verilerine yetkisiz erişimin farkına varırsa veya müşteri verilerinin kaybolması, açıklanması veya değiştirilmesiyle sonuçlanan bu tür ekipman veya tesislere yetkisiz erişimden haberdar olursa, Microsoft aşağıdaki işlemleri alır:

- Müşteriye güvenlik olayını derhal bildirir.
- Güvenlik olayını derhal inceler ve müşterilere güvenlik olayı hakkında ayrıntılı bilgi sağlar.
- Etkileri azaltmak ve güvenlik olayından kaynaklanan hasarları en aza indirmek için makul ve hızlı adımlar atar.

Rolleri tanımlayan ve sorumlulukları ayıran bir olay yönetimi çerçevesi oluşturulmuştur. Azure güvenlik olay yönetimi ekibi, yükseltme de dahil olmak üzere güvenlik olaylarını yönetmekten ve gerektiğinde uzman ekiplerin katılımını sağlamaktan sorumludur. Azure operasyon yöneticileri, güvenlik ve gizlilik olaylarının araştırılmasını ve çözümlenmesinden sorumludur.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
