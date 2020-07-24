---
title: Yedekleme ve geri yükleme – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: Verileri yanlışlıkla bozulma veya silme işleminden koruma
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 22bc3d6efca24a88b28217b2e06ac79d33f16b2e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87030088"
---
# <a name="backup-and-restore-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda yedekleme ve geri yükleme-hiper ölçek (Citus)

PostgreSQL için Azure veritabanı – Hyperscale (Citus), her bir düğümün yedeklemesini otomatik olarak oluşturur ve yerel olarak yedekli depolamada depolar. Yedeklemeler, hiper ölçek (Citus) kümenizi belirtilen bir zamana geri yüklemek için kullanılabilir. Yedekleme ve geri yükleme, verilerinizi yanlışlıkla bozulmasından veya silmekten koruyan bir iş sürekliliği stratejisinin önemli bir parçasıdır.

## <a name="backups"></a>Yedeklemeler

Günde en az bir kez, PostgreSQL için Azure veritabanı veri dosyalarının anlık görüntü yedeklemelerini ve veritabanı işlem günlüğünü alır. Yedeklemeler, bir sunucuyu Bekletme dönemi içinde herhangi bir zaman noktasına geri yüklemenize olanak tanır. (Bekletme dönemi Şu anda tüm kümeler için 35 gündür.) Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

Kullanılabilirlik alanlarını destekleyen Azure bölgelerinde, yedekleme anlık görüntüleri üç kullanılabilirlik alanında depolanır. En az bir kullanılabilirlik bölgesi çevrimiçi olduğu sürece, hiper ölçek (Citus) kümesi yeniden geri yüklenebilen.

Yedekleme dosyaları verilemez. Yalnızca PostgreSQL için Azure veritabanı 'nda geri yükleme işlemleri için kullanılabilir.

### <a name="backup-storage-cost"></a>Yedekleme depolama maliyeti

Geçerli yedekleme depolama fiyatlandırması için bkz. PostgreSQL için Azure veritabanı-hiper ölçek (Citus) [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/postgresql/hyperscale-citus/).

## <a name="restore"></a>Geri Yükleme

PostgreSQL için Azure veritabanı 'nda, hiper ölçek (Citus) kümesini geri yüklemek, özgün düğümlerin yedeklemelerinden yeni bir küme oluşturur. 

> [!IMPORTANT]
>Hiper ölçek kümesini yalnızca aynı abonelik ve kaynak grubu içinde ve farklı bir küme adı ile geri yükleyebilirsiniz.


> [!IMPORTANT]
> Silinen hiper ölçek (Citus) kümeleri geri yüklenemez. Kümeyi silerseniz, kümeye ait olan tüm düğümler silinir ve kurtarılamaz. Küme kaynaklarını korumak için dağıtım sonrası, yanlışlıkla silme veya beklenmeyen değişikliklerden, Yöneticiler [Yönetim kilitlerinin](/azure/azure-resource-manager/management/lock-resources)faydalanabilir.

### <a name="point-in-time-restore-pitr"></a>Zaman içinde bir noktaya geri yükleme (ıNR)

Bir kümeyi, son 35 gün içinde herhangi bir zaman noktasına geri yükleyebilirsiniz.
Bir noktadan noktaya geri yükleme, birden çok senaryoda faydalıdır. Örneğin, bir kullanıcı verileri yanlışlıkla sildiğinde, önemli bir tabloyu veya veritabanını bırakır veya bir uygulama yanlışlıkla hatalı veriler içeren iyi verilerin üzerine yazar.

Geri yükleme işlemi, orijinal olarak aynı Azure bölgesinde, abonelikte ve kaynak grubunda yeni bir küme oluşturur. Kümenin özgün yapılandırması vardır: aynı düğüm sayısı, sanal çekirdek sayısı, depolama boyutu, Kullanıcı rolleri, PostgreSQL sürümü ve Citus uzantısının sürümü.

Güvenlik Duvarı ayarları ve PostgreSQL sunucu parametreleri özgün sunucu grubundan korunmaz, varsayılan değerlere sıfırlanır. Güvenlik Duvarı tüm bağlantıları engeller. Geri yükleme sonrasında bu ayarları el ile ayarlamanız gerekir.

> [!IMPORTANT]
> Hiperscale (Citus) kümenizin zaman içindeki bir noktaya geri yükleme işlemini gerçekleştirmek için bir destek isteği açmanız gerekir.

### <a name="post-restore-tasks"></a>Post-restore tasks

Kurtarma mekanizmasından geri yükleme yapıldıktan sonra, kullanıcılarınızın ve uygulamalarınızın yedeklemesini ve çalışmasını sağlamak için aşağıdakileri yapmanız gerekir:

* Yeni sunucu özgün sunucunun yerini alacak şekilde, istemcileri ve istemci uygulamalarını yeni sunucuya yeniden yönlendirin
* Kullanıcıların bağlanabilmesi için uygun sunucu düzeyi güvenlik duvarı ve VNet kurallarının bulunduğundan emin olun. Bu kurallar özgün sunucu grubundan kopyalanmaz.
* PostgreSQL sunucu parametrelerini gereken şekilde ayarlayın. Parametreler, özgün sunucu grubundan kopyalanmaz.
* Uygun oturum açma ve veritabanı düzeyi izinlerinin yerinde olduğundan emin olun
* Uyarıları uygun şekilde yapılandırma

## <a name="next-steps"></a>Sonraki adımlar

*  [Azure kullanılabilirlik alanları](/azure/availability-zones/az-overview)hakkında bilgi edinin.
* Hiperscale (Citus) sunucu gruplarında [Önerilen uyarıları](/azure/postgresql/howto-hyperscale-alert-on-metric#suggested-alerts) ayarlayın.
