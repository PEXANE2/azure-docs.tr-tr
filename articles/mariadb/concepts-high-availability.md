---
title: Yüksek kullanılabilirlik - MariaDB için Azure Veritabanı
description: Bu konu, MariaDB için Azure Veritabanı'nı kullanırken yüksek kullanılabilirlik hakkında bilgi sağlar
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: a87646f6195a06cf0a5382cb248efa5516c953f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532000"
---
# <a name="high-availability-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda yüksek kullanılabilirlik kavramları
MariaDB hizmeti için Azure Veritabanı, yüksek düzeyde kullanılabilirlik garantisi sağlar. Mali destekli hizmet düzeyi anlaşması (SLA), genel kullanılabilirlik durumuna göre %99,99'dur. Bu hizmeti kullanırken hemen hemen hiçbir uygulama kapalı zaman yoktur.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik arıza mekanizmalarına dayanır. Bir düğüm düzeyi kesintisi, donanım hatası nedeniyle veya bir hizmet dağıtımına yanıt olarak oluşabilir.

Her zaman, MariaDB veritabanı sunucusu için bir Azure Veritabanı'nda yapılan değişiklikler bir işlem bağlamında gerçekleşir. İşlem işlendiğinde değişiklikler Azure depolama alanına eşzamanlı olarak kaydedilir. Düğüm düzeyinde bir kesinti oluşursa, veritabanı sunucusu otomatik olarak yeni bir düğüm oluşturur ve veri depolamasını yeni düğüme bağlar. Etkin bağlantılar bırakılır ve herhangi bir uçak içi işlem taahhüt edilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı esastır
MariaDB veritabanı uygulamalarının bırakılan bağlantıları ve başarısız hareketleri algılamak ve yeniden denemek için oluşturulmuş olması önemlidir. Uygulama yeniden çalıştığında, uygulamanın bağlantısı saydam olarak yeni oluşturulan örne yönlendirilir ve bu durum başarısız olan örnek için devralır.

Azure'da dahili olarak, bağlantıları yeni örne yönlendirmek için bir ağ geçidi kullanılır. Bir kesinti üzerine, tüm arıza işlemi genellikle onlarca saniye sürer. Yönlendirme ağ geçidi tarafından dahili olarak işlendiğinden, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Yukarı veya aşağı ölçekleme
HA modeline benzer şekilde, MariaDB için bir Azure Veritabanı yukarı veya aşağı ölçeklendirildiğinde, belirtilen büyüklüğe sahip yeni bir sunucu örneği oluşturulur. Varolan veri depolama özgün örneğinden ayrılır ve yeni örne eklenir.

Ölçek işlemi sırasında veritabanı bağlantılarında bir kesinti oluşur. İstemci uygulamaları kesilir ve işlenmemiş açık hareketler iptal edilir. İstemci uygulaması bağlantıyı yeniden denediğinde veya yeni bir bağlantı kurduktan sonra, ağ geçidi bağlantıyı yeni boyutlandırılmış örne yönlendirir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmete genel bir bakış için [MariaDB Genel Bakış için Azure Veritabanı'na](overview.md) bakın
