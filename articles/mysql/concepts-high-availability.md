---
title: Yüksek Kullanılabilirlik-MySQL için Azure veritabanı
description: Bu konu, MySQL için Azure veritabanı kullanılırken yüksek kullanılabilirlik bilgilerini sağlar
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 50bf1502589ea0932fd45367c039e6e37135d761
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086067"
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları
MySQL için Azure veritabanı hizmeti, garantili yüksek düzeyde kullanılabilirlik sağlar. Mali olarak desteklenen hizmet düzeyi sözleşmesi (SLA), genel kullanıma yönelik olarak% 99,99 ' dir. Bu hizmet kullanılırken neredeyse hiçbir uygulama zaman kalmaz.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik yük devretme mekanizmalarına dayanır. Bir donanım arızası veya bir hizmet dağıtımına yanıt olarak düğüm düzeyinde bir kesinti meydana gelebilir.

Her zaman, MySQL için Azure veritabanı veritabanı sunucusuna yapılan değişiklikler bir işlem bağlamında gerçekleşir. İşlem tamamlandığında Azure Storage 'da değişiklikler zaman uyumlu olarak kaydedilir. Düğüm düzeyinde bir kesinti oluşursa, veritabanı sunucusu otomatik olarak yeni bir düğüm oluşturur ve yeni düğüme veri depolama alanı ekler. Herhangi bir etkin bağlantı kesilir ve tüm esnek işlemler yürütülmedi.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
MySQL veritabanı uygulamalarının, kesilen bağlantıları ve başarısız işlemleri tespit etmek ve yeniden denemek için oluşturulması önemlidir. Uygulama yeniden denendiğinde, uygulamanın bağlantısı, başarısız örnek için açık olan yeni oluşturulan örneğe saydam olarak yönlendirilir.

Azure 'da dahili olarak, bağlantıları yeni örneğe yönlendirmek için bir ağ geçidi kullanılır. Kesintiye uğratan sonra, yük devretme işleminin tamamı genellikle on saniye sürer. Yeniden yönlendirme ağ geçidi tarafından dahili olarak işlendiğinden, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Ölçeği artırma veya azaltma
HA modeline benzer şekilde, MySQL için Azure veritabanı yukarı veya aşağı ölçeklenirse, belirtilen boyuta sahip yeni bir sunucu örneği oluşturulur. Mevcut veri depolama, özgün örnekten ayrılır ve yeni örneğe eklenir.

Ölçeklendirme işlemi sırasında, veritabanı bağlantılarına bir kesinti meydana gelir. İstemci uygulamalarının bağlantısı kesilir ve açık işlenmemiş işlemler iptal edilir. İstemci uygulaması bağlantıyı yeniden dener veya yeni bir bağlantı yaptığında, ağ geçidi bağlantıyı yeni boyutlandırılmış örneğe yönlendirir.

## <a name="next-steps"></a>Sonraki adımlar
- [Geçici bağlantı hatalarını işleme](concepts-connectivity.md) hakkında bilgi edinin
- [Okuma çoğaltmalarıyla verilerinizi çoğaltmayı](howto-read-replicas-portal.md) öğrenin
