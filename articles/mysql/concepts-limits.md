---
title: Sınırlamalar-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda bağlantı ve depolama motoru seçeneklerinin sayısı gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/2/2019
ms.openlocfilehash: 07feb3ebf9720d70da441486fd0b2e6e274b68e4
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770926"
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı sınırlamaları
Aşağıdaki bölümlerde kapasiteyi, depolama altyapısı desteğini, ayrıcalık desteğini, veri işleme ekstresi desteğini ve veritabanı hizmetindeki işlev sınırlarını anlatmaktadır. Ayrıca bkz. MySQL veritabanı altyapısı için geçerli olan [genel sınırlamalar](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) .

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
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [BELLEK](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmeyen
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [KARA DELIK](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARŞIVLIYORSANıZ](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [Federasyon](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- DBA rolü: çok sayıda sunucu parametresi ve ayarı, DBMS 'nin sunucu performansını veya Negate ACID özelliklerini yanlışlıkla düşürebilir. Bu nedenle, hizmet bütünlüğünü ve SLA 'yı bir ürün düzeyinde sürdürmek için, bu hizmet DBA rolünü kullanıma sunmaz. Yeni bir veritabanı örneği oluşturulduğunda oluşturulan varsayılan kullanıcı hesabı, bu kullanıcının yönetilen veritabanı örneğinde DDL ve DML deyimlerinin çoğunu gerçekleştirmesini sağlar. 
- Süper ayrıcalık: benzer [süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) da kısıtlıdır.
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
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş Şu anda desteklenmiyor. Bir sonraki ana sürüme yükseltmek isterseniz, bir dökümünü alın ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./concepts-migrate-dump-restore.md) .

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- INR özelliği kullanılırken yeni sunucu, temel aldığı sunucuyla aynı yapılandırmalara sahip olarak oluşturulur.
- Silinen bir sunucunun geri yüklenmesi desteklenmez.

### <a name="vnet-service-endpoints"></a>VNet hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- MySQL Server örneği bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntülüyor. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
