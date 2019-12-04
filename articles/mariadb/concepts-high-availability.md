---
title: Yüksek kullanılabilirlik-MariaDB için Azure veritabanı
description: Bu konu, MariaDB için Azure veritabanı kullanılırken yüksek kullanılabilirlik bilgilerini sağlar
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 797a94a561351ac7f5317f2f215b56f6944c023f
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772535"
---
# <a name="high-availability-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda yüksek kullanılabilirlik kavramları
MariaDB için Azure veritabanı hizmeti, garantili yüksek düzeyde kullanılabilirlik sağlar. Mali olarak desteklenen hizmet düzeyi sözleşmesi (SLA), genel kullanıma yönelik olarak% 99,99 ' dir. Bu hizmet kullanılırken neredeyse hiçbir uygulama zaman kalmaz.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, düğüm düzeyinde bir kesinti oluştuğunda yerleşik yük devretme mekanizmalarına dayanır. Bir donanım arızası veya bir hizmet dağıtımına yanıt olarak düğüm düzeyinde bir kesinti meydana gelebilir.

Her zaman, MariaDB veritabanı sunucusu için Azure veritabanı 'nda yapılan değişiklikler bir işlem bağlamında gerçekleşir. İşlem tamamlandığında Azure Storage 'da değişiklikler zaman uyumlu olarak kaydedilir. Düğüm düzeyinde bir kesinti oluşursa, veritabanı sunucusu otomatik olarak yeni bir düğüm oluşturur ve yeni düğüme veri depolama alanı ekler. Herhangi bir etkin bağlantı kesilir ve tüm esnek işlemler yürütülmedi.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
MariaDB veritabanı uygulamalarının, kesilen bağlantıları ve başarısız işlemleri tespit etmek ve yeniden denemek için oluşturulması önemlidir. Uygulama yeniden denendiğinde, uygulamanın bağlantısı, başarısız örnek için açık olan yeni oluşturulan örneğe saydam olarak yönlendirilir.

Azure 'da dahili olarak, bağlantıları yeni örneğe yönlendirmek için bir ağ geçidi kullanılır. Kesintiye uğratan sonra, yük devretme işleminin tamamı genellikle on saniye sürer. Yeniden yönlendirme ağ geçidi tarafından dahili olarak işlendiğinden, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Ölçeği artırma veya azaltma
HA modeliyle benzer şekilde, bir MariaDB için Azure veritabanı yukarı veya aşağı ölçeklendirildiğinde, belirtilen boyuta sahip yeni bir sunucu örneği oluşturulur. Mevcut veri depolama, özgün örnekten ayrılır ve yeni örneğe eklenir.

Ölçeklendirme işlemi sırasında, veritabanı bağlantılarına bir kesinti meydana gelir. İstemci uygulamalarının bağlantısı kesilir ve açık işlenmemiş işlemler iptal edilir. İstemci uygulaması bağlantıyı yeniden dener veya yeni bir bağlantı yaptığında, ağ geçidi bağlantıyı yeni boyutlandırılmış örneğe yönlendirir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmete genel bakış için bkz. [MariaDB Için Azure veritabanı genel bakış](overview.md)
