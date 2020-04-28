---
title: Yüksek kullanılabilirlik-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı-tek sunucu 'da yüksek kullanılabilirlik hakkında bilgi sağlanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 80229ff78c4570db583f1218d5d2f72da2dec388
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74768580"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları-tek sunucu
PostgreSQL için Azure veritabanı hizmeti, garantili yüksek düzeyde kullanılabilirlik sağlar. Mali olarak desteklenen hizmet düzeyi sözleşmesi (SLA), genel kullanıma yönelik olarak% 99,99 ' dir. Bu hizmet kullanılırken neredeyse hiçbir uygulama zaman kalmaz.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik yük devretme mekanizmalarına dayalıdır. Bir donanım arızası veya bir hizmet dağıtımına yanıt olarak düğüm düzeyinde bir kesinti meydana gelebilir.

Her zaman, PostgreSQL için Azure veritabanı veritabanı sunucusuna yapılan değişiklikler bir işlem bağlamında gerçekleşir. İşlem tamamlandığında Azure Storage 'da değişiklikler zaman uyumlu olarak kaydedilir. Düğüm düzeyinde bir kesinti oluşursa, veritabanı sunucusu otomatik olarak yeni bir düğüm oluşturur ve yeni düğüme veri depolama alanı ekler. Herhangi bir etkin bağlantı kesilir ve tüm esnek işlemler yürütülmedi.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
PostgreSQL veritabanı uygulamalarının, kesilen bağlantıları ve başarısız işlemleri tespit etmek ve yeniden denemek için oluşturulması önemlidir. Uygulama yeniden denendiğinde, uygulamanın bağlantısı, başarısız örnek için açık olan yeni oluşturulan örneğe saydam olarak yönlendirilir.

Azure 'da dahili olarak, bağlantıları yeni örneğe yönlendirmek için bir ağ geçidi kullanılır. Kesintiye uğratan sonra, yük devretme işleminin tamamı genellikle on saniye sürer. Yeniden yönlendirme ağ geçidi tarafından dahili olarak işlendiğinden, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Ölçeği artırma veya azaltma
HA modeline benzer şekilde, PostgreSQL için Azure veritabanı yukarı veya aşağı ölçeği, belirtilen boyuta sahip yeni bir sunucu örneği oluşturulur. Mevcut veri depolama, özgün örnekten ayrılır ve yeni örneğe eklenir.

Ölçeklendirme işlemi sırasında, veritabanı bağlantılarına bir kesinti meydana gelir. İstemci uygulamalarının bağlantısı kesilir ve açık işlenmemiş işlemler iptal edilir. İstemci uygulaması bağlantıyı yeniden dener veya yeni bir bağlantı yaptığında, ağ geçidi bağlantıyı yeni boyutlandırılmış örneğe yönlendirir. 

## <a name="next-steps"></a>Sonraki adımlar
- [Geçici bağlantı hatalarını işleme](concepts-connectivity.md) hakkında bilgi edinin
- [Okuma çoğaltmalarıyla verilerinizi çoğaltmayı](howto-read-replicas-portal.md) öğrenin
