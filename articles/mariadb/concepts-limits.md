---
title: Sınırlamalar-MariaDB için Azure veritabanı
description: Bu makalede, MariaDB için Azure veritabanı 'nda bağlantı ve depolama motoru seçeneklerinin sayısı gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: fc89b6233602c81ea622a528c223adf2003f0f68
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772505"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı sınırlamaları
Aşağıdaki bölümlerde kapasiteyi, depolama altyapısı desteğini, ayrıcalık desteğini, veri işleme ekstresi desteğini ve veritabanı hizmetindeki işlev sınırlarını anlatmaktadır.

## <a name="maximum-connections"></a>En fazla bağlantı
Fiyatlandırma Katmanı ve sanal çekirdek başına en fazla bağlantı sayısı aşağıdaki gibidir:

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**| **En fazla bağlantı sayısı**|
|---|---|---|
|Temel| 1| 50|
|Temel| 2| 100|
|Genel Amaçlı| 2| 300|
|Genel Amaçlı| 4| 625|
|Genel Amaçlı| 8| 1250|
|Genel Amaçlı| 16| 2500|
|Genel Amaçlı| 32| 5000|
|Genel Amaçlı| 64| 10000|
|Bellek için İyileştirilmiş| 2| 600|
|Bellek için İyileştirilmiş| 4| 1250|
|Bellek için İyileştirilmiş| 8| 2500|
|Bellek için İyileştirilmiş| 16| 5000|
|Bellek için İyileştirilmiş| 32| 10000|

Bağlantılar sınırı aştığında, şu hatayı alabilirsiniz:
> HATA 1040 (08004): çok fazla bağlantı

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Desteklenen
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [BELLEK](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Desteklenmeyen
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [KARA DELIK](https://mariadb.com/kb/en/library/blackhole/)
- [ARŞIVLIYORSANıZ](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- DBA rolü: çok sayıda sunucu parametresi ve ayarı, DBMS 'nin sunucu performansını veya Negate ACID özelliklerini yanlışlıkla düşürebilir. Bu nedenle, hizmet bütünlüğünü ve SLA 'yı bir ürün düzeyinde sürdürmek için, bu hizmet DBA rolünü kullanıma sunmaz. Yeni bir veritabanı örneği oluşturulduğunda oluşturulan varsayılan kullanıcı hesabı, bu kullanıcının yönetilen veritabanı örneğinde DDL ve DML deyimlerinin çoğunu gerçekleştirmesini sağlar.
- Süper ayrıcalık: benzer [süper ayrıcalık](https://mariadb.com/kb/en/library/grant/#global-privileges) da kısıtlıdır.
- DEFINER: oluşturmak için süper ayrıcalıklar gerektirir ve kısıtlıdır. Bir yedekleme kullanarak veri içeri aktardıysanız, bir mysqldump gerçekleştirirken `CREATE DEFINER` komutlarını el ile veya `--skip-definer` komutunu kullanarak kaldırın.

## <a name="data-manipulation-statement-support"></a>Veri işleme ekstresi desteği

### <a name="supported"></a>Desteklenen
- `LOAD DATA INFILE` desteklenir, ancak `[LOCAL]` parametresi belirtilmelidir ve bir UNC yoluna (SMB üzerinden bağlanmış Azure depolama) yönlendirilmelidir.

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlevsel sınırlamalar

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanlarına ve bu katmanlara dinamik ölçeklendirme Şu anda desteklenmiyor.
- Sunucu depolama boyutunun düşürülmesi desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltmeleri
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş Şu anda desteklenmiyor.

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- INR özelliği kullanılırken yeni sunucu, temel aldığı sunucuyla aynı yapılandırmalara sahip olarak oluşturulur.
- Silinen bir sunucunun geri yüklenmesi desteklenmez.

### <a name="subscription-management"></a>Abonelik yönetimi
- Önceden oluşturulmuş sunucuları, abonelik ve kaynak grubu genelinde dinamik olarak taşıma işlemi şu anda desteklenmiyor.

### <a name="vnet-service-endpoints"></a>VNet hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- MariaDB sunucu örneği, bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntülüyor. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MariaDB veritabanı sürümleri](concepts-supported-versions.md)
