---
title: Yüksek kullanılabilirlik - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'nda yüksek kullanılabilirlik hakkında bilgi verilmektedir.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 80229ff78c4570db583f1218d5d2f72da2dec388
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768580"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda yüksek kullanılabilirlik kavramları - Single Server
PostgreSQL için Azure Veritabanı hizmeti, garantili yüksek düzeyde kullanılabilirlik sağlar. Mali destekli hizmet düzeyi anlaşması (SLA), genel kullanılabilirlik durumuna göre %99,99'dur. Bu hizmeti kullanırken hemen hemen hiçbir uygulama kapalı zaman yoktur.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik arıza mekanizmalarına dayanır. Bir düğüm düzeyi kesintisi, donanım hatası nedeniyle veya bir hizmet dağıtımına yanıt olarak oluşabilir.

Her zaman, PostgreSQL veritabanı sunucusu için bir Azure Veritabanı'nda yapılan değişiklikler bir işlem bağlamında gerçekleşir. İşlem işlendiğinde değişiklikler Azure depolama alanına eşzamanlı olarak kaydedilir. Düğüm düzeyinde bir kesinti oluşursa, veritabanı sunucusu otomatik olarak yeni bir düğüm oluşturur ve veri depolamasını yeni düğüme bağlar. Etkin bağlantılar bırakılır ve herhangi bir uçak içi işlem taahhüt edilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı esastır
PostgreSQL veritabanı uygulamalarının bırakılan bağlantıları ve başarısız hareketleri algılamak ve yeniden denemek için oluşturulmuş olması önemlidir. Uygulama yeniden çalıştığında, uygulamanın bağlantısı saydam olarak yeni oluşturulan örne yönlendirilir ve bu durum başarısız olan örnek için devralır.

Azure'da dahili olarak, bağlantıları yeni örne yönlendirmek için bir ağ geçidi kullanılır. Bir kesinti üzerine, tüm arıza işlemi genellikle onlarca saniye sürer. Yönlendirme ağ geçidi tarafından dahili olarak işlendiğinden, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Yukarı veya aşağı ölçekleme
HA modeline benzer şekilde, PostgreSQL için bir Azure Veritabanı yukarı veya aşağı ölçeklendirildiğinde, belirtilen büyüklüğe sahip yeni bir sunucu örneği oluşturulur. Varolan veri depolama özgün örneğinden ayrılır ve yeni örne eklenir.

Ölçek işlemi sırasında veritabanı bağlantılarında bir kesinti oluşur. İstemci uygulamaları kesilir ve işlenmemiş açık hareketler iptal edilir. İstemci uygulaması bağlantıyı yeniden denediğinde veya yeni bir bağlantı kurduktan sonra, ağ geçidi bağlantıyı yeni boyutlandırılmış örne yönlendirir. 

## <a name="next-steps"></a>Sonraki adımlar
- Geçici [bağlantı hatalarını işleme](concepts-connectivity.md) hakkında bilgi edinin
- [Okuma yinelemeleriyle verilerinizi nasıl çoğaltacağınızı](howto-read-replicas-portal.md) öğrenin
