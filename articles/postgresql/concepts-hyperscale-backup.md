---
title: Yedekleme ve geri yükleme – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: Verileri yanlışlıkla bozulma veya silme işleminden koruma
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 8dfc82ce79f33553be5220b52a1e415d99c26518
ms.sourcegitcommit: afb79a35e687a91270973990ff111ef90634f142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107483920"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)

PostgreSQL için Azure veritabanı – Hyperscale (Citus), her bir düğümün yedeklemesini otomatik olarak oluşturur ve yerel olarak yedekli depolamada depolar. Yedeklemeler, hiper ölçek (Citus) kümenizi belirtilen bir zamana geri yüklemek için kullanılabilir. Yedekleme ve geri yükleme her iş sürekliliği stratejisinin temel parçalarıdır çünkü bunlar verilerinizi yanlışlıkla bozulmalara veya silmelere karşı korur.

## <a name="backups"></a>Yedeklemeler

Günde en az bir kez, PostgreSQL için Azure veritabanı veri dosyalarının anlık görüntü yedeklemelerini ve veritabanı işlem günlüğünü alır. Yedeklemeler, bir sunucuyu Bekletme dönemi içinde herhangi bir zaman noktasına geri yüklemenize olanak tanır. (Bekletme dönemi Şu anda tüm kümeler için 35 gündür.) Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

Kullanılabilirlik alanlarını destekleyen Azure bölgelerinde, yedekleme anlık görüntüleri üç kullanılabilirlik alanında depolanır. En az bir kullanılabilirlik bölgesi çevrimiçi olduğu sürece, hiper ölçek (Citus) kümesi yeniden geri yüklenebilen.

Yedekleme dosyaları verilemez. Yalnızca PostgreSQL için Azure veritabanı 'nda geri yükleme işlemleri için kullanılabilir.

### <a name="backup-storage-cost"></a>Yedekleme depolama maliyeti

Geçerli yedekleme depolama fiyatlandırması için bkz. PostgreSQL için Azure veritabanı-hiper ölçek (Citus) [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/).

## <a name="restore"></a>Geri Yükleme

PostgreSQL için Azure veritabanı 'nda, hiper ölçek (Citus) kümesini geri yüklemek, özgün düğümlerin yedeklemelerinden yeni bir küme oluşturur. 

> [!IMPORTANT]
>Hiperscale (Citus) kümesini yalnızca aynı abonelik ve kaynak grubu içinde ve farklı bir küme adı ile geri yükleyebilirsiniz.


> [!IMPORTANT]
> Silinen hiper ölçek (Citus) kümeleri geri yüklenemez. Kümeyi silerseniz, kümeye ait olan tüm düğümler silinir ve kurtarılamaz. Küme kaynaklarını korumak için dağıtım sonrası, yanlışlıkla silme veya beklenmeyen değişikliklerden, Yöneticiler [Yönetim kilitlerinin](../azure-resource-manager/management/lock-resources.md)faydalanabilir.

### <a name="point-in-time-restore-pitr"></a>Zaman içinde bir noktaya geri yükleme (ıNR)

Bir kümeyi, son 35 gün içinde herhangi bir zaman noktasına geri yükleyebilirsiniz.
Bir noktadan noktaya geri yükleme, birden çok senaryoda faydalıdır. Örneğin kullanıcı yanlışlıkla verileri silebilir, önemli bir tabloyu veya veritabanını bırakabilir ya da bir uygulama yanlışlıkla iyi verilerin üzerine hatalı verileri yazabilir.

Geri yükleme işlemi, orijinal olarak aynı Azure bölgesinde, abonelikte ve kaynak grubunda yeni bir küme oluşturur. Kümenin özgün yapılandırması vardır: aynı düğüm sayısı, sanal çekirdek sayısı, depolama boyutu, Kullanıcı rolleri, PostgreSQL sürümü ve Citus uzantısının sürümü.

Güvenlik Duvarı ayarları ve PostgreSQL sunucu parametreleri özgün sunucu grubundan korunmaz, varsayılan değerlere sıfırlanır. Güvenlik Duvarı tüm bağlantıları engeller. Geri yükleme sonrasında bu ayarları el ile ayarlamanız gerekir. Genel olarak, önerilen [geri yükleme sonrası görevleri](howto-hyperscale-restore-portal.md#post-restore-tasks)listemize bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portal [bir sunucu grubunu geri yükleme](howto-hyperscale-restore-portal.md) adımlarına bakın.
*  [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md)hakkında bilgi edinin.
